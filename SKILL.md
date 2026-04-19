---
name: smart-workspace
version: 1.1.0
description: 轻量化的项目文件管理技能。自动管理智能体工作区，根据任务关键词匹配项目历史，智能决定使用现有文件夹或创建新文件夹。避免文件混乱，大大增强智能体的项目管理能力。
description_en: Lightweight project file management skill. Automatically manages agent workspace, matches task keywords to project history, intelligently decides whether to use existing folders or create new ones. Prevents file chaos, greatly enhances agent project management capabilities.
keywords: [文件整理, 项目管理, workspace, folder, organize, project]
metadata:
  openclaw:
    emoji: "📁"
---

# Smart Workspace Organizer 📁 / 智能项目文件组织助手

Lightweight project file management skill. / 轻量化的项目文件管理技能。

## First-Time Setup / 首次安装（自动初始化）

> **首次加载此技能时，自动扫描 workspace 现有文件夹，生成 PROJECT-HISTORY.md！**

```
EN: If ./PROJECT-HISTORY.md does not exist:
    1. Scan all folders in workspace root directory
    2. Filter out system folders (.git, .openclaw, memory, skills, etc.)
    3. For each real project folder found, create an entry in PROJECT-HISTORY.md
    4. Set created = today, last_used = today
    5. Continue to normal workflow

CN: 如果 ./PROJECT-HISTORY.md 不存在：
    1. 扫描 workspace 根目录下所有文件夹
    2. 过滤系统文件夹（.git、.openclaw、memory、skills 等）
    3. 为每个真实项目文件夹创建条目，存入 PROJECT-HISTORY.md
    4. created = 今天日期，last_used = 今天日期
    5. 继续执行主流程
```

### 自动扫描逻辑

```
扫描 workspace/ 根目录
    ↓
发现 ./articles/、./data-science-tutorial/ 等文件夹
    ↓
过滤系统文件夹：.git, .openclaw, memory, skills, .learnings 等
    ↓
为每个真实项目生成 PROJECT-HISTORY.md 条目
    ↓
继续执行主流程
```

## Core Features / 核心功能

| # | English | 中文 |
|---|---------|------|
| 1 | **Project History Search** - Match keywords in `PROJECT-HISTORY.md` | **项目历史搜索** - 在 `PROJECT-HISTORY.md` 中匹配关键词 |
| 2 | **Smart Folder Recommendation** - Directly use matched folder | **智能文件夹推荐** - 直接使用匹配到的文件夹 |
| 3 | **New Project Creation** - Create folders and record when needed | **新项目创建** - 必要时创建新项目文件夹并记录 |
| 4 | **History Updates** - Keep project history current | **历史记录更新** - 保持项目历史最新 |

## Project History File / 项目历史文件

Location / 位置: `workspace/PROJECT-HISTORY.md`

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

Project history: ~/.openclaw/workspace-[agentname]/PROJECT-HISTORY.md
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

## Execution Flow / 执行流程（全自动）

### Step 1: Initialize (First Time Only) / 初始化（仅首次）

```
IF ./PROJECT-HISTORY.md 不存在:
    扫描 workspace 根目录所有文件夹
    过滤系统文件夹（.git、.openclaw、memory、skills、.learnings 等）
    为每个真实项目创建初始条目
    继续 Step 2
ELSE:
    直接继续 Step 2
```

### Step 2: Understand Task / 理解任务

```
EN: Analyze user's task, extract 2-3 keywords, determine project type
CN: 分析用户任务，提取2-3个关键词，判断属于哪类项目
```

### Step 3: Search & Match / 搜索匹配

```
读取 PROJECT-HISTORY.md
    ↓
匹配规则（优先级顺序）：
    1. key 完全匹配 → 直接使用
    2. name/desc 包含关键词 → 直接使用
    3. 中文关键词映射命中 → 直接使用
    4. 无匹配 → 创建新项目
    5. 多个模糊匹配 → 使用第一个，并向用户说明
```

### Step 4: Execute & Record / 执行与记录

```
【已有项目】
    1. 创建 ./[project]/ 文件夹（如不存在）
    2. 更新 PROJECT-HISTORY.md 中 last_used = 今天日期
    3. 回复："已将此任务整理到 [项目名]（last_used: 今天）"

【新项目】
    1. 创建 ./[project-name]/ 文件夹
       - 如文件夹已存在，自动追加 _1, _2 后缀
    2. 在 PROJECT-HISTORY.md 追加新条目
    3. 回复："已创建新项目 [项目名]，文件将整理到此目录"
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

### Case 1: Match Found → Direct Use / 匹配成功 → 直接使用

**User / 用户**: "帮我分析今天足球数据" / "Analyze today's football data"

**Agent / 智能体**:
```
[扫描] ./PROJECT-HISTORY.md（如不存在则初始化）
[提取关键词] "足球" → football
[匹配结果] football -> ./data_analyzer/
  created: 2026-03-01, last_used: 2026-04-18

CN: "已将此任务整理到 data_analyzer/（last_used: 今天）"
EN: "Task organized to data_analyzer/ (last_used: today)"
```

### Case 2: No Match → Auto Create / 无匹配 → 自动创建

**User / 用户**: "我要开发一个新的推荐算法" / "I want to develop a new recommendation algorithm"

**Agent / 智能体**:
```
[提取关键词] "推荐"
[匹配结果] 无匹配

1. 创建 ./recommender/ 文件夹
2. 在 PROJECT-HISTORY.md 追加新条目
3. 回复："已创建新项目 recommender，文件将整理到此目录"

CN: "已创建新项目 recommender，文件将整理到此目录"
EN: "Created new project recommender, files will be organized here"
```

### Case 3: New Project in Existing Folder / 新项目在已有文件夹中

**User / 用户**: "做一下射击项目的数据分析" / "Do data analysis for the shooting project"

**Agent / 智能体**:
```
[提取关键词] "射击" → shooting
[匹配结果] shooting -> ./shooting/

CN: "已将此任务整理到 shooting/（last_used: 今天）"
EN: "Task organized to shooting/ (last_used: today)"
```

## Test Cases / 测试案例

### ✅ Test 1: Football Analysis / 足球分析
```
Input: "分析今天足球数据" / "Analyze today's football data"
Expected: Match football → 直接使用 ./data_analyzer/，更新 last_used
```

### ✅ Test 2: Battery Research / 电池研究
```
Input: "做一下电池测试分析" / "Do battery test analysis"
Expected: Match battery → 直接使用 ./battery-ana/，更新 last_used
```

### ✅ Test 3: New Project / 新项目
```
Input: "开发一个天气预测系统" / "Develop a weather prediction system"
Expected: No match → 自动创建 ./weather-prediction/ 文件夹
```

### ✅ Test 4: First-Time Setup / 首次安装
```
Scenario: Agent has folders ./articles/, ./data-science/, memory/, .git/
Expected: 
  - 过滤 .git, memory, skills 等系统文件夹
  - 为 articles, data-science 创建 PROJECT-HISTORY.md 条目
  - created = today, last_used = today
```

## Cross-Agent Compatibility / 跨Agent兼容性

This skill can be used by different agents with automatic workspace adaptation:

```
Agent A workspace:
  ~/.openclaw/workspace-data_analyzer/
  ├── PROJECT-HISTORY.md
  └── data_analyzer/

Agent B workspace:
  ~/.openclaw/workspace-mlbots/
  ├── PROJECT-HISTORY.md
  └── baseball_project/
```

Path `./PROJECT-HISTORY.md` automatically resolves to the corresponding workspace.

## Permission Notes / 权限说明

| Permission / 权限 | Scope / 范围 |
|-------------------|-------------|
| Read / 读取 | `PROJECT-HISTORY.md` in current workspace |
| Write / 写入 | Create new folders, update history in current workspace |
| No access / 不访问 | System files, credentials, other agents' files |

## Notes / 注意事项

1. **Be direct / 直接处理**: If match found, just use it and tell the user / 如匹配到项目，直接使用并告知用户
2. **Auto-create / 自动创建**: If no match, create new folder without asking / 无匹配时直接创建新文件夹，无需询问
3. **Update always / 始终更新**: Always update `last_used` when using existing project / 使用已有项目时必须更新 last_used
4. **Record new projects / 记录新项目**: Must update `PROJECT-HISTORY.md` when creating new projects / 每次新建项目必须更新记录
5. **Bilingual / 双语兼容**: Support both Chinese keyword mapping and English key matching / 同时支持中文关键词映射和英文key直接匹配
6. **Workspace isolation / workspace隔离**: Different agents' project histories are independent / 不同Agent的项目历史互相独立
7. **System folder filter / 系统文件夹过滤**: Skip .git, .openclaw, memory, skills, .learnings, .env, .DS_Store / 跳过系统文件夹
8. **Regular archival / 定期归档**: Check monthly, archive projects with `last_used` > 3 months / 建议每月检查一次归档长期未用的项目
