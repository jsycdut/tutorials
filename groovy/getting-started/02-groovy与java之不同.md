# groovy与java之不同
groovy在设计之初就特别考虑到了Java用户，对Java用户而言，会是很亲切的一门语言。接下来介绍groovy和java之间的主要不同之处。

## 默认引入
groovy中默认引入了一些包，内容如下

* java.io.*
 
* java.lang.*
 
* java.math.BigDecimal
 
* java.math.BigInteger
 
* java.net.*
 
* java.util.*
 
* groovy.lang.*
 
* groovy.util.*

## 运行时调度（多重方法）

groovy中的方法调用是在运行时选择确定的，这被称为运行时调度或者多重方法。这意味着调用哪个方法将有运行时传入参数的类型决定，这在java中是不可想象的，java中在编译期就确定了运行时该调用什么方法（依据方法参数的声明类型）。

下面这段代码，符合Java规范，也能被groovy执行，但是执行结果是不同的
```
int method(String arg) {
    return 1;
}
int method(Object arg) {
    return 2;
}
Object o = "Object";
int result = method(o);
```

在java中，输出结果是2，而在groovy中，输出为1。

原因是，java使用静态信息类型，o被声明为Object，所以Java运行时会去调用第二个方法，但是groovy是在运行时决定调用什么方法的，由于o本质为字符串，所以groovy在运行时选择了第一个方法。

## 数组初始化

groovy中,`{}`被留给了闭包使用，所以你不能像下面一样创建数组
```
int[] arr = {1, 2, 3,};
```
而是应该使用
```
int[] arr = [1, 2, 3]
```

## 包可见性

在java中，对字段或者方法不使用任何修饰符，将导致这个字段或方法拥有包内访问权限，但是groovy不是这样，在groovy中不使用修饰符的话，将会创建一个私有字段，以及其getter和setter方法。

如果你需要一个包私有属性，应该加上`@PackageScope`注解

```
class Person{
  @PckageScope String name
}
```

## ARM块
ARM，也就是jdk7引入的try语句块自动资源管理，groovy不支持这种写法，但是，groovy提供了依赖于闭包的不同方法来实现相同效果，其做法更加自然。看下面java代码

```java
Path file = Paths.get("/path/to/file");
Charset charset = Charset.forName("UTF-8");
try (BufferedReader reader = Files.newBufferedReader(file, charset)) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }

} catch (IOException e) {
    e.printStackTrace();
}
```

可以写为groovy 代码
```groovy
new File('/path/to/file').eachLine('UTF-8'){
  println it
}
```

或者写为一个更接近java的groovy代码
```groovy
new File('/path/to/file').withReader('UTF-8'){reader ->
  reader.eachLine{
    print it
  }
}
```

## 内部类

groovy的匿名内部类和嵌套类和java的规范相同，你也不用拿出java规范，然后摇着头指责groovy和java可是不一样的语言啊，干嘛要设计的一模一样。java内部类的实现看起来像groovy.lang.Closure，有些不同，也有些好处。获取私有属性和方法会成为一个问题，但是本地变量不必声明为final。

### 静态内部类

```
class A{
  static class B{}
}

new A.B()
```

静态内部类是groovy支持的最好的一宗内部类，如果你着实需要一个内部类，那么就写成静态的吧。

### 匿名内部类

```
import java.util.concurrent.CountDownLatch
import java.util.concurrent.TimeUnit

CountDownLatch called = new CountDownLatch(1)

Timer timer = new Timer()
  timer.schedule(new TimeTask(){
      void run(){
        called.countDown()
      }
  }, 0)

assert called.await(10, TimeUnit.SECONDS)
```

### 创建非静态内部类实例

在Java中，可以这么干
```java
public class Y{
  public class X {}
  public X foo(){
    return new X();
  }
  public static X createX(Y y){
    return y.new X();
  }
}
```

groovy不支持`y.new X()`这种语法，你必须写成`new X(y)`，就像下面的样子
```
public class Y{
  public class X{}
  public X foo{
    return new X()
  }

  public static X createX(Y y){
    return new X(y)
  }
}
```

需要注意的是，groovy支持这么一种情况，调用某个方法，这个方法有一个形参，但是运行的时候不用给实参，该参数自动置为null，对于构造器也是一样，所以可能出现一种危险情况，写new X()而不是写new X(this)的时候，就会出现new X(null)这种实际情况，目前来说，还没有什么能避免这种情况的办法。

### lambdas
java 8支持lambdas，以及方法引用
```
Runnable run = () -> System.out.println("Run");
list.forEach(System.out::println);
```

java 8的lambda或多或少可以认为是一个匿名内部类，groovy不支持lambda，但是却支持闭包
```
Runnable run = {println 'run'}
list.each(println it)
```

### GStrings
一个双引号括起来的字符串字面量被groovy称作GString值，当处理一个拥有`$`符号的String值的类的时候，Groovy可能编译失败，或者说会产生一些微妙的和java不同的地方。

一般来说，当API声明参数类型的时候，groovy会自动转化GString和String，但是注意java api中那些接受Object类型参数并且检查实际类型的情况。

### String以及Character常量

在groovy中，单引号括起来的常量被称作String，双型号括起来的，成为String或者GString，这取决于常量是否拥有插入符号
```
assert 'c'.getClass()==String
assert "c".getClass()==String
assert "c${1}".getClass() in GString
```

当参数类型是char的时候，传入String，会被转为char，当调用char类型参数的方法时，我们需要显式的转换或者确保该值已经被提前转换。
```
char a='a'
assert Character.digit(a, 16)==10 : 'But Groovy does boxing'
assert Character.digit((char) 'a', 16)==10

try {
  assert Character.digit('a', 16)==10
  assert false: 'Need explicit cast'
} catch(MissingMethodException e) {
}
```

groovy在转换字符串到char的时候，支持两种方法（groovy风格和C风格），但是当字符串拥有多个字符的时候，这俩方法有些不同的区别。groovy风格的转换要更加宽容，会采用首个字符，然而C风格的转换会因异常而失败

```
// 对单个字符的字符串来说，两种风格效果相同
assert ((char) "c").class==Character
assert ("c" as char).class==Character

// 当字符串拥有多个字符的时候，两种风格就不一样了
try {
  ((char) 'cx') == 'c' //这一步就会丢出异常，因为c风格在不支持多个字符的字符串转为char类型
  assert false: 'will fail - not castable'
} catch(GroovyCastException e) {
}
assert ('cx' as char) == 'c'
assert 'cx'.asType(char) == 'c'
```

### 基础类型以及包装器

groovy用对象代表一切（java：你黑我有基础类型？你可别忘了我在jdk5里面的自动拆装箱哦小老弟！！！），groovy会自动给基础类型套上了引用类型，这和java不同，java里面，有个精度拓宽的操作，叫做widening，比如在long参数类型的方法里面送int进入，int会被拓宽为long，但是groovy没这套操作
```
int i
m(i)

void m(long l) {           
  println "in m(long)"
}

void m(Integer i) {        
  println "in m(Integer)"
}

```

java，就会调第一个m方法，groovy就会调第二个方法

### `==`
在java中，`==`意为比较基础类型的相等，以及对象的身份，在groovy中，`==`摇身一变，当a和b是`Comparable`时，意为`a.compareTo(b)==0`，其他情况，视作`a.equals(b)`，如果检查对象身份，groovy用is，比如`a.is(b)`

这里对象的身份意思为，a和b是不是同一个对象。

### 转换
java会做自动转换，比如int到long或者int到char这种精度拓宽和缩减的操作，java自动转换之操作以及groovy之转换，请参考[这里的12小节](http://www.groovy-lang.org/differences.html)

### groovy额外的关键字

groovy继承了java所有的关键字，但是也扩展了几个

* as
* def
* in
* trait

第二章就这么翻译完了,耗时4个小时，2018-12-31:00:13:20

元旦快乐，今晚喝了点白酒，配着雪碧，所以我好像有点醉...
