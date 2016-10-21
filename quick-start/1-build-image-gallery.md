## Lesson 1. Build a photo album

### Step 1. Add a navbar

Kendo UI Mobile apps are made up of views, and a basic one has already been created for you (`<div data-role="view">`). To liven up this view, you'll start by adding a Kendo UI header and NavBar widget to display the view's title.

<hr data-action="start" />

#### Action

* **a**. Open this project’s index.html file by double clicking it in the Project Navigator on the right-hand side of the screen.
* **b**. Find the defined view, and add the following markup inside it:
```
<header data-role="header">
    <div data-role="navbar">
        <span data-role="view-title"></span>
    </div>
</header>
```

**Tip**: The keyboard shortcut `Ctrl` + `Alt` + `F` cleans up indentation and formatting for you. Try using it after you paste in code throughout these lessons.

* **c**. Add a `data-title` attribute to the view, e.g. `<div data-role="view" data-title="Photos">`.
* **d**. Save the index.html file.

<hr data-action="end" />

Kendo UI Mobile automatically styles the navbar to look great on each platform. To see what this looks like, let’s try this app out in the AppBuilder simulators.

<hr data-action="start" />

#### Action

* **e**. Open the iPhone simulator (select **Run** --> **Open Simulator**) and then select any iPhone from the **iOS Devices** section in the drop-down menu located in the top-left corner.
* **f**. With the simulator open, change the title of the view by updating its `data-title` attribute—for instance `<div data-role="view" data-title="My Cool Photos">`.
* **g**. Save your index.html file and note how the simulator updates automatically.

<hr data-action="end" />

The simulators automatically update with every change you make. Take a minute to experiment with the various controls the simulator offers. You can simulate multiple devices, rotate devices to test portrait & landscape modes, and a lot more.

### Step 2. Show data in a listview

Your next step is to show some photos in your album, and you'll use the Kendo UI ListView widget to do that. The ListView widget automatically handles complex use cases such as endless scrolling and pull-to-refresh, but for now you'll start by displaying a static list of images.

<hr data-action="start" />

#### Action

* **a**. Add a `<ul>` element with an `id` of `"images"` (`<ul id="images"></ul>`) underneath the header. The view should look like this:
```
<div data-role="view" data-title="My Cool Photos">
    <header data-role="header">
        <div data-role="navbar">
            <span data-role="view-title"></span>
        </div>
    </header>

    <ul id="images"></ul>
</div>
```
* **b**. Save the index.html file.
* **c**. Open the project's app.js file, located in the scripts folder.
* **d**. In your app.js, use the code below to select the `<ul>` with jQuery and convert it into a ListView widget. Place this code directly *after* the `kendo.mobile.Application()` call, as the Kendo UI Mobile app needs to exist before you initialize widgets.
```
$("#images").kendoMobileListView({
    dataSource: ["images/01.jpg", "images/02.jpg", "images/03.jpg", "images/04.jpg", "images/05.jpg", "images/06.jpg", "images/07.jpg"],
    template: "<img src='#: data #'>"
});
```
* **e**. Run this in the simulator to see how the photos display.

**Tip**: In case at some point you see a white screen, most likely a JavaScript exception occurred. Please open the developer console (**F12** key for Windows and **Command + Option + I** for OS X) and check for any errors listed under the Console tab. See also [Debug in the Simulator](http://docs.telerik.com/platform/appbuilder/cordova/debugging-your-code/debug-in-simulator).

<hr data-action="end" />

### Step 3. Deploy your app using the Companion app

The Telerik Platform Companion app is a dashboard app that lists the applications on your account and lets you scan QR codes. The Companion app utilizes player apps to run your applications in a sandbox environment. As such, you’ll use the Companion app to load apps on your account, and the player app to run your apps on real devices, without the need to manage SDKs or deal with complex provisioning options.

<hr data-action="start" />

#### Action

* **a**. Get your iOS, Android, or Windows Phone device.
* **b**. Download and install the Telerik Platform (AppBuilder for Windows Phone) Companion app from your device's app store — i.e. the App Store for iOS users, Google Play for Android users, or the Windows Store for Windows Phone users (search for **Platform Companion** for iOS/Android and **AppBuilder** for Windows Phone or use the links provided below).

[![iOS app store](images/app-store-icon.png)](https://itunes.apple.com/us/app/platform-companion/id1083895251?mt=8)
[![Google Play](images/google-play-icon.png)](https://play.google.com/store/apps/details?id=com.telerik.PlatformCompanion&hl=en)
[![Windows Phone Store](images/windows-phone-store-icon.png)](https://www.windowsphone.com/en-us/store/app/appbuilder/0171d46b-b5f2-43d9-a36b-0a78c9692aab?signin=true)

* **c**. **iOS or Android:** Open the Telerik Platform Companion app on your device, and tap on **Help & About**. In the newly opened page, locate the Cordova section and select **Install Dev Application**. You will be redirected to your device's app store, where you should click **Install**. This will download and install the Cordova player app, which the Telerik Platform Companion app will use to run your applications.
* **d**. **Windows Phone:** Open the AppBuilder Companion app on your device, and then use a two-finger swipe to reveal the Companion app's menu.
![Using a two-finger swipe on your device](images/swipe.png)
* **e**. In the browser, select **Run** --> **Build**, choose your device's platform (iOS/Android/Windows Phone), select **AppBuilder companion app**, and click **Next**. Once the build finishes, tap the **QR Scanner** option in the Companion app's menu and use the integrated scanner to scan the generated QR code.

<hr data-action="end" />

When scanned, the QR code loads your image gallery application in the Companion app. Now that you have the app on your device, let's make some changes to see how easy it is to get application updates without rescanning the QR code.

<hr data-action="start" />

#### Action

* **f**. Change the `data-title` attribute of the app's view (for instance `<div data-role="view" data-title="My Awesome Photos">`).
* **g**. Save your index.html file.
* **h**. On your device, within the Companion app, tap with three fingers and hold until a popup appears.
![Using a three-finger refresh on your device](images/three-finger-tap.png)

<hr data-action="end" />

This process is known as *LiveSync*, and it makes updating your apps as easy as a quick tap. Now that you have a functioning app, and can test it on your device, let's see how to use the device's camera.
