---
title: 设计模式入门 - 建造者模式
layout: article
tags: Java 设计模式
---

将一个复杂的对象分为不同的构建过程，通过不同的过程的不同实现方式来构建对象。


## 组成

- 抽象建造者：建造流程规范
- 具体建造者：建造流程规范的执行者
- 指挥类：通过调用具体建造者的不同流程来构建产品
- 产品类：建造者建造的产品



------



## 场景

建筑公司有一套建筑建造流程，总工程师负责指挥不同的施工组建造不同的部分，
最终完成房屋建造。

- 产品类：房屋
- 抽象建造者：建造流程
- 具体建造者：不同的施工组
- 指挥类：总工程师


**产品类：**

```java

// 产品类：房租
class House {
    // 房屋风格
    String style;
    // 地基
    String foundation;
    // 墙体
    String wall;
    // 屋顶
    String roof;
}

```


**抽象建造者：**

```java

// 抽象建造者：房租建造流程
interface HouseBuildProcedure {

    // 打地基
    void layFoundation();

    // 砌墙
    void bricklaying();

    // 封顶
    void roofed();

}

```

**具体建造者：**

```java

// 具体建造者：中国建造团队
class ChinaHouseBuilder implements HouseBuildProcedure {

    House house = new House();
    {
        house.style ="中式";
    }

    @Override
    public void layFoundation() {
        house.foundation = "人工地基";
    }

    @Override
    public void bricklaying() {
        house.wall = "雕梁画柱";
    }

    @Override
    public void roofed() {
        house.roof = "斗拱";
    }

    @Override
    public House build() {
        return house;
    }
}

// 具体建造者：西方建造团队
class EuropHouseBuilder implements HouseBuildProcedure {

    House house = new House();
    {
        house.style ="欧式";
    }

    @Override
    public void layFoundation() {
        house.foundation = "人工地基";
    }

    @Override
    public void bricklaying() {
        house.wall = "石柱石墙";
    }

    @Override
    public void roofed() {
        house.roof = "穹顶";
    }

    @Override
    public House build() {
        return house;
    }
}

```


**指挥者：**


```java

// 指挥者：总工程师
class chefEngineer {
    private HouseBuildProcedure procedure;

    public chefEngineer(HouseBuildProcedure procedure) {
        this.procedure = procedure;
    }

    public House build() {
        // 地基
        procedure.layFoundation();
        // 垒墙
        procedure.bricklaying();
        // 封顶
        procedure.roofed();
        // 完工
        return procedure.build();
    }
}

```


## 使用

总工程师指挥中国团队和欧洲团队建造不同风格的房子。

```java

public static void main(String[] args) {
    // 总工程师
    ChiefEngineer chiefEngineer = new ChiefEngineer();

    // 中国风格
    HouseBuildProcedure chinaTeam = new ChinaHouseBuilder();
    chiefEngineer.setProcedure(chinaTeam);
    House chinaStyleHouse = chiefEngineer.build();
    System.out.println(chinaStyleHouse);

    // 欧洲风格
    HouseBuildProcedure europeTeam = new EuropHouseBuilder();
    chiefEngineer.setProcedure(europeTeam);
    House europeStyleHouse = chiefEngineer.build();
    System.out.println(europeStyleHouse);
}

```



