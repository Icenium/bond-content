## Lesson 1. Build a photo album

Throughout this QuickStart tutorial, you will learn how to load data in a UI control, how to bind UI properties to a view-model and how to define the style of the UI with CSS. As a result you will have a native Photo Album application written all in xml and JavaScript.

### Step 1. Create a new project. Project structure.

We are now going to create our first NativeScript project. The fours steps below do just that. You can skip them and directly open the Photo Album Native workspace and then Photo Album Native project.

<hr data-action="start" />

#### Action

* **a.** Click the Create Workspace button. Name your workspace “Photo Album Native app”
* **b.** Create a new AppBuilder Native project
* **c.** Choose a NativeScript Blank (JavaScript) project
* **d.** Name the project Photo Album Native UI app

<hr data-action="end" />

This newly created project is automatically checked into the integrated AppBuilder version control. AppBuilder opens your new project and lists all project files in the Project Navigator. There you can see the following items:

* **app** The app folder contains the entire app functionality.
* **bootstrap.js** This file contains the code that initializes your project as a native app.
* **app.js** The app.js module contains application specific code. Here is the place where you can set application-specific code, such as which page is the starting page of the application. 
* **main.js** This is the first JavaScript file used to implement the business logic.
* **main.xml** This is the file used to implement the UI of this first page.
* **App_Resources** The App_Resources folder contains application assets such as icons, splash screens and configuration files such as Info.plist and AndroidManifest.xml.
* **tns_modules** The tns_modules folder contains the NativeScript libraries for accessing the native device platform functionality. Libraries might consist of platform-specific and common files and an index.js that exports the module. Platform-specific files contain platform-specific NativeScript code. When you build your app for iOS or Android, AppBuilder automatically picks up the needed platform-specific files. 

We will focus on the app folder to create the logic of our application. You can also quickly check the tns_modules to get a better idea of what components the NativeScript framework offers.

### Step 2. Define the User Interface

We are now going to create a page containing a button and a listview. The button will be positioned at the bottom and the listview will fill the remaining screen area. This layout will be arranged by a grid layout panel. Use the XML file to declare the UI of the page.

<hr data-action="start" />

#### Action 

// TODO - fix these steps
* **a.** Right-click the Photo Album Native project from the project navigation and choose **Add** --> **New File**. This will be our xml file containing the controls and layout declarations.
* **a.** Name the file after its corresponding JavaScript file. It the current case, the JavaScript file is the main application file named `main.js`, so we should name our xml file `main.xml`.
* **c.** We should start our xml declaration with a simple page:
```<Page>
</Page>
```
* **d.** Then, we should create a GridPanel instance and set up its layout. The layout will consist of two rows - the first row will take all available space the GridPanel gives and the second row will be sized according to its content:
```
<GridPanel>                
	<GridPanel.rowDefinitions>
		<RowDefinition height="*" />
		<RowDefinition height="auto" />
	</GridPanel.rowDefinitions>  
</GridPanel>
```
* **e.** Now it’s time for the controls that we want to arrange with the GridPanel. The declarations of the controls should be placed right after the enclosing tag for the GridPanel.rowDefinitions. The controls declaration are as simple as:
```
<ListView row="0" />         
<Button row="1"/>
```

The whole xml declaration looks like this:
```
// remove this first XML line.
<?xml version="1.0" encoding="UTF-8" ?>
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

* **c.** In the browser, select **Run** --> **Build**, select your device's platform (iOS/Android), choose "AppBuilder companion app", and click Next. You will see a QR code pointing to the application. 
* **e.** **Android:** Open the Telerik NativeScript app on your device. With the NativeScript app opened, open the notification drawer. Tap the Scan button and use the integrated scanner to scan the QR code displayed in the browser.
* **f.** **iOS:** Open the Telerik Native Script app on your device and then use a two-finger swipe to reveal the companion app's menu. Click the “QR Scanner” option in the menu and use the integrated scanner to scan the QR code displayed in the browser.
![Using a two-finger swipe on your device](images/swipe.png)

* **e.** Windows Phone: Windows Phone companion app is not available at this time.

<hr data-action="end" />

When scanned, the QR code loads your app in the Telerik NativeScript companion app. You should see the button at the bottom and some white space taken from the data-empty list view. Now that you have the app on your device, let's make some changes to see how easy is to get application updates without rescanning the QR code.

<hr data-action="start" />

#### Action

* **a.** Set the text attribute in the Button declaration. The result should look like this:
`<Button text=”Test Message" row="1"/>`
* **b.** **Android:** With the NativeScript application opened, open the notification drawer and tap the Sync button. 
* **c.** **iOS:** On your device, within the companion app, tap with three fingers and hold until a popup appears. 

<hr data-action="end" />

Your updated app will be downloaded on the device and you will immediately see the change in the Button text.

This process is known as LiveSync, and it makes updating your apps as easy as a quick tap. Now that you have an app, and can test it on your device, let's fill the ListView with items.


### Step 4. Fill a ListView with images, respond to events and put some style

Let’s first fill our ListView with items. For our demo purposes, we will add a few images to the project and will set them to be the items source of the ListView. The items source definition will be created in a view-model file and will be then consumed by the ListView from the main js/xml files.

If you have started following this tutorial from the Photo Album Native application the three steps below have already been done for you.

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
var imageSourceModule = require("image-source");
var fileSystemModule = require("file-system");
```
We need the first one in order for the changes that happen in the view-model to be reflected in the UI. The other two we need in order to load the image files that we have just added to the project. * * **b.** Create an ObservableArray() and add the images there using the push method:
```
var array = new observableArrayModule.ObservableArray();

var directory = "/res/"
var img1 = imageSourceModule.fromFile(fileSystemModule.path.join(__dirname, directory + "01.jpg"));
var img2 = imageSourceModule.fromFile(fileSystemModule.path.join(__dirname, directory + "02.jpg"));
var img3 = imageSourceModule.fromFile(fileSystemModule.path.join(__dirname, directory + "03.jpg"));
var img4 = imageSourceModule.fromFile(fileSystemModule.path.join(__dirname, directory + "04.jpg"));
var img5 = imageSourceModule.fromFile(fileSystemModule.path.join(__dirname, directory + "05.jpg"));
var img6 = imageSourceModule.fromFile(fileSystemModule.path.join(__dirname, directory + "06.jpg"));

array.push([img1, img2, img3, img4, img5, img6]);

var img7 = imageSourceModule.fromFile(fileSystemModule.path.join(__dirname, directory + "07.jpg"));
var img8 = imageSourceModule.fromFile(fileSystemModule.path.join(__dirname, directory + "08.jpg"));
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
'this.set("message", "Added new images");'

We will bind the button’s text property to the PhotoAlbumCollection’s message property later, and every change we apply to the message property will be reflected on the button’s text.

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
* **g.** Finally, we should not forget to declare the PhotoAlbumModel in the module exports, to make this model accessible from the UI.
`exports.PhotoAlbumModel = PhotoAlbumModel;`
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
`exports.onPageLoaded = onPageLoaded;`
* **k.** In order to call the onPageLoaded method when the page loads, add the loaded attribute with value "onPageLoaded" in the Page tag in `main.xml`:
`<Page loaded="onPageLoaded">`
* **l.** We now should bind the ListView to the photoItems collection of the PhotoAlbumModel. To do this, add set the `items` attribute of the `ListView` tag to `{{ photoItems }}`. 
```<ListView items="{{ photoItems }}" itemLoading="listViewItemLoading" row="0">```
* **m.** The ListView is not bound to the collection of images. But in order to show the images, we should set the appropriate template consisting of a `GridPanel` with an `Image` object inside:
```
<ListView.itemTemplate>                
	<GridPanel>
		<Image/>
	</GridPanel>               
</ListView.itemTemplate>
```
* **n.** In order to give the Image object an image from `photoItems` to show, we can handle the `itemLoading` event which is executed whenever and a `ListView` item is going to be shown. Here is how this function can be realized in `main.js`:
```
/// TODO: see if we can bind the image source
function listViewItemLoading(args) {
    var image = args.view._children[0];
    image.source = args.object.items.getItem(args.index);
}
```
* **o.** Set the `exports.listViewItemLoading` to make the `listViewItemLoading` function accessible from the UI:
`exports.listViewItemLoading = listViewItemLoading;`
* **p.** In the `main.xml` file we need to attach the `listViewItemLoading` function to the itemLoading event of the `ListView`. To do so, set the itemLoading attribute to listViewItemLoading in the `ListView` tag. The result should be: 
```
<ListView items="{{ photoItems }}" itemLoading="listViewItemLoading" row="0">
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
	array.push(img7);
	array.push(img8);
        
	this.set("message", "Images added");
};
```
* **b.** In the `main.js` create a function that we will call then the button is tapped:
```
function buttonClick(args) {
    model.tapAction();
}
```
* **c.** In the `main.xml` file, set the `click` attribute of the `Button` tag to `buttonClick`. This will call the `buttonClick` function when the button is tapped:
```
<Button click="buttonClick" row="1"/>
```
* **d.** In the `main.xml` file, set the `text` attribute of the `Button` tag to `{{ message }}`. This will bind the Button’s text property to the message property of the PhotoAlbumModel. So, initially, the Button will display `Add new images` and after we tap it, it will display `Images added. Total images: 8`.
```
<Button text="{{ message }}" click="buttonClick" row="1"/>
```

<hr data-action="end" />

Finally, let’s beautify the button. NativeScript uses standards based CSS syntax to style the UI elements.

<hr data-action="start>

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
<hr data-action="end>

This is it! Now you know how to load data in a UI control, how to bind UI properties to a view-model and how define the style of the UI with CSS.

Use the LiveSync feature of the Telerik NativeScript application to update the application on the device and see the results.