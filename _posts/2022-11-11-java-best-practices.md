---
title: Java Best Practices Quick Reference
---
Developers have a big responsibility to make the right decision every day and the best thing to help make good decisions is the experience. Not everyone has long experience in software development, but everyone can leverage others' experience. These are some tips that I have acquired with my experience in working with Java and I hope it can help you to improve the readability and reliability of your Java code.

## Programming Principles

Don’t write code that the only  _works_. Aim to write code that can  _be maintained_  — not only by yourself but by anyone else who may end up working on the software at some point in the future.

80% of the time a developer is reading code and 20% writing and testing the code. So, please focus on writing readable code!

Your code should not need comments to understand what it is doing!

To help us to develop good code, there are many programming principles that we can use as guidelines. Below I will list the most important ones.

-   KISS — It stands for “Keep It Simple, Stupid”. You may notice that developers at the beginning of their journey try to implement complicated, ambiguous design.
-   DRY — “Don’t Repeat Yourself”. Try to avoid any duplicates, instead, you put them into a single part of the system or a method.
-   [YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) — “You Ain’t Gonna Need It”. If you run into a situation where you are asking yourself, “What about adding extra (feature, code, …etc.) ?”, you probably need to re-think it.
-   [Clean code over clever code](https://guifroes.com/clever-code-is-bad/) — Speaking of clean code, leave your ego at the door, and  **forget about writing clever code**.
-   [Avoid premature optimization](https://stackify.com/premature-optimization-evil/) — The problem with premature optimization is that you can never really know where a program’s bottlenecks will be until after the fact.
-   [Single responsibility](https://medium.com/@severinperez/writing-flexible-code-with-the-single-responsibility-principle-b71c4f3f883f)  — Every class or module in a program should only concern itself with providing one bit of specific functionality.
-   [Composition over Inheritance](https://dzone.com/articles/good-practices-interface-overuse)  — Objects with complex behaviors should do so by containing instances of objects with individual behaviors rather than inheriting a class and adding new behaviors.
-   [Object calisthenics](https://medium.com/r?url=https%3A%2F%2Fjavflores.github.io%2Fobject-calisthenics%2F) — Object Calisthenics are  **programming exercises**, formalized as a set of  [9 rules](https://javflores.github.io/object-calisthenics/)
-   [Fail fast, fail hard](https://www.martinfowler.com/ieeeSoftware/failFast.pdf) **—** The fail-fast principle stands for stopping the current operation as soon as any unexpected error occurs. Adhering to this principle generally results in a more stable solution

## Packages

1.  Favor structuring packages by  [domain concerns](https://mnapoli.fr/organizing-code-into-domain-modules/)  rather than technical layers.
2.  Favor layouts that promote encapsulation and information hiding to protect against improper usage over organizing classes by technical concerns.
3.  View packages as providing a strict API — do not expose the inner workings (classes) that are meant for internal processing only.
4.  Not  _public_  access scope for classes that are supposed to be only used inside the package.

## Classes

### Static

1.  Do not allow instantiation of a static class. Always create a private constructor.
2.  Static classes should be stateless, immutable, not allow subclassing, and thread-safe.
3.  Static classes should be side-effect free and provided as utilities, such as filtering a list.

### Inheritance

1.  Prefer composition over inheritance.
2.  Do not expose  _protected_  fields. Provide a  _protected_  accessor instead.
3.  If a class variable can be marked  **final**, make it.
4.  If inheritance is not expected, make the class  **final**.
5.  Mark a method  **final**  unless subclasses are expected to be allowed to override it.
6.  If no constructor is required do not create a default one with no implementation logic. Java will automatically provide a default constructor if none is specified.

### Interfaces

1.  Do not use the constant interface pattern (an interface of constants) as it allows classes to implement and dirty up the API. Use a static class instead. This has the added benefit of allowing you to perform more complex object initialization in a static block (such as populating a Collection).
2.  Avoid  [Interface overuse](https://blog.hovland.xyz/2017-04-22-stop-overusing-interfaces/)
3.  Having  **one and only one** class implement an interface is likely to overuse interfaces and it does more harm than good.  [More](https://tamasgyorfi.net/tag/interface-overuse/)
4.  **“Program to an interface, not to an implementation”** doesn’t mean you should pair each and every one of your domain classes with a more or less identical interface, doing so, you are violating  [YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it)
5.  Always keep interfaces small and specific so that clients will only have to know about the methods that are of interest to them. Check out the ISP from SOLID.

### Finalizers

1.  Object#finalize() should be used judiciously and only as a fail-safe for resource clean-up (e.g. closing a file). Always provide an explicit clean-up method (e.g. close()).
2.  In an inheritance hierarchy, always call the parent’s finalize() within a  _try_  block. The class’s cleanup should be in the  _finally_  block.
3.  If the explicit clean-up method was not called and the finalizer closed the resources, then log this error.
4.  If a logger is not accessible then use the Thread’s exception handler (which eventually delegates to  _standard error_  which is captured in the logs).

## General

### Assertions

An assertion, usually in the form of a precondition check, enforce the type’s contract in a  _fail-fast, fail-hard_  manner. They should be used liberally to catch programming errors as close to the source as possible.

Object state:

-   An object should never be constructed or transition into an invalid state.
-   On constructors and methods, always describe and enforce the contract through validations.
-   The Java keyword  **assert**  should be avoided as it can be disabled and is generally a brittle construct.
-   Use the  _Assertions_  utility class to avoid verbose  _if-else_  conditions for precondition checks.

### Generics

A full, extremely detailed explanation is available in the  [Java Generics FAQ](http://www.angelikalanger.com/GenericsFAQ/JavaGenericsFAQ.html). Below are the common scenarios that developers should be aware of.

1.  When possible, prefer using type inference rather than returning a base class/interface:
```
// MySpecialObject o = MyObjectFactory.getMyObject();
public <T extends MyObject> T getMyObject(int type) { 
 return (T) factory.create(type);
}
```

2. When a type cannot automatically be inferred, inline it.
```
public class MySpecialObject extends MyObject<SpecialType> {
 public MySpecialObject() {
 super(Collections.emptyList());   // This is ugly, as we loose type
 super(Collections.EMPTY_LIST();    // This is just dumb
 // But this is beauty
 super(new ArrayList<SpecialType>()); 
 super(Collections.<SpecialType>emptyList());
 }
}
```
3. Wildcards:
   Use an  **extended**  wildcard when you only get values out of a structure, use a  **super**  wildcard when you only put values into a structure and don’t use a wildcard when you do both.

1.  Everyone loves  [PECS](https://wiki.deem.com/download/attachments/17497/TS-6623.pdf?version=1&modificationDate=1219090255000&api=v2)! (_Producer-extends, Consumer_-super)
2.  Use  _Foo<? extends T>_  for a T producer.
3.  Use  _Foo<? super T>_  for a T consumer.

## Singletons

A singleton should never be written in the classic  _Design Patterns_  style, which is quite valid in C++ (as it was written with) but inappropriate in Java.

1.  While correctly thread-safe, never implement as follows. (This has been a performance bottleneck!)
```
public final class MySingleton {
 private static MySingleton instance;
 private MySingleton() {
   // singleton
 }

 public static synchronized MySingleton getInstance() {
 if (instance == null) {
   instance = new MySingleton();
 }
   return instance;
 }
}
```
2. If lazy initialization is truly desirable, then a combination of the two approaches will do the job!
```
public final class MySingleton {
 private MySingleton() {
   // singleton
 }
 private static final class MySingletonHolder {
   static final MySingleton instance = new MySingleton();
 } 

 public static MySingleton getInstance() {
   return MySingletonHolder.instance;
 }
}
  ```

3. Spring: By default, a bean is registered with a singleton scope, meaning that only one instance will be created by the container and be wired to all consumers. This provides the same semantics as a normal Singleton, without the performance or coupling limitations.

## Exceptions

1. Use checked exceptions for recoverable conditions and run-time exceptions for programming errors. Example: Getting an Integer from a String.

-   Bad: NumberFormatException extends RuntimeException, so it is meant to indicate programming errors.
-   Do not do the following:

```
// String str = input string
Integer value = null;
try {
 value = Integer.valueOf(str);
} catch (NumberFormatException e) {
  // non-numeric string
}
if (value == null) {
  // handle bad string
} else {
   // business logic
}
```
-   Correct usage:
```
// String str = input string

// Numeric string with at least one digit and optional leading negative sign
if ( (str != null) && str.matches("-?\\d++") ) { 
 Integer value = Integer.valueOf(str);
 // business logic
} else {
  // handle bad string
}
```

2. You should handle exceptions in the right place, the right place is at the domain level.

-   **WRONG WAY — The data**  object layer doesn’t know what to do when there is a database exception.
```
class UserDAO{
 public List<User> getUsers(){
 try{
   ps = conn.prepareStatement("SELECT * from users");
   rs = ps.executeQuery();
   //return result
 }catch(Exception e){
   log.error("exception")
   return null
   }finally{
     //release resources
   }
  }
 }
```

-   **RECOMMENDED WAY**  — The data layer should just rethrow the exception and transfer the responsibility to handle the exception or not to the right layer.


```
class UserDAO{
 public List<User> getUsers(){

 try{
   ps = conn.prepareStatement("SELECT * from users");
   rs = ps.executeQuery();
   //return result
   }catch(Exception e){
         throw new DataLayerException(e);
     }finally{
       //release resources
    }
  }
}
```

3. Exceptions should in general NOT be logged at the point they are thrown, but rather at the point they are actually handled. Logging exceptions when they are thrown or rethrown tends to fill the log files with noise. Also, note that the exception stack trace captures where the exception was generated anyway.

4. Favour the use of standard exceptions

5. Use Exceptions rather than Return codes.

## Equals and HashCode

There are a number of concerns to be aware of for writing proper object equivalence and hash code methods. To simplify usage, use java.util.Objects’  _equals_  and  _hash_.

```
public final class User {
 private final String firstName;
 private final String lastName;
 private final int age;
 ...
 public boolean equals(Object o) {

 if (this == o) {
   return true;
 } else if (!(o instanceof User)) {
   return false;
 }
 User user = (User) o;
 return Objects.equals(getFirstName(), user.getFirstName()) && 
 Objects.equals(getLastName(),user.getLastName()) &&
 Objects.equals(getAge(), user.getAge());
 }

 public int hashCode() {
   return Objects.hash(getFirstName(),getLastName(),getAge());
 }
}
```

## Resource Management

1.  Methods for safely releasing resources:
2.  The  _try-with-resources_  statement ensures that each resource is closed at the end of the statement. Any object that implements java.lang.AutoCloseable, which includes all objects which implement  [java.io](http://java.io/).Closeable, can be used as a resource.

```
private doSomething() {

try (BufferedReader br = new BufferedReader(new FileReader(path))) {

 try {
   // business logic
 }
}
```

## Provide Shutdown Hooks

Provide a  _shutdown hook_  to be called if the JVM is gracefully terminated. (This will not handle abrupt terminations, such as due to a power outage)

This is the recommended alternative instead of declaring a  **finalize()**  method, which will only be run if  _System.runFinalizersOnExit()_  is true (by default it is false).

```

public final class SomeObject {
 var distributedLock = new ExpiringGeneralLock ("SomeObject", "shared");
 public SomeObject() {

 Runtime
  .getRuntime()
 .addShutdownHook(new Thread(new LockShutdown(distributedLock)));
 }

 /** Code may have acquired lock across servers */
 ...

 /** Safely releases the distributed lock. */

 private static final class LockShutdown implements Runnable {

 private final ExpiringGeneralLock distributedLock;

 public LockShutdown(ExpiringGeneralLock distributedLock) {
 if (distributedLock == null) {
   throw new IllegalArgumentException("ExpiringGeneralLock is null");
 }
   this.distributedLock = distributedLock;
 }
 public void run() {
  if (isLockAlive()) {
   distributedLock.release();
  }
 }
 /** @return True if the lock is acquired and has not expired yet. */

 private boolean isLockAlive() {
   return distributedLock.getExpirationTimeMillis() > System.currentTimeMillis();
 }
 }
}
```
_Allow resources to expire (and also be renewable) is shared between servers. (This allows recovery from abrupt termination, such as power outages)._

_See code sample above, which uses an ExpiringGeneralLock (a lock that is shared across systems)._

## Date-Time

Java 8 introduces a new date-time API under the package java.time. With Java 8, a new Date-Time API is introduced to cover the following drawbacks of old date-time API: Not thread-safe, Poor design, Difficult time zone handling and etc.

## Concurrency

### General

1.  Beware of the following libraries, which are surprisingly not thread-safe. Always synchronize against the objects if shared between multiple threads.
2.  Date (_not immutable_) — Use the new Date-time API that is thread-safe;
3.  SimpleDateFormat — Use the new Date-time API that is thread-safe;
4.  Prefer using  _java.util.concurrent.atomic_  classes over making variables  **volatile**.
5.  The behavior of the atomic classes is more obvious to the average developer, whereas  **volatile**  requires understanding the Java Memory Model.
6.  The atomic classes wrap  **volatile**  variables in a more user-friendly interface.
7.  Understand the use-cases where  **volatile**  is appropriate. (see  [article](http://www.ibm.com/developerworks/java/library/j-jtp06197.html))
8.  Use Callable<Void> when requiring a checked exception but there is no return type. As Void cannot be instantiated, this communicates the intent and can return  _null_  safely.

### Threads

1.  _java.lang.Thread_  should be considered depreciated. While it is not, officially, in almost all instances the  _java.util.concurrent_  package provides a cleaner solution to the problem.
2.  It is considered poor practice to  **extend** _java.lang.Thread_  — instead  **implement** _Runnable_  and create a new thread with the instance in the constructor (rule of composition over inheritance).
3.  Prefer executors and streams when required concurrent processing
4.  It is always a good idea to specify your own custom thread factory to control the configuration of the threads being created.  [More](https://wilddiary.com/understanding_custom_threadfactory_in_java/)
5.  Use DaemonThreadFactory in Executors for non-critical threads so that the thread pool can be shut down immediately on server shutdown.  [more](https://www.baeldung.com/java-daemon-thread)

```

this.executor = Executors.newCachedThreadPool((Runnable runnable) -> {

 Thread thread = Executors.defaultThreadFactory().newThread(runnable);
 thread.setDaemon(true);
 return thread;
});
```

1.  Java synchronization is no longer slow (55–110ns). Do not avoid it by using broken tricks like  _double-checked locking_.
2.  Prefer synchronizing against an internal object, rather than the class, as users may synchronize against your class/instance.
3.  Always synchronize multiple objects in the same order to avoid deadlocks.
4.  Synchronizing against the class does not inherently block access to its internal objects. Always use the same locks when accessing a resource.
5.  Beware that the  **synchronized**  keyword is not considered part of a method’s signature and will thus not be inherited.
6.  Avoid excessive synchronization, it can cause reduced performance and deadlock. Use the  **synchronized**  keyword strictly to the piece of code that requires synchronization.

### Collections

1.  Use Java-5 concurrent collections when possible in multi-threaded code. These are safe and have superior performance.
2.  Use CopyOnWriteArrayList over synchronizedList when suitable
3.  Use Collections.unmodifiable list(…) or copy the collection when receiving it as a parameter  _new ArrayList(list)_. Avoid having local Collections changed from outside your class.
4.  Always return a copy of your collection, avoiding your list be changed from outside the  _new ArrayList(list)_
5.  Each collection should get wrapped in its own class, so now  **behaviors related to the collection have a home**  (e.g. filter methods, applying a rule to each element).

## Miscellaneous

1.  Prefer lambdas to anonymous classes
2.  Prefer method references to lambdas
3.  Use enums instead of int constants.
4.  Avoid use float and double if exact answers are required, use BigDecimal instead, ex Money
5.  Prefer primitive types to boxed primitives
6.  The use of magic numbers in the code should be avoided. Use constants
7.  Don’t return Null. Communicate with your method client with `Optional`. The same for Collections — Return empty arrays or collections, not nulls
8.  Avoid creating unnecessary objects, reuse objects, and avoid unnecessary GC clean up

## Lazy Initialization

Lazy initialization is a performance optimization. It’s used when data is deemed to be “expensive” for some reason. With Java 8 we should use the Supplier functional interface for that.

```

== Thread safe Lazy initialization ===

public final class Lazy<T> {
 private volatile T value;
 public T getOrCompute(Supplier<T> supplier) {
   final T result = value; // Just one volatile read
   return result == null ? maybeCompute(supplier) : result;
 }

 private synchronized T maybeCompute(Supplier<T> supplier) {
 if (value == null) {
   value = supplier.get();
 }
   return value;
 }
}
Lazy<String> lazyToString= new Lazy<>()
return lazyToString.getOrCompute( () -> "(" + x + ", " + y + ")");
  ```

That's it for now, hope it was useful!

---

### Free Advanced Java Course
I am the author of the [Advanced Java for adults course](https://www.udemy.com/course/advanced-java-for-adults). This course contains advanced and not conventional lessons. In this course, you will learn to think differently from those who have a limited view of software development. I will provoke you to reflect on decisions that you take in your day to day job, which might not be the best ones. This course is for middle to senior developers and we will not teach Java language features but how to lead complex Java projects.

This course's lectures are based on a Trading system, an opensource project hosted on my [Github](https://github.com/apssouza22/trading-system).