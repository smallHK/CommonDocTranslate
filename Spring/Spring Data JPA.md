
## 定义仓库接口

定义域特定类仓库接口，接口必须扩展Repository，并且输入域类域ID类型。

如果想要获取CRUD放，可以扩展CrudRepository。

### 微调仓库定义

通常，仓库接口扩展Repository、CrudRepository、PagingAndSortingRepository。

如果不想扩展Spring Data接口，需要在仓库接口上设置@RepositoryDefinition接口。


中间层仓库应该使用@NoRepositoryBean，确保Spring Data不会在运行时创建实例。

### 使用具有多个Spring Data模块的仓库

如果应用使用多个Spring Data模块，应用需要区分不同的持久化技术。

当应用在classpath中检测到多个仓库工厂时，Spring Data会进入严格仓库配置模式。

严格配置使用仓库或域类上的详细信息，决定Spring Data与仓库定义的绑定。


- 仓库定义扩展了模块特定的仓库
- 域类使用了特定模块的类型注解


### 定义查询方法

仓库代理通过两种方法从方法名中派生特定存储的查询
- 方法名直接派生
- 手动定义查询

## JAP 仓库

### 查询方法

JPA模块支持使用字符串手动定义查询或从方法名派生查询。


### 查询创建
