## Lesson 3. Adding a registration screen

### Step 6. Build a registration screen

You're getting pretty close to a completely functional grocery list app. You have a UI for building lists, and a login screen that ensures that each user sees and manages their own list. The last step is to remove your reliance on hardcoded user accounts, and to allow registration directly within your app. Let's gets started by adding a registration form.

<hr data-action="start" />

#### Action

* **a**. Open views/login.xml and add the following button immediately after the existing “Sign In” button. This will act as the link to the registration page.
```
<Button text="Sign up for Groceries" click="register" />
```
* **b**. The button above needs a click handler defined to make it work. To do that, open views/login.js and add the following code to the bottom. This new function navigates the user to the register page.
```
exports.register = function( args ) {
    var topmost = frameModule.topmost();
    topmost.navigate( "app/views/register" );
};
```
* **c**. The login page now links to a register page which is defined by views/register.xml. Open up that file and paste in the following code:
```
<Page loaded="load">
    <StackPanel>
        <Image url="http://bs2.cdn.telerik.com/v1/GWfRtXi1Lwt4jcqK/28d417c0-9ccb-11e4-ad75-053ddcb57c52" />

        <Label text="Email Address:" />
        <TextField width="200" text="{{email_address}}" hint="Email Address" />

        <Label text="Username:" />
        <TextField width="200" text="{{username}}" hint="Username" />

        <Label text="Password:" />
        <TextField width="200" text="{{password}}" secure="true" hint="Password" />

        <Button text="Sign Up" click="register" cssClass="first-button" />
    </StackPanel>
</Page>
```
* **d**. Finally, to make the register page perform the actual registration, open views/register.js file and paste in the following code:
```
var frameModule = require( "ui/frame" ),
    dialogModule = require( "ui/dialogs" ),
    userCredentials = require( "../models/userCredentials" ),
    el = require( "../models/el" );

exports.load = function( args ) {
    var page = args.object;
    userCredentials.set( "email_address", "" );
    userCredentials.set( "username", "" );
    userCredentials.set( "password", "" );
    page.bindingContext = userCredentials;
};
exports.register = function() {
    el.Users.register(
        userCredentials.get( "username" ),
        userCredentials.get( "password" ),
        { Email: userCredentials.get( "email_address" ) },
        function( response ) {
            dialogModule
                .alert( "Your account was successfully created." )
                .then(function() {
                    frameModule.topmost().navigate( "app/views/login" );
                });
        },
        function( error ) {
            console.log( JSON.stringify( error ) );
        }
    );
};
```
* **d**. Save login.xml, login.js, register.xml, and register.js, and then perform a LiveSync to test out the registration process in your app.

<hr data-action="end" />

You can see a number of things we covered earlier being used in the code above. The actual call to perform the registration happens with a single call to the Backend Services SDK (`el.Users.register`); the NativeScript dialog module is used to show a confirmation message after successful registration (`dialogModule.alert`); and the NativeScript frame module is used to navigate between pages in the app (`frameModule.topmost().navigate()`).

The power of NativeScript here is how easy it makes getting a cross-platform native user interface up and running. In this lesson you were able to build an entire user management system—complete with cloud data storage—in about a half hour. And that's something worth celebrating. Congrats!
