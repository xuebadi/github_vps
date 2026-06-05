# 🖥️ GitHub Codespaces Windows 11 云桌面

> 在浏览器中使用免费的 Windows 11 桌面，基于 GitHub Codespaces + Docker。

## 🚀 快速开始

1. 打开本仓库，点击绿色 **"Code"** 按钮 → 选择 **"Create codespace on main"**
2. 等待 Codespace 构建完成（约 2 分钟）
3. 设置脚本会自动执行：
   - 拉取 `dockur/windows` Docker 镜像（首次约 10-15 分钟，约 10GB）
   - 启动 Windows 11 容器（支持 KVM 硬件加速）
4. **打开端口 8006**（Ports 面板中可见，或等待自动转发）
5. 在浏览器中即可使用 Windows 11 桌面！

## 创建示例

### Ubuntu
<p align="center">
  <img width="512" alt="image" src="https://github.com/user-attachments/assets/93f97616-8aaf-4206-857a-5d17aed8c4d2" />
</p>

### Windows
<p align="center">
  <img width="512" alt="image" src="https://github.com/user-attachments/assets/f40bc167-62b7-4b29-91e0-15e07a76e21c" />
</p>

## 部署教程

先给脚本添加执行权限。

```bash
chmod +x start.sh
```

## Ubuntu 使用教程

### 启动 Ubuntu

```bash
bash start.sh ubuntu
```

### 默认登录信息

Web 终端：

```text
地址：http://<url>:4200
用户名：root
密码：root
```

SSH：

```text
地址：<url>:8022
用户名：root
密码：root
```

RDP：

```text
地址：<url>:3389
用户名：root
密码：root
```

### 修改 Ubuntu root 密码

启动时可以通过 `ROOT_PASSWORD` 指定 root 密码。

```bash
ROOT_PASSWORD='admin@123' bash start.sh ubuntu
```

### 连接 SSH

本地连接示例：

```bash
ssh root@localhost -p 8022
```

如果使用外部地址，将 `localhost` 替换为你的访问地址。

```bash
ssh root@<url> -p 8022
```

### 连接 RDP

```text
地址：<url>:3389
用户名：root
密码：root
```

如果你启动时设置了 `ROOT_PASSWORD`，这里的密码就是你设置的值。

### 停止 Ubuntu

```bash
bash start.sh stop ubuntu
```

## Windows 使用教程

> 不同Windows版本可以在这里看：https://hub.docker.com/r/dockurr/windows
> 
> 然后改：
> ```bash
> environment:
>     VERSION: "11"
> ```

### 启动 Windows 11

默认启动 Windows 11。

```bash
bash start.sh
```

也可以显式指定 Win11。

```bash
bash start.sh win11
```

### 默认登录信息

管理界面：

```text
地址：http://<url>:8006
```

RDP：

```text
地址：<url>:3389
用户名：MASTER
密码：admin@123
```

### 修改 Windows 登录信息

启动前可以通过环境变量修改用户名和密码。

```bash
WINDOWS_USERNAME='MASTER' WINDOWS_PASSWORD='admin@123' bash start.sh win11
```

也可以修改资源配置。

```bash
WINDOWS_RAM_SIZE='4G' WINDOWS_CPU_CORES='4' WINDOWS_DISK_SIZE='64G' bash start.sh win11
```

### 停止 Windows

```bash
bash start.sh stop win11
```

## 停止全部

如果需要同时停止 Windows 和 Ubuntu，执行：

```bash
bash start.sh stop
```

## 端口说明

| 系统      | 服务       | 端口   |
| ------- | -------- | ---- |
| Ubuntu  | Web 终端   | 4200 |
| Ubuntu  | SSH      | 8022 |
| Ubuntu  | RDP      | 3389 |
| Windows | Web 管理界面 | 8006 |
| Windows | RDP      | 3389 |

注意，Ubuntu 和 Windows 都会使用 `3389` 端口。如果需要切换系统，请先停止当前正在运行的系统。

```bash
bash start.sh stop ubuntu
```

或者：

```bash
bash start.sh stop win11
```

## 常见问题

## ⚠️ 限制说明

1. **免费额度**：每月 120 核心小时
   - 4 核配置 = 约 30 小时/月
   - 2 核配置 = 约 60 小时/月
2. **数据持久性**：Docker 卷会保留数据，但 Codespace 本身是临时的
   - 重要文件请保存到 GitHub 或下载到本地
3. **Windows 未激活**：右下角有水印（功能完全正常）
4. **性能**：不适合重度计算或游戏
5. **首次启动**：Windows 安装需要 5-10 分钟
6. **无音频**：Web 方式（noVNC）不支持音频

## 🏗️ 架构

`
GitHub Codespaces（Linux 虚拟机）
└── Docker-in-Docker
    └── dockur/windows 容器
        ├── QEMU/KVM 虚拟化
        ├── Windows 11 客户系统
        ├── noVNC（Web 远程桌面 :8006）
        └── RDP 服务（:3389）
`

### 远程控制延迟太高
默认codespaces的VNC是卡的。可以在系统里装一个远程控制软件，比如Todesk、向日葵等，速度就很快了。

### 远程怎么访问
把URL从private改为public：

<img width="1080" height="434" alt="image" src="https://github.com/user-attachments/assets/f96a9008-dd7f-4874-954f-bcbb4dd6caf9" />

### 一小时就自动删了
没有活动下会被删，可以跑点任务，并把auto-delete关了：

<img width="1080" height="491" alt="image" src="https://github.com/user-attachments/assets/88001754-7bc5-4b43-b854-e3b2a02ee033" />

## 📄 许可

本项目使用 [dockur/windows](https://github.com/dockur/windows) Docker 镜像。






