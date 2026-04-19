# Smart Workspace 📁 / 智能项目文件组织助手

[English](#english) | [中文](#中文)

---

## English

### What is it?

Smart Workspace Organizer helps AI Agents automatically manage files and keep their workspace clean.

**Without it:** Agent may dump all files in one place, getting messy.

**With it:** Agent remembers your project folders and automatically categorizes new tasks.

---

### Installation

#### Option 1: GitHub (Recommended)

```bash
openclaw skills install https://github.com/yaoweizhang/smart-workspace
```

#### Option 2: ClawHub

```bash
# Login first (if needed)
clawhub login
openclaw skills install smart-workspace
```

---

### First-Time Setup / 首次安装

> **Important! / 重要！**
> 
> On first load, the skill will automatically create an empty `PROJECT-HISTORY.md` file in your workspace. No manual action needed.

### How to Use

Just describe your task naturally:

| You say | Agent automatically |
|---------|-------------------|
| "Analyze football data" | → Categorize to football project |
| "Do battery test" | → Categorize to battery project |
| "Shooting prediction" | → Categorize to shooting project |
| "New project: develop XX" | → Ask to create new folder |

---

### How It Works

1. **Understand:** Agent analyzes your task and extracts 2-3 keywords
2. **Search:** Agent reads `PROJECT-HISTORY.md` to match keywords against project history
3. **Decide:**
   - **Full match** → Use existing folder automatically
   - **Partial match** → Ask for your confirmation
   - **No match** → Ask if you want to create a new project
4. **Execute:** Agent organizes files or creates new folder, then records it

---

### Supported Keywords

| Keywords | Project |
|----------|---------|
| football, prediction, verify | Football prediction |
| battery | Battery analysis |
| shooting | Shooting prediction |
| other | Create new project |

---

### FAQ

**Q: Do I need to configure anything?**
A: No. It works out of the box after installation.

**Q: Will it delete my files?**
A: No. It only updates the record file, never deletes actual files.

**Q: How to add new projects?**
A: Just tell Agent "I want to start a new project: xxx", and Agent will help create it.

**Q: Can multiple Agents share it?**
A: Each Agent has its own project history, they don't affect each other.

---

## 中文

### 是什么？

智能项目文件组织助手。帮助 AI Agent 自动管理文件，保持工作区整洁。

**没有它：** Agent 可能每次都把文件堆在同一个地方，混乱不堪。

**有了它：** Agent 会记住你每个项目的文件夹，新任务自动归类。

---

### 安装

#### 方式1：GitHub 安装（推荐）

```bash
openclaw skills install https://github.com/yaoweizhang/smart-workspace
```

#### 方式2：ClawHub 安装

```bash
# 需要先登录（如果需要）
clawhub login
openclaw skills install smart-workspace
```

---

### 首次安装

> **重要！**
> 
> 首次加载技能时，会自动在你的 workspace 下创建空的 `PROJECT-HISTORY.md` 文件。无需手动操作。

### 使用方法

只需要描述你的任务：

| 你说 | Agent 自动 |
|------|-----------|
| "分析足球数据" | → 归类到足球预测项目 |
| "做电池测试" | → 归类到电池分析项目 |
| "射击预测" | → 归类到射击预测项目 |
| "新项目：开发xx" | → 询问是否创建新文件夹 |

---

### 工作原理

1. **理解任务：** Agent 分析你的任务，提取2-3个关键词
2. **搜索历史：** Agent 读取 `PROJECT-HISTORY.md`，用关键词匹配项目历史
3. **决策判断：**
   - **完全匹配** → 自动使用已有文件夹
   - **部分匹配** → 询问你确认
   - **无匹配** → 询问是否创建新项目
4. **执行：** Agent 整理文件或创建新文件夹，并记录

---

### 支持的关键词

| 关键词 | 项目 |
|--------|------|
| 足球、预测、验证 | 足球预测相关 |
| 电池 | 电池分析相关 |
| 射击 | 射击预测相关 |
| 其他 | 创建新项目 |

---

### 常见问题

**问：需要我配置什么吗？**
答：不需要。安装后自动生效。

**问：会删我的文件吗？**
答：不会。只会更新记录文件，不会删除任何实际文件。

**问：怎么添加新项目？**
答：告诉 Agent "我要做一个新项目 xxx"，Agent 会帮你创建。

**问：多个 Agent 共用吗？**
答：每个 Agent 有独立的项目历史，互不影响。

---

## Changelog / 更新日志

### v1.0.0
- 支持中文关键词匹配 / Support Chinese keyword mapping
- 自动创建和记录项目 / Auto create and record projects
- 双语版本 / Bilingual version (EN/CN)
