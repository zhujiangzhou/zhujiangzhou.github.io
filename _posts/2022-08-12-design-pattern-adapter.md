---
title: 设计模式入门 - 适配器模式
layout: article
tags: Java 设计模式
---

让原先不兼容的两个接口相互兼容。



## 组成

- 目标类：需要兼容的类
- 适配器类：提供兼容的类

------

##  场景

TF 卡无法直接在电脑上使用，通过读卡器就可以在电脑上使用。

- 目标类：TF 卡
- 适配器类：读卡器


```java

// 适器接口
interface StroageMedium {
    byte[] readData();
}

// 目标类
class TFCard {
    private byte[] data;

    public TFCard(byte[] data) {
        this.data = data;
    }

    public byte[] getData() {
        return data;
    }
}

// 适配器类
class TFCardReader implements StroageMedium {

    private TFCard tfCard;

    public TFCardReader(TFCard tfCard) {
        this.tfCard = tfCard;
    }

    @Override
    public byte[] readData() {
        return tfCard.getData();
    }
}

// 电脑
class Computer {
    public static void readData(StroageMedium data) {
        String content = new String(data.readData());
        System.out.println("读取的数据：" + content);
    }
}

```


------

## 使用

TF 卡套上读卡器让电脑读取数据。


```java

public static void main(String[] args) {
    // TF 卡
    TFCard tfCard = new TFCard("hello, world".getBytes());
    // TF 卡装读卡器
    TFCardReader tfCardReader = new TFCardReader(tfCard);
    // 电脑读取 TF 卡内容
    Computer.readData(tfCardReader);
}

```
