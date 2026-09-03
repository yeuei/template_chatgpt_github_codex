# artifacts 目录

本目录用于保存需要被 GitHub/ChatGPT 看见的**轻量、可审计实验或执行证据**。

适合：

- 小型 manifest；
- summary.json / summary.md；
- 关键统计；
- 运行身份证据；
- hash / lineage；
- 小型失败样例；
- implementation inventory。

不适合：

- 模型权重；
- 大型数据集；
- 大型 raw logs；
- API key / secret；
- 本地隐私信息。

如果大型 artifact 不能提交 GitHub，应在 `agent汇报.md` 中写：

```text
本地 artifact path: ...
identity/hash: ...
关键统计: ...
```

并尽可能把足以让 ChatGPT判断问题的最小证据同步到本目录。
