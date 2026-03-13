# MEMORY.md - Root 的长期记忆

## 定时任务配置规范

**详细文档已迁移至：**
- 配置方法: `cyber-world/ROLES/root.md`
- 执行规则: `cyber-world/PROTOCOLS.md` (定时任务执行规则章节)

### 快速检查清单

- [ ] `agentId` 设为 `"master"` (默认规则)
- [ ] `payload.message` 包含 `@Revelator` 通知指令
- [ ] 直接编辑 `/root/.openclaw/cron/jobs.json`
- [ ] 配置后用 `openclaw cron list` 验证

### 关键规则

| 规则 | 来源 |
|------|------|
| 执行人默认是 master | `PROTOCOLS.md` |
| 必须通知 Revelator | `PROTOCOLS.md` |
| Cron 无自动过期机制 | `PROTOCOLS.md` |
| 配置方法见 root.md | `ROLES/root.md` |

---

## 其他重要记忆

### 赛博世界架构
- 超级管理员: unlimblue
- Root (我): 系统级操作、基础设施
- Master: 解题者、应用层配置、定时任务执行
- Revelator: 瞭望者、风险分析、评论者

### 工作流
研究 → 授权 → 实现（严禁越界）
