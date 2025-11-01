# Insurance Claim Automation Project

## Problem Analysis

The core challenge is to evolve Barkibu's claims process from a fully manual, expert-driven bottleneck into a highly automated, intelligent, and scalable workflow. The objective is not just to replace manual tasks but to codify the specialized knowledge of the veterinary team into a system that can handle the majority of claims with speed and precision.

Success means faster reimbursements (improving user loyalty), lower operational costs, and the ability to scale the user base without proportionally scaling the operations team. Failure means inaccurate payouts (financial loss) or incorrect rejections (eroding user trust).

### Key Parts to Solve

* **Unstructured Data Ingestion:** The inputs (invoices, reports, medical histories) are unstructured documents. The single most critical task is to reliably and accurately convert these into structured JSON data. This is an OCR (Optical Character Recognition) and NER (Named Entity Recognition) problem.
* **Policy Engine:** Build a reliable, configurable, extensible, and auditable policy engine to evaluate claims against coverage.
* **Risk Analysis:** Perform risk analysis to prevent fraud, abuse, and waste.
* **Pre-existing Conditions (PXC) Detector:** Build a pre-existing condition (PXC) detector based on the pet's medical history and the current diagnosis.
* **Human-in-the-Loop (HITL):** The system must automatically route low-confidence, high-risk, or highly complex claims to the same back-office application the vets already use.
* **Feedback Loop:** The system must learn. When a vet manually corrects a decision, that correction must be captured to measure accuracy, retrain models, and improve both data extraction and decision logic.
* **Data Analytics Pipeline:** The structured data must feed the user data analytics system to improve the product, pricing, and user experience.
* **Workflow Management:** Provide a system to visualize the data output from each step, retry failed steps, and access the claim's state in real-time.

### Constraints, Risks, and Complexities

* **Data Quality:** The entire system's accuracy is capped by its ability to understand poor-quality scans, varied document formats, and handwritten notes. This is the highest technical risk.
* **Auditability & Explainability:** As an insurance provider, we must be able to explain *why* a claim was approved or rejected. The decision engine must produce auditable, human-readable reasoning for every decision.
* **Policy Variance:** The risk of unmanageable complexity as policy rules diverge across different products and countries. The policy engine must be designed to handle this variance without constant code changes.
* **PXC Model Data:** The PXC detector will require a substantial and high-quality dataset to build a reliable model that can understand complex medical nuances and relationships.
* **GenAI Model Risk:** Using Generative AI for extraction introduces stochasticity (randomness). The same document could produce slightly different results on different runs. This must be managed, and the risk of model "hallucinations" (inventing data) must be mitigated, likely through strong confidence scoring and validation rules.

## System Design

I propose a decoupled, event-driven architecture following the **SAGA Orchestrator pattern**. This approach is highly scalable, resilient, auditable, and flexible. The entire workflow becomes a pipeline of services that communicate by producing and consuming events via a central message broker (like Apache Kafka or RabbitMQ).

The system will be composed of multiple components, which can be deployed as independent microservices:

* **Claim Intake Service:** This service validates the input (user auth, file types), securely saves the documents in a file store (e.g., S3), and publishes a `ClaimSubmitted` event to the message broker.
* **Document OCR Service:** This is the core extraction pipeline. It consumes `ClaimSubmitted` events and is responsible for:
    1.  Classifying documents (invoice, medical report, etc.).
    2.  Running OCR on all documents to get raw text.
    3.  Using a GenAI model (e.g., a fine-tuned model or a powerful one like GPT5) to extract key entities from the raw text (diagnoses, symptoms, invoice line items, costs, dates) into a structured format.
* **Pet History Service:** This service manages the pet's consolidated medical history. It evaluates if an extracted diagnosis matches or is semantically related to any pre-existing conditions.
    * This service will likely need to manage its own database and use medical ontologies (like SNOMED) to understand that "otitis" and "ear infection" are related.
* **Policy & Coverage Service:** This service digitally represents a user's policy. It likely contains an internal Rules Engine (e.g., a JSON-based ruleset or a dedicated engine) to perform coverage checks for each invoice line item and calculate the final reimbursement amount.
* **Fraud Detector Service:** This service performs risk analysis to prevent fraud, abuse, and waste. It checks for duplicate claims, unusual provider activity, or patterns indicative of abuse.
* **Claim Orchestrator Service (The SAGA Orchestrator):** This is the "brain" of the workflow. It subscribes to events from all other services and maintains the state of each claim. It orchestrates the flow (e.g., "After OCR is done, call the PXC service *and* the Policy service").
    * It is also responsible for the HITL logic. Based on confidence scores, fraud flags, or PXC ambiguity, it can pause the SAGA and publish a `ClaimNeedsReview` event to a manual review queue.
* **Manual Review Service (Back-Office):** This service powers the vet's back-office application. It consumes from the `ClaimNeedsReview` queue, presenting a clear interface for vets to make a decision. When the vet submits their adjudication, this service publishes a `ClaimReviewed` event, which the Orchestrator consumes to resume the SAGA.
* **Model Feedback Service:** Service responsible for capturing manual corrections from the Back-Office to measure model accuracy. It stores the "before" (model output) and "after" (vet correction) to create a dataset for model retraining.

<img width="4040" height="4568" alt="claim-workflow-arch" src="https://github.com/user-attachments/assets/785b9445-bd72-48e5-b282-3e3685dab6d9" />


### High-Level Architectural Flow

To make this flow simpler, I am describing the "happy path" and the key "human-in-the-loop" deviation.

1.  The Mobile App sends documents and claim details to the **Claim Intake Service**. This service validates, saves the files, and publishes a `ClaimSubmitted` event.
2.  The **Claim Orchestrator** sees this event and starts a new SAGA. It first invokes the **Document OCR Service**.
3.  The **Document OCR Service** classifies the docs, extracts all data, and publishes a `ClaimDataExtracted` event.
4.  The **Claim Orchestrator** consumes this, saves the state, and then triggers multiple services in parallel:
    * It calls the **Pet History Service** to check for PXC matches.
    * It calls the **Policy & Coverage Service** to calculate coverage.
    * It calls the **Fraud Detector Service** to get a risk score.
5.  These services return their results (e.g., `PXCResultCalculated`, `CoverageCalculated`, `FraudScoreCalculated`).
6.  The **Claim Orchestrator** aggregates these results.
    * **Happy Path:** If the fraud score is low, PXC is clear, and confidence is high, the Orchestrator issues the final `ClaimDecisionApproved` (or `ClaimDecisionRejected`) event.
    * **HITL Path:** If the fraud score is high, PXC is ambiguous, or extraction confidence is low, the Orchestrator pauses the SAGA and publishes a `ClaimNeedsReview` event. The **Manual Review Service** picks this up for a vet. When the vet submits, it publishes `ClaimReviewed`, which the Orchestrator uses to issue the final decision.
7.  A **Payment Service**  listens for `ClaimDecisionApproved` events and processes the reimbursement.
8.  A **User Data Service** (or Analytics Sink) listens to *all* events (`ClaimSubmitted`, `ClaimDataExtracted`, `ClaimDecisionApproved`, etc.) and streams them into a **central data lake**. This provides a complete, real-time feed for analytics, product insights, and populating the back-office dashboard.
9.  The **Model Feedback Service** listens for `ClaimReviewed` events to log the vet's corrections and improve the models.

### Pros and Cons of This Approach

#### Advantages

* **Scalability:** Each service can be scaled independently. If OCR is the bottleneck, you can scale only that service.
* **Resilience:** If the Policy Service fails, it can be retried without affecting the OCR Service. The Orchestrator manages the retry logic, making the system fault-tolerant.
* **Extensibility:** Need to add a new check? You can build a new service and simply add it to the Orchestrator's workflow without touching the 10 other services.
* **Phased Automation:** You can launch with a high percentage of claims going to HITL (human-in-the-loop) and gradually "turn up" the automation as the models improve, all without changing the architecture.

#### Disadvantages

* **Development Complexity:** This architecture is significantly more complex to build and test than a single application. Managing distributed transactions (the SAGA pattern) requires careful design.
* **Debugging Challenges:** When a claim fails, it could be across multiple services. This requires robust distributed tracing to follow a claim's journey through the entire system.
* **Eventual Consistency:** Data is not updated instantly across the system. The "Pet History" might be a few seconds behind. This is generally acceptable for claims processing but must be a conscious design choice.
* **Operational Overhead:** You must build, deploy, monitor, and manage 5-10 services plus a message broker (like Kafka), which has its own operational demands.

## Alternative Architecture: The Scheduled Job (Batch Processing)

This architecture decouples the initial claim submission from the processing. The user gets an immediate response, and a background job processes the claims in batches. This is much simpler than an event-driven system but more robust than a fully synchronous one.

### How It Would Work

1.  **Claim Intake Service:** The user submits their claim. This service validates the input (file types, user auth), securely saves the documents to S3, and writes the claim's metadata to the **Database** with a status of `Pending`. 
2.  **Scheduled Job (e.g., Cron Job):** A scheduler triggers the **Claim Processing Service** at a regular interval (e.g., every 1 minute).
3.  **Claim Processing Service (The Batch):** This service is now a background worker. Its logic is:
    * **Fetch:** Query the `Database` for a batch of claims with `status = 'Pending'`.
    * **Loop:** For each claim in the batch:
        * Make a **synchronous, blocking call** to the `Document OCR Service`.
        * Make a **synchronous, blocking call** to the `Pet History Service`.
        * Make a **synchronous, blocking call** to the `Policy & Coverage Service`.
        * Make a **synchronous, blocking call** to the `Fraud Detector Service`.
    * **Decide & Update:** After gathering all responses for a claim, the service runs its decision logic and updates the claim's row in the `Database` to `Approved`, `Rejected`, or `PendingReview`.
4.  **Human-in-the-Loop:** The vet's back-office application queries the same `Database` for claims with the `PendingReview` status.

## My Implementation Plan: Augment First, Then Automate

My approach is grounded in the **LEAN methodology** and the **KISS (Keep It Simple, Stupid)** principle. I believe in delivering the most significant value to our customers as early as possible.

Therefore, I will not start with a complex, event-driven architecture. Instead, I will begin with a **Modular Monolith** and focus on **augmenting** our existing vet system. My strategy is to first build tools that reduce the team's manual burden, make their jobs easier, and *simultaneously* capture the feedback we need. Only after we have validated these tools and gathered sufficient data will we "flip the switch" to selective automation.

This is my phased plan to build the system incrementally, get value at every step, and collect feedback sooner rather than later.

### Phase 1: Build the Core Logic (Policy Engine)

My first priority is to tackle the most critical part of the process: the financial decision. This will immediately reduce human error and create a consistent audit trail.

* **Actions:** I will build the `Policy & Coverage Service` as an internal module. I'll then integrate this directly into the existing back-office app the vets already use.
* **New Workflow:** A vet will still manually type in the claim data, but instead of doing manual math, they will click a new **"Calculate Coverage" button**. The UI will instantly show them the exact, auditable calculation.
* **Value Delivered:** This delivers immediate value by eliminating guesswork, ensuring every decision is consistent, and giving vets more confidence.

### Phase 2: Augment Data Entry (OCR + Review)

Next, I'll attack the most time-consuming task: manual data entry.

* **Actions:** I will build the `Claim Processing Service` as a scheduled background job. This job will call a new `Document OCR Service` to extract data from uploaded documents and save it to the database, marking the claim `PendingReview`.
* **New Workflow:** The vet's queue will now be populated by these pre-processed claims. I will build a **new "Review" screen** that shows the document side-by-side with the pre-filled form fields. The vet's job is no longer *data entry*, but *data verification*.
* **Value Delivered:** This will dramatically cut claim processing time and free up our expert team from tedious typing.

### Phase 3: Build the Feedback Loop

This is a critical, parallel step. As vets correct the data from Phase 2, I need to capture their changes to start training our models.

* **Actions:** I'll build a `Model Feedback Service`. When a vet saves their corrections on the "Review" screen, this service will log the `diff` between the original OCR output and the vet's final "ground truth" data.
* **New Workflow:** This is invisible to the vet, but now their corrections are actively making our system smarter.
* **Value Delivered:** We start building our high-quality training dataset for free, simply by capturing the work our team is already doing.

### Phase 4: Add Intelligence in "Shadow Mode"

Now that I have clean, vet-reviewed data, I can start building our intelligent models and validating them in a zero-risk environment.

* **Actions:** I'll build the `Pet History Service` (to detect Pre-existing Conditions) and the `Fraud Detector Service`. I will add both to the `Claim Processing Service`.
* **New Workflow:** When a vet opens the "Review" screen, they will now see **new informational widgets**. For example: "Our model suggests this may be a PXC" or "Fraud Risk: LOW." These are *suggestions*, not decisions.
* **Value Delivered:** We empower our vets to make even better, more informed decisions while we simultaneously validate our models against their expert behavior.

### Phase 5: "Flipping the Switch" (Selective Automation)

Once I have analyzed the feedback from Phase 3 and see high confidence in our models, I will begin true automation.

* **Actions:** I will add the final decision logic into the `Claim Processing Service` job.
* **New Workflow:** The job will run its checks. **IF** the claim is low-risk (e.g., < $150, high OCR confidence, no PXC, low fraud score), the system will automatically call the `Policy Engine` and approve it. The claim *never even appears* in the vet's queue. All other claims are routed to `PendingReview` with a suggested decision and reimbursement amount.
* **Value Delivered:** This is the payoff. A large portion of simple claims become "touchless," freeing our vets to focus exclusively on the complex cases that require their expertise.

### Phase 6: Evolve the Architecture (When Needed)

The simpler modular architecture will serve us well, but as we scale, we might need to evolve it to an Saga event driven architecture.

## Future Features Enabled by Data Collection

* Breed-Specific Health Monitoring
* Local Outbreak Alerts
* Preventative Care Gap
* Generic & Alternative Suggestions
* Coverage "What-If" Simulator
