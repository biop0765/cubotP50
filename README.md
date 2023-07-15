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

## Backup, resize, edit, and flash system.img

```
# adb shell "ls -Alg /dev/block/by-name | grep 'super'"
## did not use this # adb shell su -c dd if=/dev/block/mmcblk0p35 of=/storage/emulated/0/P50-mmcblk0p35.img
# adb shell su -c dd if=/dev/block/dm-2 of=/storage/emulated/0/P50-dm-2.img
# adb pull /storage/emulated/0/P50-dm-2.img ./
```
- Get the size of img
```
# du -sk P50-dm-2.img system-AOSP11-arm64-ab-vanilla.img 
1242320	P50-dm-2.img
1540172	system-AOSP11-arm64-ab-vanilla.img #as a comparison
```
- Run fsck and then resize
```
# e2fsck -f P50-dm-2.img
# resize2fs P50-dm-2.img 1342320K
```
- Mount and edit `/mnt/1/system/etc/hosts`
```
# mount  P50-dm-2.img /mnt/1
```
- After edits set proper selinux contexts
```
# In the mounted image inside linux x86_64
apt-get install attr
ls -Z /mnt/1/system/etc/hosts
setfattr -n security.selinux -v u:object_r:system_file:s0 /mnt/1/system/etc/hosts
umount /mnt/1
```
- Reboot and flash system
```
# adb reboot bootloader
# fastboot reboot fastboot
# fastboot flash system  P50-dm-2.img
```

## GSI

- Download A11 AOSP GSI from https://github.com/phhusson/treble_experimentations or use `system-230628-treble_arm64_bvN-userdebug-roar-arm64-ab-vanilla.img.xz`

  
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


# limitations

- Bluetooth does not find devices
- Headphone jack (3.5mm) works after enabling `android:key="key_misc_headset_devinput"` in https://github.com/phhusson/treble_app
