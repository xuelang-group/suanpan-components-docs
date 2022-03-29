# 异步HTTP请求（多输入多输出）使用文档
| 组件名称 | 异步HTTP请求（多输入多输出） |  |  |
| --- | --- | --- | --- |
| 工具集 | 网络通讯|  |  |
| 组件作者 | 雪浪云-燕青 |  |  |
| 文档版本 | 1.0 |  |  |
| 功能 | 将http请求获取数据发给下游组件 |  |  |
| 镜像名称 | common-components-docker:0.0.13 |  |  |
| 开发语言 | Python |  |  |

## 组件原理
使用该组件将http请求获取的数据发送到下游组件，该组件支持多输入多输出，同时设置多个http请求配置

## 输入桩
支持任意多个端口输入，端口支持任意类型数据输入，输入输出端口个数与参数配置中的个数相匹配。

### 输入端子1

- **端口名称：** 输入1
- **输入类型：** 所有类型
- **功能描述：** 触发对应配置下的http请求

## 输出桩
支持任意多个端口输出，端口支持对象数据输出，输入输出端口个数与参数配置中的个数相匹配。

### 输出端子1

- **端口名称：** 输出1
- **输出类型：** 对象
- **功能描述：** http返回的对象发给下游组件

## 参数配置

### 配置
- **功能描述**：设置每个端口对应的http请求的相关配置
- **必选参数**：是
- **默认值**：
```
[{
    "url": "",
    "method":"get",
    "data": None,
    "json": None,
    "params": None,
    "headers": None,
    "cookies": None
}]
```

## 使用示例
该组件用户可以自定义等量的输入与输出，在配置中填入等量的配置信息（配置为n个对象的列表）。

如图搭建模板，设置http请求组件为异步：

![](../img/%E5%BC%82%E6%AD%A5HTTP%E8%AF%B7%E6%B1%82%EF%BC%88%E5%A4%9A%E8%BE%93%E5%85%A5%E5%A4%9A%E8%BE%93%E5%87%BA%EF%BC%891.png)

填入如下配置信息，注意该项目中有两输入两输出，所以配置列表长度为2 。

可以使用输入数据中的信息，只要引入inputData变量。使用python的eval执行该字符串。

其他配置可以参考获取上文外部数据模板。

```
[{
    "url": "\"https://jsonplaceholder.typicode.com/todos/\"+str(inputData)",
    "method":"get",
    "data": "None",
    "json": "None",
    "params": "None",
    "headers": "None",
    "cookies": "None"
},{
    "url": "\"https://jsonplaceholder.typicode.com/todos/\"+str(inputData)",
    "method":"get",
    "data": "None",
    "json": "None",
    "params": "None",
    "headers": "None",
    "cookies": "None"
}]
```

分别在webinput输入数字，拼接相应的请求路径：

![](../img/%E5%BC%82%E6%AD%A5HTTP%E8%AF%B7%E6%B1%82%EF%BC%88%E5%A4%9A%E8%BE%93%E5%85%A5%E5%A4%9A%E8%BE%93%E5%87%BA%EF%BC%892.png)

在前面板页面查看结果：

![](../img/%E5%BC%82%E6%AD%A5HTTP%E8%AF%B7%E6%B1%82%EF%BC%88%E5%A4%9A%E8%BE%93%E5%85%A5%E5%A4%9A%E8%BE%93%E5%87%BA%EF%BC%893.png)
