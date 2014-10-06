## Lesson 1: Accessing native device features with Cordova

Hybrid mobile apps built with AppBuilder use Apache Cordova as a bridge between your HTML5 application and native device features. You use simple JavaScript commands to access these "core" Cordova plugins such as the camera, stored contacts, geolocation data, and more. In this lesson we will create an app that interacts with these native device features and displays your contacts and lets you assign a picture to each contact using the camera. Let's get started!

### Step 1: Build a screen to list your device contacts

Before you begin, make sure your `index.html` is open. You'll notice we have two Kendo UI Mobile views set up for you (`<div>` and `<div>`). These views have been pre-populated with `data-title` properties to give each view it's own distinguishing title. Each view also has a Kendo UI Mobile navbar (`<div data-role="navbar">`) which gives you a header navigation bar to display the view title in the header. Let's start by building a screen to display the contacts from your device.

<hr data-action="start" />

#### Action

* **a**. Add a `<ul>` element with an id="contacts" inside of your first Kendo UI Mobile view (`<div>`) like so:

`<div><ul id="contacts"></ul></div>`

This will become a Kendo UI Mobile ListView element, where you will populate all of the contacts from your phone.

* **b**. Now let's create a new Kendo UI template. Templates allow you to create HTML chunks that can be merged with JavaScript data. So the template you create will be merged with your contact data to provide a repeating list of your contacts in that ListView. Copy and paste this Kendo UI template into your page (outside of your Kendo UI views):

`template`

* **c**. Finally, let's wire up your device contact data with the markup you just created. This is where some of the magic of Cordova shines through. Paste the following JavaScript code into your `app.js` file (located in the `scripts` directory):

`js`

This code queries your local device contact database to load all of your contacts and binds that set of data to the ListView element you created earlier. Using Kendo UI, you are taking advantage of a JavaScript template to format each row of data in the ListView.

Now before we continue, let's quickly take a look at how you manage your Cordova plugins.

* **d**. Double click your project **Properties** in the **Project Navigator**.

* **e**. In the window provided, choose **Plugins**.

The plugin management screen is broken up into **Core Plugins** and **Custom Plugins**. By default all core Cordova plugins are enabled (you can disable individual plugins if you desire - doing so you will speed up your app builds. We will talk about custom Cordova plugins in a future lesson.

<hr data-action="end" />

### Step 2: Run the app in the AppBuilder device simulator

It's fun to see the fruits of our labor, even if we have only just begun the lesson! Let's learn a little bit about the AppBuilder device simulator - which is a built-in way to simulate how your app will look on a real mobile device. The simulator lets you pick and choose from a variety of pre-defined devices, including iOS, Android, and Windows Phone platforms. You can even customize the appearance of the simulator based on a specific OS version that you choose.

<hr data-action="start" />

#### Action

* **a**. Click the `Run` button and choose any of the provided simulator options (note: we are using the "flat" Kendo UI theme so our app will have a consistent UI across all platforms).

* **b**. With the simulator open, bring up your browser's developer tools by hitting the F12 key. Notice that you can use the dev tools to inspect your app's elements, profile your app performance, check for any network latency issues, view the console log, and more.

* **c**. Leave the simulator open, but let's go back to your code and change the title of the view that is visible in the simulator. Change the `data-title` property in the `id` view to something new and save your change. Now return to your simulator window. Notice anything different? This is what we call LiveSync - it's the ability to automatically view changes you make to ANY part of your code in the simulator (or any connected devices!) without reloading the page.

<hr data-action="end" />

*Did you know the device simulator is available in our other IDE options as well? You can choose from our native Windows client, Visual Studio extension, Command Line Interface, or a Sublime Text package.*

### Step 3: Build a screen to display contact details

You have a nice list of all of your device contacts, so the natural next step is to create a screen that displays more detailed information about a selected contact.

<hr data-action="start" />

#### Action

* **a**. (something about replacing our listview items with an href)

* **b**. Add the following markup to your second Kendo UI Mobile view (`<div id="?">`):

`html`

* **b**. Next, let's add a `data-show` property to this view. The `data-show` property tells your app to run the JavaScript function that is listed every time the app is displayed. Go ahead and add `data-show="?"` - your markup should look like this:

`html`

* **c**. Now we need to specify what that `data-show` JavaScript function is, so paste this function into your `app.js` file:

`js`

There is a fair amount going on inside this function. You are retrieving the details of the contact based on the id that you are sending in the querystring from the first view. This contact object is then used to populate the markup that you just created in the second Kendo UI Mobile view.

Go ahead and run it in the simulator again to see how it works now!

<hr data-action="end" />

Your next step is to use another core Cordova plugin (the camera) to add some flair to your app. After that we will take a look at using custom Cordova plugins from the Verified Plugins Marketplace to add even more native functionality to your app.