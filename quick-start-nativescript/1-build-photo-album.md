## Lesson 1. Build a photo album

### Step 1. Create a new project

In this tutorial, you will learn how to load data in a UI control, how to bind UI properties to a view model and how to define the style of the UI with CSS. As a result you will have a native Photo Album application written entirely in XML and JavaScript.

You will now create your first NativeScript project. The four steps below do just that. You can skip them and directly open the Photo Album Native app and then the Photo Album Native project.

<hr data-action="start" />

#### Action

* **a.** Click the blue **Create app** button at the top-left. 
* **b.** Select **Start from a blank app**.
* **c.** Name your app `Photo Album Native` and click the green **Create app** button.
* **d.** Click the **Create AppBuilder Native project** button.
* **e.** Select **Choose project template** and then select the  **NativeScript Blank (JavaScript)** template.
* **f.** Name the project `Photo Album Native Code` and click the green **Create Project** button.

<hr data-action="end" />

This newly created project is automatically checked into the integrated AppBuilder version control. AppBuilder opens your new project and lists all project files in the Project Navigator. There you can see the following items:

* **app** The `app` folder contains the entire app functionality.
* **bootstrap.js** This file contains the code that initializes your project as a native app.
* **app.js** The app.js module contains application specific code, such as which page is the starting page of the application. 
* **main-page.js** This is the main JavaScript file used to implement the business logic of the initial page.
* **main-page.xml** This is the file used to implement the UI of the initial page.
* **App_Resources** The App_Resources folder contains application assets such as icons, splash screens and configuration files such as Info.plist and AndroidManifest.xml.
* **tns_modules** The tns_modules folder contains the NativeScript libraries for accessing the native device platform functionality. Libraries might consist of platform-specific and common files and an index.js that exports the module. Platform-specific files contain platform-specific NativeScript code. When you build your app for iOS or Android, AppBuilder automatically picks up the needed platform-specific files. 

You will focus on the `app` folder to create the logic of our application. You can also quickly check the `tns_modules` to get a better idea of what components the NativeScript framework provides.

### Step 2. Define the user interface

You are now going to create a page that contains a button at the bottom and a listview that spans over the remaining screen area. You will implement this layout with a grid panel. You will declare the UI of the page in `main-page.xml`.

<hr data-action="start" />

#### Action 

* **a.** Open the `app --> main-page.xml` file.
* **b.** Between the `Page` start and end tags, create a `GridLayout` instance and set up its layout. For the purposes of the application, the grid should consist of two rows - the first row will take up all the available space the `GridLayout` provides and the second row will be sized based on its content:
```
<GridLayout rows="*, auto">
</GridLayout>
```
* **c.** Now, it's time to add a `Button` and `ListView` to the `GridLayout`. The declarations of the controls should be placed right after the closing tag for the `GridLayout`. These declarations are as simple as:
```
<ListView row="0" />  
<Button row="1" />
```

The complete XML declaration looks like this:
```
<Page xmlns="http://www.nativescript.org/tns.xsd" >
    <GridLayout rows="*, auto">
        <ListView row="0" />
        <Button row="1" />
    </GridLayout>
</Page>
```

<hr data-action="end" />

Regardless of the small amount of UI and settings that you have put on the page, you probably want to try the result on a real device now. So, in the next step, let's see how easy it is to deploy this native app.

### Step 3. Deploy your app on a device using the companion app

The NativeScript companion app makes it easy for you to test your app on real devices, without the need to manage SDKs or deal with complex provisioning options.

![iOS app store](images/native-script-app.png)

<hr data-action="start" />

#### Action

* **a.** Get your iOS or Android device.
* **b.** Download and install the NativeScript companion app from the App Store or Google Play, respectively.

[![iOS app store](images/app-store-icon.png)](https://itunes.apple.com/bg/app/nativescript/id882561588?mt=8)
[![Google Play](images/google-play-icon.png)](https://play.google.com/store/apps/details?id=com.telerik.NativeScript&hl=en)

* **c.** In the browser, select **Run** --> **Build**, select your platform (iOS/Android), choose **AppBuilder companion app**, and click **Next**. You will see a QR code pointing to the application. 
* **e.** **Android:** Open the NativeScript app on your device. With the NativeScript app running, open the notification drawer. Tap **Scan** and use the integrated scanner to scan the QR code displayed in the browser.
* **f.** **iOS:** Open the NativeScript app on your device and with a two-finger left-to-right swipe reveal the companion app menu. Tap **QR Scanner** in the menu and use the integrated scanner to scan the QR code displayed in the browser.
![Using a two-finger swipe on your device](images/swipe.png)

* **e.** **Windows Phone:** A companion app for Windows Phone is not yet available.

<hr data-action="end" />

When scanned, the QR code loads your app in the NativeScript companion app. You should see the button at the bottom and some white space taken from the empty list view. Now that you have the app on your device, let's make some changes to see how easy it is to get application updates without rescanning the QR code.

<hr data-action="start" />

#### Action

* **a.** Set the `text` attribute in the `Button` declaration. The result should look like this:
```
<Button text="Test Message" row="1"/>
```
* **b.** **Android:** With the NativeScript application running, open the notification center and tap **Sync**.
* **c.** **iOS:** With the NativeScript application running, tap with three fingers and hold until a popup appears.

<hr data-action="end" />

Your updated app will be downloaded on the device and you will immediately see the change in the Button text.

This process is known as LiveSync and makes updating your apps as easy as a quick tap. Now that you have an app and can test it on your device, let's populate the `ListView` with items.


### Step 4. Populate a ListView with images

First, let's populate the `ListView` with items. You will add a few images to the project and will set them to be the items source of the `ListView`. The items source definition will be created in a view model file and will be then consumed by the `ListView` from the `main-page.js/main-page.xml` files.

If you have started this tutorial from the ready-made Photo Album Native application, the three steps below have already been done for you.

<hr data-action="start" />

#### Action

* **a.** Right-click the `app` folder and choose **Add** --> **New File**. Name the file `view-model.js`. We will define the model here.
* **b.** Right-click the `app` folder and choose **Add** --> **New Folder**. Name the folder `res`. We will store the images here.
* **c.** Right-click the `res` folder and choose **Add** --> **Existing Files**. Browse your machine and add six images. 

<hr data-action="end" />

Now, it's time to define the data source in the view model.

<hr data-action="start" />

#### Action

* **a.** Open the `view-model.js` file that we have just created and add the following declarations that load the required modules from the `tns_modules` folder: 
```
var observable = require("data/observable");
var observableArrayModule = require("data/observable-array");
var imageSourceModule = require("image-source");
var fileSystemModule = require("file-system");
```
You need the first one to reflect the changes that happen in the view model in the UI. The second is needed for the collection where the image objects will be stored. The other two are needed to load the image files that you have just added to the project.
* **b.** Create an `ObservableArray()` and add the images there using the push method:
```
var array = new observableArrayModule.ObservableArray();
var directory = "/res/";
function imageFromSource(imageName) {
    return imageSourceModule.fromFile(fileSystemModule.path.join(__dirname, directory + imageName));
};
var item1 = {itemImage: imageFromSource("01.jpg")}; 
var item2 = {itemImage: imageFromSource("02.jpg")}; 
var item3 = {itemImage: imageFromSource("03.jpg")}; 
var item4 = {itemImage: imageFromSource("04.jpg")}; 
var item5 = {itemImage: imageFromSource("05.jpg")}; 
var item6 = {itemImage: imageFromSource("06.jpg")}; 
array.push([item1, item2, item3, item4, item5, item6]);
var item7 = {itemImage: imageFromSource("07.jpg")}; 
var item8 = {itemImage: imageFromSource("08.jpg")}; 
```
Note that two images are not pushed to the array. This is because you will add them later on a button click.
* **c.** Create the view model implementation, let's call it `PhotoAlbumModel`. The basic implementation of the model looks like this: 
```
var PhotoAlbumModel = (function (_super) {
    __extends(PhotoAlbumModel, _super);
    function PhotoAlbumModel() {
        _super.call(this);
    }
    return PhotoAlbumModel;
})(observable.Observable);
```

* **d.** Inheriting the `PhotoAlbumModel` from `Observable()` lets you bind controls properties from the main page to properties in the view model. To get all the functionality that `Observable` (or any other parent type) provides, you should add the following code before the `PhotoAlbumModel` implementation:
```
var __extends = this.__extends || function (d, b) {
    for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];
    function __() { this.constructor = d; }
    __.prototype = b.prototype;
    d.prototype = new __();
};
```

> Tip: Keyboard shortcut `Ctrl` + `Alt` + `F` cleans up indentation and formatting for you. Try using it after you paste in code throughout these lessons.

* **f.** Expose a `photoItems` property at the `PhotoViewModel` that returns the `ObservableArray` containing the images:
```
Object.defineProperty(PhotoAlbumModel.prototype, "photoItems", {
      get: function () {
          return array;
      },
      enumerable: true,
      configurable: true
  })
```
* **g.** At the bottom of the `view-model.js`, outside the `PhotoAlbumModel` implementation, declare the `PhotoAlbumModel` in the module `exports` to make this model accessible from the UI.
```
exports.PhotoAlbumModel = PhotoAlbumModel;
```
* **h.** With the view model created, let's get back to our `main-page.js/main-page.xml` files to populate `ListView` with data.
In the `main-page.js` file, load the view model specifying its location in the project folder structure and then create an instance of the `PhotoAlbumModel`.
```
var modelModule = require("./view-model");
var model = new modelModule.PhotoAlbumModel();
```
* **i.** Create a function called `onPageLoaded`. In this function you will set the `bindingContext` of the page to be our view model:
```
function onPageLoaded(args) {
    var page = args.object;
    page.bindingContext = model;
}
```
* **j.** Set the `exports.onPageLoaded` at the end of `main.js` to make the `onPageLoaded` function accessible from the UI:
```
exports.onPageLoaded = onPageLoaded;
```
* **k.** To call the `onPageLoaded` method when the page loads, add the loaded attribute with value `onPageLoaded` in the `Page` tag in `main-page.xml`:
```
<Page loaded="onPageLoaded" xmlns="http://www.nativescript.org/tns.xsd" >
```
* **l.** You now should bind the `ListView` to the `photoItems` collection of the `PhotoAlbumModel`. To do this, set the `items` attribute of the `ListView` tag to `{{ photoItems }}`:
```
<ListView items="{{ photoItems }}" row="0">
```
* **m.** The `ListView` is now bound to the collection of images. But in order to display the images, you should set the appropriate template consisting of a `GridLayout` with an `Image` object inside. Place the template declaration between the `ListView` start and end tags and bind the `Image`'s `source` property to the `itemImage` property of the `photoItems` objects:
```
<ListView items="{{ photoItems }}" row="0" >
	<ListView.itemTemplate>                
		<GridLayout>
			<Image source="{{ itemImage }}" row="0"/>
		</GridLayout>               
	</ListView.itemTemplate>
</ListView>
```

<hr data-action="end" />

As a result, the `ListView` is populated with images. Use the `LiveSync` feature of the NativeScript companion app to see them. Let's now add two more images on a button click and style the button.


### Step 5. Respond to actions and add some style

The `ListView` is already populated with a few images. You will now add a few more images on a button tap. Then, you will learn how to bind the Button's `text` property to a property of the PhotoAlbumModel, thus informing the end-user about the successful tap action. Finally, you will understand how to put some style to the button.

<hr data-action="start" />

#### Action

* **a.** In the `PhotoAlbumModel`, create a function called `tapAction`. In the body of this function add two more images to the `ObservableArray` instance.
```
PhotoAlbumModel.prototype.tapAction = function () {
	array.push(item7);
	array.push(item8);
};
```
* **b.** In `main-page.js` create an event handler function for the `click` event of the button. There, call the `tapAction` function of the `PhotoAlbumModel`:
```
function buttonClick(args) {
    model.tapAction();
}
```
* **c.** At the bottom of the `main-page.js` declare the `buttonClick` function in the module exports to make it accessible from the UI.
```
exports.buttonClick = buttonClick;
```
* **d.** In the `main-page.xml` file, set the `click` attribute of the `Button` tag to the `buttonClick` function. This will call the function when the button is tapped:
```
<Button text="Test Message" click="buttonClick" row="1"/>
```

<hr data-action="end" />

Tapping the Button will now add two more images to the ListView. But let's make the text of the Button reflect the action that happened. To do so, you can bind the `text` property of the Button to the `message` property of the `PhotoAlbumModel`.

<hr data-action="start" />

#### Action

* **a.** In the `PhotoAlbumCollection` constructor, add the following line to prompt the user to add more images:
```
this.set("message", "Add new images");
```
* **b.** In the `tapAction` function, add the following line to inform the user about the new images and the total count of images in the ListView:
```
this.set("message", "Images added. Total images:" + array.length);
```
* **c.** In `main-page.xml`, set the `text` attribute of the `Button` tag to `{{ message }}`. This will bind the `Button`'s `text` property to the `message` property of the `PhotoAlbumModel`. So, initially, the button will show `Add new images` and after you tap it, it will show `Images added. Total images: 8`.
```
<Button text="{{ message }}" click="buttonClick" row="1"/>
```

<hr data-action="end" />

Finally, let's beautify the button. NativeScript uses standards-based CSS syntax to style the UI elements.

<hr data-action="start">

#### Action

* **a.** Right-click the `app` folder and choose **Add** --> **New File**.
* **b.** Name the file after the file that contains the UI objects that need styling. In our case, the name should be `main-page.css`.
* **c.** In order to change the foreground color, background color and font properties of the `Button` type, set the following CSS:
```
Button {
    background-color: gray;
    font-size: 20;
    color: #3BB9FF; 
}
```
<hr data-action="end">

This is it! Now you know how to load data in a UI control, how to bind UI properties to a view model and how to define the style of the UI with CSS.

Use `LiveSync` to update the application on the device and see the results.