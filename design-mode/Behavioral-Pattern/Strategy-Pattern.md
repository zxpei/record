### 一、策略模式的简介

##### 1.定义

策略模式属于对象的行为模式。其用意是针对一组算法，将每一个算法封装到具有共同接口的独立的类中，从而使得它们可以相互替换。策略模式使得算法可以在不影响到客户端的情况下发生变化。

##### 2.使用场景

针对一个对象，其行为有些是固定的不变的，有些是容易变化的，针对不同情况有不同的表现形式。那么对于这些容易变化的行为，我们不希望将其实现绑定在对象中，而是希望以动态的形式，针对不同情况产生不同的应对策略。那么这个时候就要用到策略模式了。简言之，策略模式就是为了应对对象中复杂多变的行为而产生的。

### 二、策略模式的结构

策略模式是对算法的包装，是把调用算法的责任（行为）和算法本身（行为实现）分割开来，委派给不同的对象管理。策略模式通常把一个系列的算法包装到一系列的策略类里面，作为一个抽象策略类的子类。用一句话来说，就是：“准备一组算法，并将每一个算法封装起来，使得它们可以互换”。下面就以一个示意性的实现讲解策略模式实例的结构。  


![](http://upload-images.jianshu.io/upload_images/3985563-b97fa59581b3c88c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


  
　　这个模式涉及到三个角色：

　　●　　环境\(Context\)角色：持有一个Strategy的引用，即具有复杂多变行为的对象。

　　●　　抽象策略\(Strategy\)角色：这是一个抽象角色，通常由一个接口或抽象类实现。此角色给出所有的具体策略类所需的接口。

　　●　　具体策略\(ConcreteStrategy\)角色：包装了相关的算法或行为。

### 三、具体场景实现

假设现在要设计一个贩卖各类书籍的电子商务网站的购物车系统。一个最简单的情况就是把所有货品的单价乘上数量，但是实际情况肯定比这要复杂。比如，本网站可能对所有的高级会员提供每本20%的促销折扣；对中级会员提供每本10%的促销折扣；对初级会员没有折扣。

　　根据描述，折扣是根据以下的几个算法中的一个进行的：

　　算法一：对初级会员没有折扣。

　　算法二：对中级会员提供10%的促销折扣。

　　算法三：对高级会员提供20%的促销折扣。

　　使用策略模式来实现的结构图如下：  


![](http://upload-images.jianshu.io/upload_images/3985563-3fcb880d2e7c5e2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


  
**抽象折扣类**

```java
public interface MemberStrategy {
    /**
     * 计算图书的价格
     * @param booksPrice    图书的原价
     * @return    计算出打折后的价格
     */
    public double calcPrice(double booksPrice);
}
```

**初级会员折扣类**

```java
public class PrimaryMemberStrategy implements MemberStrategy {

    @Override
    public double calcPrice(double booksPrice) {

        System.out.println("对于初级会员的没有折扣");
        return booksPrice;
    }

}
```

**中级会员折扣类**

```java
public class IntermediateMemberStrategy implements MemberStrategy {

    @Override
    public double calcPrice(double booksPrice) {

        System.out.println("对于中级会员的折扣为10%");
        return booksPrice * 0.9;
    }

}
```

**高级会员折扣类**

```java
public class AdvancedMemberStrategy implements MemberStrategy {

    @Override
    public double calcPrice(double booksPrice) {

        System.out.println("对于高级会员的折扣为20%");
        return booksPrice * 0.8;
    }
}
```

**价格类**

```java
public class Price {
    //持有一个具体的策略对象
    private MemberStrategy strategy;
    /**
     * 构造函数，传入一个具体的策略对象
     * @param strategy    具体的策略对象
     */
    public Price(MemberStrategy strategy){
        this.strategy = strategy;
    }

    /**
     * 计算图书的价格
     * @param booksPrice    图书的原价
     * @return    计算出打折后的价格
     */
    public double quote(double booksPrice){
        return this.strategy.calcPrice(booksPrice);
    }
}
```

具体调用：

```java
    public static void main(String[] args) {
        //选择并创建需要使用的策略对象
        MemberStrategy strategy = new AdvancedMemberStrategy();
        //创建环境
        Price price = new Price(strategy);
        //计算价格
        double quote = price.quote(300);
        System.out.println("图书的最终价格为：" + quote);
    }
```

### 四、对策略模式的深入认识

**策略模式对多态的使用**

  
　　通过让环境类持有一个抽象策略类（超类）的引用，在生成环境类实例对象时，让该引用指向具体的策略子类。再对应的方法调用中，就会通过Java的多态，调用对应策略子类的方法。从而可以相互替换，不需要修改环境类内部的实现。同时，在有新的需求的情况下，也只需要修改策略类即可，降低与环境类之间的耦合度。

**策略模式的重心**

　　策略模式的重心不是如何实现算法，而是如何组织、调用这些算法，从而让程序结构更灵活，具有更好的维护性和扩展性。

**算法的平等性**

　　策略模式一个很大的特点就是各个策略算法的平等性。对于一系列具体的策略算法，大家的地位是完全一样的，正因为这个平等性，才能实现算法之间可以相互替换。所有的策略算法在实现上也是相互独立的，相互之间是没有依赖的。

　　所以可以这样描述这一系列策略算法：策略算法是相同行为的不同实现。

**运行时策略的唯一性**

　　运行期间，策略模式在每一个时刻只能使用一个具体的策略实现对象，虽然可以动态地在不同的策略实现中切换，但是同时只能使用一个。

**公有的行为**

　　经常见到的是，所有的具体策略类都有一些公有的行为。这时候，就应当把这些公有的行为放到共同的抽象策略角色Strategy类里面。当然这时候抽象策略角色必须要用Java抽象类实现，而不能使用接口。

　　这其实也是典型的将代码向继承等级结构的上方集中的标准做法。  


![](http://upload-images.jianshu.io/upload_images/3985563-398483227b01f042.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


### 五、策略模式的优缺点

**策略模式的优点**  
　　（1）策略模式提供了管理相关的算法族的办法。策略类的等级结构定义了一个算法或行为族。恰当使用继承可以把公共的代码移到父类里面，从而避免代码重复。

　　（2）使用策略模式可以避免使用多重条件\(if-else\)语句。多重条件语句不易维护，它把采取哪一种算法或采取哪一种行为的逻辑与算法或行为的逻辑混合在一起，统统列在一个多重条件语句里面，比使用继承的办法还要原始和落后。

**策略模式的缺点**  
　　（1）客户端必须知道所有的策略类，并自行决定使用哪一个策略类。这就意味着客户端必须理解这些算法的区别，以便适时选择恰当的算法类。换言之，策略模式只适用于客户端知道算法或行为的情况。

　　（2）由于策略模式把每个具体的策略实现都单独封装成为类，如果备选的策略很多的话，那么对象的数目就会很可观。

