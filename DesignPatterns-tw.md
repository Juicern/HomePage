# 設計模式

[English Version](./DesignPatterns.md)



## 前言

------

### 概述

------

設計模式在面試中的考點通常是介紹其原理並說出優缺點。或者對比幾個比較相似的模式的異同點。在筆試中可能會出現畫出某個設計模式的 UML 圖這樣的題。雖說面試中佔的比重不大，但並不代表它不重要。恰恰相反，設計模式於程式設計師而言相當重要，它是我們寫出優秀程式的保障。設計模式與程式設計師的架構能力與閱讀原始碼的能力息息相關，非常值得我們深入學習。

面向物件的特點是**可維護、可複用、可擴充套件、靈活性好**，它最強大的地方在於：隨著業務變得越來越複雜，面向物件依然能夠使得程式結構良好，而面向過程卻會導致程式越來越臃腫。

讓面向物件保持結構良好的秘訣就是設計模式，面向物件結合設計模式，才能真正體會到程式變得可維護、可複用、可擴充套件、靈活性好。設計模式對於程式設計師而言並不陌生，每個程式設計師在程式設計時都會或多或少的接觸到設計模式。無論是在大型程式的架構中，亦或是在原始碼的學習中，設計模式都扮演著非常重要的角色。

### 設計模式的六大原則

------

設計模式的世界豐富多彩，比如生產一個個“產品”的工廠模式，銜接兩個不相關介面的介面卡模式，用不同的方式做同一件事的策略模式，構建步驟穩定、根據構建過程的不同配置構建出不同物件的建造者模式等等。

無論何種設計模式，都是基於六大設計原則：

* 開閉原則：一個軟體實體如類、模組和函式應該對修改封閉，對擴充套件開放。
* 單一職責原則：一個類只做一件事，一個類應該只有一個引起它修改的原因。】
* 里氏替換原則：子類應該可以完全替換父類。也就是說在使用繼承時，只擴充套件新功能，而不要破壞父類原有的功能。
* 依賴倒置原則：細節應該依賴於抽象，抽象不應依賴於細節。把抽象層放在程式設計的高層，並保持穩定，程式的細節變化由低層的實現層來完成。
* 迪米特法則：又名“最少知道原則”，一個類不應知道自己操作的類的細節，換言之，只和朋友談話，不和朋友的朋友談話。
* 介面隔離原則：客戶端不應依賴它不需要的介面。如果一個介面在實現時，部分方法由於冗餘被客戶端空實現，則應該將介面拆分，讓實現類只需依賴自己需要的介面方法。



## 構建者模式

------

### 工廠模式

在平時程式設計中，構建物件最常用的方式是 new 一個物件。乍一看這種做法沒什麼不好，而實際上這也屬於一種硬編碼。每 new 一個物件，相當於呼叫者多知道了一個類，增加了類與類之間的聯絡，不利於程式的松耦合。其實構建過程可以被封裝起來，工廠模式便是用於封裝物件的設計模式。

#### 簡單工廠模式

舉個例子，直接 new 物件的方式相當於當我們需要一個蘋果時，我們需要知道蘋果的構造方法，需要一個梨子時，需要知道梨子的構造方法。更好的實現方式是有一個水果工廠，我們告訴工廠需要什麼種類的水果，水果工廠將我們需要的水果製造出來給我們就可以了。這樣我們就無需知道蘋果、梨子是怎麼種出來的，只用和水果工廠打交道即可。

水果工廠：

```java
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

呼叫者：

```java
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

事實上，將構建過程封裝的好處不僅可以降低耦合，如果某個產品構造方法相當複雜，使用工廠模式可以大大減少程式碼重複。比如，如果生產一個蘋果需要蘋果種子、陽光、水分，將工廠修改如下：

```java
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

呼叫者的程式碼則完全不需要變化，而且呼叫者不需要在每次需要蘋果時，自己去構建蘋果種子、陽光、水分以獲得蘋果。蘋果的生產過程再複雜，也只是工廠的事。這就是封裝的好處，假如某天科學家發明了讓蘋果更香甜的肥料，要加入蘋果的生產過程中的話，也只需要在工廠中修改，呼叫者完全不用關心。

不知不覺中，我們就寫出了簡單工廠模式的程式碼。工廠模式一共有三種：

* 簡單工廠模式
* 工廠方法模式
* 抽象工廠模式

> 注：在 GoF 所著的《設計模式》一書中，簡單工廠模式被劃分為工廠方法模式的一種特例，沒有單獨被列出來。

總而言之，簡單工廠模式就是讓一個工廠類承擔構建所有物件的職責。呼叫者需要什麼產品，讓工廠生產出來即可。它的弊端也顯而易見：

* 一是如果需要生產的產品過多，此模式會導致工廠類過於龐大，承擔過多的職責，變成超級類。當蘋果生產過程需要修改時，要來修改此工廠。梨子生產過程需要修改時，也要來修改此工廠。也就是說這個類不止一個引起修改的原因。違背了單一職責原則。

* 二是當要生產新的產品時，必須在工廠類中新增新的分支。而開閉原則告訴我們：類應該對修改封閉。我們希望在新增新功能時，只需增加新的類，而不是修改既有的類，所以這就違背了開閉原則。

#### 工廠方法模式

為了解決簡單工廠模式的這兩個弊端，工廠方法模式應運而生，它規定每個產品都有一個專屬工廠。比如蘋果有專屬的蘋果工廠，梨子有專屬的梨子工廠，程式碼如下：

蘋果工廠：

```java
public class AppleFactory {
    public Fruit create(){
        return new Apple();
    }
}
```

梨子工廠：

```java
public class PearFactory {
    public Fruit create(){
        return new Pear();
    }
}
```

呼叫者：

```java
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

有讀者可能會開噴了，這樣和直接 new 出蘋果和梨子有什麼區別？上文說工廠是為了減少類與類之間的耦合，讓呼叫者儘可能少的和其他類打交道。用簡單工廠模式，我們只需要知道 `FruitFactory`，無需知道 `Apple` 、`Pear` 類，很容易看出耦合度降低了。但用工廠方法模式，呼叫者雖然不需要和 `Apple` 、`Pear` 類打交道了，但卻需要和 `AppleFactory`、`PearFactory` 類打交道。有幾種水果就需要知道幾個工廠類，耦合度完全沒有下降啊，甚至還增加了程式碼量！

這位讀者請先放下手中的大刀，仔細想一想，工廠模式的第二個優點在工廠方法模式中還是存在的。當構建過程相當複雜時，工廠將構建過程封裝起來，呼叫者可以很方便的直接使用，同樣以蘋果生產為例：

```java
public class AppleFactory {
    public Fruit create(){
        AppleSeed appleSeed = new AppleSeed();
        Sunlight sunlight = new Sunlight();
        Water water = new Water();
        return new Apple(appleSeed, sunlight, water);
    }
}
```

呼叫者無需知道蘋果的生產細節，當生產過程需要修改時也無需更改呼叫端。同時，工廠方法模式解決了簡單工廠模式的兩個弊端。

* 當生產的產品種類越來越多時，工廠類不會變成超級類。工廠類會越來越多，保持靈活。不會越來越大、變得臃腫。如果蘋果的生產過程需要修改時，只需修改蘋果工廠。梨子的生產過程需要修改時，只需修改梨子工廠。符合單一職責原則。

* 當需要生產新的產品時，無需更改既有的工廠，只需要新增新的工廠即可。保持了面向物件的可擴充套件性，符合開閉原則。
  OK，學以致用，接下來我們來做兩個思考題。同樣地，在以後的每一篇文章後面，都會附上幾個小練習供大家思考。希望大家能夠獨立思考出問題的答案，當然，在必要時也可參考底部的解析。

問 1：現有醫用口罩和 N95 口罩兩種產品，都繼承自 Mask 類：

```java
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

請使用簡單工廠模式完成以下程式碼：

```java
public class MaskFactory {
    public Mask create(String type){
        // TODO: 使用簡單工廠模式實現此處的邏輯
    }
}
```

使其透過以下客戶端測試：

```java
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

```java
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

客戶端測試程式碼：

```java
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

上一節中的工廠方法模式可以進一步最佳化，提取出公共的工廠介面：

```java
public interface IFactory {
    Fruit create();
}
```

然後蘋果工廠和梨子工廠都實現此介面：

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

此時，呼叫者可以將 `AppleFactory` 和 `PearFactory` 統一作為 `IFactory` 物件使用，呼叫者程式碼如下：

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

可以看到，我們在建立時指定了具體的工廠類後，在使用時就無需再關心是哪個工廠類，只需要將此工廠當作抽象的 IFactory 介面使用即可。這種經過抽象的工廠方法模式被稱作抽象工廠模式。

由於客戶端只和 IFactory 打交道了，呼叫的是介面中的方法，使用時根本不需要知道是在哪個具體工廠中實現的這些方法，這就使得替換工廠變得非常容易。

例如：

```java
public class User {
    private void eat(){
        IFactory factory = new AppleFactory();
        Fruit fruit = factory.create();
        fruit.eat();
    }
}
```

如果需要替換為吃梨子，只需要更改一行程式碼即可：

```java
public class User {
    private void eat(){
        IFactory factory = new PearFactory();
        Fruit fruit = factory.create();
        fruit.eat();
    }
}
```

`IFactory` 中只有一個抽象方法時，或許還看不出抽象工廠模式的威力。實際上抽象工廠模式主要用於替換一系列方法。例如將程式中的 SQL Server 資料庫整個替換為 Access 資料庫，使用抽象方法模式的話，只需在 `IFactory` 介面中定義好增刪改查四個方法，讓 `SQLFactory` 和 `AccessFactory` 實現此介面，呼叫時直接使用 IFactory 中的抽象方法即可，呼叫者無需知道使用的什麼資料庫，我們就可以非常方便的整個替換程式的資料庫，並且讓客戶端毫不知情。

抽象工廠模式很好的發揮了開閉原則、依賴倒置原則，但缺點是抽象工廠模式太重了，如果 IFactory 介面需要新增功能，則會影響到所有的具體工廠類。使用抽象工廠模式，替換具體工廠時只需更改一行程式碼，但要新增抽象方法則需要修改所有的具體工廠類。所以抽象工廠模式適用於增加同類工廠這樣的橫向擴充套件需求，不適合新增功能這樣的縱向擴充套件。

問：上一節中提到的問題如何用抽象工廠模式實現呢？

客戶端測試程式碼：

```java
public class Client {

    @Test
    public void test() {
        IFactory surgicalMaskFactory = new SurgicalMaskFactory();
        // 輸出：這是醫用口罩
        System.out.println(surgicalMaskFactory.create());
        IFactory N95MaskFactory = new N95MaskFactory();
        // 輸出：這是 N95 口罩
        System.out.println(N95MaskFactory.create());
    }
}
```

答案：

```java
public interface IFactory {
    Mask create();
}

public class SurgicalMaskFactory implements IFactory{

    @Override
    public Mask create() {
        return new SurgicalMask();
    }
}

public class N95MaskFactory implements IFactory {
    @Override
    public Mask create() {
        return new N95Mask();
    }
}
```



### 單例模式

------

單例模式非常常見，某個物件全域性只需要一個例項時，就可以使用單例模式。它的優點也顯而易見：

* 它能夠避免物件重複建立，節約空間並提升效率

* 避免由於操作不同例項導致的邏輯錯誤

單例模式有兩種實現方式：餓漢式和懶漢式。

#### 餓漢式

* 餓漢式：變數在宣告時便初始化。

```java
public class Singleton {
  
    private static Singleton instance = new Singleton();

    private Singleton() {
    }

    public static Singleton getInstance() {
        return instance;
    }
}
```

可以看到，我們將構造方法定義為 private，這就保證了其他類無法例項化此類，必須透過 getInstance 方法才能獲取到唯一的 instance 例項，非常直觀。但餓漢式有一個弊端，那就是即使這個單例不需要使用，它也會在類載入之後立即創建出來，佔用一塊記憶體，並增加類初始化時間。就好比一個電工在修理燈泡時，先把所有工具拿出來，不管是不是所有的工具都用得上。就像一個飢不擇食的餓漢，所以稱之為餓漢式。

#### 懶漢式

* 懶漢式：先宣告一個空變數，需要用時才初始化。例如：

```java
public class Singleton {
  
    private static Singleton instance = null;
  
    private Singleton() {
    }
    
    public static Singleton getInstance(){
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

我們先聲明瞭一個初始值為 null 的 instance 變數，當需要使用時判斷此變數是否已被初始化，沒有初始化的話才 new 一個例項出來。就好比電工在修理燈泡時，開始比較偷懶，什麼工具都不拿，當發現需要使用螺絲刀時，才把螺絲刀拿出來。當需要用鉗子時，再把鉗子拿出來。就像一個不到萬不得已不會行動的懶漢，所以稱之為懶漢式。

懶漢式解決了餓漢式的弊端，好處是按需載入，避免了記憶體浪費，減少了類初始化時間。

上述程式碼的懶漢式單例乍一看沒什麼問題，但其實它不是執行緒安全的。如果有多個執行緒同一時間呼叫 getInstance 方法，instance 變數可能會被例項化多次。為了保證執行緒安全，我們需要給判空過程加上鎖：

```java
public class Singleton {

    private static Singleton instance = null;

    private Singleton() {
    }

    public static Singleton getInstance() {
        synchronized (Singleton.class) {
            if (instance == null) {
                instance = new Singleton();
            }
        }
        return instance;
    }
}
```

這樣就能保證多個執行緒呼叫 getInstance 時，一次最多隻有一個執行緒能夠執行判空並 new 出例項的操作，所以 instance 只會例項化一次。但這樣的寫法仍然有問題，當多個執行緒呼叫 getInstance 時，每次都需要執行 synchronized 同步化方法，這樣會嚴重影響程式的執行效率。所以更好的做法是在同步化之前，再加上一層檢查：

```java
public class Singleton {
    
    private static Singleton instance = null;

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

這樣增加一種檢查方式後，如果 instance 已經被例項化，則不會執行同步化操作，大大提升了程式效率。上面這種寫法也就是我們平時較常用的雙檢鎖方式實現的執行緒安全的單例模式。

但這樣的懶漢式單例仍然有一個問題，JVM 底層為了最佳化程式執行效率，可能會對我們的程式碼進行指令重排序，在一些特殊情況下會導致出現空指標，為了防止這個問題，更進一步的最佳化是給 instance 變數加上 volatile 關鍵字。

有讀者可能會有疑問，我們在外面檢查了 instance == null, 那麼鎖裡面的空檢查是否可以去掉呢？

答案是不可以。如果裡面不做空檢查，可能會有兩個執行緒同時通過了外面的空檢查，然後在一個執行緒 new 出例項後，第二個執行緒進入鎖中又 new 出一個例項，導致建立多個例項。

除了雙檢鎖方式外，還有一種比較常見的靜態內部類方式保證懶漢式單例的執行緒安全：

```java
public class Singleton {
    
    private static class SingletonHolder {
        public static Singleton instance = new Singleton();
    }

    private Singleton() {
    }

    public static Singleton getInstance() {
        return SingletonHolder.instance;
    }
}
```

雖然我們經常使用這種靜態內部類的懶載入方式，但其中的原理不一定每個人都清楚。接下來我們便來分析其原理，搞清楚兩個問題：

* 靜態內部類方式是怎麼實現懶載入的

* 靜態內部類方式是怎麼保證執行緒安全的

Java 類的載入過程包括：載入、驗證、準備、解析、初始化。初始化階段即執行類的 clinit 方法（clinit = class + initialize），包括為類的靜態變數賦初始值和執行靜態程式碼塊中的內容。但不會立即載入內部類，內部類會在使用時才載入。所以當此 Singleton 類載入時，SingletonHolder 並不會被立即載入，所以不會像餓漢式那樣佔用記憶體。

另外，Java 虛擬機器規定，當訪問一個類的靜態欄位時，如果該類尚未初始化，則立即初始化此類。當呼叫Singleton 的 getInstance 方法時，由於其使用了 SingletonHolder 的靜態變數 instance，所以這時才會去初始化 SingletonHolder，在 SingletonHolder 中 new 出 Singleton 物件。這就實現了懶載入。

第二個問題的答案是 Java 虛擬機器的設計是非常穩定的，早已經考慮到了多執行緒併發執行的情況。虛擬機器在載入類的 clinit 方法時，會保證 clinit 在多執行緒中被正確的加鎖、同步。即使有多個執行緒同時去初始化一個類，一次也只有一個執行緒可以執行 clinit 方法，其他執行緒都需要阻塞等待，從而保證了執行緒安全。

懶載入方式在平時非常常見，比如開啟我們常用的美團、餓了麼、支付寶 app，應用首頁會立刻刷新出來，但其他標籤頁在我們點選到時才會重新整理。這樣就減少了流量消耗，並縮短了程式啟動時間。再比如遊戲中的某些模組，當我們點選到時才會去下載資源，而不是事先將所有資源都先下載下來，這也屬於懶載入方式，避免了記憶體浪費。

但懶漢式的缺點就是將程式載入時間從啟動時延後到了執行時，雖然啟動時間縮短了，但我們瀏覽頁面時就會看到資料的 loading 過程。如果用餓漢式將頁面提前載入好，我們瀏覽時就會特別的順暢，也不失為一個好的使用者體驗。比如我們常用的 QQ、微信 app，作為即時通訊的工具軟體，它們會在啟動時立即重新整理所有的資料，保證使用者看到最新最全的內容。著名的軟體大師 Martin 在《程式碼整潔之道》一書中也說到：不提倡使用懶載入方式，因為程式應該將構建與使用分離，達到解耦。餓漢式在宣告時直接初始化變數的方式也更直觀易懂。所以在使用餓漢式還是懶漢式時，需要權衡利弊。

一般的建議是：對於構建不復雜，載入完成後會立即使用的單例物件，推薦使用餓漢式。對於構建過程耗時較長，並不是所有使用此類都會用到的單例物件，推薦使用懶漢式。

問：雙檢鎖單例模式中，volatile 主要用來防止哪幾條指令重排序？如果發生了重排序，會導致什麼樣的錯誤？

答案：

```java
instance = new Singleton();
```

這一行程式碼中，執行了三條重要的指令：

* 分配物件的記憶體空間

* 初始化物件

將變數 instance 指向剛分配的記憶體空間
在這個過程中，如果第二條指令和第三條指令發生了重排序，可能導致 instance 還未初始化時，其他執行緒提前透過雙檢鎖外層的 null 檢查，獲取到“不為 null，但還沒有執行初始化”的 instance 物件，發生空指標異常。



### 建造者模式

------

建造者模式用於建立過程穩定，但配置多變的物件。在《設計模式》一書中的定義是：**將一個複雜的構建與其表示相分離，使得同樣的構建過程可以建立不同的表示。**

經典的“建造者-指揮者”模式現在已經不太常用了，現在建造者模式主要用來透過鏈式呼叫生成不同的配置。比如我們要製作一杯珍珠奶茶。它的製作過程是穩定的，除了必須要知道奶茶的種類和規格外，是否加珍珠和是否加冰是可選的。使用建造者模式表示如下：

```java
public class MilkTea {
    private final String type;
    private final String size;
    private final boolean pearl;
    private final boolean ice;

	private MilkTea() {}
	
    private MilkTea(Builder builder) {
        this.type = builder.type;
        this.size = builder.size;
        this.pearl = builder.pearl;
        this.ice = builder.ice;
    }

    public String getType() {
        return type;
    }

    public String getSize() {
        return size;
    }

    public boolean isPearl() {
        return pearl;
    }
    public boolean isIce() {
        return ice;
    }

    public static class Builder {

        private final String type;
        private String size = "中杯";
        private boolean pearl = true;
        private boolean ice = false;

        public Builder(String type) {
            this.type = type;
        }

        public Builder size(String size) {
            this.size = size;
            return this;
        }

        public Builder pearl(boolean pearl) {
            this.pearl = pearl;
            return this;
        }

        public Builder ice(boolean cold) {
            this.ice = cold;
            return this;
        }

        public MilkTea build() {
            return new MilkTea(this);
        }
    }
}
```

可以看到，我們將 MilkTea 的構造方法設定為私有的，所以外部不能透過 new 構建出 MilkTea 例項，只能透過 Builder 構建。對於必須配置的屬性，透過 Builder 的構造方法傳入，可選的屬性透過 Builder 的鏈式呼叫方法傳入，如果不配置，將使用預設配置，也就是中杯、加珍珠、不加冰。根據不同的配置可以製作出不同的奶茶：

```java
public class User {
    private void buyMilkTea() {
        MilkTea milkTea = new MilkTea.Builder("原味").build();
        show(milkTea);

        MilkTea chocolate =new MilkTea.Builder("巧克力味")
                .ice(false)
                .build();
        show(chocolate);
        
        MilkTea strawberry = new MilkTea.Builder("草莓味")
                .size("大杯")
                .pearl(false)
                .ice(true)
                .build();
        show(strawberry);
    }

    private void show(MilkTea milkTea) {
        String pearl;
        if (milkTea.isPearl())
            pearl = "加珍珠";
        else
            pearl = "不加珍珠";
        String ice;
        if (milkTea.isIce()) {
            ice = "加冰";
        } else {
            ice = "不加冰";
        }
        System.out.println("一份" + milkTea.getSize() + "、"
                + pearl + "、"
                + ice + "的"
                + milkTea.getType() + "奶茶");
    }
}
```

執行程式，輸出如下：

```
一份中杯、加珍珠、不加冰的原味奶茶
一份中杯、加珍珠、不加冰的巧克力味奶茶
一份大杯、不加珍珠、加冰的草莓味奶茶
```

使用建造者模式的好處是不用擔心忘了指定某個配置，保證了構建過程是穩定的。在 OkHttp、Retrofit 等著名框架的原始碼中都使用到了建造者模式。



### 原型模式

原型模式：**用原型例項指定建立物件的種類，並且透過複製這些原型建立新的物件。**

定義看起來有點繞口，實際上在 Java 中，Object 的 clone() 方法就屬於原型模式，不妨簡單的理解為：原型模式就是用來克隆物件的。

舉個例子，比如有一天，周杰倫到奶茶店點了一份不加冰的原味奶茶，你說我是周杰倫的忠實粉，我也要一份跟周杰倫一樣的。用程式表示如下：

奶茶類：

```java
public class MilkTea {
    public String type;
    public boolean ice;
}
```

下單：

```java
private void order(){
    MilkTea milkTeaOfJay = new MilkTea();
    milkTeaOfJay.type = "原味";
    milkTeaOfJay.ice = false;
    
    MilkTea yourMilkTea = milkTeaOfJay;
}
```

好像沒什麼問題，將周杰倫的奶茶直接賦值到你的奶茶上就行了，看起來我們並不需要 clone 方法。但是這樣真的是複製了一份奶茶嗎？

當然不是，Java 的賦值只是引用傳遞，而不是值傳遞。這樣賦值之後，yourMilkTea 仍然指向的周杰倫的奶茶，並不會多一份一樣的奶茶。

那麼我們要怎麼做才能點一份一樣的奶茶呢？將程式修改如下就可以了：

```java
private void order(){
    MilkTea milkTeaOfJay = new MilkTea();
    milkTeaOfJay.type = "原味";
    milkTeaOfJay.ice = false;
    
    MilkTea yourMilkTea = new MilkTea();
    yourMilkTea.type = "原味";
    yourMilkTea.ice = false;
}
```

只有這樣，yourMilkTea 才是 new 出來的一份全新的奶茶。我們設想一下，如果有一千個粉絲都需要點和周杰倫一樣的奶茶的話，按照現在的寫法就需要 new 一千次，併為每一個新的物件賦值一千次，造成大量的重複。

更糟糕的是，如果周杰倫臨時決定加個冰，那麼粉絲們的奶茶配置也要跟著修改：

```java
private void order(){
    MilkTea milkTeaOfJay = new MilkTea();
    milkTeaOfJay.type = "原味";
    milkTeaOfJay.ice = true;
    
    MilkTea yourMilkTea = new MilkTea();
    yourMilkTea.type = "原味";
    yourMilkTea.ice = true;
    
    // 將一千個粉絲的 ice 都修改為 true
    ...
}
```

大批次的修改無疑是非常醜陋的做法，這就是我們需要 clone 方法的理由！

運用原型模式，在 MilkTea 中新增 clone 方法：

```java
public class MilkTea{
    public String type;
    public boolean ice;

    public MilkTea clone(){
        MilkTea milkTea = new MilkTea();
        milkTea.type = this.type;
        milkTea.ice = this.ice;
        return milkTea;
    }
}
```

下單：

```java
private void order(){
    MilkTea milkTeaOfJay = new MilkTea();
    milkTeaOfJay.type = "原味";
    milkTeaOfJay.ice = false;
    
    MilkTea yourMilkTea = milkTeaOfJay.clone();
    
    // 一千位粉絲都呼叫 milkTeaOfJay 的 clone 方法即可
    ...
}
```

這就是原型模式，Java 中有一個語法糖，讓我們並不需要手寫 clone 方法。這個語法糖就是 Cloneable 介面，我們只要讓需要複製的類實現此介面即可。

```java
public class MilkTea implements Cloneable{
    public String type;
    public boolean ice;

    @NonNull
    @Override
    protected MilkTea clone() throws CloneNotSupportedException {
        return (MilkTea) super.clone();
    }
}
```

值得注意的是，Java 自帶的 clone 方法是淺複製的。也就是說呼叫此物件的 clone 方法，只有基本型別的引數會被複製一份，非基本型別的物件不會被複製一份，而是繼續使用傳遞引用的方式。如果需要實現深複製，必須要自己手動修改 clone 方法才行。



### 小結

------

* 工廠方法模式：為每一類物件建立工廠，將物件交由工廠建立，客戶端只和工廠打交道。

* 抽象工廠模式：為每一類工廠提取出抽象介面，使得新增工廠、替換工廠變得非常容易。

* 建造者模式：用於建立構造過程穩定的物件，不同的 Builder 可以定義不同的配置。

* 單例模式：全域性使用同一個物件，分為餓漢式和懶漢式。懶漢式有雙檢鎖和內部類兩種實現方式。

* 原型模式：為一個類定義 clone 方法，使得建立相同的物件更方便。



## 結構型模式

------

### 介面卡模式

------

說到介面卡，我們最熟悉的莫過於電源介面卡了，也就是手機的充電頭。它就是介面卡模式的一個應用。

試想一下，你有一條連線電腦和手機的 USB 資料線，連線電腦的一端從電腦介面處接收 5V 的電壓，連線手機的一端向手機輸出 5V 的電壓，並且他們工作良好。

中國的家用電壓都是 220V，所以 USB 資料線不能直接拿來給手機充電，這時候我們有兩種方案：

* 單獨製作手機充電器，接收 220V 家用電壓，輸出 5V 電壓。

* 新增一個介面卡，將 220V 家庭電壓轉化為類似電腦介面的 5V 電壓，再連線資料線給手機充電。

如果你使用過早期的手機，就會知道以前的手機廠商採用的就是第一種方案：早期的手機充電器都是單獨製作的，充電頭和充電線是連在一起的。現在的手機都採用了電源介面卡加資料線的方案。這是生活中應用介面卡模式的一個進步。

> 介面卡模式：將一個類的介面轉換成客戶希望的另外一個介面，使得原本由於介面不相容而不能一起工作的那些類能一起工作。

適配的意思是適應、匹配。通俗地講，介面卡模式適用於有相關性但不相容的結構，源介面透過一箇中間件轉換後才可以適用於目標介面，這個轉換過程就是適配，這個中介軟體就稱之為介面卡。

家用電源和 USB 資料線有相關性：家用電源輸出電壓，USB 資料線輸入電壓。但兩個介面無法相容，因為一個輸出 220V，一個輸入 5V，透過介面卡將輸出 220V 轉換成輸出 5V 之後才可以一起工作。

讓我們用程式來模擬一下這個過程。

首先，家庭電源提供 220V 的電壓：

```java
class HomeBattery {
    int supply() {
        // 家用電源提供一個 220V 的輸出電壓
        return 220;
    }
}
```

USB 資料線只接收 5V 的充電電壓：

```java
class USBLine {
    void charge(int volt) {
        // 如果電壓不是 5V，丟擲異常
        if (volt != 5) throw new IllegalArgumentException("只能接收 5V 電壓");
        // 如果電壓是 5V，正常充電
        System.out.println("正常充電");
    }
}
```

先來看看適配之前，使用者如果直接用家庭電源給手機充電：

```java
public class User {
    @Test
    public void chargeForPhone() {
        HomeBattery homeBattery = new HomeBattery();
        int homeVolt = homeBattery.supply();
        System.out.println("家庭電源提供的電壓是 " + homeVolt + "V");

        USBLine usbLine = new USBLine();
        usbLine.charge(homeVolt);
    }
}
```

執行程式，輸出如下：

```java
家庭電源提供的電壓是 220V

java.lang.IllegalArgumentException: 只能接收 5V 電壓
```

這時，我們加入電源介面卡：

```java
class Adapter {
    int convert(int homeVolt) {
        // 適配過程：使用電阻、電容等器件將其降低為輸出 5V
        int chargeVolt = homeVolt - 215;
        return chargeVolt;
    }
}
```

然後，使用者再使用介面卡將家庭電源提供的電壓轉換為充電電壓：

```java
public class User {
    @Test
    public void chargeForPhone() {
        HomeBattery homeBattery = new HomeBattery();
        int homeVolt = homeBattery.supply();
        System.out.println("家庭電源提供的電壓是 " + homeVolt + "V");

        Adapter adapter = new Adapter();
        int chargeVolt = adapter.convert(homeVolt);
        System.out.println("使用介面卡將家庭電壓轉換成了 " + chargeVolt + "V");

        USBLine usbLine = new USBLine();
        usbLine.charge(chargeVolt);
    }
}
```

執行程式，輸出如下：

```java
家庭電源提供的電壓是 220V
使用介面卡將家庭電壓轉換成了 5V
正常充電
```

這就是介面卡模式。在我們日常的開發中經常會使用到各種各樣的 Adapter，都屬於介面卡模式的應用。

但介面卡模式並不推薦多用。因為未雨綢繆好過亡羊補牢，如果事先能預防介面不同的問題，不匹配問題就不會發生，只有遇到源介面無法改變時，才應該考慮使用介面卡。比如現代的電源插口中很多已經增加了專門的 USB 充電介面，讓我們不需要再使用介面卡轉換介面，這又是社會的一個進步。



### 橋接模式

-------

考慮這樣一個需求：繪製矩形、圓形、三角形這三種圖案。按照面向物件的理念，我們至少需要三個具體類，對應三種不同的圖形。

抽象介面 IShape：

```java
public interface IShape {
    void draw();
}
```

三個具體形狀類：

```java
class Rectangle implements IShape {
    @Override
    public void draw() {
        System.out.println("繪製矩形");
    }
}

class Round implements IShape {
    @Override
    public void draw() {
        System.out.println("繪製圓形");
    }
}

class Triangle implements IShape {
    @Override
    public void draw() {
        System.out.println("繪製三角形");
    }
}
```

接下來我們有了新的需求，每種形狀都需要有四種不同的顏色：紅、藍、黃、綠。

這時我們很容易想到兩種設計方案：

* 為了複用形狀類，將每種形狀定義為父類，每種不同顏色的圖形繼承自其形狀父類。此時一共有 12 個類。

* 為了複用顏色類，將每種顏色定義為父類，每種不同顏色的圖形繼承自其顏色父類。此時一共有 12 個類。

乍一看沒什麼問題，我們使用了面向物件的繼承特性，複用了父類的程式碼並擴充套件了新的功能。

但仔細想一想，如果以後要增加一種顏色，比如黑色，那麼我們就需要增加三個類；如果再要增加一種形狀，我們又需要增加五個類，對應 5 種顏色。

更不用說遇到增加 20 個形狀，20 種顏色的需求，不同的排列組合將會使工作量變得無比的龐大。看來我們不得不重新思考設計方案。

形狀和顏色，都是圖形的兩個屬性。他們兩者的關係是平等的，所以不屬於繼承關係。更好的的實現方式是：**將形狀和顏色分離，根據需要對形狀和顏色進行組合**，這就是橋接模式的思想。

> 橋接模式：將抽象部分與它的實現部分分離，使它們都可以獨立地變化。它是一種物件結構型模式，又稱為柄體模式或介面模式。

官方定義非常精準、簡練，但卻有點不易理解。通俗地說，如果一個物件有兩種或者多種分類方式，並且兩種分類方式都容易變化，比如本例中的形狀和顏色。這時使用繼承很容易造成子類越來越多，所以更好的做法是把這種分類方式分離出來，讓他們獨立變化，使用時將不同的分類進行組合即可。

說到這裡，不得不提一個設計原則：合成 / 聚合複用原則。雖然它沒有被劃分到六大設計原則中，但它在面向物件的設計中也非常的重要。

> 合成 / 聚合複用原則：優先使用合成 / 聚合，而不是類繼承。

繼承雖然是面向物件的三大特性之一，但繼承會導致子類與父類有非常緊密的依賴關係，它會限制子類的靈活性和子類的複用性。而使用合成 / 聚合，也就是使用介面實現的方式，就不存在依賴問題，一個類可以實現多個介面，可以很方便地拓展功能。

讓我們一起來看一下本例使用橋接模式的程式實現：

新建介面類 IColor，僅包含一個獲取顏色的方法：

```java
public interface IColor {
    String getColor();
}
```

每種顏色都實現此介面：

```java
public class Red implements IColor {
    @Override
    public String getColor() {
        return "紅";
    }
}

public class Blue implements IColor {
    @Override
    public String getColor() {
        return "藍";
    }
}

public class Yellow implements IColor {
    @Override
    public String getColor() {
        return "黃";
    }
}

public class Green implements IColor {
    @Override
    public String getColor() {
        return "綠";
    }
}
```

在每個形狀類中，橋接 IColor 介面：

```java
class Rectangle implements IShape {

    private IColor color;

    void setColor(IColor color) {
        this.color = color;
    }

    @Override
    public void draw() {
        System.out.println("繪製" + color.getColor() + "矩形");
    }
}

class Round implements IShape {

    private IColor color;

    void setColor(IColor color) {
        this.color = color;
    }

    @Override
    public void draw() {
        System.out.println("繪製" + color.getColor() + "圓形");
    }
}

class Triangle implements IShape {

    private IColor color;

    void setColor(IColor color) {
        this.color = color;
    }

    @Override
    public void draw() {
        System.out.println("繪製" + color.getColor() + "三角形");
    }
}
```

測試函式：

```java
@Test
public void drawTest() {
    Rectangle rectangle = new Rectangle();
    rectangle.setColor(new Red());
    rectangle.draw();
    
    Round round = new Round();
    round.setColor(new Blue());
    round.draw();
    
    Triangle triangle = new Triangle();
    triangle.setColor(new Yellow());
    triangle.draw();
}
```

執行程式，輸出如下：

```java
繪製紅矩形
繪製藍圓形
繪製黃三角形
```

這時我們再來回顧一下官方定義：將抽象部分與它的實現部分分離，使它們都可以獨立地變化。抽象部分指的是父類，對應本例中的形狀類，實現部分指的是不同子類的區別之處。將子類的區別方式 —— 也就是本例中的顏色 —— 分離成介面，透過組合的方式橋接顏色和形狀，這就是橋接模式，它主要用於**兩個或多個同等級的介面。**



### 組合模式

------

上文說到，橋接模式用於將同等級的介面互相組合，那麼組合模式和橋接模式有什麼共同點嗎？

事實上組合模式和橋接模式的組合完全不一樣。組合模式用於整體與部分的結構，當整體與部分有相似的結構，在操作時可以被一致對待時，就可以使用組合模式。例如：

* 資料夾和子資料夾的關係：資料夾中可以存放檔案，也可以新建資料夾，子資料夾也一樣。

* 總公司子公司的關係：總公司可以設立部門，也可以設立分公司，子公司也一樣。

* 樹枝和分樹枝的關係：樹枝可以長出葉子，也可以長出樹枝，分樹枝也一樣。

在這些關係中，雖然整體包含了部分，但無論整體或部分，都具有一致的行為。

> 組合模式：又叫部分整體模式，是用於把一組相似的物件當作一個單一的物件。組合模式依據樹形結構來組合物件，用來表示部分以及整體層次。這種型別的設計模式屬於結構型模式，它建立了物件組的樹形結構。

考慮這樣一個實際應用：設計一個公司的人員分佈結構，結構如下圖所示。

![](.\img\odwCrq-image.png)

我們注意到人員結構中有兩種結構，一是管理者，如老闆，PM，CFO，CTO，二是職員。其中有的管理者不僅僅要管理職員，還會管理其他的管理者。這就是一個典型的整體與部分的結構。

#### 不適用組合模式的設計方案

要描述這樣的結構，我們很容易想到以下設計方案：

新建管理者類：

```java
public class Manager {
    // 職位
    private String position;
    // 工作內容
    private String job;
    // 管理的管理者
    private List<Manager> managers = new ArrayList<>();
    // 管理的職員
    private List<Employee> employees = new ArrayList<>();

    public Manager(String position, String job) {
        this.position = position;
        this.job = job;
    }
    
    public void addManager(Manager manager) {
        managers.add(manager);
    }

    public void removeManager(Manager manager) {
        managers.remove(manager);
    }

    public void addEmployee(Employee employee) {
        employees.add(employee);
    }

    public void removeEmployee(Employee employee) {
        employees.remove(employee);
    }

    // 做自己的本職工作
    public void work() {
        System.out.println("我是" + position + "，我正在" + job);
    }
    
    // 檢查下屬
    public void check() {
        work();
        for (Employee employee : employees) {
            employee.work();
        }
        for (Manager manager : managers) {
            manager.check();
        }
    }
}
```

新建職員類：

```java
public class Employee {
    // 職位
    private String position;
    // 工作內容
    private String job;

    public Employee(String position, String job) {
        this.position = position;
        this.job = job;
    }

    // 做自己的本職工作
    public void work() {
        System.out.println("我是" + position + "，我正在" + job);
    }
}
```

客戶端建立人員結構關係：

```java
public class Client {
    
    @Test
    public void test() {
        Manager boss = new Manager("老闆", "唱怒放的生命");
        Employee HR = new Employee("人力資源", "聊微信");
        Manager PM = new Manager("產品經理", "不知道幹啥");
        Manager CFO = new Manager("財務主管", "看劇");
        Manager CTO = new Manager("技術主管", "划水");
        Employee UI = new Employee("設計師", "畫畫");
        Employee operator = new Employee("運營人員", "兼職客服");
        Employee webProgrammer = new Employee("程式設計師", "學習設計模式");
        Employee backgroundProgrammer = new Employee("後臺程式設計師", "CRUD");
        Employee accountant = new Employee("會計", "背九九乘法表");
        Employee clerk = new Employee("文員", "給老闆遞麥克風");
        boss.addEmployee(HR);
        boss.addManager(PM);
        boss.addManager(CFO);
        PM.addEmployee(UI);
        PM.addManager(CTO);
        PM.addEmployee(operator);
        CTO.addEmployee(webProgrammer);
        CTO.addEmployee(backgroundProgrammer);
        CFO.addEmployee(accountant);
        CFO.addEmployee(clerk);

        boss.check();
    }
}
```

執行測試方法，輸出如下（為方便檢視，筆者添加了縮排）：

```java
我是老闆，我正在唱怒放的生命
	我是人力資源，我正在聊微信
	我是產品經理，我正在不知道幹啥
		我是設計師，我正在畫畫
		我是運營人員，我正在兼職客服
		我是技術主管，我正在划水
			我是程式設計師，我正在學習設計模式
			我是後臺程式設計師，我正在CRUD
	我是財務主管，我正在看劇
		我是會計，我正在背九九乘法表
		我是文員，我正在給老闆遞麥克風
```

這樣我們就設計出了公司的結構，但是這樣的設計有兩個弊端：

* name 欄位，job 欄位，work 方法重複了。

* 管理者對其管理的管理者和職員需要區別對待。

關於第一個弊端，雖然這裡為了講解，只有兩個欄位和一個方法重複，實際工作中這樣的整體部分結構會有相當多的重複。比如此例中還可能有工號、年齡等欄位，領取工資、上下班打卡、開各種無聊的會等方法。

大量的重複顯然是很醜陋的程式碼，分析一下可以發現， Manager 類只比 Employee 類多一個管理人員的列表欄位，多幾個增加 / 移除人員的方法，其他的欄位和方法全都是一樣的。

有讀者應該會想到：我們可以將重複的欄位和方法提取到一個工具類中，讓 Employee 和 Manager 都去呼叫此工具類，就可以消除重複了。

這樣固然可行，但屬於 Employee 和 Manager 類自己的東西卻要透過其他類呼叫，並不利於程式的高內聚。

關於第二個弊端，此方案無法解決，此方案中 Employee 和 Manager 類完全是兩個不同的物件，兩者的相似性被忽略了。

所以我們有更好的設計方案，那就是組合模式！

#### 使用組合模式的設計方案

組合模式最主要的功能就是讓使用者可以一致對待整體和部分結構，將兩者都作為一個相同的元件，所以我們先新建一個抽象的元件類：

```java
public abstract class Component {
    // 職位
    private String position;
    // 工作內容
    private String job;

    public Component(String position, String job) {
        this.position = position;
        this.job = job;
    }

    // 做自己的本職工作
    public void work() {
        System.out.println("我是" + position + "，我正在" + job);
    }

    abstract void addComponent(Component component);

    abstract void removeComponent(Component component);

    abstract void check();
}
```

管理者繼承自此抽象類：

```java
public class Manager extends Component {
    // 管理的元件
    private List<Component> components = new ArrayList<>();

    public Manager(String position, String job) {
        super(position, job);
    }

    @Override
    public void addComponent(Component component) {
        components.add(component);
    }

    @Override
    void removeComponent(Component component) {
        components.remove(component);
    }

    // 檢查下屬
    @Override
    public void check() {
        work();
        for (Component component : components) {
            component.check();
        }
    }
}
```

職員同樣繼承自此抽象類：

```java
public class Employee extends Component {

    public Employee(String position, String job) {
        super(position, job);
    }

    @Override
    void addComponent(Component component) {
        System.out.println("職員沒有管理許可權");
    }

    @Override
    void removeComponent(Component component) {
        System.out.println("職員沒有管理許可權");
    }

    @Override
    void check() {
        work();
    }
}
```

修改客戶端如下：

```java
public class Client {

    @Test
    public void test(){
        Component boss = new Manager("老闆", "唱怒放的生命");
        Component HR = new Employee("人力資源", "聊微信");
        Component PM = new Manager("產品經理", "不知道幹啥");
        Component CFO = new Manager("財務主管", "看劇");
        Component CTO = new Manager("技術主管", "划水");
        Component UI = new Employee("設計師", "畫畫");
        Component operator = new Employee("運營人員", "兼職客服");
        Component webProgrammer = new Employee("程式設計師", "學習設計模式");
        Component backgroundProgrammer = new Employee("後臺程式設計師", "CRUD");
        Component accountant = new Employee("會計", "背九九乘法表");
        Component clerk = new Employee("文員", "給老闆遞麥克風");
        boss.addComponent(HR);
        boss.addComponent(PM);
        boss.addComponent(CFO);
        PM.addComponent(UI);
        PM.addComponent(CTO);
        PM.addComponent(operator);
        CTO.addComponent(webProgrammer);
        CTO.addComponent(backgroundProgrammer);
        CFO.addComponent(accountant);
        CFO.addComponent(clerk);

        boss.check();
    }
}
```

執行測試方法，輸出結果與之前的結果一模一樣。

可以看到，使用組合模式後，我們解決了之前的兩個弊端。一是將共有的欄位與方法移到了父類中，消除了重複，並且在客戶端中，可以一致對待 Manager 和 Employee 類：

* Manager 類和 Employee 類統一宣告為 Component 物件

* 統一呼叫 Component 物件的 addComponent 方法新增子物件即可

#### 組合模式中的安全方式與透明方式

讀者可能已經注意到了，Employee 類雖然繼承了父類的 addComponent 和 removeComponent 方法，但是僅僅提供了一個空實現，因為 Employee 類是不支援新增和移除元件的。這樣是否違背了介面隔離原則呢？

> 介面隔離原則：客戶端不應依賴它不需要的介面。如果一個介面在實現時，部分方法由於冗餘被客戶端空實現，則應該將介面拆分，讓實現類只需依賴自己需要的介面方法。

答案是肯定的，這樣確實違背了介面隔離原則。這種方式在組合模式中被稱作透明方式.

> 透明方式：在 Component 中宣告所有管理子物件的方法，包括 add 、remove 等，這樣繼承自 Component 的子類都具備了 add、remove 方法。對於外界來說葉節點和枝節點是透明的，它們具備完全一致的介面。

這種方式有它的優點：讓 Manager 類和 Employee 類具備完全一致的行為介面，呼叫者可以一致對待它們。

但它的缺點也顯而易見：Employee 類並不支援管理子物件，不僅違背了介面隔離原則，而且客戶端可以用 Employee 類呼叫 addComponent 和 removeComponent 方法，導致程式出錯，所以這種方式是不安全的。

那麼我們可不可以將 addComponent 和 removeComponent 方法移到 Manager 子類中去單獨實現，讓 Employee 不再實現這兩個方法呢？我們來嘗試一下。

將抽象類修改為：

```java
public abstract class Component {
    // 職位
    private String position;
    // 工作內容
    private String job;

    public Component(String position, String job) {
        this.position = position;
        this.job = job;
    }

    // 做自己的本職工作
    public void work() {
        System.out.println("我是" + position + "，我正在" + job);
    }

    abstract void check();
}
```

可以看到，我們在父類中去掉了 addComponent 和 removeComponent 這兩個抽象方法。

Manager 類修改為：

```java
public class Manager extends Component {
    // 管理的元件
    private List<Component> components = new ArrayList<>();

    public Manager(String position, String job) {
        super(position, job);
    }

    public void addComponent(Component component) {
        components.add(component);
    }

    void removeComponent(Component component) {
        components.remove(component);
    }

    // 檢查下屬
    @Override
    public void check() {
        work();
        for (Component component : components) {
            component.check();
        }
    }
}
```

Manager 類單獨實現了 addComponent 和 removeComponent 這兩個方法，去掉了 @Override 註解。

Employee 類修改為：

```java
public class Employee extends Component {

    public Employee(String position, String job) {
        super(position, job);
    }

    @Override
    void check() {
        work();
    }
}
```

客戶端建立人員結構關係：

```java
public class Client {

    @Test
    public void test(){
        Manager boss = new Manager("老闆", "唱怒放的生命");
        Employee HR = new Employee("人力資源", "聊微信");
        Manager PM = new Manager("產品經理", "不知道幹啥");
        Manager CFO = new Manager("財務主管", "看劇");
        Manager CTO = new Manager("技術主管", "划水");
        Employee UI = new Employee("設計師", "畫畫");
        Employee operator = new Employee("運營人員", "兼職客服");
        Employee webProgrammer = new Employee("程式設計師", "學習設計模式");
        Employee backgroundProgrammer = new Employee("後臺程式設計師", "CRUD");
        Employee accountant = new Employee("會計", "背九九乘法表");
        Employee clerk = new Employee("文員", "給老闆遞麥克風");
        boss.addComponent(HR);
        boss.addComponent(PM);
        boss.addComponent(CFO);
        PM.addComponent(UI);
        PM.addComponent(CTO);
        PM.addComponent(operator);
        CTO.addComponent(webProgrammer);
        CTO.addComponent(backgroundProgrammer);
        CFO.addComponent(accountant);
        CFO.addComponent(clerk);

        boss.check();
    }
}
```

執行程式，輸出結果與之前一模一樣。

這種方式在組合模式中稱之為安全方式。

> 安全方式：在 Component 中不宣告 add 和 remove 等管理子物件的方法，這樣葉節點就無需實現它，只需在枝節點中實現管理子物件的方法即可。

安全方式遵循了介面隔離原則，但由於不夠透明，Manager 和 Employee 類不具有相同的介面，在客戶端中，我們無法將 Manager 和 Employee 統一宣告為 Component 類了，必須要區別對待，帶來了使用上的不方便。

安全方式和透明方式各有好處，在使用組合模式時，需要根據實際情況決定。但大多數使用組合模式的場景都是採用的透明方式，雖然它有點不安全，但是客戶端無需做任何判斷來區分是葉子結點還是枝節點，用起來是真香。

問：組合模式中的安全方式與透明方式有什麼區別？

答案：透明方式：在 Component 中宣告所有管理子物件的方法，包括 add 、remove 等，這樣繼承自 Component 的子類都具備了 add、remove 方法。對於外界來說葉節點和枝節點是透明的，它們具備完全一致的介面。

安全方式：在 Component 中不宣告 add 和 remove 等管理子物件的方法，這樣葉節點就無需實現它，只需在枝節點中實現管理子物件的方法即可。