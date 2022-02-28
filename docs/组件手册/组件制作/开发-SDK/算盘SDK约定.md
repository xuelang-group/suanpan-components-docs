# 算盘 SDK 约定

## 对象存储路径约定
### 用户路径
User 路径: studio/{user_id}/

### 组件路径
Component 路径:  studio/{user_id}/component/{component_id}/

### APP 路径
APP 路径: studio/{user_id}/{app_id}/

### Node 路径
APP 路径: studio/{user_id}/{app_id}/{node_id}/

### Port 路径
APP 路径: studio/{user_id}/{app_id}/{node_id}/{port_id}

### 日志路径
Logs 路径: studio/{user_id}/logs/
APP Logs  路径: studio/{user_id}/logs/{app_id}/
Node Logs  路径: studio/{user_id}/logs/{app_id}/{node_id}/

### 临时文件路径（可以针对不同层级直接删除文件夹）
APP 临时文件路径: studio/{user_id}/tmp/{app_id}/
Node 临时文件路径: studio/{user_id}/tmp/{app_id}/{request_id}/{node_id}/
Port 临时文件路径: studio/{user_id}/tmp/{app_id}/{request_id}/{node_id}/{port_id}/

### 持久文件路径
APP 持久文件路径: studio/{user_id}/share/{app_id}/{component_alias}/
Node 持久文件路径: studio/{user_id}/share/{app_id}/{node_id}/{component_alias}/
Port 持久文件路径: studio/{user_id}/share/{app_id}/{node_id}/{port_id}/{component_alias}/

### 配置文件路径（边缘部署同步此文件夹）
APP 配置文件路径: studio/{user_id}/configs/{app_id}/
Node 配置文件路径: studio/{user_id}/configs/{app_id}/{node_id}/
Port 配置文件路径: studio/{user_id}/configs/{app_id}/{node_id}/{port_id}/

### 前面板静态文件路径（边缘部署同步此文件夹）
APP 配置文件路径: studio/{user_id}/dashboard/{app_id}/

## 本地文件路径约定
### 临时文件路径（用于与对象存储文件对应）
临时文件路径: /suanpan
例如： studio/{user_id}/data/{app_id}/ -> /suanpan/studio/{user_id}/data/{app_id}/
Tips: 临时文件路径由算盘配置，通过 storage 参数传递

### 全局文件路径（仅针对私有化部署）
全局文件路径: /global/
全局持久文件路径: /global/data/
全局配置文件路径: /global/configs/
Tips: 临时文件路径由算盘配置，通过 storage 参数传递