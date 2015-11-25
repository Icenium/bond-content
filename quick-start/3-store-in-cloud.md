## Lesson 3. Stores the images in the cloud

### Step 6. Create a Backend Services project

The Telerik Platform provides tools for the entire app building experience. In this lesson, you'll setup a complete backend for storing your photos in the cloud.

<hr data-action="start" />

#### Action

* **a**. Click on the “Data” tab on the left-hand side of the screen.
* **b**. Click the blue “Enable Data” button to enable data storage for your project.

<hr data-action="end" />

The Data tab is where you manage your application’s data. You can connect to an existing database, or use the Telerik Platform’s own secure backend storage system. Regardless of which approach you use, the Telerik Platform provides a simple RESTful API for accessing your data.

On this tab you’ll see a “Files” menu, which is where you’re going to store your app’s photos. When you clicked “Enable Data”, the Telerik Platform automatically created a RESTful API for you to upload files to this backend system. Before using the API though, you need to grab your App ID.

<hr data-action="start" />

#### Action

* **c**. Click the “Settings” tab in the menu on the left-hand side of the screen.
* **d**. Copy the App ID shown under the “App ID” heading and paste it somewhere convenient; you'll need it to complete the next step.

<hr data-action="end" />

### Step 7. Upload images to your backend

With your backend system ready to go, your final step is to upload data to it. The Telerik Platform provides backend SDKs for several platforms, including .NET, iOS, Android, and Windows Phone, but for a Cordova app you're interested in the JavaScript SDK. In this step you'll add the Backend Services JavaScript SDK to your project, and use it to store your images in the cloud.

<hr data-action="start" />

#### Action

* **a**. Navigate back to your code by clicking on the “Code” tab on the left-hand side of the screen.
* **b**. In your index.html file, insert the following `<script>` tag to import the Backend Services SDK (which is code named Everlive). Place it directly after the `<script>` that imports kendo.mobile.min.js:
```
<script src="https://bs-static.cdn.telerik.com/1.4.1/everlive.all.min.js"></script>
```
* **c**. Add the following to the top of your app.js file, right *before* the `document.addEventListener()` call:
```
var everlive = new Everlive("YOUR API KEY");
```
* **d.** Replace the `"YOUR API KEY"` string with the API key you saved off in the previous step.

<hr data-action="end" />

The `everlive` object now contains an Everlive instance you can use to interact with your Backend Services project. Let's return to our photo management code to see how to use it.

<hr data-action="start" />

#### Action

* **e**. In app.js, change the camera's `success()` function to use the following code:
```
var success = function(data) {
    everlive.Files.create({
        Filename: Math.random().toString(36).substring(2, 15) + ".jpg",
        ContentType: "image/jpeg",
        base64: data
    }).then(loadPhotos);
};
```
* **f**. Next, make the following change to change how the Kendo UI Mobile ListView gets the data it needs.

Before:
```
$("#images").kendoMobileListView({
    dataSource: [ ... ],
    template: "<img src='#: data #'>"
});
```
After:
```
function loadPhotos() {
    everlive.Files.get().then(function(data) {
        var files = [];
        for (var i = data.result.length - 1; i >= 0; i--) {
            var image = data.result[i];
            files.push(image.Uri);
        }
        $("#images").kendoMobileListView({
            dataSource: files,
            template: "<img src='#: data #'>"
        });
    });
}
loadPhotos();
```

* **g**. Save your app.js and index.html files.
* **h**. Use a three-finger tap and hold to refresh the app on your device.
* **i**. Click the add button on your device.
* **j**. Put your photography skills to use. You've always wanted an artistic picture of your mouse, keyboard, or laptop haven't you?

<hr data-action="end" />

The `create()` method uploads a picture to your Backend Services project and the `get()` method retrieves all pictures currently stored there. After the upload to Backend Services completes your call to `loadPhotos()` (and subsequently `everlive.Files.get()`) reloads your list of photos — there's no need to manually append content! After you have taken a few pictures, go to the “Files” menu in your Backend Services project to see a list of photos you are storing.

And that's all there is to it, which is pretty cool if you think about it — you just built a mobile app that takes pictures and stores them in the cloud!
