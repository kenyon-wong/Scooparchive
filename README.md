# ScoopArchive

ScoopArchive 是一个使用 GitHub Actions 构建 Scoop 环境离线归档的工具，旨在帮助用户在内网环境下快速还原软件环境。

## 📖 项目概述

在许多企业或受限网络环境中，直接访问互联网安装软件可能受到限制。ScoopArchive 解决了这个问题，它通过 GitHub Actions 创建一个包含完整 Scoop 环境的离线归档文件，使用户能够在没有互联网连接的情况下轻松部署和管理软件。

## 🚀 特性

- 自动化构建：利用 GitHub Actions 自动创建 Scoop 环境归档
- 离线使用：支持在内网环境中还原软件环境
- 可定制：可以根据需求自定义要包含的软件包
- 版本控制：轻松管理和追踪不同版本的软件环境

## 🛠️ 使用方法

### 创建归档文件

1. Fork 本仓库到您的 GitHub 账户
2. 在 Actions 标签页中启用 Workflows
3. 手动触发 "Scooparchive" workflow 或等待预设的自动触发时间
4. Workflow 完成后，在 Actions 的构建结果中下载生成的归档文件

### 还原环境

1. 下载并解压归档文件
   - 将下载的归档文件解压到目标机器上
   - 注意：GitHub Actions 输出的归档是双重压缩格式，需要解压两次
   - 记录最终解压后的文件夹路径，例如：`D:\00PackageManager\`

2. 配置系统环境变量
   - 打开系统的"环境变量"设置
   - 在"用户变量"中新建变量 `SCOOP`，值为解压后的文件夹路径（如 `D:\00PackageManager\Scoop`）
   - 编辑用户变量中的 `Path`，新增一行 `%SCOOP%\shims`

3. 还原 Scoop 环境
   - 打开 PowerShell
   - 运行以下命令：
     ```powershell
     Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
     scoop reset *
     scoop cleanup -k *
     ```

4. 验证安装
   - 在 PowerShell 中运行 `scoop help`
   - 如果显示 Scoop 的帮助信息，则表示环境已成功还原

## 🔧 自定义

要自定义包含在归档中的软件包，编辑 `.github/workflows/scooparchive.yml` 文件中的 `Install Required Tools` 步骤。

## 📋 前提条件

- Windows 操作系统
- PowerShell 5.1 或更高版本

## 🤝 贡献

欢迎贡献！如果您有任何改进建议或发现了 bug，请创建 issue 或提交 pull request。

## 📄 许可

本项目采用 MIT 许可证。详情请见 [LICENSE](LICENSE) 文件。

## 📬 联系

如有任何问题或建议，请通过 GitHub Issues 与我们联系。

---

⭐ 如果您觉得这个项目有用，请给它一个 star！
