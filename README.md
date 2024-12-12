# Cline + GPT-4o API + LiteLLM 实现 Cursor 免费平替

## 1. 简介

**Cline**

Cline（原名 Claude Dev）是一款 VSCode 插件形式的编程助手。

它可以智能分析和生成代码、操作文件、处理错误、执行终端命令、辅助网页开发，支持多种模型和 API，能跟踪成本，通过管理上下文、分析代码等技术原理来工作，兼具多种优势。

- 支持项目全流程、模型选择灵活、成本效益高、人机协作良好、具备多模态能力且与 VSCode 深度集成。

- 支持 OpenRouter、Anthropic、OpenAI、Google Gemini 等多种 API 提供商，可配置任何兼容 OpenAI 的 API，也支持通过 Ollama 使用本地模型。

**LiteLLM**

LiteLLM 是 BerriAI 开发的开源 Python 库，能简化大语言模型 API 调用，有统一接口、标准化输入输出、重试回退逻辑、支持预算与速率限制、异步调用、流式传输及日志功能等。

支持超 100 种 LLM 服务，适用于多种自然语言处理相关场景。

**GPT-4o API**

OpenAI 官方 GPT-4o API 是收费的，由 GitHub Models 提供的 GPT-4o API 是免费使用，但有额度限制。
详细情况见 [GitHub Models 速率限制](https://docs.github.com/en/github-models/prototyping-with-ai-models#rate-limits)

简单总结就是：

- 速率限制层高-如：GPT-4o 每天可以使用 50 次 API
- 速率限制层低-如：GPT-4o 每天可以使用 150 次 API

对于个人开发者每天轻中度使用额度足够，俗话说都免费了，还需要啥自行车。

## 2. 获取 GPT-4o API

- 进入 [GitHub Models](https://github.com/marketplace/models/catalog)，选择模型 OpenAI GPT-4o

![](https://raw.githubusercontent.com/HogskinKitty/assets-repository/master/culpro/litellm-1.png)

- 点击右上角 Get API key，然后选择 Get developer key

![](https://raw.githubusercontent.com/HogskinKitty/assets-repository/master/culpro/litellm-2.png)

- 点击 Generate new token，选择 Generate new token(classic)

![](https://raw.githubusercontent.com/HogskinKitty/assets-repository/master/culpro/litellm-3.png)

- 自定义一个名称，点击 Generate **token**

![](https://raw.githubusercontent.com/HogskinKitty/assets-repository/master/culpro/litellm-4.png)

![](https://raw.githubusercontent.com/HogskinKitty/assets-repository/master/culpro/litellm-5.png)

- 然后将生成的 token，也就是后面需要使用的 api_key，请自行保存一份，因为只会显示一次

![](https://raw.githubusercontent.com/HogskinKitty/assets-repository/master/culpro/litellm-6.png)

## 3. 安装 LiteLLM

**api_base：** https://models.inference.ai.azure.com

![](https://raw.githubusercontent.com/HogskinKitty/assets-repository/master/culpro/litellm-7.png)

### **方式一：Docker**

创建 config.yaml 文件，内容如下：

```bash
model_list:
  - model_name: gpt-4o
    litellm_params:
      model: github/gpt-4o
      api_base: https://models.inference.ai.azure.com
      api_key: "your_api_key"
```

> [!WARNING]
>
> 注意：这里的 model 必须是 `github/` 开头，如：github/gpt-4o，以此类推
>
> 其他模型请参考 [LiteLLM 官方文档](https://docs.litellm.ai/docs/providers/)

创建 docker-compose.yml 文件，内容如下：

```yaml
version: '3.8'
services:
  litellm:
    image: ghcr.io/berriai/litellm:main-latest
    container_name: litellm
    restart: always
    ports:
      - "4000:4000"
    volumes:
      - ./config.yaml:/app/config.yaml
    command: [ "--config", "/app/config.yaml" ]
```

使用 Docker compose 启动 LiteLLM

```bash
docker-compose -f docker-compose.yml up -d
```

### **方式二：LiteLLM CLI (pip package)**

```bash
pip3 install 'litellm[proxy]'
```

创建 config.yaml 文件，内容如下：

```bash
model_list:
  - model_name: gpt-4o
    litellm_params:
      model: github/gpt-4o
      api_base: https://models.inference.ai.azure.com
      api_key: "your_api_key"
```

> [!WARNING]
>
> 注意：这里的 model 必须是 `github/ `开头，如：github/gpt-4o，以此类推
>
> 其他模型请参考 [LiteLLM 官方文档](https://docs.litellm.ai/docs/providers/)

启动 LiteLLM

```bash
litellm --config ./config.yaml
```

## 4. 安装 Cline 插件

**VSCode 安装 Cline 插件**

VSCode 插件市场搜索 Cline 安装

**配置大模型 API**

- API Provider：选择 OpenAI Compatible
- Base URL：默认是 localhost:4000 ，如有修改端口，请根据实际情况填写
- API Key：随意，因为在之前的 config.yaml 中已经配置
- Model ID：根据自己选择模型的来填写

![](https://raw.githubusercontent.com/HogskinKitty/assets-repository/master/culpro/litellm-8.png)

至此你应该可以愉快的使用 Cline + GPT-4o 来实现免费版的 Cursor，轻轻松松写代码

**如有问题**

- 请参考官方说明文档：[LittleLLM 文档](https://docs.litellm.ai/docs/)

- [提交 Issue](https://github.com/HogskinKitty/litellm-config/issues)

- WX 联系：HogskinKitty









