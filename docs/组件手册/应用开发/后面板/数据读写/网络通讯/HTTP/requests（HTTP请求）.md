# requests（HTTP请求）使用文档
| 组件名称 | requests（HTTP请求） |  |  |
| --- | --- | --- | --- |
| 工具集 | 网络通讯|  |  |
| 组件作者 | 雪浪云-燕青 |  |  |
| 文档版本 | 1.0 |  |  |
| 功能 | 将http请求获取数据发给下游组件 |  |  |
| 镜像名称 | common-components-docker:0.0.13 |  |  |
| 开发语言 | Python |  |  |

## 组件原理
使用该组件将http请求获取的数据发送到下游组件

## 输入桩
支持任意类型数据输入。

### 输入端子1

- **端口名称：** 输入1
- **输入类型：** 所有类型
- **功能描述：** 接收上游组件发送的触发信号并触发组件执行http请求获取数据

## 输出桩

### 输出端子1

- **端口名称：** out1
- **输出类型：** 对象
- **功能描述：** http返回的对象发给下游组件

## 参数配置
### API地址
- **功能描述**：http请求的url
- **必选参数**：是
- **默认值**：（无）

### 请求类型
- **功能描述**：http请求的方法
- **必选参数**：是
- **默认值**：get，支持get，post，put，delete

### 配置
- **功能描述**：http请求的相关配置
- **必选参数**：是
- **默认值**：
```
{
    "data": None,
    "json": None,
    "params": None,
    "headers": None,
    "cookies": None
}
```

### 验证方式
- **功能描述**：http请求的权限验证方式
- **必选参数**：是
- **默认值**：无，支持HTTPBasicAuth、HTTPDigestAuth、OAth1、无

### 验证用户与密码（逗号分隔）
- **功能描述**：当存在权限验证时，再此处填写用户名与密码，用英文逗号分隔
- **必选参数**：否
- **默认值**：（无）

## 使用示例

使用该组件定时获取jsonplaceholder中的数据

- 请求类型：GET
- 参数传递：通过定时触发组件与公式编辑器的输出得到
- 鉴权与header：无

通过下图查看参数配置：
注意在配置参数中需要可以填入data,json,params,headers,cookies等字段

- 这些字段具体含义请参考文档https://requests.readthedocs.io/en/master/user/quickstart/#make-a-request
- data,json为传递参数，params为传递url参数

注意：这些字段、API地址以及用户验证都可以使用inputData1（Json）关键字去获取输入中的内容，并可以使用简单的python语法得到想要的数据样式，当需要在各字段中使用inputData1时需要将提供一个可eval执行字符串，比如：
```
{
    "data": "None",
    "json": "None",
    "params": "None",
    "headers": "{\"authorization\":inputData1[\"data\"],\"appToken\":\"***********\"}",
    "cookies": "None"
}
```
在本例中就获取了输入中的url字段，最终请求地址为https://jsonplaceholder.typicode.com/todos/1。

公式编辑器设置为：{"url":"https://jsonplaceholder.typicode.com/todos/"+str(int(in1%200)+1)}

![](../img/http%E7%BB%84%E4%BB%B6.png)

项目部署后的效果为：

![](../img/http%E7%BB%84%E4%BB%B61.png)
