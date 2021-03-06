# 数据转置（Pivot）使用文档
| 组件名称 | 数据转置（Pivot） |  |  |
| --- | --- | --- | --- |
| 工具集 | 数据处理 |  |  |
| 组件作者 | 雪浪云-墨文 |  |  |
| 文档版本 | 1.0 |  |  |
| 功能 | 数据转置组件，可以对数据进行转置操作，比如从行到列，或者从列到行 |  |  |
| 镜像名称 | data-analysis-components:3 |  |  |
| 开发语言 | Python |  |  |

## 组件原理
使用该组件将对数据进行转置操作，比如从行到列，或者从列到行

## 输入桩

### 输入端子1

- **端口名称：** 输入数据
- **输出类型：** Csv文件
- **功能描述：** Csv文件输入

## 输出桩

### 输出端子1

- **端口名称：** 输出数据
- **输出类型：** Csv文件
- **功能描述：** 将转置好的Csv文件输出

## 参数配置

### 配置页面

1. 操作界面

大致分为四部分，如下图所示。1，2，3，4部分分别按顺序对应工具栏，转置设置面板，数据概览，数据详情。
![](./img/%E6%95%B0%E6%8D%AE%E8%BD%AC%E7%BD%AE1.png)
2. 输出设置

右上角"设置输出格式"按钮，如下图所示，可以选择输出的格式：
- CSV：输出结构化数据文件（输出桩需要设置为CSV文件）
- JSON：形如[{"a": 1, "b": 2}, {"a": 3, "b": 4}]格式的JSON数据（输出桩需要设置为对象）
- Echarts组件可视化：形如[["a", "b"], [1, 2], [3, 4]]格式的JSON数据，主要是与算盘的前面板组件对接，进行数据可视化使用（输出桩需要设置为对象）
- 字符串：输出选中字符串值

![](./img/%E6%95%B0%E6%8D%AE%E8%BD%AC%E7%BD%AE2.png)

3. 转置配置面板

有行至列与列至行两种模式，选择不同模式面板上将展示不同配置菜单。点击清空按钮可清空所有配置，并将数据还原。点击确定按钮可使配置生效。具体配置说明见操作指南部分。

- 数据概览中显示数据的统计分布信息。
- 数据详情中展示所有数据。

4. 操作

转置组件有两大功能——列至行与行至列

- 列至行
    - 转置方法：列至行、行至列二选一。
    - 标识列：选出一列作为索引，作为index列.
    - 转为行的列：根据输入的字符串，重命名转置结果中的变量名列，默认值为"variable"。
    - 表示值的列：根据输入的字符串，重命名转置结果中的值列，默认值为"value"。
    - 需要转为行的列：用于生成变量名列于值列的字段。
    
![](./img/%E6%95%B0%E6%8D%AE%E8%BD%AC%E7%BD%AE3.png)

- 示例1

![](./img/%E6%95%B0%E6%8D%AE%E8%BD%AC%E7%BD%AE4.png)

转置过程详解：
1. 拆出索引列A，对应配置中的标识列：A。
2. 拆出值列，对应配置中的需要转为行的列：B。
3. 重构值列，将B列拆为两列，第一列为变量名列，第二列为变量名对应的值。
4. 重命名步骤3中生成的两列为"variable"与"value"，对应配置中转为行的列：variable，表示值的列：value。
5. 重组值列与索引列，生成转置结果。

![](./img/%E6%95%B0%E6%8D%AE%E8%BD%AC%E7%BD%AE5.png)

- 行至列

    - 转置方法：列至行、行至列二选一。
    - 转置索引列：选出一列作为索引，作为index。
    - 转置列：选出一列作为字段名。

聚合设置：如下图所示，选择一个字段名与一个聚合方式，聚合方式将作用于该字段，点击“加号”添加聚合设置，点击“减号”删除聚合设置。点击“对勾”使配置生效。

![](./img/%E6%95%B0%E6%8D%AE%E8%BD%AC%E7%BD%AE6.png)

空值填充：输入用于填充空值的值。

![](./img/%E6%95%B0%E6%8D%AE%E8%BD%AC%E7%BD%AE7.png)

- 示例2

![](./img/%E6%95%B0%E6%8D%AE%E8%BD%AC%E7%BD%AE8.png)

转置过程详解：
1. 将A作为维度1，B作为维度2，对原表格进行重组。对应配置: 转置索引列：A，转置列：B
下图中:

A = 1 B = 1时，C = 1

A = 1 B = 3时，C = 空值（填充后为0，对应配置:空值填充：1）

A = 2 B = 1时，C = 空值（填充后为0）

A = 2 B = 3时，C = 2

2. 对重组后的表进行聚合，对应配置: 聚合设置 C = 求和

A = 1 B = 1时，C的和 = 1

A = 1 B = 3时，C的和 = 0

A = 2 B = 1时，C的和 = 0

A = 2 B = 3时，C的和 = 2

3. 调整列名与索引。第一列为索引（A的值），B的值（1和3）与聚合字段名C合成为新的字段名1_C,3_C。

![](./img/%E6%95%B0%E6%8D%AE%E8%BD%AC%E7%BD%AE9.png)