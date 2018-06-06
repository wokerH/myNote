# 初步了解HTTP

### 热身

- 第一步： 在浏览器中输入网址： `http://www.baidu.com` ;
- 第二步： 点击确认，浏览器会很快呈现出一个百度的首页（如果你网络好的话）;
- 第三步： 鼠标右键然后再选项框中选择检查（或直接按键盘`f12`键）,会弹出一个浏览器开发者工具框，然后选择 `Network` 选项；
- 第四步：刷新页面，并注意观察开发者工具中的变化。

然后你会看到如下的内容：

![QQ截图20180606114717](C:\Users\Administrator\Desktop\QQ截图20180606114717.png)

这里面有各种文件、图片等乱七八糟的内容就是浏览器绘制出漂亮页面所需的资源文件。而这些 `status` 为200的东东就是成功从远程服务器上加载到本地浏览器上的文件。然而，要将他们成功的加载到本地浏览器就必须要遵循相应的规则，而这个规则就是HTTP协议。



### 什么是HTTP协议

HTTP（HyperText Transfer Protocol，超文本传输协议 ）是一种能够获取如 HTML 这样的网络资源的 [protocol](https://developer.mozilla.org/en-US/docs/Glossary/protocol)(通讯协议)。它是在 Web 上进行数据交换的基础，是一种 client-server 协议。

HTTP协议设计的最初目标就是要解决资源共享的问题。即：让各个计算机直接能够顺利高效的交换数据。

HTTP被设计于上20世纪90年代初期，是一种可扩展的协议。它是应用层的协议，通过[TCP](https://developer.mozilla.org/en-US/docs/Glossary/TCP)，或者是[TLS](https://developer.mozilla.org/en-US/docs/Glossary/TLS)－加密的TCP连接来发送，理论上任何可靠的传输协议都可以使用。 