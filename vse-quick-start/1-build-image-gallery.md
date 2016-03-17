## Lesson 1. Build a photo album

### Step 1. Add Kendo UI Mobile widgets

Kendo UI Mobile apps are made up of views, and a basic one has already been created for you (`<div data-role="view">`). To liven up this view, you'll start by adding a Kendo UI header and NavBar widget to display the view's title.

<hr data-action="start" />

#### Action

* **a**. Open this project's index.html file.
* **b**. Find the defined view, and add the following markup:
```
<div data-role="view">
    <header data-role="header">
        <div data-role="navbar">
            <span data-role="view-title"></span>
        </div>
    </header>
</div>
```

* **c**. Add a `data-title` attribute to the view, e.g. `<div data-role="view" data-title="Photos">` and save the changes.
* **d**. Save the index.html file.

<hr data-action="end" />

Kendo UI Mobile automatically styles the navbar to look great on each platform. To see what this looks like, let's try this app out in the AppBuilder simulator by the **Start Debugging** command (default shortcut F5).

<hr data-action="start" />

#### Action

* **e**. With the simulator open, change the title of the view by updating its `data-title` attribute—for instance `<div data-role="view" data-title="My Cool Photos">`.
* **f**. Save your changes and note how the simulator updates automatically.

<hr data-action="end" />

**Tip**: The simulators automatically update with every change you make. Take a minute to experiment with the various controls the simulator offers. You can simulate multiple devices, rotate devices to test portrait & landscape modes, and a lot more.

### Step 2. Show data in a listview

The next step is to show some photos in the album, using the Kendo UI ListView widget for the purpose. This widget automatically handles complex use cases such as endless scrolling and pull-to-refresh, but for now you'll start by displaying a static list of images.

<hr data-action="start" />

#### Action

* **a**. Add a `<ul>` element underneath the navbar with an `id` of `"images"` (`<ul id="images"></ul>`) and save changes. The completed view should look like this:
```
<div data-role="view" data-title="Photos">
    <header data-role="header">
        <div data-role="navbar">
            <span data-role="view-title"></span>
        </div>
    </header>
    <ul id="images"></ul>
</div>
```
* **b**. Open the project's app.js file, located in the scripts folder to add code to select the `<ul>` with jQuery and convert it into a ListView widget. 
* **c**. Place this code directly *after* the `kendo.mobile.Application()` call, as the Kendo UI Mobile app needs to exist before you initialize widgets.
```
$("#images").kendoMobileListView({
    dataSource: ["images/01.jpg", "images/02.jpg", "images/03.jpg", "images/04.jpg", "images/05.jpg", "images/06.jpg", "images/07.jpg"],
    template: "<img src='#: data #'>"
});
```
* **e**. Run this in the simulator to see how the photos display.

<hr data-action="end" />

### Step 3. Deploy your app using the companion app

The AppBuilder companion app makes it easy to test your app on real devices, without the need to manage SDKs or deal with complex provisioning options.

<hr data-action="start" />

#### Action

* **a.** Get your iOS, Android, or Windows Phone device out.
* **b**. Download and install the AppBuilder companion app from your device's app store.

[![iOS app store](images/app-store-icon.png)](https://itunes.apple.com/us/app/telerik-appbuilder/id527547398?mt=8)
[![Google Play](images/google-play-icon.png)](https://play.google.com/store/apps/details?id=com.telerik.AppBuilder&hl=en)
[![Windows Phone Store](images/windows-phone-store-icon.png)](https://www.windowsphone.com/en-us/store/app/appbuilder/0171d46b-b5f2-43d9-a36b-0a78c9692aab?signin=true)

* **c**. From the VS toolbar select the APPBUILDER menu and then the **Build PhotoAlbum in Cloud** option. Having the Build wizard opened, select your device's platform (iOS/Android/Windows Phone), choose "AppBuilder companion app", and click Next.
* **d**. Open the AppBuilder app on your device, and then use a two-finger swipe to reveal the companion app's menu. Click the "QR Scanner" option in the menu and use the integrated scanner to scan the QR code displayed in the browser.
![Using a two-finger swipe on your device](images/swipe.png)

<hr data-action="end" />

When scanned, the QR code loads your image gallery app in the AppBuilder companion app. Now that you have the app on your device, let's make some changes.

<hr data-action="start" />

#### Action

* **e**. Change the `data-title` attribute of the app's view (for instance `<div data-role="view" data-title="My Awesome Photos">`) and save changes.
* **f.** From the VS toolbar select the APPBUILDER menu and then the **Synchronize PhotoAlbum with Cloud** option.
* **g.** Check the Output window where output from Build is shown and wait until the "Project files uploaded to the cloud" message is logged.
* **h**. On your device, within the companion app, tap with three fingers and hold until a popup appears.
![Using a three-finger refresh on your device](images/three-finger-tap.png)

<hr data-action="end" />

This process is known as *LiveSync*, and it makes updating your apps as easy as a quick tap. Now that you have a functioning app, and can test it on your device, let's see how to use the device's camera.

### Step 4. Deploy your app on connected device.

With AppBuilder, you can directly deploy on multiple connected devices simultaneously. You can deploy apps on devices running iOS 4.3.5 and later, Android 2.2 and later, Windows Phone or on the Kindle Fire.

<hr data-action="start" />

#### Action

* **a.** Connect a device via USB cable and ensure it is recognized by the system.
* **b**. Verify that [the AppBuilder Prerequisites](http://docs.telerik.com/platform/appbuilder/cordova/running-on-devices/running-on-connected-devices/deploy-connected#appbuilder-prerequisites) are respected.
* **c**. In the VS top menu bar, click **APPBUILDER** → **Devices List** and verify that AppBuilder lists your connected devices.
* **d**. Click **APPBUILDER** → **Build PhotoAlbum and Deploy**.

<hr data-action="end" />

**Tip**: When you build for a connected iOS device, the extension for Visual Studio attempts to build your app with an applicable pair of provisioning profile and cryptographic identity for iOS development. See [iOS Prerequisites](http://docs.telerik.com/platform/appbuilder/cordova/running-on-devices/running-on-connected-devices/deploy-connected#ios-prerequisites).

<hr data-action="start" />

#### Action

* **e**. Wait until the app is deployed on the device. If it does not launch automatically, tap the app icon to run it.
* **f.** Change the `data-title` attribute of the app's view, for example to "My awesome VS Photos" and save your changes.
* **g.** Notice that the changes will be applied automatically.

<hr data-action="end" />

**Tip**: If you tap with three fingers and hold until a popup appears, then the changes will be live synced with the latest version synchronized with the cloud and all syncs applied over the cable will be lost. To upload the latest changes to the cloud, select the APPBUILDER menu and then the **Synchronize PhotoAlbum with Cloud** option.

**Tip**: You cannot LiveSync changes to connected Windows Phone devices. To be able to LiveSync changes, deploy your app via QR code and use the three-finger refresh gesture to get your latest changes from the cloud. See [Windows Phone Prerequisites](http://docs.telerik.com/platform/appbuilder/cordova/running-on-devices/running-on-connected-devices/deploy-connected#windows-phone-prerequisites).
For the purpose of this tutorial, we will illustrate testing on Android device since it does not require any additional provisioning.