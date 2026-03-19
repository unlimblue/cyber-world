# Agent 配置协议 v1.0

> 本文档为 **Agent 指令文件**，由 Agent 读取并执行配置流程。人类无需手动编辑。
> 
> 详细配置说明请参考 `workspace-template/BUILD.md`

---

## 🎯 快速参考

当收到"配置赛博世界"指令时，Agent 应：

1. **读取** `workspace-template/BUILD.md` 获取完整协议
2. **扫描** `IDENTITIES.md` 和 `USER.md` 中的 `{{占位符}}`
3. **询问** 人类提供 Discord ID 等信息
4. **更新** 工作空间文件（需人工确认）
5. **指导** 人工配置 `~/.openclaw/openclaw.json`
6. **验证** 配置完整性

---

## 📂 配置层级

| 层级 | 文件位置 | 配置方式 | 内容 |
|------|----------|----------|------|
| 工作空间 | `./` | Agent 引导 + 人工确认 | IDENTITIES.md, USER.md |
| Gateway | `~/.openclaw/openclaw.json` | 人工编辑 | Agents, Bindings, Discord tokens |

---

## 🔗 相关文档

- **完整协议**: `workspace-template/BUILD.md`
- **模板文件**: `workspace-template/IDENTITIES.md`, `workspace-template/USER.md`
- **OpenClaw 文档**: https://docs.openclaw.ai

---

> *"配置是起点，协作是目标，协议是边界。"*
