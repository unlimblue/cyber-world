# 赛博世界快速重建方案

> 模板化、可移植、安全的重建流程

---

## 目标

- **重建时间:** < 30 分钟
- **安全等级:** Token 永不入 Git，配置中心化
- **可移植性:** 只需修改配置文件即可适配新服务器

---

## 方案架构

```
cyber-world-template/
├── README.md                 # 重建指南
├── config/
│   ├── config.yaml          # 主配置（ID、频道等）
│   └── secrets.yaml         # Token（本地，不入 Git）
├── bots/
│   ├── master/SOUL.md
│   └── revelator/SOUL.md
├── protocols/
│   └── PROTOCOLS.md
└── scripts/
    ├── deploy.sh            # 一键部署
    └── verify.sh            # 验证脚本
```

---

## 安全设计

### 1. 分层配置

| 文件 | 内容 | 存储位置 |
|------|------|----------|
| `config.yaml` | 服务器/频道/角色 ID | GitHub ✅ |
| `secrets.yaml` | Discord Tokens | 本地 🔒 |
| `.env` | 环境变量 | 本地 🔒 |

### 2. Git 保护

```gitignore
# 敏感文件永不入 Git
config/secrets.yaml
.env
*.token
*.key
```

### 3. 文件权限

```bash
chmod 600 config/secrets.yaml  # 仅所有者读写
```

---

## 重建流程（5 步骤）

### Step 1: 克隆模板

```bash
git clone https://github.com/unlimblue/cyber-world-template.git
cd cyber-world-template
```

### Step 2: 配置 ID 信息

编辑 `config/config.yaml`:

```yaml
server:
  guild_id: "YOUR_GUILD_ID"
  
channels:
  main: "YOUR_MAIN_CHANNEL_ID"
  news: "YOUR_NEWS_CHANNEL_ID"

roles:
  master_id: "MASTER_USER_ID"
  revelator_id: "REVELATOR_USER_ID"
```

### Step 3: 配置 Secrets

编辑 `config/secrets.yaml`:

```yaml
discord:
  master_token: "${DISCORD_TOKEN_MASTER}"
  revelator_token: "${DISCORD_TOKEN_REVELATOR}"
```

### Step 4: 部署

```bash
./scripts/deploy.sh
```

### Step 5: 验证

```bash
./scripts/verify.sh
```

---

## 加固建议（Revelator 审查）

| 建议 | 实施方式 |
|------|----------|
| 配置中心化 | 合并为单 `config.yaml` |
| 权限保护 | `deploy.sh` 开头执行 `chmod 600` |
| ID 验证 | 正则检查 Discord ID（17-20 位数字） |

---

## 预期效果

- ✅ 新服务器 30 分钟内重建完成
- ✅ Token 永不出现在代码仓库
- ✅ 配置灵活，支持任意 Discord 服务器
- ✅ 自动化验证确保部署成功

---

**审批状态:** ⏳ 等待管理员批准
