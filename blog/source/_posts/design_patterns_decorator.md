---
title: 装饰者模式
toc: true
tags:
- 装饰者
categories:
- 设计模式
date: 2020/03/19 22:29:00
updated: 2020/03/19 22:29:00
---

&emsp;&emsp;在学习Netty的时候，对io流，及一些常用的设计模式进行了补习。在此记录一下**装饰者模式**。

<!--more-->

### 什么是装饰者模式

装饰者模式，以类似“装饰”的方式，动态地给对象添加功能。装饰者模式要求装饰对象和被装饰对象同时实现同一个接口，装饰对象持有被装饰对象实例。

### 使用场景

给一个对象**添加新功能**，要求可以**动态**添加**动态**撤销。
相较于继承，不需要新建很多子类，而且可以按功能划分不同的装饰类，动态组合，而且对于多种不同的功能要求可以自由组合地情况，根据不同的情况创建不同的子类变得不切实际，这个时候就需要装饰者模式。

### 例子

接口：
```java
public interface KfcPlatedMeals {
    public String getName(); 
    public float getPrice();
}
```
被装饰类
```java
public class PlatedMeals implements KfcPlatedMeals {
    private String name;
    public PlatedMeals(String name) {
        this.name = name;
    }
    public String getName() {
        return this.name + ":";
    }
    public float getPrice() {
        return 0;
    }
}
```
装饰类
```java
public class FrenchFries implements KfcPlatedMeals {
    private KfcPlatedMeals platedMeals;
    public FrenchFries(KfcPlatedMeals platedMeals) {
        this.platedMeals = platedMeals;
    }
    public String getName() {
       return platedMeals.getName() + " 薯条";
    }
    public float getPrice() {
       return platedMeals.getPrice() + 11f;
    }
}
```
```java
public class Cola implements KfcPlatedMeals {
    private KfcPlatedMeals platedMeals;
    public Cola(KfcPlatedMeals platedMeals) {
        this.platedMeals = platedMeals;
    }
    public String getName() {
        return platedMeals.getName() + " 可乐";
    }
    public float getPrice() {
        return platedMeals.getPrice() + 2.5f;
    }
}
```
测试
```java
public class KfcPlatedMealsTest extends TestCase {
    public void testPlatedMeals() {
        KfcPlatedMeals platedMealsA = new Cola(new FrenchFries(new PlatedMeals("套餐A")));
        System.out.println(platedMealsA.getName() + " /价格：" + platedMealsA.getPrice());
    }
}
```
