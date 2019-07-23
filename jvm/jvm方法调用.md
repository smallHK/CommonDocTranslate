
## invokedynamic

调用动态计算调用点。


```
invokedynamic
indexbyte1
indexbyte2
0
0
```

## invokeinterface

调用接口方法

```
invokeinterface
indexbyte1
indexbyte2
count
0
```

操作数栈：
...,objectref,[arg1,[arg2 ...]]

### 描述

indexbyte1与indexbyte2用于构建到当前类的运行时常量池的索引，值为(indexbyte1 << 8) | indexbyte2。

位于该索引的运行时常量池必须为引用接口方法的符号引用，该条目给定了接口方法的名称与描述符，以及接口方法被发现的接口的符号引用。指定的接口方法需要被解析。

count操作数为不为零的无符号字节。

令C为objectref的类。方法根据C与其解析方法选出，此方法为被调用的方法。


## invokespecial

调用实例方法，直接调用实力初始化方法以及该类与其超类的方法。


## invokestatic

调用类（static）方法。



## invokevirtual

基于类进行分发，调用实例方法。

```
invokevirtual indexbyte1 indexbyte2
```

无符号indexbyte1与indexbyte2用于构建当前类运行时常量池的索引，该索引的值为(indexbyte1 << 8) | indexbyte2。运行时常量池中位于该索引的实体必须为对方法的符号引用，该符号引用给出了方法的名称与描述符以及对方法被发现的类的符号引用。指定的方法已经被解析。


