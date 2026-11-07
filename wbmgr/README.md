# WorkBuddy Manager（飞牛原生 fpk）

腾讯 CodeBuddy 账号池管理控制台，内置 OpenAI 兼容反代网关。

本包为**飞牛原生打包**：内置 Python 3.12 运行时、全部 Python 依赖与 workbuddy2api 上游网关二进制，
安装后不依赖 Docker、不需要系统 Python，也不联网下载依赖。

## 安装后

- 面板：`http://<NAS-IP>:7864/`（安装时可改端口）
- 首次启动用安装向导里设置的管理员密码登录（用户名 `admin`）
- 上游网关固定监听 `127.0.0.1:7865`，只供本机管理端调用，不对外暴露

## 与官方部署的区别

- 官方提供的是 Docker / systemd / Windows 安装器；本包是 fpk 重打包，账号、密钥、用量数据都放在应用数据目录
- 升级走应用中心，账号、密钥、配置与数据库保留
- 包内「一键更新」不适用（更新由飞牛应用中心负责）

## 使用边界

仅用于管理本人及亲属自愿授权的账号；禁止批量注册、账号交易、代注册、接码、盗号、
绕过平台风控，以及任何商业转售或盈利性用途。详见上游项目 README。

## 上游与许可

- 管理端：[ithtelab/workbuddy-manager](https://github.com/ithtelab/workbuddy-manager)（MIT）
- 上游网关：workbuddy2api（MIT，源码随官方发布包分发）
