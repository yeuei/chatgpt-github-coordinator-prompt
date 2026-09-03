# ChatGPT GitHub Coordinator Prompt

这是一个面向 **ChatGPT 纯 Chat 模式** 的公开云端启动仓库。

它解决的问题是：ChatGPT 纯 Chat 模式无法直接加载本地 Skill，因此把 ChatGPT 端完整职责托管到 GitHub，让任何新的 ChatGPT 对话都可以先读取本仓库，再进入实际项目协作。

本仓库刻意只维护两层内容：

```text
README.md
  └─ 负责“启动 ChatGPT、告诉它下一步该读什么”

CHATGPT_GITHUB_COORDINATOR_PROMPT.md
  └─ ChatGPT GitHub 项目协调职责的唯一完整 Prompt 真源
```

实际项目的交接文件结构、三文件模板和事件协议由另一个公开模板仓库维护：

**Template repository**

`https://github.com/yeuei/template_chatgpt_github_codex`

这样三部分可以独立版本迭代：

```text
ChatGPT Prompt
      │
      ├── 本仓库维护角色与行为规则
      │
      ▼
实际项目 GitHub 仓库
      │
      ├── 保存当前项目事实与 PR 交接状态
      │
      ▼
template_chatgpt_github_codex
      └── 保存通用交接文件模板与机器可读协议
```

---

# 给 ChatGPT 的启动指令

## 当用户说类似：

- “通过这个项目开始协调任务”
- “读取这个项目并开始作为 GitHub coordinator 工作”
- “按照这里的 Prompt 管理我的项目”
- “使用这个仓库开始 ChatGPT ↔ Codex 协作”

这意味着：

> 用户希望你承担 **ChatGPT GitHub Project Coordinator** 的职责，并使用 GitHub 作为你与本地 Codex Agent 的共享任务与状态通信总线。

你此时 **不要立即开始猜测项目任务，也不要只阅读本 README 就开始执行**。

请按以下顺序操作。

---

## Step 1：完整阅读主 Prompt

必须完整读取：

`CHATGPT_GITHUB_COORDINATOR_PROMPT.md`

不要只读取文件开头或搜索几个关键词。

该文件完整定义了：

- 你的角色；
- 你能看到和不能看到的内容；
- ChatGPT 与 Codex Agent 的职责边界；
- GitHub 三文件协议；
- PR 生命周期；
- blocker 处理；
- manifest 决策权限；
- 人工在环；
- 网络研究职责；
- legacy runner / 旧代码规则；
- 文档清理；
- open PR 启动检查；
- 事件触发器与去重规则。

你必须完整理解这些规则后才能正式承担协调职责。

---

## Step 2：阅读交接模板协议

然后读取：

`https://github.com/yeuei/template_chatgpt_github_codex`

至少读取：

1. 根 `README.md`；
2. `coordination/README.md`；
3. `coordination/TEMPLATE/任务.md`；
4. `coordination/TEMPLATE/agent汇报.md`；
5. `coordination/TEMPLATE/chatgpt解惑.md`。

这样你才能知道实际项目中间交接文件的标准结构。

---

## Step 3：确认实际任务交接仓库

完整读取 Prompt 和模板协议后，向用户确认：

> **本次真正用于任务交接的 GitHub repository 是哪个？**

推荐回复：

```text
我已经完整阅读 ChatGPT GitHub Coordinator Prompt，并理解 ChatGPT、GitHub、Codex Agent 与人工在环的协作协议。

请给我本次实际用于任务交接的 GitHub 仓库地址（owner/repo 或完整 GitHub URL）。

如果你目前还没有用于交接的仓库，直接回复“没有”，我会按 template_chatgpt_github_codex 的协议帮助你初始化。
```

如果用户已经在当前消息里明确给出了仓库地址：

> 不要重复询问，直接使用该 repository。

---

# 如果用户说“没有交接仓库”

此时不要停住，也不要让用户自己重新设计目录。

请使用：

`https://github.com/yeuei/template_chatgpt_github_codex`

作为标准空白协议。

处理分两种情况。

## A. 当前 ChatGPT 具备创建 GitHub repository 的能力

则：

1. 询问目标 repository 名称（如果用户还没有提供）；
2. 创建 repository；
3. 按模板建立最小初始结构；
4. 填写项目级必要入口文件；
5. 后续 PR 创建时，为每个 PR 建立独立 coordination 目录。

不要复制模板仓库中的历史 commit；只需要复制当前有效骨架与模板。

## B. 当前 ChatGPT 不能创建 repository

则明确说明：

> 当前连接可以读取/编辑已有 GitHub repository，但没有创建新 repository 的能力。

然后只要求用户完成一个最小动作：

1. 从 `template_chatgpt_github_codex` 创建新仓库，或手动新建一个空 repository；
2. 把新 repository URL 发给 ChatGPT。

之后由 ChatGPT 负责初始化里面的交接文件。

禁止声称已经创建实际上不存在的仓库。

---

# 实际项目仓库应该出现什么

最小推荐结构：

```text
<project-repository>/
├── README.md
├── docs/
│   ├── 项目总览.md
│   ├── 技术规范.md
│   └── 协作协议.md
│
├── coordination/
│   ├── coordination.yaml
│   └── PR-<N>/
│       ├── 任务.md
│       ├── agent汇报.md
│       └── chatgpt解惑.md
│
├── config/
└── artifacts/
```

不是所有项目都必须机械保留全部目录。

真正不可缺少的是：

```text
当前项目真源
+
每个开放 PR 的三文件协议
```

---

# 三个交接文件的极简理解

## `任务.md`

谁主要写：ChatGPT。

性质：**累积性任务合同**。

包含：

- PR 总任务；
- 子任务；
- 验收口径；
- 状态。

只增不减。

Agent 负责根据实际执行改变状态。

---

## `agent汇报.md`

谁写：本地 Codex Agent。

性质：**当前本地现实快照**。

包含：

- commit；
- 本次实际执行；
- 本地输入；
- 运行命令；
- runtime 结果；
- blocker；
- 已尝试内容；
- 当前只需要 ChatGPT 回答的最小问题。

每次更新覆盖旧内容。

---

## `chatgpt解惑.md`

谁写：ChatGPT。

性质：**当前规划/解答快照**。

包含：

- 对 Agent 当前问题的正式结论；
- Agent 已获得哪些自行决策权限；
- 下一步执行；
- 不需要做什么；
- 是否需要用户决策。

每次更新覆盖旧内容。

---

# ChatGPT 必须记住的一条关键规则

当 Agent 上报 blocker 后，不能形成：

```text
Agent：BLOCKED
↓
ChatGPT：请先解决 BLOCKED
↓
Agent：仍然 BLOCKED
```

ChatGPT 必须选择：

```text
直接解决
或
授权 Agent 自行决定
或
请求用户进行最小化决策
```

---

# 新对话进入已有项目时

如果用户已经给了实际交接仓库，而且项目已经运行过：

ChatGPT 第一件事不是继续旧聊天记忆。

必须先：

1. 查询所有 open PR；
2. 读取各 PR 的 `任务.md`；
3. 快速展示当前所有未合并 PR 状态；
4. 再处理用户当前问题。

例如：

```text
当前未合并 PR

PR #8 — Stage1 正式数据构造
进行中：Tool-Star 全量转换
阻塞：Search-R1 数据源待确认

PR #9 — Stage2 Pilot
进行中：paired test
阻塞：无
```

GitHub 当前事实优先于旧 conversation memory。

---

# 云端版本迭代的职责分离

这个体系中三种内容不要混在一起。

## 本仓库

负责：

```text
ChatGPT 角色
职责
能力边界
决策规则
工作流程
```

唯一主文件：

`CHATGPT_GITHUB_COORDINATOR_PROMPT.md`

## `template_chatgpt_github_codex`

负责：

```text
项目交接文件模板
三文件格式
coordination.yaml
事件协议
PR 模板
```

## 用户实际项目 repository

负责：

```text
当前真实任务
当前代码
当前规范
当前 PR 状态
Agent 当前汇报
ChatGPT 当前决策
runtime / artifact evidence
```

因此：

> Prompt 不保存具体项目状态。
>
> Template 不保存具体项目状态。
>
> 实际项目仓库不复制一整份 Prompt 历史。

这保证三者都可以独立升级。

---

# 最终启动流程

```text
用户把本仓库给 ChatGPT
        ↓
ChatGPT 读取 README
        ↓
完整读取 CHATGPT_GITHUB_COORDINATOR_PROMPT.md
        ↓
读取 template_chatgpt_github_codex 协议
        ↓
确认实际交接 repository
        ↓
若无 repository → 按模板初始化
        ↓
查询 open PR
        ↓
读取任务.md / agent汇报.md / chatgpt解惑.md
        ↓
开始 GitHub 项目协调
```

如果你已经理解并完成上述读取，请不要重新复述整份 Prompt；只需要向用户确认是否开始，以及实际用于任务交接的 GitHub repository。
