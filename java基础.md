# 1.super  和  this

![image-20200827224525500](.\typora-user-images\image-20200827224525500.png)

# 2.重写

> 有static方法

![image-20200827225707266](.\typora-user-images\image-20200827225707266.png)



> 普通方法

![image-20200827225501548](.\typora-user-images\image-20200827225501548.png)

![image-20200827225418086](.\typora-user-images\image-20200827225418086.png)

# 3.多态

![image-20200827230751536](.\typora-user-images\image-20200827230751536.png)

![image-20200828215432921](.\typora-user-images\image-20200828215432921.png)

# 4.抽象类

![image-20200828220849884](.\typora-user-images\image-20200828220849884.png)

# 5.接口

![image-20200828221807354](.\typora-user-images\image-20200828221807354.png)

# 6.集合

## 6.1概述

### （1）

![image-20200828223919312](.\typora-user-images\image-20200828223919312.png)





![image-20200903222206477](.\typora-user-images\image-20200903222206477.png)



### （2）Collection接口相关结构图



![image-20200903223610941](.\typora-user-images\image-20200903223610941.png)







![image-20200903224252139](.\typora-user-images\image-20200903224252139.png)



<img src=".\typora-user-images\image-20200903225607266.png" alt="image-20200903225607266" style="zoom: 67%;" />

![image-20200903225318436](.\typora-user-images\image-20200903225318436.png)

### （3）Map相关结构图

<img src=".\typora-user-images\image-20200903230235975.png" alt="image-20200903230235975" style="zoom:67%;" />

<img src=".\typora-user-images\image-20200903230756631.png" alt="image-20200903230756631" style="zoom:80%;" />

<img src=".\typora-user-images\image-20200903230827542.png" alt="image-20200903230827542" style="zoom: 80%;" />

### （4）小结

#### 1.Collection

![image-20200903231343101](.\typora-user-images\image-20200903231343101.png)

![image-20200903231357168](C:\Users\liuyixing\AppData\Roaming\Typora\typora-user-images\image-20200903231357168.png)

### 2.Map

![image-20200903231741011](.\typora-user-images\image-20200903231741011.png)

![image-20200903231845738](.\typora-user-images\image-20200903231845738.png)

**Map集合的key,就是一一个Set集合。**
**往Set集合中放数据，实际上放到了Map集合的key部分**。

## 6.2Collection接口

### 1.常用方法

```java
Collection c = new ArrayList();
c.add("liu");              //添加内容
c.clear();                 //清空集合
c.add("yi");               
boolean contains = c.contains("yi");//查看集合中是否包含某一元素
c.remove("yi");            //移除某一元素
 boolean empty = c.isEmpty();//判断集合是否为空 
Object[] objects = c.toArray();//转化为数组
        Iterator iterator = c.iterator();//迭代器
        while (iterator.hasNext()){    //判断集合中迭代器后面是否还有元素，并返回Boolen类型
            Object next = iterator.next();   //迭代器前进一位，并返回当前元素
            System.out.println(next);
        }
```

```java
        Collection a = new ArrayList();
        a.add(111);
        a.add(222);
        a.add(333);
        Iterator it1 = a.iterator();
        while (it1.hasNext()){
            Object n1 = it1.next();
            System.out.println(n1);
        }


        Collection c2 = new HashSet();
        c2.add(101);
        c2.add(103);
        c2.add(147);
        Iterator it2 = c2.iterator();
        while (it2.hasNext()){
            Object n2 = it2.next();
            System.out.println(n2);
        }
```

ArrayList与HashSet的区别

![image-20200904224321003](.\typora-user-images\image-20200904224321003.png)

> 调用 contains（）方法时 注意：

检查调用对象是否重写equals(),未重写时比较内存地址，重写后对比内容；

存放在集合中的类型，一定要进行重写equals()方法。

### 2.测试remove()方法

```java
        Collection o = new ArrayList();
        String s1 = new String("liu");
        o.add(s1);
        String s2= new String("liu");
        o.remove(s1);
        System.out.println(o.size());

```

> 当重写了equals()方法之后，remove()方法删除的是内容，所有指向当前内容的对象都被删除

### 3.迭代器相关约束

![image-20200906164939583](.\typora-user-images\image-20200906164939583.png)

> 改变结构时必须重新获取迭代器！！！

> 要删除集合元素可以直接调用迭代器的remove()方法删除当前迭代器所指向元素

**原因： 迭代器按照集合的快照进行迭代，若直接更改结构则快照与原集合不相同，产生错误，而调用迭代器的修改方法则集合和快照都会更新**

### 4.Collection子接口——List

#### 	1、List集合特点

​		有序可重复

> 有序：List集合中的元素有下标。从0开始，以1递增。

> 可重复:存储一个1 ,还可以再存储1.

#### 2、常用方法

```java
        List mylist =  new ArrayList();
        mylist.add("A");
        //默认都是向集合末尾添加元素。

        mylist.add("B");
        mylist.add("C");
        mylist.add("D");
        mylist.add("D");

        //在列表的指定位置插入指定元素(第一个参数是下标)
        //这个方法使用不多.因为对FArrayL ist集合来说效率比较低。
        mylist.add(2,"LIU");
        Iterator iterator = mylist.iterator();
        while (iterator.hasNext()){
            Object next = iterator.next();
            System.out.println(next);
        }

        //根据下标获取元素
        Object o = mylist.get(2);
        System.out.println(o);

        //List特殊遍历方式
        for (int i = 0; i < mylist.size(); i++) {
            Object o1 = mylist.get(i);
            System.out.println(o1);
        }

        // 获取指定对象第一次出现处的索引。
        System.out.println(mylist.indexOf("LIU"));
        //获取指定对象最后一次出现处的索引。
        System.out.println(mylist.lastIndexOf("D"));

        //删除指定下标位置的元素
        mylist.remove(4);
        System.out.println(mylist.size());
        System.out.println("================================");

        //修改指定位置的元素
        mylist.set(3,"YI");

        for (int i = 0; i < mylist.size(); i++) {
            Object o1 = mylist.get(i);
            System.out.println(o1);
        }
```

##### 1、ArryList集合

```java
        //默认初始值为10,数组长度为10
        List list = new ArrayList();

        //指定初始值20，数组长度为20
        List list2 = new ArrayList(20);

        Collection c = new HashSet();
        c.add(111);
        c.add(222);
        c.add(333);

        
        //通过这个构造方法就可以将HashSet集合转换成L ist集合。
        List mylist = new ArrayList(c);
        for (int i = 0; i < mylist.size(); i++) {
            System.out.println(mylist.get(i));
        }
```

#### 2、LinkedList



<img src=".\typora-user-images\image-20200909220442864.png" alt="image-20200909220442864" style="zoom:67%;" />



> ArrayList :把检索发挥到极致。
> L inkedlist :把随机增删发挥到极致。



> 将ArrayList转换成线程安全的

```java
        List mylist = new ArrayList();


        Collections.synchronizedList(mylist);//Collections为集合工具类
        mylist.add(111);
        mylist.add(222);
        mylist.add(333);
```

### 3、泛型

**当使用泛型时，所有存储的数据必须为指定类型**

**方便返回类型固定，无需强转**

> 泛型这种语法机制,只在程序编译阶段起作用，只是给编译器参考的。( 运行阶段泛型没用! )

> 泛型的缺点是什么?
>             导致集合中存储的元素缺乏多样性!

**调用子类型特有的方法还是需要向下转换的!!!!**

> 自定义泛型

```java
/*
自定义泛型可以吗?可以
自定义泛型的时候, <>尖括号中的是一个标识符,随便写。
java,源代码中经常出现的是: .
<E>和<T>
E是ELement单词首字母。
T是Type单词首字母。
*/

```

## 6.3Map接口

```java
/*
java.util.Map接口中常用的方法:
1、Map和collection没有继承关系。
2、Map集合以key积value的方式存储数据:键值对
key和value都是引用数据类型。
key和value都是存储对象的内存地址。
key起到主导的地位, value是key的一个附属品。
3、Map接口中常用方法:

V put(K key, V value) 向Map集合中添加键值对
V get(Object key)通过key获取value
void clear()清空Map集合
boolean containsKey(object key) 判断Map中是否包含某个key
boolean containsValue(object value) 判断Map中是否包含某个value
boolean isEmpty ( )判断Map 集合中元素个数是否为0
Set<K> keySet() 获取Map集合所有的key(所有的键是一个set集合)
V remove(Object key)通过key删除键值对
int size() 获取Map集合中键值对的个数。
Collection<V> values() 获取Map集合中所有的value ,返回一个Collection .
Set<Map. Entry<K, V>> entrySet()
将Map集合转换成set集合
假设现在有一个Map集合,如下所示:
map1集合对象
key                 value
1                 zhangsan
2                 lisi
3                 wangwu
4                 zhaoliu
Set set = map1. entrySet(); .
set集合对象
1 = zhangsan [注 意: Map集合通过entrySet()方法转换成的这个Set集合, Set集合中元素的类型是Map. Entry<K, V>]
2 = lisi     [Map. Entry和String一样，都是一种类型的名字，只不过: Map. Entry是静态内部类,是Map中的静态内部类]
3 = wangwu
4 = zhaoliu

 */
public class MapTest01 {
    public static void main(String[] args) {
        HashMap<Integer, String> map = new HashMap<>();
        map.put(1,"liu");
        map.put(2,"yi");
        map.put(3,"xing");

        Set<Integer> keys = map.keySet();

                   //第一种方式，效率较低，数据量小时可以使用
/* 1.       Iterator<Integer> it1 = keys.iterator();
        while (it1.hasNext()){
            Integer key = it1.next();
            String value = map.get(key);
            System.out.println(key+"="+value);


     2.   for (Integer key : keys){
            System.out.println(key+ "="+map.get(key));

        }
 */
        //第二种方式  效率高于第一种，适合大量数据
        Set<Map.Entry<Integer, String>> set = map.entrySet();
        Iterator<Map.Entry<Integer, String>> it2 = set.iterator();
        while (it2.hasNext()){
            Map.Entry<Integer, String> node = it2.next();
            System.out.println(node.getKey()+ "=" +node.getValue());
        }

        for (Map.Entry node : set){
            System.out.println(node.getKey()+" ====> "+node.getValue());
        }

    }

```

#### 6.3.1HashMap概述

HashMap集合: 

1、HashMap集合底层是哈希表/散列表的数据结构。 

2、哈希表是一个怎样的数据结构呢? 

哈希表是一个数组和单向链表的结合体。 

数组∶在查询方面效率很高，随机增删方面效率很低。 

单向链表:在随机增删方面效率较高，在查询方面效率很低。 

哈希表将以上的两种数据结构融合在一起，充分发挥它们各自的优点。

3、HashMap集合底层的源代码:

```java
public class HashMap {
    // HashMap底层实际上就是一个数组。(一维数组）
    Node<K,v>[] table;
     //静态的内部类HashMap.Node
    static class Node<K,V> {
         final int hash; //哈希值（哈希值是key的hashCode()方法的执行结果。hash值通过哈希函数/算法，可以转换成存储成数组的下标)
         final K key;    //存储到Map集合中的那个Key
         v value;        //存储到Map集合中的那个Value
         Node<K,V> next; //下一个节点的内存地址
         
         //哈希表/散列表:一维数组，这个数组中每一个元素是一个单向链表。（数组和链表的结合体。)
    }
}

```



#### 6.3.2HashMap数据结构

1.HashMap底层数据结构：
      一维数组与单向链表的结合

​		![image-20200915232108500](.\typora-user-images\image-20200915232108500.png)



> **map.put(k,v)实现原理∶**

```mark
  第一步:先将k,v封装到Node对象当中。
  第二步:
  	底层会调用k的hashCode0方法得出hash值，
  		然后通过哈希函数/哈希算法，将hash值转换成数组的下标，
  		下标位置上如果没有任何元素，
        	就把Node添加到这个位置上了。
        如果说下标对应的位置上有链表，
        	此时会拿着k和链表上每一个节点中的k进行equals，
  		如果所有的equals方法返回都是false，
  			那么这个新节点将会;被添加到链表的末尾。
  		如果其中有一个equals返回了true，
  			那么这个节点的value将会被覆盖。
```
> **map.get(k,v)实现原理∶**

```mark
v = map.get(k)实现原理∶
	先调用k的hashCode0方法得出哈希值，
	通过哈希算法转换成数组下标，通过数组下标快速定位到某个位置上，
	如果这个位置上什么也没有，
		返回null。
	如果这个位置上有单向链表，
		那么会拿着参数k和单向链表上的每个节点中的k进行equals，
		如果所有equals方法返回false，
			那么get方法返回null，
		只要其中有一个节点的k和参数k equals的时候返回true，
			那么此时这个节点的value就是我们要找的value ,
    get方法最终返回这个要找的value
```









![image-20200916211545592](.\typora-user-images\image-20200916211545592.png)

#### 6.3.3

> 扩容量为原容量2倍

>  为什么哈希表的随机增删，以及查询效率都很高?

增删是在链表上完成。
查询也不需要都扫描，只需要部分扫描。



> 为什么放在HashMap集合key部务的元素需要重写equals方法呢?

equals认比较的是两个对象的内存地址。
我们应该比较内容。



> HashMap集合的key部分特点:
> 无序，不可重复。

为什么无序?
	因为不一定挂到哪个单向链表上。
不可重复是怎么保证的? 
	equals方法来保证HashMap集合的key不可重复。
	如果key重复了，value会覆盖。

放在HashMap集合key部分的元素其实就是放到HashSet集合中了。
所以HashSet集合中的元素也需要同时重写hashCode()+equals()方法。

**注意:**

##### **1.同一个单向链表上**


所有节点的hash相同，
因为他们的数组下标是—样的。

##### **2.但同一个链表上k和k的equals方法**

肯定返回的是
false，都不相等。

##### **3.哈希表HashMap使用不当时无法发挥性能!**

假设将所有的hashCode()方法返回值固定为某个值，
那么会导致底层哈希表变成了纯单向链表。这种情况我们成为:散列分布不均匀。
什么是散列分布均匀?
	假设有100个元素，10个单向链表，那么每个单向链表上有10个节点，这是最好的，
	是散列分布均匀的。
假设将所有的hashCode()方法返回值都设定为不一样的值，可以吗，有什么问题?
	不行，因为这样的话导致底层哈希表就成为一维数组了，没有链表的概念了。
	也是散列分布不均匀。
	散列分布均匀需要你重马hashCode()方法时有一定的技巧。



##### **==4.重点:==**

**==放在HashMap集合key部分的元素，以及放在HashSet集合中的元素，==**

**==需要同时重写hashCode和equals方法。==**



##### 5.HashMap集合的默认初始化容量

是16，默认加栽因子是0.75
这个默认加载因子是当HashMap集合底层数组的容量达到75%的时候，数组开始扩容。

**重点，记住:HashMap集合初始化容量必须是2的倍数，这也是官方推荐的，
这是因为达到散列均匀，为了提高HashMap集合的存取效率，所必须的。**

**否则性能大打折扣**

##### 6.注意:

如果一个类的equals方法重写了，那么hashCode()方法必须重写。
并且equals方法返回如果是true , hashCode()方法返回的值必须一样。
equals方法返回true表示两个对象相同，在同一个单向链表上比较。
那么对于同一个单向链表上的节点来说，他们的哈希值都是相同的。
所以hashCode(）方法的返回值也应该相同。

##### 7.HashMap集合底层数据结构

HashMap集合底层是哈希表数据结构，是非线程安全的
在JDK8之后如果哈希表单向链表中元素超过8个，
	单向链表这种教据结构会变成红黑树数数据结构
 当红黑树上的节点数量小于6时，
	会重新把红黑树转换成单向链表数据结构。

这种方式也是为了提高检索
效率，二又树的检索会再次
缩小扫描范围。提高效率。

##### 8.对于哈希表数据结构来说:

##### 如果o1和o2的hash值相同，一定是放到同一个单向链表上。
当然如果o1和o2的hash值不同，但由于哈希算法执行结束之后转换的数组下标可能相同，此时会发生“哈希碰撞”。

##### 9.对于null的区别

HashMap来说，key和value值都可以为空，
但是在HashTable中，key和value值为空都会引发空指针异常

#### 6.3.4LinkedHashMap

1. 继承HashMap集合

2. 特点：

   1）底层是哈希表+链表（保证迭代顺序）

   2）它是一个有序集合，存储元素顺序与取出元素数据一致。



#### 6.3.4HashTable

初始化容量是11，默认加载因子为0.75

扩容量为原容量2倍+1

#### 6.3.5Properties

目前只需要掌握Properties属性类对象的相关方法即可。
Properties是一个Map集合，继承Hashtable ,Properties的ley和value都是string类型。
Properties被称为属性类对象。

### 6.3.6TreeMap/TreeMap集合

#### 1.TreeSet/TreeMap是自平衡二叉树。

​		遵循左小右大原则存放。
​		存放是要依靠左小右大原则，所以这个存放的时候要进行比较。

#### 2.遍历二叉树的时候有三种方式:

​		前序遍历∶根左右
​		中序遍历∶左根右
​		后序遍历: 左右根

##### 注意:

前中后说的是“根”的位置。
根在前面是前序，根在中间
是中序，根在后面是后序。

#### 3.TreeSet集合/TreeMap集合采用的是:中序遍历方式。

Iterator迭代器采用的是中序遍历方式。
左根右。

#### 4.结论


放到TreeSet或者TreeMap集合key部分的元素要想做到排序，包括两种方式:

第一种:放在集合中的元素实现java.Lang.Comparable接口。

第二种:在构造TreeSet或者TreeMap集合的时候给它传一个比较器对象。

Comparable利Comparator怎么选择呢?
如果比较规则有多个，并且需要多个比较规则之间频繁切换，建议使用Comparator接口。、

Comparator接口的设计符合ocP原则。
当比较规则不会发生改变的时候，或者说当比较规则只有1个的时候，建议实现Comparable接口。