## Lesson 4. Test the app on a real mobile device

The AppBuilder device simulator and Companion Apps are great, but nothing substitutes for testing your app on a real device. In this lesson you'll learn how to configure a device, create a build of your app for iOS/Android/Windows Phone - and deploy the app to your device.

The first step in this lesson is to do a one-time configuration of your device to make sure you can easily install apps yourself. Please follow the step appropriate to your device.

### Step 10. **Android:** Learn about installing apps from unknown sources

Before you may manually deploy apps to your Android device, you have to make sure that you're able to install apps from unknown sources. This Android security setting allows you to deploy apps to your device outside of the Google Play store.

#### Action

<hr data-action="start" />

* On Android 3.x and earlier devices, go to `Settings -> Applications` and verify that `Unknown sources` is selected.

* On Android 4.x devices, go to `Settings -> Security -> Device administration` and verify that `Unknown sources` is selected.

<hr data-action="end" />

### Step 11. **iOS:** Learn about provisioning profiles

If you are planning on deploying to an iOS device, you need to know a little bit about device provisioning. Apple is restrictive when it comes to installing apps that don't come from their app store (even for testing purposes). In order to deploy an iOS app, you need to assign one or more device ids to a "provisioning profile" that is configured on Apple's developer web site. Once your provisioning profile is created, you then merge that profile with your app when creating a build in AppBuilder. Any devices registered in that profile will be able to install and run your app.

The detailed instructions for how to do this are outside the scope of this tutorial (because it's a rather lengthy process). However, we do have resources for you, such as a [step-by-step video](https://www.youtube.com/watch?v=Y1umDPO4Ly4) and [extensive information in our docs](http://docs.telerik.com/platform/appbuilder/code-signing-your-app/code-signing).

### Step 12. **Windows Phone:** Learn about registering your device for development

Microsoft requires that all Windows Phone 8/8.1 devices be registered as "development devices" before you may manually deploy an app to them.

#### Action

<hr data-action="start" />

* **a**. Download and install the [Windows Phone SDK 8.0](http://dev.windows.com/en-us/develop/download-phone-sdk).

* **b**. Make sure your phone is unlocked and that the date and time are correct. Connect your phone to your computer.

* **c**. Search on your computer for **Windows Phone Developer Registration** and start the application.

* **d**. Click the **Register** button to unlock your phone. Sign in with your Microsoft account and your phone should be registered as a development device.

<hr data-action="end" />

### Step 13. Deploy your app to an iOS, Android, or Windows Phone device

Now that you've learned how to properly set up your device, let's create a build and deploy it!

<hr data-action="start" />

#### Action

* **a**. Click the `Run` menu and choose the `Build` option.

* **b**. Choose the platform you are targeting and make sure the `App package` option is selected.

> Tip: You'll notice that we are leaving the `Configure LiveSync` option "on". By enabling LiveSync we can view changes made to our app immediately on our device (even after it's been deployed!).

* **c**. Click the `Next` button to generate your build in the cloud. Note that this build process may take a minute.

* **d**. **Android:** Either scan the provided code with a QR code reader, type the provided URL into your Android browser, or download and manually install the app on your device. Any of these will let you install the app on your Android device.

* **e**. **iOS:** Before the build commences, you must choose the iOS provisioning profile and certificate you created already after reading step 11 above. Click `Next` to generate your build, and when it's done, download your app. Connect your device to your computer, double-click the downloaded IPA file to add it to iTunes, select the app in iTunes, and click `Install`.

* **f**. **Windows Phone:** Either scan the provided code with a QR code reader or type the provided URL into IE on your device. Either of these will let you install the app on your Windows Phone device.

* **g**. Run your app on your device. At this point you should be able to run and view your app running natively on your device!

<hr data-action="end" />

> Tip: If you use another AppBuilder IDE such as our Windows client, Visual Studio extension, Command Line Interface, or Sublime Text package, you can deploy directly to any connected device without going through the process of scanning QR codes or downloading and manually installing the app!*

### Step 14. Use LiveSync to see your changes without re-building

If you had to go through all of these steps each time you built a new version of your app, that might get a little painful. Luckily with LiveSync enabled in your app, all you have to do is a three finger tap in your app to trigger a download of the latest version. No re-building and re-deploying necessary!

#### Action

<hr data-action="start" />

* **a**. Back in AppBuilder, open up the `main.css` file, found in your `styles` directory.

* **b**. Paste in the following CSS code:

```
body {
    background-color: Red;
}
```

* **c**. Save the file. Go back to your device and hold down three fingers to initiate a LiveSync refresh.

* **d**. This will download the latest version of your app and you should see the new background color applied to your app (all without re-building it!).

<hr data-action="end" />

Congratulations - you built an app from scratch that utilizes four different Cordova plugins to add native functionality to your hybrid mobile app!