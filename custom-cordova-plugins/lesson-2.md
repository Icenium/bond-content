## Lesson 2: Extending your app by integrating with the device camera

The app you have created functions just great right now by consuming contacts data, but let's take a look at how we can easily integrate with another core Cordova plugin, the camera.

### Step 1: Customize your main contacts view to display an inline profile picture

Let's do a slight customization to your Kendo UI template to retrieve and display any profile pictures you already have stored for your contacts.

<hr data-action="start" />

#### Action

* **a**. Add the following `img` element to your Kendo UI template: `<img>`. The end result should look like this:

`template`

* **b**. Now, when binding your contact data to the ListView, you'll have to make some adjustments to also now bind any photos. Go ahead and add this to your `app.js` file, tbd.

`js`

<hr data-action="end" />

### Step 2: Customize the contacts detail screen to display a profile picture

Now, let's do the same thing to your contacts detail screen. We want to show the profile picture when the contact details are displayed.

<hr data-action="start" />

#### Action

* **a**. Add the following `img` element to the top of your second Kendo UI Mobile view (`<div>`): `<img>`. The end result should look like this:

`view`

* **b**. In order to actually display the photo, you'll have to add one line of JavaScript to your existing function that populates the other contact details: `js`. The final function should look like this:

`js`

<hr data-action="end" />

### Step 3: Use the device camera to add a profile picture

Since you have a place to display a picture, now is a good time to learn how to populate the picture with a real image.

<hr data-action="start" />

#### Action

* **a**. Add a button to your contacts detail view (`<div>`) that will initiate the camera. This can be placed underneath the the existing elements which display data about your contact.

`<button>`

* **b**. The button references a JavaScript function you haven't created yet, so let's get that in there. Paste the following function into your `app.js` file:

`js`

What's happening here is that once the button is pressed, Cordova finds your device camera and enables it. You may then take a picture or choose an image from your device's photo library. There are two callback functions as well, `xxx`, and `yyy`. The former is called when you successfully take or use an image, the latter is called if something goes wrong.

* **c**. Save all of your changes and run your app in the device simulator again. Another useful feature of the device simulator is that you can test out most core Cordova plugin features without using a real device!

<hr data-action="end" />

Your next step is learn how to use custom Cordova plugins from the Verified Plugins Marketplace to add even more native functionality to your app. Finally, you will learn how to run your app on a real mobile device.