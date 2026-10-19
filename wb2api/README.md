<p>🤖 WorkBuddy2API 是一个自托管的 OpenAI 兼容反向代理网关，将腾讯 CodeBuddy 账号包装为统一的 <code>/v1/chat/completions</code> 服务，并自带 Web 管理面板。不保证未来可用性与账户安全，介意勿用。</p>

## 使用说明

- 面板：`http://192.168.x.x:7863/panel`
- 模型地址：`http://127.0.0.1:7863/v1`
- 面板密码与模型 API Key 相同，安装向导里设置

安装后在面板「添加账号」完成 OAuth 登录，即可在 OpenAI 兼容客户端里使用。

## 当前版本

`1.13.6`

- 修复 `6020 queue.waiting` / `6021 queue.full` 被误判为账号限流，导致整池账号被逐个冻结、其他账号也报 429
- 跨进程 token 刷新兜底：撞 `12153` 时读回磁盘上已被其它进程刷新的新 refreshToken 重试，修复双进程部署掉登录
- `11140 request illegal` 由永久禁用账号改为软冷却，不再误杀仍可用的账号
- 新增任务补领清扫：分钟级幂等 accept / claim，把「进度已达标但没人领」的成长任务奖励补领掉
- 新增 `WorkBuddy_Valorant`（无畏契约拼豆专家）一键完成

## 说明

- 上游项目：[linguo2625469/workbuddy2api-panel](https://github.com/linguo2625469/workbuddy2api-panel)
- 本包为飞牛 OS 原生 fpk（x86 / arm），保留 `auths/`、`config.json`、`state.json`，覆盖升级不丢数据
- 应用数据目录：`/vol1/@appdata/wb2api`（面板「应用文件」可查看）
