# 配置 LiteLLM 统一网关

LiteLLM 用于为所有不同的模型（如 DeepSeek、智谱 GLM 等）提供一个与 OpenAI/Anthropic 格式兼容的统一接口，以便客户端（如 Claude Code）统一调用。

## 1. 安装 LiteLLM

通过 pip 安装支持代理模式的 LiteLLM：

```bash
pip install litellm[proxy]
```

## 2. 配置文件设置 (Windows 示例)

在本地创建配置目录并生成 `litellm_config.yaml` 配置文件：

### 创建目录与配置文件

在 PowerShell 中执行以下命令：

```powershell
mkdir C:\ai-bridge
cd C:\ai-bridge
New-Item litellm_config.yaml
```

### 编辑配置文件内容

将以下模型列表配置写入 `C:\ai-bridge\litellm_config.yaml`。该配置集成了 DeepSeek 和智谱 AI (BigModel) 的模型，并将其映射为兼容 Anthropic API 格式的终端。

```yaml
model_list:
  # 1. deepseek-v4flash
  - model_name: claude-deepseek-v4flash
    litellm_params:
      model: anthropic/deepseek-v4-flash
      api_base: "https://api.deepseek.com/anthropic"
      api_key: "os.environ/DEEPSEEK_API_KEY_STOCK"

  # 2. deepseek-v4pro
  - model_name: claude-deepseek-v4pro
    litellm_params:
      model: anthropic/deepseek-v4-pro
      api_base: "https://api.deepseek.com/anthropic"
      api_key: "os.environ/DEEPSEEK_API_KEY_STOCK"

  # 3. chatglm-5.2
  - model_name: claude-chatglm-5.2
    litellm_params:
      model: anthropic/glm-5.2
      api_base: "https://open.bigmodel.cn/api/anthropic"
      api_key: "os.environ/ZHIPUAI_API_KEY_STOCK"

  # 4. chatglm-5
  - model_name: claude-chatglm-5
    litellm_params:
      model: anthropic/glm-5
      api_base: "https://open.bigmodel.cn/api/anthropic"
      api_key: "os.environ/ZHIPUAI_API_KEY_STOCK"
```

## 3. 启动 LiteLLM 代理服务

使用刚才创建的配置文件在本地 `4000` 端口启动服务：

```bash
litellm --config C:\ai-bridge\litellm_config.yaml --port 4000
```

## 4. Claude Code 接入配置

编辑 Claude Code 配置文件 `~/.claude/settings.json`，使其通过本地 LiteLLM 统一网关转发请求：

```json
{
  "model": "claude-deepseek-v4pro",
  "availableModels": [
    "claude-deepseek-v4flash",
    "claude-deepseek-v4pro",
    "claude-chatglm-5.2",
    "claude-chatglm-5"
  ],
  "env": {
    "ANTHROPIC_API_KEY": "sk-my-secret-gateway-key",
    "ANTHROPIC_BASE_URL": "http://127.0.0.1:4000",
    "ANTHROPIC_MODEL": "claude-deepseek-v4pro",
    "CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY": "1"
  },
  "theme": "dark"
}
```