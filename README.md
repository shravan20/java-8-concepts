# Guide to Java 8 Features


 This repository covers features released in Java 8
 Repository contains simple definition of the feature with simplest code snippet

---

<p align="center">  Feel free to contribute to repository </p>

---

## Table of Content

- [Built-in Functional Interface](#functional-interface)
  - [UnaryOperator](#unaryoperator)
  - [BinaryOperator](#binaryoperator)
  - [Function](#function)
  - [Predicate](#predicate)
  - [Supplier](#supplier)
  - [Consumer](#consumer)

- [Default Methods for Interfaces](#default-methods-for-interfaces)
- [Lambda Expressions](#lambda-expression)
- [Method/Constructor References](#methodconstructor-references)
- [Optional](#optional)
- [Streams](#streams)

---

### Functional Interface

`@FunctionalInterface` was introduced with Java 8, which has only one abstract method. The compiler treats any interface meeting the definition of a functional interface as a functional interface, i.e, `@FunctionalInterface` annotation was optional

Examples:


```java
@FunctionalInterface
interface Converter<F, T> {
    T to(F from);
}
```

```java
Converter<String, Integer> converter = (from) -> Integer::valueOf;
Integer converted = converter.to("999");
System.out.println(converted);    // 999
```

`Note:` Even without the `@FunctionalInterface`, it is valid as it meets the definition of **Functional Interface**

- ### UnaryOperator

`UnaryOperator` is a functional interface which extends `Function`. UnaryOperator takes one argument and returns result of the same data type of the argument passed.

Examples:

```java
@FunctionalInterface
public interface UnaryOperator<T> extends Function<T, T> {
}
```

```java
UnaryOperator<Integer> sqr = x -> x * x;
Integer result = sqr.apply(5);
System.out.println(result);    // 25
```


- ### BinaryOperator

`BinaryOperator` is functional interface which extends `BiFunction`. It takes two arguments of the same type and return a result of the same type of the arguments passed.

**Note: BiFunction is a functional interface which accepts two arguments and produces result, i.e., 2-arity specialization.**

Examples:

```java
@FunctionalInterface
public interface BinaryOperator<T> extends BiFunction<T,T,T> {
}
```

```java
BiOperator<Integer> add = (x,y) => x + y;
Integer result = add.apply(5,5);
System.out.println(result); // 10

```

- ### Function

In Java 8, Function is a functional interface; it takes an argument (object of type T) and returns an object (object of type R). The argument and output can be a different type.


`Function` is a functional interface in Java 8, which takes an argument (object of type T) and returns an object of type R. Here the argument passed and output can be of different types.

Examples:

```java
@FunctionalInterface
public interface Function<T, R> {
      R apply(T t);
}
```

```
Function<String, Integer> func = x -> x.length();
Integer size = func.apply("Helloo");   // 6
System.out.println(size); // 6
```

- ### Predicate

`Predicate` is a functional interface, which only allows one abstract method within the interface scope. It contains various default methods for composing predicates to complex logic terns [and, or, negate].


Examples:

```java
Predicate<String> predicate = (str) -> str.length() > 0;

predicate.test("shravan20");               // true
predicate.negate().test("shravan20")       // false
```

- ### Supplier

`Supplier` is a functional interface; which takes no arguments and returns a result. Unline Functions, Suppliers do not accept arguments. 

Examples:

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```

```java
Supplier<Double> randomValue = () -> Math.random();
System.out.println(randomValue.get());   // prints some random number
```

- ### Consumer

`Consumer` represent operations to be performed on a single input argument. Consumer interface consists of the following two functions:
 - void accept(T t) // accepts one value and performs the operation on the given argument
 - default Consumer <T> andThen(Consumer<? super T> after) //returns a composed Consumer wherein the parameterized Consumer will be executed after the first one


Examples:

```java

/**
* Assume a class Person with firstName and lastName as properties
*/

Consumer<Person> greeter = (p) -> System.out.println("Hello, " + p.firstName);
greeter.accept(new Person("Luke", "Skywalker")); // Hello Luke
```



### Default Methods for Interfaces


Java 8 allows us to add non-abstract methods in the interfaces. These methods must be declared default methods. These were introduced to enable the functionality of lambda expression.

Prior to Java 8, if a new method was introduced in an interface then all the implementing classes used to break, as we would need to provide the imlementation of that method in all the implementing classes.

Sometimes methods have nly single implementation and there is no need to provide their implementation in each class. In such cases, we declare that method as default method in the interface and provide its implementation in the interface itself.

Examples:

```java
public interface Vehicle {

  void clean();
  
  default void start() {
    System.out.println("Start vehicle");
  }

}


public class Car implements Vehicle {

    @Override
    public void clean() {
        System.out.println("Clean the vehicle");
    }
 
    
    public static void main(String []args) {
      Car car = new Car();
      car.clean();  // Clean the vehicle
      car.start();  // Start Vehicle
    }

}

```


### Lambda Expression

With the introduction of Lambda expressions in Java 8, Java now supports Higher Order Functions. The main concept in Lambda calculus is the expression. An expression can be expressed as:

```math
<expression> := <variable> | <function>| <application>
```

You can't use any arbitrary interface with lambda expressions. Only those interfaces which have only one non-object abstract method can be used with lambda expressions, i.e., can only be used with `FunctionalInterface`.

Examples:

```java
interface MyMath {
    int getDoubleOf(int a);
}
	
MyMath d = a -> a * 2; // associated to the interface
d.getDoubleOf(4); // is 8
```

### Method/Constructor References

Java 8 enables you to pass references of methods or constructors via the `::` keyword. A reference to a constructor takes the following syntax: `ClassName::new`. As constructor in Java is a special method, method reference could be applied to it too with the help of new as a method name.

Whereas, the reference to an instance method holds the following syntax: `containingInstance::methodName`.

Examples:
```java

// Reference to instance method
User user = new User();
boolean isLegalName = list.stream().anyMatch(user::isLegalName);


// Reference to an Instance Method of an Object of a Particular Type
long count = list.stream().filter(String::isEmpty).count();


// Reference to a Constructor
Stream<User> stream = list.stream().map(User::new);
```

### Optional

The new Optional class is one of the most interesting feature introduced in Java 8 to handle `NullPointerExeception(NPE)`. Essentially a wrapper class that contains an optional value, i.e., it can contain an value or can be empty.

Prior to Optional class, if we have to print simple statement like `System.out.println(user.getAddress().getCountry().getIsocode().toUpperCase())`, we had to nest it under several conditional clauses to avoid NPE, as shown below:

```java

if (user != null) {
    Address address = user.getAddress();
    if (address != null) {
        Country country = address.getCountry();
        if (country != null) {
            String isocode = country.getIsocode();
            if (isocode != null) {
                System.out.println(user.getAddress().getCountry().getIsocode().toUpperCase());
            }
        }
    }
}
```

Examples/Usages:

 - Creating Optional Instances:
  ```java
	// Creates an empty Optional
	Optional<User> emptyOpt = Optional.empty();
	emptyOpt.get();
	
	
       	// Creates an Optional with value
        Optional<User> opt = Optional.of(user);

	// If object can be both null or not null
	Optional<User> opt = Optional.ofNullable(user);
  ```

- Accessing the Value of Optional Objects
 ```java
	// Using get()
	String name = "John";
	Optional<String> opt = Optional.ofNullable(name);
	System.out.print(opt.get()); // John ; if value did not exist would have printed null
	
	
	// to return default values
	User user = null;
	User user2 = new User("anna@gmail.com", "1234");
	User result = Optional.ofNullable(user).orElse(user2);
```

- Other methods:

```java
Optional<String> optional = Optional.of("bam");

optional.isPresent();           // true
optional.get();                 // "bam"
optional.orElse("fallback");    // "bam"

optional.ifPresent((s) -> System.out.println(s.charAt(0)));     // "b"
```

### Streams

In simple words, **streams** are wrappers around a data source, allowing us to operate with that data source and making bulk processing convenient and fast. A stream does not store data and, in that sense, is not a data structure. It also never modifies the underlying data source. Streams are Monads, thus playing a big part in bringing functional programming to Java.

Stream operations are either intermediate or terminal. Intermediate operations return a stream so we can chain multiple intermediate operations without using semicolons. Terminal operations are either void or return a non-stream result.

Most stream operations accept some kind of lambda expression parameter, a functional interface specifying the exact behavior of the operation. Most of those operations must be both non-interfering and stateless.


>A function is **non-interfering** when it does not modify the underlying data source of the stream.
>A function is stateless when the execution of the operation is deterministic.


A Java Stream is a component that is capable of internal iteration of its elements, meaning it can iterate its elements itself. There are 2 terminologies which are very important, viz.,
- **Terminal Operations:** The operations which return other than stream are called terminal operations. [count(). min(), max() ..etc]
- **Non Terminal/Intermediate Operations:** The operations which return stream themselves are called intermediate operations. [filter(), map(), sorted()..etc]


#### Creating Streams
- **Stream.of():**
To create streams from the specified values, we can use `Stream.of(T…t)` method that creates specified t values, where t are the elements. This method returns a sequential Stream containing the t elements.

Example:
```java
	Stream<Integer> numbers = Stream.of(1, 2, 3, 4, 5, 6);
        // Displaying the sequential ordered stream
	numbers.forEach( number -> System.out.print(number + " ")); // 1 2 3 4 5 6
```

- **Stream.of(array):**
Two commonly used methods for creating a sequential stream from a specified array are 
  - `Stream.of`
  - `Arrays.stream`
These are used to create a sequential stream from specified array. Both these methods returns a Stream when called with a non-primitive type T.

Example:
```java
	String[] arr = new String[] { "a", "b", "c" };
	Stream<String> streamArr = Arrays.stream(arr);
```
- List.stream()
- Stream.generate() or Stream.iterate()
- Stream of String chars or tokens


#### Stream Collectors

- Collect Stream elements to a List
- Collect Stream elements to an Array




