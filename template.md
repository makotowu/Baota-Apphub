
# 宝塔面板 Docker 应用模板开发指南

## 📋 概述

本文档详细说明了如何为宝塔面板 Docker 外部应用功能创建标准化的应用模板。通过遵循本指南，开发者可以创建兼容宝塔面板的 Docker 应用配置，实现一键部署和管理。

### 🎯 模板功能

- **标准化配置**：统一的应用配置格式和目录结构
- **可视化部署**：通过宝塔面板图形界面进行应用部署
- **参数化配置**：支持用户自定义端口、路径等配置参数
- **资源管理**：内置 CPU 和内存限制配置
- **网络控制**：支持内外网访问控制

### ✅ 适用范围

- 宝塔面板 10.0 及以上版本
- 基于 Docker Compose 的应用部署
- 需要参数化配置的 Docker 应用

## 🏗️ 目录结构

每个应用模板必须遵循以下标准目录结构：

```
应用名称/
├── app.json                    # 应用配置文件（必需）
├── icon.png                    # 应用图标（必需，100x100像素）
├── latest/                     # 最新版本目录（必需）
│   ├── docker-compose.yml     # Docker Compose 配置文件
│   └── .env                    # 环境变量配置文件
└── [版本号]/                   # 特定版本目录（可选）
    ├── docker-compose.yml     # 对应版本的 Docker Compose 配置
    └── .env                    # 对应版本的环境变量配置
```

### 📁 目录说明

| 文件/目录 | 必需性 | 说明 |
|-----------|--------|------|
| `app.json` | ✅ 必需 | 应用元数据和配置参数定义 |
| `icon.png` | ✅ 必需 | 应用图标，尺寸为 100x100 像素 |
| `latest/` | ✅ 必需 | 最新版本的配置文件目录 |
| `[版本号]/` | ⚪ 可选 | 特定版本的配置文件目录 |

## 📝 配置文件详解

### 1. app.json 配置文件

`app.json` 是应用的核心配置文件，定义了应用的基本信息、版本管理、用户界面参数等。

#### 基本结构

```json
{
    "appid": -1,
    "appname": "应用名称",
    "apptitle": "显示标题",
    "apptype": "应用类型",
    "appTypeCN": "应用类型中文名",
    "appversion": [],
    "appdesc": "应用描述",
    "appstatus": 1,
    "home": "项目主页",
    "help": "帮助文档链接",
    "updateat": 时间戳,
    "depend": null,
    "field": [],
    "env": [],
    "volumes": {}
}
```

#### 参数详细说明

##### 基础信息参数

| 参数 | 类型 | 必需 | 说明 | 示例 |
|------|------|------|------|------|
| `appid` | Number | ✅ | 应用ID，固定使用 -1 | `-1` |
| `appname` | String | ✅ | 应用名称，用于目录命名 | `"alist"` |
| `apptitle` | String | ✅ | 应用显示标题 | `"Alist"` |
| `apptype` | String | ✅ | 应用类型标识符 | `"Storage"` |
| `appTypeCN` | String | ✅ | 应用类型中文名称 | `"存储/网盘"` |
| `appdesc` | String | ✅ | 应用功能描述 | `"一个支持多存储的文件列表程序"` |
| `appstatus` | Number | ✅ | 应用状态（1=可用，0=不可用） | `1` |
| `home` | String | ⚪ | 项目主页或 GitHub 地址 | `"https://github.com/alist-org/alist"` |
| `help` | String | ⚪ | 帮助文档链接 | `"https://alist.nn.ci"` |
| `updateat` | Number | ✅ | 更新时间戳（秒） | `1752027587` |
| `depend` | Null | ✅ | 依赖项，暂不使用，固定为 null | `null` |

##### 应用类型对照表

| apptype | appTypeCN | 说明 |
|---------|-----------|------|
| `BuildWebsite` | 建站 | 网站建设相关应用 |
| `Database` | 数据库 | 数据库管理系统 |
| `Storage` | 存储/网盘 | 文件存储和网盘应用 |
| `Tools` | 实用工具 | 各类实用工具 |
| `Middleware` | 中间件 | 系统中间件 |
| `AI` | AI/大模型 | 人工智能相关应用 |
| `Media` | 多媒体 | 音视频处理应用 |
| `Email` | 邮件/邮局 | 邮件服务相关 |
| `DevOps` | DevOps | 开发运维工具 |
| `System` | 系统 | 系统管理工具 |

##### 版本管理配置

```json
"appversion": [
    {
        "m_version": "latest",
        "s_version": []
    },
    {
        "m_version": "3",
        "s_version": ["42.0", "41.0"]
    }
]
```

- `m_version`：主版本号
- `s_version`：子版本号数组，与主版本号拼接形成完整版本号

##### 用户界面字段配置 (field)

`field` 数组定义了用户在部署应用时需要填写的参数：

```json
"field": [
    {
        "attr": "参数名",
        "name": "显示名称",
        "type": "参数类型",
        "default": "默认值",
        "suffix": "提示信息",
        "unit": "单位"
    }
]
```

###### 必需的标准字段

每个应用都必须包含以下标准字段：

```json
{
    "attr": "domain",
    "name": "域名",
    "type": "textarea",
    "default": "",
    "suffix": "浏览器访问的域名，非必填",
    "unit": ""
},
{
    "attr": "allow_access",
    "name": "允许外部访问",
    "type": "checkbox",
    "default": true,
    "suffix": "允许直接通过主机IP+端口访问",
    "unit": ""
},
{
    "attr": "cpus",
    "name": "CPU核心数限制",
    "type": "number",
    "default": 0,
    "suffix": "0为不限制，最大可用核心数为：",
    "unit": ""
},
{
    "attr": "memory_limit",
    "name": "内存限制",
    "type": "number",
    "default": 0,
    "suffix": "0为不限制，最大可用内存为：",
    "unit": ""
}
```

###### 字段类型说明

| 类型 | 说明 | 示例 |
|------|------|------|
| `number` | 数字输入框 | 端口号、资源限制 |
| `string` | 文本输入框 | 用户名、密码 |
| `textarea` | 多行文本框 | 域名、配置内容 |
| `checkbox` | 复选框 | 开关选项 |
| `select` | 下拉选择框 | 预定义选项 |

##### 环境变量配置 (env)

`env` 数组定义了应用运行时的环境变量：

```json
"env": [
    {
        "key": "变量名",
        "type": "变量类型",
        "default": null,
        "desc": "变量描述"
    }
]
```

###### 必需的标准环境变量

```json
{
    "key": "app_path",
    "type": "path",
    "default": null,
    "desc": "应用数据目录"
},
{
    "key": "host_ip",
    "type": "string",
    "default": null,
    "desc": "主机IP"
},
{
    "key": "cpus",
    "type": "number",
    "default": null,
    "desc": "CPU核心数限制"
},
{
    "key": "memory_limit",
    "type": "number",
    "default": null,
    "desc": "内存大小限制"
}
```

###### 环境变量类型

| 类型 | 说明 | 验证规则 |
|------|------|----------|
| `port` | 端口号 | 自动检测端口占用 |
| `path` | 文件路径 | 路径格式验证 |
| `string` | 字符串 | 无特殊验证 |
| `number` | 数字 | 数值范围验证 |

##### 数据卷配置 (volumes)

```json
"volumes": {
    "data": {
        "type": "path",
        "desc": "数据目录"
    },
    "config": {
        "type": "file",
        "desc": "配置文件"
    }
}
```

### 2. .env 环境变量文件

`.env` 文件定义了 Docker Compose 使用的环境变量模板：

```bash
# 自定义变量（对应 app.json 中的 env 配置）
CUSTOM_PORT=
CUSTOM_PATH=

# 必需的标准变量
HOST_IP=                # 主机IP地址
CPUS=                   # CPU核心数限制
MEMORY_LIMIT=           # 内存限制
APP_PATH=               # 应用数据路径
```

#### 变量命名规则

- `app.json` 中的 `env.key` 会转换为大写形式
- 例如：`web_port` → `WEB_PORT`

### 3. docker-compose.yml 配置文件

Docker Compose 配置文件定义了容器的运行参数：

```yaml
services:
  应用名称:
    image: 镜像名称:标签
    deploy:
      resources:
        limits:
          cpus: ${CPUS}
          memory: ${MEMORY_LIMIT}
    environment:
      - 环境变量配置
    ports:
      - ${HOST_IP}:${外部端口}:${内部端口}
    restart: always
    volumes:
      - ${APP_PATH}/数据目录:容器内路径
    labels:
      createdBy: "bt_apps"
    networks:
      - baota_net

networks:
  baota_net:
    external: true
```

#### 配置要点

1. **资源限制**：必须使用 `${CPUS}` 和 `${MEMORY_LIMIT}` 变量
2. **端口映射**：使用 `${HOST_IP}:${端口变量}:容器端口` 格式
3. **数据卷**：使用 `${APP_PATH}/` 前缀确保数据持久化
4. **标签**：必须包含 `createdBy: "bt_apps"` 标签
5. **网络**：推荐使用 `baota_net` 外部网络

#### 最佳实践

- **避免使用 `container_name`**：可能导致应用状态识别异常
- **多服务网络**：超过5个服务时建议使用自定义网络
- **环境变量**：所有可配置参数都应使用环境变量
- **重启策略**：建议使用 `restart: always`

## 🚀 开发流程

### 步骤 1：创建应用目录

```bash
mkdir 应用名称
cd 应用名称
```

### 步骤 2：准备应用图标

- 格式：PNG
- 尺寸：100x100 像素
- 文件名：`icon.png`

### 步骤 3：编写 app.json

1. 复制模板配置
2. 修改基本信息参数
3. 定义用户界面字段
4. 配置环境变量映射
5. 设置数据卷挂载

### 步骤 4：创建版本目录

```bash
mkdir latest
# 可选：创建特定版本目录
mkdir 1.0.0
```

### 步骤 5：编写配置文件

在每个版本目录中创建：
- `docker-compose.yml`：容器编排配置
- `.env`：环境变量模板

### 步骤 6：测试验证

1. 在宝塔面板中导入应用
2. 测试部署流程
3. 验证功能完整性
4. 检查资源限制

## ❓ 常见问题解答

### Q1: 为什么应用部署后无法访问？

**A:** 检查以下配置：
- 端口映射是否正确使用 `${HOST_IP}:${端口变量}` 格式
- 防火墙是否开放对应端口
- `allow_access` 参数是否正确配置

### Q2: 如何处理多端口应用？

**A:** 在 `app.json` 中定义多个端口字段：
```json
"field": [
    {
        "attr": "web_port",
        "name": "Web端口",
        "type": "number",
        "default": 8080
    },
    {
        "attr": "api_port", 
        "name": "API端口",
        "type": "number",
        "default": 8081
    }
]
```

### Q3: 数据持久化如何配置？

**A:** 使用 `${APP_PATH}` 变量：
```yaml
volumes:
  - ${APP_PATH}/data:/app/data
  - ${APP_PATH}/config:/app/config
```

### Q4: 如何设置应用依赖关系？

**A:** 在 `docker-compose.yml` 中使用 `depends_on`：
```yaml
services:
  web:
    depends_on:
      - database
  database:
    image: mysql:8.0
```

### Q5: 环境变量不生效怎么办？

**A:** 检查：
- `app.json` 中的 `env.key` 与 `.env` 文件中的变量名是否匹配（注意大小写转换）
- `field.attr` 与 `env.key` 是否一致
- 变量类型是否正确

## 📋 开发检查清单

### 必需文件
- [ ] `app.json` 配置完整
- [ ] `icon.png` 图标（100x100像素）
- [ ] `latest/docker-compose.yml` 
- [ ] `latest/.env`

### 配置验证
- [ ] 应用基本信息完整
- [ ] 包含所有必需的标准字段
- [ ] 环境变量映射正确
- [ ] 端口配置使用变量格式
- [ ] 数据卷使用 `${APP_PATH}` 前缀
- [ ] 包含必需的 Docker 标签

### 功能测试
- [ ] 应用可以正常部署
- [ ] 端口访问正常
- [ ] 数据持久化工作
- [ ] 资源限制生效
- [ ] 外部访问控制正常

## 🔧 最佳实践建议

### 1. 安全性
- 避免在配置中硬编码敏感信息
- 使用环境变量传递密码和密钥
- 设置合理的资源限制

### 2. 可维护性
- 使用清晰的变量命名
- 添加详细的配置说明
- 保持版本目录结构一致

### 3. 用户体验
- 提供合理的默认值
- 添加详细的字段说明
- 设置适当的输入验证

### 4. 性能优化
- 合理设置资源限制
- 使用适当的重启策略
- 优化镜像选择

## 📚 参考资源

- [Docker Compose 官方文档](https://docs.docker.com/compose/)
- [宝塔面板官方论坛](https://www.bt.cn/bbs/)
- [Docker 最佳实践指南](https://docs.docker.com/develop/dev-best-practices/)

---

**💡 提示**：开发过程中遇到问题，建议先查看现有应用的配置作为参考，或在社区论坛寻求帮助。