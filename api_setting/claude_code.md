# 🤖 Claude Code 配置指南

> 本文档介绍如何将 Claude Code 接入我们配置的代理服务器。

---

## 📋 配置方式概览

| 方式 | 适用场景 | 复杂度 |
|:---|:---|:---:|
| 🖥️ 环境变量 | 终端运行、脚本自动化 | ⭐⭐ |
| 🔧 CC-Switch | 图形化配置、快速切换 | ⭐ |

---

## 🖥️ 方式一：环境变量配置

在终端运行 Claude Code 时，需要设置以下环境变量：

### 基础配置（必填）

```bash
export ANTHROPIC_BASE_URL=http://yourserverip:8317
export ANTHROPIC_API_KEY=yourapikey
```

### 模型配置

根据 Claude Code 版本选择对应的配置方式：

#### 📦 2.x.x 版本

```bash
export ANTHROPIC_DEFAULT_OPUS_MODEL=model1
export ANTHROPIC_DEFAULT_SONNET_MODEL=model2
export ANTHROPIC_DEFAULT_HAIKU_MODEL=model3
```

#### 📦 1.x.x 版本

```bash
export ANTHROPIC_MODEL=model4
export ANTHROPIC_SMALL_FAST_MODEL=model5
```

### 💡 配置示例

`model` 可以是配置文件中任意可用的模型名称。以下是一个灵活配置示例：

```bash
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-5-thinking
export ANTHROPIC_DEFAULT_SONNET_MODEL=gemini-3-pro-high
export ANTHROPIC_DEFAULT_HAIKU_MODEL=MiniMax-M2.1
```

> ✨ **优势**：这样配置后，可以在不同模型间自由切换，实现多模型混用。

### ✅ 配置效果

配置完成后，Claude Code 的模型选择菜单将显示我们配置的模型：

```
/model to try Opus 4.5

────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 Select model
 Switch between Claude models. Applies to this session and future Claude Code sessions. For other/previous model names,
  specify with --model.

   1. Default (recommended)  Use the default model (currently MiniMax-M2.1) · $3/$15 per Mtok
   2. Opus                   Opus 4.5 · Most capable for complex work · $5/$25 per Mtok
   3. Haiku                  Haiku 4.5 · Fastest for quick answers · $1/$5 per Mtok
 > 4. MiniMax-M2.1 √         Custom model

 Enter to confirm · escape to exit
```

### 🎯 自定义模型调用

除了使用预设模型，还可以临时切换到任意模型：

```bash
/model [模型名称]
```

**示例：**

```
# 输入
/model gpt-5

# 输出
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
> Try "refactor <filepath>"
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 ? for shortcuts                                                                                   Set model to gpt-5
```

---

## 🔧 方式二：CC-Switch 配置

如果觉得环境变量配置繁琐，可以使用 [CC-Switch](https://github.com/farion1231/cc-switch.git) 图形化工具快速切换 Claude Code 配置。

### 安装与使用

1. 安装 CC-Switch
2. 打开应用，选择 **添加 Claude API**
3. 填写以下配置项：

### 📊 配置项对应关系

| CC-Switch 配置项 | 对应环境变量 |
|:---|:---|
| API Key | `ANTHROPIC_API_KEY` |
| 供应商地址 | `ANTHROPIC_BASE_URL` |
| 主模型 | `ANTHROPIC_MODEL` |
| 推理模型 (Thinking) | `ANTHROPIC_SMALL_FAST_MODEL` |
| Haiku 默认模型 | `ANTHROPIC_DEFAULT_HAIKU_MODEL` |
| Sonnet 默认模型 | `ANTHROPIC_DEFAULT_SONNET_MODEL` |
| Opus 默认模型 | `ANTHROPIC_DEFAULT_OPUS_MODEL` |

> 💡 **提示**：CC-Switch 的优势在于可以保存多套配置，一键切换不同的 API 提供商和模型组合。

---

## 💻 方式三：VS Code 插件配置

本节介绍如何在 VS Code 中配置 **Claude Code for VS Code** 插件。

### 配置步骤

1. 在 VS Code 中安装 **Claude Code** 插件
2. 打开设置（`Ctrl + ,`）
3. 点击右上角的 **在 settings.json 中编辑** 图标
4. 添加以下配置：

### 📝 settings.json 配置

```json
"claudeCode.environmentVariables": [
    {
        "name": "ANTHROPIC_BASE_URL",
        "value": "http://yourserverip:8317"
    },
    {
        "name": "ANTHROPIC_AUTH_TOKEN",
        "value": "yourapikey"
    },
    {
        "name": "API_TIMEOUT_MS",
        "value": "3000000"
    },
    {
        "name": "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC",
        "value": "1"
    },
    {
        "name": "ANTHROPIC_MODEL",
        "value": "model1"
    },
    {
        "name": "ANTHROPIC_SMALL_FAST_MODEL",
        "value": "model2"
    },
    {
        "name": "ANTHROPIC_DEFAULT_SONNET_MODEL",
        "value": "model3"
    },
    {
        "name": "ANTHROPIC_DEFAULT_OPUS_MODEL",
        "value": "model4"
    },
    {
        "name": "ANTHROPIC_DEFAULT_HAIKU_MODEL",
        "value": "model5"
    }
]
```

### 📊 配置项说明

| 配置项 | 说明 | 必填 |
|:---|:---|:---:|
| `ANTHROPIC_BASE_URL` | 代理服务器地址 | ✅ |
| `ANTHROPIC_AUTH_TOKEN` | API 密钥 | ✅ |
| `API_TIMEOUT_MS` | API 超时时间（毫秒） | ⬜ |
| `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` | 禁用非必要流量（建议设为 `1`） | ⬜ |
| `ANTHROPIC_MODEL` | 主模型 | ⬜ |
| `ANTHROPIC_SMALL_FAST_MODEL` | 快速推理模型 | ⬜ |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | Sonnet 默认模型 | ⬜ |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | Opus 默认模型 | ⬜ |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | Haiku 默认模型 | ⬜ |

### 🎯 使用自定义模型

如需使用配置文件中的其他模型，可以通过插件设置指定：

1. 打开 VS Code 设置
2. 搜索 `Claude Code: Selected Model`
3. 输入需要使用的模型名称（如 `gpt-5`）

> 💡 **提示**：配置完成后，重启 VS Code 使配置生效。