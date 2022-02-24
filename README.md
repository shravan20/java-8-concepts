## Guide to Java 8 Features


 This repository covers features released in Java 8
 Repository contains simple definition of the feature with simplest code snippet

---

<p align="center">  Feel free to contribute to repository </p>

---

## Table of Content

- [Built-in Functional Interface](#functional-interface)
  - [UnaryOperator]()
  - [BinaryOperator]()
  - [Function]()
  - [Predicate]()
  - [Supplier]()
  - [Consumer]()

- [Default Methods for Interfaces]()
- [Lambda Expressions]()
- [Method/Constructor References]()

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


- ### Binary Operator

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






Now, with the introduction of Lambda expressions in Java 8, Java supports higher order functions. Let us look at the canonical example of Lambda expression -- a sort function in Java's Collections class. The sort function has two variants -- one that takes a List and another that takes a List and a Comparator. The second sort function is an example of a Higher order function that accepts a lambda expression as shown below in the code snippet.
