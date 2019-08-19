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
///模拟等待两秒，返回OK
request() async {
  await Future.delayed(Duration(seconds: 1));
  return "ok!";
}

///得到"ok!"后，将"ok!"修改为"ok from request"
doSomeThing() async {
  String data = await request();
  data = "ok from request";
  return data;
}

///打印结果
renderSome() {
  doSomeThing().then((value) {
    print(value);
    ///输出ok from request
  });
}
```
