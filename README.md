# sharelatex-full (Overleaf)

[![GitHub license](https://img.shields.io/github/license/Tuetenk0pp/sharelatex-full)](https://github.com/Tuetenk0pp/sharelatex-full/blob/master/LICENSE)
[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/Tuetenk0pp/sharelatex-full/build-test.yml)](https://github.com/Tuetenk0pp/sharelatex-full/actions/workflows/build-test.yml)
[![GitHub issues](https://img.shields.io/github/issues/tuetenk0pp/sharelatex-full)](https://github.com/Tuetenk0pp/sharelatex-full/issues)
[![Docker Pulls](https://img.shields.io/docker/pulls/tuetenk0pp/sharelatex-full)](https://hub.docker.com/r/tuetenk0pp/sharelatex-full)

Extended Overleaf Docker Image.
Current Features include:

- fully updated TeX Live installation, including all available packages
- support for minted
- support for `svg`-images through the addition of inkscape
- support for lilipond
- shell-escape enabled by default

Have a look at the [Dockerfile](./Dockerfile) to find out more.

## Installation

### Overleaf Toolkit
国内服务器，先设置docker的加速代理。然后

'''
docker pull tuetenk0pp/sharelatex-full
'''

第二步安装docker compose 最新版本到系统中，使用系统默认的包管理器方式安装能够很好地自动配置守护进程：

'''
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
'''

第三步，使用非管理员权限用户直接与 docker 守护进程通信，每次使用 docker 命令的时候不必记得加上 sudo 关键字，在 docker 安装文档里其实也有说明。个人来说比较推荐配置这一步，因为大部分教程默认没有 sudo 提权命令，配置非管理员权限用户能更好地避开一下不必要的麻烦。

'''
sudo groupadd docker
sudo usermod -aG docker $USER
'''

'''
git clone https://github.com/overleaf/toolkit.git ./overleaf-toolkit
cd ./overleaf-toolkit
./bin/init
'''

config目录三份配置文件中比较重要的配置项如下：

overleaf.rc
SHARELATEX_PORT 默认为 80 端口，但这个一般会被系统各大应用抢占，如果和你的有冲突，建议修改为 1024~65535 之间的一个数值。

variables.env
SHARELATEX_APP_NAME 这个名字可以自定义，没什么特别的影响。
SHARELATEX_SITE_URL 这个会影响生成用户激活链接里的 URL 的域名地址，建议根据自己的实际情况修改。注意，该域名必须能够被有效解析到服务器，否则请填写 ip 地址作为替代。
SHARELATEX_NAV_TITLE 标签页里的标题，可以自定义。
SHARELATEX_HEADER_IMAGE_URL 为了和 overleaf.com 的区分，我把这个 URL 指向了我们学校的 logo
SHARELATEX_LEFT_FOOTER 显示在注册界面的提示信息，由于没有实际的注册功能，因此需要显示一段文字说明管理员的联系方式，就是这个字段的配置内容。

version
不需要更改。

bin目录docker-compose.base.yml修改image为目标镜像

'''
sharelatex:
    restart: always
    image: tuetenk0pp/sharelatex-full
    container_name: sharelatex
'''

#需要加速镜像

'''
./bin/up 
./bin/start
'''

### Docker Compose

Alternatively, use the [docker-compose.yml](https://github.com/overleaf/overleaf/blob/main/docker-compose.yml) provided in the [official GitHub](https://github.com/overleaf/overleaf), but change the image to ``tuetenk0pp/sharelatex-full``.
Also, note the additional instructions in the [official Wiki](https://github.com/overleaf/overleaf/wiki/Release-Notes--4.x.x#manually-setting-up-mongodb-as-a-replica-set).
