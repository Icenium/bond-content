### Step 4. Deploy your app using the companion app

The AppBuilder companion app makes it easy to test your app on real devices, without the need to manage SDKs or deal with complex provisioning options.

<hr data-action="start" />

#### Action

* a. Download and install the AppBuilder companion app from your device's app store—i.e. the App Store for iOS users, Google Play for Android users, or the Windows Store for Windows Phone users.
* b. In the browser, select **Run** --> **Build**, select your device's platform (iOS/Android/Windows Phone), choose "AppBuilder companion app", and click Next.
* c. Scan the resulting QR code on your device.

<hr data-action="end" />

When scanned, the QR code launches the AppBuilder companion app on your device, and shows the image gallery app you just built. Now that you have the app on your device, let's make some changes.

<hr data-action="start" />

#### Action

* d. Change the `data-title` attribute of the app's view and save your changes.
* e. On your device, within the companion app, tap with three fingers and hold until a popup appears. (We need an image here.)

<hr data-action="end" />

This process is known as *LiveSync*, and it makes updating your apps as easy as a quick tap. Now that you have a functioning app, and can test it on your device, let's see how to use the device's camera.

## Lesson 2. Add images to the gallery using your device's camera

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