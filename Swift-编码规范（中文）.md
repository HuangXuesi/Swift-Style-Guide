# swift-编码规范（中文）

## 目录
 + [Correctness(正确性)](#正确性)  
 + [Naming(命名)](#命名)  
   - [Protocols(协议命名)](#协议命名)  
   - [Enumerations(枚举)](#枚举)    
   - [Selectors(选择器)](#选择器)  
   - [Generics(泛型)](#泛型)  
   - [Language(语言)](#语言)
 + [Code Organization(代码组织)](#代码组织)  
   - [Protocol Conformance(协议实现)](#协议实现)  
   - [Unused Code(无用代码)](#无用代码)  
   - [Minimal Imports(最少引入)](#最少引入)  
 + [Spacing(空格)](#空格)  
 + [Comments(注释)](#注释)  
 + [Classes and Structures(类和结构体)](#类和结构体)  
   - [Use of Self(self用法)](#self用法)  
   - [Protocol Conformance(协议一致性)](#协议一致性)  
   - [Computed Properties(计算属性)](#计算属性)  
   - [Final(final关键字)](#final关键字)
 + [Function Declarations(函数定义)](#函数定义)  
 + [Closure Expressions(闭包表达式)](#闭包表达式)
 + [Types(类型)](#类型)
   - [Constants(常量)](#常量)  
   - [Static Methods and Variable Type Properties(静态方法和类型属性)](#静态方法和类型属性)  
   - [Optionals(可选型)](#可选型)  
   - [Struct Initializers(结构体初始化)](#结构体初始化)  
   - [Lazy Initialization(懒加载)](#懒加载)  
   - [Type Inference(类型引用)](#类型引用)  
   - [Syntactic Sugar(语法)](#语法)  
 + [Functions vs Methods(函数和方法)](#函数和方法)  
 + [Memory Management(内存管理)](#内存管理)  
   - [Extending Lifetime(扩展声明周期)](#扩展声明周期)  
 + [Access Control(访问控制)](#访问控制)  
 + [Control Flow(控制流)](#控制流)  
 + [Golden Path(黄金路线)](#黄金路线)  
   - [Failing Guards(失败返回)](#失败返回)  
 + [Semicolons(分号)](#分号)  
 + [Parentheses(圆括号)](#圆括号)  
 + [Copyright Statement](#CopyrightStatement)  
 + [Smiley Face](#SmileyFace)  
 
## 正确性
将 warnings 视为 errors 。比如不要用 ++ 、-- 或 c 风格的循环或 string 型的 selectors 。
## 命名
1. classes, structures, enumerations and protocols 等类型名字以首字母大写驼峰命名。变量、方法名以小写驼峰方式命名。
#### 推荐
```swift
private let maximumWidgetCount = 100

class WidgetContainer {
  var widgetButton: UIButton
  let widgetHeightPercentage = 0.85
}
```
#### 不推荐
```swift
let MAX_WIDGET_COUNT = 100

class app_widgetContainer {
  var wBut: UIButton
  let wHeightPct = 0.85
}
```
2. 尽量避免“简称”和“缩略词”，通用缩略词应该整体大写或整体小写。
#### 推荐
```swift
let urlString: URLString
let userID: UserID
```
#### 不推荐
```swift
let uRLString: UrlString
let userId: UserId
```
3. 初始化方法或其他方法，每个参数前都应该有明确说明。
```swift
func dateFromString(dateString: String) -> NSDate
func convertPointAt(column column: Int, row: Int) -> CGPoint
func timedAction(afterDelay delay: NSTimeInterval, perform action: SKAction) -> SKAction!

// 调用方式:
dateFromString("2014-03-14")
convertPointAt(column: 42, row: 13)
timedAction(afterDelay: 1.0, perform: someOtherAction)
```
4. 遵循 Apple 官方习俗为方法命名。
```swift
class Counter {
  func combineWith(otherCounter: Counter, options: Dictionary?) { ... }
  func incrementBy(amount: Int) { ... }
}
```
### 协议命名
protocols是描述能力的应该以-ing, -able或 -ible结尾。如： Equatable, Resizing。
### 枚举
每个枚举值用小写字母开始。
```swift
enum Shape {
  case rectangle
  case square
  case rightTriangle
  case equilateralTriangle
}
```
### 选择器
#### 推荐
```swift
let sel = #selector(viewDidLoad)
```
#### 不推荐
```swift
let sel = #selector(ViewController.viewDidLoad)
```
### 泛型
泛型参数应该描述清楚所规定的泛型。当不确定泛型类型是才使用传统的大写字母 如`T`, `U`, or `V`表示泛型。
#### 推荐
```swift
struct Stack<Element> { ... }
func writeTo<Target: OutputStream>(inout target: Target)
func max<T: Comparable>(x: T, _ y: T) -> T
```
#### 不推荐
```swift
struct Stack<T> { ... }
func writeTo<target: OutputStream>(inout t: target)
func max<Thing: Comparable>(x: Thing, _ y: Thing) -> Thing
```
### 语音
用美式英语拼写。
## 代码组织
### 协议实现
每个 protocol 的实现分别对应一个 extension 的方式实现。
#### 推荐
```swift
class MyViewcontroller: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewcontroller: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewcontroller: UIScrollViewDelegate {
  // scroll view delegate methods
}
```
#### 不推荐
```swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```
由于编译器不允许一个类重新申明 protocol 实现，因此，不是一直要求每个 protocols 都要分离出一个 extension.特别是当该 class 是个最终 class 或该 class 方法很少。  
对UIKit view controllers，将lifecycle, custom accessors, and IBAction等方法单独放在一个 class extension 中。
### 无用代码
无用的code ，包括 Xcode 注释都应该被移除。空方法应该被移除。
#### 推荐
```swift
override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  return Database.contacts.count
}
```
#### 不推荐
```swift
override func didReceiveMemoryWarning() {
   super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}

override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
   // #warning Incomplete implementation, return the number of sections
   return 1
}

override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // #warning Incomplete implementation, return the number of rows
  return Database.contacts.count
}
```
### 最少引入
不要引用`UIKit` 和 `Foundation`
## 空格
1. 缩进用4个空格而不是用tab调整对齐。用命令Ctr+I重新排列代码。
#### 推荐
```swift
if user.isHappy {
  // Do something
} else {
  // Do something else
}
```
#### 不推荐
```swift
if user.isHappy
{
  // Do something
}
else {
  // Do something else
}
```
2. 每两个方法之间空一行。
3. 冒号左边无空格，右边有一个空格。? : 和 空字典[:] 例外。
#### 推荐
```swift
class TestDatabase: Database {
  var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```
#### 不推荐
```swift
class TestDatabase : Database {
  var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```
## 注释
必要的时候添加注释注明为什么。避免一大段注释解释一行代码，代码应该有自我解释性。
## 类和结构体




