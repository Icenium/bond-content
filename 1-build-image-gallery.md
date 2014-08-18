## Lesson 1. Build an image gallery

(I'd really like to have some sort of theme for this so we're not just using random images, and so I have something to introduce briefly before diving into code. Something that segues nicely into the second lesson—using the camera—would be nice too. What if the theme were "Famous Developers" and we used appropriately licensed images of people like Donald Knuth, Douglas Crockford, and such. It could be fun when we get to adding an image using the camera in lesson 2. Cats are always a safe choice too. Other ideas?)

### Step 1. Add a navbar

Kendo UI Mobile apps are made up of views, and a basic one view has already been created for you (`<div data-role="view">`). To liven up this view, you'll start by adding a Kendo UI NavBar widget to display the view's title.

<hr data-action="start" />

#### Action

* a. Add the following markup to the view (`<div data-role="view">`):
```
<div data-role="navbar">
    <span data-role="view-title"></span>
</div>
```
* b: Add a `data-title` attribute to the view, e.g. `<div data-role="view" data-title="Images">`.

<hr data-action="end" />

Kendo UI Mobile automatically styles the navbar to look great on each platform. To see what this looks like, let’s try this app out in the AppBuilder simulators.

<hr data-action="start" />

#### Action

* c. Open the iPhone 5 simulator by selecting **Run** --> **iPhone 5**.
* d. With the simulator open, change the title of the view by updating its `data-title` attribute 
The simulators automatically update with every change you make. Take a minute to experiment with the various things the simulator can do. You can even open multiple simulators at once!

<hr data-action="end" />

### Step 2. Show data in a listview

Your next step is to show some images in your gallery, and you'll use the Kendo UI ListView widget to do that. The ListView widget automatically handles complex use cases such as endless scrolling and pull-to-refresh, but for now you'll start by displaying a static list of images.

<hr data-action="start" />

#### Action

* a. Add a `<ul>` element underneath the navbar with an `id` of `"images"` (`<ul id="images"></ul>`).
* b. In your app.js, use the code below to select the `<ul>` with jQuery and convert it into a ListView widget. Make sure this code appears after the `kendo.mobile.Application()` call, as the Kendo UI Mobile app needs to exist before you initialize widgets.
```
$("#images").kendoMobileListView({
    dataSource: [
        "http://twitter.com/favicon.ico",
        "http://facebook.com/favicon.ico",
        "http://google.com/favicon.ico"
    ]
});
```
* d. Run this in the simulator to see how this displays.

<hr data-action="end" />

If you tried this in the simulator, you saw that this code displays file names rather than images. To tell the ListView widget how to display its data—e.g. the list of images—you can use Kendo UI Mobile's templating system.

<hr data-action="start" />

#### Action

* e. Add a `template` option to the `kendoMobileListView` plugin and set it to `"<img src='#: data #''>"` (that is, `template: "<img src='#: data #''>"`).
* f. Run the app in the simulator to see the resulting images.

<hr data-action="end" />

### Step 3. Add data to the the listview

Since apps are rarely comprised of static data, let's see you can alter your list of images. You'll start by adding images to the list, and to do that, you'll use a button in the app's navbar.

<hr data-action="start" />

#### Action

* a. Add the following markup to the navbar: `<button data-role="button" data-align="right">Add</button>`. The `data-align` attribute controls which side the button appears on. Try playing with it in the simulator to see how it affects the app's layout.

<hr data-action="end" />

The next step is to listen for clicks on the button, and then add an image to the listview. You'll do so using Kendo UI's MVVM data bindings, as it provides an elegant way to the data (or the model) from the view.

<hr data-action="start" />

#### Action

* b. Paste the following code into your app.js file, *before* the `kendo.mobile.Application` call.
```
window.listView = kendo.observable({
    addImage: function() {
        $("#images")
            .data("kendoMobileListView")
            .prepend([ "http://telerik.com/favicon.ico" ]);
    }
});
```

<hr data-action="end" />

This creates the an observable object with the functionality you need for your view.

<hr data-action="start" />

#### Action

* c. Bind the view to the `listView` object by adding a `data-model="listView"` attribute to the `<div data-role="view">` element (`<div data-role="view" data-model="listView">`).
* d. Add a `data-bind` attribute to the add button (`<button data-role="button" data-align="right" data-bind="click: addImage">Add</button>`). This tells Kendo UI Mobile to invoke the `addImage` method when the user clicks the add button.
* e. Test out the button in the simulator.

<hr data-action="end" />

Now, when the user clicks the button, the `addImage()` method runs, which calls the ListView widget's `prepend()` method to add a new image to the list.

At this point you have a list of images, and a mechanism for adding new images to the list. In the next lesson, you'll switch the add button to add images using your device's camera. And in lesson 3 you'll move all the data to a database that lives in the cloud. Before getting into that though, let's see how to test this app on physical devices.