# chromium-for-stadia
This is a quick hack to get [Google Stadia](https://stadia.google.com/) running on unsupported AOSP devices like the [Oculus Quest](https://www.oculus.com/quest/).

The `ChromiumForStadia.apk` was created using the following steps:

1. A stock version of Chromium for Android was built using the [official guide](https://chromium.googlesource.com/chromium/src/+/master/docs/android_build_instructions.md) @ 3333b9d62d44b68615a6f40c429d7f93cbc71781 from 4/13/2020.
1. The changes in `chromiumForStadia.diff` were made to the stock source. The core changes are:
   * Modifying `chrome/android/java/AndroidManifest.xml`. This allows the app to show up on [Android TV devices](https://developer.android.com/training/tv/start/start).
   * Modifying `FullscreenHtmlApiHandler.applyEnterFullscreenUIFlags`. This fakes `{navigationUI: "hide"}` and avoids https://bugs.chromium.org/p/chromium/issues/detail?id=933436#c7.
   * Modifying `device/gamepad/gamepad_platform_data_fetcher_android.c` to mark any controller's `mapping` as `standard`. Without this, Stadia doesn't detect the controller.
   * Modifying `third_party/blink/renderer/core/dom/document_or_shadow_root.h` to perform the equivalent of `document.pointerLockElement = document.fullscreenElement`. This hack gets around the fact that the [Pointer Lock API](https://developer.mozilla.org/en-US/docs/Web/API/Pointer_Lock_API) doesn't work as Stadia expects on mobile devices.

With these changes, going to [stadia.google.com](https://stadia.google.com) in this app and enabling **Desktop Mode** will allow you to stream Stadia to the browser on Android devices. You can also use unsupported USB & Bluetooth controllers with the browser. You can test your gamepad at [html5gamepad.com/](https://html5gamepad.com/).

This app was tested on a Pixel 4 & Oculus Quest using an [ASUS Bluetooth Gamepad](https://www.asus.com/us/Home-Entertainment/Gamepad-TV500BG/), PS4 Dualshock, X-Box One controller, and the official Stadia controller via USB C.

Note that this app is not officially supported by Google nor Oculus and may unexpectedly break at any point.
[This reddit post](https://www.reddit.com/r/Stadia/comments/e11897/how_to_get_stadia_running_in_android_chrome_on/) has some more notes on how to get Stadia working in the browser without using a custom app. The app was just meant to streamline those instructions.

## Release notes

*  4/19/2020 - Pull in upstream changes. Fixes XBox controller support and improves fullscreen UI.
*  1/ 4/2020 - Pull in upstream changes. Remove localstorage hack. Remove controller mapping hack since the changes were merged upstream.
* 12/ 9/2019 - Improve support for PS4 controllers on older OSes.
* 12/ 4/2019 - Remove user-agent spoofing since this was causing a login issue for some people: https://github.com/sigmaxipi/chromium-for-stadia/issues/4
* 12/ 3/2019 - Added support for PS4 Dualshock controllers by updating the mapping to work around https://github.com/sigmaxipi/chromium-for-stadia/issues/5
* 12/ 2/2019 - Delete standalone 32-bit apk and make the main apk 32-bit. Workaround infinite loading issue by spoofing localStorage write. 
* 12/ 1/2019 - Fixed [an issue](https://www.reddit.com/r/Stadia/comments/e18a9s/instructions_for_running_google_stadia_on_the/f8tknkq/?context=3) where holding the trigger buttons on the Stadia controller causes a pop up due to loss of pointerlock.
