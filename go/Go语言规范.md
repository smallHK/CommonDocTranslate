
## 介绍


GO语言规范是GO语言的参考手册。

Go为用于系统编程的通用语言。

强类型、垃圾回收并且显式支持并发编程。

程序由package构建。

## 编辑

使用EBNF定义语法。

```
Production  = production_name "=" [ Expression ] "." .
Expression  = Alternative { "|" Alternative } .
Alternative = Term { Term } .
Term        = production_name | token [ "…" token ] | Group | Option | Repetition .
Group       = "(" Expression ")" .
Option      = "[" Expression "]" .
Repetition  = "{" Expression "}" .
```

```
|   alternation
()  grouping
[]  option (0 or 1 times)
{}  repetition (0 to n times)
```


小写production用于标识词汇token。

## 类型



### 切片类型
切片是对底层数据连续段的描述符，可以提供对数组中编号元素序列的访问。

切片类型表示其元素类型的数组的所有切片的集合。

元素的数量被称为切片的长度，永远不为负值。

未初始化的切片值位nil。

```
SliceType = "[" "]" ElementType
```

内置函数len可以获取切片长度。

切片的长度可以在执行时改变。

给定元素的切片索引可能小于底层数组中相同元素的索引。

每个初始化的切片都会关联一个持有元素的底层数组。

切片与底层数组共享存储。

数组底层的切片可能扩展切片的末端。

capacity是切片的长度与数组超出切片的长度之和。

可以通过从原有切片中切出新片，创建长度达到容积的切片。

可以使用cap(a)获取切片的capacity。

可以使用内置函数make创建一个给定类型的新的初始化的切片。make创建的切片会分配一个新的隐藏的数组。

数组的数组，内部的数组总是具有相同的长度，切片的切片或切片的数组，内部长度可以动态改变。内部的切片必须单独初始化。



### 函数类型

函数类型表示具有相同参数与返回值类型的所有函数的集合。

函数签名中最后传入的参数可以具有前缀...，此时的函数称之为variadic，并且可以被另个或多个参数值调用。


### interface类型

接口类型指定方法集合称之为接口。

接口类型的变量可以存储携带接口方法集超集的任意类型的值。

这种类型被称为实现接口。

未初始化的接口类型变量值为nil。

```
InterfaceType      = "interface" "{" { MethodSpec ";" } "}"
MethodSpec         = MethodName Signature | InterfaceTypeName
MethodName         = identifier
InterfaceTypeName  = TypeName
```

类型可以实现任意数量的接口。



### map类型

map是为排序的一种类型元素被另一种类型元素索引的元素。

未初始化的map为nil。

```
MapType     = "map" "[" KeyType "]" ElementType
KeyType     = Type
```

比较操作符==与!=必须被key类型定义，因此key类型不能为函数、map或切片。

map元素的数量为map的长度。

元素可以通过复制添加，通过索引表达式检索，可以通过delete内置函数删除。

新的空map类型可以使用内置函数make创建。

nil映射等于空映射，但是不能添加元素。


## 声明与范围

### 变量声明

变量声明创建一个或多个变量，并为其绑定对应的标识符，给定类型与初始值。

```
VarDecl     = "var" ( VarSpec | "(" { VarSpec ";" } ")" )
VarSpec     = IdentifierList ( Type [ "=" ExpressionList ] | "=" ExpressionList )
```

### 函数声明

函数声明将标识符与方法绑定。

```
FunctionDecl = "func" FunctionName Signature [ FunctionBody ] .
FunctionName = identifier .
FunctionBody = Block .
```


## 表达式

### 类型断言

断言接口类型的表达式x不会nil，并且存储在x中的值为T类型。
```
x.(T)
```
如果T并非接口类型，x.(T)断言x的动态类型与T相同


类型断言可以用于复制或初始化，并且生成一个额外的布尔值。
```
v, ok = x.(T)
v, ok := x.(T)
var v, ok = x.(T)
var v, ok T1 = x.(T)
```


### 将参数值传递给...参数



### 转化

转化将表达式的类型转为指定类型。转化可以包含在源码中，也可能被表达式出现的上下文隐含。

```
Conversion = Type "(" Expression [ "," ] ")"
```

非常常量值转化的条件：
- x可以赋值给T
- x的类型与T具有相同的底层类型
- x的类型与T都为整数类型或浮点数类型
- x的类型与T都是复杂类型





## 语句

语句控制执行


### if 语句

```
IfStmt = "if" [ SimpleStmt ";" ] Expression Block [ "else" ( IfStmt | Block ) ]
```

SimpleStmt在Expression计算之前执行。




### 赋值语句

```
Assignment = ExpressionList assign_op ExpressionList
```
### defer语句

### for语句

```
ForStmt = "for" [ Condition | ForClause | RangeClause ] Block
Condition = Expression
```





## 内置函数

内置函数为预定义的函数。


### make()

make用于创建slice、map或channel。
