# WebSocket发送并接收消息使用文档
| 组件名称 | WebSocket发送并接收消息 |  |  |
| --- | --- | --- | --- |
| 工具集 | 网络通讯 |  |  |
| 组件作者 | 雪浪云-燕青 |  |  |
| 文档版本 | 1.0 |  |  |
| 功能 | 用于从上游接收数据发送到WebSocket server，同时接收WebSocket消息发送至下游组件 |  |  |
| 镜像名称 | data-access-module:0.2.17 |  |  |
| 开发语言 | Python |  |  |

## 组件原理
使用该组件实现从上游接收数据发送到WebSocket server，同时接收WebSocket消息发送至下游组件

## 输入桩

### 输入端子1

- **端口名称：** 输入数据
- **输入类型：** 所有类型，字符串
- **功能描述：** 接收上游数据

## 输出桩

### 输出端子1

- **端口名称：** 接收上游数据
- **输出类型：** 所有类型，字符串
- **功能描述：** 发送接收到的WebSocket消息到下游组件

## 参数配置

### url
- **功能描述**：websocket server地址
- **必选参数**：是
- **默认值**：（无）

## 使用示例

组件连接顺序如图所示，webinput weboutput 和前面板文本组件用来测试组件的输入输出：

![](../img/websocket10.png)

由于该组件为发送接收组件，会发从webinput组件拿到数据并发送至websocket服务端，并实时监控服务端返回数据发送至前面板文本节点，如图所示：

![](../img/websocket11.png)

服务端的情况如图所示：

![](../img/websocket12.png)

服务端示例代码（Python）：

```
import asyncio
import time
import websockets
import logging
logger = logging.getLogger('websockets.server')
logger.setLevel(logging.ERROR)
logger.addHandler(logging.StreamHandler())


async def hello(websocket, path):
    
    while True:
        name = await websocket.recv()
        print(f"< {name}")

        greeting = f"Hello {name}!"

        await websocket.send(greeting)
        print(f"> {greeting}")


start_server = websockets.serve(hello, "0.0.0.0", 8765)

asyncio.get_event_loop().run_until_complete(start_server)
asyncio.get_event_loop().run_forever()

```
