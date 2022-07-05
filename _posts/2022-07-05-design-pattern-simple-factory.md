---
title: 设计模式入门 - 简单工厂模式
layout: article
tags: java 设计模式
---

顾名思义，工厂是生产一系列同类产品的地方。

简单工厂模式是工厂模式的最简化版本，有的甚至不认为它算是一种设计模式。但是这里不讨论它是否设计模式，我们就学习它的用法。



## 组成

- 抽象产品：指某一类产品的抽象，比如"车"。
- 具体产品：抽象产品的具体实现，比如"小汽车"。
- 工厂：生产具体产品的工厂。


------



## 场景

- 抽象产品：玩具车
- 具体产品： 小汽车玩具、卡车玩具、巴士玩具
- 工厂：玩具车工厂

当我们向工厂说我们需要小汽车玩具时，工厂生产小汽车玩具给我们；
当我们说需要卡车玩具时，工厂生产卡车玩具给我们；
当我们说需要巴士玩具时，工厂生产巴士玩具给我们。

**抽象产品：**

```java

interface ToyCar {
    void toyInfo();
}

```

**具体产品：**

```java

class CarToys implements ToyCar {
    @Override
    public void toyInfo() {
        System.out.println("汽车玩具");
    }
}

```

```java

class TruckToys implements ToyCar {
    @Override
    public void toyInfo() {
        System.out.println("卡车玩具");
    }
}

```

```java

class BusToys implements ToyCar {
    @Override
    public void toyInfo() {
        System.out.println("巴士车玩具");
    }
}

```

**工厂：**

工厂的获取方法也可以是静态方法，如果是静态方法就可以直接调用，不需要实例化工厂类。

```java

class CarToyFactory {
    public ToyCar get(String carType) {
        switch (carType) {
            case "car":
                return new CarToys();
            case "truck":
                return new TruckToys();
            case "bus":
                return new BusToys();
            default:
                throw new RuntimeException("不生产" + carType + "玩具车");
        }
    }
}

```


------



## 使用

首先实例化一个工厂对象，然后通过工厂分别获取小汽车玩具、卡车玩具、巴士玩具以及一个不存在的车类玩具。

正常情况下，前面三个玩具车可以正常获取，但是获取不存在的车类玩具时就会报错（根据代码而定，也可以返回 `null` 值）。

```java

public static void main(String[] args) {
    // 实例化工厂
    ToyCarFactory factory = new ToyCarFactory();
    // 向工厂获取汽车玩具对象
    ToyCar toyCar = factory.get("car");
    toyCar.toyInfo();
    // 向工厂获取卡车玩具对象
    ToyCar truckCar = factory.get("truck");
    truckCar.toyInfo();
    // 向工厂获取巴士玩具对象
    ToyCar busCar = factory.get("bus");
    busCar.toyInfo();
    // 向工厂获取它无法生产的对象（此时会报错）
    ToyCar unknownCar = factory.get("OptimusPrime");
}

```
