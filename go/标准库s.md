
## sql

import "database/sql"

sql提供了围绕SQL数据库的通用接口包。

sql包必须与数据库驱动联合使用。

不支持上下文取消的驱动在查询完成之前不会返回。


###  type DB

DB是表示连接池的数据库句柄。可以通过多个go协程并发安全使用。

sql包自动创建释放链接，并且维护一个空闲链接池。

如果数据库具有预链接状态的概念，类似的状态可以在事务（Tx）或链接（Conn）中被可靠观察到。

一旦DB.Begin被调用，返回地Tx被绑定在单个链接上。

一旦事物的Commit或Rollback被调用，事务的链接将会被返回给DB的空闲连接池。

可以通过SetMaxIdleConns控制链接池大小。



#### func (*DB) Query

```
func (db *DB) Query(query string, args ...interface{}) (*Rows, error)
```
Query执行查询，返回行。

args为查询中的占位符参数。

### type RawBytes
RawBytes为保存数据库自身持有的内存的引用的字节切片。

```
type RawBytes []byte
```


### type Rows

Rows是查询的结果。游标开始于结果的第一行。


#### func (*Rows) Columns
```
func (rs *Rows) Columns() ([]string, error)
```
Columns返回列名。

如果rows已经关闭，Columns返回一个error。


#### func(*Rows) Next

```
func (rs *Rows) Next() bool
```

Next为Scan方法准备下一个被读取的结果行。

成功时返回true，如果出现错误或没有下一行，将会返回false。

应该通过Err区别错误和没有下一行的情况。

每一次调用，甚至是第一次调用之前，都必须先调用Next。





#### func (*Rows) Scan
```
func (rs *Rows) Scan(dest ...interface{}) error
```
Scan复制当前行到被dest被指向的值。

dest中值的数量必须与Rows中的列的数量相同。


## fmt

import "fmt"

实现了格式化IO，类似于C的printf与scanf。

> %s 字符串或切片的未解释字节流



### finc Sprint

```
func Sprint(a ...interface{}) string
```
Sprint使用默认格式格式化操作数，返回生成的字符串。

### func Sprintf

```
func Sprintf(format string, a ...interface{}) string
```

根据格式说明符格式化，返回生成字符串。



## time

import "time"

time包提供了用于测量显示时间的函数。

日历计算假定为格里高利历，即公历，没有闰秒。

### Parse

```
func Parse(layout, value string) (Time, error)
```
Parse解析格式化的字符串，并返回其表示的时间。


当缺少时区时，Parse返回UTC时间。


### func ParseInLocation

```
func ParseInLocation(layout, value string, loc *Location) (Time, error)
```

ParseInLocation解释时间为给定位置。


## strconv

import "strconv"

strconvy实现了基本数据类型与字符串表示之间的转化


### func ParseInt

```
func ParseInt(s string, base int, bitSize int) (i int64, err error)
```
ParseInt使用给定基地base与给定的位数解释字符串s，返回对应的值。

如果base==0，默认字符串携带了基地的前缀。


bitSize参数用于指定结果蛮子的整数类型。

### func ParseUint
类似于ParseInt，但是用于转化无符号数字
