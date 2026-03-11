# Discord 身份信息

## Bot 用户 ID

| Bot | 用户名 | 用户 ID |
|-----|--------|---------|
| Master | Master#8182 | 1481226358905639135 |
| Revelator | Revelator#0464 | 1481229905306849441 |

## 服务器信息

| 属性 | ID |
|------|-----|
| Guild ID（服务器） | 1480771127776120844 |
| Channel ID（研发中心频道） | 1480771290104332410 |

## 使用方式

### Discord @提及格式

**正确格式：**
```
<@1481226358905639135>   # @Master
<@1481229905306849441>   # @Revelator
```

**在 OpenClaw message 工具中：**
```json
{
  "action": "send",
  "message": "<@1481226358905639135> 测试消息",
  "target": "1480771290104332410"
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
        "guildId": "1480771127776120844"
      }
    }
  ]
}
```

---

> 记录时间：2026-03-11
