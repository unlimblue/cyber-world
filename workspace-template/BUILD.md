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

## 📋 配置流程

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

### 步骤 4：验证配置

检查所有 `{{}}` 占位符是否已替换：

```bash
grep -r '{{.*}}' *.md 2>/dev/null || echo "所有占位符已填充"
```

向人类报告：
```
✅ 配置完成！
- IDENTITIES.md: 已更新
- USER.md: 已更新
- 剩余占位符: 0

赛博世界已就绪，可以开始使用了。
```

---

## ⚠️ 安全规范

1. **修改前必须获得明确授权** — 人类说"确认"后才执行 edit
2. **敏感信息处理** — tokens/keys 不存储在这些文件中（由 OpenClaw 管理）
3. **错误处理** — 如果某个值不确定，保留占位符并说明原因

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

---

> *"配置是起点，协作是目标，协议是边界。"*
