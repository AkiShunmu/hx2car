## 使用自定JPA相关接口的方法

- Repo接口统一继承BaseRepo
- Manage接口继承JpaBaseManage接口
- ManageImpl继承JpaBaseManageImpl类

## 原理

- JPA的数据查询时先通过工厂方法生成代理代理对象，然后调用代理对象的方法
- JPA自带的一些方法getOne、findAll等是通过SimpleJpaRepository
- 首先我们先写一个接口BaseRepo，继承JpaRepository、JpaSpecificationExecutor等JPA的接口，目的是方便以后对公用的JPA查询方法扩展
- 我们先写一个类BaseRepoImpl继承SimpleJpaRepository，同时实现BaseRepo接口
- 重写SimpleJpaRepository中相关的方法，比如findAll，我们可以在查询时给flag添加默认值0等等
- 然后我们自定义一个工厂类BaseRepoFactoryBean，通过这个工厂类生成BaseRepoImpl
- 最后通过EnableJpaRepositories指定Repo的代理生成工厂为我们自定义的工厂，这样我们就将原有的JPA的公用方法的实现类以及工厂类全部替换为了我们的自定义的类

## 扩展共有方法

- 现在BaseRepo接口中添加方法
- 然后在BaseRepoImpl中实现即可
- 所有继承了BaseRepo接口的Repo接口就都可以使用这个新增的公有方法了

## 注解使用

- QueryDefaul
    - 用于查询是给字段添加默认值
    - value
        - 字段默认值
    - isCompulsory（默认为false）
        - 等于true时，不管实体中该字段是否有传值，强制使用注解指定的值
        - 等于false时，如果实体中该字段有值，则使用该值，如果没有，则使用注解指定的默认值

## 案例

- flag默认值
    - 在flag字段上添加注解@QueryDefault(value = "0")
    - 通过共有方法查询时如果没有指定flag，则默认flag等于0，不会查出删除的数据
- 添加默认创建时间以及修改时间
    - 数据新增或者修改时，会自动给实体添加创建时间以及修改时间，不需要自己再手动传入