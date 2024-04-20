---
layout: article
title: 如何优雅地在windows中使用singbox
tags: 奇技淫巧
key: 2024-03-16-key
---

### singbox简介

**[sing-box](https://sing-box.sagernet.org/)** 是免费跨平台代理软件（The universal proxy platform），客户端支持 Windows、Mac、Android、iOS 和 Apple TV 等平台上使用
<!--more-->

sing-box 项目文档：

**[https://sing-box.sagernet.org](https://sing-box.sagernet.org/)**

sing-box Github 项目地址：

**[https://github.com/SagerNet/sing-box](https://github.com/SagerNet/sing-box)**

sing-box 由于支持新的 Reality 和 hysteria2 协议，目前多为自建节点的科学上网爱好者所用，订阅格式和 Clash、Surge 等软件也不兼容，需使用专属订阅格式

### 使用目标

开机自启，无感使用，使用时没有黑框弹出

目前安卓与ios都有相应的客户端，而windows端官方客户端还在开发中，当然你也可以使用第三方客户端：

- **[GUI.for.SingBox](https://github.com/GUI-for-Cores/GUI.for.SingBox)**
- **[Hiddify-Next](https://github.com/hiddify/hiddify-next)**
- [Nekoray](https://github.com/MatsuriDayo/nekoray)

目前clash生态和v2ray比较成熟，相关的客户端也比较好用，不喜欢折腾的人推荐使用[clash verge](https://github.com/clash-verge-rev/clash-verge-rev)，喜欢折腾的继续往下看

### 简单配置

windows平台至github[下载页](https://github.com/SagerNet/sing-box/releases)选择amd64下载，解压备用，路径随意，请勿删除

- 如果机场有提供singbox订阅链接，那么可以这样做：
    - 配置winsw服务
        
        **[WinSW](https://github.com/winsw/winsw)**，这是一个可以将任何可执行文件配置成 Windows 服务的工具。
        
        先创建一个目录，这里我的目录就叫 sing-box，然后下载 **[WinSW-x64.exe](https://github.com/winsw/winsw/releases/tag/v2.12.0)**，放到这个目录并将其重命名为：**`winsw.exe`**。然后我们创建一个 **`winsw.xml`**，通过这个文件对 sing-box 进行配置。注意！这两个文件的名字一定要相同，否则 WinSW 将无法读取到配置文件。
        
        然后编辑 **`winsw.xml`**，写入以下内容：
        
        ```jsx
        <service>
          <id>sing-box</id>
          <name>sing-box</name>
          <description>This service runs sing-box continuous integration system.</description>
          <download from="https://你的订阅链接" to="%BASE%\config.json" auth="sspi" />
          <executable>C:\Users\homin\scoop\shims\sing-box.exe</executable> <!-- 这里替换为你的绝对路径-->
          <arguments>run</arguments>
          <log mode="reset" />
          <onfailure action="restart" />
        </service>
        ```
        
- 若机场没有提供singbox订阅链接
    
    使用https://github.com/Toperlock/sing-box-subscribe进行订阅转换，如果你有能力，请自行使用github账户fork项目后，在vercel部署
    
    部署完成后，获得你自己的链接，然后再进行订阅转换，这样可以减轻作者的服务器压力
    
    - 订阅转换
        
        转换方法为“你的部署链接/config/机场订阅地址”
        
        以作者的链接：sing-box-subscribe-doraemon.vercel.app为例，假如我的机场订阅地址为https://123456.com&token=xxxxxx，则转换公式为：
        
        ```jsx
        https://sing-box-subscribe-doraemon.vercel.app/config/https://123456.com&token=xxxxxx
        ```
        
        此时会生成一个配置，将其复制到新建文件“config.json”中即可
        
    - 配置winsw
        
        winsw安装方法同上，只需要改动winsw.xml中的内容即可，改动内容后的代码：
        
        ```jsx
        <service>
          <id>sing-box</id>
          <name>sing-box</name>
          <description>This service runs sing-box continuous integration system.</description>
          <executable>C:\Users\homin\scoop\shims\sing-box.exe</executable> <!-- 这里替换为你的绝对路径-->
          <arguments>run</arguments>
          <log mode="reset" />
          <onfailure action="restart" />
        </service>
        ```
        
- 安装winsw服务
    
    ```jsx
    ./winsw.exe install
    ./winsw.exe start
    ./winsw.exe status
    ```
    
    显示Started则表示成功了
    

### 存在问题(已解决)：

有时候wifi图标会显示为有线连接，但并不会影响网络连接
参考了一些方法，解决方法是[让“www.msftconnecttest.com”这个域名走公共DNS解析](https://github.com/SagerNet/sing-box/issues/283#issuecomment-1356181959)，需要在配置文件中添加该域名。

2024-4-20更新：
以上问题已经完全不会出现，可安心食用

### 参考文章：

**[如何在 Windows 中优雅的使用 sing-box](https://www.hauhau.cn/blog/proxy/sing-box-on-windows)**

[Winsw项目文档](https://github.com/winsw/winsw/blob/v3/docs/xml-config-file.md)
