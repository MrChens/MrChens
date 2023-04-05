# 可空类型
nil合并运算符要么获取值，要么使用默认值
字典
`var dict: Dictionary<Key, Value> = [:]`

# 值类型，引用类型
Swift的基本类型都是结构体实现（`Array，Dictionary，Int，String`）

# 集合Set
集合需要其元素符合Hashable协议
`var groceryBag = Set<String>()`

并集操作,包含2个集合中的所有元素 `.union()`
交集操作，只有2个集合中重复的元素（重复的那部分）`.intersection`
不相交，2个集合是不是没有重合的元素`.isDisjoint(with:)`

# 函数
## 变长参数
 ```
 func greetings(to names: String...) {
  for name in names{

  }
}
```

## in-out 参数
修改实参的值，不能有默认值，变长参数不能标记为`inout`
```
var error = "failed"
func appendErrorCode(_ code: Int, toErrorString errorString: inout String){
  errorString += "bad request"
}
appendErrorCode(400, toErrorString:error)
```
## 嵌套函数
```
func arreaOfTriangleWith(base: Double, height: Double) -> Double {
    let numerator = base * height
    func divide() -> Double {
        return numerator / 2
    }
    return divide()
}
```
# 闭包
## 闭包表达式语法
关键字in用来分隔闭包的参数，返回值与闭包体内的语句
```
{(parameters) -> return type in
    //code
}

let counts = [1,23,4,5,3,2]
let sorted = counts.sorted { i, j in
    i < j
}
let sorted = counts.sorted(by: {
    (i: Int, j:Int) -> Bool in
    return i < j
})
```
## 函数作为返回值
```
func makeTownGrand() -> (Int, Int) -> Int {
    func buildRoads(byAddingLights lights: Int, toExistingLights existingLights: Int) -> Int {
        return lights + existingLights
    }
    return buildRoads(byAddingLights:toExistingLights:)
}

let grand = makeTownGrand()
let lights = grand(4, 4)
print(lights)// 8
```
#函数作为参数
```
func makeTownGrand(withbudget budget: Int, condition: (Int) -> Bool) -> ((Int, Int) -> Int)? {
    if condition(budget) {
        func buildRoads(byAddingLights lights: Int, toExistingLights existingLights: Int) -> Int {
                return lights + existingLights
            }
        return buildRoads(byAddingLights:toExistingLights:)
    } else {
        return nil
    }
}

func evaluate(budget: Int) -> Bool {
    return budget > 10_000
}
var stoplights = 4
if let exLights = makeTownGrand(withbudget: 100_000, condition: evaluate(budget:)){
    stoplights = exLights(4, 4)
}
print(stoplights)// 8
```
# 枚举（值类型）
## 方法
```
enum Lightbulb {
    case on
    case off

    mutating func toggle() {
        switch self {
        case .on:
            self = .off
        case .off:
            self = .on
        }
    }

    func toggle(_ self:inout Lightbulb) {
        switch self {
        case .on:
            self = .off
        case .off:
            self = .on
        }
    }
}
```

## 关联值
```
enum ShapeDimesions {
    case square(side: Double)
    case rectangle(width: Double, height: Double)

    func area() -> Double {
        switch self {
        case let .square(side: side):
            return side * side
        case let .rectangle(width: w, height: h):
            return w * h
        }
    }
}
```
## 递归枚举
```
// indirect enum FamilyTree{}// 指针
enum FamilyTree {
    case noKnownParents
    indirect case oneKnowParent(name: String, ancestors: FamilyTree)
    indirect case twoKnownParents(fatherName: String, fatherAncestors: FamilyTree,
                                  motherName: String, motherAncestors: FamilyTree)
}

let fredAncestors = FamilyTree.twoKnownParents(
    fatherName: "Fred Sr",
    fatherAncestors: .oneKnowParent(name: "Beth", ancestors: .noKnownParents),
    motherName: "Marsha",
    motherAncestors: .noKnownParents
)
```
# 结构体
```
struct Town {
  var population = 1
  mutating func changePopulation(by amount: Int){
    population += amount
  }
}
```
# 类（可以继承，结构体不行）
## 类方法，禁止类方法重写，禁止方法重写
```
class Monster {
    var name = "Monster"
    func terroizeTown() {}
}

class Zombie: Monster {
    var walksWithLimp = true
    //类方法
    class func makeSpookyNoise() -> String { "" }
    //阻止类方法被子类重写
    static func makeSpookyNoises() -> String { "" }
    //阻止类方法被子类重写
    final class func makeSpookyNoisess() -> String { "" }
    // 禁止重写
    final override func terroizeTown() { }
}
```
# mutating方法的第一个参数是self，并以inout的形式传入。因为值类型在传递的时候会被复制，所以对于非mutating方法，self其实是值的副本，为了进行修改，self需要被声明为inout，而mutating就是swift编译器让你完成这个任务的工具

# 属性（存储属性stored，计算属性computed）
## 计算属性可以使用在类，结构体，枚举中
## 嵌套类型
## 惰性存储属性（lazy属性必须声明为var，因为他的值会发生变化）
```
class Monsters {
    var name = "Monster"
    //计算属性computed
    var victimPool: Int {
        get {
            name.count
        }
        set {
            name = "\(newValue)"
        }
    }
}
```

## 访问控制

| 访问层级 |   描述   |   对.....可见   |  能在.....中继承    |
| :------------- | :------------- | :------------- | :------------- |
|    open   |    实体对模块内的所有文件以及引入了该模块的文件都可见，并且可以继承   |   实体所在的模块以及引入实体所在模块的模块   |  实体所在的模块以及引入实体所在模块的模块    |
|    public   |    实体对模块内的所有文件以及引入了该模块的文件都可见   |   实体所在的模块以及引入实体所在模块的模块   |   实体所在的模块   |
|    internal(默认)   |  实体对模块内的文件可见     |   实体所在的模块   |  实体所在的模块    |
|    fileprivate   |   实体只对所在的源文件可见    |   实体所在的文件   |   实体所在的文件   |
|    private   |   实体只对所在的作用域可见    |   所在的作用域   |  所在的作用域    |


# 初始化
结构体在初始化的时候可以给常量再赋值一个值

## 类的便捷初始化方法
```
convenience init(nickName: String) {
        self.init()
    }
```
## 类的必须初始化方法
```
required init(nick: String) {}
```
## 反初始化 `deinit{}`

## 可失败的初始化方法
```
init?(name: String) {}
```
# 协议
能让你定义类型需要慢猪的接口
```
class Monster:CustomStringConvertible {
    var description: String{
        return name
    }
  }
```
## 协议继承，协议组合
```
// 协议继承
protocol TabularDataSource: CustomStringConvertible {
    var numberOfRows: Int { get }
    var numberOfColumns: Int { get }

}
struct Person {

}
struct Department: TabularDataSource {
    var description: String {
        name
    }

    let name: String
    var people = [Person]()

    mutating func add(_ person: Person) {
        people.append(person)
    }
    var numberOfRows: Int {
        people.count
    }

    var numberOfColumns: Int {
        3
    }
}
func printTable(_ data: [[String]], withColumnLabels columLabels: String...) {}
//单个协议
func printTable(_ dataSource: TabularDataSource) {}
//协议组合
func printTable(datasource: TabularDataSource & CustomStringConvertible) {}
```
# 错误处理
可恢复的错误recoverable error 不可恢复的错误nonrecoverable error
## 捕获不可恢复的错误的工具
调试模式时才有效，发布模式会被删除
```
precondition(false, "当条件为false时触发断言")
assert(false, "当条件为false时触发assert")
```
## 捕获错误
`do { try } catch {print("\(error)")}`
当一个函数声明为抛出异常，则可以不写do/catch,如果try失败了， 错误会被抛出到`parse()`外面
`func parse() throws -> Int { try }`
使用try?调用函数时，必须处理返回为nil的可能性

# 拓展
## 用拓展添加初始化方法（可以不失去成员初始化方法）
## 嵌套类型和拓展

# 范型
## 类型约束
```
func checkIfEqual<T: Equatable>(_ first: T, _ second: T) -> Bool {
    first == second
}
func checkIfDescriptionsMatch<T: CustomStringConvertible, U: CustomStringConvertible>(_ first: T,_ second: U) -> Bool {
    return first.description == second.description
}
```
## 关联类型协议
```
protocol IteratorProtocol {
    associatedtype Element
    mutating func next() -> Element?
}
struct Stack<Element> {
    var items = [Element]()

    mutating func pop() -> Element? {
        guard !items.isEmpty else {
            return nil
        }
        return items.removeLast()
    }
}

struct StackIterator<T>: IteratorProtocol {
    typealias Element = T
    var stack: Stack<T>

    mutating func next() -> T? {
        return stack.pop()
    }
}

var myStack = Stack<Int>()
var myStackIterator = StackIterator(stack: myStack)
while let value = myStackIterator.next() {}
```
## 类型约束中的where子句
## 带where子句的协议拓展
`extension Sequence where Iterator.Element == Exercise{}`
