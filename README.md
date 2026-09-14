# Grafana Dashboards

本仓库存放 Grafana Dashboard 的 JSON 文件，包含容器监控、主机监控、天数智芯 GPU 监控以及 LLM 推理服务器（vLLM / SGLang）监控面板。

## Dashboard 列表

| 文件 | 名称 | 说明 |
|---|---|---|
| `cadvisor.json` | 容器监控 (cAdvisor) | 容器级指标：CPU、内存、网络、磁盘 IO、PSI、OOM 等 |
| `node.json` | Linux 主机监控 (Node Exporter) | 主机级指标：CPU、内存、磁盘、网络、TCP/UDP/ICMP 错误、IRQ、进程等 |
| `iluvatar.json` | 天数智芯 GPU 监控 (Iluvatar Exporter) | GPU 级指标：核心/显存利用率、显存与进程占用、温度、时钟频率、功耗、PCIe 吞吐、ECC 与 XID 错误 |
| `llm1.json` | LLM 推理监控 (vLLM/SGLang) · 核心概览 | 请求成功率、活跃请求数、TTFT P99、抢占率、请求耗时 P50/P99、Token 吞吐等 |
| `llm2.json` | LLM 推理监控 (vLLM/SGLang) · 扩展分析 | 调度器效率、KV Cache 使用率、TTFT/TPOT 延迟、Prefix Cache 命中率、请求完成原因分布 |
| `llm3.json` | LLM 推理监控 (vLLM/SGLang) · 统一视图 | 请求量、Token 吞吐、延迟分位数、队列状态、缓存行为、Engine / TP Rank 维度分布 |

## 导入方法

Grafana 中导入 dashboard：

1. 打开 Grafana，进入 **Dashboards → New → Import**。
2. 上传 JSON 文件（或粘贴 JSON 内容）。
3. 选择 Prometheus 数据源，点击 **Import**。

也可以将 JSON 文件放入 provisioning 目录挂载：

```
grafana:
  provisioning:
    dashboards:
      - type: file
        options:
          path: /var/lib/grafana/dashboards
```

## 前置依赖

- **数据源**：所有 dashboard 均依赖 Prometheus 数据源。
- **指标端点**：
  - `cadvisor.json` — 需要 [cAdvisor](https://github.com/google/cadvisor) 暴露的容器指标。
  - `node.json` — 需要 [node_exporter](https://github.com/prometheus/node_exporter) 暴露的主机指标。
  - `iluvatar.json` — 需要天数智芯 GPU exporter 暴露的 `ix_*` 指标（如 `ix_gpu_utilization`、`ix_mem_used`），指标自带 `node_name` / `gpu` / `uuid` 标签。
  - `llm1.json` / `llm2.json` — 需要 vLLM 开启 metrics 端点（`vllm:request_*` 等指标）。
  - `llm3.json` — 需要 vLLM 和/或 SGLang 的 metrics 端点。

## 查找更多 Dashboard

所有 Dashboard 都可以在 [Grafana.com Dashboards](https://grafana.com/grafana/dashboards/) 上搜索并下载。