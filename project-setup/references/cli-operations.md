# CLI 操作参考

本文件包含新建项目时各步骤的具体 CLI 命令和工具使用细节。SKILL.md 中的流程步骤会在需要时指向本文件。

---

## 目录创建

使用 `mkdir -p` 创建所有目录。`00_上下文/` 和 `07_归档/` 下的文件会在后续步骤中通过 obsidian CLI 创建，目录会自动生成；其余空目录需要 mkdir 提前建好。

```bash
PROJECT="项目/[项目名]"
mkdir -p "$PROJECT/00_上下文" "$PROJECT/01_主题" "$PROJECT/02_形式" \
         "$PROJECT/03_世界" "$PROJECT/04_人物" "$PROJECT/05_叙事" \
         "$PROJECT/06_执行/草稿" "$PROJECT/07_归档"
```

---

## 初始文件创建

### 上下文文件（基于模板）

使用 `template` 参数直接从 Obsidian 模板创建，这是最高效的方式：

```bash
# 创建项目简报
obsidian create path="项目/[项目名]/00_上下文/项目简报.md" template="AI上下文模板" silent

# 创建当前阶段
obsidian create path="项目/[项目名]/00_上下文/当前阶段.md" template="AI上下文模板" silent
```

创建后需要用 Edit 工具填充项目特定信息（项目名称、媒介类型、日期等），因为模板只提供结构骨架。

**回退方案：** 如果 `template` 参数失败，先用 `obsidian read path="模板/AI上下文模板.md"` 读取模板内容，再用 Write 工具创建文件。CLI 命令长度超过约 200 字符时容易失败，长内容建议使用 Write 工具。

### 归档文件（简短内容）

归档文件只需 frontmatter 和标题，内容短可以直接用 CLI：

```bash
obsidian create path="项目/[项目名]/07_归档/探索记录.md" content="---\n最后更新: [今天日期]\n---\n\n# 探索记录" silent
obsidian create path="项目/[项目名]/07_归档/废弃路径.md" content="---\n最后更新: [今天日期]\n---\n\n# 废弃路径" silent
obsidian create path="项目/[项目名]/07_归档/风险清单.md" content="---\n最后更新: [今天日期]\n---\n\n# 风险清单" silent
```

如果 CLI 不可用，使用 Write 工具创建，内容参见 [templates.md](templates.md)。

### 添加双向链接

文件创建后，为上下文文件互相添加链接：

```bash
obsidian append path="项目/[项目名]/00_上下文/项目简报.md" content="\n\n---\n相关链接：[[当前阶段]]" silent
obsidian append path="项目/[项目名]/00_上下文/当前阶段.md" content="\n\n---\n相关链接：[[项目简报]]" silent
```

### 验证

```bash
obsidian ls path="项目/[项目名]/00_上下文"
obsidian ls path="项目/[项目名]/07_归档"
```

---

## 第一次会话结束时的文档更新

### 更新当前阶段.md

**短内容用 obsidian CLI：**

```bash
obsidian append file="当前阶段" content="- [新增待处理项]" silent
obsidian insert path="项目/[项目名]/00_上下文/当前阶段.md" after="## 已完成" content="\n- [本次完成的任务]" silent
```

**长内容用 Edit 工具。**

### 追加探索记录.md

```bash
obsidian append file="探索记录" content="\n## [日期] 第一次会话\n\n**讨论内容：**\n\n**主要发现：**\n\n**达成的结论：**\n\n**未解决的问题：**\n\n**下次会话的起点：**" silent
```

内容较长时用 Edit 工具。格式参见 [templates.md](templates.md) 末尾的"会话结束时的探索记录格式"。

### 更新 frontmatter 日期

使用 Edit 工具直接修改 YAML frontmatter 中的「最后更新」字段：

```markdown
# 将
最后更新: 2026-03-01
# 改为
最后更新: 2026-03-02
```

---

## 阶段推进时的模板调用

进入新阶段需要创建阶段文档时，使用 `obsidian create ... template="[模板名]" silent` 从模板创建。

详细的模板调用流程（包括回退方案、双向链接添加、各阶段对应模板表）见：
`enter-existing-project` skill 的 `references/template-workflow.md`

两个 skill 共享同一套模板调用规范，避免重复维护。
