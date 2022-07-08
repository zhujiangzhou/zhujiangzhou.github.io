---
title: 设计模式入门 - 工厂方法模式
layout: article
tags: Java 设计模式 
---

工厂方法模式将对象的实例化交给工厂实现类决定。


## 组成

- 抽象产品：指某一类产品的抽象
- 具体产品：抽象产品的具体实现，由具体工厂创建
- 抽象工厂：所有工厂的父类（接口）
- 具体工厂：抽象工厂的实现，也是创建具体产品的类



------



## 场景

有一家交通工具制造生产企业，他们的产品涵盖了多种交通工具，并且有专门的工厂制造生产交通工具产品。

- 抽象产品：交通工具
- 具体产品：汽车、自行车
- 抽象工厂：企业
- 具体工厂：企业的工厂

**抽象产品：**

```java

// 抽象产品：交通工具
interface Transport {
    // 驾驶
    void drive();
}

```

**具体产品：**

```java

// 具体产品：汽车
class Car implements Transport {
    @Override
    public void drive() {
        System.out.println("开车");
    }
}

// 具体产品：自行车
class Bike implements Transport {
    @Override
    public void drive() {
        System.out.println("骑车");
    }
}

```


**抽象工厂：**

```java

// 抽象工厂：企业
interface TransportFactory {
    Transport create();
}

```


**具体工厂：**

```java

// 具体工厂：汽车工厂
class CarFactory implements TransportFactory {
    @Override
    public Transport create() {
        // 汽车工厂只生产汽车
        return new Car();
    }
}

// 具体工厂：自行车工厂
class BikeFactory implements TransportFactory {
    @Override
    public Transport create() {
        // 自行车工厂只生产自行车
        return new Bike();
    }
}

```



------



## 使用

通过企业找到工厂，然后通过工厂生产交通工具。


```java

public static void main(String[] args) {
    // 通过汽车工厂生产汽车
    TransportFactory carFactory = new CarFactory();
    Transport car = carFactory.create();
    car.drive();

    // 通过自行车工厂生产自行车
    TransportFactory bikeFactory = new BikeFactory();
    Transport bike = bikeFactory.create();
    bike.drive();
}

```
