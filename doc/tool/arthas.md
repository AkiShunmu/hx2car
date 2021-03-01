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
- trace
    - 打印方法内部的调用路径或者说调用过的每个方法的耗时时间