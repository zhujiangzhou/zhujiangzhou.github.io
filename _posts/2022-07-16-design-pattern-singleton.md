---
title: 设计模式入门 - 单例模式
layout: article
tags: Java 设计模式
---

在整个程序运行期间，某个类只允许存在一个对象（只实例化一次）。


## 组成

- 单例类：构造方法必须私有

------

##  场景

奥运会男子乒乓球单打项目只能有一个冠军。

- 单例类：冠军


**懒汉单例：**

```java

// 懒加载
class LazyChampion {
    // 构造方法必须私有
    private LazyChampion() {
    }

    private volatile static LazyChampion champion;

    public static LazyChampion getInstance() {
        if (champion == null) {
            synchronized (LazyChampion.class) {
                if (champion == null) {
                    champion = new LazyChampion();
                }
            }
        }
        return champion;
    }

}

```


**饿汉单例：**

```java

class HungryChampion {
    // 构造方法私有
    private HungryChampion() {}

    private static final HungryChampion champion = new HungryChampion();

    public static HungryChampion getInstance() {
        return champion;
    }

}

```


**静态内部类：**

```java

class StaticInnerClassChampion {

    // 构造方法私有
    private StaticInnerClassChampion() {
    }

    // 静态内部类
    public static class ChampionGetter {
        private static final StaticInnerClassChampion champion = new StaticInnerClassChampion();

        public static StaticInnerClassChampion getInstance() {
            return champion;
        }
    }

    public static StaticInnerClassChampion getInstance() {
        return ChampionGetter.getInstance();
    }
}

```


------

## 使用

```java

// 使用多线程模式来验证可能会更好
public static void main(String[] args) {
    // 懒汉模式
    LazyChampion lazyChampion1 = LazyChampion.getInstance();
    LazyChampion lazyChampion2 = LazyChampion.getInstance();
    System.out.println(lazyChampion1 == lazyChampion2);
    // 饿汉模式
    HungryChampion hungryChampion1 = HungryChampion.getInstance();
    HungryChampion hungryChampion2 = HungryChampion.getInstance();
    System.out.println(hungryChampion1 == hungryChampion2);
    // 静态内部类
    StaticInnerClassChampion instance1 = StaticInnerClassChampion.getInstance();
    StaticInnerClassChampion instance2 = StaticInnerClassChampion.getInstance();
    System.out.println(instance1 == instance2);
    // 枚举...
}

```
