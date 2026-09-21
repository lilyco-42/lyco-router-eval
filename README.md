# router-eval

`lyco42/lyco-agent-qwen3-0.6b-ondevice` 第三方独立横向评测：40+ 组实测，全量复现文件见 `prompts/`，结论见 [REPORT.md](REPORT.md)。

- 评测环境：llama.cpp b11045，temp 0，thinking 关
- 与模型卡片结论一致，未发现矛盾项；新增发现集中在否定可靠性、复合意图拆分、人设污染三条
