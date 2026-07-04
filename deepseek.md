# DeepSeek API 参考文档

本指南整理了 DeepSeek V4 API 的基础配置、计费标准以及思考模式开关和强度的配置方式。

## 1. 基础配置与模型参数

| 参数 / 功能 | deepseek-v4-flash | deepseek-v4-pro |
| :--- | :--- | :--- |
| **BASE URL (OpenAI 格式)** | `https://api.deepseek.com` | `https://api.deepseek.com` |
| **BASE URL (Anthropic 格式)** | `https://api.deepseek.com/anthropic` | `https://api.deepseek.com/anthropic` |
| **模型标识 (model)** | `deepseek-v4-flash` | `deepseek-v4-pro` |
| **模型版本** | DeepSeek-V4-Flash | DeepSeek-V4-Pro |
| **思考模式** | 支持非思考与思考模式（默认开启） | 支持非思考与思考模式（默认开启） |
| **上下文长度** | 1M | 1M |
| **最大输出长度** | 384K | 384K |
| **Json Output** | 支持 | 支持 |
| **Tool Calls (工具调用)** | 支持 | 支持 |
| **对话前缀续写 (Beta)** | 支持 | 支持 |
| **FIM 补全 (Beta)** | 仅非思考模式支持 | 仅非思考模式支持 |
| **并发限制** | 2500 | 500 |

---

## 2. 计费标准 (人民币)

| 计费项目 (每百万 tokens) | deepseek-v4-flash | deepseek-v4-pro |
| :--- | :--- | :--- |
| **输入（缓存命中）** | 0.02 元 | 0.025 元 |
| **输入（缓存未命中）** | 1.00 元 | 3.00 元 |
| **输出** | 2.00 元 | 6.00 元 |

---

## 3. 思考模式开关与强度控制

### 控制参数对比

| 控制功能 | OpenAI 格式参数 | Anthropic 格式参数 |
| :--- | :--- | :--- |
| **思考模式开关** | `{"thinking": {"type": "enabled/disabled"}}` | `{"thinking": {"type": "enabled/disabled"}}` |
| **思考强度控制** | `{"reasoning_effort": "high/max"}` | `{"output_config": {"effort": "high/max"}}` |

### 补充说明

1. **默认状态**：思考开关默认为 `enabled`（开启状态）。
2. **强度策略**：在思考模式下，普通请求的默认强度（effort）为 `high`；对于复杂的 Agent 类请求（如 Claude Code、OpenCode），系统会自动将 effort 提升至 `max`。
3. **参数兼容**：为了保证兼容性，如果您传入 `low` 或 `medium` 会被自动映射为 `high`；传入 `xhigh` 则会被映射为 `max`。

---

## 4. 相关资源与链接

*   🔗 **官方文档**：[DeepSeek Anthropic API 指南](https://api-docs.deepseek.com/zh-cn/guides/anthropic_api)
*   📊 **用量与充值查询**：[DeepSeek 开放平台使用情况](https://platform.deepseek.com/usage)

> [!WARNING]
> **安全提示**：请妥善保管您的 API Key (`sk-...`)，切勿将其直接硬编码在代码或公开提交到 GitHub 仓库中。建议使用环境变量 `DEEPSEEK_API_KEY_STOCK` 进行配置。