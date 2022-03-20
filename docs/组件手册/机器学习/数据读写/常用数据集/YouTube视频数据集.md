# YouTube视频数据集使用文档
| 组件名称 |YouTube视频数据集 |  |  |
| --- | --- | --- | --- |
| 工具集 | 数据读写/常用数据集 |  |  |
| 组件作者 | 雪浪云-墨文 |  |  |
| 文档版本 | 1.0 |  |  |
| 功能 | 用于获取通用数据集-YouTube视频数据集 |  |  |
| 镜像名称 | ml_components:3 |  |  |
| 开发语言 | Python |  |  |

## 组件原理
使用该组件实现将YouTube视频数据集下发给后续组件使用


- 输出YouTube视频数据集，该数据集包括几个月的YouTube每日流行视频数据，可以用于以下用途
    - 多种形式的情感分析 
    - 根据评论和统计数据对YouTube视频进行分类。
    - 训练像RNNs这样的最大似然算法来生成他们自己的YouTube评论。 
    - 分析哪些因素会影响YouTube视频的受欢迎程度
    - 一些时序分析
- 数据描述
    - video_id：视频id
    - trending_date：流行日期
    - title：标题
    - channel_title：频道名称
    - category_id：分类id
    - publish_time：上传时间
    - tags：标签
    - views：播放数
    - likes：点赞数
    - dislikes：不喜欢数
    - comment_count：评论数
    - thumbnail_link：thumbnail链接
    - commnents_disabled：评论关闭
    - ratings_disabled：评分关闭
    - video_error_or_removed：视频出错或删除
    - description：描述

## 输入桩
无

## 输出桩
支持单个csv文件输出。
### 输出端子1

- **端口名称：** 数据
- **输出类型：** Csv文件
- **功能描述：** 输出YouTube视频数据集

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

![](./img/YouTube视频数据集.png)

![](./img/1568086602280-f3f7a128-867e-458b-b13a-917dc628f8ac.png)
