# 命令行工具笔记

## Linux文件管理

查找文件:
> find /path -maxdepth 2 -name "filename"

btrfs备份/删除快照:
> sudo btrfs subvol snapshot -r / snapshot_25mmdd  
> sudo btrfs subvol delete snapshot_25mmdd

## FFMPEG

视频剪辑:
> ffmpeg -i in.mp4 -ss 00:00:00 -t 00:00:10 out.mp4

裁剪局部画面:
> ffmpeg -i in.mp4 -vf crop=w:h(:x:y) out.mp4

整体缩放:
> ffmpeg -i in.mp4 -vf scale=iw*.5:ih*.5 out.mp4

变速:
> ffmpeg -i in.mp4 -filter:v "setpts=0.5*PTS" out.mp4

调整码率:
> ffmpeg -i in.mp4 -b:v 1m out.mp4
> ffmpeg -i in.mp4 -b:v 256k -crf 0 out.mp4

调整音量:
> ffmpeg -i in.wav -af "volume=2" out.wav

## ADB

查看已连接设备:
> adb devices

模拟操作:
> adb shell input tap x y
> adb shell input swipe x1 y1 x2 y2 duration(ms)

安装apk:
> adb install -r xxx.apk

查看所有已安装应用包名:
> adb shell pm -l

根据包名获取已安装应用的apk位置:
> adb shell pm path 包名

传输文件:
> adb push xxx.apk /sdcard/...  
> adb pull /sdcard/oculus/screenshots screenshots

安装分卷apk:
> *先传文件*  
> adb push 1.apk /data/local/tmp/1.apk  
> adb push 2.apk /data/local/tmp/2.apk  
> adb push 3.apk /data/local/tmp/3.apk  
> adb shell  
> pm install-create  
> *返回的会话ID替换下面数字*  
> pm install-write 1234567890 1.apk /data/local/tmp/1.apk  
> pm install-write 1234567890 2.apk /data/local/tmp/2.apk  
> pm install-write 1234567890 3.apk /data/local/tmp/3.apk  
> pm install-commit 1234567890

安卓已安装应用目录结构:
/data/app/com.example.myapp/
├── base.apk
├── lib/
│   └── armeabi-v7a/
│       ├── libmyapp.so
├── res/
│   ├── drawable/
│   │   ├── ic_launcher.png
│   │   └── my_icon.png
│   ├── layout/
│   │   ├── activity_main.xml
│   │   └── main_menu.xml
│   ├── values/
│   │   ├── strings.xml
│   │   └── styles.xml
│   └── xml/
│       └── settings.xml
├── assets/
│   ├── images/
│   │   └── my_image.jpg
│   └── sounds/
│       └── my_sound.mp3
├── classes.dex
├── libEGL.so
├── libGLESv2.so
├── META-INF/
│   ├── CERT.RSA
│   ├── MANIFEST.MF
│   └── SDK-VERSION
└── uninstall.sh

安卓love2d应用数据结构（除了应用名文件夹外全是空的）
/data/data/com.example.myapp/
├── cache
├── code_cache
├── databases
├── no_backup
├── shared_prefs
├── files
│   ├── game
│   ├── save/
│   │   ├── ASET
│   │   ├── Techmino/
│   │   │   ├── replay
│   │   │   ├── record
│   │   │   ├── ...

读/写系统设置:
> adb shell settings get/put system/global KEY (VALUE)  
> 休眠时间 adb shell settings put system screen_off_timeout 36000000  
> 屏幕亮度 adb shell settings put system screen_brightness 255  
> 动画速度 adb shell settings put global window_animation_scale 10  
> 动画速度 adb shell settings put global transition_animation_scale 10  
> 动画速度 adb shell settings put global animator_duration_scale 10

## LIBINPUT

列出设备：
> sudo libinput list-devices | grep "Device:"
> event12 触摸板
> event14 鼠标
> event4 无线键盘

全追踪：
> sudo libinput debug-events

追踪鼠标：
> sudo libinput debug-events --compress-motion-events --device "/dev/input/event14"
> sudo libinput debug-events --device "/dev/input/event14"

追踪键盘：
> sudo libinput debug-events --show-keycodes --device "/dev/input/event4"
