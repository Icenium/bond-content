## Lesson 2. Store the images in the cloud

### Step 6. Create a Backend Services project

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

### Step 7. Upload images to your backend

With a Backend Services project in place, your next step is to add data to it. Backend Services provides SDKs for several platforms, including .NET, iOS, Android, and Windows Phone, but for a NativeScript app you're interested in the JavaScript SDK. In this step you'll add the Backend Services JavaScript SDK to your project, and use it to store your images in the cloud.

<hr data-action="start" />

#### Action

* **a**. Download the JavaScript SDK from: [https://bs-static.cdn.telerik.com/latest/everlive.all.min.js](https://bs-static.cdn.telerik.com/latest/everlive.all.min.js "https://bs-static.cdn.telerik.com/latest/everlive.all.min.js")
* **b**. Navigate back to your `Photo Album Native Code` AppBuilder project that is part of your application. 
* **c**. Right-click your `app` folder and select **Add** --> **Existing Files**. 
* **d**. Browse to the downloaded SDK file and click **Upload**.
* **e**. Add the following line to the top of your `view-model.js` file to load the SDK module:
```
var Everlive = require('./everlive.all.min');
```
* **f**. Right after loading the module declare an `everlive` object:
```
var everlive = new Everlive("YOUR API KEY");
```
* **d.** Replace the `"YOUR API KEY"` string with the API key you saved off in the previous step.

<hr data-action="end" />

The `everlive` object now contains an Everlive instance you can use to interact with your Backend Services project. Let's return to our photo management code to see how to use it.

<hr data-action="start" />

#### Action

* **e**. Add the following code to the bottom of the `photoAlbumModel`'s `tapAction`:
```
for (i = 0; i < array.length; i++) {
    var file = {
        "Filename": Math.random().toString(36).substring(2, 15) + ".jpg",
        "ContentType": "image/jpeg",
        "base64": array.getItem(i).itemImage.toBase64String(imageSourceModule.ImageFormat.JPEG, 100)
    };

    everlive.Files.create(file,
        function (data) {
        },
        function (error) {
        });
}
```

<hr data-action="end" />

This code gets all images loaded in the ListView and then using the `create()` method asynchronously uploads them to the Telerik Backend Services. Use the `LiveSync` feature and tap the Button to upload the images to the cloud.

<hr data-action="start" />

* **f**. Next, make the following code snippet replacement to change how the NativeScript ListView gets the data it needs.
Before:

```
Object.defineProperty(photoAlbumModel, "photoItems", {
    get: function () {
        return array;
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

* **g**. Save your `view-model.js` file and use the `LiveSync` feature to update the application. Your images will now be loaded from the Telerik Backend Services into the ListView.

<hr data-action="end" />

The `get()` method retrieves all pictures currently stored in the Telerik Backend Services. After you store a few pictures following the demonstrated approach, go to the “Files” menu in your Backend Services project to see a list of the photos you are storing.

And that's all there is to it, which is pretty cool if you think about it — you have just built a native mobile app that gets and uploads pictures in the cloud!

You can find the complete [Photo Album project at GitHub](https://github.com/NativeScript/sample-PhotoAlbum).