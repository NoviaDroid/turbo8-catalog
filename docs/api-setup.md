# Connect an API provider to Turbo8

[English](#english) · [中文](#中文) · [Codex / ChatGPT sign-in guide](codex-setup.md)

## English

Use an API provider when you want Turbo8 to send requests to an API endpoint
using your provider's credentials. This mode does **not** need Codex CLI or
Node.js. If you want ChatGPT sign-in instead, choose **Codex** and follow the
separate guide linked above.

### Recommended: OpenAI GPT-6 / GPT-6.1

We recommend the **OpenAI GPT-6 family** for AI-assisted game creation in Turbo8.
Our quality-first choice is **GPT-6 Astra** (`gpt-6-astra`); **GPT-6.1 Sol** is
also a good option. The official API example below uses GPT-6 Astra. Confirm model
access and pricing in your own OpenAI account before sending requests.

### Example 1: OpenAI official API

Create a key in the [OpenAI API dashboard](https://platform.openai.com/api-keys),
confirm API billing, then open **Settings → AI settings** in Turbo8.
Fill in these fields in order: provider, endpoint, key, model, protocol.

| Setting | GPT-6 Astra example |
| --- | --- |
| Provider | **OpenAI** |
| ENDPOINT | `https://api.openai.com/v1` |
| API KEY | Your own OpenAI API key |
| MODEL | `gpt-6-astra` |
| API PROTOCOL | **Responses** (or **Auto (recommended)** at this official URL) |
| REASONING EFFORT | **Medium** |

Use the exact model ID `gpt-6-astra`, not the shorthand `astra`. This model needs
Responses for tool calling, which Turbo8 uses to edit games. Medium is a supported
starting point; do not select None or Minimal for this model. See the
[official GPT-6 Astra documentation](https://developers.openai.com/api/docs/models/gpt-6-astra).
If your account cannot access it, choose another documented model you can access
and check its tool/protocol support. This example does not change your saved model.

Click **SAVE SETTINGS**, then **TEST CONNECTION / PROTOCOL**. Once that succeeds,
try a small game-editing request to check tool calling as well.

### Example 2: APIKEY.FAN (third-party gateway)

Sign in at [APIKEY.FAN](https://apikey.fan/) and create a key in its own dashboard.
Check your key's group, model access and balance before use. Use the gateway key,
not your OpenAI key or ChatGPT login.

| Setting | APIKEY.FAN example |
| --- | --- |
| Provider | **Custom** |
| ENDPOINT | `https://api.apikey.fan/v1` |
| API KEY | Your APIKEY.FAN-issued key |
| MODEL | `gpt-6.1-sol`, only if enabled for your key/group |
| API PROTOCOL | **Responses** — select explicitly, not Auto |
| REASONING EFFORT | **Medium**; use Provider default if the gateway rejects this parameter |
| PROVIDER WEB SEARCH | Disabled initially; enable only after confirming support |

The [provider's public quick-start example](https://apikey.fan/) lists this base
URL, model and Responses protocol; see also its [integration docs](https://apikey.fan/docs).
In Turbo8, set the endpoint **before** the key, use **FETCH MODELS** or **MANUAL
INPUT**, then save and test. Do not enter the website URL `https://apikey.fan/`
or append `/responses`. Custom + Auto normally selects Chat Completions, so this
example requires choosing **Responses** yourself.

This is a configuration example, not a verified end-to-end compatibility test or
an endorsement. APIKEY.FAN is not OpenAI's official API: the gateway receives your
requests and relevant editor context, and controls its own billing and routing.
Check its terms/privacy policy before use. A successful connection test does not
prove tool calling, web search or every advertised model works in Turbo8.

### Quick setup

1. Obtain an API key from the provider's own dashboard. Confirm model access and
   API billing/credit before use; a ChatGPT subscription is not an API balance.
   [Subscription and API usage are separate](https://learn.chatgpt.com/docs/pricing).
   For OpenAI, start with the [official API quickstart](https://developers.openai.com/api/docs/quickstart)
   and [API key dashboard](https://platform.openai.com/api-keys).
2. In Turbo8, open **Settings → AI settings** and choose **OpenAI** or **Custom**.
   Additional provider presets are experimental and require Developer Mode in
   Turbo8. You do not need to enable it for OpenAI or Custom.
3. Set **ENDPOINT first**, then **API KEY**. Editing the endpoint clears the key
   field deliberately, to reduce accidental credential reuse on another host.
4. Click **FETCH MODELS**, then choose a model available to your account. If model
   listing is unsupported, use **MANUAL INPUT** and enter the exact provider model
   ID. Listing a model does not prove that your account can run it.
5. Use the protocol in the relevant example above; otherwise start with
   **Auto (recommended)** under **API PROTOCOL**. Choose **Provider
   default** for reasoning effort if the model does not support reasoning controls.
6. Click **SAVE SETTINGS**, then **TEST CONNECTION / PROTOCOL**. The test sends a
   small real request and may incur charges. Review its response and protocol.
   A successful text test is not a full tool-calling/game-creation compatibility test.

### Base URLs and protocols

These are Turbo8's built-in preset values, not a promise that every model or
third-party gateway supports every feature. Confirm the provider's current
documentation and your account permissions.

| Turbo8 provider | Preset ENDPOINT | Auto selection |
| --- | --- | --- |
| OpenAI | `https://api.openai.com/v1` | Responses at this official URL |
| Custom | Your provider's documented base URL | Chat Completions, except recognized official URLs |
| Anthropic (experimental) | `https://api.anthropic.com/v1` | Anthropic Messages |
| DeepSeek (experimental) | `https://api.deepseek.com/v1` | Chat Completions |
| OpenRouter (experimental) | `https://openrouter.ai/api/v1` | Chat Completions |
| Ollama (experimental) | `http://localhost:11434/v1` | Chat Completions |

Supply a **base URL**, not the final request route. Turbo8 appends `/responses`,
`/chat/completions` or the Anthropic messages route. For example, do not enter
`https://api.openai.com/v1/responses` in ENDPOINT. Remote endpoints must use HTTPS;
plain HTTP is accepted only for loopback hosts. `localhost` means the device
running Turbo8, not another computer on the network.

Auto makes a deterministic protocol choice; it does not probe multiple protocols
or silently retry against another one. For a compatible gateway that supports
Responses, select **Responses** explicitly. Use **Anthropic Messages** only for
an endpoint implementing that protocol.

### Search, local models and costs

- **PROVIDER WEB SEARCH** is optional. Turbo8's Auto enables it only for the
  official OpenAI Responses endpoint. Other endpoints need compatible hosted
  `web_search` support and explicit configuration; search can add charges.
  **TEST WEB SEARCH** also makes a real request. Disable it if unsupported.
- A local server may not require a key, but it must already be running and expose
  the compatible API. Model installation and server configuration happen outside
  Turbo8. Local models still need adequate tool-calling capability for editing.
- Long conversations and repeated tool rounds can consume more quota. Check the
  provider's usage dashboard and budgets; Turbo8's connection test does not
  certify spending limits. Review [OpenAI API pricing](https://developers.openai.com/api/docs/pricing)
  or your chosen provider's pricing before use.

### Troubleshooting

| Symptom | Check |
| --- | --- |
| 401 / 403 | Correct provider key, project permissions and account/model access |
| 404 | Base URL, accidental duplicate route, exact model ID and selected protocol |
| 429 | Rate limit or exhausted credit/quota; inspect the provider message |
| Model list unavailable | Enter a documented model ID manually; do not guess an ID |
| Unsupported reasoning/search parameter | Use Provider default reasoning or disable search |
| Model mismatch | Turbo8 rejected a served model different from the requested one; check the gateway without silently substituting models |
| Timeout / TLS / cannot connect | Network, DNS, proxy, certificate and provider status; do not disable certificate verification |

Only send keys to a provider/endpoint you trust. The selected endpoint receives
your prompts and relevant editor/tool context. API keys belong in the settings
field, never in cartridge scripts, public GitHub issues or screenshots. Do not
share settings files containing credentials. If a key leaks, revoke it in the
provider dashboard. See [OpenAI API authentication and key safety](https://developers.openai.com/api/reference/overview).

## 中文

API 模式直接连接你选择的服务商，不需要安装 Codex CLI 或 Node.js。若要使用 ChatGPT
账号登录，请切到 **Codex**，参阅[另一份指引](codex-setup.md)。ChatGPT 订阅不是 API
余额；API 权限、计费和用量请在对应服务商的后台确认。

### 推荐使用 OpenAI GPT-6 / GPT-6.1

推荐使用 **OpenAI GPT-6 系列**进行 Turbo8 AI 游戏创作。追求极致效果首选
**GPT-6 Astra**（`gpt-6-astra`），**GPT-6.1 Sol** 也是可选方案。
以下官方 API 案例直接以 GPT-6 Astra 为例；调用前请在自己的 OpenAI 账号中确认模型权限和费用。

### 案例一：OpenAI 官方 API

先在 [OpenAI API 后台](https://platform.openai.com/api-keys)创建密钥并确认 API 计费，
然后打开 Turbo8 的 **Settings → AI settings**，按服务商、地址、密钥、模型、协议
的顺序填写：

| 设置项 | GPT-6 Astra 示例 |
| --- | --- |
| Provider | **OpenAI** |
| ENDPOINT | `https://api.openai.com/v1` |
| API KEY | 你自己的 OpenAI API Key |
| MODEL | `gpt-6-astra` |
| API PROTOCOL | **Responses**（这个官方地址也可以选 **Auto (recommended)**） |
| REASONING EFFORT | **Medium** |

模型名要填写完整的 `gpt-6-astra`，不要只写 `astra`。Turbo8 编辑游戏需要工具调用，
此模型应使用 Responses 协议；推理强度可先选 Medium，不要选 None 或 Minimal。
详见 [GPT-6 Astra 官方文档](https://developers.openai.com/api/docs/models/gpt-6-astra)。
如果账号暂时没有权限，请自行选择有权限的模型并确认其工具调用和协议支持；
本示例不会替换你已保存的模型设置。

点 **SAVE SETTINGS**，再点 **TEST CONNECTION / PROTOCOL**；连接成功后，
再尝试一个小型游戏修改请求，检查工具调用是否正常。

### 案例二：APIKEY.FAN 第三方中转

在 [APIKEY.FAN](https://apikey.fan/) 登录并在其后台创建密钥，确认密钥对应的分组、
模型权限和余额。这里使用该平台发放的 Key，不是 OpenAI Key，也不是 ChatGPT 登录。

| 设置项 | APIKEY.FAN 示例 |
| --- | --- |
| Provider | **Custom** |
| ENDPOINT | `https://api.apikey.fan/v1` |
| API KEY | APIKEY.FAN 后台创建的密钥 |
| MODEL | `gpt-6.1-sol`，前提是你的密钥／分组有权限 |
| API PROTOCOL | 手动选 **Responses**，不要保留 Auto |
| REASONING EFFORT | **Medium**；若网关拒绝该参数，改为 Provider default |
| PROVIDER WEB SEARCH | 先关闭，确认支持后再开启 |

以上地址、模型与 Responses 协议来自[服务商首页的公开接入示例](https://apikey.fan/)，
也可查看其[接入文档](https://apikey.fan/docs)。在 Turbo8 中**先填地址，再填密钥**，
用 **FETCH MODELS** 获取模型或用 **MANUAL INPUT** 手动填写，再保存并测试。
不要把网站首页 `https://apikey.fan/` 当成 API 地址，也不要在地址后附加 `/responses`。
Custom 的 Auto 通常会选择 Chat Completions，因此本案例必须手动选择 **Responses**。

这只是配置案例，不代表已完成实际调用兼容性验证，也不是对服务商的背书。
APIKEY.FAN 不是 OpenAI 官方 API；中转服务会接收请求和相关编辑器上下文，
计费与路由由它管理，使用前请阅读其条款和隐私政策。连接成功也不代表工具调用、
联网搜索和所有宣传模型都已在 Turbo8 中验证可用。

### 配置顺序

1. 在服务商自己的后台创建 API Key，并确认可用模型、余额或计费方式。
2. 打开 **Settings → AI settings**，选择 **OpenAI** 或 **Custom**。其他实验性预设
   需要 Turbo8 的 Developer Mode；使用 OpenAI / Custom 不需要开启它。
3. **先填 ENDPOINT，再填 API KEY**。修改地址会清空密钥输入框，避免误发给另一个主机。
4. 点 **FETCH MODELS** 获取模型；不支持列模型的服务可用 **MANUAL INPUT** 填写准确
   模型 ID。列出来不等于账号一定有调用权限。
5. 上面两套案例按各自的协议填写；其他配置可先选 **Auto (recommended)**。
   官方 OpenAI 地址走 Responses，Anthropic
   走 Messages，普通兼容服务默认走 Chat Completions。Auto 不会轮流试接口。
6. 保存后点 **TEST CONNECTION / PROTOCOL**。这是真实的小请求，可能消耗额度；
   文本测试成功不代表所有工具调用和游戏编辑功能都兼容。

Endpoint 填服务商的**基础地址**，不要再附加 `/responses` 或 `/chat/completions`。
上方表格列出了 Turbo8 内置默认值。远程地址必须使用 HTTPS；`localhost` 指运行
Turbo8 的本机，在手机上填写它并不会连接电脑上的模型服务。

不支持推理参数时，把 **REASONING EFFORT** 改为 **Provider default**。不支持联网
搜索时关闭 **PROVIDER WEB SEARCH**；搜索测试也会真实调用服务并可能收费。

401/403 优先查密钥和权限；404 查基础地址、模型名和协议；429 查额度及限流；
网络或证书报错不要靠关闭安全校验解决。若返回模型与请求不同，应检查网关路由，
不要在不知情的情况下换用另一模型。

密钥只填进可信服务商对应的设置，不要放进游戏脚本、聊天截图或 GitHub。请求内容
和相关编辑器上下文会发送给你配置的 Endpoint；泄露密钥后应立即在服务商后台撤销。

官方参考：[OpenAI 入门](https://developers.openai.com/api/docs/quickstart)、
[密钥后台](https://platform.openai.com/api-keys)、
[API 定价](https://developers.openai.com/api/docs/pricing)。
