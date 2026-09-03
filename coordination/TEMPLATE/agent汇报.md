# Agent 当前汇报

> 本文件是**即时现实快照**，由本地 Codex Agent 维护。
> 每次重要 push 时完整覆盖旧内容；不要追加历史记录。
> 必须假设 ChatGPT 看不到本地电脑，因此需要提供足够的最小上下文，但不得写 secrets。

## 当前身份

- PR: `#<N>`
- branch: `<branch>`
- commit: `<sha>`
- 本地仓库: `<path>`

## 当前任务

- 任务 ID: `T<N>.<M>`
- 状态: `[~] / [!] / [?] / [x]`

## 本次完成

1. `<做了什么>`
2. `<做了什么>`

## 代码变化

- `<path>`：`<修改目的>`
- `<path>`：`<修改目的>`

## 本地输入 / 运行环境

- dataset / input: `<...>`
- model: `<...>`
- config / manifest: `<...>`
- runtime: `<...>`

## 实际运行

命令：

```bash
<command>
```

run directory / artifact：

```text
<path or artifact ref>
```

## 当前结果

```text
processed = ...
accepted = ...
rejected = ...
failures = ...
```

如适用，补充关键指标。

## 当前问题 / BLOCKER

`<没有则写“无”>`

如果存在 blocker，必须说明：

- 具体在哪里发生；
- 错误或冲突是什么；
- 用到了哪些文件/代码；
- 已经尝试过什么；
- 为什么 Agent 无法在不改变正式口径的情况下自行解决。

## 已尝试

1. `<...>`
2. `<...>`

## 不受影响、仍可继续的任务

- `<任务>`
- `<任务>`

局部 blocker 不得默认阻塞整个 PR。

## 需要 ChatGPT 回答

只提出**最小问题**。

例如：

```text
Search-R1 有 A/B 两个不同来源的 10K 文件；选择不同 source 会改变正式数据来源。
除此之外 manifest 的 seed/path/schema 等工程字段我已自行冻结。
请只决定 A 还是 B；我推荐 A，因为……
```

## 示例：好的汇报

```text
PR #8 / commit abc1234
T8.2 Tool-Star 全量转换正在运行
processed 31240/53971, accepted 21904, rejected 1277
ReTool 1000 已完成

Blocker 只影响 Search-R1：A/B 两个 10K source 来自不同生成模型，当前规范未明确哪个是正式来源。
Tool-Star 仍继续执行，PR 未整体停止。
需要 ChatGPT 只决定 Search-R1 A/B。
```
