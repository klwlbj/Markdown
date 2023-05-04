#### 主要特征

- 不同于普通类库扩展，它直接接管php控制权，进入事件循环，回调到指定函数，程序员仅需处理回调函数；

- 纯C语言编写，异步tcp/udp，redis，mysql；

- 支持websocket；

  cpu上下文切换

  协程

  同步

  异步

  事件循环

#### 异步非阻塞操作

  Swoole1.X中大量使用异步回调

##### 对比生成器迭代器

隐式协程切换

##### 相关操作

如SQl查询等IO操作



#### 阻塞操作

- `sleep/usleep` 等

- 非Swoole提供的SQL、HTTP等操作

##### 解决

协程客户端

swoole4后一键协程化

#### **Swoole对比Golang**

|            | Swoole                                             | Golang           |
| ---------- | -------------------------------------------------- | ---------------- |
| 线程       | **单线程**，无线程安全问题，同一时间，只有一个协程 | 多线程，支持多核 |
| 偏向       | 业务                                               | 中间件           |
| 扩展兼容性 | 部分拓展不兼容                                     | 语言原生支持协程 |
| 强弱类型   | 弱类型自由灵活                                     | 强类型，效率高   |

单线程，无线程安全问题；多线程，支持多核

偏向业务；偏向中间件

扩展不支持协程，无法兼容；天生支持协程

弱类型自由灵活；强类型效率高

#### **生命周期**

Master进程：多线程。创建R。

Reactor进程：负责网络连接，收发数据，数据缓冲和拼接拆包。发送数据包和php回调给W。请求处理完成，返回给tcp客户端。

Manager进程：负责创建和回收W和TW进程。

Worker进程：投递任务给TW，响应数据给Manager。

TaskWorker进程：处理异步任务。

**类比**：R可理解为Nginx，W是PHP-FPM。TaskWorker是异步处理队列。

![img](D:\Markdown\assets\1.png)

#### **Server运行模式**

SWOOLE_PROCESS

多tcp=>多worker，Master=>Worker，单个Worker挂，连接不中断

SWOOLE_BASE

#### **单tcp=>单Worker，无Master，单个Worker挂，连接中断**

#### 

#### **Hyperf框架**

##### **协程全局变量**

###### 问题

单个Worker可有多个协程，Worker内可共享全局变量；

###### 种类

包含$_开头的变量、global 变量和 static 变量：

###### 变量周期

- 全局周期

- 协程周期



##### 使用协程

go co

###### 通道

协程间通信，占用内存，无IO消耗

类似Golang，push pop

###### WaitGroup

###### 协程上下文

set get has等

##### 注解

作用：解耦

默认收集至`Hyperf\Di\Annotation\AnnotationCollector`中，但也可自定义注解收集器

需继承自`Hyperf\Di\Annotation\AnnotationInterface`接口类

##### 热启动

```
composer require hyperf/watcher --dev

php bin/hyperf.php vendor:publish hyperf/watcher # 发布

php bin/hyperf.php server:watch # 监听代码修改，有修改则重启
```

##### 