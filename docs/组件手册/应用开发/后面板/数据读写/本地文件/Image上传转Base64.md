# Image上传转Base64使用文档
| 组件名称 | Image上传转Base64 |  |  |
| --- | --- | --- | --- |
| 工具集 | 本地文件|  |  |
| 组件作者 | 雪浪云-燕青 |  |  |
| 文档版本 | 1.0 |  |  |
| 功能 | 用于将图片转为base64字符串 |  |  |
| 镜像名称 | common-components-docker:0.0.13 |  |  |
| 开发语言 | Python |  |  |

## 组件原理
使用该组件实现将图片转为base64字符串的功能

## 输入桩

### 输入端子1

- **端口名称：** in1
- **输入类型：** 图片
- **功能描述：** 接收图片

## 输出桩

### 输出端子1

- **端口名称：** out1
- **输出类型：** 字符串
- **功能描述：** 将图片转为base64字符串并输出给下游组件
