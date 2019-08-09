
## 包描述

多路复用、无阻塞IO比面向线程的阻塞IO更具有扩展性。selector、selectable channel、selection keys提供了无阻塞IO。

selector是可选择通道的多路复用器，同时也是可以进入非阻塞模式的特殊通道。为了执行多路复用操作，需要首先创建一个或多个可选择通道，设置进入无阻塞模式，将其注册在选择器上。通道被注册时，指定一组由选择器检测IO操作集，然后返回一个表示注册的选择键。


通道在选择器中注册之后，选择操作便可以用于查找已经准备好执行之前已经声明的感兴趣的操作的通道。如果通道已经准备好，那么注册时返回的键将会被添加进选择器的已选择键集合。可以通过检查该键集合与其中的键判断每个通道已经准备好的操作。从每个键可以用于检索对应的通道，用于执行需要做的IO操作。


选择键表示通道已经可以执行某些操作，但并不保证线程一定不会阻塞。需要编写多路复用IO代码，以便提示被证明不正确时忽略它们。

该报定义的可选择通道类对应DatagramSocket，ServerSocket与Socket类。这些类只有很少的改动用于支持关联通道的插头。该包还实现了简单的单向管道类。通过调用对应类的open静态方法可以创建新的可选择通道。如果通道需要关联socket，那么将会创建一个socket。


选择器、选择通道、选择键可以通过插入java.nio.channels.spi中的SelectorProvider的另一个定义或示例替换。并不推荐开发者使用该工具，只有当需要高性能时，由高级用户使用特定于操作系统的IO复用机制。

实现多路复用IO抽象的大多数记录与同步由java.nio.channels.spi包下的Abstract等类执行。当定义定制选择器提供者时，只有AbstractSelector与AbstractSelectionKey类应该被直接子类化，定制通道类需要扩展定义在java.nio.channels下适当的SelectableChannel子类。




## SelectableChannel

通过Selector进行多路传输。

## SelectionKey

表示SelectableChannel在Selector上注册的token。

每当通道在选择器上注册时，一个选择键将被创建。

调用键的cancel()方法、关闭通道、关闭选择器，将会令键无效。

选择键包含两组使用整数值表示的操作集。操作集的每一位表示此键通道支持的selectable操作类别。

interest set决定下一次选择器选择方法调用时检查的准备状态的操作类别。

此类定义了所有已知的操作集bit，但是具体支持的bit依赖于通道类型。

对通道不支持的操作执行测试-设置操作将引发运行时异常。

选择键并发线程安全。选择操作总是使用操作开始时的interest-set值。

## Selector

表示SelectableChannel对象的多路复用器。  

选择器通过open方法创建，此方法使用系统默认的选择器提供者创建新的选择器。选择器可以通过调用定制选择器提供者的openSelector方法创建。

通过选择器的close方法，可以关闭选择器。

选择器维护三组选择键。  
key set、selected-key set、cancelled-key set


选择操作   
选择操作查询底层操作系统，关于注册通道的准备状态的更新。