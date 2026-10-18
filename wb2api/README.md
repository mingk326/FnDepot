<p>🤖 WorkBuddy2API 是一个自托管的 OpenAI 兼容反向代理网关，将腾讯 CodeBuddy 账号包装为统一的 <code>/v1/chat/completions</code> 服务，并自带 Web 管理面板。不保证未来可用性与账户安全，介意勿用。</p>

## 使用说明

- 面板：`http://192.168.x.x:7863/panel`
- 模型地址：`http://127.0.0.1:7863/v1`
- 面板密码与模型 API Key 相同，安装向导里设置

安装后在面板「添加账号」完成 OAuth 登录，即可在 OpenAI 兼容客户端里使用。

## 当前版本

`1.13.3`

- 修复并发刷新导致 refreshToken 被重复消费、账号掉登录
- 余额探测 401 增加 refresh 求证，面板透出「疑似登录失效 · 待重登」
- chat 出站恒发 `X-Conversation-*` 三件套，修复国际版 `11-128 / conversation id missing`
- `429 + code 6021 queue.full` 不再误判为账号限流，避免整池冷却

## 说明

- 上游项目：[linguo2625469/workbuddy2api-panel](https://github.com/linguo2625469/workbuddy2api-panel)
- 本包为飞牛 OS 原生 fpk（x86 / arm），保留 `auths/`、`config.json`、`state.json`，覆盖升级不丢数据
- 应用数据目录：`/vol1/@appdata/wb2api`（面板「应用文件」可查看）
