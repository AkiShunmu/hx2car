## API接口返回体

- 必须返回Result

## API请求体的封装与使用

- 必须继承基类BaseVo
- 怎么获取请求头中的数据
    - 所有HTTP请求中的请求经过**SOUL**网关时全部回转存到BaseVo中的**headers**中
    - 在**BaseVo**中定义对应的**HeaderName**枚举类型
    - 通过**getHeader**方法获取
- 分页查询参数
    - 禁止自己写pageSize、currentPage在vo中，必须在VO中添加**PageVo**参数，通过PageVo传递分页参数
    - 从API向service传递分页参数时，建议通过PageVo传参

## API常用注解

- CheckLogin
    - 作用
        - 用于判断用户是否登陆
    - 使用方式
        - 在需要登陆的接口上添加注解即可
    - 实现方式
        - 从header中获取token，先判断token是否过期
        - 如果没有过期再判断该用户的状态（正常、注销、账号异常、已注销）
        - 具体实现看**CheckLoginAspect**类
- TryException
    - 作用
        - 用于异常捕捉
    - 使用方式
        - 在对应的方法上添加注解
        - 添加项目的Operate类中添加对应的操作类型，复制给注解的operate
    - 实现方式
        - 通过环绕Aspect实现，如果执行方法过程中出现异常，回包装成一个异常Result返回
        - 具体实现看**ServiceExceptionAspect**

## API接口请求日志打印

- 实现方式
    - 通过Aspect实现日志打印（**LogAspect**）
    - 通过**spring.factories**实现自动加载