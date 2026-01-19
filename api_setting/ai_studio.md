# 🎨 AI Studio 配置指南

> 本文档介绍如何将 CLIProxyAPI 作为后端，配合 [AI Studio](https://aistudio.google.com/apps/drive) 应用使用。

---

## 📋 配置步骤

### 步骤 1：启动 CLIProxyAPI 服务

确保您的 CLIProxyAPI 实例正在本地或远程运行。

### 步骤 2：访问 AI Studio 应用

1. 在浏览器中登录您的 **Google 账户**
2. 打开以下链接：

👉 [AI Studio 应用](https://aistudio.google.com/apps/drive/1CPW7FpWGsDZzkaYgYOyXQ_6FWgxieLmL)

---

## 🔗 连接配置

默认情况下，AI Studio 应用会尝试连接到本地的 CLIProxyAPI：

```
ws://127.0.0.1:8317
```

### 连接到远程服务

如需连接到远程部署的 CLIProxyAPI，请修改 AI Studio 应用中的 `config.ts` 文件：

```typescript
// 更新 WEBSOCKET_PROXY_URL 的值
WEBSOCKET_PROXY_URL = "ws://yourserverip:8317"
```

### 📊 协议选择

| 场景 | 协议 | 示例 |
|:---|:---|:---|
| 未启用 SSL | `ws://` | `ws://yourserverip:8317` |
| 已启用 SSL | `wss://` | `wss://yourserverip:8317` |

---

## 🔐 认证配置

默认情况下，CLIProxyAPI 的 WebSocket 连接 **不要求认证**。

如需启用认证，请完成以下两步配置：

### 1️⃣ 服务端配置

在 CLIProxyAPI 的 `config.yaml` 文件中启用认证：

```yaml
ws_auth: true
```

### 2️⃣ 客户端配置

在 AI Studio 应用的 `config.ts` 文件中设置认证令牌：

```typescript
JWT_TOKEN = "your-auth-token-here"
```

---

## ✅ 配置完成

完成以上配置后，即可通过 AI Studio 使用 CLIProxyAPI 代理的所有模型！