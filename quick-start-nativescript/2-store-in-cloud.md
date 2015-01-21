## Lesson 2. Store the images in the cloud

### Step 6. Create a Backend Services project

The Telerik Platform provides tools for the entire app building experience. In this lesson, you'll use create a new Backend Services project to store your data.

<hr data-action="start" />

#### Action

* **a**. Click on your account name in the top-left corner of the screen.

<hr data-action="end" />

You are now looking at your Telerik Platform home screen, which lists all workspaces on your account. Workspaces are a way of organizing your projects. You can use them to manage team members, permissions, and more. For now you have a single “Photo Album app” workspace that contains the AppBuilder project you've been working on. Your next step is to create a Backend Services Project and incorporate it into your project.

<hr data-action="start" />

#### Action

* **b**. Click on the “Photo Album Native app workspace to enter it.
* **c**. Within the “Photo Album Native app” workspace, click the green “Create Backend Services project” button.
* **d**. Give your project a name — for instance “Photo Album Native Backend” — and then click “Create project”.
* **e**. Find the “Cloud Files” box and click its "Add to project" button to enable file storage for your Backend Services project.
* **f**. Click the “API Keys” option from the menu on the left-hand side of the screen.
* **g**. Copy the API key shown under the “API Key” heading and paste it somewhere convenient; you'll need it to complete the next step.

<hr data-action="end" />

Within your Backend Services project you can see all the things Backend Services makes possible, such as user management, push notifications, email messaging, and more. For your photo uploader you're interested in the “Files” menu option. You'll see how to use it in the next step.

### Step 7. Upload images to your backend

With a Backend Services project in place, your next step is to add data to it. Backend Services provides SDKs for several platforms, including .NET, iOS, Android, and Windows Phone, but for a NativeScript app you're interested in the JavaScript SDK. In this step you'll add the Backend Services JavaScript SDK to your project, and use it to store your images in the cloud.

<hr data-action="start" />

#### Action

* **a**. Download the JavaScript SDK from: [https://bs-static.cdn.telerik.com/latest/everlive.all.min.js](https://bs-static.cdn.telerik.com/latest/everlive.all.min.js "https://bs-static.cdn.telerik.com/latest/everlive.all.min.js")
* **b**. Navigate back to your “Photo Album Native” AppBuilder project that is part of your workspace. 
* **c**. Right-click your `app` folder and select **Add** --> **Existing Files**. 
* **d**. Browse to the downloaded SDK file and click **Upload**.
* **e**. Add the following line to the top of your `view-model.js` file to load the SDK module:
```
var backendServices = require('./everlive.all.min');
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

* **e**. Add the following code to the bottom of the `PhotoAlbumModel`'s `tapAction`:
```
for (i = 0; i < array.length; i++) {
    var file = {
        "Filename": Math.random().toString(36).substring(2, 15) + ".jpg",
        "ContentType": "image/png",
        "base64": array.getItem(i).photoItemImage.toBase64String(1, 100)
    };

    everlive.Files.create(file,
        function (data) {
        },
        function (error) {
        });
}
```
This code gets all images loaded in the ListView and asynchronously uploads them to the Telerik Backend Services.
* **f**. Next, make the following change to change how the NativeScript ListView gets the data it needs.
Before:
```
Object.defineProperty(PhotoAlbumModel.prototype, "photoItems", {
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

Object.defineProperty(PhotoAlbumModel.prototype, "photoItems", {
    get: function () {
        everlive.Files.get().then(function (data) {
                data.result.forEach(function (fileMetadata) {
                    imageSourceModule.fromUrl(fileMetadata.Uri).then(function (result) {
                        var item = {
                            photoItemImage: result
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
* **g**. Save your `view-model.js` file and use the LiveSync feature to update the application on your iOS/Android.

<hr data-action="end" />

The `create()` method asynchronously uploads a picture to your Backend Services project and the `get()` method retrieves all pictures currently stored there. After you store a few pictures following the demonstrated approach, go to the “Files” menu in your Backend Services project to see a list of photos you are storing.

And that's all there is to it, which is pretty cool if you think about it — you just built a native mobile app that gets and uploads pictures in the cloud!
