# 設計模式

[English Version](./DesignPatterns.md)



## 前言

------

### 概述

------

設計模式在面試中的考點通常是介紹其原理並說出優缺點。或者對比幾個比較相似的模式的異同點。在筆試中可能會出現畫出某個設計模式的 UML 圖這樣的題。雖說面試中佔的比重不大，但並不代表它不重要。恰恰相反，設計模式於程序員而言相當重要，它是我們寫出優秀程序的保障。設計模式與程序員的架構能力與閱讀源碼的能力息息相關，非常值得我們深入學習。

面向對象的特點是**可維護、可複用、可擴展、靈活性好**，它最強大的地方在於：隨著業務變得越來越複雜，面向對象依然能夠使得程序結構良好，而面向過程卻會導致程序越來越臃腫。

讓面向對象保持結構良好的秘訣就是設計模式，面向對象結合設計模式，才能真正體會到程序變得可維護、可複用、可擴展、靈活性好。設計模式對於程序員而言並不陌生，每個程序員在編程時都會或多或少的接觸到設計模式。無論是在大型程序的架構中，亦或是在源碼的學習中，設計模式都扮演著非常重要的角色。

### 設計模式的六大原則

------

設計模式的世界豐富多彩，比如生產一個個“產品”的工廠模式，銜接兩個不相關接口的適配器模式，用不同的方式做同一件事的策略模式，構建步驟穩定、根據構建過程的不同配置構建出不同對象的建造者模式等等。

無論何種設計模式，都是基於六大設計原則：

* 開閉原則：一個軟件實體如類、模塊和函數應該對修改封閉，對擴展開放。
* 單一職責原則：一個類只做一件事，一個類應該只有一個引起它修改的原因。】
* 里氏替換原則：子類應該可以完全替換父類。也就是說在使用繼承時，只擴展新功能，而不要破壞父類原有的功能。
* 依賴倒置原則：細節應該依賴於抽象，抽象不應依賴於細節。把抽象層放在程序設計的高層，並保持穩定，程序的細節變化由低層的實現層來完成。
* 迪米特法則：又名“最少知道原則”，一個類不應知道自己操作的類的細節，換言之，只和朋友談話，不和朋友的朋友談話。
* 接口隔離原則：客戶端不應依賴它不需要的接口。如果一個接口在實現時，部分方法由於冗餘被客戶端空實現，則應該將接口拆分，讓實現類只需依賴自己需要的接口方法。



## 構建者模式

------

### 工廠模式

在平時編程中，構建對象最常用的方式是 new 一個對象。乍一看這種做法沒什麼不好，而實際上這也屬於一種硬編碼。每 new 一個對象，相當於調用者多知道了一個類，增加了類與類之間的聯繫，不利於程序的松耦合。其實構建過程可以被封裝起來，工廠模式便是用於封裝對象的設計模式。

#### 簡單工廠模式

舉個例子，直接 new 對象的方式相當於當我們需要一個蘋果時，我們需要知道蘋果的構造方法，需要一個梨子時，需要知道梨子的構造方法。更好的實現方式是有一個水果工廠，我們告訴工廠需要什麼種類的水果，水果工廠將我們需要的水果製造出來給我們就可以了。這樣我們就無需知道蘋果、梨子是怎麼種出來的，只用和水果工廠打交道即可。

水果工廠：

```Java
public class FruitFactory {
    public Fruit create(String type){
        switch (type){
            case "蘋果": return new Apple();
            case "梨子": return new Pear();
            default: throw new IllegalArgumentException("暫時沒有這種水果");
        }
    }
}
```

調用者：

```Java
public class User {
    private void eat(){
        FruitFactory fruitFactory = new FruitFactory();
        Fruit apple = fruitFactory.create("蘋果");
        Fruit pear = fruitFactory.create("梨子");
        apple.eat();
        pear.eat();
    }
}
```

事實上，將構建過程封裝的好處不僅可以降低耦合，如果某個產品構造方法相當複雜，使用工廠模式可以大大減少代碼重複。比如，如果生產一個蘋果需要蘋果種子、陽光、水分，將工廠修改如下：

```Java
public class FruitFactory {
    public Fruit create(String type) {
        switch (type) {
            case "蘋果":
                AppleSeed appleSeed = new AppleSeed();
                Sunlight sunlight = new Sunlight();
                Water water = new Water();
                return new Apple(appleSeed, sunlight, water);
            case "梨子":
                return new Pear();
            default:
                throw new IllegalArgumentException("暫時沒有這種水果");
        }
    }
}
```

調用者的代碼則完全不需要變化，而且調用者不需要在每次需要蘋果時，自己去構建蘋果種子、陽光、水分以獲得蘋果。蘋果的生產過程再複雜，也只是工廠的事。這就是封裝的好處，假如某天科學家發明了讓蘋果更香甜的肥料，要加入蘋果的生產過程中的話，也只需要在工廠中修改，調用者完全不用關心。

不知不覺中，我們就寫出了簡單工廠模式的代碼。工廠模式一共有三種：

* 簡單工廠模式
* 工廠方法模式
* 抽象工廠模式

> 注：在 GoF 所著的《設計模式》一書中，簡單工廠模式被劃分為工廠方法模式的一種特例，沒有單獨被列出來。

總而言之，簡單工廠模式就是讓一個工廠類承擔構建所有對象的職責。調用者需要什麼產品，讓工廠生產出來即可。它的弊端也顯而易見：

* 一是如果需要生產的產品過多，此模式會導致工廠類過於龐大，承擔過多的職責，變成超級類。當蘋果生產過程需要修改時，要來修改此工廠。梨子生產過程需要修改時，也要來修改此工廠。也就是說這個類不止一個引起修改的原因。違背了單一職責原則。

* 二是當要生產新的產品時，必須在工廠類中添加新的分支。而開閉原則告訴我們：類應該對修改封閉。我們希望在添加新功能時，只需增加新的類，而不是修改既有的類，所以這就違背了開閉原則。

#### 工廠方法模式

為了解決簡單工廠模式的這兩個弊端，工廠方法模式應運而生，它規定每個產品都有一個專屬工廠。比如蘋果有專屬的蘋果工廠，梨子有專屬的梨子工廠，代碼如下：

蘋果工廠：

```Java
public class AppleFactory {
    public Fruit create(){
        return new Apple();
    }
}
```

梨子工廠：

```Java
public class PearFactory {
    public Fruit create(){
        return new Pear();
    }
}
```

調用者：

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

有讀者可能會開噴了，這樣和直接 new 出蘋果和梨子有什麼區別？上文說工廠是為了減少類與類之間的耦合，讓調用者儘可能少的和其他類打交道。用簡單工廠模式，我們只需要知道 `FruitFactory`，無需知道 `Apple` 、`Pear` 類，很容易看出耦合度降低了。但用工廠方法模式，調用者雖然不需要和 `Apple` 、`Pear` 類打交道了，但卻需要和 `AppleFactory`、`PearFactory` 類打交道。有幾種水果就需要知道幾個工廠類，耦合度完全沒有下降啊，甚至還增加了代碼量！

這位讀者請先放下手中的大刀，仔細想一想，工廠模式的第二個優點在工廠方法模式中還是存在的。當構建過程相當複雜時，工廠將構建過程封裝起來，調用者可以很方便的直接使用，同樣以蘋果生產為例：

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

調用者無需知道蘋果的生產細節，當生產過程需要修改時也無需更改調用端。同時，工廠方法模式解決了簡單工廠模式的兩個弊端。

* 當生產的產品種類越來越多時，工廠類不會變成超級類。工廠類會越來越多，保持靈活。不會越來越大、變得臃腫。如果蘋果的生產過程需要修改時，只需修改蘋果工廠。梨子的生產過程需要修改時，只需修改梨子工廠。符合單一職責原則。

* 當需要生產新的產品時，無需更改既有的工廠，只需要添加新的工廠即可。保持了面向對象的可擴展性，符合開閉原則。
  OK，學以致用，接下來我們來做兩個思考題。同樣地，在以後的每一篇文章後面，都會附上幾個小練習供大家思考。希望大家能夠獨立思考出問題的答案，當然，在必要時也可參考底部的解析。

問 1：現有醫用口罩和 N95 口罩兩種產品，都繼承自 Mask 類：

```Java
abstract class Mask {
}

public class SurgicalMask extends Mask {
    @NonNull
    @Override
    public String toString() {
        return "這是醫用口罩";
    }
}

public class N95Mask extends Mask {
    @NonNull
    @Override
    public String toString() {
        return "這是 N95 口罩";
    }
}
```

請使用簡單工廠模式完成以下代碼：

```Java
public class MaskFactory {
    public Mask create(String type){
        // TODO: 使用簡單工廠模式實現此處的邏輯
    }
}
```

使其通過以下客戶端測試：

```Java
public class Client {

    @Test
    public void test() {
        MaskFactory factory = new MaskFactory();
        // 輸出：這是醫用口罩
        System.out.println(factory.create("Surgical"));
        // 輸出：這是 N95 口罩
        System.out.println(factory.create("N95"));
    }
}
```

答案：

```Java
public class MaskFactory {
    public Mask create(String type){
        // 使用簡單工廠模式實現此處的邏輯
        switch (type){
            case "Surgical":
                return new SurgicalMask();
            case "N95":
                return new N95Mask();
            default:
                throw new IllegalArgumentException("Unsupported mask type");
        }
    }
}
```

問 2：如何用工廠方法模式實現呢？

客戶端測試代碼：

```Java
public class Client {

    @Test
    public void test() {
        SurgicalMaskFactory surgicalMaskFactory = new SurgicalMaskFactory();
        // 輸出：這是醫用口罩
        System.out.println(surgicalMaskFactory.create());
        N95MaskFactory N95MaskFactory = new N95MaskFactory();
        // 輸出：這是 N95 口罩
        System.out.println(N95MaskFactory.create());
    }
}
```

答案：

```java
public class SurgicalMaskFactory{

    public Mask create() {
        return new SurgicalMask();
    }
}

public class N95MaskFactory {
    public Mask create() {
        return new N95Mask();
    }
}
```



### 抽象工廠模式

------

上一節中的工廠方法模式可以進一步優化，提取出公共的工廠接口：

```java
public interface IFactory {
    Fruit create();
}
```

然後蘋果工廠和梨子工廠都實現此接口：

```java
public class AppleFactory implements IFactory {
    @Override
    public Fruit create(){
        return new Apple();
    }
}

public class PearFactory implements IFactory {
    @Override
    public Fruit create(){
        return new Pear();
    }
}
```

此時，調用者可以將 `AppleFactory` 和 `PearFactory` 統一作為 `IFactory` 對象使用，調用者代碼如下：

```java
public class User {
    private void eat(){
        IFactory appleFactory = new AppleFactory();
        Fruit apple = appleFactory.create();
        IFactory pearFactory = new PearFactory();
        Fruit pear = pearFactory.create();
        apple.eat();
        pear.eat();
    }
}

```