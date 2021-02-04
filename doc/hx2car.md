# [hx2car](http://www.hx2car.com)

## soul

- 250:http://10.90.60.101:19095/#/home
- 221(本地):http://10.90.60.221:9095/#/home
- 线上:http://10.90.60.101:9095/#/home

## 打包步骤

- 以当前最新代码分支为0.2.19为例
- 将代码合并到master
- 从master剪出名字为0.2.20的分支
- 将分支中的pom的版本修改为0.2.20
- 打包分支代码，部署到私服

## Modules

- doc  存放说明文档
- script    存放可执行的脚本文件
- hx-common 公共依赖
- hx-core   华夏核心包，暂时用来解决项目之间依赖
- mongodb-service   提供长数据存储服务，mongodb doris
- pubnet-service    提供对外访问服务
- scheduler-service 提供任务执行服务
- send-service  提供消息服务
- soul  网关
- buyCars-service 买车工程，提供对外暴露接口和买车服务
- vehicle-service   车辆工程，提供对外车辆暴露接口和车辆服务
- saleCars-service  卖车工程，提供对外卖车暴露接口和卖车服务