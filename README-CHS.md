#   Rockchip TWRP 编译指南

##  概述
<font size=4>Team Win Recovery Project (TWRP)是一个开放源码软件的定制恢复模式映像，供基于安卓的设备使用。它提供了一个支持鼠标操作的界面，允许用户向第三方安装固件和备份当前的系统。因此经常在 root 系统时安装，但是它并不是 root 之后才能安装。</font>

##  编译步骤
1.  请获取Rockchip Android SDK，建立编译环境，参考：<https://source.android.com/setup/build/initializing>

2.  ***备份默认recovery，目录路径为bootable/recovery，以便随时恢复recovery提供完整SDK。***

3.  删除默认原版recovery

    `rm -rf bootable/recovery`
4.  克隆一份Rockchip提供的TWRP源码。

    `git clone https://github.com/rockchip-software/TWRP.git bootable/recovery`
5.  source build/envsetup.sh
    加载Android开发环境。
6.  lunch

    选择Android编译模式，一般情况下选择rk****-userdebug模式。
7.  修改device/rockchip/common/device.mk

    `TARGET_TWRP_FLAG := true`
8.  make clean

    清除之前编译的内容，由于已经生成的out目录中的文件与TWRP冲突，会导致TWRP编译失败。
9.  make recoveryimage -j($ JOB)

    编译recoveryimage镜像，$JOB为当前编译环境最大线程+1，可由客户自行选择。
10. 编译成功，生成recovery.img，该文件位于out/target/product/rkxxxx/。


**<font color=red>注意：备份的原版recovery务必保留，此TWRP无法与SDK进行完整编译，目前只能单独编译，编译完整update.img或者ota包，都无法使用TWRP作为recovery，目前此twrp只作为单独镜像提供。删除原版recovery的一切后果概不负责。</font>**

**在device/rockchip/common/device.mk中，变量TARGET_TWRP_FLAG即为控制是否加入TWRP选项的开关，暂无编译TWRP时，请关闭此选项。**

##  变量说明

+   `TW_THEME`</br>
    此变量可选择 `landscape_hdpi landscape_mdpi portrait_hdpi portrait_mdpi`，即横竖屏的高低分辨率。

+   `TW_EXTRA_LANGUAGES`</br>
    此变量为是否编译其余的语言包，包括中文、日本、韩文等，设置为false即无法选择中文等。

+   `TW_DEFAULT_LANGUAGE`</br>
    此变量为设置默认语言，可选择如 `en zh_CN`。语言包位于/gui/theme/common/languages/，附带的语言包位于/gui/theme/extra-languages/languages/。

+   `DEVICE_RESOLUTION`</br>
    此变量为设置设备的分辨率。

+   `TW_NO_BATT_PERCENT`</br>
    无电池设备。

+   `TARGET_RECOVERY_FORCE_PIXEL_FORMAT`</br>
    此变量可选择较多，目前Rockchip只支持RGB_565。

+   `TW_HAS_MTP`</br>
    此变量取决于设备是否为MTP设置，需要修改TWRP中的部分init

+   `TW_IGNORE_MISC_WIPE_DATA`</br>
    此变量为是否忽略从Bootloader传递而来的清除data分区的指令。这里的misc分区存放了Bootloader传递给启动映像（boot或recovery）的指令，可以控制它们启动的行为。

+   `TWRP_EVENT_LOGGING`</br>
    该变量为是否打印事件信息，如鼠标，键盘等事件。

+   `TW_USE_TOOLBOX`</br>
    该变量为选择TWRP使用toolbox或者busybox，目前使用toolbox，平台尚未处理busybox的源码编译。


##  常见问题
+   无法加载内置存储

    如果你的工程开启了data分区加密，而TWRP对于已经加密的内置存储无法识别，因此需要对内置存储进行格式化操作之后才可以识别。
    即使用 清除-高级清除 或者 格式化Data分区 进行格式化操作。

+   无法挂载存储

    同是因为你的工程开启了data分区加密，需要格式化存储后才可识别到分区。
