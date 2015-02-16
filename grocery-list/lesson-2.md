## Lesson 2. Managing users and authorization

### Step 4. Configure permissions

Backend Services automates the tricky process of registering and managing user accounts. In this lesson you'll create user accounts, configure your backend to use them, and add a login screen to your application. To start, let's add a new user to your Backend Services project.

<hr data-action="start" />

#### Action

* **a**. Click the “Grocery List link in the top-left corner to return to your app's homepage.
* **b**. Click the “Grocery List Backend” box to enter your Backend Services project.
* **c**. Click the “Services” option in the menu on the left-hand side of the screen.
* **d**. Find the “User Management”  box and click its "Add to project" button to enable user management for your Backend Services project.
* **e**. Click the “Users Browser” link in the menu on the left-hand side of the screen.
* **f**. Click the blue “Add a user” button.
* **g**. Use the pane on the right-hand side of the screen to configure a dummy user to use for testing. Feel free to use fake data, but make sure to remember the username and password you use, as you'll need them later in this step.

<hr data-action="end" />

Now you have a dummy user account to test your app with. Before we head back to your app though, we have to make one tweak to the Groceries content type you created in the last lesson.

<hr data-action="start" />

#### Action

* **h**. Click the “Permissions” link in the menu on the left-hand side of the screen.
* **i**. Change the Groceries permission dropdown from “Public” to “Private”, and then click the green “Save” button on the bottom of the screen.

<hr data-action="end" />

By changing the Groceries content type to private, you prevent access to the content type for unauthenticated users. You also implicitly associate the Groceries content type to user accounts. This means that each authenticated user will get their own grocery list to manage. Let's see how to change your app to accommodate this change.

<hr data-action="start" />

#### Action

* **j**. Return to your AppBuilder project by returning to your app's homepage (click the “Grocery List” link in the top-right corner of the screen), and then clicking the “Grocery List Code” box.
* **k**. Open your app.js file from the scripts folder, and add the following code as the very first thing in the `initialize()` function:
```
el.Users.login("username", "password",
    function(data) {
        groceryDataSource.read();
    }
);
```
* **l**. In the `login()` call you just added, substitute the “username” and “password” strings with the values you used to configure your dummy user in Backend Services.
* **m**. Save app.js and open your app in the simulator.

<hr data-action="end" />

When you first open your app the grocery is empty, as none of the existing groceries are associated with your dummy user. But if you add some groceries they'll be added to your dummy user's list, and Backend Services will take care of enforcing that other users cannot access the dummy user's data automatically. Pretty cool huh?

You next step is to remove the dummy user's hardcoded credentials from your code and to build a true login screen.

### Step 5. Build a login screen

A login screen is the single most used UI component in mobile apps. Almost everyone needs one, and the Telerik Platform makes it trivial to get one up and running in a few minutes. Let's see how.

<hr data-action="start" />

#### Action

* **a**. Open your app's index.html file and place the following code as the very first thing after the opening `<body>` tag:
```
<div data-role="view" id="login" data-title="Login" data-model="loginView">
    <div data-role="navbar">
        <span data-role="view-title"></span>
    </div>
    <form>
        <ul data-role="listview" data-style="inset">
            <li>
                <label>
                    Username:
                    <input type="text" name="username" data-bind="value: username">
                </label>
            </li>
            <li>
                <label>
                    Password:
                    <input type="password" name="password" data-bind="value: password">
                </label>
            </li>
            <li>
                <button style="width: 100%" data-role="button" data-bind="click: submit">Login</button>
            </li>
        </ul>
    </form>
</div>
```
* **b**. In your app.js file, in the `initialize()` function, remove the call to `el.Users.login` that contains hardcoded user credentials.
* **c**. Add the following code to app.js—after `initialize()`, and before the `window.addView` declaration.
```
window.loginView = kendo.observable({
    submit: function() {
        if (!this.username) {
            navigator.notification.alert("Username is required.");
            return;
        }
        if (!this.password) {
            navigator.notification.alert("Password is required.");
            return;
        }
        el.Users.login(this.username, this.password,
            function(data) {
                window.location.href = "#list";
                groceryDataSource.read();
            }, function() {
                navigator.notification.alert("Unfortunately we could not find your account.");
            });
    }
});
```
* **d**. Save your index.html and app.js files, and then open your app in the simulator.
* **e**. Input the credentials for your dummy users to verify that the new login screen works.

<hr data-action="end" />

A couple cool things happened here. The first is that your app now opens to the login screen rather than the list screen. This happens because Kendo UI starts on the first view (`<div data-role="view">`) that it finds in the DOM. You can also configure this by setting the `initial` option when you configure your `kendo.mobile.Application`.

The other cool thing is the use of Kendo UI's MVVM data binding functionality. Note that in the `loginView` code you have direct access to the form's values with simple `this.username` and `this.password` references. This happens because those `<input>` elements use `data-bind` attributes to tie their value to properties in the `loginView` object. The same `data-bind` attribute ties the click event of the login button to the `submit` function that starts the login process.

Now that you have a login in place, it's time to add the opposite functionality: the ability to logoff.

<hr data-action="start" />

#### Action

* **f**. In your index.html change the existing `<div data-role="view" id="list">` with the one below. The two changes are a `data-model` attribute and a new logout button.
```
<div data-role="view" id="list" data-model="listView" data-title="Grocery List">
    <div data-role="navbar">
        <a data-role="button" href="#login" data-align="left" data-bind="click: logout">Logout</a>
        <span data-role="view-title"></span>
        <a data-role="button" data-align="right" data-rel="modalview" href="#add">Add</a>
    </div>
    <ul id="grocery-list"></ul>
</div>
```
* **g**. In app.js, paste the following model object directly after the `window.loginView` declaration:
```
window.listView = kendo.observable({
    logout: function(event) {
        // Prevent going to the login page until the login call processes.
        event.preventDefault();
        el.Users.logout(function() {
            this.loginView.set("username", "" );
            this.loginView.set("password", "");
            window.location.href = "#login";
        }, function() {
            navigator.notification.alert("Unfortunately an error occurred logging out of your account.");
        });
    }
});
```
* **h**. Save your index.html and app.js files and open the project in the simulator.

<hr data-action="end" />

And with that you have a completely functional login process! If you'd like, you can create a few more dummy accounts in your Backend Services project, and see how each user now maintains their own list.

Now that your data is stored, and you have a user management system in place, it's time to add the final piece of the puzzle: registration.