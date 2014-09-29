## Lesson 3: Adding a registration screen

### Step 7. Build a registration screen

You're getting pretty close to a completely functional grocery list app. You have a UI for building lists, and login screen that ensures that each user sees and manages their own list. The last step is to remove your reliance on hardcoded user accounts, and to allow registration directly within your app. Let's gets started by adding a registration form.

<hr data-action="start" />

#### Action

* **a**. Add the following markup to your index.html file. The view should come directly after the login view (`<div id="login">`) and before the list view (`<div id="list">`).
```
<div data-role="view" id="register" data-title="Register" data-model="registerView">
    <div data-role="navbar">
        <a href="#login" data-align="left" data-role="button">Back</a>
        <span data-role="view-title"></span>
    </div>
    <form>
        <ul data-role="listview" data-style="inset">
            <li>
                <label>
                    Email Address:
                    <input type="email" name="email" data-bind="value: email">
                </label>
            </li>
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
                <button style="width: 100%" data-role="button" data-bind="click: submit">Register</button>
            </li>
        </ul>
    </form>
</div>
```
* **b**. Next you need to add a link to the register view from the login view. Find the login view (`<div id="login">`) and switch it to use the code below. (Only copy and paste `<a href="#register">Create an account</a>` as that's all you need to add.)
```
<div data-role="view" id="login" data-title="Login" data-model="loginView">
    <!-- the existing contents of the view -->
    <a href="#register">Create an account</a>
</div>
```
* **c**. Add the following code to your app.js file:
```
window.registerView = kendo.observable({
    submit: function() {
        if (!this.username) {
            navigator.notification.alert("Username is required.");
            return;
        }
        if (!this.password) {
            navigator.notification.alert("Password is required.");
            return;
        }
        el.Users.register(this.username, this.password, { Email: this.email },
            function() {
                navigator.notification.alert("Your account was successfully created.");
                window.location.href = "#login";
            },
            function() {
                alert("Unfortunately we were unable to create your account.");
            });
    }
});
```
* **d**. Save index.html and app.js and open your app in the simulator.

<hr data-action="end" />

fdsfds

### Step 8. Configuring emails