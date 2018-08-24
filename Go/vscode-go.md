# VSCode 中配置 Go 开发环境的问题

## 背景

安装完 vscode 官方提供的 Go 插件后，可以发现，当准备将插件投入使用时，vscode 将会产生很多提醒，提醒如 golint 之类的工具没有安装，并可以通过 vscode 提供的快捷按钮将工具链全部安装好

但是，这一系列工具并不能全部安装成功。相当一部分工具是需要从 [https://golang.org/x/tools](https://golang.org/x/tools) 上进行获取，然而，由于网络的原因，这个地址并不能很通畅地进行访问，甚至是不能访问。这导致了一部分工具安装失败

---

即使，当我们将 Shadowsocks 配置为全局代理后，也是不能访问到工具的下载地址，这是由于在终端中，其发起的网络访问并没有经过我们的代理

因此，我们需要手动在终端中配置代理

## 终端配置代理

- 查看 Shadowsocks 的偏好设置，找到 HTTP 的 tab, 记下代理监听的 IP 地址与端口，如本人的是 `127.0.0.1:1087`
- 到终端中输入脚本 `export https_proxy="http://127.0.0.1:1087"`, 由于工具的下载地址是 https 协议的，我们只需要配置 https 的代理就好了。使用以上方法配置的终端代理，只会在这个终端的环境下有效，终端关闭后，代理将会消失
- 在当前终端中，可以通过 `code /path/to/your/go-project` 打开 vscode, 此时，当前终端的环境会带到 vscode 中，因此，可以通过 vscode 的提示，一键安装好 Go 开发的工具

---

于是，可以愉快地学习 Go 了
