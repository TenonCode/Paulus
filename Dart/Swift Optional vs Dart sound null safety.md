> Swift Optional vs Dart sound null safety

# Swift Optional vs Dart sound null safety

A comparison between Swift Optional type and sound null safety in Dart.

Swift `Optional` 类型 与 Dart 中的 `sound null safety` 比较

Aug 23, 2020
>Recently the Dart team announced sound null safety. Dart was already a type-safe language, and now it is even better.

最近 Dart 开发团队宣布了 `sound null safety`，Dart 已经是类型安全的语言了，现在甚至更好。


>Sound null safety means that if a variable is not declared as nullable, then, whenever you access that variable, you will never find null, for its entire lifetime. This should remind you of Swift Optional.

`sound null safety` 意味着如果一个变量没有被声明为 `nullable`, 那么，不论你在这个变量的整个生命周期内何时访问这个变量，你都不可能获取 null。这听起来应该能让你回想起 Swift 中的 `Optional`

>Dart sound null safety and Swift Optional have a lot in common, but they are implemented in different ways. In Swift, Optional is a generic type, which means that, when we write var x: String?, what we mean is var x: Optional<String>: x is a variable that can contain a String or nil.


Dart 中 `sound null safety` 和 Swift `Optional` 有很多共同点，但是他们以不同的方式实现。
在 Swfit 中 `Optional` 是泛型，意味着当我们声明 `var x: String?` 时，我们所表达的是 `var x: Optional<String>` x 是一个变量可能是 String 或者 nil

**其实这里说的不够具体， 译者建议大家看下 Optional 源码，Optional是枚举，定义的时候使用了泛型**

>The Dart team took a different approach.

Dart的开发团队采用了一种不同的方法。

>When you write String? x, the meaning is the same, x is a variable that can contain a String or null, but String? is a union type, it describes a value that can be one of several types.

当你这么写： `String? x`, 语意上与 Swift Optional 相同，x 是一个变量它的值可能是 String 或 null，但是 `String? ` 是一种 联合类型，它代表一个这个值为多种类型中的一种。

>In Dart, String is called a non-nullable type, while String? is a nullable type. In the rest of the article, we are going to refer to nullable types in Dart and to Optional in Swift as optionals, or optional types.

在 Dart 中， String 是一种叫做 `non-nullable(不可空)` 类型，而 String? 是 `nullable(可空)` 类型

>The two implementations are different, but they have a lot in common.

这两种 `null-safe` 方式 的实现虽然不同，但他们也有很多相同之处。

= - = 译者吐槽： Swift 的 Optional 设计 基本与 Rust 中 Optional 一致

> Optional binding

## 可选绑定（Optional binding）

>In Swift you can bind the value of an optional to a new variable, if that value is not nil, using an if let statement or a guard let:

在 Swift 中 你可以把一个 Optional 类型值绑定给一个新变量，如下所示：

```Swift
var x: String?

guard let nonOptionalX = x else {
    return
}

print(nonOptionalX.count) 


if let nonOptionalX = x  {

    print(nonOptionalX.count) 

}

```

>In Dart, the same behavior can be obtained by simply checking if the variable is null:

在 Dart 中 相同的行为可以通过 简单的判断语言来实现:

```Dart
String? x;

if (x == null) {
    
    return;
}
    
print(x.length); 
```


>Optional chaining

## 可选访问链

Optional chaining is used to safely access properties or methods of an optional, and it is the same in both languages. It was already available in Dart before the introduction of sound null safety.

`Optional chaining` 常用来 安全地访问一个 `optional（可选类型）` 的属性或者方法。这一点在两种语言中相同。在 Dart 宣布 `sound null safety` 特性之前就已经支持了。


```Swift
var x: String?
let length = x?.count
```

```Dart
String? x;
var length = x?.length; 
```

> Nil-coalescing operator

## 空合运算符

As per the optional chaining, the nil-coalescing operator is available in both languages, and it was also available before the introduction of sound null safety.
和 可选访问链 一样， 空合运算符在两种语言中都可用。Dart 中也是 在 `sound null safety` 发布之前就支持。

```Swift
var x: Bool?
... 
print(x ?? false) 
```

```Dart
bool? x;
print(x ?? false); 

```

> late keyword
## 关键词 late

>In Dart, you can use the late keyword in front of a non-nullable property, to inform the compiler that that property is not initialized immediately, but you will assigne a non-null value to it before accessing it.
From the official article by the Dart team:

在 Dart 中，你可以用 late 关键词 在一个不可空的属性前来告诉编译器这个属性不会立刻初始化，但是你保证在它被访问之前会给他一个非空值。

```Dart
class Goo {
    late Viscosity v;
    Goo(Material m) {
        v = m.computeViscosity();
    }
}
```

>In my opinion, you should avoid this use of late as much as possible. Use an Optional instead. The code might be clear to you while you write it, but it lacks in maintainability.
在我看来，你应该尽量少用 late，使用 Optional 类型代替，这种代码你写的时候逻辑很清晰，但是缺乏维护性。

>You can also use late on a property that has an initializer:

你也可以在在构造方法中用 late:

```Dart
class Goo {
    late Viscosity v = _m.computeViscosity();
}
```

>In this case, the property v becomes lazy and it is initialized only when it is accessed for the first time. This is the same as using lazy in Swift:

这种情况， 属性 v 变成了懒加载，只在第一次被访问的时候初始化，用法和 `lazy` 在Swift 中相同。

```Swift
class Goo {
    lazy var v = m.computeViscosity()
}

```

>Extending the optional type
## 扩展可选类型

In both languages, we can extend the functionalities of the optional type:

在两种语言中，我们都能扩展 optional 类型的方法：

```Swift
extension Optional where Wrapped == Bool {
    
    func orFalse() -> Bool {
        return self ?? false
    }
}

let x: Bool? = nil
print(x.orFalse()) 
```

```Dart
extension UnwrapOrFalse on bool? {
    
    bool orFalse() {
        return this ?? false;
    }
}
bool? x;
print(x.orFalse()); 
```
> How programming in Dart will change
## Dart 编程会如何变化

> I am a big fan of Swift Optional. I like that the developer is forced to specified when a certain variable or parameter could be nil. Having a very similar feature in Dart will be welcomed by the entire community.

我是 Swift Optional 的追崇者，我喜欢那种开发者被强迫去定义清楚，一个变量或者参数是否可空。在 Dart 中拥有相似的特性会使得其受社区欢迎。

ps： 喜欢强迫···· = 3 =

>Say goodbye to all those, now useless, asserts on initializers and method definitions.

向那些初始化方法和方法无意义的断言说再见吧

```Dart
Article({this.id, this.description}):
    assert(id != null),
    assert(description != null);
```





