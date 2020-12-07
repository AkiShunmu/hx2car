# [hx2car](http://www.hx2car.com)

## 项目环境

- 拉去项目
  - git clone git@10.90.60.209:web/hx2car.git --recurse-submodules
- 配置文件
  - local 本地开发环境、229环境
  - dev 测试环境
  - pro 线上环境
- 启动脚本
  - service.sh
  - 使用方式
    - ./service.sh dev restart pubnet-service(脚本 配置文件 启动方式项目名称)

## Modules

* doc  存放说明文档
* script    存放可执行的脚本文件
* hx-common 公共依赖
* hx-core   华夏核心包，暂时用来解决项目之间依赖
* mongodb-service   提供长数据存储服务，mongodb doris
* pubnet-service    提供对外访问服务
* scheduler-service 提供任务执行服务
* send-service  提供消息服务
* soul  网关
* buyCars-service 买车工程，提供对外暴露接口和买车服务
* vehicle-service   车辆工程，提供对外车辆暴露接口和车辆服务
* saleCars-service  卖车工程，提供对外卖车暴露接口和卖车服务

## Project structure

* base 该包下存放所有基础类
* dal 该包下提供所有数据获取的底层操作，包括不限于数据库、缓存、es
* config 该包下存放所有配置相关的
    * sharding 分库分表的相关配置
* service 该包下提供本工程相关服务
* open-service 该包下提供本工程对外暴露的接口
* bean 
    * po (persistent object) 持久对象。与数据库里表字段一一对应。PO是一些属性，以及set和get方法组成。一般情况下，一个表，对应一个PO。是直接与操作数据库的crud相关。
    * vo (view object) 通常用于业务层之间的数据传递，和PO一样也是仅仅包含数据而已。但应是抽象出的业务对象，可以和表对应，也可以不，这根据业务的需要。对于页面上要展示的对象，可以封装一个VO对象，将所需数据封装进去。
    * bo (bussiness object) 业务对象。封装业务逻辑的 java 对象 , 通过调用 DAO 方法 , 结合 PO,VO 进行业务操作
    * dto (data trasfer object)数据传输对象。主要用于远程调用等需要大量传输对象的地方。
* util 存放所有的工具类
* manage 通用业务处理层，它有如下特征：
    * 对第三方平台封装的层，预处理返回结果及转化异常信息；
    * 对service层通用能力的下沉，如换成方案、中间件通用处理；
    * 与dal层交互，对多个dao的组合使用。

## Coding specification

* 使用linux换行符。
* 缩进（包含空行）和上一行保持一致。
* 类声明后与下面的变量或方法之间需要空一行。
* 不应有无意义的空行。请提炼私有方法，代替方法体过长或代码段逻辑闭环而采用的空行间隔。
* 类、方法和变量的命名要做到顾名思义，避免使用缩写。
* 返回值变量使用result命名；循环中使用each命名循环变量；map中使用entry代替each。
* 配置文件使用Spinal Case命名（一种使用-分割单词的特殊Snake Case）。
* 需要注释解释的代码尽量提成小方法，用方法名称解释。
* equals和==条件表达式中，常量在左，变量在右；大于小于等条件表达式中，变量在左，常量在右。
* 除了构造器入参与全局变量名称相同的赋值语句外，避免使用this修饰符。
* 除了用于继承的抽象类之外，尽量将类设计为final。
* 嵌套循环尽量提成方法。
* 成员变量定义顺序以及参数传递顺序在各个类和方法中保持一致。
* 优先使用卫语句。
* 类和方法的访问权限控制为最小。
* 方法所用到的私有方法应紧跟该方法，如果有多个私有方法，书写私有方法应与私有方法在原方法的出现顺序相同。
* 方法入参和返回值不允许为null。
* 优先使用三目运算符代替if else的返回和赋值语句。
* 优先考虑使用LinkedList，只有在需要通过下标获取集合中元素值时再使用ArrayList。
* ArrayList，HashMap等可能产生扩容的集合类型必须指定集合初始大小，避免扩容。
* 日志与注释一律使用英文。
* 注释只能包含javadoc，todo和fixme。
* 公开的类和方法必须有javadoc，其他类和方法以及覆盖自父类的方法无需javadoc。
* 通过checkStyle检查（强制），模板位置在hx2car\script\checkstyle.xml，请使用checkstyle 8.8运行

## Parameter validation

**open-service 必须开启参数效验 @Service(validation = "true")**

- 空检查
  - @Null 验证对象是否为null
  - @NotNull 验证对象是否不为null, 无法查检长度为0的字符串
  - @NotBlank 检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
  - @NotEmpty 检查约束元素是否为NULL或者是EMPTY.
- Booelan检查
  - @AssertTrue 验证 Boolean 对象是否为 true
  - @AssertFalse 验证 Boolean 对象是否为 false
- 长度检查
  - @Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内
  - @Length(min=, max=) Validates that the annotated string is between min and max included.
- 日期检查
  - @Past 验证 Date 和 Calendar 对象是否在当前时间之前
  - @Future 验证 Date 和 Calendar 对象是否在当前时间之后
  - @Pattern 验证 String 对象是否符合正则表达式的规则
- 数值检查
  - 建议使用在Stirng,Integer类型，不建议使用在int类型上，因为表单值为“”时无法转换为int，但可以转换为Stirng为”“,Integer为null
  - @Min 验证 Number 和 String 对象是否大等于指定的值
  - @Max 验证 Number 和 String 对象是否小等于指定的值
  - @DecimalMax 被标注的值必须不大于约束中指定的最大值. 这个约束的参数是一个通过BigDecimal定义的最大值的字符串表示.小数存在精度
  - @DecimalMin 被标注的值必须不小于约束中指定的最小值. 这个约束的参数是一个通过BigDecimal定义的最小值的字符串表示.小数存在精度
  - @Digits 验证 Number 和 String 的构成是否合法
  - @Digits(integer=,fraction=) 验证字符串是否是符合指定格式的数字，interger指定整数精度，fraction指定小数精度。
  - @Range(min=, max=) Checks whether the annotated value lies between (inclusive) the specified minimum and maximum.
  - @Range(min=10000,max=50000,message=”range.bean.wage”)
  - @Valid 递归的对关联对象进行校验, 如果关联对象是个集合或者数组,那么对其中的元素进行递归校验,如果是一个map,则对其中的值部分进行校验.(是否进行递归验证)
  - @CreditCardNumber信用卡验证
  - @Email 验证是否是邮件地址，如果为null,不进行验证，算通过验证。
  - @ScriptAssert(lang= ,script=, alias=)
  - @URL(protocol=,host=, port=,regexp=, flags=)