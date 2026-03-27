# 🚀 OpenList for PaaS

本项目专为各类 **PaaS 平台**（如 Koyeb, Render, Heroku, HuggingFace, ClawCloud 等）提供完美兼容的 OpenList [<sup>1</sup>](https://github.com/OpenListTeam/OpenList) Docker 镜像。

## 💡 为什么会有这个项目？

从 OpenList v4.1.0 版本开始，为了提高容器安全性，官方移除了 Docker 镜像中的默认 Root 权限，改用无特权用户 (`uid: 1001`) 运行程序。

然而，许多 PaaS 平台在分配容器资源时，会强制指定随机的用户 UID，或者限制对数据卷卷的写入权限。直接部署官方镜像通常会导致以下报错并无限重启：

> `Error: Current user does not have write and/or execute permissions for the ./data directory: /opt/openlist/data`
> `错误：当前用户没有 ./data 目录 (/opt/openlist/data) 的写和/或执行权限。Exiting...`

**本项目的解决方案：**
每天通过 GitHub Actions 自动拉取官方最新镜像，在不修改任何底层源码的前提下，通过 Wrapper 方式**将用户切回 `root`，并赋予 `/opt/openlist/data` 目录 `777` 最高读写权限**，从而完美兼容所有受限的 PaaS 平台。

## ✨ 项目特性

- 🔄 **每天自动同步更新**：紧跟官方进度，每天北京时间上午 10 点自动构建。
- 📦 **零代码修改**：以官方镜像为基础底包构建，100% 原汁原味的官方程序，无任何夹带。
- 🌐 **全架构支持**：支持 `amd64`, `arm64`, `arm/v7`, `arm/v6`, `386` 等所有常见架构。
- 🏷️ **版本支持**：提供 `latest` (正式版)、`beta` (测试版)

## 🚀 如何使用？

在任何 PaaS 平台、Docker 或 Docker Compose 部署时，只需将官方镜像名称替换为本项目的镜像名称即可。

### 镜像标签 (Tags)

| 版本类型 | 对应的 Docker 镜像名 |
| :--- | :--- |
| **最新正式版** | `amsnowflake/openlist-for-paas:latest` |
| **测试版** | `amsnowflake/openlist-for-paas:beta` |
| **指定版本 (如v4.1.0)** | `amsnowflake/openlist-for-paas:v4.1.0` |

## 🛠️ 自己动手构建 (Fork)

如果你想用自己的账号自动构建，只需按照以下步骤操作：

1. **Fork** 本仓库到你的 GitHub 账号下。
2. 在你的 GitHub 仓库中，进入 `Settings` -> `Secrets and variables` -> `Actions`。
3. 添加两个 Secrets：
   - `DOCKER_USERNAME`: 你的 Docker Hub 用户名
   - `DOCKER_PASSWORD`: 你的 Docker Hub 密码或访问令牌 (Access Token)
4. 进入 `Actions` 选项卡，手动触发一次 `Build OpenList for PaaS (Docker Hub)` 工作流即可。

## 鸣谢

- OpenListTeam/OpenList [<sup>1</sup>](https://github.com/OpenListTeam/OpenList) - 核心程序提供者。
- 此项目仅做权限适配层的打包构建，所有解释权和开源协议同上游项目。
