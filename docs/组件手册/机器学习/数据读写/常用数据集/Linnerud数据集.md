# Linnerud数据集使用文档
| 组件名称 | Linnerud数据集 |  |  |
| --- | --- | --- | --- |
| 工具集 | 数据读写/常用数据集 |  |  |
| 组件作者 | 雪浪云-墨文 |  |  |
| 文档版本 | 1.0 |  |  |
| 功能 | 用于获取通用数据集-Linnerud数据集 |  |  |
| 镜像名称 | ml_components:3 |  |  |
| 开发语言 | Python |  |  |

## 组件原理
使用该组件实现将Linnerud数据集下发给后续组件使用


- 输出Linnerud数据集，共有20个实例，每个实例都有3个生理数据和三个运动数据
- 数据描述
    - Weight：体重
    - Waist：腰围
    - Pulse：脉搏
    - Chins：引体向上
    - Situps：仰卧起坐
    - Jumps：跳跃

## 输入桩
无

## 输出桩
支持单个csv文件输出。
### 输出端子1

- **端口名称：** 数据
- **输出类型：** Csv文件
- **功能描述：** 输出Linnerud数据集

## 参数配置
### 输出结果

- **功能描述**： 查看结果数据
- **必选参数**： 是
- **默认值**： 无

## 使用方法
- 将组件拖入到项目中
- 与后一个组件输入的端口连接（必须是csv类型）
- 组件输入不需要连接其他节点
- 点击运行该节点

![](./img/Linnerud数据集.png)

![](./img/1568086602280-f3f7a128-867e-458b-b13a-917dc628f8ac.png)
