## Lesson 3. Extend your app with a custom Cordova plugin

Custom plugins allow you to tap into native device features that you cannot normally access via the core functionality of Cordova. In this lesson you will learn how to find and install a custom plugin called "Toast" which allows you to show a native alert dialog. Let's get started!

### Step 7. Discover the AppBuilder package manager

The AppBuilder package manager is your central hub for discovering and installing new frameworks, libraries, and Cordova plugins for your app. This lesson focuses on the **Verified Plugins Marketplace**, which is a curated list of Cordova plugins that have been fully tested and verified to empower hybrid mobile developers to build better mobile apps.

<hr data-action="start" />

#### Action

* **a**. In your **Project Navigator**, right-click on your project name—**Contacts**—and choose **Manage Packages**. This will open the AppBuilder package manager which you may use to install new versions of **Kendo UI**, install libraries and frameworks with **Bower**, and install custom Cordova plugins from the **Verified Plugins Marketplace**.

* **b**. Click on the **Plugins Marketplace** tab. Here you will find all of the custom Cordova plugins that are available as part of the Verified Plugins Marketplace. Scroll through the list to get some ideas of other ways to extend your app.

<hr data-action="end" />

### Step 8. Setup and configure the Toast plugin

Let's install the "Toast" plugin. Again, this plugin allows you to display a "toast" (a.k.a. native alert dialog) in your app that fades away on its own without user intervention. You'll use this to show a notification after you have used your device's camera to populate a profile picture.

<hr data-action="start" />

#### Action

* **a**. In the provided search box, type "toast". Highlight the plugin and click the `Install` button.

* **b**. Customize the `onPhotoSuccess` function to use a toast alert by adding this code:

```
if (window.plugins && window.plugins.toast) {
    window.plugins.toast.showShortCenter("The profile picture has been updated!");
}
```

* **c**. Save all of your changes.

<hr data-action="end" />

Note that custom Cordova plugins cannot (yet) be tested in the Companion Apps. However, you may now create a build of your app and run it on a real mobile device! In the final lesson you'll learn how to create a build of your mobile app and run it on an iOS, Android, or Windows Phone device.
