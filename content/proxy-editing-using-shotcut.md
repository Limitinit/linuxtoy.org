Title: 在 Shotcut 中使用代理剪辑编辑视频
Date: 2022-01-10 17:00
Category: Hotwo
Tags: shotcut, nle
Slug: proxy-editing-using-shotcut
Authors: lovenemesis

随着视频录制设备和高速移动网络的普及，人们越来越热衷于使用视频的方式记录生活，之后通过社交网络传播。虽然现在能录制和回放 4K 视频的设备价格已经十分亲民了，但是能流畅剪辑 4K 视频的设备依然门槛不低，特别是在现在这个芯片荒的时期。幸运的是，不少非线性视频剪辑编辑软件可以通过“代理素材”的方式，配合录制设备的“机内代理”功能，在低配置甚至轻薄本上流畅的完成 4K 视频素材的剪辑工作。

<!-- PELICAN_END_SUMMARY -->

# 使用软件 #

这里使用基于[开源非线性编辑媒体框架 MLT](https://www.mltframework.org/) 的 [Shotcut](https://www.shotcut.org/) 为例。相较于其他使用 MLT 框架的开源非线编软件，Shotcut 的主力开发者和 MLT 框架为同一人，可以紧跟 MLT 的改善，此外其跨平台兼容性也更完善一些。不少同类软件也具备该功能，可以参考其帮助手册获知。

# 第一步：开启机内代理功能 #

所谓机内代理，是指设备在正常录制视频素材的同时以相同帧率和色彩空间再额外录制一份低码率低分辨率的剪辑。这个剪辑便是正常录制的视频素材的代理素材，由于是在录制设备直接生成的，亦可称之“机内代理”。

不同录制设备的开启方式不尽相同，Sony 微单系列的开启方式按产品推出时间分为两类，这里列举出来供参考：

* [Sony A7M3 及更早机型](https://helpguide.sony.net/ilc/1720/v1/zh-cn/contents/TP0001667532.html)
* [Sony A7SM3 及更新机型](https://helpguide.sony.net/ilc/2010/v1/zh-cn/contents/TP0003226532.html)

其他品牌和设备的开启方式可以自行查阅设备的说明书即可。

接下来的步骤说明将使用 Sony A7M3 生成的视频素材作为例子，但其步骤和原理适用于所有具备“机内代理”功能的设备。

# 第二步：获取机内代理视频 #

Sony 设备录制的原始视频素材保存在存储卡 `PRIVATE\MP4ROOT\CLIP` 文件夹下，而对应的代理素材则保存在 `PRIVATE\MP4ROOT\CLIP\SUB` 文件夹下。原始视频素材的名字和代理素材也有一定关系，假如原始视频素材的文件名是 `C0001.MP4`，那么其对应代理素材的文件名则会是 `C0001S03.MP4`。

若此时已经启动了 Shotcut 并创建了一个新项目，或者打开了一个现有项目，那么可以将原始素材复制到项目的文件夹下，代理素材则需要放置到项目文件夹中**名称为 `proxies` 的子文件夹**中（注意大小写）。

假设现在开启的剪辑项目名称是 `ProxyTest`，那么原始素材应存放于 `ProxyTest\C0001.MP4`，而代理素材则位于 `ProxyTest\proxies\C0001S03.MP4`。

# 第三步：识别并使用机内代理视频 #

假设 Shotcut 处于“编辑”布局的默认状态：

1. 那么在 Shotcut 中打开 `ProxyTest\C0001.MP4`，它的图像会显示在界面中间播放器的“源”标签中；
2. 此时在左侧面板内点击“属性”标签，可以看到的原始素材的编解码器、分辨率等信息；
3. 在上述界面下方，点击 “代理素材” 按钮，在弹出菜单中选择 “复制哈希码”，此时会有弹出框提示已经将哈希码复制到剪贴板，比如 `816aab2bfc97c39327c35537ebeb15fd`；
4. 将 `proxies` 子文件夹下的**代理素材文件重命名为上述哈希码**，后缀名为 `.mp4`（注意大小写），比如这里就需要将 `ProxyTest\proxies\C0001S03.MP4` 重命名为`ProxyTest\proxies\816aab2bfc97c39327c35537ebeb15fd.mp4`；
5. 如果有多个视频剪辑的话，重复上述步骤，将所有的代理素材文件使用源自各自原始素材的哈希码重命名。
6. 之后在 Shotcut 菜单栏中选择 “设置” - “代理素材” - “使用代理素材” 开启代理素材使用，或者使用快捷键 `F4`。
7. 此时，在“属性”标签里，分辨率的那一栏会变成代理素材的分辨率（比如 `1280x720`），并有括号显著的标识是代理素材，同时中间播放器区域也会提示“以 540p 启用代理素材”。如果 1 - 4 步骤操作完全正确的话，这个步骤应该是瞬间完成的。注意这里 `540p` 代表 Shotcut 默认的后期代理素材的分辨率。因为这里直接使用的机内代理，所以肯定和实际代理素材的尺寸有出入，但并没有任何影响；
8. 此时再使用“时间线”工具栏的“追加”按钮将“源”追加到时间线时，便是小尺寸的机内代理素材了。

# 第四步：输出 #

由于代理素材较小的分辨率和低码率，在诸如轻薄本之类低配置的设备上进行剪辑操作也不会有问题，同时机内代理完全相同的帧率和色彩空间也允许进行时间轴和色调的调整。而在输出的时候，**Shotcut 默认会调用原始素材进行渲染工作**的，所以是无需任何额外操作的。

另一方面，一个视频通常在最终版本交付前会有多个实验性质的中间版本，此时也可以**利用“代理素材”加快输出那些对于画质无要求的中间版本**，减少等待时间。此时只需要在点击“输出”后弹出的面板里，勾选“高级”对话框中的“使用预览缩放”即可。

# 如果设备不具备机内代理功能 #

如果手头的录制设备不具备机内代理功能，那么只能依赖 Shotcut 的后期代理素材功能了。默认情况下 Shorcut 会在开启“使用代理素材”后为每个时间线中的的剪辑片段用转码的方式生成代理素材文件，并在完成中提示重启项目并加载。

上述的方案固然解决了剪辑操作时快速跳转等问题，但为每个剪辑生成代理素材的制作过程有可能相当耗费时间，特别是在低配置设备上。此时可以使用另一款[开源视频剪辑软件 Avidemux](http://avidemux.sourceforge.net/) 进行“预剪辑”，将素材中用不到的片段以免转码的方法先行剔除，再导入 Shotcut 中生成代理素材。这样可以减少生成代理素材所用的时间。

# 来源及参考 #

* [Proxy Editing](https://forum.shotcut.org/t/proxy-editing/18517/5)
* [Shotcut FAQ](https://www.shotcut.org/FAQ/#how-do-i-cut-or-trim-a-clip-without-encoding-or-transcoding-it)