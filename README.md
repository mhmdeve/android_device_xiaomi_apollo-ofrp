# android_device_xiaomi_lmi
For building OFRP for Redmi K30 Pro

OFRP device tree for Redmi K30 Pro

Kernel and all blobs are extracted from [miui_LMI_21.6.23_c35d67a8d7_11.0](https://hugeota.d.miui.com/21.6.23/miui_LMI_21.6.23_c35d67a8d7_11.0.zip) firmware.

The Redmi K30 Pro (codenamed _"lmi"_) is high-end smartphones from Redmi.

Redmi K30 Pro was announced and released in February 2020.

## Device specifications

| Device       | Redmi K30 Pro                       |
| -----------: | :------------------------------------------ |
| SoC          | Qualcomm SM8250 Snapdragon 865              |
| CPU          | 8x Qualcomm® Kryo™ 585 up to 2.84GHz        |
| GPU          | Adreno 630                                  |
| Memory       | 8GB / 12GB RAM (LPDDR5)                     |
| Shipped Android version | 10                               |
| Storage      | 128GB / 256GB / 512GB UFS 3.1 flash storage |
| Battery      | Non-removable Li-Po 4700mAh                 |
| Dimensions   | 163.3 x 75.4 x 8.9 mm                     |
| Display      | 24000 x 1080 (19.5:9), 6.67 inch             |

## Device picture

![Redmi K30 Pro](pictures/lmi.jpg)

## Features

**Works**

- Booting.
- [Decryption](https://github.com/simonsmh/android_bootable_recovery/commits/android-10.0).
- ADB
- MTP
- Super partition functions
- Vibration

**Not Working**
- OTG

Redmi K30 Pro is using Dynamic Partition! We need update from TWRP.

## Compile

Prepare the build environment

```
sudo apt update && sudo apt install git-core gnupg flex bison gperf zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache 
libgl1-mesa-dev libxml2-utils xsltproc unzip openjdk-8-jdk build-essential git repo fastboot adb
sudo apt install git aria2 -y
git clone https://gitlab.com/OrangeFox/misc/scripts
cd scripts
sudo bash setup/android_build_env.sh
sudo bash setup/install_android_sdk.sh
```

Configure ccache
```
# Enable ccache
export USE_CCACHE=1
# Change the ccache cache path
export CCACHE_DIR=~/.ccache
# Take effect
source ~/bashrc
# Configure ccache size
ccache -M 50G
```

First checkout minimal ofrp with omnirom tree:

```
mkdir ~/OrangeFox_10
cd ~/OrangeFox_10
rsync rsync://sources.orangefox.download/sources/fox_10.0 . --progress -a
```

Then add these projects to .repo/manifest.xml:

```xml
<project path="device/xiaomi/lmi" name="KyuoFoxHuyu/android_device_xiaomi_lmi-ofrp" remote="github" revision="R11.0" />
```

Finally execute these:

```
. build/envsetup.sh
lunch omni_lmi-eng
mka recoveryimage ALLOW_MISSING_DEPENDENCIES=true # Only if you use minimal twrp tree.
```

To test it:

```
fastboot boot out/target/product/lmi/recovery.img
```

## Thanks
- [FsCrypt fix by mauronofrio](https://github.com/mauronofrio/android_bootable_recovery)
- [Decryption by bigbiff](https://github.com/bigbiff/android_bootable_recovery)
- [Oneplus 8 TWRP by mauronofrio](https://github.com/mauronofrio/android_device_oneplus_instantnoodle_TWRP)
