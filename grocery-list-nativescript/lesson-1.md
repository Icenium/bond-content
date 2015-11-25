## Lesson 1. Add data services to your app

### Step 1. Configure an endpoint

Data is an important component of most applications. The Telerik Platform contains a Backend Services solution that makes it simple to configure and interact with a backend that lives in the cloud. In this lesson you'll create a backend structure for managing grocery lists. Within a few minutes you'll have a complete data service, a UI that reads and writes to it, a login screen, and even a screen for user registration. Let's get started.

Before you interact with data you need to setup a structure for your data to live in. Although you're free to store your data wherever you'd like, the Telerik Platform provides a Backend Services offering that integrates with AppBuilder and NativeScript apps. To start, you're going to create a Backend Services project to serve as the backend of your grocery list app.

<hr data-action="start" />

#### Action

* **a**. Click the “Data” tab on the left-hand side of the screen.
* **b**. Click the blue “Enable Data” button.
* **c**. Click the “Create a Content Type” button.
* **d**. Type “Groceries” in the “Type name” input box.
* **e**. Under “Add a field”, type “Name”, leave the dropdown set to “Text”, and then click the “Add” button.
* **f**. Click the blue “Save” button.

<hr data-action="end" />

With this you now have a “Groceries” content type (which you can think of like a database table) with a “Name” field (which you can think of like a database column). You can add more complex data structures—relationships, dates, geopoints, and so forth—but for now we're sticking with a basic content type for simplicity. You can interact with this content type over a RESTful API, with one of the many SDKs that Backend Services provides, or manually through the Platform's UI. Before getting into the SDKs, let's add a few groceries to the list.

<hr data-action="start" />

#### Action

* **g**. Click the “Add an item” button.
* **h**. In the right pane, enter a grocery into the text box (e.g. Bananas, Apples, and so forth), and then press the blue “Save” button.
* **i**. Repeat steps **g** and **h** so that you have at least 5 groceries in the list.

<hr data-action="end" />

Now you have a backend configured and some data ready to go. Next, let's see how easy it is to integrate this data into an app.

### Step 2. Read from an endpoint

The true power of the Telerik Platform’s Backend Services is how easy it makes complex data interactions. In this step you'll learn how to read data from the content type you just configured. Some boilerplate code has already been setup for you, so your next task is to connect the app to the backend you just setup.

**Note**: This step requires you to have the NativeScript companion app installed on your device and to use LiveSync to push code updates. If you haven't already, complete the NativeScript quick start tutorial for a guided walkthrough of these features. You can access the NativeScript quick start tutorial by clicking the **Getting started** menu on the top of the screen, and then selecting **Interactive tutorials**.

<hr data-action="start" />

#### Action

* **a**. Click the “Code” tab on the left-hand side of the screen.
* **b**. Click the **Run** menu and then select **Build**. Choose your device's platform (iOS, Android, or Windows Phone), make sure the “AppBuilder companion app” radio button is selected, and then click **Next**.
* **c**. Scan the resulting QR code using your device's QR reader to open the app in the NativeScript companion app.

<hr data-action="end" />

As you can see, in its current state, this project has a basic list UI but no data. Your next step is to use NativeScript to read the grocery data from your backend.

<hr data-action="start" />

#### Action

* **d**. Click the “Settings” tab on the left-hand side of the screen.
* **e**. Copy the App ID that appears under the “App ID” heading to your clipboard.
* **f**. Click the “Code” tab to return to your app’s code.
* **g**. Open config.js in the app/shared/models folder.
* **h**. Replace the `API KEY` string with your project's API key that you copied to the clipboard.
* **i**. Open app/views/list.js. This is the file that controls the list page that the app opens into.
* **j**. Paste the following function at the end of list.js file:
```
function loadGroceries() {
    httpModule.getJSON({
        url: "http://api.everlive.com/v1/" + config.apiKey + "/Groceries",
        method: "GET"
    }).then(function(result) {
        result.Result.forEach(function(grocery) {
            groceries.push({ name: grocery.Name });
        });
    });
};
```
* **m**. Add a call to `loadGroceries()` as the last line in the existing `exports.load` function.
* **n**. Save list.js and config.js, and then perform a LiveSync to see the changes to your app. You should see the list of groceries you added to your backend.

<hr data-action="end" />

Why did this work? Note how the callback of the JSON GET call does nothing more than add each grocery returned to a `groceries` array. If you open the XML file for the list page (app/views/list.xml) you'll see a line that looks like this `<ListView items="{{ groceries }}" />`. This, along with the `page.bindingContext = pageData` statement in list.js, binds the grocery array to the `<ListView>` element—meaning, if you update the array, the `<ListView>` rerenders automatically. Cool right?

This example also shows how NativeScript modules can be used to abstract you from platform-specific iOS and Android code. All you need to know is that the NativeScript http module has a `getJSON()` method, not how to retrieve a JSON file in three different languages.

Next let's use the same http module to POST new data, and see how to integrate some new UI elements.

### Step 3. Post to an endpoint

A grocery list app isn't all that useful if you can't add to the list, so that's the behavior you're going to add next. As with reading data, you'll find that the http module makes interacting with backend data services easy. Let's start by building a few new UI components to collect data from the user.

<hr data-action="start" />

#### Action

* **a**. Add a `<TextField>` and a `<Button>` to your page by pasting the code below into your project's list.xml file:
```
<Page navigatedTo="load">
    <GridLayout rows="auto, *">
        <StackLayout orientation="horizontal" row="0">
            <TextField id="grocery" width="200" text="{{ grocery }}" hint="Enter a grocery" />
            <Button text="Add" tap="add"></Button>
        </StackLayout>
        <ListView items="{{ groceries }}" row="1">
            <ListView.itemTemplate>
                <Label text="{{ name }}" />
            </ListView.itemTemplate>
        </ListView>
    </GridLayout>
</Page>
```
* **b**. Save list.xml and perform a LiveSync to see how the new textfield and button look.

<hr data-action="end" />

The `<GridLayout>` element's `rows` attribute defines two rows for this page. The `row` attribute applied to subsequent elements is zero-based, so the `row="0"` attribute on the `<StackLayout>` places it in the top row, and the `row="1"` attribute on the `<ListView>` places it in the bottom row. You can try placing around with attributes to see how you can rearrange the elements on the screen.

The other new attribute is the `tap` attribute on the `<Button>` element. Your next task is defining the `add` function that the `tap` attribute refers to.

<hr data-action="start" />

#### Action

* **c**. Add the following function to your list.js file, directly after the existing `exports.load` function:
```
exports.add = function() {
    // Dismiss the keyboard before adding to the list
    viewModule.getViewById(page, "grocery").dismissSoftInput();

    addGrocery(pageData.get("grocery"));

    // Clear the text field
    pageData.set("grocery", "");
};
```

<hr data-action="end" />

This `add()` function corresponds with the `tap="add"` attribute you place on the `<Button>` element. When you click the new button NativeScript invokes this function.

The function itself retrieves the user-typed grocery from the data model (`pageData.get("grocery")`), and then calls the `addGrocery()` function with that value. Let's define `addGrocery()` to see how you can add this value to your backend.

<hr data-action="start" />

#### Action

* **d**. Add the following function to the bottom of your list.js file:
```
function addGrocery( grocery ) {
    httpModule.request({
        url: "http://api.everlive.com/v1/" + config.apiKey + "/Groceries",
        method: "POST",
        content: JSON.stringify({ Name: grocery }),
        headers: {
            "Content-Type": "application/json"
        }
    }).then( function( result ) {
        groceries.push({ name: grocery });
    });
};
```
* **e**. Save your list.js and perform a LiveSync to see the changes on your device. The Add button should now allow you to add items to the list.

<hr data-action="end" />

This `addGrocery()` function uses the same http module to send a POST request to your Backend Services backend, and then adds the grocery to the `groceries` array that's bound to the page's `<ListView>` element.

With this change in place you now have a functioning grocery list, so let's move onto another problem: user management. As is your app uses one list for all its users, which isn't very practical. Don't worry though; as it turns out, the Telerik Platform offers an elegant solution to user management as well. Let's see how it works.
