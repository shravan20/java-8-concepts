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
