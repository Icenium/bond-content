## Lesson 4. Debug on Device

AppBuilder supports debuging on devices running iOS or Android 4.4 and later while for devices, running Android 4.3 or earlier, you need to add the jsHybugger custom plugin. There are two options to debug applications in Visual Studio – with the **AppBuilder debug tools** or **Visual Studio debug tools**. For the purpose of this tutorial let’s try this on Android 4.3 or later.

### Step 10. Debug on Android devices with the Microsoft Visual Studio debug tools

<hr data-action="start" />

#### Action

* **a**. In the code editor, open the JavaScript file that you want to debug and set a breakpoint.
* **b**. In the **Standard** toolbar, from the **Start Debugging** drop-down menu, select your device to **Start Debugging**.
* **c**. Debug your app with the built-in debugger.

<hr data-action="end" />

**Tip**: For further reference, you can refer to [Debugging on Device](http://docs.telerik.com/platform/appbuilder/cordova/debugging-your-code/debugging-on-device/debugging-on-device) section.

For more information, see [How Do I... Topics: Debugger](https://msdn.microsoft.com/en-us/library/aa315451%28v=vs.60%29.aspx).

### Step 11. Debug on device with the AppBuilder debug tools

<hr data-action="start" />

#### Action

* **a**. Verify that you have connected your Android device and the extension for Visual Studio recognizes it (it is listed when selecting **AppBuilder** → **Devices List**.

**Tip**: For more information, see [Connect Android Devices](http://docs.telerik.com/platform/appbuilder/cordova/running-on-devices/running-on-connected-devices/google-android-devices/connect-android#visual-studio) (or [Connect iOS Devices](http://docs.telerik.com/platform/appbuilder/cordova/running-on-devices/running-on-connected-devices/connecting-ios-devices/connect-ios-devices#visual-studio)). Verify that you have applied the Debug build configuration.

* **b**. In the main menu bar in Microsoft Visual Studio, select **AppBuilder** → **Build <app_name> and Deploy** and wait for AppBuilder to install the application on your device.
* **c**. On your device, run the application.
* **d**. In the main menu bar in Microsoft Visual Studio, select **AppBuilder** → **Devices List**.
* **e**. Select your device and click **Debug**.
* **f**. Select your app from the list of debuggable apps found on the device. AppBuilder lists all apps that are enabled for debugging and are currently running in the foreground or suspended in the background.
* **g**. Click **Attach** to attach the debug tools to the device.

<hr data-action="end" />

When using the **AppBuilder debug tools**, you will have to restart your app on the device after each debugging session as there is a known issue with the tools sometimes not properly loading.
