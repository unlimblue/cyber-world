# Agent 配置协议 v1.0

> 本文档为 **Agent 指令文件**，由 Agent 读取并执行配置流程。人类无需手动编辑。

---

## 🎯 触发指令

当收到以下指令时，Agent 应启动配置流程：
- "配置赛博世界"
- "初始化工作空间"
- "请帮我配置"
- "setup cyber-world"

---

## 📋 配置流程总览

赛博世界配置分为 **两个层级**：

1. **工作空间层**（本文档管理）— `IDENTITIES.md`, `USER.md` 等模板文件
2. **OpenClaw Gateway 层**（需人工配置）— `~/.openclaw/openclaw.json`

---

## 第一阶段：工作空间配置

### 步骤 1：扫描待配置项

读取以下文件中的占位符 `{{VAR_NAME}}`：

| 文件 | 占位符 | 说明 | 必需 |
|------|--------|------|:----:|
| `IDENTITIES.md` | `{{ADMIN_NAME}}` | 管理员 Discord 用户名 | ✅ |
| `IDENTITIES.md` | `{{ADMIN_USER_ID}}` | 管理员 Discord 用户 ID | ✅ |
| `IDENTITIES.md` | `{{ROOT_USER_ID}}` | Root Bot 用户 ID | ✅ |
| `IDENTITIES.md` | `{{MASTER_USER_ID}}` | Master Bot 用户 ID | ✅ |
| `IDENTITIES.md` | `{{REVELATOR_USER_ID}}` | Revelator Bot 用户 ID | ✅ |
| `IDENTITIES.md` | `{{GUILD_ID}}` | Discord 服务器 ID | ✅ |
| `IDENTITIES.md` | `{{MAIN_CHANNEL_ID}}` | 主频道 ID | ✅ |
| `IDENTITIES.md` | `{{ROOT_DISCRIMINATOR}}` | Root #后四位 | ❌ |
| `IDENTITIES.md` | `{{MASTER_DISCRIMINATOR}}` | Master #后四位 | ❌ |
| `IDENTITIES.md` | `{{REVELATOR_DISCRIMINATOR}}` | Revelator #后四位 | ❌ |
| `USER.md` | `{{ADMIN_NAME}}` | 管理员名称 | ✅ |
| `USER.md` | `{{TIMEZONE}}` | 时区（如 UTC+8） | ✅ |
| `USER.md` | `{{COMMUNICATION_STYLE}}` | 交流风格描述 | ❌ |
| `USER.md` | `{{WORKFLOW}}` | 工作流描述 | ❌ |

### 步骤 2：向人类询问

在 Discord 频道发送消息：

```
⚡ 检测到以下配置项需要填写：

**IDENTITIES.md**
- ADMIN_NAME: 你的 Discord 用户名
- ADMIN_USER_ID: 你的 Discord 用户 ID
- ROOT_USER_ID: Root Bot 的用户 ID
- MASTER_USER_ID: Master Bot 的用户 ID
- REVELATOR_USER_ID: Revelator Bot 的用户 ID
- GUILD_ID: Discord 服务器 ID
- MAIN_CHANNEL_ID: 主频道 ID

**USER.md**
- ADMIN_NAME: 你的名称
- TIMEZONE: 你的时区（如 UTC+8）

**如何获取 Discord ID：**
1. Discord 设置 → 高级 → 开启开发者模式
2. 右键用户/频道/服务器 → 复制 ID

请一次性提供所有值，格式如下：
```
ADMIN_NAME=xxx
ADMIN_USER_ID=123456
...
```
```

### 步骤 3：更新配置

获得人类提供的值后：

1. **确认授权**："我将更新 IDENTITIES.md 和 USER.md，确认执行？"
2. **执行替换**：使用 edit 工具替换每个占位符
3. **报告进度**：每完成一个文件报告一次

### 步骤 4：验证工作空间配置

检查所有 `{{}}` 占位符是否已替换：

```bash
grep -r '{{.*}}' *.md 2>/dev/null || echo "所有占位符已填充"
```

---

## 第二阶段：OpenClaw Gateway 配置

⚠️ **注意**：以下配置需要人工编辑 `~/.openclaw/openclaw.json`，Agent 仅提供指导和检查。

### 2.1 Agent 定义配置

**路径**: `~/.openclaw/openclaw.json` → `agents.list`

每个 Bot 需要独立配置：

```json
{
  "agents": {
    "list": [
      {
        "id": "main"
      },
      {
        "id": "master",
        "name": "master",
        "workspace": "{{MASTER_WORKSPACE_PATH}}",
        "agentDir": "{{OPENCLAW_DIR}}/agents/master/agent",
        "model": "kimi-coding/k2p5",
        "groupChat": {
          "mentionPatterns": [
            "@master", 
            "@Master", 
            "<@{{MASTER_USER_ID}}>"
          ]
        }
      },
      {
        "id": "revelator",
        "name": "revelator", 
        "workspace": "{{REVELATOR_WORKSPACE_PATH}}",
        "agentDir": "{{OPENCLAW_DIR}}/agents/revelator/agent",
        "model": "kimi-coding/k2p5",
        "groupChat": {
          "mentionPatterns": [
            "@revelator",
            "@Revelator", 
            "<@{{REVELATOR_USER_ID}}>"
          ]
        }
      }
    ]
  }
}
```

**配置项说明：**
| 字段 | 说明 | 示例 |
|------|------|------|
| `id` | Agent 唯一标识 | `master`, `revelator` |
| `workspace` | 工作空间路径 | `~/.openclaw/workspace-master` |
| `agentDir` | Agent 配置目录 | `~/.openclaw/agents/master/agent` |
| `model` | 使用的模型 | `kimi-coding/k2p5` |
| `mentionPatterns` | @提及触发规则 | 需包含占位符中的 Discord ID |

### 2.2 Discord 绑定配置

**路径**: `~/.openclaw/openclaw.json` → `bindings`

绑定 Agent 到 Discord 频道：

```json
{
  "bindings": [
    {
      "agentId": "master",
      "match": {
        "channel": "discord",
        "accountId": "master",
        "guildId": "{{GUILD_ID}}"
      }
    },
    {
      "agentId": "revelator",
      "match": {
        "channel": "discord",
        "accountId": "revelator",
        "guildId": "{{GUILD_ID}}"
      }
    }
  ]
}
```

### 2.3 Discord 账号配置

**路径**: `~/.openclaw/openclaw.json` → `channels.discord.accounts`

需要为每个 Bot 创建 Discord Application 并获取 Token：

```json
{
  "channels": {
    "discord": {
      "accounts": {
        "default": {
          "token": "{{ROOT_BOT_TOKEN}}",
          "groupPolicy": "allowlist",
          "guilds": {
            "{{GUILD_ID}}": {
              "requireMention": true
            }
          }
        },
        "master": {
          "enabled": true,
          "token": "{{MASTER_BOT_TOKEN}}",
          "groupPolicy": "allowlist",
          "guilds": {
            "{{GUILD_ID}}": {
              "requireMention": true
            }
          }
        },
        "revelator": {
          "enabled": true,
          "token": "{{REVELATOR_BOT_TOKEN}}",
          "groupPolicy": "allowlist",
          "guilds": {
            "{{GUILD_ID}}": {
              "requireMention": true
            }
          }
        }
      }
    }
  }
}
```

**获取 Bot Token 步骤：**
1. 访问 https://discord.com/developers/applications
2. 创建 New Application
3. 进入 Bot 标签页 → Add Bot
4. 复制 Token（仅显示一次，务必保存）

### 2.4 Gateway 基础配置

**路径**: `~/.openclaw/openclaw.json` → `gateway`

```json
{
  "gateway": {
    "port": 18789,
    "mode": "local",
    "bind": "loopback",
    "auth": {
      "mode": "token",
      "token": "{{GATEWAY_TOKEN}}"
    }
  }
}
```

### 2.5 模型配置

**路径**: `~/.openclaw/openclaw.json` → `models`

```json
{
  "models": {
    "mode": "merge",
    "providers": {
      "kimi-coding": {
        "baseUrl": "https://api.kimi.com/coding/",
        "api": "anthropic-messages",
        "models": [
          {
            "id": "k2p5",
            "name": "Kimi for Coding",
            "reasoning": true
          }
        ]
      }
    }
  }
}
```

---

## 第三阶段：验证完整配置

### 检查清单

- [ ] `IDENTITIES.md` 中所有占位符已替换
- [ ] `USER.md` 中所有占位符已替换
- [ ] `~/.openclaw/openclaw.json` 中 `agents.list` 定义了 master 和 revelator
- [ ] `~/.openclaw/openclaw.json` 中 `bindings` 绑定了 Discord 频道
- [ ] `~/.openclaw/openclaw.json` 中 `channels.discord.accounts` 配置了 Bot tokens
- [ ] 每个 Bot 的 `mentionPatterns` 包含正确的 Discord ID

### 启动验证

```bash
# 检查 Gateway 状态
openclaw gateway status

# 检查 Bot 是否在线
openclaw status
```

---

## ⚠️ 安全规范

1. **修改前必须获得明确授权** — 人类说"确认"后才执行 edit
2. **敏感信息处理** — tokens/keys 存储在 `~/.openclaw/openclaw.json`，不在工作空间文件中
3. **错误处理** — 如果某个值不确定，保留占位符并说明原因
4. **权限边界** — Agent 不直接修改 `~/.openclaw/openclaw.json`，仅提供指导和检查

---

## 📚 配置后文件说明

配置完成后，各文件用途：

| 文件 | 用途 | 由谁读取 |
|------|------|----------|
| `WORLD.md` | 世界背景、通用协议 | 所有 Bot |
| `PROTOCOLS.md` | 对话规则、协作机制 | 所有 Bot |
| `IDENTITIES.md` | Discord ID、角色分工 | 所有 Bot |
| `USER.md` | 管理员偏好、时区 | 所有 Bot |
| `AGENTS.md` | 启动指南、会话启动序列 | 所有 Bot |
| `TOOLS.md` | 本地工具配置（可选） | 所有 Bot |
| `HEARTBEAT.md` | 心跳任务配置（可选） | 所有 Bot |
| `ROLES/*/SOUL.md` | 角色灵魂定义 | 对应角色 Bot |
| `ROLES/*/IDENTITY.md` | 角色身份信息 | 对应角色 Bot |
| `~/.openclaw/openclaw.json` | Gateway 配置、模型、绑定 | OpenClaw Gateway |

---

> *"配置是起点，协作是目标，协议是边界。"*
