# config 目录

本目录用于保存**当前机器可读配置真源**，例如：

```text
config/
├── model_registry.yaml
├── tool_registry.yaml
├── data_manifest.yaml
└── profiles/
```

规则：

1. 只保留当前正式配置；
2. 不使用 `v1/v2/final` 文件名堆叠历史；
3. 旧配置通过 Git 历史追溯；
4. secrets 不得提交；
5. ChatGPT/Agent 若讨论“当前配置是什么”，优先读取这里的 resolved / frozen 配置，而不是从旧代码推断；
6. 普通工程字段可由 Agent 冻结，但会改变正式项目口径的参数必须遵守 `docs/协作协议.md` 的人工在环规则。
