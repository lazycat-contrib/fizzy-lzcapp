# Fizzy for LazyCat

这是 [Fizzy](https://github.com/basecamp/fizzy) 的 LazyCat LPK v2 项目，包名为 `community.lazycat.cpp.fizzy`。

运行镜像来自 `ghcr.1ms.run/basecamp/fizzy:sha-<commit>`。同步工作流读取 `basecamp/fizzy` 的最新 GitHub Release（例如 `fizzy@fababf6`），严格转换成同一提交的镜像标签 `sha-fababf6`。发布工作流强制校验 1ms 加速镜像与官方 GHCR 的 `linux/amd64` digest 相同；digest 变化时递增 LPK 补丁版本、创建带版本号的 GitHub Release Asset，并且只发布到喵喵社区商店。

同步工作流每天 01:17 UTC 检查上游 Release，发布工作流每天 03:23 UTC 使用同步后的精确 SHA 标签。也可以分别手动触发两个工作流。

## 设置向导

安装时可以配置：

- 多租户模式；
- SMTP 发件服务器；
- Web Push 的 VAPID 密钥；
- 可选的 S3 兼容对象存储。

`SECRET_KEY_BASE` 会通过 LazyCat 的稳定密钥函数自动生成。`BASE_URL` 使用 LazyCat 分配的应用域名，容器内 SSL 已关闭，由 LazyCat 统一终止 HTTPS。未启用 SMTP 时，Fizzy 会把 6 位登录验证码写入应用日志。

## 构建

需要 `lzc-cli` 2.0.0 或更高版本：

```bash
lzc-cli project release -o build/fizzy.lpk
```

## GitHub Secrets

仓库需要配置以下 Secrets：

- `APPSTORE_URL`
- `APPSTORE_TOKEN`

同名 Organization Secret 必须显式授权当前仓库；Repository Secret 会覆盖同名 Organization Secret。

## 已确认的取舍

- 不添加 Docker/LazyCat 健康检查，与上游 Docker Compose 保持一致。
- 不加入 LazyCat 文件选择器拦截器。
- 不启用 LazyCat 官方商店发布。
