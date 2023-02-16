+++
date = 2016-11-13T13:50:00Z
title = "Nexus 6 刷 CM 详细步骤"

+++
十月份 Android 7.0 在 Nexus 6 上正式推送后，我的 N6 只要联网就一直会在后台下载东西，特别是在感人的联通数据连接外网的速度下，基本上 SS 服务器的最大速度是多少就能跑到多少，吃了我三四 G 的流量，多交了几十块钱的话费，各种关闭后台数据限制全都没用，于是决定刷机，早就听闻 CM 的大名，决定试一下  
官方的 CM WiKi 有遗漏和错误的地方，让我走了不少弯路，弯路我就不贴出来了，直接上正确步骤  
**## 步骤**  
**#### 准备**  
* 下载系统文件压缩包和 Recovery 镜像  
 直接在官网下载：[[https://download.cyanogenmod.org/?device=shamu](https://download.cyanogenmod.org/?device=shamu "https://download.cyanogenmod.org/?device=shamu")]([https://download.cyanogenmod.org/?device=shamu](https://download.cyanogenmod.org/?device=shamu "https://download.cyanogenmod.org/?device=shamu"))  
 * 开发版 nightly or 稳定版 snapshot * CyanogenMod Build 为 系统文件压缩包，CyanogenMod Recovery 为镜像文件  
* 我是 Recovery 和 CM 系统都刷，如果只是想刷系统不刷 CM 系统的话可以只下载压缩包不下载 Re 文件  
**### 1. 解锁 Bootloader**  
官方说解锁 Bootloader 会清除数据，可我解锁 Bootloader 之后在没刷系统之前又用了一下午手机也没事  
**#### 确保你的电脑可以使用 Fastboot 和 ADB**   
两个命令行工具，因为我安装过 Android Studio ，所以直接在 bash 的环境变量里设定一下路径，让它指向 ADB 和 Fastboot 的目录即可 * adb环境变量设置：  
 打开终端Terminal，输入`open -e .bash_profile`回车打开.bash_profile文件，之前没配置过应该是空的，输入`export PATH=${PATH}:~/Library/Android/sdk/tools:~/Library/Android/sdk/platform-tools`这是两个路径tools和platform-tools，根据自己电脑SDK下tools和platform-tools的路径替换，保存。然后在Terminal输入`source .bash_profile`再输入 adb 和fastboot shell 命令和就不会 command not found 了  
  
**#### 在你的手机上打开 USB 调试模式**  
各位应该都会吧，不会我也没办法了  
**#### 在手机的开发者选项中开启 OEM 解锁选项**  
  
**#### 通过 USB 方式连接手机与电脑**  
略  
**#### Terminal 输入 以下命令让手机进入 Bootloader(Fastboot) 模式**  
`adb reboot bootloader`  
或者手机关机按住电源键＋音量减键也可以进入  
**#### 检查 Fastboot 连接**手机进入 Bootloader 模式后，在终端中输入 `fastboot devices` 检查是否可以通过 Fastboot 工具操作手机  
`fastboot devices`  
* 如果没有显示你的手机序列号，而是显示 waiting for device , Fastboot 可能没有在你的电脑上被正确引导，可以查看一下 [Fastboot 文档]([https://wiki.cyanogenmod.org/w/Doc:_fastboot_intro](https://wiki.cyanogenmod.org/w/Doc:_fastboot_intro "https://wiki.cyanogenmod.org/w/Doc:_fastboot_intro")) 寻找更多信息  
* 如果显示 no permissions fastboot , 尝试一下使用 root 命令运行（需要管理员密码）  
 `sudo fastboot devices` **#### 解锁**  
在 Terminal 中继续输入下面的命令解锁  
`fastboot oem unlock`  
手机上将会显示免责声明，该声明必须同意才能继续解锁，使用音量键上下移动选项，电源键确认选择  
如果手机没有自动重启，你可以手动重启，还是音量键上下电源键确认， Reboot System 选项，现在手机应该是解锁的了，你可以通过手机重启时 Google Logo 下面的开着的小锁图标验证是否解锁，锁开着就表示已经解锁了  
__**由于手机被完全重置，你要重新开启 USB 调试模式以便后续的操作**__  
__**官方文档说是解锁会清除手机里的全部数据，但我到此步为止，手机开机仍然可以正常使用，数据全都存在，完全清除手机里的数据是在第三大步时手动 Wipe 的**__  
**### 2. 使用 Fastboot 安装 Recovery ( 安装 CM Recovery 非 CM 系统)**  
**#### 通过下述命令或手动操作手机进入 Bootloader 模式**  
`adb reboot bootloader`  
并且检查是否进入  
`fastboot devices`  
**#### 通过下面的命令将 Recovery 镜像文件写入手机**  
`fastboot flash recovery your_recovery_image.img`  
有下划线的部分是 Recovery 镜像文件的文件名字  
我在操作此步时遇到了不能找到 img 文件的提示，我把 img 文件放手机 /sdcard 目录里放电脑上 fastboot 文件夹同级目录下都不行，最后找到了原因，让我特别怀疑自己，正确方法应该是__**将 Terminal 工作目录切换到 Recovery 所在的目录，然后执行写入命令**__，我把 Recovery 文件放到桌面上，所以要先 `cd Desktop` 再然后执行写入命令  
**#### 写入成功后，重启设备进入 Recovery 确认安装成功**  
电源键＋音量减进入 Bootloader 模式然后音量键上下电源键确认选择 Recovery 模式进入  
CM 官方提示：某些 ROM 可能会在手机重启后覆盖你写入的 Recovery ，立即启动 Recovery 可以避免这种情况  
**### 3. 在 Recovery 中安装 CM 系统**  
**#### 下载好 CM 系统 zip 包及可选包**  
* 信仰加成可选包：谷歌应用框架 GAE  
 推荐使用 OpenGApps , 一个开源的谷歌框架项目，你能想到的所有优点它都有  
 Nexus 6 的 GAE 在 [这里]([http://opengapps.org/?api=6.0&variant=nano](http://opengapps.org/?api=6.0&variant=nano "http://opengapps.org/?api=6.0&variant=nano")) 下载  
__**下载好之后放到电脑上准备写入手机，因为在接下来的操作中会删除手机上的所有数据**__  
**#### 清除数据**  
这一步可以选择备份，我没有备份，全删了，有种用新手机的错觉哈哈   
手动进入 Recovery ，在 Recovery 中选择 Wipe 和 Factory Reset  
* 先进入 Advanced 菜单选择 Wipe system partition 确定 Wipe  
* 然后返回主菜单选择 Factory reset  
**#### 写入系统 zip 包**  
选择主菜单的 Apply update ，这时有两个选项，用 ADB 方法获得 zip 包更新和使用手机里的 zip 包更新  
因为手机里的东西删光了，所以选择 Apply from ADB，这时屏幕上只显示一个 Cancel sideload 选项，这时候我们在电脑 Terminal 中输入  
`adb sideload /path/to/rom.zip`  
/path/to/rom.zip 为你的 zip 包位置，和写入 Recovery 一样，可以把 zip 包放到桌面然后将 Terminal 的工作目录切换到桌面  
`cd Desktop`  
现在可以直接把 /path/to/rom.zip 替换为 文件名.zip   
在这一步也是停了很久，官方的下载给了 CM 的 Recovery ，文档中却没有写怎么在 CM 的 Recovery 中刷 CM ，坑爹  
输入上述命令后，手机就会显示英文信息正在刷机之类，信仰加成包 OpenGApps 在写入 CM 之后不要重启继续在 Recovery 中再写入，也是用同样的方法  
**#### 安装完成**  
返回 Recovery 主菜单，选择 Reboot system now然后静静等待 CM 系统启动，第一次启动会稍微慢一点，耐心等待一下就好  
* __**重要**__  
 启动完成后会进行一系列初始化，在选择 Wi-Fi 那一步选择__**跳过**__！因为系统初次启动要和国外服务器连接验证之类，由于 GFW 的存在无连接外网，如果没有跳过加入了某个 Wi-Fi 就会卡在验证页面，这时候按住电源键强制重启再跳过即可。当然，如果你有自带 SS 的 Wi-Fi 当我没说  
进入系统界面后连接 Wi-Fi 会发现 Wi-Fi 上有感叹号，这是在 Android 5.0 之后新加入的网络评估功能，用以检测是否连接到网络，可以替换为国内可访问的地址或直接关闭也可，ADB 命令为  
`adb shell settings put global captive_portal_detection_enabled 0`  
至此，CM 就正式可以使用了，第一次使用 Markdown 写这么长的文章，内容难免纰漏错误，有什么看不懂的地方，欢迎各位留言。