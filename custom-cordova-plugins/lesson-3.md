## Lesson 3: Extend your app with a custom Cordova plugin from the Verified Plugins Marketplace

Custom plugins allow you to tap into native device features that you normally wouldn't be able to do with the core functionality of Cordova. In this lesson you will learn how to find and install two custom plugins: "Native Page Transitions", which allows you to use native device animations to seamlessly transition between views, and "Toast" which allows you to show a native alert dialog. Let's get started!

### Step 1: Discover the AppBuilder package manager

The AppBuilder package manager is your central hub for discovering and installing new frameworks, libraries, and plugins for your app. This lesson focuses on the **Verified Plugins Marketplace**, which is a curated list of Cordova plugins that have been fully tested and verified to empower hybrid mobile developers to build bigger and better mobile apps.

<hr data-action="start" />

#### Action

* **a**. In your `Project Navigator`, right-click on your project name and choose `Manage Packages`. This will open the AppBuilder package manager which you can use to switch between versions of **Kendo UI**, find and install new libraries with **Bower**, and install custom Cordova plugins from the **Verified Plugins Marketplace**.

* **b**. Click on the **Plugins Marketplace** tab. Here you will find all of the custom Cordova plugins that are available as part of the Verified Plugins Marketplace. Scroll through the list to get some ideas of other ways to extend your app.

<hr data-action="end" />

### Step 2: Setup and configure the Native Page Transitions plugin

Let's install the "Native Page Transitions" plugin. As previously mentioned, this plugin allows you to tap into the native animations available on your device to move your users between views. This makes your app feel more native than by using the default CSS-based transitions.

<hr data-action="start" />

#### Action

* **a**. In the provided search box type "native page transitions". Highlight the plugin and click the `Install` button.

* **b**. With the plugin installed, go back to your `app.js` file and add a new navigation function:

`js`

We will call this function whenever we want to navigate between our views. This function calls the Native Page Transition plugin to perform fully native transitions.

* **c**. In our Kendo UI template, let's change the `href` to call our new JavaScript function instead, like so: `href=''`. Your complete Kendo UI template should look like this now:

`template`

* **d**. Likewise, let's change the default behavior of our `back` button on the contact detail view to use a native page transition as well. TBD.

`code`

<hr data-action="end" />

### Step 3: Setup and configure the Toast plugin

Let's next install the "Toast" plugin. Again, this plugin allows you to display a "toast" (a.k.a. native alert dialog) in your apps that fade away on their own without user intervention. You'll use this to show a notification after you have used your device's camera.

<hr data-action="start" />

#### Action

* **a**. In the provided search box type "toast". Highlight the plugin and click the `Install` button.

* **b**. Let's customize the `onCameraSuccess` and `onCameraFailure` functions to use Toasts instead of standard alerts:

`js` 

<hr data-action="end" />

Currently custom Cordova plugins cannot be used with the device simulator. However, we can now create a build of our app and learn how to run it on a real mobile device. In the final lesson you'll learn how to create a build and run it on your iOS, Android, or Windows Phone device.
