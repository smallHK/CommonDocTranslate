# class文件格式

每个class文件包括单独类、接口或模块的定义。

class文件由8位字节流构成。16位与32位通过分别读取两个或四个8位字节流构成。多字节数据项中，大的字节存储在前。u1、u2与u4分别表示无符号1、2或4字节。

Java SE平台API中，class文件格式接口java.io.DataInput与java.io.DataOutput支持。例如，java.io.DataInput可以通过readUnsignedByte、readUnsignedShort与readInt读取u1、u2、u4。

class文件中的项在字节流中连续存储，中间没有空格。

由零个或多个可变大小的项组成的表，用于多个class文件结构。

## Class文件结构

```
ClassFile {
    u4             magic;
    u2             minor_version;
    u2             major_version;
    u2             constant_pool_count;
    cp_info        constant_pool[constant_pool_count-1];
    u2             access_flags;
    u2             this_class;
    u2             super_class;
    u2             interfaces_count;
    u2             interfaces[interfaces_count];
    u2             fields_count;
    field_info     fields[fields_count];
    u2             methods_count;
    method_info    methods[methods_count];
    u2             attributes_count;
    attribute_info attributes[attributes_count];
}
```

magic  
magic项标识class文件，值为0xCAFEBABE

minor_version、major_version  
minor m与major M标识class文件版本，M.m

constant_pool_count
常量池项的个数+1
大于零，小于cpc的索引被认为合法

constant_pool[]
表示不同字符串常量、类与接口名称、字段名称以及其他被Class文件接口引用或其子结构引用的常量。

access_flags
用于标识访问权限以及类或接口的属性
ACC_INTERFACE、ACC_ENUM、ACC_ANNOTATION、ACC_MODULE，接口、枚举、注解、模块
ACC_SYNTHETIC表示类或接口由编译器生成。

this_class
引用常量池中的CONSTANT_Class_Info结构

super_class
为零或常量池合法索引。
引用常量池的项为CONSTANT_Class_Info结构
表示类的直接超类
如果为0，表示Object
接口的super_class表示Object

interfaces_count
表示接口数量

interfaces[]
每一个元素都为一个对常量池表的合法引用
每个项都为CONSTANT_Class_Info
表示从左到右的接口

fields_count表示字段数量
fields中field_info的数量，表示所有的字段，包括类变量与实例变量，被类或接口生命。

fields[]
fields中每个值都为field_info结构
只包括当且类声明的字段，不包括从接口或类继承而来的字段

methods_count
methods中method_info的数量

methods[]
每一项为method_info
method_info包括所有声明的方法，包括实例方法、类方法、实例初始化方法以及类或接口的类初始化方法。
不包括继承的方法。

attributes_count表示属性数量
attributes_count给出了attributes表中的属性数量。

attributes[]

attributes表中的每一项都必须为attribute_info结构。


## 名称

### 二进制类名与接口名

出现在class文件结构中的类与接口的名称总是表示为二进制名称。类名与接口名以CONSTANT_Utf8_info结构表示。

类与接口的名称通过CONSTANT_NameAndType_info结构引用，用于表示描述符的一部分，并且来自所有的CONSTANT_Class_info结构。

由于历史原因，class文件结构中的二进制名称语法与JLS中的二进制名称语法不同。在内部形式，ASCII(/)通常用于分隔标识符。标识符自身必须为非限制名称。

例如，java.lang.Thread在class文件中的CONSTANT_Utf8_info中，使用java/lang/Thread表示。

### 非限定名称

方法、字段、本地变量以及形参都存储为非限定名。非限定名最少包括一个Unicode点，不能包括任何ASCII字符.;[/。

除非特殊名称，<init>与<clinit>，方法名不能包括尖括号。

字段名与接口方法名可以为<init>或<clinit>，没有方法调用指令可以引用<clinit>方法，只有invokespecial可以引用<init>方法。


### 模块名与包名




## 描述符
描述符为表示字段或方法类型的字符串。class文件使用修改的utf-8字符串表示描述符。

字段描述符表示类、实例或局部变量的类型。

方法描述符，通过参数描述符与返回描述符组成。




## 常量池

jvm指令并不依赖类、接口、类实例与数组的运行时分布，指令引用constant_pool中的符号信息。




常量池中所有项具有通用格式：
cp_info {
    u1 tag;
    u1 info[];
}

constant_pool表中的每个条目必须以1byte标记指示该条目的常量类型。一共存在17种常量。每个标记byte必须跟随两个或多个子接给定有关特定常量的信息。依赖于tag字节的额外信息的格式，即info数组的内容根据tag的取值变化。



info数组内容随着tag的值变化。


一些常量池中的项是可载入的，这些常量可以在运行时被押入栈，进行进一步计算。


### CONSTANT_Class_info结构

CONSTANT_Class_info用于表示类或接口。

CONSTANT_Class_info {
    u1 tag;
    u2 name_index;
}

tag为7

name_index的值必须为constant_pool表中的合法索引值。该索引的条目必须为表示以内部格式表示的合法二进制类或接口名的CONSTANT_Uft_8_info结构。

因为数组为对象，所以anewarray与multianewarray可以通过constant_pool表中的CONSTANT_Class_Info引用数组“类”。

对于每个数组类，类名为数组类的描述符。



### CONSTANT_Integer
- tag 3
- byte u4


### CONSTANT_String_info
CONSTANT_String_info {
    u1 tag;
    u2 string_index;
}
tag为8
index为对CONSTANT_Utf8_info的引用。


### CONSTANT_Fieldref，CONSTANT_Methodref，CONSTANT_InterfaceMethodref

字段、方法与接口方法使用相似的结构表示。

CONSTANT_Fieldref_info {
    u1 tag;
    u2 class_index;
    u2 name_and_type_index;
}

CONSTANT_Methodref_info {
    u1 tag;
    u2 class_index;
    u2 name_and_type_index;
}

CONSTANT_InterfaceMethodref_info {
    u1 tag;
    u2 class_index;
    u2 name_and_type_index;
}


### CONSTANT_NameAndType

用于表示字段或方法
CONSTANT_NameAndType_info {
    u1 tag;
    u2 name_index;
    u2 descriptor_index;
}


### CONSTANT_Utf8_info

用于表示字符串常量值。
CONSTANT_Utf8_info {
    u1 tag;
    u2 length;
    u1 bytes[length];
}

字符串内容使用已修改的UTF-8进行编码。


()I，表方法示无参返回int类型值的描述
()Ljava/lang/Object，表示方法返回Object类型值的描述
copmareTo，表示方法的名称


### CONSTANT_MethodHandle_info

表示一个方法句柄
CONSTANT_MethodHandle_info {
    u1 tag;
    u1 reference_kind;
    u2 reference_index;
}
reference_kind表示方法句柄的类型。
reference_index表示对常量池的索引。
当kind为1~4时，index指向CONSTANT_Fieldref_info结构，表示方法句柄建立的字段。
当kind为5、8时，index指向CONSTANT_Methodref_info结构，表示方法句柄建立的方法或构造器。
如果kind为8，必须为<init>的CONSTANT_Methodref_info结构。


### CONSTANT_Dynamic_info与CONSTANT_InvokeDynamic_info
通过直象计算实体的代码，并不直接表示实体。
被指向的代码称为启动方法，有jvm在解析符号引用时调用。
每个结构指定一个启动方法以及一个辅助名称与类型定义被计算的实体。
CONSTANT_Dynamic_info {
    u1 tag;
    u2 bootstrap_method_attr_index;
    u2 name_and_type_index;
}
CONSTANT_Dynamic_info用于表示动态计算常量。
辅助类型为动态计算常量的类型。

CONSTANT_InvokeDynamic_info {
    u1 tag;
    u2 bootstrap_method_attr_index;
    u2 name_and_type_index;
}
CONSTANT_InvokeDynamic_info表示动态计算调用位置
java.lang.invoke.CallSite产生于invokedynamic指令过程中的启动方法调用。
辅助类型为动态计算调用点的方法类型。

bootstrap_method_attr_index
该值为对class文件bootstrap_methdos表中项的合法索引。

### CONSTANT_Module_info

```
CONSTANT_Module_info {
    u1 tag;
    u2 name_index;
}
```

### CONSTANT_Package_info






## 字段
字段被field_info结构描述
field_info {
    u2             access_flags;
    u2             name_index;
    u2             descriptor_index;
    u2             attributes_count;
    attribute_info attributes[attributes_count];
}


access_flags
ACC_PUBLIC、ACC_PRIVATE、ACC_PROTECTED
ACC_STATIC、ACC_FINAL、ACC_VOLATILE、TRANSTENT
ACC_ENUM
ACC_SYNTHETIC
接口字段，必须public、static、final
synthetic字段由编译器生成


## 方法
method_info描述方法，包括实例初始化方法以及类或接口初始化方法。
相同的class文件中，不能存在相同的名称与描述符。

method_info {
    u2             access_flags;
    u2             name_index;
    u2             descriptor_index;
    u2             attributes_count;
    attribute_info attributes[attributes_count];
}

access flag



## 属性


属性用于class文件的ClassFile、field_info、method_info、Code_attribute结构。

```
attribute_info {
    u2 attribute_name_index;
    u4 attribute_length;
    u1 info[attribute_length];
}
```

对于所有的属性，attribute_name_index必须为只想常量池的无符号16位索引。位于此处的constant_pool必须为CONSTANT_Utf8_info结构。attribute_length表示接下来信息的字节长度。


一共定义了28种属性。


位于Class文件的位置：
SourceFile、InnerClasses、Enclosing、SourceDebugExtension、BootstrapMethods
Module、ModulePackages、ModuleMainClass
NestHost、NestMembers

位于field_inf的位置：
ConstantValue

位于method_info的位置：
Code、Exceptions
RuntimeVisibleParameterAnnotations、RuntimeInvisibleParameterAnnotations
AnnotationDefualt
MethodParameters

位于Class文件、fields_info、method_info的位置：
Synthetic、Deprecated、Signature
RuntimeVisibleAnnotatios、RuntimeInvisibleAnnotations

位于Code:
LineNumberTable
LocalVariableTabe、LocalVariableTypeTable
StackMapTable

位于Class文件、field_info、method_info、Code的位置：
RuntimeVisibeTypeAnnotations、RuntimeInvisibleTypeAnnotations




六种属性对于jvm解释class文件极为重要
ConstantValue
Code
StackMapTable
BootstrapMethods
NestHost
NestMembers

九种属性对于JVM通过Java se平台类库解释class文件至关重要，但是有些属性为可选
Exception
InnerClasses
EnclosingMethod
Synthetic
Signature
SourceFile
LineNumberTable
LocalVariableTable
LocalVariableTypeTable

十三种属性对于jvm解释class文件不是很重要，但是包括class文件的元信息，可以被java se平台使用。
SourceDebugExtension
Deprecated
RuntimeVisibleAnnotations
RuntimeInvisibleAnnotations
RuntimeVisibleParameterAnnotations
RuntimeInvisibleParameterAnnotations
RuntimeVisibleTypeAnnotations
RuntimeInvisibleTypeAnnotations
AnnotationDefault
MethodParameters
Module
ModulePackages
ModuleMainClass


### Code
Code属性是method_info结构中attributes表的项。
Code属性包含jvm方法的指令与辅助信息，方法包括实力初始化方法与类或接口初始化方法。
如果方法为native或abstract，并且不是类或接口的初始化方法，则方法method_info必须不包含Code属性。否则，method_info结构必须具有一个Code属性表。
Code_attribute {
    u2 attribute_name_index;
    u4 attribute_length;
    u2 max_stack;
    u2 max_locals;
    u4 code_length;
    u1 code[code_length];
    u2 exception_table_length;
    {   u2 start_pc;
        u2 end_pc;
        u2 handler_pc;
        u2 catch_type;
    } exception_table[exception_table_length];
    u2 attributes_count;
    attribute_info attributes[attributes_count];
}

attribute_name_index
指向CONSTANT_Utf8_info结构，结构值为"Code"


BootstrapMethods属性
BootstrapMethods属性是ClassFile结构中attributes表中一个长度可变属性。
BootstrapMethods属性记录了用于产生动态计算常量与动态计算调用位置的启动方法。
当ClassFile结构的constant_pool表中具有至少一个CONSTANT_Dynamic_info或CONSTANT_InvokeDynamic_info项时，必须存在一个BootstrapMethods属性。
attributes表中最多只有一个BootstrapMethods表。
BootstrapMethods_attribute {
    u2 attribute_name_index;
    u4 attribute_length;
    u2 num_bootstrap_methods;
    {   u2 bootstrap_method_ref;
        u2 num_bootstrap_arguments;
        u2 bootstrap_arguments[num_bootstrap_arguments];
    } bootstrap_methods[num_bootstrap_methods];
}
bootstrap_methods表中每一个项都包含一个对CONSTANT_MethodHandle_info结构的索引
该方法句柄制定一个启动方法以及启动方法一系列的静态参数。
attribute_name_index指向表示"BootstrapMethods"的CONSTANT_Utf8_info结构。
num_bootstrap_methods表示bootstrap_methods数组的长度。
bootstrap_method_ref必须指向一个CONSTANT_MethodHandle_info结构。
num_bootstrap_arguments给定bootstrap_arguments数组的长度。
bootstrap_arguments的元素必须为对constant_pool中可载入元素的索引。


Deprecated
- attribute_name_index u2
- attribute_length u4

attribute_name_index
- 指向包含Deprecated的UTF-8常量
attribute_length
- 为0

ConstantValue
- attribute_name_index u2
- attribute_length u4
- constantvalue_index u2

attribute_name_index
- 包含ConstantValue字面量的UTF-8索引
attribute_length
- 为2
constantvalue_index
- 常量池，指向常量池，可以是UTF-8，Float，Double等





LineNumberTable

表示字节码与行数的对应关系


LocalVariableTable - Code属性的属性
表示局部变量表描述
index表示此局部变量在Slot中的位置，局部变量的Slot可以在方法体中被重用
如果之前的局部变量已经在作用域之外，后面的局部变量重用前面的局部变量以节省空间




Exceptions属性
- 和Code属性平级
- 表示方法抛出的异常（不是try catch部分，而是throws部分），与code属性中的Exceptions不同

- 结构
- attribute_name_index u2
- attribute_length u4
- number_of_exception u2
- exception_index_table[number_of_exceptions] u2
- 指向Constant_Class的索引






SourceFile
- 描述生成Class文件的源码文件名称

- 结构
- attribute_name_index u2
- attribute_length u4
- 固定为2
- source_index u2
UTF-8常量索引
