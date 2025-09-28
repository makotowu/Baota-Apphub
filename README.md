# Baota-Apphub - 宝塔面板 Docker 应用资源库

[![GitHub stars](https://img.shields.io/github/stars/makotowu/Baota-Apphub?style=flat-square)](https://github.com/makotowu/Baota-Apphub/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/makotowu/Baota-Apphub?style=flat-square)](https://github.com/makotowu/Baota-Apphub/network)
[![GitHub issues](https://img.shields.io/github/issues/makotowu/Baota-Apphub?style=flat-square)](https://github.com/makotowu/Baota-Apphub/issues)

> **社区维护的宝塔面板 Docker 应用资源库，提供完整配置的 Docker 应用，支持一键导入部署**

## 🚀 项目简介

Baota-Apphub 是一个由社区维护的 Docker 应用资源库，专为宝塔面板用户设计。本项目收录了大量经过完整配置的 Docker 应用，用户可以通过宝塔面板的外部应用功能一键导入并部署，极大简化了 Docker 应用的部署流程。

**⚠️ 重要声明：本项目由社区维护，与宝塔官方无任何关联**

## ✨ 核心特性

### 🔧 完善的 Docker 应用配置
- **预配置完成**：所有 Docker 应用均已预先配置完成，包含完整的 docker-compose.yml 文件
- **开箱即用**：无需复杂配置，导入后即可直接部署运行
- **多版本支持**：提供多个版本选择，满足不同需求
- **标准化结构**：统一的目录结构和配置格式，确保兼容性

### 🎯 宝塔面板集成支持
- **一键导入**：支持通过 Git 仓库地址直接导入到宝塔面板
- **简化流程**：无需手动编写 Docker 配置文件
- **可视化管理**：通过宝塔面板图形界面管理 Docker 应用
- **批量部署**：支持批量导入多个应用

### 🌟 社区维护优势
- **持续更新**：社区成员持续维护和更新应用配置
- **快速响应**：问题反馈和功能请求得到及时处理
- **开源透明**：所有配置文件开源，安全可靠
- **丰富资源**：涵盖各类常用 Docker 应用

## 📦 应用目录

本仓库包含 **200+** 个精心配置的 Docker 应用，涵盖以下分类：

- **🌐 Web 服务**：Nginx、Apache、Caddy 等
- **💾 数据库**：MySQL、PostgreSQL、Redis、MongoDB 等
- **📊 监控工具**：Grafana、Prometheus、Netdata 等
- **🔒 安全工具**：Bitwarden、Vaultwarden、SafeLine 等
- **📁 文件管理**：Nextcloud、Seafile、Alist 等
- **🎵 媒体服务**：Jellyfin、Plex、Navidrome 等
- **🤖 AI 工具**：ChatNio、Lobe-Chat 等
- **📝 笔记工具**：Trilium、Siyuan、Obsidian 等
- **🔧 开发工具**：GitLab、Gitea、Code-Server 等

## 🛠️ 安装部署

### 前置要求

- 宝塔面板版本 **10.0 及以上**
- 已安装 Docker 和 Docker Compose
- 服务器具备互联网访问能力

### 导入方法

#### 方法一：Git 仓库导入（推荐）

1. 登录宝塔面板
2. 进入 **Docker** → **应用商店** → **更多** → **外部应用**
3. 点击 **导入应用** 按钮
4. 选择 **Git 仓库导入**
5. 输入仓库地址：
   ```
   https://github.com/makotowu/Baota-Apphub.git
   ```
   或国内镜像：
   ```
   https://cnb.cool/makotowu/Baota-Apphub.git
   ```
6. 点击 **导入** 完成

#### 方法二：压缩包导入

1. 下载本仓库的 ZIP 压缩包
2. 在宝塔面板中选择 **从文件导入**
3. 上传下载的压缩包文件
4. 系统自动解析并导入应用

### 详细导入教程

更详细的导入步骤和使用说明，请参考宝塔官方教程：

**📖 [宝塔面板 Docker 外部应用使用教程](https://www.bt.cn/bbs/thread-147864-1-1.html)** <mcreference link="https://www.bt.cn/bbs/thread-147864-1-1.html" index="0">0</mcreference>

## 📁 目录结构

```
apphub/                               # 应用根目录（必须为 apphub）
├── alist/                           # 应用目录（应用名称）
│   ├── app.json                     # 应用配置文件
│   ├── icon.png                     # 应用图标
│   ├── 3.42.0/                      # 版本目录
│   │   └── docker-compose.yml       # Docker Compose 配置
│   └── latest/                      # 最新版本
│       └── docker-compose.yml
├── nextcloud/
│   ├── app.json
│   ├── icon.png
│   └── latest/
│       └── docker-compose.yml
└── ...                             # 更多应用
```

## 🤝 参与贡献

我们欢迎社区成员参与项目贡献！

### 贡献方式

- **🐛 报告问题**：发现 Bug 或配置问题请提交 Issue
- **💡 功能建议**：有新的应用需求或功能建议请提交 Issue
- **🔧 提交 PR**：修复问题或添加新应用请提交 Pull Request
- **📖 完善文档**：帮助改进文档和使用说明

### 添加新应用

1. Fork 本仓库
2. 在 `apphub/` 目录下创建新的应用目录
3. 按照标准格式添加 `app.json` 和 `docker-compose.yml`
4. 测试配置文件的正确性
5. 提交 Pull Request

详细的应用配置格式请参考：[模板格式说明](template.md)

## ⚠️ 安全提醒

- **仅使用可信应用**：请只导入和使用来源可靠的应用配置
- **定期更新**：建议定期更新应用到最新版本
- **备份数据**：部署前请做好重要数据备份
- **网络安全**：注意配置防火墙和访问控制

## 📞 技术支持

- **GitHub Issues**：[提交问题和建议](https://github.com/makotowu/Baota-Apphub/issues)
- **QQ 交流群**：1061259740（宝塔 Docker 官方交流群）

## 📄 开源协议

本项目采用 [MIT License](LICENSE) 开源协议。

## 🙏 致谢

感谢 [@okxlin](https://github.com/okxlin) 提供的 [AppStore](https://github.com/okxlin/appstore) 项目，为本项目的导入功能提供了重要支持。

感谢所有为本项目贡献代码、提出建议和反馈问题的社区成员！

---

**⭐ 如果这个项目对你有帮助，请给我们一个 Star！**