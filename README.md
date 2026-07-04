# AI Bridge 统一集成配置中心

本项目提供了一套完整的 AI 助手桥接与多模型代理网关配置方案，实现了通过 **飞书 (Feishu/Lark)** 接入 **Claude Code / Coding Agent**，并利用 **LiteLLM** 统一调度管理 **DeepSeek** 和 **智谱 AI (BigModel)** 大模型。

## 1. 架构流图 (Architecture Flowchart)

下面是该集成系统的核心请求与控制数据流图：

```mermaid
graph TD
    %% Define styles
    classDef client fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef bridge fill:#e8f5e9,stroke:#388e3c,stroke-width:2px;
    classDef agent fill:#fff3e0,stroke:#f57c00,stroke-width:2px;
    classDef gateway fill:#ede7f6,stroke:#5e35b1,stroke-width:2px;
    classDef provider fill:#ffebee,stroke:#c62828,stroke-width:2px;

    User["飞书用户 / Feishu User"]:::client
    FeishuBridge["飞书桥接器<br/>(lark-channel-bridge)"]:::bridge
    CodingAgent["开发 Agent / Claude Code"]:::agent
    LiteLLM["LiteLLM 统一网关<br/>(Port: 4000)"]:::gateway
    
    subgraph ModelProviders [大模型提供商 / LLM Providers]
        DeepSeek["DeepSeek API<br/>(v4-pro / v4-flash)"]:::provider
        ZhipuAI["智谱 AI API<br/>(GLM-5 / GLM-5.2 / GLM-4)"]:::provider
    end

    %% Flow connections
    User -->|1. 发送消息/对话| FeishuBridge
    FeishuBridge -->|2. 转发请求至本地 Agent 接口| CodingAgent
    CodingAgent -->|3. 发起 Chat 补全请求 (Anthropic 格式)| LiteLLM
    
    %% Routing logic
    LiteLLM -->|4a. 路由并格式化| DeepSeek
    LiteLLM -->|4b. 路由并格式化| ZhipuAI

    %% Return flows
    DeepSeek -->|5a. 响应| LiteLLM
    ZhipuAI -->|5b. 响应| LiteLLM
    LiteLLM -->|6. 统一返回结构| CodingAgent
    CodingAgent -->|7. 输出开发解答/代码修改| FeishuBridge
    FeishuBridge -->|8. 投递结果消息| User
```

---

## 2. 模块目录

本项目包含以下核心模块的配置及使用指南：

1.  **[接入飞书](接入飞书.md)**：将本地 Agent 对接至飞书渠道，实现随时随地对话。
2.  **[配置 LiteLLM](配置litellm.md)**：配置本地多模型代理网关，将各厂商模型映射为统一的兼容 API。
3.  **[DeepSeek API 参考文档](deepseek.md)**：DeepSeek 模型参数、计费和思考模式等高级控制说明。
4.  **[智谱 AI (BigModel) 开发指南](chatglm.md)**：智谱大模型资费说明、SDK 调用示例和财务配置。

---

## 3. 快速启动链路

要使整个桥接网络工作，请按以下顺序依次启动：

1.  **设置环境变量**：
    在运行 LiteLLM 之前配置各厂商 API 密钥：
    ```bash
    # Windows PowerShell
    $env:DEEPSEEK_API_KEY_STOCK="your-deepseek-key"
    $env:ZHIPUAI_API_KEY_STOCK="your-zhipu-key"
    ```
2.  **启动 LiteLLM 代理**：
    ```bash
    litellm --config C:\ai-bridge\litellm_config.yaml --port 4000
    ```
3.  **启动飞书桥接服务**：
    ```bash
    lark-channel-bridge run --skip-check-lark-cli
    ```
