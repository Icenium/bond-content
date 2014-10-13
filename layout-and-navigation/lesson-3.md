## Lesson 3: App navigation and routing

### Step 1: Specifying app transitions

So far in this tutorial, you've learned how to work with layouts, how to leverage common navigation widgets like the TabStrip and Drawer, and how to utilize MVVM to quickly configure widgets and wire up events. In this lesson, we'll look at how to control transitions, and how to work with view navigations in your app.

<hr data-action="start" />

As you've worked through this tutorial so fare

#### Action

* **a**. Open the app.js file, and add a transition property to the Kendo UI Application constructor so that it looks like this:
```
new kendo.mobile.Application(body, { transition: "slide" });
```

* **b**. Open up the iPhone simulator and click a link or button. Notice the transition between pages.
* **c**. Now change the Application constructor to use a different transition, in this case "zoom." The code should now look like this:
```
new kendo.mobile.Application(body, { transition: "zoom" });
```

* **d**. Now open the iPhone simulator back up and navigate within the app. Notice how the transition has changed from the last step?

<hr data-action="end" />

If you clicked on the drawer menu during simulation, you probably noticed that it also used the new navigation set in the lest few steps. Users typically expect drawer menus to slide out on activation, so this change is less than ideal. Thankfully, Kendo UI allows us to set transitions at the view level, in addition to the app level.

<hr data-action="start" />

#### Action

* **e**. Open index.html, find the `back-layout` layout that you created in the last lesson, and add a `data-transition="slide"` property to the link for the back button. It should now look like this:
```
<a data-role="button" data-transition="slide">Back</a>
```

* **f**. Open the iPhone simulator back up and click the back button. Notice that the drawer slides out when activated, even as the rest of the app uses the app-level transiton you specificed above.

<hr data-action="end" />

### Step 2: Using simple navigation

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
	    	<a data-click="app.settings" data-role="button">Settings</a>
	    </div>
	</div>
</div>
```

* **b**. Open the app.js file and, inside of the empty settings function, add the following navigation code:
```
CODE app.navigate()
```

* **c**. Open the iPhone simulator and click the settings button to see your navigation code in action. 

<hr data-action="end" />

As noted above, Kendo UI allows you to perform navigation with code or declaratively, primarially though the `href` attribute on links and the use of url fragments starting with the pound or hash character (#).

<hr data-action="start" />

* **d**. In the index.html file, remove the `data-click` attribute from the settings button and replace it with an `href` attribute so that the link looks like this:
```
<a href="#settings" data-role="button">Settings</a>
```
* **e**. Reload the iPhone simulator and click the settings button again. Things should still work just fine, and you've saved yourself a few lines of JavaScript to boot!

<hr data-action="end" />

Great job! In just a few minutes, you've mastered some of the critical basics to building hybrid apps with Kendo UI Mobile and the Telerik Platform. With these skills in hand, creating the navigation, layout and major views in your next mobile app should be a breeze!
