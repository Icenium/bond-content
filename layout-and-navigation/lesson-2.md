## Lesson 2. Understanding MVVM

### Step 4. Working with simple bindings

In the last lesson, you may have noticed that you were able to add a good bit of structure and interactivity to your app without having to write much JavaScript. This is because Kendo UI allows you to use the MVVM pattern to declaratively (meaning, using HTML markup instead of JavaScript code) wire up your apps.

In this lesson, we'll look at how Kendo UI allows you to leverage a declaritive style for a variety of common tasks, including defining layouts, binding to data sources and wiring up events.

<hr data-action="start" />

Let's start by using MVVM to define a layout for certain views. In the last lesson, we defined a master layout for all of our views in the Application constructor. You can also define layout by using data attributes that Kendo UI will interpret at execution time.

#### Action

* **a**. In the index.html file, add a new layout just before the closing `<body>` tag with the following markup:
```
<div data-role="layout" data-id="back-layout">
	<div data-role="header">
		<div data-role="navbar">
	    	<a data-role="button">Back</a>
	    	<span data-role="view-title"></span>
	    </div>
	</div>
</div>
```

<hr data-action="end" />

Notice the liberal use of `data-` attributes in the snippet above. These attributes form the backbone of Kendo UI's MVVM implementation and can be used extensively throughout your mobile applications.

> Tip: Nearly every property and event for a Kendo UI widget can be set in JavaScript or using MVVM `data-` attributes. Check out the [Kendo UI Mobile documentation](http://docs.telerik.com/kendo-ui/mobile/mvvm) for more information.

Let's use this new layout in one of our existing views.

<hr data-action="start" />

#### Action

* **b**. Open the details.html file in the views folder, and add a `data-layout` attribute that points to `back-layout` to the view's main `<div>`. The view should now look like this:
```
CODE
```

* **c**. Open up the iPhone simulator and navigate to the details view by clicking on one of the items in the list. Notice that, instead of a drawer menu button, we now have a nice back button in its place. In the next step, we'll wire this button up using, you guessed it, MVVM.

<hr data-action="end" />

### Step 5. Binding to events

To make this back button functional, we need to define an event and tell Kendo UI to call this event when the button is clicked. We'll start by defining our click event.

<hr data-action="start" />

#### Action

* **a**. Open the app.js file and add the following JavaScript to the empty 'back' function already defined:
```
CODE
```

* **b**. Now, open index.html, find the new layout you created earlier in this lesson, and add a `data-click` attribute that points to the function you just created. The layout should now look like this:
```
<div data-role="layout" data-id="back-layout">
	<div data-role="header">
		<div data-role="navbar">
	    	<a data-click="app.back" data-role="button">Back</a>
	    	<span data-role="view-title"></span>
	    </div>
	</div>
</div>
```

* **c**. Save the index.html and app.js files and re-open the iPhone simulator. Navigate back to the details view and click on the back button, which should take you right back to the main screen.

<hr data-action="end" />

MVVM can be used to declaratively wire-up every event supported by a Kendo UI widget, which saves you from having to create your own event listeners in code.

### Step 6. Binding to data

Another big time-saver for MVVM comes when working with data, which we'll look at now. For our current app, you've probably noticed that we've started with a ListView with data on the initial screen. We'll add a second ListView to work with a related data set in this step.

<hr data-action="start" />

#### Action

* **a**. Open the favorites.js file in the js folder. Notice that we have a Kendo UI DataSource already defined, and that this source points to a JSON file local to the project. Normally, we'd be working with remote data, but we'll keep things simple for now.
* **b**. Open the favorites.html file and add the following markup inside of the main `<div>` for the view:
```
CODE
```

<hr data-action="end" />

Now that we have a ListView, let's do some declarative binding.

<hr data-action="start" />

#### Action

* **c**. Open favorites.html file and add a `data-source` attribute to the ListView `<div>` defined in the last step. You'll point this attribute to the `favoritesData` property in the favorites.js model. Your ListView should now look like this:
```
CODE
```

* **d**. TODO: additional action using `data-bind` to set the text of a view element [Maybe a status bar at the top with a loading message].
* **e**. Save the favorites.html and favorites.js files and re-open, the iPhone simulator and navigate to the Favorites page from the Drawer menu.

<hr data-action="end" />

MVVM is a powerful pattern, and is particularly useful when building mobile apps. Instead of littering your JavaScript with a lot of plumbing code, Kendo UI MVVM allows you to tie this code declararively to your views. The end result is cleaner code, focused on app data and logic. 

With layouts and MVVM in hand, let's turn to the final pillar in the mobile app basics trio: app navigation.