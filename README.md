# 钉钉提供的内网穿透工具

因安全合规要求，该工具已经下线，推荐采用最新推出的 Stream 模式 5分钟快速接入：


“钉钉开放平台Stream模式共创群”群的钉钉群号： 35365014813

https://qr.dingtalk.com/action/joingroup?code=v1,k1,NrXMLmpJdhjHp1OEGUdSy4zSfaGjzAjiC9VWxMHECmg=&_dt_no_comment=1&origin=11? 邀请你加入钉钉群聊钉钉开放平台Stream模式共创群，点击进入查看详情

![stream](https://github.com/open-dingtalk/pierced/assets/22822/50d15d55-a06b-4ed5-b818-214b133f51e4)



本仓库及以下说明来自钉钉官方开发文档。

> 注意：鉴于很多开发者在临时体验开发时往往没有公网域名或者公网IP，本工具提供了一个公网代理服务，目的是方便开发测试。
> 
> 本工具当前不保证多个开发者随意设置相同的子域名导致的冲突以及通道稳定性，因此正式应用、正式环境必须是真实的公网IP或者域名，正式应用上线绝对不能使用本工具。
>
>
>
测试结束之后请及时关闭内网穿透功能，释放资源以供其他同学共享，共建和谐友爱社区。


## 内网穿透示意图

![](https://img.alicdn.com/imgextra/i2/O1CN01aJ6Q4k1hlEYAyHZL0_!!6000000004317-2-tps-1524-858.png)

## 使用方法

### HTTP 穿透

1. 下载工具

    ```
    git clone https://github.com/open-dingtalk/dingtalk-pierced-client.git
    ```

2. 执行命令 `./ding -config=./ding.cfg -subdomain=域名前缀 端口`。

    以 Mac 为例(m1芯片请进入mac_m1目录)：

    ```
    cd mac/
    chmod 777 ./ding
    ./ding -config=./ding.cfg -subdomain=dingabcde 8080
    ```

    Windows：

    ```
    cd windows_64
    ./ding -config ding.cfg -subdomain dingabcde 8080
    ```

    启动后界面如下图所示：

    ![](https://img.alicdn.com/imgextra/i2/O1CN01BuAh4h1OMysVWmLQl_!!6000000001692-0-tps-1778-604.jpg)

    命令参数说明：

    | 参数      | 说明                                                                                                                              |
    |-----------|-----------------------------------------------------------------------------------------------------------------------------------|
    | config    | 内网穿透的配置文件，按命令照示例固定为钉钉提供的./ding.cfg，无需修改。                                                            |
    | subdomain | 您需要使用的域名前缀，该前缀将会匹配到“vaiwan.cn”前面，例如你的 subdomain 是 dingabcde，启动工具后会将 dingabcde.vaiwan.cn 映射到本地。 |
    | 端口      | 您需要代理的本地服务 http-server 端口，例如你本地端口为 8080 等。                                                                 |

3. 启动完客户端后，你访问 http://dingabcde.vaiwan.cn/xxxxx 都会映射到 http://127.0.0.1:8080/xxxxx。

### 数据库穿透

1. 下载工具

    ```
    git clone https://github.com/open-dingtalk/dingtalk-pierced-client.git
    ```

2. 执行命令 `./ding -config=./ding.cfg -proto=tcp start ssh`。

    以 Mac 为例：

    ```
    cd mac_64
    chmod 777 ./ding
    ./ding -proto=tcp -config=./ding.cfg start ssh
    ```

    启动后界面如下图所示：

    ![](https://img.alicdn.com/imgextra/i2/O1CN01KAwyrE1WGaH0X2Cok_!!6000000002761-0-tps-1766-402.jpg)

    命令参数说明：

    | 参数   | 说明                                                                   |
    |--------|------------------------------------------------------------------------|
    | config | 内网穿透的配置文件，按命令照示例固定为钉钉提供的./ding.cfg，无需修改。 |
    | proto  | 启动的是 TCP 协议穿透。                                                |

3. 在数据库里面执行：

    ```sql
    GRANT ALL PRIVILEGES ON *.* TO root@'%' IDENTIFIED BY '123456';
    FLUSH PRIVILEGES;
    ```

    注意 123456 是数据库远程登录的密码，root 为用户名。

4. 数据库连接命令：

    ```
    mysql -h vaiwan.cn -u root -p -P 1234 //端口号地址
    ```

    1234 是启动远程数据库连接默认的端口，可以在 ding.cfg 中进行修改。

## 注意

1. 你需要访问的域名是 http://dingabcde.vaiwan.cn/xxxxx 而不是 http://dingabcde.vaiwan.cn:8082/xxxxx。

2. 你启动命令的 subdomain 参数有可能被别人占用，尽量不要用常用字符，可以用自己公司名的拼音，例如：alibaba、dingding 等。

3. 可以在本地起个 http-server 服务，放置一个 index.html 文件，然后访问 http://dingabcde.vaiwan.cn/index.html 测试一下。

## 官方文档

- <https://ding-doc.dingtalk.com/doc#/kn6zg7/hb7000>

