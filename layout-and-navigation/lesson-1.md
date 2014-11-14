## Lesson 1. Working with app layouts

### Step 1. Add common app layout

<hr data-action="start" />

When creating views with Kendo UI, you'll often find yourself using common elements like headers and footers throughout your app. With layouts, we can define those elements in a single place, and reuse them throughout.

#### Action

* **a**. In the index.html file, add the following markup just before the closing `<body>` tag:
```
<div data-role="layout" data-id="main-layout">
    <div data-role="header">Header</div>
    <div data-role="footer">Footer</div>
</div>
```

* **b**. Add a `data-title="Books"` attribute to the first view (`<div data-role="view">`).

* **c**. Remove the full header div (`<div data-role="header">`) from the first view and copy the NavBar into the div with `data-role="header"`. Your layout should now look like this:
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
* **g**. Open the iPhone 5 simulator (select **Run** --> **iPhone 5**) to see your new layout in action.

<hr data-action="end" />

With a common layout, we can now add multiple views to our app and ensure that the application shell remains consistent throughout. In order to see the benefit of our new layout, we need to add some basic navigation. Kendo UI provides two basic navigation paradigms out of the box, the TabStrip and the Drawer.

### Step 2. Add a TabStrip

The TabStrip, as the name indicates, provides a tab menu at the base or top of your app (depending on the mobile platform) that allows you to switch between multiple views. Let's add one to the layout we created in the last step. 

<hr data-action="start" />

#### Action

* **a**. In the index.html file, add initialization markup for the TabStrip in the `data-role="footer"` div of your Layout. The result should look like this:
```
<div data-role="layout" data-id="main-layout">
    <div data-role="header">
        <div data-role="navbar">
            <span data-role="view-title"></span>
        </div>
    </div>
    <div data-role="footer">
        <div data-role="tabstrip">

        </div>
    </div>
</div>
```

* **b**. Tabs in the TabStrip are simply anchor (`<a>`) elements, so let's add two of those now, inside the `data-role="tabstrip"` div:
```
<a href="#index" data-icon="home">Home</a>
<a href="views/about.html" data-icon="globe">About</a>
```

> Tip: The `data-icon` attribute allows you to specify the image that will accompany a tab or drawer item. You can learn more about icons, including how to create your own, in [this article](http://docs.telerik.com/kendo-ui/mobile/icons).

* **c**. Save the index.html file and re-open the iPhone 5 simulator. Click each tab and notice how the layout is preserved, even as the main content changes.
* **d**. Close the iPhone simulator and open an Android phone simulator. Notice how Kendo UI automatically adapts to an Android style by moving the TabStrip to the top of the screen.

<hr data-action="end" />

### Step 3. Switch to a Drawer

While the TabStrip has been a common navigation pattern since the early days of mobile, drawer navigation has gained in popularity in recent years. In mobile apps, the Drawer is a hidden navigation menu that slides into view from the right or left side of the screen based on user interaction, like clicking a button or swiping in from the edge of the screen.

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

* **b**. Now, you'll want to remove the TabStrip you added in the last step from your app layout. Go ahead and remove the footer element from your layout so that it looks like this:
```
<div data-role="layout" data-id="main-layout">
    <div data-role="header">
        <div data-role="navbar">
            <span data-role="view-title">
            </span>
        </div>
    </div>
</div>
```

<hr data-action="end" />

Users can activate the Drawer by swiping in from the left by default, but most apps also include a button or link so that users can explicitly invoke this feature. Let's add a button for our drawer next.

<hr data-action="start" />

* **c**. Add a button to invoke the drawer by adding the following markup inside of the `data-role="navbar"` div of your layout. (It can go before or after the `<span data-role="view-title">` element.)
```
<a href="#app-drawer" data-rel="drawer" data-role="button" data-align="left" data-icon="details"></a>
```
* **d**. Open up the iPhone simulator, click the Menu link in the header, and watch the drawer magically appear!

<hr data-action="end" />

Working with Layouts and navigation widgets is an essential first step to building a mobile app. Now that we've laid the foundation, let's explore these topics a bit more, starting with an overview of MVVM.