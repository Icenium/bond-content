## Lesson 2. Extend your app with the device camera

The app you have created functions just fine right now by consuming your contact data, but let's take a look at how we can easily integrate another core Cordova plugin, the device camera.

### Step 4. Customize your main contacts view to display an inline profile picture

Let's do a slight customization to your Kendo UI template to retrieve and display any profile pictures you already have for your contacts.

<hr data-action="start" />

#### Action

* **a**. Add an image element to your Kendo UI template that accounts for a profile picture that either exists or does not exist. The end result should look like this:

```
<script id="contacts-template" type="text/x-kendo-template">
    <li>
        <a href="\#view-contact?id=${id}" class="expand">
            # if (photos && photos.length) { #
                <img class="smallProfile" src="${photos[0].value}" />
            # } else { #
                <img class="smallProfile" src="styles/blankProfile.png" />
            # } #
    
            # if (name.givenName) { #
                ${name.givenName}
            # } #
    
            # if (name.familyName) { #
                ${name.familyName}
            # } #
        </a>
    </li>
</script>
```

> Tip: When we query the contacts database, we are asking for all fields (`var fields = ["*"];`). This means the profile pictures have already been included in this data set, so there is nothing more we need to do to request them!

* **b**. Save your changes.

* **c**. Use the Companion App on your device to do a three-finger tap and hold to download the latest version of your app to see if any profile pictures load.

<hr data-action="end" />

### Step 5. Customize the contacts detail screen to display a profile picture

Let's also load profile pictures on your contacts detail screen. We want to show the profile picture when the contact details are displayed.

<hr data-action="start" />

#### Action

* **a**. Add the following image element to the top of your second Kendo UI Mobile view (`<div id="view-contact">`) and underneath the header element: `<img class="largeProfile" />`. The end result should look like this:

```
<div id="view-contact" data-role="view" data-title="Contact Details" data-show="window.getContactDetails">
    <header data-role="header">
        <div data-role="navbar">
            <a data-align="left" data-role="backbutton">Back</a>
            <div data-role="view-title"></div>
        </div>
    </header>
    <img class="largeProfile" />
    <h2 id="contact-name"></h2>
    <h4 id="contact-phone"></h4>
</div>
```

* **b**. In order to display the photo in this view, you'll have to add some code that checks if any photos exist to your existing function (`onContactDetailSuccess`). The final function should look like this:

```
function onContactDetailSuccess(contacts) {
    for (var i = 0; i < contacts.length; i++) 
    {  
        if (contacts[i].id == selectedContactId)
        {
            $("#contact-name").text(getName(contacts[i]));
            
            if (contacts[i].phoneNumbers) {
                $("#contact-phone").text(contacts[i].phoneNumbers[0].value);
            } else {
                $("#contact-phone").text("");
            }
            
            if (contacts[i].photos && contacts[i].photos.length) {
                $(".largeProfile").attr("src", contacts[i].photos[0].value);
            } else {
                $(".largeProfile").attr("src", "styles/blankProfile.png");
            }
            
            selectedContact = contacts[i];
            
            break;
        }
    }  
}
```

* **c**. Save your changes.

<hr data-action="end" />

### Step 6. Use the device camera to add a profile picture

Since you have a place to display a picture, now is a good time to learn how to populate the image element with a picture captured from your device!

<hr data-action="start" />

#### Action

* **a**. Add a button to your contacts detail view (`<div id="view-contact">`) that will start up the camera. This can be placed underneath the existing elements that display your selected contact.

	`<button id="update-photo" data-role="button" onclick="window.updatePhoto()">Update Photo</button>`

* **b**. This button references a JavaScript function you haven't created yet (`window.updatePhoto`), so let's create it and its associated callback function (`onPhotoSuccess`). Paste the following into your `app.js` file:

```
window.updatePhoto = function() {
    navigator.camera.getPicture(onPhotoSuccess, onError, { quality: 50, destinationType: Camera.DestinationType.FILE_URI });
}

function onPhotoSuccess(imageURI) {
    $(".largeProfile").attr("src", imageURI);
    var photo=[];
    photo[0] = new ContactField('photo', imageURI, false)
    selectedContact.photos = photo;
    selectedContact.save();
}
```

Once the button is pressed, Cordova finds your device camera and enables it. You are then prompted to take a picture. The callback function `onPhotoSuccess` is called when you successfully take a picture.

* **c**. Save all of your changes and do a three finger tap in your Companion App again. Try adding a profile picture to one of your contacts!

<hr data-action="end" />

Your next step is learn how to use custom Cordova plugins from the Verified Plugins Marketplace to add even more native functionality to your app. Finally, you will learn how to run your app on a real mobile device.