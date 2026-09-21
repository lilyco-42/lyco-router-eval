# lyco-router-eval：0.6B 端侧命令路由器的独立横向评测

> “别再用爱因斯坦帮你关灯”——关灯、调温、查 issue 这类任务，到底需要多少智能？
> 本仓用 40+ 组实测回答：**0.6B 够了，但得配三件套（拒答 + 白名单 + 人工拍板）。**

评测对象：[`lyco42/lyco-agent-qwen3-0.6b-ondevice`](https://huggingface.co/lyco42/lyco-agent-qwen3-0.6b-ondevice)
（Qwen3-0.6B SFT，GGUF Q4_K_M，Apache-2.0）。
评测机：llama.cpp b11045，temp 0，thinking 关；780M Vulkan（~85 TPS）/ 桌面 CPU（~78 TPS）/ 安卓模拟器（11 TPS）三档。

## 一句话结论

| 结论 | 证据 |
|---|---|
| 关灯类任务签收当快脑 | 空调/灯/风扇，域动作槽位全对 |
| 跨域首步可用，子命令半对 | rust/k8s/docker/tf/npm，schema+few-shot 可扶正 |
| 拒答可靠，危险不拒 | noop 场景正确；`cargo publish`/`rm` 照给 |
| 否定词零可靠 | 反义/乱躲/硬上，全看运气，执行层必须正则拦截 |
| 人设进 sys 会带偏路由 | `kubectl get pods` 退化成 `gh pods`，人设归 UI |
| 未知工具硬套最近项 | yt-dlp→ff；白名单治 |
| 确定性 3/3，多轮 KV 命中 | temp 0 可复现，并发无压力 |

## 报告与复现

- 全量结论：[REPORT.md](REPORT.md)
- `prompts/` 下 44 个文件即全部用例（`router_p*.txt` 输入、`router_sys*.txt` 系统词、`router_t*.json` 多轮），同配方可复跑
- 出货配方（App 实测）：sys = 卡片原配方 + 能力表 + 2 个只读 few-shot；`enable_thinking=false` 必传，否则空 think 收工

## 与模型卡片的关系

卡片自述全部复现通过，未发现矛盾项；新增发现（否定零可靠、复合意图缝合、人设污染、few-shot 安全跷跷板）均已同步为执行层铁律，详见报告 §4。
