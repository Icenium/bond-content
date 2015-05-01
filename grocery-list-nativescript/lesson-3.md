## Lesson 3. Adding a registration screen

### Step 6. Build a registration screen

You're getting pretty close to a completely functional grocery list app. You have a UI for building lists, and a login screen that ensures that each user sees and manages their own list. The last step is to remove your reliance on hardcoded user accounts, and to allow registration directly within your app. Let's gets started by adding a registration form.

<hr data-action="start" />

#### Action

* **a**. Open views/login.xml and add the following button immediately after the existing “Sign In” button. This will act as the link to the registration page.
```
<Button text="Sign up for Groceries" tap="register" />
```
* **b**. The button above needs a tap handler defined to make it work. To do that, open views/login.js and add the following code to the bottom. This new function navigates the user to the register page.
```
exports.register = function(args) {
    var topmost = frameModule.topmost();
    topmost.navigate("./views/register");
};
```
* **c**. The login page now links to a register page which is defined by views/register.xml. Open up that file and paste in the following code:
```
<Page loaded="load">
    <StackLayout>
        <Image source="{{ logoSource }}" />

        <Label text="Email Address:" />
        <TextField width="200" text="{{ email_address }}" id="email" hint="Email Address" keyboardType="email" />

        <Label text="Username:" />
        <TextField width="200" text="{{ username }}" id="username" hint="Username" />

        <Label text="Password:" />
        <TextField width="200" text="{{ password }}" secure="true" hint="Password" />

        <Button text="Sign Up" tap="register" />
    </StackLayout>
</Page>
```
* **d**. Finally, to make the register page perform the actual registration, open views/register.js file and paste in the following code:
```
var dialogs = require( "ui/dialogs" );
var el = require( "../shared/models/el" );
var frameModule = require( "ui/frame" );
var images = require( "../shared/utils/images" );
var pageData = require( "../shared/models/userCredentials" );
var viewModule = require( "ui/core/view" );
exports.load = function(args) {
    var page = args.object;
    var email = viewModule.getViewById(page, "email");
    var username = viewModule.getViewById(page, "username");
    pageData.set("email_address", "");
    pageData.set("username", "");
    pageData.set("password", "");
    pageData.set("logoSource", images.logo);
    page.bindingContext = pageData;
    // Turn off autocorrect and autocapitalization for iOS
    if (username.ios) {
        email.ios.autocapitalizationType =
            UITextAutocapitalizationType.UITextAutocapitalizationTypeNone;
        email.ios.autocorrectionType =
            UITextAutocorrectionType.UITextAutocorrectionTypeNo;
        username.ios.autocapitalizationType =
            UITextAutocapitalizationType.UITextAutocapitalizationTypeNone;
        username.ios.autocorrectionType =
            UITextAutocorrectionType.UITextAutocorrectionTypeNo;
    }
};
exports.register = function() {
    el.Users.register(
        pageData.get("username"),
        pageData.get("password"),
        { Email: pageData.get("email_address") },
        function(response) {
            dialogs
                .alert("Your account was successfully created.")
                .then(function() {
                    frameModule.topmost().navigate("./views/login");
                });
        },
        function(error) {
            dialogs.alert({
                message: "Unfortunately we were unable to create your account.",
                okButtonText: "OK"
            });
        }
    );
};
```
* **e**. Save login.xml, login.js, register.xml, and register.js, and then perform a LiveSync to test out the registration process in your app.

<hr data-action="end" />

You can see a number of things we covered earlier being used in the code above. The actual call to perform the registration happens with a single call to the Backend Services SDK (`el.Users.register`); the NativeScript dialog module is used to show a confirmation message after successful registration (`dialogModule.alert`); and the NativeScript frame module is used to navigate between pages in the app (`frameModule.topmost().navigate()`).

The power of NativeScript here is how easy it makes getting a cross-platform native user interface up and running. In this lesson you were able to build an entire user management system—complete with cloud data storage—in about a half hour. And that's something worth celebrating. Congrats!
