
## java.sql

提供了java语言访问处理存储在数据源中数据的api。

该api依靠不同的驱动访问不同的数据源。

JDBC除了传递SQL语句给数据库，还提供了从表格式数据源中读写数据的能力。

读写工具可以通过javax.sql.RowSet接口组中获取，可以自定义以使用表格数据。

### java.sql内容

通过DriverManager工具与数据库建立连接
- DriverManager类，与驱动建立连接
- SQLPermission类，当代码运行在安全管理器下时，提供权限。
- Driver接口，提供api，用于注册链接基于JDBC技术的驱动，通常只被DriverManager使用
- DriverPropertyInfo，为JDBC驱动提供属性，不被一般用户使用

向数据库发送SQL请求
- Statement，用于发送基础SQL语句

遍历更新查询结果
- ResultSet接口

java语言中对SQL类型的标准映射
- Array接口，映射SQL ARRAY

定制SQL用户自定义类型与java语言类的映射
- SQLData接口，指定UDT与此类实例的映射

元数据
- DatabaseMetaData接口，提供有关数据库的信息

异常
- SQLException



## Class Timestamp


封装java.util.Date用于表示SQL TIMESTAMP值。

通过指定小数秒为纳秒精度，添加了持有SQL TIMESTAMP小数秒值的能力。

Timestamp提供了格式化解析操作，支持JDBC的时间戳值的转义语法。

Timestamp对象的精度计算：
- 19，yyyy-mm-dd hh:mm:ss
- 20 + s，yyyy-mm-dd hh:mm:ss.[fff...]，s表示给定时间戳的比例，其小数秒精度

该类型是java.util.Date与独立纳秒值组成。只有整数秒存储在java.util.Date组件中。

Timestamp.equals(Object)方法与java.util.Date.equals(Object)方法并不对成。

hashCode方法使用了底层java.util.Date实现。

推荐代码不要将Timestamp值看作java.util.Date实例。

Timestamp与java.util.Date之间的继承关系只表示实现继承，而不是类型继承。



## package javax.sql

module java.sql

提供java语言访问处理服务器端数据源的api。

该包补充java.sql包，1.4之后，包括进java平台。

javax.sql包提供
- DataSource接口
- 连接池与语句池
- 分布事务
- Rowset

应用可以直接使用DatSrouce与RowSet api，但是连接池与分布式事务api用于中阶层内部。

### 使用DataSource建立连接

DataSource机制比DriverManager机制更好。

- 可以对数据源属性进行修改，这意味着，当数据源或驱动改变时，无需改变代码
- 可以通过DataSource对象使用连接池与语句池
