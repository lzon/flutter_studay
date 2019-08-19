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

  // 在构造函数体执行之前设置实例变量的语法
  Point(this.x, this.y);
}
```
