### cubot P50

This is a  dummy repo for Cubot P50

## Magisk and unlock

- Download and install magisk
- Default settings

```
fastboot flashing unlock
fastboot flash boot magisk_boot.img
fastboot flash vbmeta --disable-verity --disable-verification vbmeta.img.empty
```

- reboot and setup device. If needed run

```
adb shell settings put global setup_wizard_has_run 1
adb shell settings put secure user_setup_complete 1
adb shell settings put global device_provisioned 1
```

- To remove orange state message use ghex to edit lk.img and

```
fastboot flash lk lk_edit.img
```

## Remove statusbar icons

```
adb shell settings put secure icon_blacklist rotate,volume,do_not_disturb,vibrate,headset,location
```

## Remount,rw

```
adb shell su -c mount -o rw,remount /
adb shell su -c mount -o rw,remount /product
```

## SMS
- disable `com.google.android.ims` to get SMS

```
adb shell su -c rm -f /product/priv-app/CarrierServices/CarrierServices.apk
```

## others

```
#!/bin/bash

adb shell su -c mount -o rw,remount /
adb shell su -c mount -o rw,remount /product

adb shell su -c rm -fr /system/system_ext/priv-app/DuraSpeed
adb shell su -c rm -fr /system/app/AudioRecorderStoneoim
adb shell su -c rm -fr /product/app/YouTube
adb shell su -c rm -fr /product/priv-app/Velvet
adb shell su -c rm -fr /product/app/AssistantShell
adb shell su -c rm -fr /system/system_ext/priv-app/GoogleOneTimeInitializer
#adb shell su -c rm -fr /product/app/Messages/Messages.apk
adb shell su -c rm -fr /product/priv-app/ConfigUpdater
adb shell su -c rm -fr /product/priv-app/Phonesky
adb shell su -c rm -fr /product/app/talkback
adb shell su -c rm -fr /product/app/Gmail2
adb shell su -c rm -fr /product/app/Duo
#adb shell su -c rm -fr /system/system_ext/priv-app/SetupWizard/SetupWizard.apk
adb shell su -c rm -fr /product/priv-app/Wellbeing
#adb shell su -c rm -fr /product/priv-app/GoogleDialer/GoogleDialer.apk
adb shell su -c rm -fr /product/priv-app/FilesGoogle
adb shell su -c rm -fr /product/app/Drive
adb shell su -c rm -fr /product/app/Maps
#adb shell su -c rm -fr /product/app/GoogleContacts/GoogleContacts.apk
#adb shell su -c rm -fr /product/app/GoogleContactsSyncAdapter/GoogleContactsSyncAdapter.apk
adb shell su -c rm -fr /product/priv-app/GmsCore
adb shell su -c rm -fr /product/app/SpeechServicesByGoogle
adb shell su -c rm -fr /product/app/GmsEEAType4cIntegration
adb shell su -c rm -fr /product/priv-app/GooglePartnerSetup
adb shell su -c rm -fr /product/app/Videos
adb shell su -c rm -fr /system/app/GooglePrintRecommendationService
adb shell su -c rm -fr /product/app/Photos
adb shell su -c rm -fr /product/app/CalendarGoogle
adb shell su -c rm -fr /product/priv-app/AndroidAutoStub
adb shell su -c rm -fr /product/priv-app/SearchSelector
#adb shell su -c rm -fr /product/app/GoogleLocationHistory
adb shell su -c rm -fr /product/app/YTMusic
adb shell su -c rm -fr /system/system_ext/priv-app/GmsSimProcessor
adb shell su -c rm -fr /system/priv-app/Stk1
```

## Gallery

- Unzip and dump Gallery2.tar as follows

```
P50:/ # ls -alR /product/priv-app/Gallery2                                                                                                       
/product/priv-app/Gallery2:
total 13716
drwxr-xr-x  3 root root     4096 2023-06-22 17:35 .
drwxr-xr-x 16 root root     4096 2023-06-24 12:03 ..
-rwxr-xr-x  1 root root 14030002 2023-06-22 17:35 Gallery2.apk
drwxr-xr-x  3 root root     4096 2023-06-22 17:35 lib

/product/priv-app/Gallery2/lib:
total 12
drwxr-xr-x 3 root root 4096 2023-06-22 17:35 .
drwxr-xr-x 3 root root 4096 2023-06-22 17:35 ..
drwxr-xr-x 2 root root 4096 2023-06-22 17:35 arm64

/product/priv-app/Gallery2/lib/arm64:
total 396
drwxr-xr-x 2 root root   4096 2023-06-22 17:35 .
drwxr-xr-x 3 root root   4096 2023-06-22 17:35 ..
-rwxr-xr-x 1 root root  11280 2023-06-22 17:35 libjni_gallery_eglfence.so
-rwxr-xr-x 1 root root  40168 2023-06-22 17:35 libjni_gallery_filters.so
-rwxr-xr-x 1 root root 342392 2023-06-22 17:35 libjni_gallery_jpegstream.so
```


## Backup

```
adb shell "ls -Alg /dev/block/by-name | grep 'super'"
adb shell su -c dd if=/dev/block/mmcblk0p35 of=/storage/emulated/0/P50-mmcblk0p35.img
adb shell su -c dd if=/dev/block/dm-2 of=/storage/emulated/0/P50-dm-2.img
```

## GSI

- Download A11 AOSP GSI from https://github.com/phhusson/treble_experimentations
  
```
adb reboot bootloader
fastboot reboot fastboot
fastboot flash system system-roar-arm64-ab-vanilla.img #A11
# Without fail factory reset from stock recovery
```
- For AOSP 11 one needs to Phh Treble settings > Misc features > Use alternate way to detect headsets to enabled
- Install Camera from GrapheneOS project
- Everything just works fine

- Notes: If fastboot tells you there isn't enough place, do

```
fastboot delete-logical-partition product
fastboot delete-logical-partition product_a
fastboot delete-logical-partition product_b
then
fastboot flash system system-roar-arm64-ab-vanilla.img #A11
```
