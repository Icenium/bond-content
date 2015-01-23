## Lesson 1. Build a photo album

### Step 1. Create a new project. Project structure.

Throughout this QuickStart tutorial, you will learn how to load data in a UI control, how to bind UI properties to a view-model and how to define the style of the UI with CSS. As a result you will have a native Photo Album application written all in xml and JavaScript.

We are now going to create our first NativeScript project. The fours steps below do just that. You can skip them and directly use the Photo Album Native workspace with the Photo Album Native project that have been created for you.

<hr data-action="start" />

#### Action

* **a.** Click the blue **Create Workspace** button at the top-left. Name your workspace `Photo Album Native app` and click the green **Create Workspace** button.
* **b.** Click the **Create AppBuilder Native Project** to create a new NativeScript project.
* **c.** Choose a **NativeScript Blank (JavaScript)** project.
* **d.** Name the project `Photo Album Native app` and click the green **Create Project** button.

<hr data-action="end" />

This newly created project is automatically checked into the integrated AppBuilder version control. AppBuilder opens your new project and lists all project files in the Project Navigator. There you can see the following items:

* **app** The app folder contains the entire app functionality.
* **bootstrap.js** This file contains the code that initializes your project as a native app.
* **app.js** The app.js module contains application specific code, such as which page is the starting page of the application. 
* **main.js** This is the JavaScript file used to implement the business logic of the first page.
* **main.xml** This is the file used to implement the UI of this first page.
* **App_Resources** The App_Resources folder contains application assets such as icons, splash screens and configuration files such as Info.plist and AndroidManifest.xml.
* **tns_modules** The tns_modules folder contains the NativeScript libraries for accessing the native device platform functionality. Libraries might consist of platform-specific and common files and an index.js that exports the module. Platform-specific files contain platform-specific NativeScript code. When you build your app for iOS or Android, AppBuilder automatically picks up the needed platform-specific files. 

We will focus on the `app` folder to create the logic of our application. You can also quickly check the `tns_modules` to get a better idea of what components the NativeScript framework offers.

### Step 2. Define the User Interface

We are now going to create a page containing a button and a listview. The button will be positioned at the bottom and the listview will fill the remaining screen area. This layout will be arranged by a grid layout panel. Use the XML file to declare the UI of the page.

<hr data-action="start" />

#### Action 

* **a.** Open the main.xml file and add a declaration for a simple page:
```
<Page>
</Page>
```
* **b.** Then create a GridPanel instance and set up its layout. The layout will consist of two rows - the first row will take all available space the GridPanel gives and the second row will be sized according to its content:
```
<GridPanel>                
	<GridPanel.rowDefinitions>
		<RowDefinition height="*" />
		<RowDefinition height="auto" />
	</GridPanel.rowDefinitions>  
</GridPanel>
```
* **c.** Now it’s time for the controls that we want to arrange with the GridPanel. The declarations of the controls should be placed right after the end tag of ```GridPanel.rowDefinitions```. The controls declaration are as simple as:
```
<ListView row="0" />         
<Button row="1"/>
```

The whole xml declaration looks like this:
```
<Page>
    <GridPanel>                
        <GridPanel.rowDefinitions>
            <RowDefinition height="*" />
            <RowDefinition height="auto" />
        </GridPanel.rowDefinitions>  
        <ListView row="0"/>
		<Button row="1"/>
    </GridPanel>
</Page>
```

<hr data-action="end" />

Regadless of the small amount of UI and settings that we have put on the page, I am sure you will want to try the result on a real device now. So, in the next step, let’s see how easy it is to deploy this native app.

### Step 3. Deploy your app on a device using the companion app

The AppBuilder NativeScript companion app makes it easy to test your app on real devices, without the need to manage SDKs or deal with complex provisioning options.

![iOS app store](images/native-script-app.png)

<hr data-action="start" />

#### Action

* **a.** Get your iOS or Android device out.
* **b**. Download and install the Telerik NativeScript companion app from your device's app store—i.e. the App Store for iOS users, Google Play for Android users.

[![iOS app store](images/app-store-icon.png)](https://itunes.apple.com/bg/app/nativescript/id882561588?mt=8)
[![Google Play](images/google-play-icon.png)](https://play.google.com/store/apps/details?id=com.telerik.NativeScript&hl=en)

* **c.** In the browser, select **Run** --> **Build**, select your device's platform (iOS/Android), choose **AppBuilder companion app**, and click Next. You will see a QR code pointing to the application. 
* **e.** **Android:** Open the Telerik NativeScript app on your device. With the NativeScript app opened, open the notification drawer. Tap the **Scan** button and use the integrated scanner to scan the QR code displayed in the browser.
* **f.** **iOS:** Open the Telerik Native Script app on your device and then use a two-finger swipe to reveal the companion app's menu. Click the **QR Scanner** option in the menu and use the integrated scanner to scan the QR code displayed in the browser.
![Using a two-finger swipe on your device](images/swipe.png)

* **e.** Windows Phone: Windows Phone companion app is not available at this time.

<hr data-action="end" />

When scanned, the QR code loads your app in the Telerik NativeScript companion app. You should see the button at the bottom and some white space taken from the data-empty list view. Now that you have the app on your device, let's make some changes to see how easy is to get application updates without rescanning the QR code.

<hr data-action="start" />

#### Action

* **a.** Set the `text` attribute in the Button declaration. The result should look like this:
`<Button text=”Test Message" row="1"/>`
* **b.** **Android:** With the NativeScript application opened, open the notification center and tap the **Sync** button. 
* **c.** **iOS:** On your device, within the companion app, tap with three fingers and hold until a popup appears. 

<hr data-action="end" />

Your updated app will be downloaded on the device and you will immediately see the change in the Button text.

This process is known as LiveSync, and it makes updating your apps as easy as a quick tap. Now that you have an app, and can test it on your device, let's fill the ListView with items.


### Step 4. Fill a ListView with images

Let’s now fill our ListView with items. For our demo purposes, we will add a few images to the project and will set them to be the items source of the ListView. The items source definition will be created in a view-model file and will be then consumed by the ListView from the main js/xml files.

If you have started following this tutorial from the ready-made Photo Album Native application, the three steps below have already been done for you.

<hr data-action="start" />

#### Action

* **a.** Right-click the `app` folder and choose **Add** --> **New File**. Name the file `view-model.js`. This is where the model will be defined.
* **b.** Right-click the `app` folder and choose **Add** --> **New Folder**. Name the folder `res`. This is where the images will be stored.
* **c.** Right-click the `res` folder and choose **Add** --> **Existing Files**. Browse to a few images on your machine and add them. 

<hr data-action="end" />

Now, it’s time to define the data source in the view model.

<hr data-action="start" />

#### Action

* **a.** Open the `view-model.js` file that we have just created and add the following declarations, that load appropriate modules from the `tns_modules` folder: 
```
var observable = require("data/observable");
var observableArrayModule = require("data/observable-array");
var imageSourceModule = require("image-source");
var fileSystemModule = require("file-system");
```
We need the first one in order for the changes that happen in the view-model to be reflected in the UI. The second we need for the collection where we are going to store the image objects. The other two we need in order to load the image files that we have just added to the project.
* **b.** Create an `ObservableArray()` and add the images there using the `push` method:
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
You can notice that two images are not added to the array. This is because we are going to add them later on a button click.
* **c.** Create the view-model implementation, let’s call it PhotoAlbumModel. The basic implementation of the model looks like this: 
```
var PhotoAlbumModel = (function (_super) {
    __extends(PhotoAlbumModel, _super);
    function PhotoAlbumModel() {
        _super.call(this);
    }
    return PhotoAlbumModel;
})(observable.Observable);
```

* **d.** As you can see, we are inheriting the PhotoAlbumModel from Observable(). This will allow us to bind controls’ properties from the main page to properties in the view model. In order to get all the functionality that Observable (or any other parent type) gives, we should add the following code before the PhotoAlbumModel implementation:
```
var __extends = this.__extends || function (d, b) {
    for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];
    function __() { this.constructor = d; }
    __.prototype = b.prototype;
    d.prototype = new __();
};
```
* **e.** In the PhotoAlbumCollection’s constructor add the following line:
```
this.set("message", "Add new images");
```

We will bind the button’s `text` property to the PhotoAlbumCollection’s message property later, and every change we apply to the `message` property will be reflected on the button’s `text`.

> Tip: Keyboard shortcut `Ctrl` + `Alt` + `F` cleans up indentation and formatting for you. Try using it after you paste in code throughout these lessons.

* **f.** Expose an `photoItems` property at the PhotoViewModel that returns the ObservableArray containing the images:
```
Object.defineProperty(PhotoAlbumModel.prototype, "photoItems", {
      get: function () {
          return array;
      },
      enumerable: true,
      configurable: true
  })
```
* **g.** At the bottom of the `view-model.js` declare the PhotoAlbumModel in the module `exports` to make this model accessible from the UI.
```
exports.PhotoAlbumModel = PhotoAlbumModel;
```
* **h.** With the view-model created, let’s get back to our main.js/xml to fill ListView with data.
Load the view-model specifying its location in the project folder structure and then create an instance of the PhotoAlbumModel.
```
var modelModule = require("./view-model");
var model = new modelModule.PhotoAlbumModel();
```
* **i.** Create a function called onPageLoaded. In this function we will set the bindingContext of the page to be our view-model:
```
function onPageLoaded(args) {
    var page = args.object;
    page.bindingContext = model;
}
```
* **j.** Set the exports.onPageLoaded at the end of main.js to make the onPageLoaded function accessible from the UI:
```
exports.onPageLoaded = onPageLoaded;
```
* **k.** In order to call the onPageLoaded method when the page loads, add the `loaded` attribute with value `onPageLoaded` in the Page tag in `main.xml`:
```
<Page loaded="onPageLoaded">
```
* **l.** We now should bind the ListView to the photoItems collection of the PhotoAlbumModel. To do this, add set the `items` attribute of the `ListView` tag to `{{ photoItems }}`:
```
<ListView items="{{ photoItems }}" row="0">
```
* **m.** The ListView is now bound to the collection of images. But in order to show the images, we should set the appropriate template consisting of a `GridPanel` with an `Image` object inside. Note that we are binding the Image source to the `itemImage` property of the `photoItems` objects, and that we are placing the template between the ListView start and end tags:
```
<ListView items="{{ photoItems }}" row="0" >
	<ListView.itemTemplate>                
		<GridPanel>
			<Image source="{{ itemImage }}" row="0"/>
		</GridPanel>               
	</ListView.itemTemplate>
</ListView>
```

<hr data-action="end" />

Now the ListView is filled with 6 images. If you use the LiveSync feature of the Telerik NativeScript companion app, you will see them. Let’s now add two more images on a button click and put some style in the button.


### Step 5. Respond to actions and put some style

The ListView is already filled with images. But let’s see how we can add more images on a button click and how we can put some style in the button.

<hr data-action="start" />

#### Action

* **a.** In the PhotoAlbumModel create a function called `tapAction`. In this function we add two more images to the `ObservableArray` instance. We also change the PhotoAlbumModel’s message property value.
```
PhotoAlbumModel.prototype.tapAction = function () {
	array.push(item7);
	array.push(item8);
        
	this.set("message", "Images added. Total images:" + array.length);
};
```
* **b.** In the `main.js` create a function that we will call then the button is tapped:
```
function buttonClick(args) {
    model.tapAction();
}
```
* **c.** At the bottom of the `main.js` declare the buttonClick function in the module exports to make it accessible from the UI.
```
exports.buttonClick = buttonClick;
```
* **d.** In the `main.xml` file, set the `click` attribute of the `Button` tag to `buttonClick`. This will call the `buttonClick` function when the button is tapped:
```
<Button click="buttonClick" row="1"/>
```
* **d.** In the `main.xml` file, set the `text` attribute of the `Button` tag to `{{ message }}`. This will bind the Button’s text property to the message property of the PhotoAlbumModel. So, initially, the Button will display `Add new images` and after we tap it, it will display `Images added. Total images: 8`.
```
<Button text="{{ message }}" click="buttonClick" row="1"/>
```

<hr data-action="end" />

Finally, let’s beautify the button. NativeScript uses standards based CSS syntax to style the UI elements.

<hr data-action="start">

#### Action

* **a.** Right-click the app folder and choose **Add** --> **New File**.
* **b.** Name the file after the file that contains the UI objects to be styled. In our case, the name should be `main.css`.
* **c.** In order to change the foreground color, background color and font properties of the `Button` type, set the following CSS:
```
Button {
    background-color: gray;
    font-size: 20;
    color: #3BB9FF; 
}
```
<hr data-action="end">

This is it! Now you know how to load data in a UI control, how to bind UI properties to a view-model and how define the style of the UI with CSS.

Use the LiveSync feature of the Telerik NativeScript application to update the application on the device and see the results.