# 📚 Memory 目录说明

> cyber-world/memory/ 的维护与同步流程

---

## 👥 记录责任分工

| 角色 | 职责 | 时间 |
|:----:|------|------|
| 🎯 **Master** | 日常实时记录 | 事件发生时 |
| ⚡ **Root** | 每日整理归档 | 每日 00:00 |

### Master 记录流程

```
事件发生时 → 实时记录到 memory/YYYY-MM-DD.md
                ↓
          标记重要决策 #ARCHIVE
                ↓
          如有需要 @Root 请归档
```

### Root 归档流程

```
每日 00:00 → 读取当日 memory 文件
                ↓
          提取 #ARCHIVE 标记内容
                ↓
          生成摘要
                ↓
          提交到 GitHub
```

---

## 🔄 定时日志工作流程

### 执行时间
每日 **00:00 UTC** 自动执行

### 执行者
⚡ **Root** — 系统基石

### 工作流程

```mermaid
flowchart TB
    Start([每日 00:00]) --> Read[读取当日<br/>memory/YYYY-MM-DD.md]
    Read --> Extract[提取重要决策<br/>和关键事件]
    Extract --> Summary[生成摘要报告]
    Summary --> Check{有重要<br/>更新?}
    Check -->|是| Commit[提交到 GitHub]
    Check -->|否| Skip[跳过提交]
    Commit --> Notify[发送摘要到 Discord]
    Skip --> Notify
    Notify --> End([结束])
    
    style Start fill:#4CAF50,color:#fff
    style End fill:#4CAF50,color:#fff
    style Commit fill:#2196F3,color:#fff
```

### 详细步骤

| 步骤 | 操作 | 输出 |
|:----:|------|------|
| 1 | 读取当日记忆文件 | 原始日志内容 |
| 2 | 提取重要决策 | 决策列表 |
| 3 | 提取关键事件 | 事件列表 |
| 4 | 生成摘要 | 结构化报告 |
| 5 | 判断是否提交 | 有重要内容则提交 |
| 6 | GitHub 同步 | 更新仓库 |
| 7 | Discord 通知 | 发送摘要到频道 |

---

## 📝 文件命名规范

```
memory/
├── README.md              # 本文件
├── 2026-03-11.md         # 构建日志（示例）
├── 2026-03-12.md         # 日常记录
└── ...                   # 每日自动归档
```

**文件名格式：** `YYYY-MM-DD.md`

---

## 🎯 记录内容规范

### 应记录的内容

- ✅ 重要决策和协议变更
- ✅ 系统架构调整
- ✅ 角色分工变化
- ✅ 重大事件和里程碑

### 不应记录的内容

- ❌ 临时调试信息
- ❌ 个人意见和推测
- ❌ 未确认的假设
- ❌ 敏感凭证和密钥

---

## 🔒 安全说明

- 所有记录必须在 Discord 公开频道进行
- 禁止私下通信记录
- Root 仅执行技术操作，不接触应用层内容评估
- 敏感信息需脱敏后记录

---

> *"记忆是赛博世界的历史，也是未来的基石。"*
