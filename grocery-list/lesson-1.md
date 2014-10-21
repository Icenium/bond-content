## Lesson 1. Add data services to your app

Data is an important component of most applications. The Telerik Platform contains a Backend Services solution that makes it simple to configure and interact with a backend that lives in the cloud. In this lesson you'll create a backend structure for managing grocery lists. Within a few minutes you'll have a complete data service, a UI that reads and writes to it, a login screen, and even a screen for user registration. Let's get started.

### Step 1. Configure an endpoint

Before you interact with data you need to setup a structure for your data to live in. Although you're free to store your data wherever you'd like, the Telerik Platform provides a Backend Services offering that integrates with AppBuilder and Kendo UI Mobile. To start, you're going to create a Backend Services project to serve as the backend of your grocery list app.

<hr data-action="start" />

#### Action

* **a**. Click the green “Create Backend Services project” to create a new project.
* **b**. Give your project a name — for instance “Grocery List Backend” — and then click the green “Create project” button.
* **c**. Click the “Types” option in the menu on the left-hand side of the screen.
* **d**. Click the blue “Create a Content Type” button.
* **e**. Type “Groceries” in the “Type name” input box.
* **f**. Under “Add a field”, type “Name”, leave the dropdown set to “Text”, and then click the blue “Add” button.
* **g**. Click the green “Save” button.

<hr data-action="end" />

With this you now have a “Groceries” content type (which you can think of like a database table) with a “Name” field (which you can think of like a database column). You can add more complex data structures—relationships, dates, geopoints, and so forth—but for now we're sticking with a basic content type for simplicity. You can interact with this content type over a RESTful API, with one of the many SDKs that Backend Services provides, or manually through the Platform's UI. Before getting into the SDKs, let's add a few groceries to the list.

<hr data-action="start" />

#### Action

* **h**. Click the blue “Add an item” button.
* **i**. In the right pane, enter a grocery into the text box (e.g. Bananas, Apples, and so forth), and then press the green “Save” button.
* **j**. Repeat steps **h** and **i** so that you have at least 5 groceries in the list.

<hr data-action="end" />

Now you have a backend configured and some data ready to go. Next, let's see how easy it is to integrate this data into an app.

### Step 2. Read from an endpoint

The true power of Backend Services is how easy it makes complex data interactions. In this step you'll learn how to read data from the content type you just configured. A boilerplate AppBuilder project has already been setup for you, so you're next task is to add data to it. You'll start by opening up the AppBuilder project.

<hr data-action="start" />

#### Action

* **a**. Click the “Grocery List app” link in the upper-left corner of the screen to return to this project's workspace.
* **b**. Click the “Grocery List” box to enter the AppBuilder project for this tutorial.
* **c**. Run this project in the simulator by clicking **Run** --> **iPhone 5** in the menu at the top of the screen.

<hr data-action="end" />

As you can see, this project has a basic UI, but no data. If you look in this project's index.html you'll see a `<script>` tag that imports everlive.all.min.js, which is the Backend Services JavaScript SDK. Next, you're going to use the SDK to read the grocery data from your Backend Services_blank project. Your first step is to plug in your new project's API key.

<blockquote>Tip: You can also download the Backend Services SDK, or any other Telerik Platform resources you may need from <a href="https://platform.telerik.com/#downloads" target="_blank">https://platform.telerik.com/#downloads</a></blockquote>

<hr data-action="start" />

#### Action

* **d**. Find and click the “Data” link on the very right-hand side of the screen.
* **e**. In the Data Navigator (the panel on the right-hand side of the screen), expand “Backend Services”, and then right click on “Grocery List Backend” (or whatever you named your Backend Services project). Select “Properties” from the context menu.
* **f**. In the Properties menu (bottom right-hand side of the screen), find the project's API key and copy it.
* **g**. Click the “Project” link on the right-hand side of the screen to switch back to this project's files.
* **h**. Open app.js in the scripts folder.
* **g**. Replace the `YOUR API KEY` string with your project's API key that you copied to the clipboard.
* **h**. Paste the following code in the `initialize()` function (after the `var app = ...;` line).
```
$("#grocery-list").kendoMobileListView({
    dataSource: groceryDataSource,
    template: "#: Name #"
});
```
* **i**. Save app.js and open your project in the simulator. You should see the list of groceries you added in your Backend Services project.

<hr data-action="end" />

There are a couple cool things that happened here. First, notice how little code it takes to read from your backend. You don't even have to explicitly make a call to retrieve the data for the list. As soon as you use a Kendo UI DataSource in a Kendo UI widget—in this case a ListView—Kendo UI knows to go get the data you need.

The DataSource itself is an elegant way of interacting with any backend service you may have. Here, the `transport` option tells Kendo UI the URL location of this service and the data type to expect (JSON). The `schema` option tells Kendo UI where the data it needs is located in the JSON response. The DataSource expects an array, and in this case the array of data is in a `Result` member of the JSON result.

The cool part of using a Backend Services project is you didn't have to write a RESTful service to access your backend data—it's generated for you! But you're just getting started, as the Kendo UI DataSource makes all sorts of things possible. Next let's see how to post new data.

### Step 3. Post to an endpoint

A grocery list app isn't all that useful if you can't add to the list, so that's the behavior you're going to add next. As with reading data, you'll find that the Kendo UI DataSource makes interacting with backend data services easy. Let's start by building a few new UI components to collect data from the user.

<hr data-action="start" />

#### Action

* **a**. Open the project's index.html file, and add a button to the file's navbar. The entire navbar code should look like this:
```
<div data-role="navbar">
    <span data-role="view-title"></span>
    <a data-role="button" data-align="right" data-rel="modalview" href="#add">Add</a>
</div>
```
* **b**. Add the following view as a child of the `<body>` element. (It should come right before the `</body>` tag.)
```
<div data-role="modalview" id="add" style="width: 90%" data-model="addView">
    <div data-role="header">
        <div data-role="navbar">
            <span>Add Grocery</span>
        </div>
    </div>

    <ul data-role="listview" data-style="inset">
        <li>
            <label>
                Grocery:<input type="text" data-bind="value: grocery">
            </label>
        </li>
    </ul>

    <button data-bind="click: add" class="add-button" data-role="button">Add</button>
    <button data-bind="click: close" class="close-button" data-role="button">Close</button>
</div>
```
* **c**. Save index.html and open this app in the simulator.

<hr data-action="end" />

Notice how the `href` attribute of the new anchor matches the `id` attribute of the modalview you just added. This, and the anchor's `data-rel` attribute tells Kendo UI Mobile to open the dialog when you click the link in the header—no extra JavaScript is required. This gives you the UI you need, now you have to make tie this new dialog into your backend.

<hr data-action="start" />

#### Action

* **d**. In your app.js file, add the following code directly after the `initialize()` function:
```
window.addView = kendo.observable({
    add: function() {
        if (this.grocery.trim() === "") {
            navigator.notification.alert("Please provide a grocery.");
            return;
        }

        groceryDataSource.add({ Name: this.grocery });
        groceryDataSource.one("sync", this.close);
        groceryDataSource.sync();
    },
    close: function() {
        $("#add").data("kendoMobileModalView").close();
        this.grocery = "";
    }
});
```

<hr data-action="end" />

The last thing you need to do is configure the DataSource to create new items. You *could* add `create` configuration to the existing `transport` option, but for Backend Services projects Kendo UI offers a shortcut.

<hr data-action="start" />

#### Action

* **e**. Replace the existing declaration of `groceryDataSource` with the code below:
```
var groceryDataSource = new kendo.data.DataSource({
    type: "everlive",
    transport: {
        typeName: "Groceries"
    }
});
```
* **f**. Save your app.js and open the project in the simulator.

<hr data-action="end" />

When you use a `type` of `"everlive"`, Kendo UI takes care of most of the DataSource configuration for you. You still need to provide a `typeName` so Kendo UI knows which content type to request from the Backend Services project.

And with this change you now have a functioning grocery list. When you add groceries through the UI they're added to the backend and the list refreshes automatically. How does the list know to refresh? When you call the `sync()` method on a Kendo UI DataSource, it intelligently makes the calls needed to sync the backend data. All Kendo UI widgets that are driven by that data then know to refresh their views to reflect the updates.

Using the DataSource as an abstraction makes all sorts of things possible. For example, suppose you wanted to sort the groceries by their name? You can get that behavior with the `sort` option:

```
var groceryDataSource = new kendo.data.DataSource({
    type: "everlive",
    sort: { field: "Name", dir: "asc" },
    transport: {
        typeName: "Groceries"
    }
});
```

Now that you have a functioning grocery list, your next problem to tackle is user management, as your app's users probably don't want to all edit the same list. As it turns out, Backend Services offers an elegant solution to this problem as well.
