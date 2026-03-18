# Discord 身份信息

> ⚠️ **配置必读**：复制模板后，必须将以下 `{{占位符}}` 替换为实际的 Discord ID。
> 获取方法：在 Discord 中开启开发者模式，右键用户/频道复制 ID。

---

## 超级管理员

- **名称**: {{ADMIN_NAME}}
- **Discord ID**: {{ADMIN_USER_ID}}

---

## Bot 用户 ID

| Bot | 用户名 | 用户 ID |
|-----|--------|---------|
| Root | Root#{{ROOT_DISCRIMINATOR}} | {{ROOT_USER_ID}} |
| Master | Master#{{MASTER_DISCRIMINATOR}} | {{MASTER_USER_ID}} |
| Revelator | Revelator#{{REVELATOR_DISCRIMINATOR}} | {{REVELATOR_USER_ID}} |

---

## 服务器信息

| 属性 | ID |
|------|-----|
| Guild ID（服务器） | {{GUILD_ID}} |
| Channel ID（主频道） | {{MAIN_CHANNEL_ID}} |

---

## 使用方式

### Discord @提及格式

**✅ 正确格式（用户 ID）：**
```
<@{{MASTER_USER_ID}}>   # @Master
```

**❌ 错误格式（角色 ID）：**
```
<@&{{ROLE_ID}}>   # 角色ID，不会触发 Bot
```

**💡 提示：** 在 Discord client 中输入 `@` 时，选择**用户名**（Master#xxxx）而非**角色标签**（彩色 Master）

### OpenClaw message 工具示例

```json
{
  "action": "send",
  "message": "<@{{MASTER_USER_ID}}> 测试消息",
  "target": "{{MAIN_CHANNEL_ID}}"
}
```

### 绑定配置参考

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
    }
  ]
}
```

---

## 角色分工速查

| Agent | 核心职能 | 基础设施 | 应用配置 |
|:-----:|----------|:--------:|:--------:|
| ⚡ Root | 系统诊断、基础设施 | ✅ | ❌ |
| 🎯 Master | 方案制定、任务执行 | ❌ | ✅ |
| 🔮 Revelator | 风险扫描、盲点提示 | — | — |

**协作原则：**
- Root 负责 Gateway、网络、账号等基础设施
- Master 负责定时任务内容、投递逻辑等应用层配置
- Revelator 负责洞察任何潜在的风险点
- 自己没有权限时，可以请 Root 代劳

---

## 配置清单

复制模板后，编辑本文件替换以下占位符：

```markdown
{{ADMIN_NAME}}              → 你的 Discord 用户名
{{ADMIN_USER_ID}}           → 你的 Discord 用户 ID
{{ROOT_USER_ID}}            → Root Bot 的用户 ID
{{MASTER_USER_ID}}          → Master Bot 的用户 ID
{{REVELATOR_USER_ID}}       → Revelator Bot 的用户 ID
{{GUILD_ID}}                → Discord 服务器 ID
{{MAIN_CHANNEL_ID}}         → 主频道 ID
{{ROOT_DISCRIMINATOR}}      → Root 的 # 后四位数字
{{MASTER_DISCRIMINATOR}}    → Master 的 # 后四位数字
{{REVELATOR_DISCRIMINATOR}} → Revelator 的 # 后四位数字
```
