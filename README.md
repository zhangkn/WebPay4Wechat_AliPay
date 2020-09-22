# 前言
微信支付宝网页支付的重点步骤：

- 微信的网页支付主要是替换 redirect_url
- 支付宝的网页支付主要是替换fromAppUrlScheme
- 其中包含的回调地址redirect_url是网页支付中, 跳转回来后当前网页刷新的地址（前端定义的, 根据实际情况处理）




# 1. 设置URL Scheme

这个相当于你App的铭牌, 别的App想要跳转到你的App的时候, 就需要知道你App的铭牌,一个App可以设置多个.

# 2. AppDelegate( 这一步 看大家需求操作吧 )

设置进入前台(跳转回来) 处理服务器需要刷新的网页

# 3网页支付

## 3.1 微信网页支付大致流程:

- 获取当前请求的链接,判断是不是微信的
- 保存连接中的参数名为redirect_url的对应数据为appWillBecomeActiveWebReloadUrlString,
- 替换redirect_url = “自定App的URLScheme” 用于,
替换完后, 重新请求连接,
- 跳转,
- 支付完成, 返回app(能返回是因为redirect_url)
在AppDelegate的程序进入前台的代理方法中处理当前网页刷新之前保存的appWillBecomeActiveWebReloadUrlString(至于这个保存的appWillBecomeActiveWebReloadUrlString的链接地址内容是什么,取决于前端如何处理).



## 3.2 - 支付网页支付

流程:

- 获取当前请求的链接,是不是支付宝的
- 保存连接中的参数名为redirect_url的对应数据为appWillBecomeActiveWebReloadUrlString,
- 获取当前请求的链接,判断是不是支付宝跳转的协议链接, 并且包含fromAppUrlScheme
- WebPayTool.handleWebUrl(urlString) 处理支付宝跳转的协议链接, （重点步骤：修改fromAppUrlScheme的参数为自定义的App的URL Scheme）
- 跳转
- 支付完成, 返回app(能返回是因为redirect_url)
- 在AppDelegate的程序进入前台的代理方法中处理当前网页刷新之前保存的appWillBecomeActiveWebReloadUrlString(至于这个保存的appWillBecomeActiveWebReloadUrlString的链接地址内容是什么,取决于前端如何处理).
