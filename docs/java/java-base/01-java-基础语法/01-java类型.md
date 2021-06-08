## Integer

包装类型
Java语言是一个面向对象的语言，但是Java中的基本数据类型却是不面向对象的，这在实际使用时存在很多的不便，为了解决这个不足，在设计类时为每个基本数据类型设计了一个对应的类进行代表，这样八个和基本数据类型对应的类统称为包装类(Wrapper Class)。

包装类均位于java.lang包，包装类和基本数据类型的对应关系如下表所示

![img](https://gitee.com/ZXiangC/picture/raw/master/img/1160961-20190505211120867-229490299.png)

在这八个类名中，除了Integer和Character类以后，其它六个类的类名和基本数据类型一致，只是类名的第一个字母大写即可。

### 1、为什么需要包装类
​      很多人会有疑问，既然Java中为了提高效率，提供了八种基本数据类型，为什么还要提供包装类呢？

这个问题，其实前面已经有了答案，因为Java是一种面向对象语言，很多地方都需要使用对象而不是基本数据类型。比如，在集合类中，我们是无法将int 、double等类型放进去的。因为集合的容器要求元素是Object类型。

![image-20210608110400390](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210608110400390.png)

​    为了让基本类型也具有对象的特征，就出现了包装类型，它相当于将基本类型“包装起来”，使得它具有了对象的性质，并且为其添加了属性和方法，丰富了基本类型的操作。

### 2、自动拆箱与自动装箱

- 自动装箱: 就是将基本数据类型自动转换成对应的包装类。

- 自动拆箱：就是将包装类自动转换成对应的基本数据类型。 

```java
Integer i =10; //自动装箱
int b= i;   //自动拆箱
Integer i=10 可以替代 Integer i = new Integer(10);，这就是因为Java帮我们提供了自动装箱的功能，不需要开发者手动去new一个Integer对象。
```

### 3、自动装箱与自动拆箱的实现原理

既然Java提供了自动拆装箱的能力，那么，我们就来看一下，到底是什么原理，Java是如何实现的自动拆装箱功能。

我们有以下自动拆装箱的代码：

```java
public static void main(String[]args){
  Integer integer=1; //装箱
  int i=integer; //拆箱
}
```

对以上代码进行反编译后可以得到以下代码：

```java
public static void main(String[]args){
  Integer integer=Integer.valueOf(1); 
  int i=integer.intValue(); 
}
```

从上面反编译后的代码可以看出，int的自动装箱都是通过Integer.valueOf()方法来实现的，Integer的自动拆箱都是通过integer.intValue来实现的。如果读者感兴趣，可以试着将八种类型都反编译一遍 ，你会发现以下规律：

- 自动装箱都是通过包装类的valueOf()方法来实现的。

- 自动拆箱都是通过包装类对象的xxxValue()来实现的。

### 4、哪些地方会自动拆装箱

我们了解过原理之后，在来看一下，什么情况下，Java会帮我们进行自动拆装箱。前面提到的变量的初始化和赋值的场景就不介绍了，那是最简单的也最容易理解的。

我们主要来看一下，那些可能被忽略的场景。

#### 场景一、将基本数据类型放入集合类
我们知道，Java中的集合类只能接收对象类型，那么以下代码为什么会不报错呢？

```java
List<Integer> li = new ArrayList<>();
for (int i = 1; i < 50; i ++){
  li.add(i);
}
```

将上面代码进行反编译，可以得到以下代码：

```java
List<Integer> li = new ArrayList<>();
for (int i = 1; i < 50; i += 2){
  li.add(Integer.valueOf(i));
}
```

以上，我们可以得出结论，当我们把基本数据类型放入集合类中的时候，会进行自动装箱。

#### 场景二、包装类型和基本类型比较

有没有人想过，当我们对Integer对象与基本类型进行大小比较的时候，实际上比较的是什么内容呢？看以下代码：

```java
 public static void main(String[] args) {
        Integer a=1;
        System.out.println(a==1?"等于":"不等于");// 等于
        Boolean bool=false;
        System.out.println(bool?"真":"假"); // 假
    }
```

对以上代码进行反编译，得到以下代码：

```java
Integer a=1;
System.out.println(a.intValue()==1?"等于":"不等于");
Boolean bool=false;
System.out.println(bool.booleanValue?"真":"假");
```

可以看到，包装类与基本数据类型进行比较运算，是先将包装类进行拆箱成基本数据类型，然后进行比较的。

#### 场景三、包装类型的运算

有没有人想过，当我们对Integer对象进行四则运算的时候，是如何进行的呢？看以下代码：

```java
Integer i = 10;
Integer j = 20;
System.out.println(i+j);
```

反编译后代码如下：

```java
Integer i = Integer.valueOf(10);
Integer j = Integer.valueOf(20);
System.out.println(i.intValue() + j.intValue());
```

我们发现，两个包装类型之间的运算，会被自动拆箱成基本类型进行。

#### 场景四、三目运算符的使用

这是很多人不知道的一个场景，作者也是一次线上的血淋淋的Bug发生后才了解到的一种案例。看一个简单的三目运算符的代码：

```java
boolean flag = true;
Integer i = 0;
int j = 1;
int k = flag ? i : j;
```

很多人不知道，其实在int k = flag ? i : j;这一行，会发生自动拆箱。反编译后代码如下：

```java
boolean flag = true;
Integer i = Integer.valueOf(0);
int j = 1;
int k = flag ? i.intValue() : j;
```

这其实是三目运算符的语法规范：当第二，第三位操作数分别为基本类型和对象时，其中的对象就会拆箱为基本类型进行操作。

因为例子中，flag ? i : j;片段中，第二段的i是一个包装类型的对象，而第三段的j是一个基本类型，所以会对包装类进行自动拆箱。如果这个时候i的值为null，那么久会发生NPE。（自动拆箱导致空指针异常）

#### 场景五、函数参数与返回值

这个比较容易理解，直接上代码了：

```java
//自动拆箱
public int getNum1(Integer num) {
 return num;
}
//自动装箱
public Integer getNum2(int num) {
 return num;
}
```

### 5、自动拆装箱带来的问题

当然，自动拆装箱是一个很好的功能，大大节省了开发人员的精力，不再需要关心到底什么时候需要拆装箱。但是，他也会引入一些问题。

包装对象的数值比较，不能简单的使用==，虽然-128到127之间的数字可以，但是这个范围之外还是需要使用equals比较。

前面提到，有些场景会进行自动拆装箱，同时也说过，由于自动拆箱，如果包装类对象为null，那么自动拆箱时就有可能抛出NPE。

如果一个for循环中有大量拆装箱操作，会浪费很多资源。

### 6、常见笔试题：

```java
 // 1 整形包装类值比较都用 equals
    @Test
    public void test1() {
        Integer i = 12;
        Integer i2 = new Integer(12);
        System.out.println(i == i2);//false
        System.out.println(i.equals(i2));//true
    }

    // 2 Integer 最大最小值
    @Test
    public void test2() {
        System.out.println(Integer.MAX_VALUE);// 2^31 -1
        System.out.println(Integer.MIN_VALUE);// -2^31
    }

    // 3 Integer 初始了 -128 ~ 127 之间的值 -2^7 ~ 2^7 -1 。在范围内的值都放入cache 缓存中。超过范围就会 在堆中 new 对象
    @Test
    public void test3() {
        Integer i1 = 127;
        Integer i2 = 127;
        System.out.println(i1 == i2); // true

        Integer i3 = 128;
        Integer i4 = 128;
        System.out.println(i3 == i4); // false
    }
    
     // 4 综合题
    @Test
    public void test99(){
        Integer i1 =59;
        int i2 = 59;
        Integer i3 = Integer.valueOf(59);
        Integer i4 = new Integer(59);
        System.out.println(i1 == i2); // true：包装类和基本类型比较时，包装类自动拆箱为基本类型
        System.out.println(i1 == i3); // true：数值59 在-128到127之间，上文所说的缓存，因此为true，若数值不在-128到127之间则为false
        System.out.println(i1 == i4); // false：引用类型比较地址值，地址值不同 。 一个是缓存中地址，一个是堆中地址。
        System.out.println(i2 == i3); // true：i1的源码是i3，i2和i3比较结果和i2与i1比较结果相同，包装类和基本类型比较时自动拆箱
        System.out.println(i2 == i4); // true：包装类和基本类型比较时自动拆箱

        System.out.println(i3 == i4); // false  同i1 == i4 引用类型比较地址值，地址值不同 一个是缓存中地址，一个是堆中地址。
    }

```