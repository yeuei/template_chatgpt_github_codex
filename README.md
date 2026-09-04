# ChatGPT GitHub ↔ Codex 交接模板实例化协议

你正在读取一个供 ChatGPT/Agent 使用的模板仓库。不要把本文件当作面向人类的介绍页；把它当作“如何识别、实例化和继续使用 GitHub 交接仓库”的协议。

本仓库是模板，不是实际项目。文件中的尖括号、示例 PR、示例统计和示例分支都不是项目事实。Git 保存历史，当前 HEAD 只表达当前有效协议和项目事实。

## 一、目录与文件语义

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
├── config/README.md
├── artifacts/README.md
└── .github/PULL_REQUEST_TEMPLATE.md
```

- `README.md`：当前交接仓库的入口协议。它必须说明本仓库是什么、如何进入项目、如何同步 ChatGPT 与 Agent；实例化后必须保留并扩写，不能只留下项目简介。
- `docs/项目总览.md`：项目目标、阶段、主线、不变量、未决项和阅读路径。创建新交接仓库后填写。
- `docs/技术规范.md`：当前技术实现与验收真源。创建新交接仓库后填写；已有正式规范时整合或链接，不要并列保留已废弃方案。
- `docs/协作协议.md`：项目可选的协作补充规则。核心交接规则仍必须能从根 README 进入。
- `coordination/README.md`：开放 PR 协作目录的使用规则。
- `coordination/coordination.yaml`：通用事件、路由、去重、触发提示和安全协议；实例化后替换占位符，但不得写入秘密。
- `coordination/TEMPLATE/`：每个真实 PR 的三文件起始模板，不代表任何真实 PR。
- `coordination/PR-<N>/`：仅对应真实存在且需要交接的 PR；其中 `任务.md` 是累积任务合同，`agent汇报.md` 是 Agent 当前现实快照，`chatgpt解惑.md` 是 ChatGPT 当前决策快照。
- `config/`：项目确实需要共享配置入口时保留；否则可删除。
- `artifacts/`：项目确实需要共享产物入口时保留；否则可删除。
- `.github/PULL_REQUEST_TEMPLATE.md`：PR 描述模板，通常保留并按项目需要扩写。

## 二、从模板创建新交接仓库

当用户选择“从模板开始”时，先进行一次且仅一次的项目意图采集，再写入项目级文件。

### 一次性 Project Intent Intake

先检查用户当前消息和前文是否已经明确说明：

- 这个交接仓库主要服务什么项目、目标或服务范围；
- 当前最先要完成什么任务或阶段。

如果已经明确到足以初始化，不要再次询问。如果没有明确，ChatGPT 只主动询问一次：

> 在初始化交接仓库前，请先告诉我一次：这个仓库主要服务什么项目，以及你现在最先想完成什么任务或阶段？你可以自然描述，我会据此初始化 README、项目总览和第一个任务。

这不是详细需求问卷。用户给出大致目标后，ChatGPT 不应连续追问普通工程细节，而应根据现有信息继续初始化。采集完成后，也不要周期性或反复主动询问项目目标；后续变化由用户主动提出，或从当前 GitHub 任务、规范和事实演化。

采集结果必须落入以下位置：

- 目标仓库根 `README.md`：在本模板核心协作协议基础上扩写“本仓库服务目标”和当前入口；
- `docs/项目总览.md`：写入项目目标、当前阶段、主线和首要工作；
- 第一个可关闭的任务合同：写入真实 PR 的 `coordination/PR-<真实编号>/任务.md`；
- 如果还没有真实 PR：只在项目总览和 README 中记录目标，不创建虚构的 PR 编号或 `coordination/PR-N/`。

初始化步骤：

1. 使用本仓库当前有效文件建立目标仓库，不复制本模板的 Git 历史。
2. 保留目标仓库的根 `README.md`，并在其基础上扩写项目入口、当前事实和本协议的核心协作规则。未来 ChatGPT 可能直接从已有交接仓库进入，不能删除这些规则。
3. 填写 `docs/项目总览.md`，并填写 `docs/技术规范.md` 或提供到正式技术规范入口的明确链接。
4. 保留 `coordination/README.md`、`coordination/coordination.yaml` 和 `coordination/TEMPLATE/`。YAML 中的仓库、项目和客户端字段改为目标项目的实际值或协议允许的占位符。
5. `coordination/TEMPLATE/` 只保留说明性占位符和通用示例结构，不保留虚构的项目状态、统计数字、PR 号、分支名、commit 或用户信息。
6. 不要预建任何虚构的 `coordination/PR-N/`。只有真实 PR 出现后，才把 `coordination/TEMPLATE/` 复制为对应的 `coordination/PR-<真实编号>/`，再填写任务和状态。
7. `config/` 与 `artifacts/` 仅在项目需要时保留；无需求时可删除其 README 和目录。
8. 删除或改写模板中与当前项目无关的示例文字，但不要删除根 README 中解释协作方式、入口选择、三文件语义和权限故障处理的核心协议。
9. 不预建版本号文件。当前规范、汇报和解惑均原位更新，历史由 Git 保存。

模板初始化完成后的最小核验：

- 根 README 能让 ChatGPT 在没有原始对话的情况下理解交接方式；
- 项目总览和技术规范入口已明确；
- YAML 没有秘密、具体模板作者账号或虚构项目值；
- 没有虚构的 `PR-N` 目录；
- 只有真实 PR 才有对应三文件目录。

## 三、已有交接仓库入口

用户可以直接提供一个已经从本模板创建、或已经交接过多次的仓库。此时：

- 不要求重新从模板复制；
- 先读取该仓库当前 README，遵守其当前项目事实和协作协议；
- 再核验 open PR、当前 HEAD、相关 `coordination/PR-<N>/`、项目总览和技术规范；
- 仅当已有 README 缺少模板协议时，才把本 README 作为补充规范；
- 不用模板示例覆盖已有项目事实，也不把模板历史当作当前状态。

## 四、标准 PR 交接循环

真实 PR 创建后，复制 `coordination/TEMPLATE/` 为 `coordination/PR-<N>/`。三个文件的职责必须分开：

- `任务.md`：ChatGPT 创建/追加任务，Agent 根据真实执行修改状态；已完成任务不删除。
- `agent汇报.md`：Agent 覆盖写入当前 branch、commit、执行、runtime 结果、阻塞和仍可继续的工作。
- `chatgpt解惑.md`：ChatGPT 覆盖写入当前结论、Agent 获得的工程自决权限、立即执行项和最小用户决策。

状态统一使用 `[ ] TODO`、`[~] RUNNING`、`[x] DONE`、`[!] BLOCKED`、`[?] WAITING_USER`、`[-] SUPERSEDED`。PR 合并后删除对应的 `coordination/PR-<N>/`，历史仍在 Git 中。

## 五、权限故障

如果仓库不可见、只能读不能写，或无法创建 branch、提交文件、创建/更新 PR，不要声称成功。提醒用户访问：

https://github.com/settings/installations

配置 GitHub App 的 repository access；或者提供一个已经拥有足够读写权限的空白交接仓库或现有交接仓库。权限恢复后，从当前 HEAD 继续核验，不要重复制造历史文件。
