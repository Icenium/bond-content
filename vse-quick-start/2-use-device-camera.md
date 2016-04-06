## Lesson 2. Add photos to the gallery using your device's camera

### Step 5. Add data to the the listview

Since apps are rarely comprised of static data, let's see how you can alter your list of photos. You'll start by adding images to the list, and to do that, you'll use a button in the app's navbar.

<hr data-action="start" />

#### Action

* **a**. Add the following markup to your index.html: `<button data-role="button" data-align="right">Add</button>` within the navbar. So your markup should look like this:
```
<div data-role="navbar">
    <span data-role="view-title"></span>
    <button data-role="button" data-align="right">Add</button>
</div>
```
* **b**. Save the changes and open the app in the simulator to view the new button.

<hr data-action="end" />

**Tip**: The `data-align` attribute controls which side of the navbar the button appears on. Try playing with it to see how it affects the app's layout.

The next step is to listen for clicks on the button, and add a photo to the listview. You'll do so using Kendo UI's MVVM data bindings, as they provide an elegant way to separate the data (or the model) from the view.

<hr data-action="start" />

#### Action

* **c**. In app.js find the line of code that declares the `kendo.mobile.Application` and replace it with the code below:
```
window.listview = kendo.observable({
    addImage: function() {
        $("#images")
            .data("kendoMobileListView")
            .prepend([ "images/08.jpg" ]);
    }
});
var app = new kendo.mobile.Application(document.body, { skin: "nova" });
```
* **d**. Bind the defined view to the `listview` object by adding a `data-model="listview"` attribute (it should become `<div data-role="view" data-title="My Awesome VS Photos" data-model="listview">`).
* **e**. Add a `data-bind` attribute to the button to tell Kendo UI Mobile to invoke the `addImage` method when you click on it (`<button data-role="button" data-align="right" data-bind="click: addImage">Add</button>`).
* **f**. Save your changes and test out the result in the simulator.
* **g**. Press F12 to have the developer console shown. This article explains how you can [debug your app while running it in the simulator](http://docs.telerik.com/platform/appbuilder/cordova/debugging-your-code/debug-in-simulator#visual-studio) when needed.

<hr data-action="end" />

Now, when you click the button, the `addImage()` method runs, which calls the ListView widget's `prepend()` method to add a new image to the list.

At this point you have a list of photos, and a mechanism for adding new photos to the list. Next, let's see how you can switch from adding a hardcoded image to one that uses your device's camera.

### Step 6. Use the Cordova camera API

The Cordova library makes invoking native device methods as easy as JavaScript method calls. Let's see how to use Cordova plugins, starting with the API to take a picture using the camera: `navigator.camera.getPicture()`.

<hr data-action="start" />

#### Action

* **a**. Switch the contents of the `addImage()` method to the following code and save your changes:
```
addImage: function() {
    var success = function(data) {
        $("#images")
            .data("kendoMobileListView")
            .prepend(["data:image/jpeg;base64," + data]);
    };
    var error = function() {
        navigator.notification.alert("Unfortunately we could not add the image");
    };
    var config = {
        destinationType: Camera.DestinationType.DATA_URL,
        targetHeight: 400,
        targetWidth: 400
    };
    navigator.camera.getPicture(success, error, config);
}
```
* **b**. Notice how the change is applied on the connected device once saved.
* **c**. Click the *Add* button on the device and take a picture. If everything went right, you should see the picture appear at the top of the list.

<hr data-action="end" />

The `getPicture()` method takes three arguments: a success callback, an error callback, and a configuration object. Here you are using the success callback to take the raw data from the camera and append it to the listview. The error callback shows you an error message if something goes wrong, and the configuration object tells Cordova to capture the image as a Base64-encoded string.

What's great is that all of this is as simple as some basic configuration. Cordova turns cross-platform device access into simple JavaScript method calls, and AppBuilder makes it easy to test those calls on real devices.

Your app can now take photos from the camera and build a photo gallery, which is pretty cool. But there's one problem: the photos aren't persisted. If you close the app, all the photos in the list go away. Let's see how you can take those photos and store them in the cloud.
