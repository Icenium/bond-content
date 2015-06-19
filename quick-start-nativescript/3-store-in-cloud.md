## Lesson 3. Store the images in the cloud

### Step 7. Create a Backend Services project

The Telerik Platform provides tools for the entire app building experience. In this lesson, you'll use create a new Backend Services project to store your data.

<hr data-action="start" />

#### Action

* **a**. Click on your account name in the top-left corner of the screen.

<hr data-action="end" />

You are now looking at your Telerik Platform home screen, which lists all applications on your account. Applications are a way of organizing your projects. You can use them to manage team members, permissions, and more. For now you have a single “Photo Album Native” application that contains the AppBuilder project you've been working on. Your next step is to create a Backend Services Project and incorporate it into your project.

<hr data-action="start" />

#### Action

* **b**. Click on the `Photo Album Native` application to enter it.
* **c**. Within the `Photo Album Native` application, click the green **Create Backend Services project** button.
* **d**. Select **Start from scratch**.
* **e**. Give your project a name — for instance `Photo Album Native Backend` — and then click **Create project**.
* **f**. Find the **Cloud Files** box and click its **Add to project** button to enable file storage for your Backend Services project.
* **g**. Click the **API Keys** option from the menu on the left-hand side of the screen.
* **h**. Copy the API key shown under the **API Key** heading and paste it somewhere convenient; you'll need it to complete the next step.

<hr data-action="end" />

Within your Backend Services project you can see all the things Backend Services makes possible, such as user management, push notifications, email messaging, and more. For your photo uploader you're interested in the **Files** menu option. You'll see how to use it in the next step.

### Step 8. Upload images to your backend

With a Backend Services project in place, your next step is to add data to it. Backend Services provides SDKs for several platforms, including .NET, iOS, Android, and Windows Phone, but for a NativeScript app you're interested in the JavaScript SDK. The `Photo Album Native Code` project has already been provisioned with the appropriate JavaScript SDK in the form of the `everlive.all.min.js` file. You should now get back to your `Photo Album Native Code` project and load the JavaScript SDK implementation following the steps below.

<hr data-action="start" />

#### Action

* **a**. Click on your account name in the top-left corner of the screen.
* **b**. Click on the `Photo Album Native` application to enter it.
* **c**. Open the `view-model.js` file and add the following line at the top to load the SDK module:
```
var Everlive = require('./everlive.all.min');
```
* **d**. Right after loading the module declare an `everlive` object:
```
var everlive = new Everlive("YOUR API KEY");
```
* **e.** Replace the `"YOUR API KEY"` string with the API key you saved off in the previous step.

<hr data-action="end" />

The `everlive` object now contains an Everlive instance you can use to interact with your Backend Services project. Let's return to our photo management code to see how to use it.

> Tip: The latest versions of the JavaScript SDKs can be downloaded from [this documentation article](http://docs.telerik.com/platform/backend-services/development/javascript-sdk/introduction). 

<hr data-action="start" />

#### Action

* **f**. Make the following code snippet replacement to change the way the NativeScript ListView gets the data it needs.

Before:

```
Object.defineProperty(photoAlbumModel, "photoItems", {
    get: function () {
        return localImagesArray;
    },
    enumerable: true,
    configurable: true
});
```
After:

```
var backendArray = new observableArrayModule.ObservableArray();

Object.defineProperty(photoAlbumModel, "photoItems", {
    get: function () {
        everlive.Files.get().then(function (data) {
                data.result.forEach(function (fileMetadata) {
                    imageSourceModule.fromUrl(fileMetadata.Uri).then(function (result) {
                        var item = {
                            itemImage: result
                        };
                        backendArray.push(item);
                    });
                });
            },
            function (error) {});

        return backendArray;
    },
    enumerable: true,
    configurable: true
});
```

The `get()` method retrieves all pictures stored in the Telerik Backend Services. If you now deploy the Photo Album app using `LiveSync` feature, the ListView will appear empty, because there are no images in the Telerik Backend Services project yet. But don't worry, we will soon add a few.

* **g**. Add the following code to the bottom of the `cameraModule.takePicture` promise:
```
var file = {
    "Filename": Math.random().toString(36).substring(2, 15) + ".jpg",
    "ContentType": "image/jpeg",
    "base64": picture.toBase64String(enums.ImageFormat.jpeg, 100)
};

everlive.Files.create(file,
    function (data) {},
    function (error) {});
```

Here is what the tapAction implementation should look like:

```
photoAlbumModel.tapAction = function () {
    //localImagesArray.push([item7, item8]);

    cameraModule.takePicture({
        width: 300,
        height: 300,
        keepAspectRatio: true
    }).then(function (picture) {
        var item = {
            itemImage: picture
        };
        backendArray.push(item);

        var file = {
            "Filename": Math.random().toString(36).substring(2, 15) + ".jpg",
            "ContentType": "image/jpeg",
            "base64": picture.toBase64String(enums.ImageFormat.jpeg, 100)
        };

        everlive.Files.create(file,
            function (data) {},
            function (error) {});
    });
};
```

This code displays the images you take in the ListView (using the `backendArray` instead of the `localImagesArray`) and uploads them asynchronously to the Telerik Backend Services using the `create()` method.

* **g**. Save the `view-model.js` file and use the `LiveSync` feature to update the application. On the device, tap the `Add new photos` to take photos and upload the images to the cloud.
* **h**. After you store a few pictures, go to the “Files” menu in your Backend Services project to see a list of the photos you are storing.
* **i**. To see how the images are downloaded asynchronously from the Telerik Backend Services, close (kill) the `Photo Album Native` app on your device and reopen it. Your previously taken photos will load accordingly.

<hr data-action="end" />

And that's all there is to it, which is pretty cool if you think about it — you have just built a native mobile app that gets and uploads pictures in the cloud!

You can find the complete [Photo Album project at GitHub](https://github.com/NativeScript/sample-PhotoAlbum).