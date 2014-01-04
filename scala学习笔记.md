## scala的入门（“第一步”/“下一步”章节笔记）
### 定义变量
- val和var
- 变量写后面（方面类型推断）

### 定义函数
- def 函数名(形参1 : 形参1类型, 形参2 : 形参2类型) : 返回类型 = {函数体}
- 如果函数体是一句话可省略大括号
- 可不写返回类型，但建议写上，增强可读性
- 如果定义无入参和无返回值的函数，repl会返回例如 函数名:()Unit 的定义，Unit和java里的void类似。这类函数就是为了其副作用而定义，如在标准输出上打印一句话

### scala脚本
- scala用arg(0)来表示第一个启动参数，不像java里用arg[0]
- 注释和java一样，单行//，多行/**/

### 循环（while）/条件（if）
- java里的i++和++i都不生效，必须写成 i += 1 或者 i = i + 1
- 和java里一样，while和if的条件必须放在括号里，如果{}里只有一条语句，花括号可以省略
- 和java里不一样，分号不是必须的

### 函数式风格的for枚举和foreach
- foreach可以这么用[^1]：

		args.foreach(arg => println(arg)) 或者
		args.foreach((arg: String) => println(arg)) 或者
		args.foreach(println)

- for (arg <- args) print(arg)里的arg是val

[^1]:对灵活性的思考：scala里很多东西都可以省略，如变量类型、返回值、调用的“.”、分号等等，这种灵活性是一种双刃剑，简洁或者灵活必定意味着语言本身的表达能力更强，是一种语言上的优势，但这对使用者也提出更高的要求，有更高的学习门槛，使用不当会更容易导致问题，这点可以算是一种“劣势”，但这并不意味着语言灵活本身有什么不对，关键在于使用者对灵活性的驾驭能力，这种驾驭能力的锻炼就是学习成本了

### 使用Array
- 和java一样，可以在val类型的数组实例化时使用参数赋值，之后数组引用不能再被赋值，但内部的元素可以随意改变，也即数组本身是可变的
- 数组可以用以下方式初始化

		val greetStrings = new Array[String](2)
		greetStrings(0) = "Hello"
		greetStrings(1) = "world"
也可以这样初始化

		val greetStrings = Array("Hello", "world")
后者是scala里推荐的方式，后者之所以不用new是因为它被转化成对半生对象的apply方法的调用
- 为何scala用()来访问数组而非[]，因为数组也是一种对象（类的实例），数组用()取值时就被转化为对apply方法的调用，赋值是则转化为对update方法的调用，array(3)即array.apply(3)，array(3) =  "three"即array.update(3,"three")
- scala没有操作符，=*-等符号其实是方法名，1+1等价于(1).+(1)，由于scala里规定方法只有一个入参时可以不带点或括号调用它，于是就等价于1+1这种写法了
- scala的这种统一性（这里指一切皆对象，一切皆方法调用）并不会影响性能，编译后均会转化成java数组和java的基本类型

### 使用List
- 与Array不同，scala里的List是不可变的（也有可变版本），这个与java是不同的
- List的初始化 val oneTwoThree = List(1, 2, 3) ，List不用new和Array不用new一样，是被转化成对apply方法的调用
- List里的方法调用都不会改变原有的List，如最常用的::操作（把新元素加到最前端）
- 如果一个方法被用作操作符标注,如 a * b,那么方法被左操作数调用,就像 a.*(b)——除非方法名以冒号结尾。这种情况下,方法被右操作数 调用。因此,1 :: twoThree里,::方法被twoThree调用,传入1,像这样:twoThree.::(1)

### 使用Tuple
- Array是共享**相同类型**的**可变**对象序列，List共享**相同类型**的**不可变**对象序列，Tuple是可以包含不**同类型**的**不可变**对象元组
- 元组一般用于方法需要返回多个返回结果的场景，此时返回元组比返回一个JavaBean要灵活的多，后者需要预先定义好一个用于封装返回结果的贫血对象（例如一个JavaBean），对象此时仅仅用于变量的封装
- 尽管理论上你可以创建任意长度的元组,然而当前 scala库仅支持到Tupe22
- val pair = (99, "Luftballons")定义元组后用println(pair._1)、println(pair._2)来访问，_N从1开始（参照了拥有静态类型元组的其它语言的传统，如Haskell和ML）
- 不用pair(0)是因为apply只返回相同类型的结果，而paire._1和pair._2返回可能是不同类型

### 使用Set/Map
- scala的容器类型均有可变不可变版本，就如Array可变List不可变，Set和Map也有可变不可变两个版本
- scala api用基本功能的trait与可变特性的trait和不可变特性的trait进行组合构造出可变和不可变两种版本（trait可译为特质，与java的接口类似，但可以包含实现，用于类型的组合，类型组合可以达到多重继承的效果，但又避免了菱形继承的问题）
- 不可变的Set和Map是默认的，如果需要使用可变的版本，可以用import显示引入，如import scala.collection.mutable.Map

### 函数式风格
