# 腾讯云 Werobot Serverless Component

## 简介

腾讯云 Werobot Serverless Component.

## 目录

0. [准备](#0-准备)
1. [安装](#1-安装)
1. [配置](#2-配置)
1. [部署](#3-部署)
1. [移除](#4-移除)

### 0. 准备 `

安装Werobot，新建python文件，例如`app.py`：

```pyyhon
import os
import werobot

robot = werobot.WeRoBot(token=os.environ.get('wxtoken'))

robot.config['SESSION_STORAGE'] = False
robot.config["APP_ID"] = os.environ.get('appid')
robot.config["APP_SECRET"] = os.environ.get('secret')

# @robot.handler 处理所有消息
@robot.handler
def hello(message):
    return 'Hello World!'

if __name__ == "__main__":
    # 让服务器监听在 0.0.0.0:80
    robot.config['HOST'] = '0.0.0.0'
    robot.config['PORT'] = 80
    robot.run()



```

并将python所需要的依赖安装到项目目录，例如本实例需要`werobot`，所以可以通过`pip`进行安装：

```
pip install werobot -t ./
```

如果因为网络问题，可以考虑使用国内源，例如：

```
pip install werobot -t ./ -i https://pypi.tuna.tsinghua.edu.cn/simple
```

### 1. 安装

通过 npm 全局安装 [serverless cli](https://github.com/serverless/serverless)

```shell
$ npm install -g serverless
```

### 2. 配置

本地创建 `serverless.yml` 文件，在其中进行如下配置

```shell
$ touch serverless.yml
```

```yml
Weixin_Werobot:
  component: "@serverless/tencent-werobot"
  inputs:
    functionName: Weixin_Werobot
    code: ./test
    werobotProjectName: app
    werobotAttrName: robot
    functionConf:
      timeout: 10
      memorySize: 256
      environment:
        variables:
          wxtoken: *********
          appid: *********
          secret: *********
      apigatewayConf:
        protocols:
          - http
        environment: release

```

* 这里的`werobotProjectName`实际上是我们`werobot`项目的入口文件，例如本例子终究是`app.py`，此处写`app`即可，`werobotAttrName`则是创建的`WeRoBot()`对象的名字，例如`robot = werobot.WeRoBot(token='yourtoken')`，则此处填写`robot`即可

- [更多配置](docs/configure.md)

### 3. 部署

如您的账号未 [登陆](https://cloud.tencent.com/login) 或 [注册](https://cloud.tencent.com/register) 腾讯云，您可以直接通过 `微信` 扫描命令行中的二维码进行授权登陆和注册。

通过 `sls` 命令进行部署，并可以添加 `--debug` 参数查看部署过程中的信息

```shell
$ sls --debug
```

### 4. 移除

通过以下命令移除部署的服务

```shell
$ sls remove --debug
```

### 账号配置（可选）

当前默认支持 CLI 扫描二维码登录，如您希望配置持久的环境变量/秘钥信息，也可以本地创建 `.env` 文件

```shell
$ touch .env # 腾讯云的配置信息
```

在 `.env` 文件中配置腾讯云的 SecretId 和 SecretKey 信息并保存

如果没有腾讯云账号，可以在此 [注册新账号](https://cloud.tencent.com/register)。

如果已有腾讯云账号，可以在 [API 密钥管理](https://console.cloud.tencent.com/cam/capi) 中获取 `SecretId` 和`SecretKey`.

```text
# .env
TENCENT_SECRET_ID=123
TENCENT_SECRET_KEY=123
```

