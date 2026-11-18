# ARTEX PostgreSQL（artexpg）

ARTEX 专用的 PostgreSQL 16.14 原生运行时，作为 `artex` 的安装依赖自动部署，
也可以单独安装，给本机其他应用使用。

## 说明

- 仅监听 `127.0.0.1:55432`，不对外暴露；socket 位于 `/vol*/@appdata/artexpg/run`
- 超级用户 `artex`，默认数据库 `artex`；密码安装时随机生成，写入 `/vol*/@appdata/artexpg/pgpass`（0644）
- 集群数据在 `/vol*/@appdata/artexpg/pgdata`，首次启动时初始化；日志在 `pg.log`
- 服务进程以专用用户 `artexpg` 运行，**不使用 root**
- 不修改宿主系统目录：PG 编译期写死的 SHAREDIR 由每次启动的私有 mount 命名空间绑定，宿主 `/usr/share/postgresql` 保持原样
- 压缩包内只带 PG16 与必要共享库，JIT 已关闭；glibc 核心库跟随宿主（要求 glibc ≥ 2.34）

## 卸载

卸载时可选择保留或删除 `pgdata`。若 `artex` 或其他应用仍在使用该数据库，
请先停止它们再卸载；删除数据后不可恢复。

## 上游与许可

- 上游：[PostgreSQL](https://www.postgresql.org/)，二进制取自 [PGDG](https://apt.postgresql.org/) `16.14-1.pgdg12+1`
- 许可证：PostgreSQL License / BSD 类，随包附带版权声明（`LICENSE.txt`）
