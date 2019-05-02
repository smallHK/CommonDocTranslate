
## java.sql

提供了java语言访问处理存储在数据源中数据的api。

该api依靠不同的驱动访问不同的数据源。

JDBC除了传递SQL语句给数据库，还提供了从表格式数据源中读写数据的能力。

读写工具可以通过javax.sql.RowSet接口组中获取，可以自定义以使用表格数据。




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
