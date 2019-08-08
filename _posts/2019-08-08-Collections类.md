---
layout:     post
title:      Collections类和Arrays类
subtitle:   
date:       201-08-08
author:     ctrlcoder
header-img: 
catalog: true
tags:
    - Java
typora-root-url: ..
---

> 都是包含静态方法的工具类

## Collections类

### 排序操作

- void reverse(List list)：反转

- void shuffle(List list)：随机排序

- void swap(List list, int i , int j))：交换两个索引位置的元素

- void rotate(List list, int distance))：旋转。当distance为正数时，将list后distance个元素整体移到前面。当distance为负数时，将 list的前distance个元素整体移到后面。

```java
public static <T extends Comparable<? super T>> void sort(List<T> list)
 
public static <T> void sort(List<T> list,Comparator<? super T> c)
```

使用`List`时想根据`List`中存储对象的某一字段进行排序，那么我们要用到`Collections.sort`方法对`list`排序，用`Collections.sort`方法对`list`排序有两种方法：

- 第一种是`list`中的对象实现`Comparable`接口；
- 第二种方法是根据`Collections.sort`重载方法来实现。

```java
public class SortTest {
    public static void main(String[] args) {
        List<String> listS = new ArrayList<String>();
        List<Employer1> list1 = new ArrayList<Employer1>();
        List<Employer2> list2 = new ArrayList<Employer2>();
        List<Employer3> list3 = new ArrayList<Employer3>();
        
        //一.将String类型的变量插入到listS中并排序
        //listS中的对象String 本身含有compareTo方法，所以可以直接调用sort方法，按自然顺序排序，即升序排序
        listS.add("5");
        listS.add("2");
        listS.add("9");
        Collections.sort(listS);
        
        //二.将Employer1类的对象插入到list1中并排序
        //将已创建的实现了Comparator接口的比较类MyCompare传入Collections的sort方法中即可实现依照MyCompare类中的比较规则。
        Employer1 a1 = new Employer1();
        Employer1 b1 = new Employer1();
        Employer1 c1 = new Employer1();
        a1.setName("a1");   a1.setAge(44);
        b1.setName("b1");   b1.setAge(55);
        c1.setName("b1");   c1.setAge(33);
        list1.add(a1);
        list1.add(b1);
        list1.add(c1);
        //Collections类的sort方法要求传入的第二个参数是一个已实现Comparator接口的比较器
        Collections.sort(list1, new MyCompare());

        //三.将Employer2类的对象插入到list2中并排序
        //其实原理和上面的二类似，只是没有单独创建MyCompare类，而是用匿名内部类来实现Comparator接口里面的具体比较。
        Employer2 a2 = new Employer2();
        Employer2 b2 = new Employer2();
        Employer2 c2 = new Employer2();
        a2.setName("a2");   a2.setAge(66);
        b2.setName("b2");   b2.setAge(33);
        c2.setName("b2");   c2.setAge(22);
        list2.add(a2);
        list2.add(b2);
        list2.add(c2); 
        //Collections类的sort方法要求传入的第二个参数是一个已实现Comparator接口的比较器
        Collections.sort(list2,new Comparator<Employer2>(){
            @Override
            public int compare(Employer2 a2, Employer2 b2) {
                return a2.getOrder().compareTo(b2.getOrder());
            }

        });

        //四.将Employer3类的对象插入到list3中并排序
        //被排序的类Employer3实现了Comparable接口,在类Employer3中通过重载compareTo方法来实现具体的比较。
        Employer3 a3 = new Employer3();
        Employer3 b3 = new Employer3();
        Employer3 c3 = new Employer3();
        a3.setName("a3");   a3.setAge(77);
        b3.setName("b3");   b3.setAge(55);
        c3.setName("b3");   c3.setAge(99);
        list3.add(a3);
        list3.add(b3);
        list3.add(c3);
        Collections.sort(list3);
        //Collections类的sort方法要求传入的List中的对象是已实现Comparable接口的对象

        System.out.println(listS);
        System.out.println(list1);
        System.out.println(list3);
        System.out.println(list2);
    }
}
class Employer1{
    private String name;
    private Integer age;
    public void setName(String name) {
        this.name = name;
    }
    public Integer getAge() {
        return age;
    }
    public void setAge(Integer age) {
        this.age = age;
    }
    @Override//重载了Object类里的toString方法，使之可以按照我们要求的格式打印
    public String toString() {
        return "name is "+name+" age is "+ age;
    }
}
class MyCompare implements Comparator<Employer1> {
    @Override//重载了Comparator接口里面的compare方法实现具体的比较
    public int compare(Employer1 o1, Employer1 o2) {
        return o1.getAge().compareTo(o2.getAge());
    }
}
class Employer2{
    private String name;
    private Integer age;
    public void setName(String name) {
        this.name = name;
    }
    public Integer getOrder() {
        return age;
    }
    public void setAge(Integer age) {
        this.age = age;
    }
    @Override//重载了Object类里的toString方法，使之可以按照我们要求的格式打印
    public String toString() {
        return "name is "+name+" age is "+age;
    }
}
class Employer3 implements Comparable<Employer3>{
    private String name;
    private Integer age;
    public void setName(String name) {
        this.name = name;
    }
    public Integer getAge() {
        return age;
    }
    public void setAge(Integer age) {
        this.age = age;
    }
    @Override//重载了Object类里的toString方法，使之可以按照我们要求的格式打印
    public String toString() {
        return "name is "+name+" age is "+age;
    }
    @Override//重载了Comparable接口里的compareTo方法来实现具体的比较
    public int compareTo(Employer3 a) {
        return this.age.compareTo(a.getAge());
    }
}
```

#### compareble接口

`Arrays.sort()`方法可对任何实现`compareble`接口的对象数组排序, 像`Integer`,`String`,这两种引用类型都实现了`compareble`接口，所以这两种类型的数组都可直接使用`Arrays.sort()`进行排序

> 该接口对实现它的每个类的对象强加一个整体排序。这个排序被称为类的*自然排序*  ，类的`compareTo`方法被称为其*自然比较方法* 。

#### compareTo()

由上面的程序我们可以看出，无论是实现了`Comparable`接口的方法还是实现了`Comparato`r接口的方法，最终比较的返回值都是通过`compareTo`方法实现的，故就把`compareTo`方法单独拿出来做个小结。

　　compareTo()的返回值是整型，它是先比较对应字符的大小(ASCII码顺序)，如果第一个字符和参数的第一个字符不等，结束比较，返回他们之间的差值，如果第一个字符和参数的第一个字符相等，则以第二个字符和参数的第二个字符做比较，以此类推，直至比较的字符或被比较的字符有一方全比较完，这时就比较字符的长度。例如：

```java
String s1 = "abc"; 
String s2 = "abcd"; 
String s3 = "abcdfg"; 
String s4 = "1bcdfg"; 
String s5 = "cdfg"; 
System.out.println( s1.compareTo(s2) ); // -1 (前面相等,s1长度小1) 
System.out.println( s1.compareTo(s3) ); // -3 (前面相等,s1长度小3) 
System.out.println( s1.compareTo(s4) ); // 48 ("a"的ASCII码是97,"1"的的ASCII码是49,所以返回48) 
System.out.println( s1.compareTo(s5) ); // -2 ("a"的ASCII码是97,"c"的ASCII码是99,所以返回-2)
```

### 查找，替换操作

- int binarySearch(List list, Object key)：对List进行二分查找，返回索引，注意List必须是有序的

- int max(Collection coll)：根据元素的自然顺序，返回最大的元素。 类比int min(Collection coll)

- int max(Collection coll, Comparator c)：根据定制排序，返回最大元素，排序规则由Comparatator类控制。类比int min(Collection coll, Comparator c)

- void fill(List list, Object obj)：用元素obj填充list中所有元素

- int frequency(Collection c, Object o)：统计元素出现次数

- int indexOfSubList(List list, List target)：统计targe在list中第一次出现的索引，找不到则返回-1，类比int lastIndexOfSubList(List source, list target).

- boolean replaceAll(List list, Object oldVal, Object newVal)：用新元素替换旧元素。

### 同步控制

Collections中几乎对每个集合都定义了同步控制方法，例如 SynchronizedList(), SynchronizedSet()等方法，来将集合包装成线程安全的集合。

```java
List list = Collections. synchronizedList(new ArrayList()); 
Set set = Collections. synchronizedSet(new HashSet()); 
Map map = Collections. synchronizedMap(new HashMap()); 
```



## Arrays类

> 该类包含用于操作数组的各种方法（如排序和搜索）。 该类还包含一个静态工厂，可以将数组视为`List`。

**asList(T... a)**

由给定的数组a，返回一个固定大小的List对象。在这里，着重解释一下前面这句话的深层含义，我们可以看Arrays类的源码，来帮助我们理解。