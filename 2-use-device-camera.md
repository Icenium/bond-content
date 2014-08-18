## Lesson 2. Add images to the gallery using your device's camera

### Step 4. Add data to the the listview

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

### Step 5. Use the Cordova camera API

The Cordova library makes invoking native device methods as easy as JavaScript method calls. Let's see how to use Cordova plugins, starting with the camera.

<hr data-action="start" />

#### Action

* a. In your app.js, remove the current contents of the `addImage()` method, and replace it with a call to `navigator.camera.getPicture()`, and pass it an empty function for now (`navigator.camera.getPicture(function(){})`).
* b. Use a three-finger tap to refresh the companion app and test your changes.

<hr data-action="end" />

Now, when you tap the Add button, the camera displays on your device, but nothing happens after taking the picture. Let's see how you can render the photos you take on the screen.

### Step 6. Add a photo to the gallery

The Cordova `navigator.camera.getPicture()` method takes a required callback function that runs after you take a photo. Currently you're using an empty callback function (`function(){}`), so the photo is discarded. Let's switch the code to add the photo to the existing list of images.

<hr data-action="start" />

#### Action

* a. Switch the existing call to `navigator.camera.getPicture()` with the one below.
```
navigator.camera.getPicture(function(data) {
    $("#images")
        .data("kendoMobileListView")
        .prepend([ "data:image/jpeg;base64," + data ]);
}, function() {
    navigator.notification.alert("Unfortunately we could not add the image");
}, {
    destinationType: Camera.DestinationType.DATA_URL
});
```
* b. Use a three-finger tap to refresh the app on your device.
* c. Use the add button to take a photo and add it to the list.

<hr data-action="end" />

You are now using your device's camera to add photos to the list, but on most devices the default camera resolution is way too big for the screen, and photos jump out of the list; therefore you need to reduce the dimensions of the photo.

To do that, you'll need to add some additional configuration options to your call to `navigator.camera.getPicture()`. Currently you're only setting a `destinationType` (which is needed to display the photo in an `<img>` tag). Let's add a few more values to that object.

<hr data-action="start" />

#### Action

* a. Add `targetWidth` and `targetHeight` properties to the final `navigator.camera.getPicture()` argument. You can use the object shown below.
```
{
    destinationType: Camera.DestinationType.DATA_URL,
    targetWidth: 300,
    targetHeight: 400
}
```
* b. Use the three-finger tap to refresh the app on your device.
* c. Use the add button to take a photo and add it to the list.
* d. Try playing with a few different values for the `targetWidth` and `targetHeight`. Use *LiveSync* to see how they render on your device.

<hr data-action="end" />

As you see, Cordova turns cross-platform device access into simple JavaScript method calls, and AppBuilder makes it easy to test those calls on real devices.

Your app can now take photos from the camera and build a photo gallery, which is pretty cool. But there's one problem: the images aren't persisted. If you close the app, all the images in the list go away. Let's see how you can take those images and store them in the cloud.