## Lesson 1. Working with app layouts

### Step 1. Add common app layout

<hr data-action="start" />

When creating views with Kendo UI, you'll often find yourself using common elements like headers and footers throughout your app. With layouts, we can define those elements in a single place, and reuse them throughout.

**Tip**: The default CSS rules provided in Kendo UI Mobile are included in the **kendo.mobile.all.min.css** file.

#### Action

* **a**. In the index.html file, add the following markup just before the closing `</body>` tag:
```
<div data-role="layout" data-id="main-layout">
    <div data-role="header">Header</div>
    <div data-role="footer">Footer</div>
</div>
```

* **b**. Add a `data-title="Books"` attribute to the first view (`<div data-role="view">`).

* **c**. Cut the whole header div (`<div data-role="header">`) from the 'index' view and replace with it the `data-role="header"` in the view you just created. Your layout should now look like this:
```
<div data-role="layout" 
    data-id="main-layout">
    <div data-role="header">
        <div data-role="navbar">
            <span data-role="view-title">
            </span>
        </div>
    </div>
    <div data-role="footer"></div>
</div>
``` 

* **d**. Save the index.html file.

* **e**. Open the app.js file in the `js` folder and modify the Kendo UI application constructor to include your layout:
```
app = new kendo.mobile.Application(document.body, { layout: "main-layout" });
```

* **f**. Save the app.js file.
* **g**. To see your new layout in action, select **Run** &#8594; **Open Simulator** and then select any iPhone from the **iOS Devices** section in the drop-down menu located in the top-left corner. Please note the defined layout will appear specific to the particular platform. 

**Tip**: Depending on the project requirements, the mobile application may be styled in several different ways. Check out the documentation on [Kendo UI Mobile CSS Dependencies](http://docs.telerik.com/kendo-ui/mobile/styling#kendo-ui-mobile-css-dependencies) for full configuration options on styling in different ways.

<hr data-action="end" />

With a common layout, we can now add multiple views to our app and ensure that the application shell remains consistent throughout. In order to see the benefit of our new layout, we need to add some basic navigation. Kendo UI provides two basic navigation paradigms out of the box, the TabStrip and the Drawer.

### Step 2. Add a Drawer

In mobile apps, the Drawer is a hidden navigation menu that slides into view from the right or left side of the screen based on user interaction, like clicking a button or swiping in from the edge of the screen.

<hr data-action="start" />

#### Action

* **a**. To add a Drawer widget, add the following markup to the index.html file before the closing `<body>` tag:
```
<div data-role="drawer" id="app-drawer">
    <div data-role="header">
        <div data-role="navbar">
            <span data-role="view-title">Menu</span>
        </div>
    </div>

    <ul data-role="listview">
        <li><a href="#index" data-icon="home">Home</a></li>
        <li><a href="views/favorites.html" data-icon="favorites">Favorites</a></li>
		<li><a href="views/about.html" data-icon="globe">About</a></li>
    </ul>
</div>
```

<hr data-action="end" />

Users can activate the Drawer by swiping in from the left by default, but most apps also include a button or link so that users can explicitly invoke this feature. Let's add a button for our drawer next.

<hr data-action="start" />

* **b**. Add a button to invoke the drawer by adding the following markup inside of the `data-role="navbar"` div of your layout. (It can go before or after the `<span data-role="view-title">` element.)
```
<a href="#app-drawer" data-rel="drawer" data-role="button" data-align="left" data-icon="details"></a>
```
* **c**. Open up the iPhone simulator, click the Menu link in the header, and watch the drawer magically appear!

<hr data-action="end" />

### Step 3. Switch to a TabStrip

The TabStrip, as the name indicates, provides a tab menu at the base or top of your app (depending on the mobile platform) that allows you to switch between multiple views. Let's add one to the layout we created in the first step. 

<hr data-action="start" />

#### Action

* **a**. In the index.html file, add initialization markup for the TabStrip in the `data-role="footer"` div of your Layout. The result should look like this:
```
<div data-role="layout" data-id="main-layout">
    <div data-role="header">
        <div data-role="navbar">
            <span data-role="view-title"></span>
            <a href="#app-drawer" data-rel="drawer" data-role="button" data-align="left" data-icon="details"></a>
        </div>
    </div>
    <div data-role="footer">
        <div data-role="tabstrip">

        </div>
    </div>
</div>
```

* **b**. Tabs in the TabStrip are simply anchor (`<a>`) elements, so let's add three of those now, inside the `data-role="tabstrip"` div:
```
<a href="#index" data-icon="home">Home</a>
<a href="views/favorites.html" data-icon="favorites">Favorites</a>
<a href="views/about.html" data-icon="globe">About</a>
```

**Tip**: The `data-icon` attribute allows you to specify the image that will accompany a tab or drawer item. You can learn more about icons, including how to create your own, in [this article](http://docs.telerik.com/kendo-ui/mobile/icons).

* **c**. Now, you'll want to remove the Drawer you added in the last step. Delete the button that invokes the drawer (`<a href="#app-drawer" data-rel="drawer" data-role="button" data-align="left" data-icon="details"></a>`) and the entire Drawer div (`<div data-role="drawer" id="app-drawer">`) from your index.html file.
* **d**. Save the index.html file and re-open the iPhone simulator. Click each tab and notice how the layout is preserved, even as the main content changes.
* **e**. Switch to Android phone simulator by selecting it in the drop-down menu located in the top-left corner. Notice how Kendo UI automatically adapts to an Android style by moving the TabStrip to the top of the screen.


<hr data-action="end" />

Working with Layouts and navigation widgets is an essential first step to building a mobile app. Now that we've laid the foundation, let's explore these topics a bit more, starting with an overview of MVVM.
