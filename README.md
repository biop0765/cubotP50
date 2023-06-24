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
adb shell su -c rm -fr /system/app/AudioRecorderStoneoim
adb shell su -c rm -f /product/app/YouTube/YouTube.apk
adb shell su -c rm -f /product/priv-app/Velvet/Velvet.apk
adb shell su -c rm -f /product/app/AssistantShell/AssistantShell.apk
adb shell su -c rm -f /system/system_ext/priv-app/GoogleOneTimeInitializer/GoogleOneTimeInitializer.apk
#adb shell su -c rm -f /product/app/Messages/Messages.apk
adb shell su -c rm -f /product/priv-app/ConfigUpdater/ConfigUpdater.apk
adb shell su -c rm -f /product/priv-app/Phonesky/Phonesky.apk
adb shell su -c rm -f /product/app/talkback/talkback.apk
adb shell su -c rm -f /product/app/Gmail2/Gmail2.apk
adb shell su -c rm -f /product/app/Duo/Duo.apk
#adb shell su -c rm -f /system/system_ext/priv-app/SetupWizard/SetupWizard.apk
adb shell su -c rm -f /product/priv-app/Wellbeing/Wellbeing.apk
#adb shell su -c rm -f /product/priv-app/GoogleDialer/GoogleDialer.apk
adb shell su -c rm -f /product/priv-app/FilesGoogle/FilesGoogle.apk
adb shell su -c rm -f /product/app/Drive/Drive.apk
adb shell su -c rm -f /product/app/Maps/Maps.apk
#adb shell su -c rm -f /product/app/GoogleContacts/GoogleContacts.apk
#adb shell su -c rm -f /product/app/GoogleContactsSyncAdapter/GoogleContactsSyncAdapter.apk
adb shell su -c rm -f /product/priv-app/GmsCore/GmsCore.apk
adb shell su -c rm -f /product/app/SpeechServicesByGoogle/SpeechServicesByGoogle.apk
adb shell su -c rm -f /product/app/GmsEEAType4cIntegration/GmsEEAType4cIntegration.apk
adb shell su -c rm -f /product/priv-app/GooglePartnerSetup/GooglePartnerSetup.apk
adb shell su -c rm -f /product/app/Videos/Videos.apk
adb shell su -c rm -f /system/app/GooglePrintRecommendationService/GooglePrintRecommendationService.apk
adb shell su -c rm -f /product/app/Photos/Photos.apk
adb shell su -c rm -f /product/app/CalendarGoogle/CalendarGoogle.apk
adb shell su -c rm -f /product/priv-app/AndroidAutoStub/AndroidAutoStub.apk
adb shell su -c rm -f /product/priv-app/SearchSelector/SearchSelector.apk
adb shell su -c rm -f /product/app/GoogleLocationHistory/GoogleLocationHistory.apk
adb shell su -c rm -f /product/app/YTMusic/YTMusic.apk
adb shell su -c rm -f /system/system_ext/priv-app/GmsSimProcessor/GmsSimProcessor.apk
adb shell su -c rm -f /system/priv-app/Stk1/Stk1.apk
```
