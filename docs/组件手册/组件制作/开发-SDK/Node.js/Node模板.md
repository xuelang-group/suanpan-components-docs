# Node模板

## 如何使用

1. 模版repo https://github.com/ryanaltair/suanpan_nodejs_template/
2. 点击 Use_This_Template 按钮创建 github repo
3. 将 repo 代码 git clone 到本地

## 初始化

1. 执行 npm init 初始化
2. 执行 npm install suanpan_node_sdk

## 如何构建容器

### 修改待推送容器名字与标签

- nano docker/imagename
- nano docker/version

### 推荐使用以下脚本构建容器

- 构建并推送 bash docker/build.sh
- 仅构建不推送 bash docker/buildonly.sh

### 可使用 update.sh 将代码rsync到指定位置并构建
```Bash
bash update.sh
```