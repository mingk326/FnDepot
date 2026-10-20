<p>🤖 WorkBuddy2API 是一个自托管的 OpenAI 兼容反向代理网关，将腾讯 CodeBuddy 账号包装为统一的 <code>/v1/chat/completions</code> 服务，并自带 Web 管理面板。不保证未来可用性与账户安全，介意勿用。</p>

## 使用说明

- 面板：`http://192.168.x.x:7863/panel`
- 模型地址：`http://127.0.0.1:7863/v1`
- 面板密码与模型 API Key 相同，安装向导里设置

安装后在面板「添加账号」完成 OAuth 登录，即可在 OpenAI 兼容客户端里使用。

## 当前版本

`1.13.9`

- **新增网关默认思考档位**：`thinking.default_effort` / `thinking.per_model`，客户端未显式带档位时自动注入，Codex / Trae 等客户端无需单独设置；显式档位与关闭思考不覆盖；面板可热改
- 截断自动续写：长输出在 `finish_reason=length` 截断时自动续写拼接，Codex 写大文件不再中断
- `/v1/messages`（Claude Code）与 `/v1/responses`（Codex）的 `web_search` 由网关代执行
- 新增积分变动流水、补偿 / 礼包面板领取、错误聚合、模型繁忙度、账号套餐分类
- 支持静态 API Key 账号（`ck_` / `pt_`）、国际版额度新口径、Webhook 告警、`/admin` 手动补跑
- 更早版本改动见 [Releases](https://github.com/mingk326/FnDepot/releases)（1.13.3 起）

## 说明

- 上游项目：[linguo2625469/workbuddy2api-panel](https://github.com/linguo2625469/workbuddy2api-panel)
- 本包为飞牛 OS 原生 fpk（x86 / arm），保留 `auths/`、`config.json`、`state.json`，覆盖升级不丢数据
- 应用数据目录：`/vol1/@appdata/wb2api`（面板「应用文件」可查看）
