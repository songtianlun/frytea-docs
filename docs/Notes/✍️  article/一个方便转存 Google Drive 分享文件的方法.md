用过 Google Drive (以下简称GD) 的朋友们应该都清楚，GD 分享的文件可以一键添加到自己的云盘中，速度很快，一度让我感觉 Google 好牛，但仔细一看会发现这并不是将文件转存到自己的 GD 中，以大神分享的爱情公寓5资源为例：

![2020-01-30-11-22-39-eb6691c425cbb3e1.png](https://imagehost-cdn.frytea.com/images/2020/01/30/2020-01-30-11-22-39-eb6691c425cbb3e1.png)

如上图所示，我已经将该资源通过 GD 提供的一键保存按钮将资源放在我的云盘，我已经可以在我的云盘看到，但是仔细看文件详情，目前我还是以分享的方式查看，文件所有者还是共享者。

## 方法一

为了解决这一问题，有多种方法，最直接的一种就是直接在文件上右键，制作一个拷贝，这样一来， GD 就为我们拷贝一份放在了我们的云盘。

![2020-01-30-11-26-01-d8d4773485ac2507.png](https://imagehost-cdn.frytea.com/images/2020/01/30/2020-01-30-11-26-01-d8d4773485ac2507.png)

这一方法很简单直接，但是问题也显而易见，就是对文件夹执行该操作。

除了这一方法，还有一种较为专业，操作起来也较为复杂，但是可以对任何文件进行转存，可以批量处理。

## 方法二

本方法基于 `rclone` ，需准备一台境外大带宽服务器，安装 rclone，绑定云盘，然后使用命令一键转存：

`rclone copy gdvideo:/Movies/Grab/爱情公寓5 onedrivee5:/Public/Video/ -P`

这样的方式可以在云盘内或是云盘间转存文件，灵活方便，功能强大，为问题在于门槛较高。

使用 Rclone 还可以 [Linux 下使用 rclone 挂载网盘到本地](https://blog.frytea.com/archives/31/)

下面来介绍一种最为简单，操作较为方便的方法，基于 Telegram。

## 方法三

前些日子在 Telegram [@NewlearnerChannel](https://t.me/NewlearnerChannel) 频道看到这样一则推送：

```
#Bot #GoogleDrive #telegram

谷歌网盘管理Bot🤖️
@GoogleDriveManagerBot

谷歌网盘挂载观影是很多人喜欢的看剧姿势，但目前转存不便以及文件命名不规范令人心烦，此 Bot 旨在帮助解决这一问题

🪓功能
 ⁃ 谷歌网盘资源转存
 ⁃ 网盘内资源批量重命名

💡提醒
 ⁃ 首次使用需要授权
 ⁃ 普通用户每日有转存上限

✍🏻作者：上官断大

参考内容 (https://softgateon.herokuapp.com/urltodrive/)

本文首发于 @GoogoCC

频道：@NewlearnerChannel
```

本机器人可以实现谷歌网盘资源转存以及网盘内资源批量重命名，普通用户仅可绑定一个 GD 账号，绑定好之后向机器人发送 `/copy`，机器人提示`请输入要拷贝的 Google Drive 资源链接 (可以通过浏览器或 APP 复制):` ，输入您需要转存的资源连接，之后机器人提示 `请输入保存此资源的文件夹链接 (可以通过浏览器或 APP 复制):`，此时输入您需要存入文件夹的 ID（网页访问文件夹，拷贝网址最后一段代码），之后机器人询问是否确认将文件拷贝到某文件夹，**使用键盘** 选择确认即可，之后就可以在 GD 中看到存好的文件了。

![2020-01-30-11-38-57-2902e6a34b0c69c7.png](https://imagehost-cdn.frytea.com/images/2020/01/30/2020-01-30-11-38-57-2902e6a34b0c69c7.png)

## 总结

本文介绍了三种转存 GD 分享文件到自己 GD 的方法，GD 普通用户使用方法三即可，高级用户可使用方法二，普通少文件方法一即可，此外还有其他方法欢迎一起探索！

全文完。
