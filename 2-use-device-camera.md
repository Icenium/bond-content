## Lesson 2. Add photos to the gallery using your device's camera

### Step 4. Add data to the the listview

Since apps are rarely comprised of static data, let's see you can alter your list of photos. You'll start by adding images to the list, and to do that, you'll use a button in the app's navbar.

<hr data-action="start" />

#### Action

* **a**. Add the following markup to your index.html: `<button data-role="button" data-align="right">Add</button>`. The button should go within the navbar, so your markup should look like this:
```
<div data-role="navbar">
    <span data-role="view-title"></span>
    <button data-role="button" data-align="right">Add</button>
</div>
```
* **b**. Save your index.html file and open your app in the simulator to view the new button.

<hr data-action="end" />

> Tip: The `data-align` attribute controls which side of the navbar the button appears on. Try playing with it to see how it affects the app's layout.

The next step is to listen for clicks on the button, and then add a photo to the listview. You'll do so using Kendo UI's MVVM data bindings, as it provides an elegant way to the data (or the model) from the view.

<hr data-action="start" />

#### Action

* **c**. Paste the following code into your app.js file, directly *before* the `kendo.mobile.Application` call.
```
window.listView = kendo.observable({
    addImage: function() {
        $("#images")
            .data("kendoMobileListView")
            .prepend([ "images/08.jpg" ]);
    }
});
```
* **d**. Bind the view to the `listView` object by adding a `data-model="listView"` attribute to the `<div data-role="view">` element (`<div data-role="view" data-model="listView">`).
* **e**. Add a `data-bind` attribute to the add button (`<button data-role="button" data-align="right" data-bind="click: addImage">Add</button>`). This tells Kendo UI Mobile to invoke the `addImage` method when you click the add button.
* **f**. Save your index.html and app.js files.
* **g**. Test out the button in the simulator.

<hr data-action="end" />

Now, when you click the add button, the `addImage()` method runs, which calls the ListView widget's `prepend()` method to add a new image to the list.

At this point you have a list of photos, and a mechanism for adding new photos to the list. Next, let's see how you can switch from adding a hardcoded image to one that uses your device's camera.

### Step 5. Use the Cordova camera API

The Cordova library makes invoking native device methods as easy as JavaScript method calls. Let's see how to use Cordova plugins, starting with the camera.

<hr data-action="start" />

#### Action

* **a**. In your app.js, remove the current contents of the `addImage()` method, and replace it with a call to `navigator.camera.getPicture()`, and pass it an empty function for now. You code should look like this:
```
addImage: function() {
    navigator.camera.getPicture(function(){});
}
```
* **b**. Save the app.js file.
* **c**. Use a three-finger tap to refresh the companion app and test your changes.

<hr data-action="end" />

Now, when you tap the Add button, the Cordova `getPicture()` API invokes the camera on your device, but nothing happens after taking the picture. Let's see how you can render the photos you take on the screen.

### Step 6. Add a photo to the gallery

The Cordova `navigator.camera.getPicture()` method takes a required callback function that runs after you take a photo. Currently you're using an empty callback function (`function(){}`), so the photo is discarded. Let's switch the code to add the photo to the existing list of images.

<hr data-action="start" />

#### Action

* **a**. Switch the contents of the `addImage()` method to the following code:
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
* **b**. Save your app.js file.
* **c**. Use a three-finger tap to refresh the app on your device.
* **d**. Use the add button to take a photo and add it to the list.

<hr data-action="end" />

The `getPicture()` method takes three arguments: a success callback, an error callback, and a configuration object. Here you are using the success callback to take the raw data from the camera and append it to the listview. The error callback shows you an error message if something goes wrong, and the configuration object tells Cordova to capture the image as a Base64-encoded string (which the `success()` function uses).

What's great is that all of this is as simple as some basic configuration. Cordova turns cross-platform device access into simple JavaScript method calls, and AppBuilder makes it easy to test those calls on real devices.

Your app can now take photos from the camera and build a photo gallery, which is pretty cool. But there's one problem: the photos aren't persisted. If you close the app, all the photos in the list go away. Let's see how you can take those photos and store them in the cloud.