# template_chatgpt_github_codex

这是一个用于 **用户（Human）+ ChatGPT + GitHub + 本地 Codex Agent** 协作的模板仓库。

目标不是把聊天记录搬进 GitHub，而是让 GitHub 成为双方共享的**当前事实与任务通信总线**：

```text
用户
  ↓
ChatGPT（规划 / 解惑 / 外部研究）
  ↓
GitHub：任务.md
  ↓
Codex Agent（本地执行 / 代码 / 实验）
  ↓
GitHub：agent汇报.md
  ↓
ChatGPT
  ↓
GitHub：chatgpt解惑.md
  ↓
Codex Agent继续执行
```

真正无法由 ChatGPT 或 Agent 决定的问题才升级给用户。

---

## 1. 最重要的原则

### 1.1 Git 保存历史，HEAD 只表达当前有效事实

不要通过文件名保存历史版本：

```text
❌ 技术规范_v1.md
❌ 技术规范_v2.md
❌ 技术规范_final.md
❌ agent汇报_20260903.md
```

应该始终原位更新：

```text
✅ docs/技术规范.md
✅ coordination/PR-8/agent汇报.md
```

旧内容已经存在于 Git commit history 中，不需要继续留在当前 HEAD 污染模型上下文。

### 1.2 一个 PR = 一个可以关闭的总任务

每个开放 PR 都对应：

```text
coordination/PR-<PR号>/
├── 任务.md
├── agent汇报.md
└── chatgpt解惑.md
```

PR 总任务完成后才允许进入合并流程。PR 合并后，该 PR 的 coordination 目录应尽快从 HEAD 清理；历史仍保存在 Git 中。

### 1.3 三个文件承担不同职责

| 文件 | 性质 | 主要写入者 | 更新方式 |
|---|---|---|---|
| `任务.md` | 累积任务合同 | ChatGPT 创建/追加；Agent 改状态 | **只增不减** |
| `agent汇报.md` | Agent 当前现实快照 | Codex Agent | **每次覆盖** |
| `chatgpt解惑.md` | ChatGPT 当前决策/解答 | ChatGPT | **每次覆盖** |

### 1.4 不要让局部 BLOCKED 停止整个项目

Agent 遇到 blocker 后：

```text
工程实现问题 → Agent 自行解决
规划/解释问题 → ChatGPT 解决
会改变科研/产品核心口径的问题 → 请求用户
```

ChatGPT 收到 blocker 后必须走向以下三种结果之一：

```text
A. 直接解决
B. 明确授权 Agent 自行决定
C. 明确请求用户做最小化决策
```

禁止只回复“先解决 blocker”。

---

## 2. 仓库结构

```text
.
├── README.md
├── docs/
│   ├── 项目总览.md
│   ├── 技术规范.md
│   └── 协作协议.md
├── coordination/
│   ├── README.md
│   ├── coordination.yaml
│   └── TEMPLATE/
│       ├── 任务.md
│       ├── agent汇报.md
│       └── chatgpt解惑.md
├── config/
│   └── README.md
├── artifacts/
│   └── README.md
└── .github/
    └── PULL_REQUEST_TEMPLATE.md
```

新项目从这个模板创建后，先填写：

1. `docs/项目总览.md`
2. `docs/技术规范.md`
3. `coordination/coordination.yaml`

不要一开始就创建大量专项文档。

---

## 3. ChatGPT 和 Agent 各自能看到什么

### ChatGPT 默认可以看到

- 已 push 到 GitHub 的代码、文档、PR、commit、diff；
- `任务.md`；
- `agent汇报.md`；
- `chatgpt解惑.md`；
- Git 历史；
- 必要时可做外部网络研究。

### ChatGPT 默认看不到

- Agent 本地未 push 文件；
- working tree / uncommitted diff；
- 本地终端、运行进程、GPU 状态；
- 本地 run 目录、日志、cache、dataset；
- 本地 secrets。

因此，Agent 请求 ChatGPT 判断本地问题时，必须把**足够的最小上下文**写入 `agent汇报.md`。

### Codex Agent 默认可以看到

- 本地仓库和工作树；
- 本地代码、dataset、模型、日志、运行进程；
- CPU/GPU/磁盘；
- 本地 config / manifest / artifact。

### Codex Agent 默认看不到

- 用户刚刚在 ChatGPT 对话里说但尚未写入 GitHub 的决定；
- ChatGPT 在其他 conversation 中的隐藏上下文；
- 用户与 ChatGPT 未同步到当前仓库的信息。

因此 Agent 不得猜测“ChatGPT 应该知道什么”，正式执行以 GitHub 当前真源为准。

---

## 4. 一个 PR 的标准生命周期

### Step 1：建立 PR

PR 创建后，把：

```text
coordination/TEMPLATE/
```

复制为：

```text
coordination/PR-8/
```

### Step 2：ChatGPT 填写任务

示例：

```markdown
# [ ] PR #8 总任务：完成正式数据转换并生成可训练数据池

- [x] T8.1 扫描原始数据
- [~] T8.2 全量转换 Tool-Star
- [!] T8.3 确定 Search-R1 正式来源
- [ ] T8.4 生成 accepted candidate pool
- [ ] T8.5 输出统计报告
```

状态统一为：

```text
[ ] TODO
[~] RUNNING
[x] DONE
[!] BLOCKED
[?] WAITING_USER
[-] SUPERSEDED
```

### Step 3：Agent 执行并覆盖 `agent汇报.md`

示例：

```markdown
# Agent 当前汇报

## 当前
- PR: #8
- branch: data/full-conversion
- commit: abc1234

## 正在执行
T8.2 Tool-Star 全量转换

processed: 31240 / 53971
accepted: 21904
rejected: 1277

## 当前 blocker
Search-R1 有两个 10K 文件，选择不同来源会改变正式数据口径。

## 不受影响工作
Tool-Star 继续运行；ReTool 已完成。

## 需要 ChatGPT
只需要决定 Search-R1 使用 A 还是 B。
```

### Step 4：ChatGPT 覆盖 `chatgpt解惑.md`

示例：

```markdown
# ChatGPT 当前解答

## 基于
PR #8 / commit abc1234

## 结论
Search-R1 使用 A。

## Agent 已获得的权限
manifest 路径、seed、row-id 编码、run 目录等工程细节由 Agent 自行冻结，无需再次请示。

## 立即执行
1. Tool-Star 继续运行；
2. Search-R1 使用 A 全量转换；
3. 不新增 smoke。

## 需要用户决定
无。
```

### Step 5：完成 / 合并

所有仍有效子任务完成后：

```text
# [x] PR #8 总任务：...
```

表示业务上具备合并条件；仍需正常检查 CI、冲突和用户要求。

PR 合并后，`coordination/PR-8/` 不应永久留在 HEAD。Git 历史已经保存全部交接过程。

---

## 5. “应该怎么做”和“实际上做到哪里”是两套优先级

### 规范性问题：应该怎么做？

```text
用户最新明确指令
> 当前 PR 的 任务.md
> 当前 chatgpt解惑.md
> docs/技术规范.md
> 当前正式 config / manifest
> Git 历史
> 模型记忆
```

### 现实状态问题：实际做到哪里？

```text
当前 branch / commit / runtime evidence
> 最新 agent汇报.md
> 任务.md 的状态
> 其他当前文档
> 历史记录
```

旧代码不能反向定义当前规范。

---

## 6. 旧代码 / Runner 的规则

旧 runner、旧 prompt、旧 config、历史 smoke 只能：

```text
阅读 → 理解经验 → 参考实现
```

不能因为“以前跑通过”就直接成为正式当前实现。

当前实现必须从：

```text
任务.md + chatgpt解惑.md + 当前技术规范
```

重新推导需求。

只有被当前规范明确认定为 `current approved component` 的组件才可以直接复用。

---

## 7. 人工在环

用户拥有最终覆盖权。

通常：

### Agent 自行决定

- 文件路径；
- manifest schema；
- deterministic seed；
- row ID 表示；
- JSON/JSONL；
- retry / logging / resume；
- 普通 bug；
- 模块拆分。

### ChatGPT 可以决定

- 任务优先级；
- 哪些工作并行；
- blocker 是否真的阻塞主线；
- 旧实现是否被新规范覆盖；
- 普通工程方案是否符合当前规划。

### 必须请求用户

会改变项目核心结论或实验/产品定义的问题，例如：

- 数据源性质；
- 正式训练数据规模/比例；
- 模型替换；
- reward / benchmark / tool cap；
- 核心方法路线；
- 论文/产品核心 claim。

请求用户时必须把问题缩到最小，并给出推荐，而不是把整份设计重新丢给用户。

---

## 8. 为未来事件触发器预留的协议

模板已经在 `coordination/coordination.yaml` 中预留事件字段。

建议 commit / PR 更新带来源信息：

```text
Coordination-Origin: agent
Coordination-Client: local-agent-01
Coordination-Event-Id: evt_xxx
```

或：

```text
Coordination-Origin: chatgpt
Coordination-Client: web-gpt-01
Coordination-Event-Id: evt_xxx
```

未来云端 Coordination Hub 可以据此实现：

```text
Agent 更新 PR → 只唤醒 ChatGPT
ChatGPT 更新 PR → 只唤醒 Agent
```

Trigger prompt 只负责“唤醒”，**绝不能成为新的任务真源**。

推荐唤醒提示：

### 唤醒 ChatGPT

```text
GitHub 协作事件：PR #<N> 已由 Agent 更新。
请读取该 PR 最新 diff、任务.md 与 agent汇报.md，按照 ChatGPT 协作职责处理。
本事件只负责唤醒；GitHub 当前文件才是正式任务与状态真源。
```

### 唤醒 Agent

```text
GitHub 协作事件：PR #<N> 已由 ChatGPT 更新。
请 fetch/pull 最新远端状态，读取任务.md 与 chatgpt解惑.md，核对本地实际状态后继续执行。
本事件只负责唤醒；不要重复已完成任务。
```

---

## 9. 新项目使用清单

- [ ] 填写 `docs/项目总览.md`
- [ ] 填写 `docs/技术规范.md`
- [ ] 配置 `coordination/coordination.yaml`
- [ ] 删除模板注释中与当前项目无关的示例
- [ ] 创建第一个 PR
- [ ] 为 PR 建立 `coordination/PR-<N>/`
- [ ] ChatGPT 写 `任务.md`
- [ ] Agent 开始执行并维护 `agent汇报.md`
- [ ] ChatGPT 用 `chatgpt解惑.md` 处理阻塞
- [ ] PR 完成后合并并清理该 coordination 目录

这套模板的核心不是“写更多文档”，而是让双方永远知道：**现在该读哪三个文件、谁有权改什么、什么问题应该自己解决、什么问题必须升级。**
