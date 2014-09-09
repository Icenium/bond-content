## Lesson 3. Stores the images in the cloud

### Step 6. Create a Backend Services project

The Telerik Platform provides tools for the entire app building experience. In this lesson, you'll use create a new Backend Services project to store your data.

<hr data-action="start" />

#### Action

* **a**. Click on your account name in the top-left corner of the screen.

<hr data-action="end" />

You are now looking at your Telerik Platform home screen, which lists all workspaces on your account. Workspaces are a way of organizing your projects. You can use them to manage team members, permissions, and more. For now you have a single “Photo Album app” workspace that contains the AppBuilder project you've been working on. Your next step is to create a Backend Services Project and incorporate it into your project.

<hr data-action="start" />

#### Action

* **b**. Click on the “Photo Album app workspace to enter it.
* **c**. Within the “Photo Album app” workspace, click the green “Create Backend Services project” button.
* **d**. Give your project a name — for instance “Photo Album Backend” — and then click “Create project”.
* **e**. Click the “API Keys” menu option.
* **f**. Copy the API key shown under the “API Key” heading and paste it somewhere convenient; you'll need it to complete the next step.

<hr data-action="end" />

Within your Backend Services project you can see all the things Backend Services makes possible, such as user management, push notifications, email messaging, and more. For your photo uploader you're interested in the “Files” menu option. You'll see how to use it in the next step.

### Step 7. Upload images to your backend

With a Backend Services project in place, your next step is to add data to it. Backend Services provides SDKs for several platforms, including .NET, iOS, Android, and Windows Phone, but for a Cordova app you're interested in the JavaScript SDK. In this step you'll add the Backend Services JavaScript SDK to your project, and use it to store your images in the cloud.

<hr data-action="start" />

#### Action

* **a**. Navigate back to your “Photo Album” AppBuilder project that is part of your workspace. 
* **b**. In your index.html file, insert the following `<script>` tag to import the Backend Services SDK (which is code named Everlive). Place it directly after the `<script>` that imports kendo.mobile.min.js:
```
<script src="https://bs-static.cdn.telerik.com/1.2.5/everlive.all.min.js"></script>
```
* **c**. Add the following to the top of your app.js file, right *before* the `window.listView` definition:
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
    template: "<img src='#: data #''>"
});
```
After:
```
function loadPhotos() {
    everlive.Files.get().then(function(data) {
        var files = [];
        data.result.forEach(function(image) {
            files.push(image.Uri);
        });
        $("#images").kendoMobileListView({
            dataSource: files,
            template: "<img src='#: data #''>"
        });
    });
}
loadPhotos();
```

* **g**. Save your app.js and index.html files.
* **h**. Use a three-finger tap to refresh the app on your device.
* **i**. Click the add button on your device.
* **j**. Put your photography skills to use. You've always wanted an artistic picture of your mouse, keyboard, or laptop haven't you?

<hr data-action="end" />

The `create()` method uploads a picture to your Backend Services project and the `get()` method retrieves all pictures currently stored there. After the upload to Backend Services completes your call to `loadPhotos()` (and subsequently `el.Files.get()`) reloads your list of photos — there's no need to manually append content! After you have taken a few pictures, go to the “Files” menu in your Backend Services project to see a list of photos you are storing.

And that's all there is to it, which is pretty cool if you think about it — you just built a mobile app that takes pictures and stores them in the cloud!
