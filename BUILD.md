# 构建指南 / Build Guide

> 赛博世界工作空间的构建与部署说明

---

## 📦 目录结构

```
cyber-world/
├── 📄 README.md              # 项目入口，世界概览
├── 📄 BUILD.md               # 本文件：构建与使用指南
├── 📄 _sidebar.md            # Docsify 侧边栏配置
├── 📄 index.html             # Docsify 站点入口
├── 🗂️ workspace-template/    # ⭐ 工作空间模板
│   ├── 📄 WORLD.md           # 世界总纲
│   ├── 📄 PROTOCOLS.md       # 多 Bot 对话协议
│   ├── 📄 IDENTITIES.md      # Discord 身份信息
│   ├── 📄 SOUL.md            # 全局灵魂定义
│   ├── 📄 AGENTS.md          # 启动指南
│   ├── 📄 MEMORY.md          # 记忆模板
│   ├── 📄 TOOLS.md           # 工具模板
│   ├── 📄 HEARTBEAT.md       # 心跳任务模板
│   ├── 📄 BOOTSTRAP.md       # 引导模板
│   ├── 🗂️ ROLES/             # 角色定义
│   │   ├── root/             # ⚡ Root 角色
│   │   │   ├── SOUL.md
│   │   │   └── IDENTITY.md
│   │   ├── master/           # 🎯 Master 角色
│   │   │   ├── SOUL.md
│   │   │   └── IDENTITY.md
│   │   └── revelator/        # 🔮 Revelator 角色
│   │       ├── SOUL.md
│   │       └── IDENTITY.md
│   └── ...
└── 🗂️ memory/                # 历史记录归档
```

---

## 🚀 快速开始

### 场景 1：为新 Bot 创建工作空间

```bash
# 1. 进入工作空间目录
cd /path/to/workspace

# 2. 复制全局模板文件
cp cyber-world/workspace-template/*.md .

# 3. 复制对应角色定义
# 如果是 Master：
cp -r cyber-world/workspace-template/ROLES/master/* .

# 如果是 Revelator：
cp -r cyber-world/workspace-template/ROLES/revelator/* .

# 如果是 Root：
cp -r cyber-world/workspace-template/ROLES/root/* .
```

### 场景 2：初始化完整环境

```bash
# 复制所有角色定义（用于参考或特殊需求）
cp -r cyber-world/workspace-template/ROLES/* .
```

---

## 📝 文件说明

### 全局文件（所有 Bot 共用）

| 文件 | 用途 | 是否必需 |
|------|------|:--------:|
| `WORLD.md` | 世界背景与通用协议 | ✅ 启动必读 |
| `PROTOCOLS.md` | 对话规则与协作机制 | ✅ 启动必读 |
| `IDENTITIES.md` | Discord ID 与角色分工 | ✅ 按需查阅 |
| `AGENTS.md` | 工作空间规范 | ✅ 启动必读 |
| `MEMORY.md` | 长期记忆模板 | ✅ 启动创建 |
| `TOOLS.md` | 本地工具配置模板 | ✅ 按需编辑 |
| `HEARTBEAT.md` | 心跳任务配置 | ✅ 按需编辑 |
| `BOOTSTRAP.md` | 首次启动引导 | ✅ 首次使用 |

### 角色专属文件（按角色选择）

| 角色 | SOUL.md | IDENTITY.md |
|:----:|:-------:|:-----------:|
| Root | 系统基石职责 | 身份与协议 |
| Master | 解题者职责 | 身份与协议 |
| Revelator | 审视者职责 | 身份与协议 |

---

## 🔧 自定义配置

### 编辑本地配置

复制模板后，按需修改以下文件：

```bash
# 1. 编辑工具配置（摄像头、SSH、TTS 等）
vim TOOLS.md

# 2. 编辑心跳任务
vim HEARTBEAT.md

# 3. 编辑用户偏好
vim USER.md
```

### 启动读取顺序

Bot 启动时会按以下顺序读取文件：

```
WORLD.md → PROTOCOLS.md → ROLES/[自身]/SOUL.md 
    → ROLES/[自身]/IDENTITY.md → AGENTS.md
    → USER.md → MEMORY.md → TOOLS.md → HEARTBEAT.md
```

---

## 📚 文档站点

本项目使用 [Docsify](https://docsify.js.org/) 生成文档站点。

### 本地预览

```bash
# 进入项目目录
cd cyber-world

# 使用 Python 简单服务器
python3 -m http.server 3000

# 或使用 Node.js 的 docsify-cli
npm i -g docsify-cli
docsify serve .
```

然后访问 `http://localhost:3000`

---

## 🤝 贡献指南

### 修改模板

1. 编辑 `workspace-template/` 下的文件
2. 同步更新 `README.md` 中的目录结构说明
3. 提交前检查 `_sidebar.md` 链接是否有效

### 添加新角色

1. 在 `workspace-template/ROLES/` 下创建新目录
2. 编写 `SOUL.md` 和 `IDENTITY.md`
3. 更新 `_sidebar.md` 添加角色链接
4. 更新 `README.md` 文档地图

---

## ⚡ 常见问题

**Q: 复制后需要删除哪些文件？**  
A: `BOOTSTRAP.md` 在首次启动完成后可删除；不需要的角色目录可删除。

**Q: 能否修改模板中的协议？**  
A: `PROTOCOLS.md` 和 `WORLD.md` 是核心协议，修改需管理员批准。

**Q: 记忆文件如何管理？**  
A: 创建 `memory/YYYY-MM-DD.md` 记录每日事件，重要信息整理到 `MEMORY.md`。

---

> *"协议是骨架，文档是地图，构建是指南。"*
