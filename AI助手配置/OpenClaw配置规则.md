# OpenClaw 配置核心规则

> **重要**: 修改 `openclaw.json` 前必须遵循本规则

---

## 🔴 核心规则：修改配置必须确认

**在修改 `~/.openclaw/openclaw.json` 之前，必须先告知用户并获得同意！**

### 适用场景
- 添加新的模型配置
- 修改 API Key
- 更改认证模式（oauth / api_key）
- 更改模型服务商
- 删除或禁用现有配置

### 操作流程
```
1. 告知用户具体要修改什么
2. 说明修改原因
3. 等待用户确认同意
4. 执行修改
5. 验证修改是否成功
```

---

## ⚠️ MiniMax 配置常见错误

### 错误场景
用户提供了 MiniMax API Key，但配置成了 OAuth 模式

### 正确配置方式

#### 方式一：OAuth 模式（不需要 API Key）
```json
{
  "auth": {
    "profiles": {
      "minimax-portal": {
        "provider": "minimax-portal",
        "mode": "oauth"
      }
    }
  }
}
```
- ✅ 不需要 API Key
- ✅ 使用 MiniMax 账号登录验证
- ⚠️ 需要定期重新授权

#### 方式二：API Key 模式（需要 API Key）
```json
{
  "auth": {
    "profiles": {
      "minimax-portal": {
        "provider": "minimax-portal",
        "mode": "api_key"
      }
    }
  },
  "models": {
    "providers": {
      "minimax-portal": {
        "apiKey": "sk-xxxxxxxxxxxxxxxxxxxxxxxx"
      }
    }
  }
}
```
- ✅ 使用 API Key 直接调用
- ✅ 无需用户登录
- ⚠️ 需要有效的 API Key

### 常见错误配置
```json
// ❌ 错误：mode 是 oauth 但提供了 apiKey
{
  "auth": {
    "profiles": {
      "minimax-portal": {
        "provider": "minimax-portal",
        "mode": "oauth"   // ← 问题在这里！
      }
    }
  },
  "models": {
    "providers": {
      "minimax-portal": {
        "apiKey": "sk-xxx"  // ← OAuth 模式不需要这个
      }
    }
  }
}

// ✅ 正确：api_key 模式 + 有效 API Key
{
  "auth": {
    "profiles": {
      "minimax-portal": {
        "provider": "minimax-portal",
        "mode": "api_key"
      }
    }
  },
  "models": {
    "providers": {
      "minimax-portal": {
        "apiKey": "sk-xxx"
      }
    }
  }
}
```

---

## 📋 远程电脑修复步骤

### 问题诊断
远程电脑的 MiniMax 配置错误：
- 配置了 API Key
- 但 auth mode 设置为 "oauth"
- 导致认证失败

### 修复方案

**方案一：切换到 api_key 模式**
```bash
# 1. 备份当前配置
cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.backup

# 2. 使用 openclaw 命令配置
openclaw configure models

# 选择：
# - 添加/编辑 MiniMax 提供商
# - 选择模式：api_key
# - 输入 API Key
```

**方案二：使用 OAuth 模式（不需 API Key）**
```bash
# 如果想用 OAuth 模式：
# 1. 移除 apiKey 配置
# 2. 确保 mode 设为 "oauth"
# 3. 通过浏览器完成 OAuth 授权
```

---

## 🛡️ 配置检查清单

修改 `openclaw.json` 前必须检查：

- [ ] 确认认证模式（oauth / api_key）
- [ ] 确认 API Key 格式正确
- [ ] 确认 baseUrl 正确
- [ ] 确认模型 ID 正确
- [ ] 告知用户并获得同意

---

**规则制定日期**: 2026-02-14
**最后更新**: 2026-02-14
