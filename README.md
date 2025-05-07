# OpenVPN 守护进程自动化工作流

这个GitHub Actions工作流用于在Ubuntu最新版本上自动化执行以下任务:

1. 下载并执行`quick_start.sh`脚本（用于安装1Panel）
2. 启动OpenVPN守护进程并连接到VPN
3. 等待OpenVPN守护进程结束（无限期等待）

## 工作流特点

- 完全自动化：整个过程不需要人工干预
- 可靠的VPN连接：使用OpenVPN建立稳定的VPN连接
- 安全的凭证管理：通过GitHub Secrets存储敏感的VPN配置信息
- 健壮的进程管理：正确启动和等待进程，避免资源泄漏

## 配置方法

### 前提条件

- GitHub账户
- OpenVPN配置文件(.ovpn)

### 设置步骤

1. **添加OpenVPN配置到仓库Secrets**
   
   将您的OpenVPN配置文件内容添加到仓库的Secrets中，命名为`OPENVPN_CONFIG`。
   
   路径：仓库 → Settings → Secrets and variables → Actions → New repository secret

2. **自定义工作流(可选)**

   如果需要，您可以修改`ubuntu_openvpn.yml`文件来：
   - 调整VPN连接参数
   - 修改quick_start.sh脚本的下载源
   - 调整超时和等待时间

## 使用方法

工作流配置为手动触发，您可以通过以下方式启动：

1. 在GitHub仓库页面，点击"Actions"标签
2. 选择"在最新Ubuntu上运行脚本并循环检查网络 (通过OpenVPN)"工作流
3. 点击"Run workflow"按钮
4. 确认并启动工作流

## 工作流详解

### 步骤1: 检出代码
检出当前仓库代码，准备执行后续步骤。

### 步骤2: 下载并执行quick_start.sh
下载`quick_start.sh`脚本并以sudo权限执行它。这一步通常用于安装1Panel或其他需要的软件。

### 步骤3: 启动OpenVPN守护进程
安装OpenVPN客户端，配置VPN连接，并在后台启动OpenVPN进程。这一步会检查进程是否成功启动，并将PID保存供后续步骤使用。

### 步骤4: 等待OpenVPN守护进程结束
无限期地等待OpenVPN进程结束。每60秒会输出一次状态信息。当VPN进程结束时，会清理相关资源并完成工作流。

## 故障排除

- **VPN连接问题**: 检查OpenVPN配置是否正确，确认配置文件格式
- **权限问题**: 工作流使用sudo执行一些命令，确保GitHub Actions有足够权限
- **脚本执行超时**: 可能是网络连接问题或VPN配置问题

## 安全注意事项

- 永远不要在工作流文件中直接包含VPN凭据
- 使用GitHub Secrets存储敏感信息
- 审查第三方脚本(如quick_start.sh)以确保安全性

