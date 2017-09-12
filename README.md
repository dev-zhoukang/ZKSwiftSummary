##### 1. 遍历数组
```
func minMax(array: [Int]) -> (min: Int, max: Int) {
var currentMin = array[0]
var currentMax = array[0]
for value in array[1..<array.count] {
if value < currentMin {
currentMin = value
} else if value > currentMax {
currentMax = value
} }
return (currentMin, currentMax)
}
```

每个函数都有种特定的函数类型，函数的类型由函数的参数类型和返回类型组成。

闭包是自包含的函数代码块，可以在代码中被传递和使用。Swift 中的闭包与 C 和 Objective-C 中的代码块(b locks)以及其他一些编程语言中的匿名函数比较相似。
闭包可以捕获和存储其所在上下文中任意常量和变量的引用。被称为包裹常量和变量。 Swift 会为你管理在捕获 过程中涉及到的所有内存操作。

##### 2. 闭包
```
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
var runningTotal = 0
func incrementer() -> Int {
runningTotal += amount
return runningTotal
}
return incrementer
}
```
incrementer() 函数并没有任何参数，但是在函数体内访问了 runningTotal 和 amount 变量。这是因为它从 外围函数捕获了 runningTotal 和 amount 变量的引用。捕获引用保证了 runningTotal 和 amount 变量在 调用完 makeIncrementer 后不会消失，并且保证了在下一次执行 incrementer 函数时，runningTotal 依旧存 在。
```
let incrementByTen = makeIncrementor(forIncrement: 10)    
incrementByTen() // 返回的值为10    
incrementByTen() // 返回的值为20 
incrementByTen() // 返回的值为30

let incrementBySeven = makeIncrementor(forIncrement: 7) 
incrementBySeven() // 返回的值为7 注意，跟上面的加10互不影响
incrementBySeven() // 返回的值为14

incrementByTen() // 返回的值为40 注意这值跟上面的加7没有影响。
```
上面的例子中，incrementBySeven 和 incrementByTen 都是常量，但是这些常量指向的闭包仍然可以增加其捕 获的变量的值。这是因为函数和闭包都是引用类型。

##### 3. 结构体和枚举是值类型 
结构体和枚举是值类型 
值类型被赋予给一个变量、常量或者被传递给一个函数的时候，其值会被拷贝。 
实际上，在 Swift 中，所有的基本类型:整数(Integer)、浮 点数(floating-point)、布尔值(Boolean)、字符串(string)、数组(array)和字典(dictionary)，都是 值类型，并且在底层都是以结构体的形式所实现。
当值类型的实例被声明为常量的时候，它的所有属性也就成了常量。所以，像数组、字典等都是这样。

##### 4. 类和结构体的选择
在你的代码中，你可以使用类和结构体来定义你的自定义数据类型。 
然而，结构体实例总是通过值传递，类实例总是通过引用传递。这意味两者适用不同的任务。
当你在考虑一个工 程项目的数据结构和功能的时候，你需要决定每个数据结构是定义成类还是结构体。 
按照通用的准则，当符合一条或多条以下条件时，请考虑构建结构体: 
• 该数据结构的主要目的是用来封装少量相关简单数据值。 
• 有理由预计该数据结构的实例在被赋值或传递时，封装的数据将会被拷贝而不是被引用。 
• 该数据结构中储存的值类型属性，也应该被拷贝，而不是被引用。 
• 该数据结构不需要去继承另一个既有类型的属性或者行为。
举例来说，以下情境中适合使用结构体: 
• 几何形状的大小，封装一个 width 属性和 height 属性，两者均为 Double 类型。 
• 一定范围内的路径，封装一个 start 属性和 length 属性，两者均为 Int 类型。 
• 三维坐标系内一点，封装 x ， y 和 z 属性，三者均为 Double 类型。 
在所有其它案例中，定义一个类，生成一个它的实例，并通过引用来管理和传递。实际中，这意味着绝大部分的 自定义数据构造都应该是类，而非结构体。
##### 5. 字符串、数组、和字典类型的赋值与复制行为
Swift 中，许多基本类型，诸如 String ， Array 和 Dictionary 类型均以结构体的形式实现。这意味着被赋值给 新的常量或变量，或者被传入函数或方法中时，它们的值会被拷贝。
Objective-C 中 NSString ， NSArray 和 NSDictionary 类型均以类的形式实现，而并非结构体。它们在被赋值或 者被传入函数或方法时，不会发生值拷贝，而是传递现有实例的引用。
注意 以上是对字符串、数组、字典的“拷贝”行为的描述。在你的代码中，拷贝行为看起来似乎总会发生。然而，Sw ift 在幕后只在绝对必要时才执行实际的拷贝。Swift 管理所有的值拷贝以确保性能最优化，所以你没必要去回 避赋值来保证性能最优化。

##### 6. 注意 如果一个被标记为 lazy 的属性在没有初始化时就同时被多个线程访问，则无法保证该属性只会被初始化一 次。
存储型类型属性是延迟初始化的，它们只有在第一次被访问的时候才会被初始化。即使它们被多个线程同时访 问，系统也保证只会对其进行一次初始化，并且不需要对其使用 lazy 修饰符。

##### 7. 
在方法的 func 关键字之前加上关键字 static ，来指定类型方法。类还可以用关键字 class 来允许子类重写 父类的方法实现。
注意
在 Objective-C 中，你只能为 Objective-C 的类类型(classes)定义类型方法(type-level methods)。在 Swift 中，你可以为所有的类、结构体和枚举定义类型方法。每一个类型方法都被它所支持的类型显式包含。

##### 8. 代理的写法
```
@objc protocol RequestHandle {
@objc optional func requestFinished()
}

class Request {
weak var delegate: RequestHandle!

func send() {
print("发送数据")
}

func getResponse() {
delegate?.requestFinished?()
}
}

class RequestManager: RequestHandle {
@objc func requestFinished() {
print("请求完成")
}

func sendData() {
let req = Request()
req.delegate = self
req.send()
}
}
```
##### 9. 解决闭包里面的循环引用
printName 这个闭包 是 self 的属性, 即被 self 强持有, 所以如果在 printName 强持有 self 的话, 就会引起循环引用. 解决方法就是如下, 使用weak self
```
class Person {
let name: String
lazy var printName: ()->() = {
[weak self] in
if let strongSelf = self {
print("The name is \(strongSelf.name)")
}
}

init(personName: String) {
name = personName
}
deinit {
print("Person deinit \(self.name)")
}
}

var xiaoming: Person? = Person(personName: "XiaoMing")
xiaoming!.printName()
xiaoming = nil
```
注意这种带有中括号的闭包中参数标注方法.
```
// 带标注
lazy var test: (Int) -> Bool = {
(number: Int) -> Bool in
return true
}
// 不带标注    
lazy var test1: (Int) -> Bool = {
[weak self, weak someObj] (number: Int) -> Bool in
return true
}
```
##### 10. 隔离代码的写法
注意后面的圆括号
```
// 使用自执行的匿名闭包的形式来分离代码, 注意后面的圆括号
let titleLabel:UILabel = {
let label = UILabel(frame: CGRect(x: 100, y: 100, width: 100, height: 100)
label.textColor = .red
label.text = "title"
return label
}()
```
##### 11. 格式化字符(小数点格式化)
可以写个小扩展
```
extension Double {
func format(_ f: String) -> String {
return String(format: "%\(f)f", self)
}
}

let b = 2323.68686868
let f = ".2"
print("double:\(b.format(f))")
```
##### 12. 位运算枚举
定义的时候如下:
```
struct YourOption: OptionSet {
let rawValue: UInt
static let none = YourOption(rawValue: 0)
static let option1 = YourOption(rawValue: 1 << 0)
static let option2 = YourOption(rawValue: 1 << 1)
// ...
}
```
使用的时候, 用集合的形式(相当于OC中的或运算符)
```
UIView.animate(withDuration: .3,
delay: 0,
options: [.beginFromCurrentState, .curveEaseInOut],
animations: {},
completion: nil)
```
##### 13. 给扩展添加关联属性
```
// 添加关联值
private var key: Void?
extension ZKTestClass {
var title: String? {
get {
return objc_getAssociatedObject(self, &key) as? String
}
set {
objc_setAssociatedObject(self, &key, newValue, .OBJC_ASSOCIATION_RETAIN_NONATOMIC)
}
}
}

// for test
func printTitle(_ input: ZKTestClass) {
if let title = input.title {
print("title is \(title)")
}
else {
print("set fail")
}
}

let foo = ZKTestClass()
printTitle(foo)
foo.title = "ZK is an iOS developer"
printTitle(foo)
```
##### 14. 善于利用抛出异常机制
```
var users: [String: String] = [ "xiaoming": "123", "xiaohong": "567" ]
// 抛出异常
enum LoginError: Error {
case UserNotFound, UserPasswordNotMatch
}

func loginAction(account: String, password: String) throws {
if users.keys.contains(account) {
throw LoginError.UserNotFound
}
if users[account] != password {
throw LoginError.UserPasswordNotMatch
}
print("Login successfully")
}

// 调用就是这种形式, 注意下面的catch的分类
do {
try loginAction(account: "xiaoming", password: "2323")
} catch LoginError.UserNotFound {
print("user not found")
} catch LoginError.UserPasswordNotMatch {
print(user password not match)
}
```¡
