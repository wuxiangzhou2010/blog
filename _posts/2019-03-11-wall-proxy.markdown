---
layout: post
title: "wall proxy"
date: 2019-03-11 18:52 +0800
categories: proxy
---

<!-- # build a proxy

## Free trial VPS

most big cloud company provide 12 months free tier for specific product

- [Amazon web services free trial](https://aws.amazon.com/free/)
- [Google cloud platform](https://cloud.google.com/free/)
- [Microsoft azure](https://azure.microsoft.com/free/)

## remote access tool (ssh)

- [putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

  free

- [xshell](https://www.netsarang.com/en/xshell/)

  free for home and school use

## proxy tool

- [shadowsocks python version](https://github.com/shadowsocks/shadowsocks)
- v2ray core

  - [official website](http://v2ray.com/)

  - [github](https://github.com/v2ray)

- [wireguard](https://www.wireguard.com/)

in short -->

1.  一台海外的虚拟主机（VPS）:

    - 有不少厂商有一年的免费期， 比如亚马逊 aws， 微软 azure , 谷歌的 google cloud platform。

      - [Amazon web services free trial](https://aws.amazon.com/free/)
      - [Google cloud platform free trial](https://cloud.google.com/free/)
      - [Microsoft azure free trial](https://azure.microsoft.com/free/)

    - 收费的 VPS, 如 bandwagon,vultr, digital ocean 的话，选择配置最低的足够了，一个月 5 美元左右。
    - 地理位置尽量选择近一点的， 如台湾， 日本， 新加坡。 但是也要注意延迟和丢包率。美国西部和欧洲的服务器虽然延迟大，但是可能更稳定。

2.  远程登陆工具:

    推荐 [putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)， [xshell](https://www.netsarang.com/en/xshell/), 用来登录购买的机器， 安装，设置， 并启动代理软件

3.  代理工具:

    推荐 `v2ray` 或者 `shadowsocks`
    具体使用方法可以参考下面的连接

<!--
            - [v2ray](http://v2ray.com/)

              - [安装](https://v2ray.com/chapter_00/install.html)

                登录机器后以 root 的身份， 直接运行下面的脚本就可以了

                ```sh
                bash <(curl -L -s https://install.direct/go.sh)
                ```

              - [设置](https://v2ray.com/chapter_00/start.html)

                1. 配置文件在 `/etc/v2ray/config.json`

                2. 其中的 users id 可以用[这个](https://www.uuidgenerator.net/)链接生成
                3. 注意服务器要配置中指定的端口

                客户端配置:

                ```json
                {
                  "inbounds": [
                    {
                      "port": 1080, // SOCKS 代理端口，在浏览器中需配置代理并指向这个端口
                      "listen": "127.0.0.1",
                      "protocol": "socks",
                      "settings": {
                        "udp": true
                      }
                    }
                  ],
                  "outbounds": [
                    {
                      "protocol": "vmess",
                      "settings": {
                        "vnext": [
                          {
                            "address": "server", // 服务器地址，请修改为你自己的服务器 ip 或域名
                            "port": 10086, // 服务器端口
                            "users": [{ "id": "b831381d-6324-4d53-ad4f-8cda48b30811" }]
                          }
                        ]
                      }
                    },
                    {
                      "protocol": "freedom",
                      "tag": "direct",
                      "settings": {}
                    }
                  ],
                  "routing": {
                    "domainStrategy": "IPOnDemand",
                    "rules": [
                      {
                        "type": "field",
                        "ip": ["geoip:private"],
                        "outboundTag": "direct"
                      }
                    ]
                  }
                }
                ```

                服务器端配置:

                ```json
                {
                  "inbounds": [
                    {
                      "port": 10086, // 服务器监听端口，必须和上面的一样
                      "protocol": "vmess",
                      "settings": {
                        "clients": [{ "id": "b831381d-6324-4d53-ad4f-8cda48b30811" }]
                      }
                    }
                  ],
                  "outbounds": [
                    {
                      "protocol": "freedom",
                      "settings": {}
                    }
                  ]
                }
                ```

-->

- shadowsocks

  使用上面的 ssh 工具登录服务器

  - 下载源码

    ```sh
    git clone https://github.com/shadowsocks/shadowsocks.git ss
    cd ss
    git checkout origin/master
    ```

  - 配置服务器

    1. 查看服务器 IP， 并替换下面的配置文件中的`xx`

       ```sh
       ip addr
       ```

    2. 确保服务器相应端口开放。

    生成服务端配置文件

    ```sh
    cat>config.json<<EOF
    {
    "server":"xx.xx.xx.xx",
    "server_port":8838,
    "local_port":1080,
    "password":"xx",
    "timeout":600,
    "method":"AES-256-CFB"
    }
    EOF
    ```

  - 运行服务器

    ```sh
    python shadowsocks/server.py -c config.json -d start
    ```

  - 运行客户端

    windows 下使用图形界面的客户端， [点击下载](https://github.com/shadowsocks/shadowsocks-windows/releases)

    需要配置的内容如图

    1. server address
    2. server port (同服务器配置中的 server_port)
    3. password
    4. encription
    5. proxy port (同服务器配置中的 local_port)

    ![如图](/asserts/ssconfig.png)
