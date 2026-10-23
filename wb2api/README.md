<p>🤖 WorkBuddy2API 是一个自托管的 OpenAI 兼容反向代理网关，将腾讯 CodeBuddy 账号包装为统一的 <code>/v1/chat/completions</code> 服务，并自带 Web 管理面板。不保证未来可用性与账户安全，介意勿用。</p>

## 使用说明

- 面板：`http://192.168.x.x:7863/panel`
- 模型地址：`http://127.0.0.1:7863/v1`
- 安装向导里设置的是**管理员密钥**（旧版 `api_key` 同样恒为管理员）

安装后在面板「添加账号」完成 OAuth 登录，即可在 OpenAI 兼容客户端里使用。

## 当前版本

`1.15.3`

- **403 可观测性**：被密钥模型白名单拒绝的请求，现在会在请求记录与 stdout 流水行里带上**请求模型名**、403 状态与客户端档位（若带），不再出现 `model=null` 的无上下文 403；出图请求的白名单拒绝 / 下架模型 / 域路由不匹配也会留下带模型名的流水行
- **密钥级可用模型白名单**（`api_keys[].allowed_models`）：留空 = 不限制；裸名对 cn / global 两个域都生效，`cn:` / `global:` 前缀只限定该域。名单外模型直接 403 `model_not_allowed`（不选号、不打上游），`/v1/models` 只返回名单内模型，`/v2/chat/busy` 回 `unknown`，`pool.model_fallback` 的降级目标同样受校验；admin 密钥同样受限。面板密钥表单支持从上游模型目录勾选或手填，保存后热生效
- **请求记录「档位」列**：`max->xhigh`（客户端发 max 被降级）/ `gw:max->xhigh`（网关注入后被降级）/ `high`（一致），与 stdout 流水行 `effort=` 同源，旧归档无该字段显示 `—`
- **模型级降级**（`pool.model_fallback`）：上游 6004 / 14003 模型级限流时，同账号自动切到等价模型重试，不再直接失败；冷却仍记在原模型名下
- **品牌词作用域**（`features.sanitize_brand_scope`）：`DeepSeek Harness` 改写可选 `all`（默认）/ `identity`（只洗 system / developer，不改用户历史），面板可热改
- **DeepSeek Search 兼容**：DSH 客户端搜索结果补齐 `date` / `title` / `snippet` 空字段，避免客户端解析失败
- **多模态图片检测修正**：`content` 数组里的 image part 正确识别为图片请求
- **出图目录更新**：`hunyuan-image-alpha` / `-edit` 路由到 CN，`hy-image-3.5` 路由到 global；`gpt-image-2.5-sunburst` 下架并提示改用 `hy-image-3.5`
- **账号级摊开修复**：账号多时不再塌缩到少数时刻（按计划时刻升序遍历）
- **思考档位溯源日志**：请求流水行新增 `effort=max->high` / `gw:max->high`，一眼区分「客户端没发档位」与「网关按模型降级」
- **上游报文 dump**（`upstream.dump_dir`，默认关闭）：出站请求体 + 上游原始 SSE 旁路落盘，排障上游 400 / 工具调用异常
- **账号级摊开**（`schedule.account_spread_minutes`，默认关闭）：排程触发的签到 / 旅行 / 活跃 / 保活按号在窗口内随机偏移执行，不再整点齐发
- **`POST /v2/chat/busy`**：官方客户端模型忙闲预检，本地按池状态返回 `idle` / `busy` / `unknown`
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
