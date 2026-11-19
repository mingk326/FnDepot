# ARTEX RuoJi6 版（飞牛原生 fpk）

AI 自主渗透测试系统的中文发行版。本包为**飞牛原生打包**：PostgreSQL 16.14 由依赖包 `artexpg` 提供（安装本应用时
应用中心自动先装），装好后不依赖 Docker、不需要外部数据库、也不联网下载依赖。

当前版本 **0.3.31**：在上游 v0.3.30 的中文界面上，补齐了 BaaRai 版独有的任务凭据、目标审批门禁与覆盖账本。

## 安装后

- 访问 `http://<NAS-IP>:28789/` 完成首次初始化：直接设置管理员密码（至少 8 位），**没有初始化令牌**
- 用户名固定为 `ARTEX`，登录时填写你设置的密码
- 默认 Web 端口 `28789`（安装向导可修改），流量录制代理 `127.0.0.1:28790`，数据库 `127.0.0.1:55432`
- 使用独立数据库 `artexcn`，与 ARTEX BaaRai 版互不影响，两个应用可以同时安装
- 安装后飞牛桌面会出现「ARTEX RuoJi6 版」图标

## 0.3.31 新增（L2 合并）

三组能力**默认关闭**，不开启时行为与 0.3.30 完全一致：

| 能力 | 入口 | 说明 |
|---|---|---|
| 任务凭据管理 | 任务详情「测试账号」页；`/api/tasks/{id}/credentials` | 密码服务端掩码显示、AES-GCM 落库；空密码表示由 agent 注册后回填；agent 可用 `save_task_credential` / `list_task_credentials` |
| 资产目标审批门禁 | 任务 / 对话的 `target_gate` 开关；`/api/tasks/{id}/target-pins`、`/api/target-gate/exemptions` | 工具调用声明的网络目标必须被人工钉选或配置豁免覆盖；支持钉选撤销与豁免变更历史；WebFetch 与自定义 http / command 工具在重定向与连接层二次判定 |
| 覆盖账本 | worker 工具 `record_coverage`；覆盖接口与报告 | 记录每个面与风险类的检测结果与否定结论；未记录的风险类视为「未检查」，缺口进入规划提示与报告 |

数据库增量由启动时自动补齐（`IF NOT EXISTS`）：`target_pins`、`target_gate_exemption_log`、`coverage_ledger`、
`coverage_ledger_history` 四张表，`intercept_pending` 的 `decided_by` / `target_key`，`tasks` 的 `target_gate` /
`end_reason`，`conversations` 的 `target_gate`。

## 界面与语言

默认简体中文，右上角「切换语言」可切回英文。中文文案覆盖登录、任务、发现、资产、流量、
计划任务、代理池、请求重发、MCP、工具执行等全部页面。

## 内置技能（10 个）

- 中文版自带：`api-recon`、`playwright-cli`、`scopesentry`
- 合并自英文版：`api-security`、`bbot-recon`、`codeaudit`、`edr-bypass-re`、`patch-diff`、`php2shell`、`rea`

其中 `bbot-recon` 需要宿主安装 bbot，`rea` 需要额外的 MCP 服务，`php2shell` 需要其 PoC 脚本；
未安装时这些技能只能提供方法论提示。

## 与 BaaRai 版的区别

| 能力 | RuoJi6 版 0.3.31 | BaaRai 版 0.6.1 |
|---|---|---|
| 界面 | 简体中文（可切英文） | 英文 |
| 初始化 | 直接设密码 | 需日志中的一次性令牌 |
| 特有功能 | 定时任务、代理池、请求重放、发现案件、MCP、任务文件、系统指标 | RSI 经验学习 |
| 内置技能 | 10 个 | 10 个 |

## 使用边界

**仅限对已获得明确授权的目标使用。** 禁止对未授权资产进行扫描、验证或任何攻击性操作；
使用者需自行承担合规责任。默认不要把 Web 端口映射到公网。

## 上游与许可

- 本包版本 0.3.31，基于上游 [RuoJi6/ARTEX](https://github.com/RuoJi6/ARTEX) v0.3.30（AGPL-3.0，基于 Autumn-27/ARTEX）构建
- 本仓库只负责飞牛原生 fpk 的打包与分发，不修改上游源码
