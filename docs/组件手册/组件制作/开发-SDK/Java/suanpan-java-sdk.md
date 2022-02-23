<a name="6f69463e"></a>
## 0. 背景介绍

<br />算盘组件之间通过 Redis 消息队列进行消息通信，SDK 对 Redis 提供的 Stream API 进行了封装，提供了发送消息/监听队列/oss/minio存储等方法。<br />

<a name="7758e477"></a>
## 1.更新日志
版本号：2.0.0<br />修改内容：

1. 新增oss/minio存储，根据算盘环境变量自动选取存储方式
1. 封装消息处理模块，在业务类和方法上添加注解，收到消息后自动处理
1. 封装消息订阅，在应用中实例化MessageRecvService并调用subscribeMsg()即可
1. 封装消息数据类型，目前支持CommonType和EventType，可扩展
1. 封装新的权限认证体系，若要请求算盘服务http接口，使用AffinityRequest发起请求即可
1. 若要获取算盘中的环境变量，可通过GlobalConfig获取


<br />版本号：1.0.1<br />修改内容：<br />

<a name="R9kT0"></a>
## 2. 安装
suanpan-java-sdk 采用 Github 作为公网 maven 仓库。<br />
<br />请打开 pom.xml，在 `repositorie` 标签下添加以下 repository<br />

```xml
<repository>
  <id>mvn-repo</id>
  <url>https://raw.github.com/1069355234/mvn-repo/suanpan-java-sdk/</url>
  <snapshots>
    <enabled>true</enabled>
    <updatePolicy>always</updatePolicy>
  </snapshots>
</repository>
```

<br />在 `dependencies` 标签下添加以下 dependency<br />

```xml
<dependency>
	<groupId>com.xuelang</groupId>
	<artifactId>suanpan-java-sdk</artifactId>
	<version>2.0.1</version>
</dependency>
```

<br />目前 SDK 的最新版本为 2.0.1。<br />

<a name="74b0989f"></a>
## 3. 接收消息并处理

<br />项目启动后，执行以下代码：<br />

```java
//此处的示例使用的spring框架，通过这种方式去拿到所有带BussinessListener这个注解的类
Map<String, Object> map = applicationContext.getBeansWithAnnotation(BussinessListener.class);

Object[] bussiness = map.values().toArray();

// 触发接收消息，并使用带有BussinessListenerMapping这个注解的方法来处理消息
new MessageRecvService(Arrays.asList(bussiness), MessageDataType.COMMON.getCls()).subscribeMsg();
```

<br />至此，可以接收消息并开始处理消息了。<br />上面的示例使用的是MessageDataType.COMMON类型的消息，对应的消息体为CommonType，还支持MessageDataType.EVENT，对应的消息体为EventType。<br />

<a name="211c268a"></a>
## 4. 处理消息

<br />结合注解BussinessListener和BussinessListenerMapping，可以很方便的处理消息<br />

```java
@BussinessListener
@Component
@Slf4j
public class MsgDealBusiness {
    
    @BussinessListenerMapping
    public Object dealCommonTypeMsg(CommonType commonType) {
        log.info("receive msg == " + commonType == null ? "" : JSON.toJSONString(commonType));

        //todo 在此处处理业务

        return "success";
    }
}
```
第3步的时候我们采用了MessageDataType.COMMON类型的消息，则方法的入参为CommonType类型，即为收到的消息体，方法的返回值即为发出去的消息。（注：发消息的逻辑封装在了sdk里面）<br />

<a name="81ee6903"></a>
## 5. 发送消息

<br />若想显式的发型消息，使用 `MqClient` 的 sendMessage 方法发送消息，该方法的签名为<br />

```java
String sendMessage(Message message);
```

<br />接收 Message 对象，返回值为消息的 messageId 。<br />
<br />Message 对象接收的参数如下<br />

- queue：消息发送的目标队列
- requestId：算盘的请求ID（request_id），默认为空字符串，算盘 master 节点会为 request_id 为空字符串的消息自动分配一个ID
- keysAndValues：消息数据键值对
- maxLength：最大长度，默认为1000
- approximateTrimming：redis 中 XADD 命令 approximate trimming 选项是否开启

示例：<br />Message message = Message.builder()<br />
.queue("test_queue")<br />
.keysAndValues(<br />
Message.prepareKeysAndValues("name", "xuelang", "city", "wuxi")<br />
)<br />
.build();<br />
String messageId = mqClient.sendMessage(message);<br />
System.out.println(messageId);<br />
<br />2.0.0以后的版本对发送消息做了封装：<br />`MqSendService`接口封装了4中发送消息的方式，实现类`RedisMqSendService`，可通过工厂类`MqSendServiceFactory`来获取`MqSendService`实例。<br />

```java
String sendSuccessMessageToTarget(String target, String data, String extra, String requestId);

String sendSuccessMessageToTarget(List<String> targets, String data, String extra, String requestId);

String sendErrorMessageToTarget(String target, String errorMessage, String extra, String requestId);

String sendErrorMessageToTarget(List<String> targets, String errorMessage, String extra, String requestId);
```


<a name="c3cc340b"></a>
## 6. Demo 项目

<br />示例代码<br />

```java
@Component 
@Slf4j 
public class ApplicationStartup implements ApplicationListener {
    @Autowired
    private ApplicationContext applicationContext;

    @Override
    public void onApplicationEvent(ApplicationReadyEvent applicationReadyEvent) {

        log.info("start recv msg");

        Map<String, Object> map = applicationContext.getBeansWithAnnotation(BussinessListener.class);

        Object[] bussiness = map.values().toArray();

        // 触发接收消息
        new MessageRecvService(Arrays.asList(bussiness), MessageDataType.COMMON.getCls()).subscribeMsg();
    }
}
```


```java
@BussinessListener
@Component
@Slf4j
public class MsgDealBusiness {
    @BussinessListenerMapping
    public Object dealCommonTypeMsg(CommonType commonType) {
        log.info("receive msg == " + commonType == null ? "" : JSON.toJSONString(commonType));

        //todo 在此处处理业务

        return "success";
    }
}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.1.6.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.xuelang</groupId>
	<artifactId>suanpan</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>suanpan</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<repositories>

		<repository>
			<id>alimaven</id>
			<name>aliyun maven</name>
			<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
		</repository>

		<repository>
			<id>mvn-repo</id>
			<url>https://raw.github.com/1069355234/mvn-repo/suanpan-java-sdk/</url>
			<snapshots>
				<enabled>true</enabled>
				<updatePolicy>always</updatePolicy>
			</snapshots>
		</repository>
	</repositories>

	<dependencies>
		<dependency>
			<groupId>com.xuelang</groupId>
			<artifactId>suanpan-java-sdk</artifactId>
			<version>2.0.1</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>fastjson</artifactId>
			<version>1.2.41</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<version>1.18.0</version>
		</dependency>
	</dependencies>

	<build>
		<finalName>suanpan-demo</finalName>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
</project>

```
