# FnDepot（mingk326 的应用源）

飞牛 OS 第三方原生 fpk 应用源，结构与 [shuangji66/FnDepot](https://github.com/shuangji66/FnDepot) 一致：`fnpack.json` 描述应用与版本，图标 / 预览图 / 说明放在对应应用目录，安装包通过 GitHub Release 分发。

当前提供：

| 包名 | 显示名 | 说明 | 平台 |
|---|---|---|---|
| `wb2api` | wb2api | WorkBuddy2API：把腾讯 CodeBuddy 账号包装成 OpenAI 兼容 API 的网关，自带 Web 管理面板 | x86 / arm |
| `artex` | ARTEX BaaRai 版 | AI 驱动的自动化安全测试平台（英文界面），PostgreSQL 16.14 由依赖包提供，内置 10 个技能，原生 fpk | x86 / arm |
| `artexcn` | ARTEX RuoJi6 版 | 中文界面的 ARTEX（默认简体中文），含定时任务、代理池、请求重放、案件与 MCP，0.3.31–0.3.32 补齐任务凭据、目标审批门禁与覆盖账本的界面与后端，独立数据库可与 BaaRai 版并存 | x86 / arm |
| `artexpg` | ARTEX PostgreSQL | ARTEX BaaRai 版 / RuoJi6 版的数据库依赖包：PostgreSQL 16.14，仅监听 127.0.0.1:55432 | x86 / arm |

## 添加应用源

飞牛 OS → 应用中心 → 设置 → 第三方应用源 → 添加源，填入**仓库地址**：

```
https://github.com/mingk326/FnDepot
```

客户端会自己去拉取默认分支（`main`）根目录下的 `fnpack.json`，无需填清单文件的直链——这也是社区第三方源通用的填写方式（约定：仓库必须公开、仓库名严格叫 `FnDepot`、清单放在默认分支根目录）。

如果客户端版本不接受仓库地址，可改用清单直链（二选一）：

```
https://raw.githubusercontent.com/mingk326/FnDepot/main/fnpack.json
https://cdn.jsdelivr.net/gh/mingk326/FnDepot@main/fnpack.json
```

添加后在应用中心搜索 `wb2api` 即可安装。

## 直接下载

- [wb2api 1.25.1 x86](https://github.com/mingk326/FnDepot/releases/download/2026.11.17a/wb2api-1.25.1-x86.fpk)
- [wb2api 1.25.1 arm](https://github.com/mingk326/FnDepot/releases/download/2026.11.17a/wb2api-1.25.1-arm.fpk)

历史版本见 [Releases](https://github.com/mingk326/FnDepot/releases)：1.13.3–1.25.1（每版 x86 / arm）。

## 其他应用

- [WorkBuddy Manager 2.11.20 x86](https://github.com/mingk326/FnDepot/releases/download/wbmgr-2026.11.23/wb2mgr-2.11.20-x86.fpk)
- [WorkBuddy Manager 2.11.20 arm](https://github.com/mingk326/FnDepot/releases/download/wbmgr-2026.11.23/wb2mgr-2.11.20-arm.fpk)

WorkBuddy Manager 是 CodeBuddy 账号池管理控制台（内含 OpenAI 兼容反代网关），本包为飞牛原生 fpk，内置 Python 运行时与上游网关，不需要 Docker。默认面板端口 `7864`，上游仅监听 `127.0.0.1:7865`。


安装后面板地址：`http://<NAS-IP>:7863/panel`，模型地址：`http://<NAS-IP>:7863/v1`。面板需**管理员密钥**（安装向导设置的密钥自动为管理员），普通密钥只能调用模型接口。

### ARTEX RuoJi6 版 0.3.33

- [ARTEX RuoJi6 版 0.3.33 x86](https://github.com/mingk326/FnDepot/releases/download/artexcn-2026.11.19c/artexcn-0.3.33-x86.fpk)
- [ARTEX RuoJi6 版 0.3.33 arm](https://github.com/mingk326/FnDepot/releases/download/artexcn-2026.11.19c/artexcn-0.3.33-arm.fpk)
- 同样依赖 [ARTEX PostgreSQL 16.14](https://github.com/mingk326/FnDepot/releases/tag/artex-2026.11.19)
- 旧版：[0.3.32 x86](https://github.com/mingk326/FnDepot/releases/download/artexcn-2026.11.19b/artexcn-0.3.32-x86.fpk) / [arm](https://github.com/mingk326/FnDepot/releases/download/artexcn-2026.11.19b/artexcn-0.3.32-arm.fpk) · [0.3.31 x86](https://github.com/mingk326/FnDepot/releases/download/artexcn-2026.11.19a/artexcn-0.3.31-x86.fpk) / [arm](https://github.com/mingk326/FnDepot/releases/download/artexcn-2026.11.19a/artexcn-0.3.31-arm.fpk)

默认**简体中文**界面，可在右上角切换英文；首次打开直接设置管理员密码（至少 8 位），**没有初始化令牌**，不用翻日志。默认 Web 端口 `28789`、代理 `127.0.0.1:28790`、独立数据库 `artexcn`，因此可以和上面的 ARTEX BaaRai 版并存。内置 10 个技能（中文版 3 个 + 英文版 7 个）。上游 [RuoJi6/ARTEX](https://github.com/RuoJi6/ARTEX)，AGPL-3.0，**仅限已授权目标使用**。

**登录信息**：用户名固定 `ARTEX`；没有内置默认密码，首次打开时自行设置（至少 8 位），测试/演示环境常用 `admin123456`。忘记密码可在应用目录执行 `reset-password.sh -p '新密码'` 重置，无需重启服务。

0.3.31 补齐了 BaaRai 版独有的三组能力（默认关闭）：任务凭据管理、资产目标审批门禁、覆盖账本；0.3.32 把它们全部接入界面；0.3.33 修复日志级别误判（成功的连接测试因响应体含 `"error": null` 被标成 error，现按首行 + HTTP 状态判定）。升级时数据库增量由启动自动补齐。

### ARTEX BaaRai 版 0.6.1

- [ARTEX 0.6.1 x86](https://github.com/mingk326/FnDepot/releases/download/artex-2026.11.19/artex-0.6.1-x86.fpk)
- [ARTEX 0.6.1 arm](https://github.com/mingk326/FnDepot/releases/download/artex-2026.11.19/artex-0.6.1-arm.fpk)
- [ARTEX PostgreSQL 16.14 x86](https://github.com/mingk326/FnDepot/releases/download/artex-2026.11.19/artexpg-16.14-x86.fpk)
- [ARTEX PostgreSQL 16.14 arm](https://github.com/mingk326/FnDepot/releases/download/artex-2026.11.19/artexpg-16.14-arm.fpk)
- 旧版：[ARTEX 0.6.0 x86](https://github.com/mingk326/FnDepot/releases/download/artex-2026.11.19/artex-0.6.0-x86.fpk) / [arm](https://github.com/mingk326/FnDepot/releases/download/artex-2026.11.19/artex-0.6.0-arm.fpk)

0.6.1 起包含桌面入口：安装后飞牛桌面会直接出现 ARTEX 图标，点击打开 Web 界面（此前 0.6.0 安装后无桌面图标）。

ARTEX 是 AI 驱动的自动化安全测试平台（上游 [BaaRai/ARTEX](https://github.com/BaaRai/ARTEX)，AGPL-3.0），**仅限已获得明确授权的目标使用**。本包为飞牛原生 fpk，PostgreSQL 16.14 由依赖包 `artexpg` 提供（安装时应用中心自动先装，仅监听 `127.0.0.1:55432`），默认端口 `8787`，安装后访问 `http://<NAS-IP>:8787/`。应用中心会自动拉取依赖，也可先手动安装 `artexpg`。

## 目录结构

```
.
├── fnpack.json              # 应用源清单（schema_version 2）
├── README.md
├── artex/
│   ├── ICON.PNG             # 应用图标
│   └── README.md            # 应用详情页说明
├── artexcn/
│   ├── ICON.PNG
│   └── README.md
├── artexpg/
│   ├── ICON.PNG
│   └── README.md
└── wb2api/
    ├── ICON.PNG             # 应用图标
    ├── README.md            # 应用详情页说明
    └── Preview/
        ├── 1.png
        ├── 2.png
        └── 3.png
```

新增应用时：

1. 新建 `<包名>/` 目录，放入 `ICON.PNG`、`README.md`、`Preview/*.png`
2. 在 `fnpack.json` 的 `apps` 下增加条目，填写 `releases.<版本>.packages.<平台>` 的 `download_url`、`sha256`、`size`
3. 把对应 fpk 上传到 GitHub Release，tag 与 `download_url` 中的路径保持一致
4. 提交并推送，飞牛刷新应用源即可看到

## 说明

- `wb2api` 的上游维护者是 [linguo2625469](https://github.com/linguo2625469/workbuddy2api-panel)，本仓库只负责飞牛原生 fpk 的打包与分发
- 本应用源与腾讯 / CodeBuddy 官方无关，使用前请阅读应用详情页中的使用说明与风险提示
