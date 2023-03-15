---
title: Logging best practices
---

###### I am always posting provoking post on my Linkedin, and I have decided to replicate those here. [Let's socialize!](https://www.linkedin.com/in/alexsandro-souza-dev)

A quick guide to help produce better logs in your applications.

**Record events at the correct level**

- DEBUG - Used for debugging.

- INFO - Informs expected system operation.

- WARN - A recoverable problem occurred, but with no user experience impact.

- ERROR - An unrecoverable problem occurred, with some user experience impact.

- FATAL - Indicates fatal errors. Usually, it means the end of the program.


**Best practices**


- On error, log all the available information to reproduce the issue

- When calling an external service, in case of error, log the request and response for the called service

- When calling an external service, log the request and response for the called service as DEBUG

- When the service fails to respond to a request, add the request-id as part of the error message response

- Log the service response data as DEBUG

- Log the service request data as INFO

- Log bad request as a WARNING

- Don’t log sensitive information. Make sure you never log (Passwords, Credit card numbers, Social security numbers…)

- Include the stack trace when logging exceptions

- Include the thread’s name when logging from a multi-threaded application

- Timestamps should be at millisecond resolution (YYYY-MM-DD HH:mm:ss.yyy)

- DateTime should be UTC

- Logged durations should be milliseconds

- Log in JSON format

- An error should be handled only once. Logging an error is handling an error. So an error should either be either logged or propagated(re throw), make sure you are handling it on the right layer/place

- Enable log level change at runtime. You should be able to change the log level when troubleshooting in prod

- Split logs of different levels to different targets to control their granularity. Even if using *WARN* as the production log level. The application start-up *INFO* should still be logged.

- Log all the important environment configuration info on start up. Ex. Service dependency URLs, server ports and etc.