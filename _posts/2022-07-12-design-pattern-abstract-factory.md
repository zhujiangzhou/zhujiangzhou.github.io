---
title: 设计模式入门 - 抽象工厂模式
layout: article
tags: Java 设计模式
---

抽象工厂模式由多个具体工厂且可以生产不同系列的产品。

## 组成

- 抽象产品：某一类产品的抽象
- 具体产品：抽象产品的具体实现
- 抽象工厂：所有工厂的父类（接口），在抽象工厂模式中它可以生产不同类型的产品
- 具体工厂：抽象工厂的具体实现



------



## 场景

有大企业，他们各自生产自家的电子产品。

- 抽象产品：手机、相机
- 具体产品：索尼手机、索尼相机、索基亚手机
- 抽象工厂：跨国公司
- 具体工厂：索尼、诺基亚


**抽象产品：**

```java

// 抽象产品：手机
interface MobilePhone {
    void call();
}

// 抽象产品：相机
interface Camera {
    void takePictures();
}

```

**具体产品：**

```java

// 具体产品：索尼手机
class SonyPhone implements MobilePhone {
    @Override
    public void call() {
        System.out.println("使用索尼手机打电话");
    }
}
// 具体产品：诺基亚手机
class NokiaPhone implements MobilePhone {
    @Override
    public void call() {
        System.out.println("使用诺基亚手机打电话");
    }
}

// 具体产品：索尼相机
class SonyCamera implements Camera {
    @Override
    public void takePictures() {
        System.out.println("使用索尼相机拍照");
    }
}

```

**抽象工厂：**

```java

// 抽象工厂：跨国公司
interface InternalCompany {
    // 生产手机
    MobilePhone createPhone();
    // 生产相机
    Camera createCamera();
}

```

**具体工厂：**

```java

// 具体工厂：索尼公司
class SonyCompany implements InternalCompany {
    @Override
    public MobilePhone createPhone() {
        return new SonyPhone();
    }

    @Override
    public Camera createCamera() {
        return new SonyCamera();
    }
}

// 具体工厂：诺基亚公司
class NokiaCompany implements InternalCompany {
    @Override
    public MobilePhone createPhone() {
        return new NokiaPhone();
    }

    @Override
    public Camera createCamera() {
        throw new RuntimeException("诺基亚不生产相机，你可以用诺基亚手机拍照");
    }
}

```

**简单工厂：**

使用简单工厂来获取具体工厂。

```java

class CompanyFactory {
    public static InternalCompany getCompany(String name) {
        switch (name) {
            case "Sony":
                return new SonyCompany();
            case "Nokia":
                return new NokiaCompany();
            default:
                throw new RuntimeException("不存在的公司");
        }
    }
}

```

## 使用

通过简单工厂来获取具体工厂，然后通过具体工厂生产产品。

```java

public static void main(String[] args) {
    // 索尼公司
    InternalCompany sony = CompanyFactory.getCompany("Sony");
    // 生产索尼手机
    MobilePhone sonyPhone = sony.createPhone();
    sonyPhone.call();
    // 生产索尼相机
    Camera sonyCamera = sony.createCamera();
    sonyCamera.takePictures();

    // 诺基亚公司
    InternalCompany nokia = CompanyFactory.getCompany("Nokia");
    // 生产诺基亚手机
    MobilePhone nokiaPhone = nokia.createPhone();
    nokiaPhone.call();
    // 生产诺基亚相机将会报错，因为诺基亚没有相机
    Camera nokiaCamera = nokia.createCamera();
}

```
