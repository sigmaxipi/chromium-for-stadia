# chromium-for-stadia
This is a quick hack to get [Google Stadia](https://stadia.google.com/) running on unsupported AOSP devices like the [Oculus Quest](https://www.oculus.com/quest/).

The `ChromiumForStadia.apk` was created using the following steps:

1. A stock version of Chromium for Android was built using the [official guide](https://chromium.googlesource.com/chromium/src/+/master/docs/android_build_instructions.md).
1. The changes in `chromiumForStadia.diff` were made to the stock source. The core changes were:
   * Modifying `content/common/user_agent.cc` to make the app always pretend to run in Desktop Mode. This avoids the need for switching to Desktop Mode each time.
   * Modifying `device/gamepad/gamepad_platform_data_fetcher_android.c` to mark any controller's `mapping` as `standard`. Without this, Stadia doesn't detect the controller.
   * Modifying `chrome/android/java/AndroidManifest.xml`. This allows the app to show up on [Android TV devices](https://developer.android.com/training/tv/start/start).

With these changes, going to [stadia.google.com](https://stadia.google.com) in this app will allow you to stream Stadia to the browser on Android devices. You can also use unsupported USB & Bluetooth controllers with the browser. You can test your gamepad at [html5gamepad.com/](https://html5gamepad.com/).

This app was tested on a Pixel 4 & Oculus Quest using an [ASUS Bluetooth Gamepad](https://www.asus.com/us/Home-Entertainment/Gamepad-TV500BG/) and the official Stadia controller via USB C.

Note that this app is not officially supported by Google nor Oculus and may unexpectedly break at any point.
[This reddit post](https://www.reddit.com/r/Stadia/comments/e11897/how_to_get_stadia_running_in_android_chrome_on/) has some more notes on how to get Stadia working in the browser without using a custom app. The app was just meant to streamline those instructions.