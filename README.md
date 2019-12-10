# chromium-for-stadia
This is a quick hack to get [Google Stadia](https://stadia.google.com/) running on unsupported AOSP devices like the [Oculus Quest](https://www.oculus.com/quest/).

The `ChromiumForStadia.apk` was created using the following steps:

1. A stock version of Chromium for Android was built using the [official guide](https://chromium.googlesource.com/chromium/src/+/master/docs/android_build_instructions.md).
1. The changes in `chromiumForStadia.diff` were made to the stock source. The core changes are:
   * Modifying `device/gamepad/gamepad_platform_data_fetcher_android.c` to mark any controller's `mapping` as `standard`. Without this, Stadia doesn't detect the controller.
   * Modifying `chrome/android/java/AndroidManifest.xml`. This allows the app to show up on [Android TV devices](https://developer.android.com/training/tv/start/start).
   * Modifying `third_party/blink/renderer/core/dom/document_or_shadow_root.h` to perform the equivalent of `document.pointerLockElement = document.fullscreenElement`. This hack gets around the fact that the [Pointer Lock API](https://developer.mozilla.org/en-US/docs/Web/API/Pointer_Lock_API) doesn't work as Stadia expects on mobile devices.
   * Modifying `third_party/blink/renderer/modules/storage/storage_area.cc` to spoof setting a problematic key related to video codec. This was caused by a bug in the Chromium codebase between Nov 24 and Dec 1, and this hack should be reverted at some point.
   * Modifying `device/gamepad/android/java/src/org/chromium/device/gamepad/GamepadMappings.java` to properly map PS4 controllers. https://github.com/sigmaxipi/chromium-for-stadia/issues/5

With these changes, going to [stadia.google.com](https://stadia.google.com) in this app and enabling **Desktop Mode** will allow you to stream Stadia to the browser on Android devices. You can also use unsupported USB & Bluetooth controllers with the browser. You can test your gamepad at [html5gamepad.com/](https://html5gamepad.com/).

This app was tested on a Pixel 4 & Oculus Quest using an [ASUS Bluetooth Gamepad](https://www.asus.com/us/Home-Entertainment/Gamepad-TV500BG/), PS4 Dualshock, and the official Stadia controller via USB C.

Note that this app is not officially supported by Google nor Oculus and may unexpectedly break at any point.
[This reddit post](https://www.reddit.com/r/Stadia/comments/e11897/how_to_get_stadia_running_in_android_chrome_on/) has some more notes on how to get Stadia working in the browser without using a custom app. The app was just meant to streamline those instructions.

## Release notes

* 12/9/2019 - Improve support for PS4 controllers on older OSes.
* 12/4/2019 - Remove user-agent spoofing since this was causing a login issue for some people: https://github.com/sigmaxipi/chromium-for-stadia/issues/4
* 12/3/2019 - Added support for PS4 Dualshock controllers by updating the mapping to work around https://github.com/sigmaxipi/chromium-for-stadia/issues/5
* 12/2/2019 - Delete standalone 32-bit apk and make the main apk 32-bit. Workaround infinite loading issue by spoofing localStorage write. 
* 12/1/2019 - Fixed [an issue](https://www.reddit.com/r/Stadia/comments/e18a9s/instructions_for_running_google_stadia_on_the/f8tknkq/?context=3) where holding the trigger buttons on the Stadia controller causes a pop up due to loss of pointerlock.

## Known issues

* https://github.com/sigmaxipi/chromium-for-stadia/issues/5 PS4 controllers may not work on all devices. https://html5gamepad.com/ can be used to check gamepad button mapping. An [older version of the apk](https://github.com/sigmaxipi/chromium-for-stadia/commits/master/ChromiumForStadia.apk) may help with pre-Android 10 devices.