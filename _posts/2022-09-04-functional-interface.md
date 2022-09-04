---
title: 函数式接口
layout: article
tags: Java 函数式接口
---
自 JK1.8 开始， Java 提供了函数式接口以及函数式编程。

`函数式接口` 指的是只有一个抽象方法的接口。默认方法以及 `Object` 的所有公众的方法都不算在内。

函数式接口需要通过 `@FunctionalInterface` 注解来标识接口是函数式接口，并且需要满足只有一个抽象方法的要求。

## @FunctionalInterface 注解

通过这个注解来表明某个接口是符合 Java 要求的函数式接口。

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface FunctionalInterface {}
```

通过注解信息可以看到，这个注解只允许只允许在类型上做标识，且在运行时才有效。

## 定义一个函数式接口

通过函数式接口的要求可以知道，一个函数式接口必须有一个抽象方法，并且这个接口要添加 `@FunctionalInterface` 注解。

那么，现在通过这个定义来创建一个简单的函数式接口：这个函数式接口接收一个字符串。

- 接口名为：Receiver
- 只有一个抽象方法，且接收一个字符串参数，方法名为：receive

```java
@FunctionalInterface
public interface Receiver {
    // 接收一个字符串
    void receive(String arg);
}
```

## 使用自定义的函数式接口

函数式接口正常都是在使用的时候根据情况做不同的实现，上面我们自己定义的函数式接口 `Receiver` 能够接收一个字符串参数，那么我们可以根据需要对这个字符串进行处理或者直接输出等操作。

现在，我们使用 `Receiver` 直接输出接收的参数：

```java
//1. 实现函数式接口
Receiver stringReceiver = new Receiver() {
    @Override
    public void receive(String arg) {
        System.out.println(arg);
    }
};
//2. 调用函数式接口
stringReceiver.receive("hello, functional!");
```

由于函数式接口只有一个抽象方法，并且 JDK1.8 对于 `Lambda` 表达式的支持，上面的代码可以简化为：

```java
//1. 实现函数式接口
Receiver stringReceiver = (arg) -> {System.out.println(arg);};
//2. 调用函数式接口
stringReceiver.receive("hello, functional!");
```

然后，由于这个函数式接口只有一个参数，可以将参数的括号去掉括号：

```java
//1. 实现函数式接口
Receiver stringReceiver = arg -> {System.out.println(arg);};
//2. 调用函数式接口
stringReceiver.receive("hello, functional!");
```

然后，这个实现只有一个代码语句，可以将实现代码段的花括号去掉：

```java
//1. 实现函数式接口
Receiver stringReceiver = arg -> System.out.println(arg);
//2. 调用函数式接口
stringReceiver.receive("hello, functional!");
```

然后，由于 `System.out.println(Object x)` 方法正好也符合接收一个字符串的方法，我们可以将这个**方法引用**作为 `Receiver` 中 `receive(String arg)` 的实现：

```java
//1. 实现函数式接口
Receiver stringReceiver = System.out::println;
//2. 调用函数式接口
stringReceiver.receive("hello, functional!");
```

## 改进：接收任意类型参数

由于上面的函数式接口只能接收参数为字符串类型，如果想要根据实际情况接收不同的参数，可以使用泛型作为参数来实现：

```java
@FunctionalInterface
public interface Receiver<T> {
    // 接收一个任意类型的参数
    void receive(T arg);
}
```

```java
//1. 实现函数式接口
Receiver<String> stringReceiver = System.out::println;
Receiver<Integer> intReceiver = System.out::println;
//2. 调用函数式接口
stringReceiver.receive("hello, functional!");
intReceiver.receive(8);
```

## 总结

在实现函数式接口的时候，我们可以通过 Lambda 对函数式接口的实现进行代码上的简化，解和 Lambda 以及现有类的方法，我们甚至可以不需要真正实现，只需要引用其他已经实现的方法作为函数式接口的实现即可。

通过对函数式接口的定义、使用，可以看到函数式接口可以和 Lambda 表达式以及现有类的方法解和使用，使得代码更加灵活、简洁。D
