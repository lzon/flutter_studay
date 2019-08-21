## 1.Dart中const与final的区别
用final修饰的变量，必须在定义时将其初始化，其值在初始化后不可改变；const用来定义常量。
它们的区别在于，const比final更加严格。  
final只是要求变量在初始化后值不变，但通过final，我们无法在编译时（运行之前）知道这个变量的值；  
而const所修饰的是编译时常量，我们在编译时就已经知道了它的值，显然，它的值也是不可改变的。

```
{
  final int m1 = 60;
  const int n1 = 42;
  final int m2 = x(); // 正确(而这个不会)
  const int n2 = x(); // 错误(这个就会报错)，
}

int x() {
  // 代码...
}
```
## 2.Dart中构造函数

.默认构造函数(Default constructors)
如果类定义时，没有显式的定义构造函数，Dart 语言将提供默认的构造函数。默认的构造函数没有参数。

.构造函数不能继承(Constructors aren’t inherited)
Dart 语言中，子类不会继承父类的命名构造函数。如果不显式提供子类的构造函数，系统就提供默认的构造函数。

```
class Point {
  num x;
  num y;

  Point(num x, num y) {
    // 这种初始化赋值操作还有更好的实现方式，请往下看！
    this.x = x;
    this.y = y;
  }
}
```
可以简写为  
```
class Point {
  num x;
  num y;

  // this.x 相当于赋值 ,而dar里面建议这样写
  Point(this.x, this.y);
}
```
命名构造函数
```
class Point { 
  num x; 
  num y; 

  Point(this.x, this.y); 

  //命名构造函数
  Point.fromJson(Map json) { 
    x = json['x']; 
    y = json['y']; 
  } 
}

```
如果超类没有构造函数可以直接添加
```
class Point {
  num x;
  num y;
}
class Point1 extends Point {
  num x;
  num y;

  Point1(this.x, this.y);//可以在子类中直加
}
```
如果超类中有构造函数，子类不能重载，必需使用super
```
class Point {
  num x;
  num y;
  Point(this.x, this.y);
}

class Point1 extends Point {
  num x;
  num y;

  Point1(this.x, this.y) : super(x, y) {}//必须使用super，super后的x是把Point1的x的值传给父类x
}
```
重定向构造函数
```
class Point { 
  num x; 
  num y; 

  Point(this.x, this.y); 

  //重定向构造函数，使用冒号调用其他构造函数 , this相当于自己的构造函数， 把0赋值给y
  Point.alongXAxis(num x) : this(x, 0);
}
```
调用超类构造函数
```
void main() {
  Child child = Child.fromJson(10, 10); //调用的是第二个构造函数
  child = Child(10, 10);//调用的是第一个构造函数
  child.xx();
}

class Parent {
  int x;
  int y;

  //父类命名构造函数不会传递
  Parent.fromJson(x, y)
      : x = x,
        y = y {
    print('父类命名构造函数');
  }
}

class Child extends Parent {
  int x;
  int y;

  //若超类没有默认构造函数， 需要手动调用超类其他构造函数
  Child(x, y) : super.fromJson(x, y) {
    //调用父类构造函数的参数无法访问 this
    this.x = x;
    this.y = y;
    print('子类构造函数');
  }

  //在构造函数的初始化列表中使用super()，需要把它放到最后 ;冒号后面的赋值操作
  Child.fromJson(x, y)
      : x = x,
        y = y,
        super.fromJson(x, y) {
    print('子类命名构造函数');
  }

  xx() {
    print('x=$x , y=$y');
  }
}
```
静态构造函数
```
class ImmutablePoint {
    //如果你的类产生的对象永远不会改变，你可以让这些对象成为编译时常量。为此，需要定义一个 const 构造函数并确保所有的实例变量都是 final 的。
    final num x;
    final num y;
    const ImmutablePoint(this.x, this.y);
    static final ImmutablePoint origin = const ImmutablePoint(0, 0);
}
```
## 3.Dart可选参数的写法
```
//不在中括号和大括号里面的是必须要填的
void main() {
  getPart1("大括号", name: "小米", pwd: "123");
  getPart1("大括号", pwd: "123");
  getPart2("中括号", "华为", "123");
  getPart2("中括号", "华为");
  getDeful("默认的");
}
/**
* 1,带有大括号的:传值比较明确.也可以不填，值为null
*/
getPart1(var a, {String name, String pwd}) {
  print("a=$a name=$name pwd=$pwd");
}
/**
 * ,2 中括号的:默认按照顺序,也可以不填，值为null
 */
getPart2(var a, [String name, String pwd]) {
  print("a=$a name=$name pwd=$pwd");
}
/**
 * 3,默认参数值
 */
getDeful(String a, {String name="小明", String pwd="123"} ){
  print("a=$a name=$name pwd=$pwd");
}
```
## 3.Flutter中异步
1.Future:
```
void main() {
  new Future(futureTask)
      .then((m) => "futueTask execute result:$m") //   任务执行完后的子任务
      .then((m) => m.length) //  其中m为上个任务执行完后的返回的结果
      .then((m) => printLength(m))
      .whenComplete(() => whenTaskCompelete()); //  当所有任务完成后的回调函数
  print("会先执行，再执行上面的异步");
}

int futureTask() {
  return 21;
}

void printLength(int length) {
  print("Text Length:$length");
}

void whenTaskCompelete() {
  print("Task Complete");
}
```
2. Flutter 中支持 async/await
```
void main(){
  renderSome();
  print("测试...");
}

///1.模拟等待两秒，返回OK
request() async {
  await Future.delayed(Duration(seconds: 1));
  return "ok!";
}

///2.得到"ok!"后，将"ok!"修改为"ok from request"
doSomeThing() async {
  String data = await request();
  data = "ok from request";
  return data;
}

///3.打印结果
renderSome() {
  doSomeThing().then((value) {
    print(value);
    ///输出ok from request
  });
}
```
