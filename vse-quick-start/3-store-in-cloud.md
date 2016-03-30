## Lesson 3. Stores the images in the cloud

### Step 7. Connect Code with Cloud

Finally, you can connect your app with the Cloud.

<hr data-action="start" />

#### Action

* **a**. From the VS toolbar select the APPBUILDER menu and then the **Connect Code with Cloud option**.
* **b**. Press the Connect button to upload the app in the cloud. Similarly you can **Get Code from Cloud**.

<hr data-action="end" />

From **Connect Code with Cloud**, you can make your local project available for collaboration in the AppBuilder cloud apps. After you connect your project to a cloud app, any changes you make locally are automatically applied to the cloud copy. To retrieve changes from the cloud copy, you need to reload the local project or to restart Microsoft Visual Studio.

From **Disconnect from Cloud**, you can detach your local project from its cloud copy.

From **Get Code from Cloud**, you can create a new project by cloning an existing cloud app. Your local project will be automatically connected to the cloud app and all changes made locally will be applied to the cloud. To retrieve changes from the cloud copy, you need to reload the local project or to restart Microsoft Visual Studio.

<hr data-action="start" />

#### Action

* **c**. Log in the In-Browser Client at [https://platform.telerik.com](https://platform.telerik.com) and open the Photo Album app connected with the Cloud.

<hr data-action="end" />

### Step 8. Create a backend

The Telerik Platform provides tools for the entire app building experience. In this lesson, you'll setup a complete backend for storing your photos in the cloud.

<hr data-action="start" />

#### Action

* **a**. Navigate to the **AppBuilder menu and select Data Navigator** option.
* **b**. From the Options choose to **Open Telerik Platform Portal**.
* **c**. As the In-Browser Client is loaded, open the Photo Album app and click on the "Data" tab on the left-hand side of the screen.
* **d**. Click the blue "Enable Data" button to enable data storage for your project.

<hr data-action="end" />

The Data tab is where you manage your application's data. You can connect to an existing database, or use the Telerik Platform's own secure backend storage system. Regardless of which approach you use, the Telerik Platform provides a simple RESTful API for accessing your data.

On this tab you'll see a "Files" menu, which is where you're going to store your app's photos. When you clicked "Enable Data", the Telerik Platform automatically created a RESTful API for you to upload files to this backend system. Before using the API though, you need to grab your App ID.

<hr data-action="start" />

#### Action

* **e**. Click the “Settings” tab in the menu on the left-hand side of the screen.
* **f**. Copy the App ID shown under the “App ID” heading and paste it somewhere convenient; you'll need it to complete the next step.

<hr data-action="end" />

### Step 9. Upload images to your backend

With your backend ready to go, your final step is to add data to it. The Telerik Platform provides backend SDKs for several platforms, including .NET, iOS, Android, and Windows Phone, but for a Cordova app you're interested in the JavaScript SDK. In this step you'll add the Backend Services JavaScript SDK to your project, and use it to store your images in the cloud.

<hr data-action="start" />

#### Action

* **a**. To synchronize the latest changes from the cloud to the solution opened in Visual Studio, you need to reload the local project or to restart Microsoft Visual Studio.
* **b**. In your index.html file, insert the following `<script>` tag to import the Backend Services SDK (which is code named Everlive). Place it directly after the `<script>` that imports kendo.mobile.min.js:
```
<script src="https://bs-static.cdn.telerik.com/1.5.8/everlive.all.min.js"></script>
```
* **c**. Add the following to the top of your app.js file, right *before* the `document.addEventListener()` call:
```
var everlive = new Everlive("YOUR API KEY");
```
* **d.** Replace the `"YOUR API KEY"` string with the API key you saved off in the previous step.

<hr data-action="end" />

The `everlive` object now contains an Everlive instance you can use to interact with your backend. Let's return to our photo management code to see how to use it.

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
* **f**. Make the following modification to change how the Kendo UI Mobile ListView gets the data it needs and save changes.

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

* **g**. From the VS toolbar select the APPBUILDER menu and then the **Synchronize PhotoAlbum with Cloud** option.
* **h**. Check the Output window where output from Build is shown and wait until the "Project files uploaded to the cloud" message is logged.
* **i**. On your device, use a three-finger tap and hold to refresh the app on your device.
* **j**. Click the add button on your device to put your photography skills to use. You've always wanted an artistic picture of your mouse, keyboard, or laptop haven't you?

<hr data-action="end" />

The `create()` method uploads a picture to your backend and the `get()` method retrieves all pictures currently stored there. After the upload to your backend completes your call to `loadPhotos()` (and subsequently `everlive.Files.get()`) reloads your list of photos — there's no need to manually append content! After you have taken a few pictures, go to the “Files” menu on the “Data” tab to see a list of photos you are storing.

And that's all there is to it, which is pretty cool if you think about it — you just built a mobile app that takes pictures and stores them in the cloud!
