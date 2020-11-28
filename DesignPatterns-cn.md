# 设计模式

[English Version](./DesignPatterns.md)



## 前言

------

### 概述

------

设计模式在面试中的考点通常是介绍其原理并说出优缺点。或者对比几个比较相似的模式的异同点。在笔试中可能会出现画出某个设计模式的 UML 图这样的题。虽说面试中占的比重不大，但并不代表它不重要。恰恰相反，设计模式于程序员而言相当重要，它是我们写出优秀程序的保障。设计模式与程序员的架构能力与阅读源码的能力息息相关，非常值得我们深入学习。

面向对象的特点是**可维护、可复用、可扩展、灵活性好**，它最强大的地方在于：随着业务变得越来越复杂，面向对象依然能够使得程序结构良好，而面向过程却会导致程序越来越臃肿。

让面向对象保持结构良好的秘诀就是设计模式，面向对象结合设计模式，才能真正体会到程序变得可维护、可复用、可扩展、灵活性好。设计模式对于程序员而言并不陌生，每个程序员在编程时都会或多或少的接触到设计模式。无论是在大型程序的架构中，亦或是在源码的学习中，设计模式都扮演着非常重要的角色。

### 设计模式的六大原则

------

设计模式的世界丰富多彩，比如生产一个个“产品”的工厂模式，衔接两个不相关接口的适配器模式，用不同的方式做同一件事的策略模式，构建步骤稳定、根据构建过程的不同配置构建出不同对象的建造者模式等等。

无论何种设计模式，都是基于六大设计原则：

* 开闭原则：一个软件实体如类、模块和函数应该对修改封闭，对扩展开放。
* 单一职责原则：一个类只做一件事，一个类应该只有一个引起它修改的原因。】
* 里氏替换原则：子类应该可以完全替换父类。也就是说在使用继承时，只扩展新功能，而不要破坏父类原有的功能。
* 依赖倒置原则：细节应该依赖于抽象，抽象不应依赖于细节。把抽象层放在程序设计的高层，并保持稳定，程序的细节变化由低层的实现层来完成。
* 迪米特法则：又名“最少知道原则”，一个类不应知道自己操作的类的细节，换言之，只和朋友谈话，不和朋友的朋友谈话。
* 接口隔离原则：客户端不应依赖它不需要的接口。如果一个接口在实现时，部分方法由于冗余被客户端空实现，则应该将接口拆分，让实现类只需依赖自己需要的接口方法。



## 构建者模式

------

### 工厂模式

在平时编程中，构建对象最常用的方式是 new 一个对象。乍一看这种做法没什么不好，而实际上这也属于一种硬编码。每 new 一个对象，相当于调用者多知道了一个类，增加了类与类之间的联系，不利于程序的松耦合。其实构建过程可以被封装起来，工厂模式便是用于封装对象的设计模式。

#### 简单工厂模式

举个例子，直接 new 对象的方式相当于当我们需要一个苹果时，我们需要知道苹果的构造方法，需要一个梨子时，需要知道梨子的构造方法。更好的实现方式是有一个水果工厂，我们告诉工厂需要什么种类的水果，水果工厂将我们需要的水果制造出来给我们就可以了。这样我们就无需知道苹果、梨子是怎么种出来的，只用和水果工厂打交道即可。

水果工厂：

```Java
public class FruitFactory {
    public Fruit create(String type){
        switch (type){
            case "苹果": return new Apple();
            case "梨子": return new Pear();
            default: throw new IllegalArgumentException("暂时没有这种水果");
        }
    }
}
```

调用者：

```Java
public class User {
    private void eat(){
        FruitFactory fruitFactory = new FruitFactory();
        Fruit apple = fruitFactory.create("苹果");
        Fruit pear = fruitFactory.create("梨子");
        apple.eat();
        pear.eat();
    }
}
```

事实上，将构建过程封装的好处不仅可以降低耦合，如果某个产品构造方法相当复杂，使用工厂模式可以大大减少代码重复。比如，如果生产一个苹果需要苹果种子、阳光、水分，将工厂修改如下：

```Java
public class FruitFactory {
    public Fruit create(String type) {
        switch (type) {
            case "苹果":
                AppleSeed appleSeed = new AppleSeed();
                Sunlight sunlight = new Sunlight();
                Water water = new Water();
                return new Apple(appleSeed, sunlight, water);
            case "梨子":
                return new Pear();
            default:
                throw new IllegalArgumentException("暂时没有这种水果");
        }
    }
}
```

调用者的代码则完全不需要变化，而且调用者不需要在每次需要苹果时，自己去构建苹果种子、阳光、水分以获得苹果。苹果的生产过程再复杂，也只是工厂的事。这就是封装的好处，假如某天科学家发明了让苹果更香甜的肥料，要加入苹果的生产过程中的话，也只需要在工厂中修改，调用者完全不用关心。

不知不觉中，我们就写出了简单工厂模式的代码。工厂模式一共有三种：

* 简单工厂模式
* 工厂方法模式
* 抽象工厂模式

> 注：在 GoF 所著的《设计模式》一书中，简单工厂模式被划分为工厂方法模式的一种特例，没有单独被列出来。

总而言之，简单工厂模式就是让一个工厂类承担构建所有对象的职责。调用者需要什么产品，让工厂生产出来即可。它的弊端也显而易见：

* 一是如果需要生产的产品过多，此模式会导致工厂类过于庞大，承担过多的职责，变成超级类。当苹果生产过程需要修改时，要来修改此工厂。梨子生产过程需要修改时，也要来修改此工厂。也就是说这个类不止一个引起修改的原因。违背了单一职责原则。

* 二是当要生产新的产品时，必须在工厂类中添加新的分支。而开闭原则告诉我们：类应该对修改封闭。我们希望在添加新功能时，只需增加新的类，而不是修改既有的类，所以这就违背了开闭原则。

#### 工厂方法模式

为了解决简单工厂模式的这两个弊端，工厂方法模式应运而生，它规定每个产品都有一个专属工厂。比如苹果有专属的苹果工厂，梨子有专属的梨子工厂，代码如下：

苹果工厂：

```Java
public class AppleFactory {
    public Fruit create(){
        return new Apple();
    }
}
```

梨子工厂：

```Java
public class PearFactory {
    public Fruit create(){
        return new Pear();
    }
}
```

调用者：

```Java
public class User {
    private void eat(){
        AppleFactory appleFactory = new AppleFactory();
        Fruit apple = appleFactory.create();
        PearFactory pearFactory = new PearFactory();
        Fruit pear = pearFactory.create();
        apple.eat();
        pear.eat();
    }
}
```

有读者可能会开喷了，这样和直接 new 出苹果和梨子有什么区别？上文说工厂是为了减少类与类之间的耦合，让调用者尽可能少的和其他类打交道。用简单工厂模式，我们只需要知道 `FruitFactory`，无需知道 `Apple` 、`Pear` 类，很容易看出耦合度降低了。但用工厂方法模式，调用者虽然不需要和 `Apple` 、`Pear` 类打交道了，但却需要和 `AppleFactory`、`PearFactory` 类打交道。有几种水果就需要知道几个工厂类，耦合度完全没有下降啊，甚至还增加了代码量！

这位读者请先放下手中的大刀，仔细想一想，工厂模式的第二个优点在工厂方法模式中还是存在的。当构建过程相当复杂时，工厂将构建过程封装起来，调用者可以很方便的直接使用，同样以苹果生产为例：

```Java
public class AppleFactory {
    public Fruit create(){
        AppleSeed appleSeed = new AppleSeed();
        Sunlight sunlight = new Sunlight();
        Water water = new Water();
        return new Apple(appleSeed, sunlight, water);
    }
}
```

调用者无需知道苹果的生产细节，当生产过程需要修改时也无需更改调用端。同时，工厂方法模式解决了简单工厂模式的两个弊端。

* 当生产的产品种类越来越多时，工厂类不会变成超级类。工厂类会越来越多，保持灵活。不会越来越大、变得臃肿。如果苹果的生产过程需要修改时，只需修改苹果工厂。梨子的生产过程需要修改时，只需修改梨子工厂。符合单一职责原则。

* 当需要生产新的产品时，无需更改既有的工厂，只需要添加新的工厂即可。保持了面向对象的可扩展性，符合开闭原则。
  OK，学以致用，接下来我们来做两个思考题。同样地，在以后的每一篇文章后面，都会附上几个小练习供大家思考。希望大家能够独立思考出问题的答案，当然，在必要时也可参考底部的解析。

问 1：现有医用口罩和 N95 口罩两种产品，都继承自 Mask 类：

```Java
abstract class Mask {
}

public class SurgicalMask extends Mask {
    @NonNull
    @Override
    public String toString() {
        return "这是医用口罩";
    }
}

public class N95Mask extends Mask {
    @NonNull
    @Override
    public String toString() {
        return "这是 N95 口罩";
    }
}
```



