## 简介

- Java诊断工具
- 官方文档：https://arthas.aliyun.com/doc/

## 注意：使用完以后使用stop结束

## 基础

- arthas的命令比较多，推荐下载idea插件生成对应的命令：搜索arthas即可

- 所在文件夹：/app/soft/app/arthas

- 启动方式：java -jar arthas-boot.jar --telnet-port 端口号 --http-port -1

- 每个服务所使用的端口号

    - | 服务名                      | 使用的端口 |
          | :-------------------------- | ---------- |
      | hx2car-api-buyCars          | 41887      |
      | hx2car-api-saleCars         | 41885      |
      | Hx2car-api-vehicle          | 41886      |
      | hx2car-api-carButterfly     | 41889      |
      | hx2car-service-buyCars      | 40887      |
      | hx2car-service-saleCars     | 40885      |
      | hx2car-service-vehicle      | 40886      |
      | Hx2car-service-weibao       | 40888      |
      | hx2car-service-carButterfly | 40889      |
      | hx2car-service-send         | 40883      |
      | hx2car-service-pubnet       | 40881      |
      | Hx2car-service-mongodb      | 40882      |
      | hx2car-scheduler            | 40884      |

## 常用操作

- dashboard
    - JVM数据面板
- watch
    - 监控方法，可以打印指定方法的入参、返回结果、异常信息
    - watch 包名.类名 方法名 '{params[0].{secret},returnObj,throwExp}' -v -n 2 -x 3 'params[0].{code}[0]=="1234"'
        - {params[0].{secret}：打印方法参数列表中的第一个参数的secret字段
        - returnObj：打印返回结果
        - throwExp：打印异常信息
        - v：打印更多信息
        - n：执行次数
        - x：结果遍历深度
        - 过滤条件：
- trace
    - 打印方法内部的调用路径或者说调用过的每个方法的耗时时间
- ognl
    - 调用静态方法
        - sc -d 包名.类名 获取类加载器的hash地址classLoaderHash
        - ognl classLoaderHash '@包名.类名@方法名(方法参数)'
    - 通过获取SpringBean调用对应的非静态方法
        - sc -d 包名.类名 获取类加载器的hash地址classLoaderHash
        - ognl -c classLoaderHash '#context=@包名.SpringUtil@getBean("beanName"),#context.方法(参数)'
- tt
    - 记录案发现场
    - 步骤
        - tt -t 包名.类名 方法名 记录该方法的所有调用
        - tt -l 列出所有调用记录
        - tt -s 'method.name=="primeFactors"' 根据方法名筛选
        - tt -i 1000 查看调用信息
        - tt -i 1000 -p 重新请求