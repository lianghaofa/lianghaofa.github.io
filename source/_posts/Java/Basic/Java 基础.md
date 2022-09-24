---
title: Java 基础
summary: Java 基础
categories: java
date: 2022-05-26 08:25:00
tags:
- java
---

## Java 基础

### String StringBuilder StringBuffer

底层是 private final char value[]; char数组

每次都找常量池 ？ 效率不会低吗 ？ 用哈希直接找 ？

在HotSpot 实现的string pool功能的是一个StringTable类，它是一个Hash表，默认值大小长度是1009；这个StringTable在每个HotSpot VM的实例只有一份，被所有的类共享。字符串常量由一个一个字符组成，放在了StringTable上。
在JDK6.0中，StringTable的长度是固定的，长度就是1009，因此如果放入String Pool中的String非常多，就会造成hash冲突，导致链表过长，当调用String#intern()时会需要到链表上一个一个找，从而导致性能大幅度下降；
在JDK7.0中，StringTable的长度可以通过参数指定：

```java
String a = new String("abc"); 
```
堆中对象a,a的value指向常量池中的 "abc"； 


**拼接字符串，默认使用了StringBuilder append**
```java
String b = a + "def";
```





String 不可变 ？ 

```java
String a = new String("abc"); 
```
a 存放的地址为 0x11.

常量池 ”abc" 的地址为 0x11， ”abcdef" 的地址为 0x12

a 只能指向 0x11，由于0x11的内容不可变， 导致 a 也不可变


StringBuilder,StringBuffer 可变 ？


```java
StringBuilder sb  = new StringBuilder("abc"); 
```

sb 指向了一个对象，但由于对象中的 value[] 数组可变。 容量不足，可以扩容，int newCapacity = value.length * 2 + 2;

value = Arrays.copyOf(value, newCapacity);

StringBuffer 跟 StringBuilder 方法一样，但 StringBuffer 方法前加了 synchronized 关键字。


### 枚举 - 有限、确定

父类 java.lang.Object
```java
public class Season {
    
    // 属性
    private final String name;

    private final String desc;

    private Season(String name, String desc){
        new String();
        this.name = name;
        this.desc = desc;
    }

    public static final Season SPRING = new Season("spring", "sp");
    public static final Season SUMMER = new Season("summer", "su");
    public static final Season AUTUMN = new Season("autumn", "au");
    public static final Season WINTER = new Season("winter", "win");

    public String getName() {
        return name;
    }

    public String getDesc() {
        return desc;
    }

    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}
```

enum 父类 java.lang.Enum
```java

public enum Season {

    // 规定 常量放到最开始的位置
    SPRING("spring", "sp"),
    SUMMER("summer", "su"),
    AUTUMN("autumn", "au"),
    WINTER("winter", "win");

    // 属性
    private final String name;
    private final String desc;
    private Season(String name, String desc){
        this.name = name;
        this.desc = desc;
    }

    public String getName() {
        return name;
    }

    public String getDesc() {
        return desc;
    }

    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}
```


### ArrayList & Vector

JDK 1.7 底层为Object[]数组，数组初始化容量 DEFAULT_CAPACITY = 10；扩容为原来的 1.5 倍.

JDK 1.8 底层为Object[]数组，数组初始化为了{}，调用add方法后 DEFAULT_CAPACITY = 10 ；扩容为原来的 1.5 倍.

**ArrayList线程不安全**

```java
int newCapacity = oldCapacity + (oldCapacity >> 1);
```

JDK 1.8 底层为Object[]数组，数组初始化容量 DEFAULT_CAPACITY = 10 ；扩容为原来的 2 倍。

**Vector线程安全**

```java
int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
capacityIncrement : oldCapacity);
```

### HashSet & LinkedHashSet

hasCode 计算哈希值

计算哈希值后确认数组的位置，多个元素，equals方式是否已经存在。

数组 + 链表

数组长度，

数组类型

hashCode equals方法

同一位置的数据冲突后，如何处理？前方还是末尾？



存放自定义的类需要重写： hashCode , equals

关于Object对象的hashCode()返回值，网上对它就是一个简单的描述：“JVM根据某种策略生成的”，那么这种策略到底是什么呢？

-XX:hashCode=0 利用 Park-Miller 伪随机数生成器生成哈希值

-XX:hashCode=1或者4 基于对象指针 OOPs

-XX:hashCode=2 敏感测试，恒定为1

-XX:hashCode=3 自增序列

-XX:hashCode=5 默认实现
采用的算法是 Marsaglia's xor-shift 随机数生成法。一种快速并且散列性好的哈希算法。


HashSet 无序
- 数组 + 链表

LinkedHashSet 有序(按照输入顺序输出)

- 链表 + 链表 ？

HashSet 依赖 HashMap 实现

```java
public HashSet() {
map = new HashMap<>();
}
```


```java
    // Dummy value to associate with an Object in the backing Map
    private static final Object PRESENT = new Object();
    
    public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }
```

### TreeSet

java 底层实现 红黑树，不允许有重复元素。

### TreeMap

不允许有重复元素,后来的覆盖原有的。

基础属性

- private final Comparator<? super K> comparator;

- private transient Entry<K,V> root;

- private transient int size = 0; 
  
- private transient int modCount = 0;


```java
static final class Entry<K,V> implements Map.Entry<K,V> {
        K key;
        V value;
        Entry<K,V> left;
        Entry<K,V> right;
        Entry<K,V> parent;
        boolean color = BLACK;

        Entry(K key, V value, Entry<K,V> parent) {
            this.key = key;
            this.value = value;
            this.parent = parent;
        }

        public K getKey() {
            return key;
        }

        public V getValue() {
            return value;
        }

        public V setValue(V value) {
            V oldValue = this.value;
            this.value = value;
            return oldValue;
        }

        public boolean equals(Object o) {
            if (!(o instanceof Map.Entry))
                return false;
            Map.Entry<?,?> e = (Map.Entry<?,?>)o;

            return valEquals(key,e.getKey()) && valEquals(value,e.getValue());
        }

        public int hashCode() {
            int keyHash = (key==null ? 0 : key.hashCode());
            int valueHash = (value==null ? 0 : value.hashCode());
            return keyHash ^ valueHash;
        }

        public String toString() {
            return key + "=" + value;
        }
    }
```


插入元素后对红黑树进行调整 - fixAfterInsertion


TreeSet 调用 TreeMap
```java
    public TreeSet() {
        this(new TreeMap<E,Object>());
    }
```java

### HashMap & HashTable

HashMap JDK 1.2 开始， 线程不安全 ， 可以存 null

HashTable JDK 1.0 开始， 线程安全， 不可以存 null


Entry 类


hashcode 取模后，位置相同， 值相同，覆盖原有的值。

hashcode 取模后，位置相同， 值不相同，放到链表中。 链表的开头还是末尾，7上8下，jdk7头部，jdk8末尾。


JDK 1.7

```java
        HashMap<Integer, String> hashMap = new HashMap<>();
        hashMap.put(1, "one");
        hashMap.put(2, "two");
        hashMap.put(3, "three");
        hashMap.put(4, "four");
```

- HashMap的K,V的值，在创建对象的时候确定: K : Integer, V:String
- HashMap的父类 AbstractMap已经实现类Map接口，但是源码中又单独实现了Map接口， 多余操作 - 同时出现ArrayList 中。
```java
public class HashMap<K,V> extends AbstractMap<K,V>
implements Map<K,V>, Cloneable, Serializable {
}
```

重要属性

- static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16  初始数组长度

- static final int MAXIMUM_CAPACITY = 1 << 30; 数组最大长度

- static final float DEFAULT_LOAD_FACTOR = 0.75f;  负载因子

- transient int size; 元素个数。

- int threshold;   元组扩容阈值，不要超过的 capacity 才扩容，达到阈值就扩容。

- final float loadFactor;  负载因子， 接收  DEFAULT_LOAD_FACTOR


capacity 为 2的倍数。

#### put 方式


获取 hashCode   

对hashCode处理 
- 进行二次散列
- 扰动函数，增加值的不确定性。

取模，获取数组中的实际存储位置。

遍历 ？ 

每次遍历还要检查扩容问题。效率有点低。


#### 扩容 方式

每次put 元素都要检查 检查扩容问题。效率有点低。

装载因子 - 默认0.75

- 装载因子 = 1， 满了才扩容， 哈希冲突多了，对链表的遍历行为变多，复杂度变高。

- 装载因子 = 0， 每次都扩容， 哈希少，对链表的遍历行为少，但扩容需要复制数组，扩容代价高。

数组长度为 2 的次幂

- h & (length  - 1) 与 h % length 等效的前提是 - length 为 2 的次幂。


- 防止哈希冲突，位置冲突。 ？ （目前个人觉得这个理由不太合理）




### 泛型

![i++](https://isblog.oss-cn-guangzhou.aliyuncs.com/JVM/iplusplus.jpg)



