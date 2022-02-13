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
