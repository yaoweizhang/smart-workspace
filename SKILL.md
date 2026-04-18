---
name: smart-workspace
version: 1.0.0
description: 轻量化的项目文件管理技能。自动管理智能体工作区，根据任务关键词匹配项目历史，智能决定使用现有文件夹或创建新文件夹。避免文件混乱，大大增强智能体的项目管理能力。
description_en: Lightweight project file management skill. Automatically manages agent workspace, matches task keywords to project history, intelligently decides whether to use existing folders or create new ones. Prevents file chaos, greatly enhances agent project management capabilities.
keywords: [文件整理, 项目管理, workspace, folder, organize, project]
metadata:
  openclaw:
    emoji: "📁"
---

# Smart Workspace Organizer 📁 / 智能项目文件组织助手

Lightweight project file management skill. / 轻量化的项目文件管理技能。

## Core Features / 核心功能

| # | English | 中文 |
|---|---------|------|
| 1 | **Project History Search** - Match keywords in `project-history.md` | **项目历史搜索** - 在 `project-history.md` 中匹配关键词 |
| 2 | **Smart Folder Recommendation** - Tell user which folder to use | **智能文件夹推荐** - 告诉用户应该使用哪个文件夹 |
| 3 | **New Project Creation** - Create folders and record when needed | **新项目创建** - 必要时创建新项目文件夹并记录 |
| 4 | **History Updates** - Keep project history current | **历史记录更新** - 保持项目历史最新 |

## Project History File / 项目历史文件

Location / 位置: `workspace/project-history.md`

### Format / 格式

```markdown
# Project History / 项目历史

## project-key
- name: Project Name / 项目名称
- path: ./project-folder/
- desc: Project description / 项目描述
- created: YYYY-MM-DD
- last_used: YYYY-MM-DD
```

### Format Fields / 格式字段

| Field / 字段 | Description / 说明 | Example / 示例 |
|--------------|-------------------|----------------|
| `## key` | Unique project identifier (level-2 heading) / 项目唯一标识 | football |
| `name` | Project name in Chinese / 中文名称 | 足球预测 |
| `path` | Path relative to workspace / 相对于workspace的路径 | ./data_analyzer/ |
| `desc` | Project description (for keyword matching) / 项目描述 | 足球数据分析 |
| `created` | Creation date / 创建时间 | 2026-03-01 |
| `last_used` | Last used date / 最后使用时间 | 2026-04-18 |

### Path Resolution / 路径解析

```
Current workspace: ~/.openclaw/workspace-[agentname]/

Project history: ~/.openclaw/workspace-[agentname]/project-history.md
New project folder: ~/.openclaw/workspace-[agentname]/[projectname]/
```

## Chinese Keyword Mapping / 中文关键词映射

| Chinese / 中文 | Matches / 匹配key |
|----------------|------------------|
| 足球 / football | football |
| 电池 / battery | battery |
| 射击 / shooting | shooting |
| 预测 / prediction | football |
| 验证 / verification | verify |

## Execution Flow / 执行流程

### Step 1: Understand Task / 理解任务

```
EN: Analyze user's task, extract 2-3 keywords, determine project type
CN: 分析用户任务，提取2-3个关键词，判断属于哪类项目
```

### Step 2: Search History / 搜索历史

```
EN: Read project-history.md with read tool, parse ## key headings
CN: 用 read 工具读取 project-history.md，解析 ## key 二级标题
```

### Step 3: Decision / 决策

```
EN: Match results:
    ├── Full match (key matches) → Tell user to use that folder
    ├── Partial match (name/desc matches) → Confirm with user
    └── No match → Ask if should create new project

CN: 判断结果：
    ├── 完全匹配(key一致) → 告知用户使用该文件夹
    ├── 部分匹配(name或desc匹配) → 向用户确认
    └── 无匹配 → 询问是否创建新项目
```

### Step 4: Execute / 执行

```
EN:
Existing project:
  "This is a [project name] task (last used: YYYY-MM-DD).
   Files will be organized to [path]."

New project:
  "This is a new project. Confirm the name '[name]'?
   I will:
   1. Create ./[name]/ folder
   2. Update ./project-history.md"

CN:
已有项目：
  "好的，这是[项目名]任务（最后使用: YYYY-MM-DD）。
   文件将整理到 [path]。"

新项目：
  "这是一个新项目。确定命名为 '[name]' 吗？
   我将：
   1. 创建 ./[name]/ 文件夹
   2. 更新 ./project-history.md"
```

## Decision Rules / 决策规则

### Project vs One-time Task / 项目 vs 一次性任务

| Type / 类型 | Example / 示例 | Handling / 处理方式 |
|-------------|----------------|---------------------|
| Ongoing project / 持续项目 | 足球预测系统 / Football prediction | Create independent folder / 创建独立文件夹 |
| Daily analysis / 日常分析 | 每日预测、周报 / Daily prediction | Put in related project folder / 放入相关项目文件夹 |
| One-time task / 一次性任务 | 临时数据整理 / Temp data整理 | Put in project folder or tmp / 放入项目文件夹或tmp |

### Folder Naming / 文件夹命名规范

- Use concise English/pinyin names / 使用简洁的英文/拼音名称
- Avoid Chinese folder names / 避免中文文件夹名
- Example / 示例: `myproject/`, `recommender/`, `dataprocessor/`

## Example Dialogues / 示例对话

### Case 1: Daily Analysis → Match Existing / 日常分析 → 匹配已有项目

**User / 用户**: "帮我分析今天足球数据" / "Analyze today's football data"

**Agent / 智能体**:
```
[读取] ./project-history.md
[提取关键词] "足球" → football
[匹配结果] football -> ./data_analyzer/
  created: 2026-03-01, last_used: 2026-04-18

EN: "This is a football prediction task (last used: 2026-04-18).
    Files will be organized to data_analyzer/verify_2nd/."

CN: "好的，这是足球预测任务（最后使用: 2026-04-18）。
    文件将整理到 data_analyzer/verify_2nd/。"
```

### Case 2: New Project / 新项目

**User / 用户**: "我要开发一个新的推荐算法" / "I want to develop a new recommendation algorithm"

**Agent / 智能体**:
```
[读取] ./project-history.md
[提取关键词] "推荐"
[匹配结果] 无匹配

EN: "This is a new project. Confirm the name 'recommender'?
    I will:
    1. Create ./recommender/ folder
    2. Update ./project-history.md"

CN: "这是一个新项目。确定命名为 'recommender' 吗？
    我将：
    1. 创建 ./recommender/ 文件夹
    2. 更新 ./project-history.md"
```

### Case 3: Partial Match Confirmation / 模糊匹配确认

**User / 用户**: "做一下射击项目的数据分析" / "Do data analysis for the shooting project"

**Agent / 智能体**:
```
[读取] ./project-history.md
[提取关键词] "射击" → shooting
[匹配结果] shooting -> ./shooting/

EN: "Found project 'shooting/射击预测'. Is this the same task?"
    - Yes → Use ./shooting/
    - No → Create new project folder

CN: "找到了 'shooting/射击预测' 项目。这是同一个任务吗？"
    - 是 → 使用 ./shooting/
    - 否 → 创建新项目文件夹
```

## Test Cases / 测试案例

### ✅ Test 1: Football Analysis / 足球分析
```
Input: "分析今天足球数据" / "Analyze today's football data"
Expected: Match football → ./data_analyzer/
```

### ✅ Test 2: Battery Research / 电池研究
```
Input: "做一下电池测试分析" / "Do battery test analysis"
Expected: Match battery → ./battery-ana/
```

### ✅ Test 3: New Project / 新项目
```
Input: "开发一个天气预测系统" / "Develop a weather prediction system"
Expected: No match → Ask if should create new project
```

### ✅ Test 4: Shooting Task / 射击任务
```
Input: "射击项目的数据整理" / "Organize shooting project data"
Expected: Match shooting → ./shooting/
```

## Cross-Agent Compatibility / 跨Agent兼容性

This skill can be used by different agents with automatic workspace adaptation:

```
Agent A workspace:
  ~/.openclaw/workspace-data_analyzer/
  ├── project-history.md
  └── data_analyzer/

Agent B workspace:
  ~/.openclaw/workspace-mlbots/
  ├── project-history.md
  └── baseball_project/
```

Path `./project-history.md` automatically resolves to the corresponding workspace.

## Permission Notes / 权限说明

| Permission / 权限 | Scope / 范围 |
|-------------------|-------------|
| Read / 读取 | `project-history.md` in current workspace |
| Write / 写入 | Create new folders, update history in current workspace |
| No access / 不访问 | System files, credentials, other agents' files |

## Notes / 注意事项

1. **Don't guess / 不要猜测**: Ask user if unsure / 无法判断时，询问用户
2. **Keep clean / 保持整洁**: One project, one folder / 一个项目一个文件夹，不混放
3. **Record history / 记录历史**: Must update `project-history.md` when creating new projects / 每次新建项目必须更新
4. **Update timing / 更新时机**: Update `last_used` after each project use / 每次使用项目后更新 `last_used`
5. **Confirm first / 确认优先**: Confirm before executing / 模糊匹配时先确认，再执行
6. **Bilingual / 双语兼容**: Support both Chinese keyword mapping and English key matching / 同时支持中文关键词映射和英文key直接匹配
7. **Workspace isolation / workspace隔离**: Different agents' project histories are independent / 不同Agent的项目历史互相独立
8. **Regular archival / 定期归档**: Check monthly, archive projects with `last_used` > 3 months (keep actual files) / 建议每月检查一次，将长期未用的项目（last_used > 3个月）从 history 中标记为归档，实际文件保留不动
