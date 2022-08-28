---
title: 设计模式入门 - 原型模式
layout: article
tags: Java 设计模式
---

按照现有的一个对象，创造出另外一个完全一样的对象。


**克隆！！！序列化！！！**


## 组成

- 原型类：必须实现深克隆/序列化

------

##  场景

录好演唱会视频光碟后，通过刻录可以创造出多个同样内容的光碟。

- 原型类：光碟

```java

// 原型类：光碟
class OpticalDisk implements Cloneable, Serializable {
    Video video;
    Audio audio;

    public OpticalDisk(Video video, Audio audio) {
        this.video = video;
        this.audio = audio;
    }

    // 浅克隆，内部属性引用会指向同一个对象
    @Override
    protected OpticalDisk clone() throws CloneNotSupportedException {
        return (OpticalDisk) super.clone();
    }

    // 序列化
    public OpticalDisk generate() {
        try {
            ByteArrayOutputStream bao = new ByteArrayOutputStream();
            ObjectOutputStream oos = new ObjectOutputStream(bao);
            oos.writeObject(this);

            ByteArrayInputStream bai = new ByteArrayInputStream(bao.toByteArray());
            ObjectInputStream ois = new ObjectInputStream(bai);
            return (OpticalDisk) ois.readObject();
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
            return null;
        }
    }

    @Override
    public String toString() {
        return "OpticalDisk{" +
                "video=" + video +
                ", audio=" + audio +
                '}';
    }

}

// 视频
class Video implements Serializable {
    String video = "video";

    @Override
    public String toString() {
        return "Video{" +
                "video='" + video + '\'' +
                '}';
    }
}

// 音频
class Audio implements Serializable {
    String audio = "audio";

    @Override
    public String toString() {
        return "Audio{" +
                "audio='" + audio + '\'' +
                '}';
    }
}

```


------

## 使用

使用浅克隆会存在原型对象内部属性引用同一个对象；
使用序列化需要原型类的d内部属性都实现序列化。

```java

public static void main(String[] args) throws CloneNotSupportedException {
    // 光碟原片1
    OpticalDisk diskOriginal = new OpticalDisk(new Video(), new Audio());
    // 光碟1克隆1
    OpticalDisk disk1 = diskOriginal.clone();
    // 光碟1克隆2
    OpticalDisk disk2 = diskOriginal.clone();
    // 浅克隆，内部属性引用同一个对象
    System.out.println(disk1.video.video == disk2.video.video);
    System.out.println(disk1.audio.audio == disk2.audio.audio);
    // 修改 disk1 的内容，disk2 被影响了
    disk1.video.video = "video modified by disk1";
    System.out.println("disk1 修改视频后，disk2 的视频为：" + disk2.video.video);

    // 光碟原片2
    OpticalDisk diskOriginal2 = new OpticalDisk(new Video(), new Audio());
    // 光碟2克隆1
    OpticalDisk disk3 = diskOriginal2.generate();
    // 光碟2克隆2
    OpticalDisk disk4 = diskOriginal2.generate();
    // 序列化，内部属性各自引用自己的对象
    System.out.println(disk3.video.video == disk4.video.video);
    System.out.println(disk3.audio.audio == disk4.audio.audio);
    // 虽然引用各自的对象，但是值是一样的
    System.out.println(disk3.video.video.equals(disk4.video.video));
    System.out.println(disk3.audio.audio.equals(disk4.audio.audio));
    // 修改 disk3，但是 disk4 不会被影响
    disk3.video.video = "video modified by disk3";
    System.out.println("disk3 修改视频后，disk4 的视频为：" + disk4.video.video);
}

```
