### Aspect Oriented programming
Reduces code scattering and makes classes cleaner.

**Terms used**

*Aspect*
  - A module of code for cross-cutting concerns.

*Advice*
  - What action is taken and when it should be applied.

  **Types of advices**
  1. `before advice` - runs before method
  2. `after finally advice` - runs after the method(finally)
  3. `after returning advice` - runs after method(successful execution).
  4. `after throwing advice` - runs after method(after throwing an exception)
  5. `around advice` - runs before and after method.

*Joint point*
  - When to apply code during program execution

*Point Cut*
  - A predicate expression for where advice should be applied.


### Point cut expression language
*execution*
  - execution(
                modifiers-pattern
                return-type pattern?
                declaring-type-pattern
                method-name-pattern(param-pattern)?
                throws-pattern
                )
  - example -- 
    ```java
      @Before("execution(public void addNumber())")
    ```
   -   Optional if it has the ? wildcard
        >  modifier pattern
        >  declaring-type
        >  throws-pattern 

    - example
   ```java 
        @After("execution(public void com.aspects.Aspect.onlineStuff())")
        // for any method starting with add
        @After("execution(public void add*())")
        @Before("execution(public * processCreditCard* ())")
        // Matching the method params
        mathod(*) // Method with one param
        method(..) // method with more than one param
        mmethod(param1-type,..) // more than one paramether(comma-separated)
   ```
   
   *DONE FOR TONIGHT*

### PointCut declarations
- to share declarations between advices, use pointCuts
- format
```java
@PointCut("execution(** addUser(..))")
private void pointCut(){}

@Before("pointCut()")
```
- pointCuts can be combined using logical expressions 
  - or
  - and 
  - not
  ```java
      @PointCut("execution(** addUser(..))")
      private void pointCut(){}

      @PointCut("execution(* String *(..))")
      private anyStringReturningMethod(){}

      @Before("pointCut() && anyStringReturningMethod()")
  ```


### Ordering Aspects
- When more than one advice are affecting one method at the same time.
- Can be mitigated by using multiple aspects and setting orders on them
- The order with the lower number gets called first.
  ```java
        @Component
        @Aspect
        @Order(2)
        public class SecondAspect {
            @Before("execution(public void run*(..))")
            public void secondAdvice(){
                System.out.println("Inside before of second aspect");
            }
        }
  ```
- Ordering can be assigned even to negative numbers.
- Range -> -infinity to +infinity

### Accessing method arguments from joint-points
- accessing method signature
  - use Join Point as part of advice args
  ```public void advice(JoinPoint joinPoint)```
  - call join point to access signature
  ```MethodSignature sig = (MethodSignature)joinPoint.getSignature()```

  - to get args use the joinPoint
  ```Object[] obj = joinPoint.getArgs()```
