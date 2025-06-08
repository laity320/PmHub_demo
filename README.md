
<h3 align="center">智维项目，一个基于 SpringCloud 的智能项目管理系统</h3>

<hr/>

### 项目简介

智能项目管理系统，用户可以创建项目、指派任务，还可以分配角色进行权限管控，关联审批流程，做到项目的自动流程化管理。



### 代码结构

```
com.laigeoffer.pmhub     
├── pmhub-ui              // 前端框架 [1024]
├── pmhub-gateway         // 网关模块 [6880]
├── pmhub-auth            // 认证中心 [6800]
├── pmhub-api             // 接口模块
│       └── pmhub-api-system                          // 系统接口
│       └── pmhub-api-workflow                        // 流程接口
├── pmhub-base          // 通用模块
│       └── pmhub-base-core                           // 核心模块组件
│       └── pmhub-base-datasource                     // 多数据源组件
│       └── pmhub-base-seata                          // 分布式事务组件
│       └── pmhub-base-security                       // 安全模块组件
│       └── pmhub-base-swagger                        // 系统接口组件
│       └── pmhub-base-notice                         // 消息组件组件
├── pmhub-modules         // 业务模块
│       └── pmhub-system                              // 系统模块 [6801]
│       └── pmhub-gen                                 // 代码生成 [6802]
│       └── pmhub-job                                 // 定时任务 [6803]
│       └── pmhub-project                             // 项目服务 [6806]
│       └── pmhub-workflow                            // 流程服务 [6808]
├── pmhub-monitor             						  // 监控中心 [6888]                 
├──pom.xml                                            // 公共依赖
```


### 项目亮点

1.自定义 SpringCloud Gateway 全局过滤器，实现网关统一鉴权，并统计接口调用的耗时情况。

2.结合 Redis 和 Lua 脚本，实现了基于计数器算法的限流方式，有效防止了流量突发导致的系统崩溃。

3.基于 TransmittableThreadLocal (TTL) 自定义请求头拦截器，将Header 数据封装到线程变量中方便获取，减少用户信息数据库查询次数，同时验证当前用户有效期自动刷新有效期。

4.使用 Redis 分布式锁，确保流程状态更新按顺序执行且不被其他操作干扰，保护流程状态的更新过程。

5.采用 Cache Aside 模式，确保数据库更新后及时失效相关缓存，保证数据库与缓存的数据一致性。

6.通过 OpenFeign+Sentinel 实现自定义的 fallback 服务降级，确保系统在服务异常情况下仍然能稳定运行；并通过 Gateway+Sentinel 进行网关限流，减少峰值流量对单点服务造成强烈冲击，从而保证服务的可用性达到 99.9%以上。
