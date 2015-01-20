## Lesson 2. Managing users and authorization

### Step 4. Configure permissions

Backend Services automates the tricky process of registering and managing user accounts. In this lesson you'll create user accounts, configure your backend to use them, and add a login screen to your application. To start, let's add a new user to your Backend Services project.

<hr data-action="start" />

#### Action

* **a**. Click the “Groceries App” link in the top-left corner to return to your workspace.
* **b**. Click the “Groceries Backend” box to enter your Backend Services project.
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

By changing the Groceries content type to private, you prevent access to the content type for unauthenticated users. You also implicitly associate the Groceries content type to user accounts. This means that each authenticated user will get their own grocery list to manage. This also means that the code in your existing will no longer work, as the Backend Services REST API now requires user credentials in order to read from and write to the Groceries content type.

Let's head back to your app and add a way to get those credentials with a login screen.

### Step 5. Build a login screen

A login screen is the single most used UI component in mobile apps. Almost everyone needs one, and the Telerik Platform makes it trivial to get one up and running in a few minutes. Let's see how.

<hr data-action="start" />

#### Action

* **a**. Return to your AppBuilder project by returning to your workspace (click the “Groceries App” link in the top-right corner of the screen), and then clicking the “Groceries” box.
* **b**. Open your app's empty views/login.xml file and place the following code in it:
```
<Page loaded="load">
    <StackPanel>
        <Image url="http://bs2.cdn.telerik.com/v1/GWfRtXi1Lwt4jcqK/28d417c0-9ccb-11e4-ad75-053ddcb57c52" />

        <Label text="Username:" />
        <TextField width="200" text="{{ username }}" id="username" hint="Username" />

        <Label text="Password:" />
        <TextField width="200" secure="true" text="{{ password }}" hint="Password" />

        <Button text="Sign In" click="signIn" cssClass="first-button" />
    </StackPanel>
</Page>
```
* **b**. Open the app.js file, and change the `application.mainModule = "app/views/list";` line to `application.mainModule = "app/views/login";`. This changes the starting page of your app from the list screen to the login screen.
* **c**. Save your login.xml and app.js files, and then perform a LiveSync to see the changes.

<hr data-action="end" />

Your app now opens up to a login screen rather than a list of groceries. Your next step is to add the JavaScript to make this page work. The Backend Services REST API has URLs you can invoke to authenticate users, so you can use the NativeScript http module to handle this logic, however this time you're going to try a different approach: the Backend Services NativeScript SDK. The SDK invokes the same REST APIs under the hood, but it gives you a cleaner JavaScript API to work with. Let's see how it works.

<hr data-action="start" />

#### Action

* **d**. Open your app's views/login.js file and paste in the following code:
```
var frameModule = require( "ui/frame" ),
    userCredentials = require( "../models/userCredentials" ),
    el = require( "../models/el" );

exports.load = function( args ) {
    var page = args.object;
    page.bindingContext = userCredentials;
};

exports.signIn = function( args ) {
    el.Users.login(
        userCredentials.get( "username" ),
        userCredentials.get( "password" ),
        function() {
            frameModule.topmost().navigate( "app/views/list" );
        }
    );
};
```
* **e**. Save login.js.

<hr data-action="end" />

The actual SDK is defined by the `el` module retrieved from the `require( "../models/el" )` call. That module is defined in models/el.js, which uses the config module you placed your Backend Services project's API key in earlier.

With the Backend Services SDK a login call is as simple as `el.Users.login()`. The call returns a token that can be used on subsequent requests, and the SDK automatically stores that token so you don't have to do it yourself. The only thing you explicitly do is call `frameModule.topmost().navigate( "app/views/list" )` after a successful login to take the user to the list page.

To get this app functioning again you have one final thing to do: change the list page to use the authentication token from the login process.

<hr data-action="start" />

#### Action

* **f**. Open views/list.js file, remove the existing `loadGroceries()` and `addGrocery()` functions, and replace them with the following:
```
function loadGroceries() {
    el.data( "Groceries" ).get().then(function( data ) {
        data.result.forEach(function( grocery ) {
            groceries.push( grocery.Name );
        });
    });
}

function addGrocery( grocery ) {
    el.data( "Groceries" ).create({
        Name: grocery
    }).then( function( result ) {
        groceries.push( grocery );
    });
}
```
* **g**. Save list.js and perform a LiveSync to see the updated app on your device.
* **h**. Login with the account you created earlier and add a few items to the list.

<hr data-action="end" />

And with that you have a completely functional login process! Notice how the Backend Services SDK simplified backend calls into a few, very readable lines of code. You don't have to worry about managing login tokens or setting the appropriate HTTP headers as the SDK handles that for you.

Now that your data is stored, and you have a user management system in place, it's time to add the final piece of the puzzle: registration.