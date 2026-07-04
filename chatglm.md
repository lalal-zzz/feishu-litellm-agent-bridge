# 智谱 AI 开放平台 (BigModel) 参考指南

本指南整理了智谱 AI 开放平台的开发配置、SDK 安装、基本 API 调用示例以及资费与账户管理方式。

## 1. 快速开始 (Quick Start)

*   **官方文档**：[智谱 AI 开放文档 - 快速开始](https://docs.bigmodel.cn/cn/guide/start/quick-start)

### 基础接入步骤

1.  **注册与登录**：访问 [智谱 AI 开放平台官网](https://open.bigmodel.cn/) 注册开发者账号。
2.  **获取 API Key**：登录后进入控制台，在“API Key 管理”页面创建并复制您的 API Key。
3.  **环境准备**：支持通过标准 HTTP 请求方式调用，或直接使用官方提供的 Python/Node.js SDK。

### SDK 安装命令

```bash
# 安装 Python SDK
pip install zhipuai

# 安装 Node.js SDK
npm install zhipuai
```

### Python 调用示例 (GLM-4)

```python
from zhipuai import ZhipuAI

# 初始化客户端，建议从环境变量或安全存储中读取 API Key
client = ZhipuAI(api_key="您的_API_KEY") 

response = client.chat.completions.create(
    model="glm-4",  # 填写需要调用的模型名称
    messages=[
        {"role": "user", "content": "你好，请介绍一下你自己。"}
    ],
)

print(response.choices[0].message.content)
```

---

## 2. 计费中心与价格矩阵 (Pricing)

*   **官方价格详情**：[智谱 AI 资费说明](https://open.bigmodel.cn/pricing)

智谱 AI 采用按量计费模式，基本计费单位为 **Token**（通常 1 个汉字或词约占 1~2 个 Token）。图像及多模态大模型可能按张数或输入分辨率综合计费。

### 主流模型标准资费概览

| 模型系列 | 模型标识 (model) | 输入价格 (每百万 Tokens) | 输出价格 (每百万 Tokens) | 特点与适用场景 |
| :--- | :--- | :--- | :--- | :--- |
| **GLM-4-Plus** | `glm-4-plus` | 参见官网最新价 | 参见官网最新价 | 旗舰高智能模型，适合复杂推理、长文本及多任务处理 |
| **GLM-4-Air** | `glm-4-air` | 参见官网最新价 | 参见官网最新价 | 高性价比模型，速度与效果平衡，适合大规模并发场景 |
| **GLM-4-Flash** | `glm-4-flash` | 参见官网最新价 | 参见官网最新价 | 极速响应，通常提供免费额度或极低成本，适合高频低延迟任务 |
| **CogView-3** | `cogview-3` | 按张计费 (根据分辨率) | - | 文本生成图像多模态大模型 |

> [!NOTE]
> 智谱 AI 时常会推出针对新注册用户的免费赠送 Token 活动，具体免费额度及最新价格请以官网实时更新为准。

---

## 3. 财务中心与账户管理

*   **管理入口**：[智谱 AI 财务中心概览](https://bigmodel.cn/finance-center/finance/overview)

在财务中心中，您可以进行账户资金与消耗管理：

*   **财务概览 (Overview)**：实时查看当前账户的 **可用余额**、**赠送额度** 及其 **有效期**。
*   **充值中心 (Recharge)**：支持在线扫码支付（支付宝、微信支付等方式）或企业对公转账。
*   **账单与用量明细**：提供按日、按月、按模型维度的详细扣费账单下载，方便企业进行财务对账。
*   **发票管理 (Invoice)**：支持在线申请开具增值税普通发票或专用发票（电子/纸质发票）。

> [!WARNING]
> **安全提示**：请妥善保管您的 API Key，切勿将其直接硬编码在代码中提交。建议使用环境变量 `ZHIPUAI_API_KEY_STOCK` 进行配置。