## Lesson 3. App navigation

### Step 7. Specifying app transitions

So far in this tutorial, you've learned how to work with layouts, how to leverage common navigation widgets like the TabStrip and Drawer, and how to utilize MVVM to quickly configure widgets and wire up events. In this lesson, we'll look at how to control transitions, and how to work with view navigations in your app.

<hr data-action="start" />

As you've worked through this tutorial so far, you may have noticed that Kendo UI provides some nice built-in transitions between views and when open and closing the Drawer menu. 

#### Action

* **a**. Open the app.js file, and add a transition property to the Kendo UI Application constructor so that it looks like this:
```
app = new kendo.mobile.Application(document.body, { layout: "main-layout", transition: "fade" });
```

* **b**. Open up the iPhone simulator and click a link or button. Notice the fade transition between pages.
* **c**. Now change the Application constructor to use a different transition, in this case "zoom." The code should now look like this:
```
app = new kendo.mobile.Application(document.body, { layout: "main-layout", transition: "zoom" });
```

* **d**. Now open the iPhone simulator back up and navigate within the app. Notice how the transition has changed from the last step?

<hr data-action="end" />

If you clicked on the drawer menu during simulation, you probably noticed that it continues to use a slide transition. Much like the Drawer does by default, Kendo UI allows us to set transitions at the view level, in addition to the app level.

<hr data-action="start" />

#### Action

* **e**. Open details.html, and add a `data-transition="fade"` property to the main view. It should now look like this:
```
<div data-role="view" data-title="Book Details" 
     data-show="BookDetail.show"
     data-hide="BookDetail.hide"
     data-layout="back-layout" data-reload="true" data-transition="fade">
```

* **f**. Open the iPhone simulator back up and click on one of the items in the ListView. Notice the fade transition, even as the rest of the app uses the app-level transiton you specificed above.

<hr data-action="end" />

### Step 8. Using simple navigation

Now let's look a little bit more at navigation. As with many other features in Kendo UI, you can work with navigation in one of two ways: in JavaScript code or declaratively in your markup. Let's look at both now.

<hr data-action="start" />

#### Action

* **a**. Open the index.html file and find the `main-layout` `<div>` that we defined back in the first lesson. Inside of the NavBar for that layout, add another button for a settings view:
```
<div data-role="layout" data-id="main-layout">
	<div data-role="header">
		<div data-role="navbar">
			<a href="#app-drawer" data-rel="drawer" data-role="button">Menu</a>
	    	<span data-role="view-title"></span>
	    	<a data-click="Books.settings" data-role="button" data-align="right" data-icon="settings"></a>
	    </div>
	</div>
</div>
```

* **b**. Open the app.js file and, inside of the empty settings function, add the following navigation code:
```
app.navigate('views/settings.html');
```

* **c**. Open the iPhone simulator and click the settings button to see your navigation code in action. 

<hr data-action="end" />

As noted above, Kendo UI allows you to perform navigation with code or declaratively, primarially though the `href` attribute on links and the use of urls or fragments starting with the pound or hash character (#).

<hr data-action="start" />

* **d**. In the index.html file, remove the `data-click` attribute from the settings button and replace it with an `href` attribute so that the link looks like this:
```
<a href="views/settings.html" data-role="button" data-align="right" data-icon="settings"></a>
```
* **e**. Reload the iPhone simulator and click the settings button again. Things should still work just fine, and you've saved yourself a few lines of JavaScript to boot!

<hr data-action="end" />

Great job! In just a few minutes, you've mastered some of the critical basics to building hybrid apps with Kendo UI Mobile and the Telerik Platform. With these skills in hand, creating the navigation, layout and major views in your next mobile app should be a breeze!
