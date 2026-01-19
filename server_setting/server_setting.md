# ⚙️ 服务器配置

> 本文档记录了使用 [CLIProxyAPI 官方文档](https://help.router-for.me/cn/) 部署代理服务器的完整过程，包含踩坑经验与解决方案。

---

## 🖥️ 服务器选择

CLIProxyAPI 对服务器配置要求较低，可根据使用场景选择部署方式：

| 场景 | 推荐方案 | 备注 |
|:---|:---|:---|
| 个人使用 | 本地运行 | 最简单，无需额外配置 |
| 多人共享 | 本地 + 内网穿透 | 需配置内网穿透服务（本文不展开） |
| 生产部署 | 云服务器 | 推荐阿里云，性价比高 |

> 📝 本文档同时涵盖 **本地运行** 和 **服务器部署** 两种方案。

---

## 📦 程序部署

关于 CLIProxyAPI 的安装与运行，[官方文档](https://help.router-for.me/cn/) 已有详尽说明，此处不再赘述。

> ✅ **前提假设**：你已成功部署 CLIProxyAPI，并熟悉其 WebGUI 界面。

本文重点讲解：
- 🔐 **OAuth 登录配置**
- 🔗 **第三方 API 接入**
- 📄 **配置文件修改与模型适配**

---

## 🔐 OAuth 登录与 API 接入

| 配置类型 | 示例 | 说明 |
|:---|:---|:---|
| OAuth 登录 | Antigravity / Gemini CLI | 以 Gemini Pro 账号为例 |
| API 接入 | MiniMax | 适用于其他第三方 API 提供商 |

---

### 🔑 OAuth 登录

#### Antigravity OAuth

**操作步骤：**

1. 准备一个 **Gemini Pro 账号**
2. 进入 WebGUI → 点击 **OAuth 登录** → 选择 **Antigravity OAuth**
3. 使用 Gemini Pro 账号完成登录授权
4. 获取 OAuth 凭证

**不同运行环境的处理方式：**

| 环境 | 操作方式 |
|:---|:---|
| 🖥️ 服务器运行 | 授权后会返回 `http://localhost:...` 地址，将该地址填入 Antigravity 设置中完成登录 |
| 💻 本地运行 | 直接执行命令 `./cli-proxy-api --antigravity-login` |

![Antigravity OAuth 配置示意](image/fig1.png)

#### Gemini CLI OAuth

**操作步骤：**

1. 访问 [Google Cloud Console](https://cloud.google.com/cloud-console) 并登录 Google 账号
2. 创建一个新项目
3. 返回 WebGUI → 点击 **OAuth 登录**，按提示完成授权
4. 或使用命令行：`./cli-proxy-api --login`

> 💡 **提示**：其他提供商的 OAuth 登录方式请参阅 [官方文档](https://help.router-for.me/cn/)。

---

### 🔗 API 接入

API 接入主要支持三种风格：

| API 风格 | 适用模型 |
|:---|:---|
| Claude API | Anthropic 系列、MiniMax 等 |
| OpenAI API | GPT 系列、兼容模型 |
| Gemini API | Google Gemini 系列 |

#### 以 MiniMax 为例

由于官方文档未涵盖 MiniMax 接入，但 MiniMax 提供 Claude API 兼容接口，可直接配置：

**WebGUI 配置步骤：**

1. 进入 WebGUI → 点击 **API 提供商**
2. 选择 **添加 Claude API 配置**
3. 填写以下信息：

| 配置项 | 值 | 必填 |
|:---|:---|:---:|
| API 密钥 | 你的 API 密钥 | ✅ |
| Base URL | `https://api.minimax.com/anthropic` | ✅ |
| 自定义模型 | `MiniMax-M2.1` | ⬜ |

![API 配置示意](image/fig2.png)

> ⚠️ **重要**：自定义模型名称在后续的 **模型调用** 部分会用到，请务必正确填写。

#### config.yaml 配置示例

```yaml
claude-api-key:
  - api-key: "sk-cp-xxxxx-your-api-key-here"
    base-url: "https://api.minimaxi.com/anthropic"
    models:
      - name: "MiniMax-M2.1"
```

> 💡 **提示**：其他第三方 API 提供商的接入方式类似，只需替换相应的 Base URL 和模型名称即可。

---

## 🎯 模型调用

本章节讲解如何配置模型的 **名称** 与 **别名**，以及 AI 应用调用模型的逻辑。

### 📋 查看可用模型

完成 OAuth 登录配置后，可在 WebGUI 中查看当前可用的模型列表：

![可用模型列表](image/fig3.png)

### 📝 配置模型名称与别名

接下来需要在配置文件中确认模型名称配置正确。以下是完整的配置示例：

```yaml
oauth-model-alias:
  antigravity:
    - name: "rev19-uic3-1p"
      alias: "gemini-2.5-computer-use-preview-10-2025"
      fork: true
    - name: "gemini-3-pro-image"
      alias: "gemini-3-pro-image"
      fork: true
    - name: "gemini-3-pro-high"
      alias: "gemini-3-pro-high"
      fork: true
    - name: "gemini-3-flash"
      alias: "gemini-3-flash-preview"
      fork: true
    - name: "claude-sonnet-4-5"
      alias: "claude-sonnet-4-5"
      fork: true
    - name: "claude-sonnet-4-5-thinking"
      alias: "claude-sonnet-4-5-20250514"
      fork: true
    - name: "claude-opus-4-5-thinking"
      alias: "claude-opus-4-5-20251101"
      fork: true
    - name: "gemini-2.5-flash"
    - name: "gemini-2.5-flash-lite"
    - name: "gpt-oss-120b-medium"
  codex:
    - name: "gpt-5"
    - name: "gpt-5-codex"
    - name: "gpt-5-codex-mini"
    - name: "gpt-5.1"
    - name: "gpt-5.1-codex"
    - name: "gpt-5.1-codex-max"
    - name: "gpt-5.1-codex-mini"
    - name: "gpt-5.2"
    - name: "gpt-5.2-codex"
  gemini-cli:
    - name: "gemini-2.5-pro"
    - name: "gemini-3-pro-preview"
    - name: "gemini-2.5-flash"

claude-api-key:
  - api-key: "your-api-key-here"
    base-url: "https://api.minimaxi.com/anthropic"
    models:
      - name: "MiniMax-M2.1"
```

### 🔤 配置字段说明

| 字段 | 含义 | 必填 |
|:---|:---|:---:|
| `name` | 提供商支持的原始模型名称，也是 AI 程序默认调用的模型名称 | ✅ |
| `alias` | 模型别名，当 AI 程序不支持原始名称时使用 | ⬜ |
| `fork` | 是否启用 fork 模式（用于并行调用优化） | ⬜ |

### 💡 别名使用场景

> **什么时候需要用 `alias`？**
>
> 当 AI 应用的模型列表中 **没有对应的原始模型名称**（且无法自定义名称）时，需要通过 `alias` 为模型取一个程序能识别的别名。

**示例说明：**

以 **Kilo Code** 为例：

| 问题 | Kilo Code 的 Anthropic 模型列表中没有 `claude-opus-4-5-thinking` |
|:---|:---|
| 解决方案 | 配置 `alias: "claude-opus-4-5-20251101"` 作为别名 |
| 调用效果 | 程序调用 `claude-opus-4-5-20251101`，实际使用的是 `claude-opus-4-5-thinking` 模型 |

![模型别名配置示意](image/fig4.png)

---

> 📖 **下一步**：在 `api_setting/` 部分，我们将讲解如何让 AI 程序调用我们配置好的 API 和模型。