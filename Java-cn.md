# Java

-----

## 八股文

### 装箱和拆箱

* 装箱：自动将基本数据类型转换为包装器类型

* 拆箱：自动将包装器类型转换为基本数据类型

实现方式：

* 装箱：如Integer，调用`Integer.valueOf(int)`
* 拆箱：如int，调用`Integer.intValue()`

1.下面这段代码的输出结果是什么？

```java
public class Main {
    public static void main(String[] args) {
         
        Integer i = 10;
        int n = i;
    }
}
```

　　也许有些朋友会说都会输出false，或者也有朋友会说都会输出true。但是事实上输出结果是：

```
true
false
```

 　为什么会出现这样的结果？输出结果表明i1和i2指向的是同一个对象，而i3和i4指向的是不同的对象。此时只需一看源码便知究竟，下面这段代码是Integer的valueOf方法的具体实现：

```java
public static Integer valueOf(int i) {
        if(i >= -128 && i <= IntegerCache.high)
            return IntegerCache.cache[i + 128];
        else
            return new Integer(i);
    }
```

　　而其中IntegerCache类的实现为：

```java
 private static class IntegerCache {
        static final int high;
        static final Integer cache[];

        static {
            final int low = -128;

            // high value may be configured by property
            int h = 127;
            if (integerCacheHighPropValue != null) {
                // Use Long.decode here to avoid invoking methods that
                // require Integer's autoboxing cache to be initialized
                int i = Long.decode(integerCacheHighPropValue).intValue();
                i = Math.max(i, 127);
                // Maximum array size is Integer.MAX_VALUE
                h = Math.min(i, Integer.MAX_VALUE - -low);
            }
            high = h;

            cache = new Integer[(high - low) + 1];
            int j = low;
            for(int k = 0; k < cache.length; k++)
                cache[k] = new Integer(j++);
        }

        private IntegerCache() {}
    }
```

　　从这2段代码可以看出，在通过valueOf方法创建Integer对象的时候，如果数值在[-128,127]之间，便返回指向IntegerCache.cache中已经存在的对象的引用；否则创建一个新的Integer对象。

　　上面的代码中i1和i2的数值为100，因此会直接从cache中取已经存在的对象，所以i1和i2指向的是同一个对象，而i3和i4则是分别指向不同的对象。

2.下面这段代码的输出结果是什么？

```java
public class Main {
    public static void main(String[] args) {
         
        Double i1 = 100.0;
        Double i2 = 100.0;
        Double i3 = 200.0;
        Double i4 = 200.0;
         
        System.out.println(i1==i2);
        System.out.println(i3==i4);
    }
}
```

　　也许有的朋友会认为跟上面一道题目的输出结果相同，但是事实上却不是。实际输出结果为：

```java
false
false
```

　　至于具体为什么，读者可以去查看Double类的valueOf的实现。

　　在这里只解释一下为什么Double类的valueOf方法会采用与Integer类的valueOf方法不同的实现。很简单：在某个范围内的整型数值的个数是有限的，而浮点数却不是。

　　注意，Integer、Short、Byte、Character、Long这几个类的valueOf方法的实现是类似的。

　　　　　Double、Float的valueOf方法的实现是类似的。

3.下面这段代码输出结果是什么：

```java
public class Main {
    public static void main(String[] args) {
         
        Boolean i1 = false;
        Boolean i2 = false;
        Boolean i3 = true;
        Boolean i4 = true;
         
        System.out.println(i1==i2);
        System.out.println(i3==i4);
    }
}
```

　　输出结果是：

```java
true
true
```

　　至于为什么是这个结果，同样地，看了Boolean类的源码也会一目了然。下面是Boolean的valueOf方法的具体实现：

```java
public static Boolean valueOf(boolean b) {
    return (b ? TRUE : FALSE);
}
```

　　而其中的 TRUE 和FALSE又是什么呢？在Boolean中定义了2个静态成员属性：

```java
 public static final Boolean TRUE = new Boolean(true);

/** 
* The <code>Boolean</code> object corresponding to the primitive 
* value <code>false</code>. 
*/
public static final Boolean FALSE = new Boolean(false);
```

　　至此，大家应该明白了为何上面输出的结果都是true了。

4.谈谈Integer i = new Integer(xxx)和Integer i =xxx;这两种方式的区别。

　　当然，这个题目属于比较宽泛类型的。但是要点一定要答上，我总结一下主要有以下这两点区别：

　　1）第一种方式不会触发自动装箱的过程；而第二种方式会触发；

　　2）在执行效率和资源占用上的区别。第二种方式的执行效率和资源占用在一般性情况下要优于第一种情况（注意这并不是绝对的）。

5.下面程序的输出结果是什么？

```java
public class Main {
    public static void main(String[] args) {
         
        Integer a = 1;
        Integer b = 2;
        Integer c = 3;
        Integer d = 3;
        Integer e = 321;
        Integer f = 321;
        Long g = 3L;
        Long h = 2L;
         
        System.out.println(c==d);
        System.out.println(e==f);
        System.out.println(c==(a+b));
        System.out.println(c.equals(a+b));
        System.out.println(g==(a+b));
        System.out.println(g.equals(a+b));
        System.out.println(g.equals(a+h));
    }
}
```

　　先别看输出结果，读者自己想一下这段代码的输出结果是什么。这里面需要注意的是：当 "=="运算符的两个操作数都是 包装器类型的引用，则是比较指向的是否是同一个对象，而如果其中有一个操作数是表达式（即包含算术运算）则比较的是数值（即会触发自动拆箱的过程）。另外，对于包装器类型，equals方法并不会进行类型转换。明白了这2点之后，上面的输出结果便一目了然：

```java
true
false
true
true
true
false
true
```

　　第一个和第二个输出结果没有什么疑问。第三句由于 a+b包含了算术运算，因此会触发自动拆箱过程（会调用intValue方法），因此它们比较的是数值是否相等。而对于c.equals(a+b)会先触发自动拆箱过程，再触发自动装箱过程，也就是说a+b，会先各自调用intValue方法，得到了加法运算后的数值之后，便调用Integer.valueOf方法，再进行equals比较。同理对于后面的也是这样，不过要注意倒数第二个和最后一个输出的结果（如果数值是int类型的，装箱过程调用的是Integer.valueOf；如果是long类型的，装箱调用的Long.valueOf方法）。