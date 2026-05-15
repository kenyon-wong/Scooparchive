# ScoopArchive

通过 GitHub Actions 构建 Scoop 离线归档，支持在内网环境快速还原完整软件环境。

## 快速使用

### 1. 构建归档

1. **Fork** 本仓库，在 Actions 页面启用 Workflows
2. 手动触发 **Scoop Archive** workflow，从下拉菜单选择变体：

| 变体 | 包含 | 适用场景 |
|---|---|---|
| `base` | CLI 工具集（jq, yq, vim, curl, git...） | 最小化环境 |
| `codeql` | base + JDK 多版本 + CodeQL + Maven/Gradle | 静态代码分析 |
| `python` | base + Python + pip 安全工具包 | Python 安全审计 |
| `agent` | base + Node.js + AI Coding Agents | AI 辅助开发 |
| `cloud` | base + 云桌面应用集 | 远程桌面环境 |
| `pro` | 以上全部 + IDE + 浏览器 + 安全工具 | 完整工作站 |

> 变体继承关系：**base ⊂ codeql / python / agent / cloud ⊂ pro**

3. 构建完成后，从 Artifacts 下载 `Scoop-{variant}-{run_id}.7z`

### 2. 还原环境

1. 解压归档文件到目标路径（例：`D:\00PackageManager\`）
2. 以**管理员身份**打开 PowerShell，执行以下命令：

```powershell
# 配置 Scoop 环境变量
[Environment]::SetEnvironmentVariable('SCOOP', 'D:\00PackageManager\Scoop', 'User')

# 将 Scoop shims 加入 PATH
$regPath = 'HKCU:\Environment'
$currentPath = (Get-ItemProperty -Path $regPath -Name PATH).PATH
Set-ItemProperty -Path $regPath -Name PATH -Value "$currentPath;%SCOOP%\shims"

# 刷新当前会话的环境变量
$env:SCOOP = 'D:\00PackageManager\Scoop'
$env:PATH += ';D:\00PackageManager\Scoop\shims'

# 初始化 Scoop 环境
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
scoop reset *
scoop cleanup -k *

# 验证
scoop help
```

> **注意**：如果下载的归档是 GitHub Actions 输出的 `.zip`，需要解压两次——先解压外层 zip，再解压内层 `.7z`。

## 前提条件

- Windows 10 / 11 或 Windows Server 2019+
- PowerShell 5.1 或更高版本
- 以管理员权限运行（环境变量配置需要）

## 项目结构

```
.github/
├── actions/setup-scoop/action.yml   ← 共享基础设施（composite action）
└── workflows/scoop-archive.yml      ← 唯一入口（choice inputs 选择变体）
requirements.txt                     ← Python 包清单
```

## 自定义

- **调整包列表**：编辑 `.github/workflows/scoop-archive.yml` 中各变体对应的 `Install` step
- **修改 Python 依赖**：编辑 `requirements.txt`
- **添加新变体**：在 `workflow_dispatch.inputs.variant.options` 中添加选项，并在 `jobs.build.steps` 中添加对应 conditional step

## 许可

MIT · [LICENSE](LICENSE)
