# 使用 CI 自动 签署/部署/更新 证书并发布到 Github

本工具可以让你使用 CI 自动 签署/部署/更新 证书并发布到 Github，也提供客户端来使让你要部署的服务器可以自动拉取证书

当域名的证书不存在时，程序可以自动申请证书，但不保证成功。

## 配置

1. 克隆本项目仓库
2. 在本项目目录下创建 data 文件夹
3. 重命名 config.sh.example 为 config.sh
4. 编辑 config.sh

### 配置项说明

| 环境变量名称 | 说明 | 示例 |
| ----------- | -------- | ------ |
| ACME_DOMAINS | 要签发的证书的域名列表 | ("kenvix.com" "*.kenvix.com") |
| ISSUE_PARAMS | 签发证书的额外参数 | "--dns dns_cf" |
| CERT_GIT_COMMIT_MESSAGE | git 提交消息 | aa |

程序所需其他环境变量也请写到本文件

有关 Git 部署的设置请继续阅读

## 部署

要将此程序部署到 CI，请将以下环境变量单独设置到 CI，且应与 config.sh 中保持一致 :

| 环境变量名称 | 说明 | 示例 |
| ----------- | -------- | ------ |
| CERT_GIT_URI | 存储证书的Git地址，若为 https 则包括前面的https:// | git@github.com:kenvix/AutoCert.git <br/> https://username:password@github.com/kenvix/AutoCert.git |
| CERT_GIT_BRANCH | 存储证书的Git分支 | master  |
| CERT_GIT_USER | 存储证书的Git用户名 | kenvix |
| CERT_GIT_EMAIL | 存储证书的Git用户的邮箱 | kenvixzure@live.com |
| APP_GIT_URL | 本程序所在仓库完整地址(原样复制即可) | https://github.com/kenvix/AutoCert.git |

若使用 `https://` 协议克隆仓库，请指明用户名和密码，若使用 ssh 方式，请放置 RSA 密钥到本目录的 `deployment.key` 文件

部署修改好的仓库到支持 CI 的git环境（例如github）

若使用 Github Actions，则重命名 `.github.example` 为 `.github` 即可完成部署。若使用其他 CI 则需要在 CI 设置部署所执行的脚本：

```shell
chmod -R 777 *
ls -al
./ci-cron.sh
```

即可完成部署