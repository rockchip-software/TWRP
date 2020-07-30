#   Rockchip TWRP Compilation Guidelines

##  Overview
<font size=4>Team Win Recovery Project (TWRP) is an open-source software custom recovery image for Android-based devices. It provides a touchscreen-enabled interface with partial material design that allows users to install third-party firmware and back up the current system which are functions often unsupported by stock recovery images. It is, therefore, often installed when flashing, installing or rooting Android devices, although it isn't dependent on a device being rooted to be installed.</font>

##  Compilation Step
1.  Please get the Rockchip Android SDK and setup the build environment. Reference: <https://source.android.com/setup/build/initializing>

2.  ***Back up the default recovery, the directory path is bootable/recovery, so that you can restore the recovery at any time to provide the full SDK.***

3.  Delete the default original recovery

    `rm -rf bootable/recovery`
4.  Clone a TWRP source from Rockchip.

    `git clone https://github.com/rockchip-software/TWRP.git bootable/recovery`
5.  source build/envsetup.sh
    Loading the Android development environment.
6.  lunch

    Select Android compile mode, generally choose rk****-userdebug mode.
7.  modify device/rockchip/common/device.mk

    `BOARD_TWRP_ENABLE := true`
8.  make clean

    Clearing the previously compiled content, because the files in the out directory that have been generated conflict with TWRP, will cause TWRP compilation to fail.
9.  make recoveryimage -j($ JOB)

    Compile the recoveryimage image, $JOB is the maximum thread +1 of the current compilation environment, which can be selected by the customer.
10. The compilation is successful, and recovery.img is generated. The file is located at out/target/product/rkxxxx/.


**<font color=red>Note: The original recovery of the backup must be retained. This TWRP cannot be fully compiled with the SDK. Currently, it can only be compiled separately. Compiling the complete update.img or ota package cannot use TWRP as the recovery. Currently, this TWRP is only provided as a separate image. It is not responsible for the consequences of deleting the original recovery.</font>**

**In device/rockchip/common/device.mk, the variable BOARD_TWRP_ENABLE is the switch that controls whether to add the TWRP option. If you do not compile TWRP, please turn off this option.**

##  Variable Description

+   `TW_THEME`</br>
    This variable can be selected as `landscape_hdpi landscape_mdpi portrait_hdpi portrait_mdpi`, which is the high and low resolution of the horizontal and vertical screen.

+   `TW_EXTRA_LANGUAGES`</br>
    This variable is whether to compile the remaining language packs, including Chinese, Japanese, Korean, etc. If set to false, you cannot select Chinese ,Japanese or Korean.

+   `TW_DEFAULT_LANGUAGE`</br>
    This variable is set to the default language and can be selected as `en zh_CN`. The language pack is located at /gui/theme/common/languages/ and the accompanying language pack is located at /gui/theme/extra-languages/languages/.

+   `DEVICE_RESOLUTION`</br>
    This variable is the resolution of the setup device.

+   `TW_NO_BATT_PERCENT`</br>
    No battery equipment.

+   `TARGET_RECOVERY_FORCE_PIXEL_FORMAT`</br>
    This variable can be selected more. Currently, Rockchip only supports RGB_565.

+   `TW_HAS_MTP`</br>
    This variable depends on whether the device is an MTP setting and needs to modify part of the init in TWRP.

+   `TW_IGNORE_MISC_WIPE_DATA`</br>
    This variable is an instruction to ignore the clear data partition passed from the bootloader. The misc partition here stores the instructions that the bootloader passes to the boot image (boot or recovery) to control the behavior they start.

+   `TWRP_EVENT_LOGGING`</br>
    The variable is whether to print event information such as mouse, keyboard, etc.


##  FAQ
+   Unable to load internal storage

    If your project has data partition encryption enabled, and TWRP does not recognize the encrypted internal storage, it needs to be formatted after the internal storage is recognized.
    Use the Clear-Advanced Clear or Format Data Partition to format the operation.

+   Unable to mount storage

    The same is because your project has opened the data partition encryption, you need to format the storage before you can identify the partition.
