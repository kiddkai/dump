Type Data Class Instance
========================

### data

定义一个广泛的类型，这个类型里面包含了一系列的实际数据，比如拿电话来说，
它包含了手机和固化。所以可以定义下面的 data 类型。

```haskell
data Phone = Mobile | Tele
```

拿Boolean 来说的话就是这样子：

```haskell
data Boolean = True | False
```

有时候data 也可以带上类型参数，比如说Maybe ，它的data 定义是这样的：

```haskell
data Maybe a = Nothing | Just a
```

像上面的例子的话 a 可以是任何类型的数据，比如说 `Just "haha"` 对应的数据类型就是
`Maybe [Char]`。

还有一种玩法是带构造参数的 data ，这种方式提供了一种很直接的方式给现实的数据建模：

```haskell
data Car = Car String String Int deriving (Show)

> Car "Ford" "Mustang" 2014
```

上面例子的参数基本上都是匿名的，同样haskell支持使用带名的构造参数：

```haskell
data Car = Car { company::String, model::String, year::Int } deriving (Show)

> Car { company = "Ford", mdoel = "Mustang", year = 2014 }
```

继续上面的例子，有时候我们想把 company 换成别的类型，把 model 也换成别的类型，
也有可能把 year 换成别的数据类型。可以写成下面这样子：

```haskell
data Car c m y = Car { company::c, model::m, year::y } deriving (Show)
```

另外一个关于data 特性，就是它支持递归式地声明：

```haskell
data List a = Empty | Cons a (List a) deriving (Show, Eq, Read, Ord)
```

### type

type 在我看来就是把一个或者多个定义好的data创建一个alias，那样子可以提高代码的可阅读
性。比如说String 的定义：

首先，Char 是一个已经定义好的 data，然后在正常认知上 String 是一个由 Char 组成的 List。
那么

```haskell
type String = [Char]
```

再举一个例子，如果我们要给一组二维向量定义模型，里面的坐标点x 和y 可以简单地由一个
pair 来定义，那么我就会把这个坐标点抽象成：


```haskell
type Point = (Int, Int)
```

紧接着，任何一个图都是由一系列坐标成的数组，那么一个简单的图就可以用下面的定义来
表示：

```haskell
type Vec = [Point]
```

当当当然，既然 data 可以有类型参数，那么type 的定义也可以有类型参数啦，比如说上面
的Point 就可以有下面这样子的“泛型”定义：

```hashell
type Point t = (t, t)
```

### typeclass

Typeclass 就是一个定义了一些行为的接口，比如说Eq，它的tyleclass 的定义是这样子的：

```haskell
class Eq a where
  (==) :: a -> a -> Bool
  (/=) :: a -> a -> Bool
  x == y = not (x /= y)
  x /= y = not (x == y)
```

上面定义的 Eq 接口其实是已经实现了`==` 的默认行为，在定义自己的 typeclass 的时候可以
选择不实现上面的任何默认行为。

假设我们定义一个自己的数据类型 TrafficLight，那样的话我们可以对应的让这个类型能够
使用 Eq 的行为：

```haskell
data TrafficLight = Yellow | Green | Red


instance Eq TrafficLight where
  Red == Red = true
  Green == Green = true
  Yellow == Yellow = true
  _ == _ = false

instance Show TrafficLight where
  show Red = "RED LIGHT"
  show Green = "GREEN LIGHT"
  show Yellow = "YELLOW LIGHT"
```

由于已经有了默认函数 `(\=)` 的实现了，所以我们只用实现 `(==)`，那样的话就等于说我们只
需要实现关键方法以后，我们就可以免费获得整个 typeclass 里面的所有方法了。

### Sub-typeclass


