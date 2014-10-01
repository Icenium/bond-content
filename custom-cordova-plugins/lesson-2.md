## Lesson 2: Learn about custom Cordova plugins

Custom plugins allow you to tap into native device features that you normally wouldn't be able to do with the core functionality of Cordova. In this lesson you will learn how to find and install two custom plugins: "Native Page Transitions", which allows you to use native device animations to seamlessly transition between views, and "Toast" which allows you to show a native alert dialog. Let's get started!

### Step 1: Discover the AppBuilder package manager

Before you begin, make sure your `index.html` file is open. You'll notice we have two Kendo UI Mobile views set up for you (`<div>` and `<div>`). These views have been pre-populated with `data-title` properties to give each view it's own distinguishing title. Each view also has a Kendo UI Mobile navbar (`<div data-role="navbar">`) which gives you a header navigation bar to display the view title.

Now that you know your app's structure, let's discover the AppBuilder package manager.

<hr data-action="start" />

#### Action

* **a**. In the `Project Navigator`, right-click on your project name and choose `Manage Packages`. This will open the AppBuilder package manager which you can use to switch between versions of **Kendo UI**, find and install new libraries with **Bower**, and install custom Cordova plugins from the **Verified Plugins Marketplace**.

* **b**. Click on the **Plugins Marketplace** tab. Here you will find all of the custom Cordova plugins that are available as part of the Verified Plugins Marketplace. This is a curated list of Cordova plugins that have been tested, verified, and extended to multiple mobile device platforms.

<hr data-action="end" />

### Step 2: Find and install the Native Page Transitions plugin

Let's install the "Native Page Transitions" plugin. As previously mentioned, this plugin allows you to tap into the native animations available on your device to move your users between views. This makes your app feel more native than by using the default CSS-based transitions.

<hr data-action="start" />

#### Action

* **a**. In the provided search box type "native page transitions".

* **b**. Highlight the plugin and click the `Install` button.

<hr data-action="end" />

### Step 3: Find and install the Toast plugin

Let's next install the "Toast" plugin. Again, this plugin allows you to display native alert dialogs in your apps that fade away on their own without user intervention.

<hr data-action="start" />

#### Action

* **a**. In the provided search box type "toast".

* **b**. Highlight the plugin and click the `Install` button.

<hr data-action="end" />
