## Lesson 1. Access device contacts with a Cordova plugin

Hybrid mobile apps built with AppBuilder use Apache Cordova as the bridge between your HTML5 application and native device features. You use simple JavaScript commands to access these "core" Cordova plugins such as the camera, stored contacts, geolocation data, and more. In this lesson you will create an app that interacts with native device features, displays your contacts, and lets you assign a picture to each contact using the camera. Let's get started!

### Step 1. Build a screen to list device contacts

Before you begin, make sure your `index.html` file is open. We have two Kendo UI Mobile views set up for you (`<div id="all-contacts">` and `<div id="view-contact">`). These views have been pre-populated with `data-title` properties to give each view its own distinguishing title. Each view also has a Kendo UI Mobile header (`<header data-role="header">`) which provides a navigation bar to display the view title.

This first step focuses on the first view (`<div id="all-contacts">`). This view contains a `<ul id="contacts-list">` element which will become a Kendo UI Mobile ListView due to the added `data-role="listview"` attribute. A ListView is a great way to display a list of items.

<hr data-action="start" />

#### Action

* **a**. Your first action is to create a Kendo UI template. Templates allow you to create chunks of HTML that will be merged with data. These "chunks" are repeated for each data element in your data source. The template you create will be bound to your contacts to provide a repeating list of items (contacts) in your ListView. Copy and paste this Kendo UI template into your page (outside of your Kendo UI views but within the `<body>` tags):

```
<script id="contacts-template" type="text/x-kendo-template">
    <li>
        # if (name.formatted) { #
            ${name.formatted}
        # } else if (name.givenName) { #
            ${name.givenName + " " + name.familyName}
        # } else { #
            No Name Listed
        # } #
    </li>
</script>
```

* **b**. Next, let's wire up your contacts database to the template you just created. Paste the following JavaScript functions in your `app.js` file (located in the `scripts` directory):

```
window.getAllContacts = function() {
    var options = new ContactFindOptions();
    options.filter = "";           
    options.multiple = true;       
    var fields = ["*"];  
    navigator.contacts.find(fields, onContactSuccess, onError, options);
}

function onContactSuccess(contacts) {  
    var template = kendo.template($("#contacts-template").html());
    var dataSource = new kendo.data.DataSource({ data: contacts });
    dataSource.bind("change", function () {
        $("#contacts-list").html(kendo.render(template, dataSource.view()));
    });
    dataSource.read();
}
```

The `getAllContacts` function queries your local contact database to load all of your contacts. The `navigator.contacts.find` function has a callback function, `onContactSuccess`, which is called when your device successfully retrieves your contacts. It's in this function that your contacts are bound to your ListView element (`<ul id="contacts-list">`) via the Kendo UI template.

* **c**. Save your changes.

<hr data-action="end" />

### Step 2. Run the app in the AppBuilder Companion App

It's fun to see the fruits of your labor, even if you have only just begun the lesson! As you learned in a previous lesson, the AppBuilder Companion App lets you run your app on a real device without going through the hassle of installing and configuring SDKs or provisioning profiles.

*Note that some Cordova plugins don't work in the device simulator, but do work with the Companion App and on your own device.*

<hr data-action="start" />

#### Action

* **a**. Download the appropriate AppBuilder Companion App on your device, using your device's app store (search for **AppBuilder**).

[![iOS app store](images/app-store-icon.png)](https://itunes.apple.com/us/app/telerik-appbuilder/id527547398?mt=8)
[![Google Play](images/google-play-icon.png)](https://play.google.com/store/apps/details?id=com.telerik.AppBuilder&hl=en)
[![Windows Phone Store](images/windows-phone-store-icon.png)](https://www.windowsphone.com/en-us/store/app/appbuilder/0171d46b-b5f2-43d9-a36b-0a78c9692aab?signin=true)

* **b**. Back in AppBuilder, select **Run** --> **Build**, select your device's platform (iOS/Android/Windows Phone), choose "AppBuilder companion app", and click Next.

* **c**. Scan the provided QR code on your device and play with the app you just created!

<hr data-action="end" />

### Step 3. Build a screen to display contact details

You now have a nice list of all of your device contacts, so the natural next step is to create a screen that displays more detailed information about a single selected contact.

<hr data-action="start" />

#### Action

* **a**. In your Kendo UI template that you created earlier, inside of the `<li>` element, add an anchor tag that will serve as a link from your list of all contacts to the contact detail view: `<a href="\#view-contact?id=${id}" class="expand">`. Your updated Kendo UI template should look like this:

```
<script id="contacts-template" type="text/x-kendo-template">
    <li>
        <a href="\#view-contact?id=${id}" class="expand">
            # if (name.formatted) { #
                ${name.formatted}
            # } else if (name.givenName) { #
                ${name.givenName + " " + name.familyName}
            # } else { #
                No Name Listed
            # } #
        </a>
    </li>
</script>
```

* **b**. Let's shift focus to your second view (`<div id="view-contact">`) and add a `data-show` property to this view. The `data-show` property tells your app to run a JavaScript function every time this view is displayed (or shown). Go ahead and add `data-show="window.getContactDetails"` - the start of your markup for this should now look like this:

	`<div id="view-contact" data-role="view" data-title="Contact Details" data-show="window.getContactDetails">`

* **c**. Now you need to define what that `data-show` JavaScript function is, so paste these two functions into your `app.js` file:

```
window.getContactDetails = function(e) {
    selectedContactId = e.view.params.id;
    var options = new ContactFindOptions();
    options.multiple = true;       
    var fields = ["*"];   
    navigator.contacts.find(fields, onContactDetailSuccess, onError, options);
}

function onContactDetailSuccess(contacts) {
	for (var i = 0; i < contacts.length; i++) {  
        if (contacts[i].id == selectedContactId)
        {
            $("#contact-name").text(getName(contacts[i]));
            
            if (contacts[i].phoneNumbers) {
                $("#contact-phone").text(contacts[i].phoneNumbers[0].value);
            } else {
                $("#contact-phone").text("");
            }
            
            selectedContact = contacts[i];
            
            break;
        }
    }  
}
```

In `window.getContactDetails` you are filtering your contacts by the id passed in the query string from your first view. Since we can't trust that this is an accurate filter (it's doing a string comparison, so the id "1" would also return "10"), in the `onContactDetailSuccess` callback function we loop through the returned contacts until we find one that matches your selected id exactly. It is within this function that your contact details are bound to your detail view.

* **d**. Save your changes.

* **e**. Back in your Companion App, do a three-finger tap and hold in your app to download the latest version of your app and see how it works now.

![three finger tap](images/three-finger-tap.png)

<hr data-action="end" />

Your next step is to use another core Cordova plugin (the camera) to add some flair to your app. After that we will take a look at using custom Cordova plugins from the Verified Plugins Marketplace to add even more native functionality to your app.