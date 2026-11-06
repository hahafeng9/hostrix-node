# hostrix-node

hostrix.website 免费节点：xray（VLESS/VMess/Trojan + WS，单端口分流）+ 订阅服务 + 哪吒探针，三合一。

## 部署（hostrix.website 面板）

1. 新建服务器，语言选 Python，starter 选 Deploy from GitHub，连这个仓库（main 分支）。
2. Startup 环境变量：
   - `PORT`：平台注入（公开 HTTP 端口），不用自己填
   - `PUBLIC_URL`：公网域名（建服后在面板看，填好重启应用）
   - `NODE_UUID`、`NODE_TROJAN_PW`、`NODE_S1`、`NODE_S2`、`NODE_S3`：节点凭据（固定值）
   - `TG_BOT_TOKEN`、`TG_CHAT_ID`：TG 通知
   - `NZ_CLIENT_SECRET`：哪吒密钥
3. 公网地址见面板服务器名下方 / Network 页，订阅 `https://<PUBLIC_URL>/sub`。

## 原理

- 公开端口是 xray 的纯 TCP 分流器，按 WS 路径分到 VLESS+WS（8004）、VMess（8002）、Trojan（8003），其余流量 fallback 到 3100 订阅服务。
- 主入站必须是 TCP（WS 主入站会先按路径校验回 404，fallback 没机会分流）。
