# 总

### 1）常见的集合有哪些？

###### Map接口和Collection接口是所有集合框架的父接口

- Collection接口的子接口包括：Set接口和List接口
- Map接口的实现类主要有：HashMap、TreeMap、Hashtable、ConcurrentHashMap以及Properties等
- Set接口的实现类主要有：HashSet、TreeSet、LinkedHashSet等
- List接口的实现类主要有：ArrayList、LinkedList、Stack以及Vector等

1. ArrayList底层是数组。

2. LinkedList底层是双向链表。

3. HashMap底层与HashTable原理相同，Java 8版本以后如果同一位置哈希冲突大于8则链表变成红黑树。

4. HashTable底层是链地址法组成的哈希表（即数组+单项链表组成）。

5. HashSet底层是HashMap。

6. LinkedHashMap底层修改自HashMap，包含一个维护插入顺序的双向链表。

7. TreeMap底层是红黑树。

8. LinkedHashSet底层是LinkedHashMap。

9. TreeSet底层是TreeMap。

   ### 2) HashMap与HashTable的区别？

   > 1. HashMap没有考虑同步，是线程不安全的；Hashtable使用了synchronized关键字，是线程安全的；
   > 2. HashMap允许K/V都为null；后者K/V都不允许为null；

   ### 3) ConcurrentHashMap和Hashtable的区别？

   > ConcurrentHashMap 结合了 HashMap 和 HashTable 二者的优势。HashMap 没有考虑同步，HashTable 考虑了同步的问题。但是 HashTable 在每次同步执行时都要锁住整个结构。 ConcurrentHashMap 锁的方式是稍微细粒度的。

   ### 4)ConcurrentHashMap实现原理

   - JDK1.7 : 【数组（Segment） + 数组（HashEntry） + 链表（HashEntry节点）】

   > - ConcurrentHashMap（分段锁） 对整个桶数组进行了分割分段(Segment)，每一把锁只锁容器其中一部分数据，多线程访问容器里不同数据段的数据，就不会存在锁竞争，提高并发访问率。
   > - Segment是一种可重入锁ReentrantLock，在ConcurrentHashMap里扮演锁的角色，HashEntry则用于存储键值对数据。

   - JDK1.8 : Node数组+链表 / 红黑树

   > - 利用CAS+Synchronized来保证并发更新的安全，底层依然采用数组+链表+红黑树的存储结构。

   ### 5) ArrayList 和 Vector 的区别？

   > - Vector 是线程安全的,ArrayList 是线程不安全的。
   > - Vector在数据满时增长为原来的两倍，而 ArrayList在数据量达到容量的一半时,增长为原容量的1.5倍。

   ### 6) ArrayList和LinkedList的区别？

   > - LinkedList基于链表的数据结构；ArrayList基于动态数组的数据结构
   > - LinkedList 在插入和删除数据时效率更高，ArrayList 查询效率更高；

   ### 7) HashMap 默认的初始化长度是多少？

   - 在JDK中默认长度是16，并且默认长度和扩容后的长度都必须是 2 的幂。

   ### 8) HashMap 构造方法中初始容量、加载因子的理解

   - 初始容量代表了哈希表中桶的初始数量，即 Entry< K,V>[] table 数组的初始长度
   - 加载因子是哈希表在其容量自动增加之前可以达到多满的一种饱和度百分比，其衡量了一个散列表的空间的使用程度，负载因子越大表示散列表的装填程度越高，反之愈小。

## 一、Map

- ### 说一下 HashMap 的实现原理？

- HashMap概述： HashMap是基于哈希表的Map接口的非同步实现。此实现提供所有可选的映射操作，并允许使用null值和null键。此类不保证映射的顺序，特别是它不保证该顺序恒久不变。

- HashMap的数据结构： 在Java编程语言中，最基本的结构就是两种，一个是数组，另外一个是模拟指针（引用），所有的数据结构都可以用这两个基本结构来构造的，HashMap也不例外。HashMap实际上是一个“链表散列”的数据结构，即数组和链表的结合体。

- HashMap 基于 Hash 算法实现的

- 1. 当我们往Hashmap中put元素时，利用key的hashCode重新hash计算出当前对象的元素在数组中的下标
  2. 存储时，如果出现hash值相同的key，此时有两种情况。(1)如果key相同，则覆盖原始值；(2)如果key不同（出现冲突），则将当前的key-value放入链表中
  3. 获取时，直接找到hash值对应的下标，在进一步判断key是否相同，从而找到对应值。
  4. 理解了以上过程就不难明白HashMap是如何解决hash冲突的问题，核心就是使用了数组的存储方式，然后将冲突的key的对象放入链表中，一旦发现冲突就在链表中做进一步的对比。

- 需要注意Jdk 1.8中对HashMap的实现做了优化，当链表中的节点数据超过八个之后，该链表会转为红黑树来提高查询效率，从原来的O(n)到O(logn)

- ### HashMap在JDK1.7和JDK1.8中有哪些不同？HashMap的底层实现

- 在Java中，保存数据有两种比较简单的数据结构：数组和链表。**数组的特点是：寻址容易，插入和删除困难；链表的特点是：寻址困难，但插入和删除容易；所以我们将数组和链表结合在一起，发挥两者各自的优势，使用一种叫做拉链法**的方式可以解决哈希冲突。

- #### JDK1.8之前

- JDK1.8之前采用的是拉链法。**拉链法**：将链表和数组相结合。也就是说创建一个链表数组，数组中每一格就是一个链表。若遇到哈希冲突，则将冲突的值加到链表中即可。

- ![1596944376083](C:\Users\AnQi\AppData\Roaming\Typora\typora-user-images\1596944376083.png)

- #### JDK1.8之后

- 相比于之前的版本，jdk1.8在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间。

- ![1596944388584](C:\Users\AnQi\AppData\Roaming\Typora\typora-user-images\1596944388584.png)

- #### JDK1.7 VS JDK1.8 比较

- JDK1.8主要解决或优化了一下问题：

- 1. resize 扩容优化

  2. 引入了红黑树，目的是避免单条链表过长而影响查询效率，红黑树算法请参考

  3. 解决了多线程死循环问题，但仍是非线程安全的，多线程时可能会造成数据丢失问题。

     ![1596944412616](C:\Users\AnQi\AppData\Roaming\Typora\typora-user-images\1596944412616.png)

     ### HashMap的put方法的具体流程？

     当我们put的时候，首先计算 `key`的`hash`值，这里调用了 `hash`方法，`hash`方法实际是让`key.hashCode()`与`key.hashCode()>>>16`进行异或操作，高16bit补0，一个数和0异或不变，所以 hash 函数大概的作用就是：**高16bit不变，低16bit和高16bit做了一个异或，目的是减少碰撞**。按照函数注释，因为bucket数组大小是2的幂，计算下标`index = (table.length - 1) & hash`，如果不做 hash 处理，相当于散列生效的只有几个低 bit 位，为了减少散列的碰撞，设计者综合考虑了速度、作用、质量之后，使用高16bit和低16bit异或来简单处理减少碰撞，而且JDK8中用了复杂度 O（logn）的树结构来提升碰撞下的性能。

     putVal方法执行流程图

     ![1596944434826](C:\Users\AnQi\AppData\Roaming\Typora\typora-user-images\1596944434826.png)

     ```java
      public V put(K key, V value) {
         return putVal(hash(key), key, value, false, true);
     }
     
     static final int hash(Object key) {
         int h;
         return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
     }
     
     //实现Map.put和相关方法
     final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                        boolean evict) {
         Node<K,V>[] tab; Node<K,V> p; int n, i;
         // 步骤①：tab为空则创建 
         // table未初始化或者长度为0，进行扩容
         if ((tab = table) == null || (n = tab.length) == 0)
             n = (tab = resize()).length;
         // 步骤②：计算index，并对null做处理  
         // (n - 1) & hash 确定元素存放在哪个桶中，桶为空，新生成结点放入桶中(此时，这个结点是放在数组中)
         if ((p = tab[i = (n - 1) & hash]) == null)
             tab[i] = newNode(hash, key, value, null);
         // 桶中已经存在元素
         else {
             Node<K,V> e; K k;
             // 步骤③：节点key存在，直接覆盖value 
             // 比较桶中第一个元素(数组中的结点)的hash值相等，key相等
             if (p.hash == hash &&
                 ((k = p.key) == key || (key != null && key.equals(k))))
                     // 将第一个元素赋值给e，用e来记录
                     e = p;
             // 步骤④：判断该链为红黑树 
             // hash值不相等，即key不相等；为红黑树结点
             // 如果当前元素类型为TreeNode，表示为红黑树，putTreeVal返回待存放的node, e可能为null
             else if (p instanceof TreeNode)
                 // 放入树中
                 e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
             // 步骤⑤：该链为链表 
             // 为链表结点
             else {
                 // 在链表最末插入结点
                 for (int binCount = 0; ; ++binCount) {
                     // 到达链表的尾部
                //判断该链表尾部指针是不是空的
                 if ((e = p.next) == null) {
                     // 在尾部插入新结点
                     p.next = newNode(hash, key, value, null);
                     //判断链表的长度是否达到转化红黑树的临界值，临界值为8
                     if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                         //链表结构转树形结构
                         treeifyBin(tab, hash);
                     // 跳出循环
                     break;
                 }
                 // 判断链表中结点的key值与插入的元素的key值是否相等
                 if (e.hash == hash &&
                     ((k = e.key) == key || (key != null && key.equals(k))))
                     // 相等，跳出循环
                     break;
                 // 用于遍历桶中的链表，与前面的e = p.next组合，可以遍历链表
                 p = e;
             }
         }
         //判断当前的key已经存在的情况下，再来一个相同的hash值、key值时，返回新来的value这个值
         if (e != null) { 
             // 记录e的value
             V oldValue = e.value;
             // onlyIfAbsent为false或者旧值为null
             if (!onlyIfAbsent || oldValue == null)
                 //用新值替换旧值
                 e.value = value;
             // 访问后回调
             afterNodeAccess(e);
             // 返回旧值
             return oldValue;
         }
     }
     // 结构性修改
     ++modCount;
     // 步骤⑥：超过最大容量就扩容 
     // 实际大小大于阈值则扩容
     if (++size > threshold)
         resize();
     // 插入后回调
     afterNodeInsertion(evict);
     return null;
     } 
     ```
     

①.判断键值对数组table[i]是否为空或为null，否则执行resize()进行扩容；

②.根据键值key计算hash值得到插入的数组索引i，如果table[i]==null，直接新建节点添加，转向⑥，如果table[i]不为空，转向③；

③.判断table[i]的首个元素是否和key一样，如果相同直接覆盖value，否则转向④，这里的相同指的是hashCode以及equals；

④.判断table[i] 是否为treeNode，即table[i] 是否是红黑树，如果是红黑树，则直接在树中插入键值对，否则转向⑤；

⑤.遍历table[i]，判断链表长度是否大于8，大于8的话把链表转换为红黑树，在红黑树中执行插入操作，否则进行链表的插入操作；遍历过程中若发现key已经存在直接覆盖value即可；

⑥.插入成功后，判断实际存在的键值对数量size是否超多了最大容量threshold，如果超过，进行扩容。

### HashMap的扩容操作是怎么实现的？

①.在jdk1.8中，resize方法是在hashmap中的键值对大于阀值时或者初始化时，就调用resize方法进行扩容；

②.每次扩展的时候，都是扩展2倍；

③.扩展后Node对象的位置要么在原位置，要么移动到原偏移量两倍的位置。

在putVal()中，我们看到在这个函数里面使用到了2次resize()方法，resize()方法表示的在进行第一次初始化时会对其进行扩容，或者当该数组的实际大小大于其临界值值(第一次为12),这个时候在扩容的同时也会伴随的桶上面的元素进行重新分发，这也是JDK1.8版本的一个优化的地方，在1.7中，扩容之后需要重新去计算其Hash值，根据Hash值对其进行分发，但在1.8版本中，则是根据在同一个桶的位置中进行判断(e.hash & oldCap)是否为0，重新进行hash分配后，该元素的位置要么停留在原始位置，要么移动到原始位置+增加的数组大小这个位置上

```java
final Node<K,V>[] resize() {
    Node<K,V>[] oldTab = table;//oldTab指向hash桶数组
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    int oldThr = threshold;
    int newCap, newThr = 0;
    if (oldCap > 0) {//如果oldCap不为空的话，就是hash桶数组不为空
        if (oldCap >= MAXIMUM_CAPACITY) {//如果大于最大容量了，就赋值为整数最大的阀值
            threshold = Integer.MAX_VALUE;
            return oldTab;//返回
        }//如果当前hash桶数组的长度在扩容后仍然小于最大容量 并且oldCap大于默认值16
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
            newThr = oldThr << 1; // double threshold 双倍扩容阀值threshold
    }
    // 旧的容量为0，但threshold大于零，代表有参构造有cap传入，threshold已经被初始化成最小2的n次幂
    // 直接将该值赋给新的容量
    else if (oldThr > 0) // initial capacity was placed in threshold
        newCap = oldThr;
    // 无参构造创建的map，给出默认容量和threshold 16, 16*0.75
    else {               // zero initial threshold signifies using defaults
        newCap = DEFAULT_INITIAL_CAPACITY;
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    // 新的threshold = 新的cap * 0.75
    if (newThr == 0) {
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
    }
    threshold = newThr;
    // 计算出新的数组长度后赋给当前成员变量table
    @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];//新建hash桶数组
    table = newTab;//将新数组的值复制给旧的hash桶数组
    // 如果原先的数组没有初始化，那么resize的初始化工作到此结束，否则进入扩容元素重排逻辑，使其均匀的分散
    if (oldTab != null) {
        // 遍历新数组的所有桶下标
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
            if ((e = oldTab[j]) != null) {
                // 旧数组的桶下标赋给临时变量e，并且解除旧数组中的引用，否则就数组无法被GC回收
                oldTab[j] = null;
                // 如果e.next==null，代表桶中就一个元素，不存在链表或者红黑树
                if (e.next == null)
                    // 用同样的hash映射算法把该元素加入新的数组
                    newTab[e.hash & (newCap - 1)] = e;
                // 如果e是TreeNode并且e.next!=null，那么处理树中元素的重排
                else if (e instanceof TreeNode)
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                // e是链表的头并且e.next!=null，那么处理链表中元素重排
                else { // preserve order
                    // loHead,loTail 代表扩容后不用变换下标，见注1
                    Node<K,V> loHead = null, loTail = null;
                    // hiHead,hiTail 代表扩容后变换下标，见注1
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    // 遍历链表
                    do {             
                        next = e.next;
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                // 初始化head指向链表当前元素e，e不一定是链表的第一个元素，初始化后loHead
                                // 代表下标保持不变的链表的头元素
                                loHead = e;
                            else                                
                                // loTail.next指向当前e
                                loTail.next = e;
                            // loTail指向当前的元素e
                            // 初始化后，loTail和loHead指向相同的内存，所以当loTail.next指向下一个元素时，
                            // 底层数组中的元素的next引用也相应发生变化，造成lowHead.next.next.....
                            // 跟随loTail同步，使得lowHead可以链接到所有属于该链表的元素。
                            loTail = e;                           
                        }
                        else {
                            if (hiTail == null)
                                // 初始化head指向链表当前元素e, 初始化后hiHead代表下标更改的链表头元素
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    // 遍历结束, 将tail指向null，并把链表头放入新数组的相应下标，形成新的映射。
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```

### HashMap是怎么解决哈希冲突的？

答：在解决这个问题之前，我们首先需要知道**什么是哈希冲突**，而在了解哈希冲突之前我们还要知道**什么是哈希**才行；

#### 什么是哈希？

**Hash，一般翻译为“散列”，也有直接音译为“哈希”的，这就是把任意长度的输入通过散列算法，变换成固定长度的输出，该输出就是散列值（哈希值）**；这种转换是一种压缩映射，也就是，散列值的空间通常远小于输入的空间，不同的输入可能会散列成相同的输出，所以不可能从散列值来唯一的确定输入值。**简单的说就是一种将任意长度的消息压缩到某一固定长度的消息摘要的函数**。

所有散列函数都有如下一个基本特性**：根据同一散列函数计算出的散列值如果不同，那么输入值肯定也不同。但是，根据同一散列函数计算出的散列值如果相同，输入值不一定相同**。

#### 什么是哈希冲突？

**当两个不同的输入值，根据同一散列函数计算出相同的散列值的现象，我们就把它叫做碰撞（哈希碰撞）**。

#### HashMap的数据结构

在Java中，保存数据有两种比较简单的数据结构：数组和链表。**数组的特点是：寻址容易，插入和删除困难；链表的特点是：寻址困难，但插入和删除容易**；所以我们将数组和链表结合在一起，发挥两者各自的优势，使用一种叫做**链地址法**的方式可以解决哈希冲突：

![1596944650304](C:\Users\AnQi\AppData\Roaming\Typora\typora-user-images\1596944650304.png)

这样我们就可以将拥有相同哈希值的对象组织成一个链表放在hash值所对应的bucket下，**但相比于hashCode返回的int类型，我们HashMap初始的容量大小DEFAULT_INITIAL_CAPACITY = 1 << 4（即2的四次方16）要远小于int类型的范围，所以我们如果只是单纯的用hashCode取余来获取对应的bucket这将会大大增加哈希碰撞的概率，并且最坏情况下还会将HashMap变成一个单链表**，所以我们还需要对hashCode作一定的优化

#### hash()函数

上面提到的问题，主要是因为如果使用hashCode取余，那么相当于**参与运算的只有hashCode的低位**，高位是没有起到任何作用的，所以我们的思路就是让hashCode取值出的高位也参与运算，进一步降低hash碰撞的概率，使得数据分布更平均，我们把这样的操作称为**扰动**，在**JDK 1.8**中的hash()函数如下：

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);// 与自己右移16位进行异或运算（高低位异或）
}

```

这比在**JDK 1.7**中，更为简洁，**相比在1.7中的4次位运算，5次异或运算（9次扰动），在1.8中，只进行了1次位运算和1次异或运算（2次扰动）**；

#### JDK1.8新增红黑树 .........

### 1.HashMap

#### （1）LinkedHashMap

### 2.SortedMap

#### （1）TreeMap

#### （2）NavigableMap

### 3.Hashtable

#### （1）Properties

## 二、Collection

### 1.List

#### (1) ArrayList

#### (2) LinkedList

#### (3) Vector

### 2.Set

#### (1) HashSet

#### (2) SortedSet

#### (3) TreeSet

#### (4) KeySet

## 三、Iterator

### 1.ListIterator

## 四、Collections

## 五、Collections

## 六、Enumeration
###  JavaSE 高级 之 集合

- 请画出集合框架的继承体系图
- 说说List、Set、Map三者的区别？
- ArrayList和LinkedList的区别？
- 你知道RandomAccess接口吗？他的作用是什么？
- 你了解双向链表和双向循环链表吗？
- 集合的安全性问题？
  - 请问ArrayList、HashSet、HashMap是线程安全的吗？如果不是线程安全的集合怎么办？
- ArrayList底层怎么实现的？
  - 在什么样的场景下使用ArrayList
  - 它是线程安全的吗？为什么？
  - 如果我想使用线程安全的ArrayList，应该怎么做？
  - ArrayList的常用方法有哪些？
  - ArrayList的扩容机制是怎样的？
- 知道ArrayList和Vector的区别吗？为什么要用ArrayList取代Vector呢？
- HashMap和Hashtable的区别？
  - 知道HashMap的扩容机制吗？
  - 你知道HashMap的底层数据结构吗？不同版本的数据结构知道吗？
  - 为什么要用这样的结构实现？
  - 为什么HashMap总是使用2的幂作为哈希表的大小？
  - 知道扰动函数吗？作用是什么？
  - HashMap的长度为什么是2的幂次方？
- HashSet如何检查重复？
  - HashSet的底层原理是什么？
- HashMap多线程操作死循环问题？
- ConcurrentHashMap和Hashtable的区别
  - ConcurrentHashMap是否有升级？如果有，你知道升级的原理吗？
  - ConcurrentHashMap底层如何实现的？请结合自己画图分析
- comparable和Comparator的区别
- Comparator定制重排序？请给出排序示例代码
  - compareTo方法
- 请写出集合框架底层数据结构总结以及其操作的特点
  - List
    - ArrayList
    - Vector
    - LinkedList
  - Set
    - HashSet
    - LinkedHashSet
    - TreeSet
  - Map
    - HashMap
    - LinkedHash Map
    - Hashtable
    - TreeMap
- 如何选用集合
- 除了这些集合其他的集合了解吗？
- 并发集合和普通集合了解吗？
- 数组和链表使用的常见了解吗？
- Map中的key和value可以为null吗？
- Collections的常用API方法