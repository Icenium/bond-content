## Lesson 2. Understanding MVVM

### Step 4. Working with simple bindings

In the last lesson, you were able to add a good amount of structure and interactivity to your app without having to write much JavaScript. This is because Kendo UI allows you to use the MVVM pattern to declaratively (meaning, using HTML markup instead of JavaScript code) wire up your apps.

In this lesson, we'll look at how Kendo UI allows you to leverage a declarative style for a variety of common tasks, like defining layouts, binding to data sources and wiring up events.

<hr data-action="start" />

Let's start by using MVVM to define a layout for certain views. In the last lesson, we defined a master layout for all of our views in the Application constructor. You can also define layout by using data attributes that Kendo UI will interpret at execution time.

#### Action

* **a**. In the index.html file, add a new layout just before the closing `<body>` tag with the following markup:
```
<div data-role="layout" data-id="back-layout">
    <div data-role="header">
        <div data-role="navbar">
            <a data-role="button" data-align="left" data-icon="rewind">Back</a>
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
<div data-role="view" data-title="Book Details" data-reload="true" data-layout="back-layout">
```

* **c**. Open up the iPhone simulator and navigate to the details view by clicking on one of the items in the list. Notice that, instead of a drawer menu button, we now have a nice back button in its place. In the next step, we'll wire this button up using, you guessed it, MVVM.

<hr data-action="end" />

### Step 5. Binding to events

To make this back button functional, we need to define an event and tell Kendo UI to call this event when the button is clicked. We'll start by defining our click event.

<hr data-action="start" />

#### Action

* **a**. Open the app.js file and add the following JavaScript to the empty 'back' function already defined:
```
app.navigate("#:back");
```

* **b**. Now, open index.html, find the new layout you created earlier in this lesson, and add a `data-click` attribute that points to the function you just created. The layout should now look like this:
```
<div data-role="layout" data-id="back-layout">
    <div data-role="header">
        <div data-role="navbar">
            <a data-click="Books.back" data-role="button" data-align="left" data-icon="rewind">Back</a>
            <span data-role="view-title"></span>
        </div>
    </div>
</div>
```

* **c**. Save the index.html and app.js files and re-open the iPhone simulator. Navigate back to the details view by clicking on a book in the list on the home screen and click on the back button, which now takes you back to the main screen.

<hr data-action="end" />

MVVM can be used to declaratively wire-up every event supported by a Kendo UI widget, which saves you from having to create your own event listeners in code. 

At this point, we have an empty details screen, which isn't very useful, so let's use a bit more data binding to show more information about a selected book.

### Step 6. Binding to data

Another big time-saver for MVVM comes when working with data. For our current app, you've probably noticed that we've started with a ListView with data on the initial screen. In this step, we'll use MVVM-style data binding to show details for a single selected record.

<hr data-action="start" />

#### Action

* **a**. Open the app.js file in the js folder. Notice that we have a Kendo UI DataSource already defined, and that this source points to a JSON file local to the project. Normally, we'd be working with remote data, but we'll keep things simple for now.
* **b**. Open the details.html file and replace its contents with the following code:
```
<div data-role="view" data-title="Book Details" 
     data-show="BookDetail.show"
     data-hide="BookDetail.hide"
     data-layout="back-layout" data-reload="true">
</div>
```

<hr data-action="end" />

The `data-show` and `data-hide` attributes tell Kendo UI which methods to call when the user navigates to or away from the view. We're using these methods in order to work with our shared Kendo UI DataSource and filter by only the record we want to display. Now let's add the meat of the view.

<hr data-action="start" />

#### Action

* **c**. Add the following markup to the main `<div>` of details.html:
```
<div id="bookContent">
    <div class="title" data-bind="text: title"></div>
    <div class="bigImage">
        <img data-bind="attr: { src: image_url }" />
    </div>
    <div>
        <label for="favorite">Favorite?</label>
        <input id="favorite" type="checkbox" data-role="switch" data-checked=false />
    </div>
    <div>
        <p><a data-role="button" id="amazonLink" data-click="BookDetail.openLink">Order from Amazon</a></p>
    </div>
</div>
```

<hr data-action="end" />

In the snippet above, you'll notice the use of `data-bind` in a couple of places. This is Kendo UI's way to allowing the developer to specify values—at runtime—for widgets, forms and other UI elements. With this syntax, we're telling Kendo UI to set the text of a `<div>` and the source of an image based on values from this view's model.

Now that we've defined the view and our bindings, let's configure the model and wire up our details page.

<hr data-action="start" />

#### Action

* **d**. Open the details.js file and add the following to the end of the `show` function:
```
// Create a model for the page and bind it to the view
var book = {
  title: currentBook.name + " by " + currentBook.author,
  image_url: currentBook.image_url,
  amazon_url: currentBook.amazon_url,
  is_favorite: currentBook.is_favorite
};
kendo.bind($('#bookContent'), book, kendo.mobile.ui);
// If the current book is a favorited item, toggle the switch on the view
if (currentBook.is_favorite) {
  $('#favorite').data('kendoMobileSwitch').toggle();
}
```

<hr data-action="end" />

The code before this snippet provides a filtered view of our Books DataSource based on the selected item. With that in hand, we create a model for the book and call `kendo.bind` to wire up the declarative markup we added to the view in the last step.

<hr data-action="start" />

#### Action

* **e**. Save the details.html and details.js files and re-open the iPhone simulator. Navigate to the details page by clicking on an item in the list and you should see the book details on the screen.

<hr data-action="end" />

MVVM is a powerful pattern, and is particularly useful when building mobile apps. Instead of littering your JavaScript with a lot of plumbing code, Kendo UI MVVM allows you to tie this code declaratively to your views. The end result is cleaner code, focused on app data and logic. 

With layouts and MVVM in hand, let's turn to the final pillar in the mobile app basics trio: app navigation.
