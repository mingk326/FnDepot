# FnDepot（mingk326 的应用源）

飞牛 OS 第三方原生 fpk 应用源，结构与 [shuangji66/FnDepot](https://github.com/shuangji66/FnDepot) 一致：`fnpack.json` 描述应用与版本，图标 / 预览图 / 说明放在对应应用目录，安装包通过 GitHub Release 分发。

当前提供：

| 包名 | 显示名 | 说明 | 平台 |
|---|---|---|---|
| `wb2api` | wb2api | WorkBuddy2API：把腾讯 CodeBuddy 账号包装成 OpenAI 兼容 API 的网关，自带 Web 管理面板 | x86 / arm |

## 添加应用源

飞牛 OS → 应用中心 → 设置 → 第三方应用源 → 添加源，填入下面任一地址：

```
https://raw.githubusercontent.com/mingk326/FnDepot/main/fnpack.json
```

国内网络访问 raw.githubusercontent.com 不稳定时，可用 jsDelivr：

```
https://cdn.jsdelivr.net/gh/mingk326/FnDepot@main/fnpack.json
```

添加后在应用中心搜索 `wb2api` 即可安装。

## 直接下载

- [wb2api 1.13.3 x86](https://github.com/mingk326/FnDepot/releases/download/2026.10.18/wb2api-1.13.3-x86.fpk)
- [wb2api 1.13.3 arm](https://github.com/mingk326/FnDepot/releases/download/2026.10.18/wb2api-1.13.3-arm.fpk)

安装后面板地址：`http://<NAS-IP>:7863/panel`，模型地址：`http://<NAS-IP>:7863/v1`，面板密码与模型 API Key 相同。

## 目录结构

```
.
├── fnpack.json              # 应用源清单（schema_version 2）
├── README.md
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
