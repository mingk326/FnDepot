<p>🤖 WorkBuddy2API 是一个自托管的 OpenAI 兼容反向代理网关，将腾讯 CodeBuddy 账号包装为统一的 <code>/v1/chat/completions</code> 服务，并自带 Web 管理面板。不保证未来可用性与账户安全，介意勿用。</p>

## 使用说明

- 面板：`http://192.168.x.x:7863/panel`
- 模型地址：`http://127.0.0.1:7863/v1`
- 安装向导里设置的是**管理员密钥**（旧版 `api_key` 同样恒为管理员）

安装后在面板「添加账号」完成 OAuth 登录，即可在 OpenAI 兼容客户端里使用。

## 当前版本

`1.14.1`

- **单号旅行端点**：`POST /panel/api/accounts/{uid}/travel` 单号推进一趟（无猫领养 / 有猫派出或领奖），不带号间限速，外部调度器可实现「每号独立随机时点 + 随机间隔」；面板账号行新增「旅行」按钮（仅 CN 个人账号）
- **多 API 密钥与按密钥思考档位**：配置页新增「API 密钥与思考档位」表，可新增 / 编辑 / 删除多把密钥；每把可单独设置默认档位与按模型覆盖（优先级：客户端显式 > 密钥按模型 > 密钥默认 > 全局 `thinking.*`）
- **面板权限分权**：只有管理员密钥能进入面板与 `/admin` 补跑；普通密钥只能调用 `/v1/*`。旧版 `api_key` 继续有效且自动为管理员，老配置零迁移
- 密钥列表脱敏展示，完整密钥仅在创建时回显一次；禁止删到一把不剩或删掉最后一把管理员
- 新增网关默认思考档位（`thinking.default_effort` / `thinking.per_model`），客户端未显式带档位时自动注入
- 截断自动续写：长输出在 `finish_reason=length` 截断时自动续写拼接，Codex 写大文件不再中断
- `/v1/messages`（Claude Code）与 `/v1/responses`（Codex）的 `web_search` 由网关代执行
- 更早版本改动见 [Releases](https://github.com/mingk326/FnDepot/releases)（1.13.3 起）

## 说明

- 上游项目：[linguo2625469/workbuddy2api-panel](https://github.com/linguo2625469/workbuddy2api-panel)
- 本包为飞牛 OS 原生 fpk（x86 / arm），保留 `auths/`、`config.json`、`state.json`，覆盖升级不丢数据
- 应用数据目录：`/vol1/@appdata/wb2api`（面板「应用文件」可查看）
