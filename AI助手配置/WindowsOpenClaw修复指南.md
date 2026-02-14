# Windows 系统 OpenClaw MiniMax 配置修复指南

> 远程电脑是 Windows 系统

---

## 问题诊断

你的 OpenClaw 配置了 API Key，但使用了 OAuth 模式，导致认证失败。

---

## 修复步骤

### 方法一：通过命令修复

#### 1. 打开命令提示符或PowerShell

按 `Win + R`，输入 `cmd`，回车

#### 2. 检查当前配置

```bash
# 查看当前MiniMax配置
type "%USERPROFILE%\.openclaw\openclaw.json"
```

或者直接运行配置向导：

```bash
openclaw configure models
```

按提示操作：
- 选择添加/编辑 MiniMax
- 选择模式：`api_key`（不是OAuth！）
- 输入 API Key

#### 3. 重启 Gateway

```bash
openclaw gateway restart
```

---

### 方法二：手动修复（推荐）

#### 1. 备份配置

在文件资源管理器地址栏输入：
```
%USERPROFILE%\.openclaw\
```

复制 `openclaw.json` 为 `openclaw.json.backup`

#### 2. 编辑配置

用记事本或 VS Code 打开 `openclaw.json`

找到 `auth` 部分，修改为：

**方案A：使用 API Key（推荐）**
```json
"auth": {
  "profiles": {
    "minimax-portal": {
      "provider": "minimax-portal",
      "mode": "api_key"
    }
  }
}
```

**方案B：使用 OAuth（不需要API Key）**
```json
"auth": {
  "profiles": {
    "minimax-portal": {
      "provider": "minimax-portal",
      "mode": "oauth"
    }
  }
}
```

#### 3. 保存并重启

保存文件，然后在命令提示符输入：
```bash
openclaw gateway restart
```

---

## 快速检查清单

| 检查项 | 正确 | 错误 |
|--------|------|------|
| mode | `api_key` | `oauth` |
| API Key | 有（sk-xxx） | 无（OAuth模式不需要）|

---

## 如果还有问题

在远程电脑的命令提示符运行：
```bash
openclaw status
```

把输出发过来，我帮你分析。

---

**修复完成后告诉我，我来验证是否成功！**
