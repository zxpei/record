### 一、抽象工厂模式简介

##### 1.定义

抽象工厂模式\(Abstract Factory Pattern\)：提供一个创建一系列相关或相互依赖对象的接口，而无须指定它们具体的类。抽象工厂模式又称为Kit模式，属于对象创建型模式。

定义很难懂？没错，看起来是很抽象，不过这正反应了这种模式的强大。下面具体阐述下定义。

##### 2.定义阐述

在工厂方法模式中具体工厂负责生产具体的产品，每一个具体工厂对应一种或几种具体产品，工厂方法也具有唯一性，一般情况下，一个具体工厂中只有一个工厂方法或者一组重载的工厂方法。但是有时候我们需要一个工厂可以提供多个**不同种类**产品对象，而不是**单一种类**的产品对象。

为了更清晰地理解工厂方法模式，需要先引入两个概念：

**产品等级结构：** 产品等级结构即产品的继承结构，如一个抽象类是电视机，其子类有海尔电视机、海信电视机、TCL电视机，则抽象电视机与具体品牌的电视机之间构成了一个产品等级结构，抽象电视机是父类，而具体品牌的电视机是其子类。

**产品族：** 在抽象工厂模式中，**产品族是指由同一个工厂生产的，位于不同产品等级结构中的一组产品**，如海尔电器工厂生产的海尔电视机、海尔电冰箱，海尔电视机位于电视机产品等级结构中，海尔电冰箱位于电冰箱产品等级结构中。

**当系统所提供的工厂所需生产的具体产品并不是一个简单的对象，而是多个位于不同产品等级结构中属于不同类型的具体产品时需要使用抽象工厂模式。**

抽象工厂模式是所有形式的工厂模式中**最为抽象和最具一般性的一种形态**。

抽象工厂模式与工厂方法模式最大的区别在于，**工厂方法模式针对的是一个产品等级结构**，**而抽象工厂模式则需要面对多个产品等级结构**，一个工厂等级结构可以负责多个不同产品等级结构中的产品对象的创建 。当一个工厂等级结构可以创建出分属于不同产品等级结构的一个产品族中的所有对象时，抽象工厂模式比工厂方法模式更为简单、有效率。

### 二、抽象工厂模式结构

##### 1.模式结构

![](http://upload-images.jianshu.io/upload_images/3985563-837d93237cf1143b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


  
抽象工厂模式包含如下角色：

AbstractFactory：抽象工厂  
ConcreteFactory：具体工厂  
AbstractProduct：抽象产品  
Product：具体产品

##### 2.时序图

![](http://upload-images.jianshu.io/upload_images/3985563-912f3e3f444a691b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


  
①先调用具体工厂对象中的方法createProductX\(\)。根据具体工厂不同可以选择不同的方法，针对同一种工厂也可以选择不同的方法创建不同类型的产品对象。

②根据传入产品类型参数（也可以无参），获得具体的产品对象

③返回产品对象并使用

### 三、抽象工厂的使用实例

假设有一个移动终端工厂，可以制造苹果系列的移动产品和三星系列的移动产品。这个工厂下有两个子厂，一个负责制造苹果系列的Pad和三星系列的Pad，另一个负责制造苹果系列的手机和三星系列的手机。这便是一个典型的抽象工厂的实例。  
**抽象产品： 苹果系列**

```java
public interface Apple
{
    void AppleStyle();
}
```

**抽象产品： 三星系列**

```java
public interface Sumsung
{
    void BangziStyle();
}
```

**具体产品：iphone**

```java
public class iphone implements Apple
{
    public void AppleStyle()
    {
        Console.WriteLine("Apple's style: iPhone!");
    }
}
```

**具体产品：ipad**

```java
public class ipad implements Apple
{
    public void AppleStyle()
    {
        Console.WriteLine("Apple's style: iPad!");
    }
}
```

**具体产品：note2**

```java
public class note2 implements Sumsung
{
    public void BangziStyle()
    {
        Console.WriteLine("Bangzi's style : Note2!");
    }
}
```

**具体产品：tabs**

```java
public class Tabs implements Sumsung
{
    public void BangziStyle()
    {
        Console.WriteLine("Bangzi's style : Tab!");
    }
}
```

**抽象工厂**

```java
public interface Factory
{
    Apple createAppleProduct();
    Sumsung createSumsungProduct();
}
```

**手机工厂**

```java
public class Factory_Phone implements Factory
{
    public Apple createAppleProduct()
    {
        return new iphone();
    }

    public Sumsung createSumsungProduct()
    {
        return new note2();
    }
}
```

**pad工厂**

```java
public class Factory_Pad implements  Factory
{
    public Apple createAppleProduct()
    {
        return new ipad();
    }

    public Sumsung createSumsungProduct()
    {
        return new Tabs();
    }
}
```

**客户端调用**

```java
public static void Main(string[] args)
{
    //采购商要一台iPad和一台Tab
    Factory factory = new Factory_Pad();
    Apple apple = factory.createAppleProduct();
    apple.AppleStyle();
    Sumsung sumsung = factory.createSumsungProduct();
    sumsung.BangziStyle();

    //采购商又要一台iPhone和一台Note2
    factory = new Factory_Phone();
    apple = factory.createAppleProduct();
    apple.AppleStyle();
    sumsung = factory.createSumsungProduct();
    sumsung.BangziStyle();
    Console.ReadKey();
}
```

抽象工厂可以通过多态，来动态设置不同的工厂，生产不同的产品，同时每个工厂中的产品又不属于同一个产品等级结构。

### 四、抽象工厂模式优缺点

##### 优点

①抽象工厂模式隔离了具体类的生成，使得客户并不需要知道什么被创建。由于这种隔离，更换一个具体工厂就变得相对容易。所有的具体工厂都实现了抽象工厂中定义的那些公共接口，因此只需改变具体工厂的实例，就可以在某种程度上改变整个软件系统的行为。另外，应用抽象工厂模式可以实现高内聚低耦合的设计目的，因此抽象工厂模式得到了广泛的应用。

②增加新的具体工厂和产品族很方便，因为一个具体的工厂实现代表的是一个产品族，无须修改已有系统，符合“开闭原则”。

##### 缺点

在添加新的产品对象（不同于现有的产品等级结构）时，难以扩展抽象工厂来生产新种类的产品，这是因为在抽象工厂角色中规定了所有可能被创建的产品集合，要支持新种类的产品就意味着要对该接口进行扩展，而这将涉及到对抽象工厂角色及其所有子类的修改，显然会带来较大的不便。

开闭原则的倾斜性（增加新的工厂和产品族容易，增加新的产品等级结构麻烦）。

##### 适用环境

在以下情况下可以使用抽象工厂模式：

①一个系统不应当依赖于产品类实例如何被创建、组合和表达的细节，这对于所有类型的工厂模式都是重要的。

②系统中有多于一个的产品族，而每次只使用其中某一产品族。**与工厂方法模式的区别**

③属于同一个产品族的产品将在一起使用，这一约束必须在系统的设计中体现出来。

④系统提供一个产品类的库，所有的产品以同样的接口出现，从而使客户端不依赖于具体实现。

