目前 java-suanpan-sdk 类似于 suanpan-mq 项目，是对 Redis Stream 的底层封装。<br />类似与 Python 的 Stream-SDK，计划使用 Java 开发算盘业务 SDK，提供更高层次的封装。<br />计划使用的技术

- Java-suanpan-sdk：与消息队列进行通信
- Spring-reactor：采用 Reactor 设计模式（[https://en.wikipedia.org/wiki/Reactor_pattern](https://en.wikipedia.org/wiki/Reactor_pattern)），实现 Event-Driven 的开发模式
- Spring-boot-starter：试图将 SDK 封装为 Spring-boot-starter，利用 Spring-boot 的自动装载机制，让 SDK 做到即插即用

