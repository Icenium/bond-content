## Lesson 3. Adding a registration screen

### Step 6. Build a registration screen

You're getting pretty close to a completely functional grocery list app. You have a UI for building lists, and a login screen that ensures that each user sees and manages their own list. The last step is to remove your reliance on hardcoded user accounts, and to allow registration directly within your app. Let's gets started by adding a registration form.

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
* **c**. Add the following code to your app.js file. Place it directly after the `window.loginView` declaration.
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

That's all it takes to create a functional registration screen, which is pretty cool. The actual registration happens during the `el.Users.register()` call, which adds the user to your Backend Services project. After you create a user, if you'd like, head back to the Users view to see the user you created through your app.

With the registration screen in place, there's one last piece of functionality we need to add to have a completely functional user management process: password resets.

### Step 7. Adding password reset functionality

Users forget their passwords all the time, so any app that uses accounts must provide a way for users to reset their password. But actually implementing a password reset has traditionally been tricky, or at the very least time consuming to get right. But this is another area where Backend Services shines, as implementing a password reset is as simple as a single API call. Let's see how it works.

<hr data-action="start" />

#### Action

* **a**. Add the following markup to your index.html file. The view should come directly after the register view (`<div id="register">`) and before the list view (`<div id="list">`).
```
<div data-role="view" id="password" data-title="Reset Password" data-model="passwordView">
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
                <button style="width: 100%" data-role="button" data-bind="click: submit">Reset</button>
            </li>
        </ul>
    </form>
</div>
```
* **b**. Next you need to add a link to the password reset view from the login view. Find the login view (`<div id="login">`) and add the code below directly after the existing link (`<a href="#register">Create an account</a>`).
```
<a href="#password">Forgot password?</a>
```
* **c**. Add the following code to your app.js file. Place it directly after the `window.registerView` declaration.
```
window.passwordView = kendo.observable({
    submit: function() {
        if (!this.email) {
            navigator.notification.alert("Email address is required.");
            return;
        }
        $.ajax({
            type: "POST",
            url: "https://api.everlive.com/v1/" + apiKey + "/Users/resetpassword",
            contentType: "application/json",
            data: JSON.stringify({ Email: this.email }),
            success: function() {
                navigator.notification.alert("Your password was successfully reset. Please check your email for instructions on choosing a new password.");
                window.location.href = "#login";
            },
            error: function() {
                navigator.notification.alert("Unfortunately, an error occurred resetting your password.")
            }
        });
    }
});
```
* **d**. Save your index.html and app.js files, and open the app up in the simulator.
* **e**. Use the new reset password functionality to reset the password of an existing user. Make sure to use a user that uses a valid email address you can check. If you don't have such a user use the register screen to create one.

<hr data-action="end" />

When users reset their password they're automatically sent a link to input a new password. Notice that you don't have to worry about building any of that functionality yourself. The emails and actual UI for resetting passwords is taken care of for you.

But that doesn't have to be the case. If you want to build these interfaces yourself, and not rely on an email workflow, you can do so with the Backend Services APIs. You can also do a lot of things we didn't have time to cover today—such as logging in with social accounts (Facebook, Google, Twitter, and so forth), integrating with Active Directory, and a lot more.

What you did do today is build an app from scratch with a comprehensive user management system—all in about a half hour. And that's something worth celebrating. Congrats!
