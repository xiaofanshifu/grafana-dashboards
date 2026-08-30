# Grafana Dashboards

本仓库存放 Grafana Dashboard 的 JSON 文件，包含容器监控、主机监控以及 LLM 推理服务器（vLLM / SGLang）监控面板。

## Dashboard 列表

| 文件 | 名称 | 说明 |
|---|---|---|
| `cadvisor.json` | Cadvisor exporter v2 | 容器级指标：CPU、内存、网络、磁盘 IO 等 |
| `node.json` | node_exporter_full | 主机级指标：CPU、内存、磁盘、网络、TCP/UDP/ICMP 错误、IRQ、进程等 |
| `llm1.json` | vLLM 推理服务器监控 | 请求成功率、活跃请求数、TTFT P99、抢占率、请求耗时 P50/P99、Token 吞吐等 |
| `llm2.json` | vLLM 监控（中文汉化版） | 调度器效率、KV Cache 使用率、TTFT/TPOT 延迟、Prefix Cache 命中率、请求完成原因分布 |
| `llm3.json` | 统一 LLM 推理监控（SGLang + vLLM） | 请求量、Token 吞吐、延迟分位数、队列状态、缓存行为、API Server QPS |

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
  - `llm1.json` / `llm2.json` — 需要 vLLM 开启 metrics 端点（`vllm:request_*` 等指标）。
  - `llm3.json` — 需要 vLLM 和/或 SGLang 的 metrics 端点。

## 查找更多 Dashboard

所有 Dashboard 都可以在 [Grafana.com Dashboards](https://grafana.com/grafana/dashboards/) 上搜索并下载。