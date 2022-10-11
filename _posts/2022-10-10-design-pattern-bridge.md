---
title: 设计模式入门 - 桥接模式
layout: article
tags: Java 设计模式
---

桥接模式将抽象化和实现分离，使得他们可以有不同的实现以及不同的组合变化。

## 组成

- 抽象类
- 实现类


------



## 场景

某游戏开发公司游戏主机可以玩多款游戏

- 抽象类：Nintendo Switch、GBA
- 实现类：塞尔达传说、超级马里奥


```java

// 抽象类
public abstract class GameConsole {

    // 有用不同的实现内容
    GameCard gameCard;

    public void loadCard(GameCard card) {
        gameCard = card;
    }

    // 加载游戏并游玩
    public void play() {
        gameCard.play();
    }

}

// 抽象类具体实现
public class Switch extends GameConsole {

    @Override
    public void play() {
        gameCard.play();
    }
}

// 抽象类具体实现
public class GBA extends GameConsole {

    @Override
    public void play() {
        gameCard.play();
    }

}

// 实现类接口：游戏卡带
public interface GameCard {
    void play();
}

// 实现类具体实现：超级马里奥卡带
public class MarioGameCard implements GameCard {
    @Override
    public void play() {
        System.out.println("超级马里奥");
    }
}

// 实现类具体实现：塞尔达卡带
public class ZeldaGameCard implements GameCard {
    @Override
    public void play() {
        System.out.println("塞尔达传说");
    }
}

```





## 使用

游戏机通过不同的卡带可以玩不同的游戏：

```java

public static void main(String[] args) {
    GameConsole ns = new Switch();

    GameCard zelda = new ZeldaGameCard();
    ns.loadCard(zelda);
    ns.play();

    GameCard mario = new MarioGameCard();
    ns.loadCard(mario);
    ns.play();

    GameConsole gba = new GBA();
    gba.loadCard(mario);
    gba.play();

}

```
