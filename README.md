<div align="center">

# 🛡️ Download Guard

**AI Agent 下载管理 — 位置透明 · C盘保护 · 自动清理**

[![Platform: Windows](https://img.shields.io/badge/platform-Windows-blue)](https://github.com)
[![License: MIT-0](https://img.shields.io/badge/license-MIT--0-green)](LICENSE)
[![Version: 5.4.0](https://img.shields.io/badge/version-5.4.0-orange)](CHANGELOG.md)

</div>

---

## 😩 你是不是也遇到过这些问题？

> 用 AI Agent（Claude Code、Cursor 等）装东西，装完不知道去哪了……

| 痛点 | 场景 |
|------|------|
| 🤷 **"刚才下的东西去哪了？"** | Agent 默默装到 C 盘，你完全不知道 |
| 💥 **C 盘又红了** | 大模型 2-14 GB、npm 全局包、pip 缓存全堆在 C 盘 |
| 🔄 **重复下载** | Agent 忘了之前下过，又下一遍 |
| 🗑️ **垃圾越来越多** | 日志、缓存、临时文件只增不减 |
| 💿 **外接盘拔了** | 下载目录不可用，Agent 静默切回 C 盘 |
| ⚠️ **装完用不了** | 改了 npm prefix 后命令找不到了 |

**Download Guard = 上面这些问题的解药。**

---

## ✨ 装了之后会怎样？

### 每次下载，你都会看到这样的提示：

```
┌──────────────────────────────────────────────┐
│  🛡️ Download Guard                           │
│                                               │
│  📦 文件     : pytorch-2.3.0.whl              │
│  📏 大小     : 2.1 GB                         │
│  📂 写入至   : F:\AI-Downloads\2026-05-26\    │
│  💾 目标盘   : 132 GB 可用  ✅ OK              │
│  💿 C 盘     : 58 GB 可用   ✅ OK              │
│  🔒 路径可用 : ✅ OK                           │
│                                               │
│  ▶ 继续执行...                                │
└──────────────────────────────────────────────┘
```

### 下载完成，还会验证 + 记录：

```
┌──────────────────────────────────────────────┐
│  ✅ Download Guard · 完成                     │
│                                               │
│  📦 文件   : pytorch-2.3.0.whl  (2.1 GB)      │
│  📍 位置   : F:\AI-Downloads\2026-05-26\      │
│  📝 已记录 : download-log.md                  │
└──────────────────────────────────────────────┘
```

### 每天第一次触发，自动出环境报告：

```
┌──────────────────────────────────────────────┐
│  🛡️ Download Guard · 今日首次 · 环境快报       │
│                                               │
│  📂 下载目录   : F:\AI-Downloads (132 GB)      │
│  🔒 路径可用   : ✅ OK                         │
│  💿 C 盘       : 58 GB ✅ OK                   │
│  🗄️ 工具缓存   : 全部 OK                      │
│  📝 日志条数   : 47 条                         │
└──────────────────────────────────────────────┘
```

### 如果下载目录不可用（盘拔了/路径坏了）：

```
┌──────────────────────────────────────────────┐
│  🚫 Download Guard · BLOCKED                  │
│                                               │
│  ❌ 路径不可用 : F:\AI-Downloads               │
│  ⚠️ 原因       : Drive F: not found           │
│  🛑 操作       : 已阻止下载，未回退 C 盘       │
│                                               │
│  💡 请重新连接磁盘或修改下载目录               │
└──────────────────────────────────────────────┘
```

> **核心原则：路径不可用 → 宁可阻止，绝不静默回退 C 盘。**

---

## 🚀 一键安装

### 方式一：ClawHub（推荐）

```bash
clawhub install download-guard
```

### 方式二：手动

1. 下载或 clone 本仓库
2. 复制 `download-guard` 文件夹到 `~/.workbuddy/skills/download-guard/`
3. 下次触发下载时自动激活

---

## 🎮 首次配置（全自动引导）

安装后第一次触发下载时，Skill 会自动：

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  🛡️ Download Guard · 首次配置
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

磁盘扫描结果：

  F:  132 GB 可用  ← 推荐（空间最大）
  E:   97 GB 可用
  D:   39 GB 可用
  C:   58 GB 可用  [系统盘 - 不推荐]

推荐：F:\AI-Downloads
确认？或输入其他路径：
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

确认后自动：创建目录 → 写入配置 → 扫描 C 盘缓存 → 输出状态卡

**你只需要说一个路径，其他全自动。**

---

## 🗣️ 口令速查

对 AI Agent 说这些话即可触发对应功能：

| 说 | 干什么 |
|----|--------|
| `"下载了什么"` / `"download log"` | 查看最近 20 条下载记录 |
| `"缓存在哪"` / `"scan cache"` | 扫描所有工具缓存位置 |
| `"迁移缓存"` / `"migrate cache"` | 把 C 盘缓存一键迁走 |
| `"磁盘空间"` / `"disk space"` | 查看磁盘空间 |
| `"刚才下的在哪"` / `"where's my download"` | 看最近一次下载位置 |
| `"检查路径"` / `"check path"` | 验证下载目录是否健康 |
| `"帮我修复"` / `"fix warnings"` | 自动修复所有警告 |
| `"修改下载目录"` / `"change download dir"` | 更换下载目录 |
| `"下载版本"` / `"download guard version"` | 查看当前版本 |
| `"重置配置"` / `"reset config"` | 重新走一遍首次配置 |
| `"卸载 download guard"` | 显示清理说明 |

---

## 🔌 支持的工具

安装这些工具时自动触发守护：

| 类别 | 工具 |
|------|------|
| **Python** | `pip install` · `uv pip install` · `conda install` |
| **Node.js** | `npm install -g` · `pnpm add -g` · `bun install -g` |
| **Rust / Go** | `cargo install` · `go install` |
| **版本控制** | `git clone` |
| **AI 模型** | `ollama pull` · `huggingface-cli download` |
| **容器** | `docker pull` |
| **系统包** | `winget install` · `choco install` · `scoop install` |
| **通用下载** | `curl` · `wget` |

> 🧠 **智能识别**：在虚拟环境（venv）里的 `pip install` 不会触发 — 本地安装不影响 C 盘。

---

## ⚙️ 配置（一个文件，改了即时生效）

文件位置：`~/.workbuddy/skills/download-guard/config.md`

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `DOWNLOAD_ROOT` | *(首次配置时设置)* | 下载目标目录 |
| `MIN_FREE_GB` | `0.5` | 低于此值 = 阻止下载 |
| `WARN_FREE_GB` | `2` | 低于此值 = 警告但继续 |
| `C_DRIVE_WARN_GB` | `5` | C 盘低于此值 = 额外提醒 |
| `LOG_RETENTION_DAYS` | `30` | 日志自动归档天数（设 `0` 永不清理） |
| `LOG_ARCHIVE_MAX_MB` | `10` | 归档文件超过此大小自动删除 |

---

## 🔄 工作流程图

```
触发下载
  │
  ├─ ❌ 首次使用？ ──→ 引导配置（扫描磁盘→选路径→写配置→扫缓存）
  │
  ├─ 1️⃣ 读取配置 ──→ 验证配置完整性
  │
  ├─ 2️⃣ 路径分类 ──→ 缓存(A·安全) / 安装目录(B·需同步PATH) / 文件(C·安全)
  │
  ├─ 3️⃣ 重复检测 ──→ 已存在？→ 询问是否重新下载
  │
  ├─ 4️⃣ 可用性检查 ──→ 盘不存在/不可写 → 🚫 BLOCK（不走C盘）
  │                  → 空间不足 → 🚫 BLOCK
  │                  → 大文件警告 → ⚠️ WARN
  │
  ├─ 5️⃣ 告知用户 ──→ 文件名 + 路径 + 磁盘状态
  │
  ├─ 6️⃣ 执行下载
  │
  └─ 7️⃣ 验证+记录 ──→ 确认文件存在 → 写入日志（自动清理）→ 告知用户完成
```

---

## 🧰 包含的脚本

| 脚本 | 功能 |
|------|------|
| `scripts/check-space.ps1` | 磁盘空间 + 路径可用性校验（盘存在→可写→自动创建） |
| `scripts/log-download.ps1` | 下载日志记录 + 自动归档清理 |
| `scripts/scan-tool-cache.ps1` | 扫描 12+ 种工具缓存位置，标记 C 盘危险项 |
| `scripts/migrate-cache.ps1` | 一键将 C 盘缓存迁移到指定盘 |

---

## 🏗️ 项目结构

```
download-guard/
├── SKILL.md                ← 核心规则（Agent 加载此文件）
├── reference.md             ← 详细参考（按需加载）
├── config.md                ← 用户配置（模板：SETUP_DONE=false）
├── README.md                ← 本文件
├── CHANGELOG.md             ← 版本历史
├── LICENSE                  ← MIT-0
└── scripts/
    ├── check-space.ps1      ← 磁盘空间 + 路径可用性校验
    ├── log-download.ps1     ← 下载日志 + 自动清理
    ├── scan-tool-cache.ps1  ← 缓存位置扫描
    └── migrate-cache.ps1    ← 缓存迁移
```

---

## ⚠️ 已知限制

| 限制 | 说明 |
|------|------|
| 仅 Windows | 使用 PowerShell 脚本，不支持 macOS/Linux |
| nvm 管理的 Node | nvm 切版本可能重置 npm prefix 到 C 盘 |
| `pip install --user` | 装到 C 盘用户 site-packages — 建议用 venv |
| Docker 镜像存储 | 由 Docker Desktop (WSL2 VHDX) 管理 |
| 编译中间产物 | `cmake build`、`cargo build` 不在管辖范围 |

---

## 📜 版本历史

详见 [CHANGELOG.md](CHANGELOG.md)

---

## 📄 许可证

MIT-0 — 随便用。详见 [LICENSE](LICENSE)
