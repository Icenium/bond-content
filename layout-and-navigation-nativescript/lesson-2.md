## Lesson 2. App Navigation

### Step 7. Pages

Each NativeScript application is built upon the concept of pages (represented by the Page class). Pages are the different screens that your application shows. Each page has a content property which holds the root visual element of the page UI. Navigating between different pages is done with methods of the Frame class. The Frame class represents the logical unit that is responsible for navigation between different pages, i.e. going from one page to another, keeping a history stack for going back etc.

Pages can be defined in two ways. To separate the user interface from the logic, you should use an XML file to describe the UI of the page. Store the business logic of the page in a code-behind file. 

```
<Page loaded="onPageLoaded">
  <Label text="Hello, world!"/>
</Page>
```

```
function onPageLoaded(args) {
    console.log("Page Loaded");
}
exports.onPageLoaded = onPageLoaded;
```

<hr data-action="start" />

#### Action

* **a**. Modify the main page of your app.

**main.xml**
```
<Page>
  <StackPanel>
    <Label text="Hello, this is Main!" />
    <Button text="Go to Page1" tap="buttonForwardTap" />
  </StackPanel>
</Page>
```

**main.js**
```
var frameModule = require("ui/frame");
function buttonForwardTap(args) {
    var topmost = frameModule.topmost();
    topmost.navigate("app/page1");
}
exports.buttonForwardTap = buttonForwardTap;
```

* **b**. Create two additional pages - Page1 and Page2. You'd need two files per each page - one xml and one js. Then add the following markup and javascript to each page.

**page1.xml**
```
<Page>
  <StackPanel>
    <Label text="Hello, this is Page1!" />
    <Button text="Go to Page2" tap="buttonForwardTap" />
    <Button text="Go back to MainPage" tap="buttonBackTap" />
  </StackPanel>
</Page>
```

**page1.js**
```
var frameModule = require("ui/frame");
function buttonForwardTap(args) {
    var topmost = frameModule.topmost();
    topmost.navigate("app/page2");
}
exports.buttonForwardTap = buttonForwardTap;
function buttonBackTap(args) {
    var topmost = frameModule.topmost();
    topmost.goBack();
}
exports.buttonBackTap = buttonBackTap;
```

**page2.xml**
```
<Page>
  <StackPanel>
    <Label text="Hello, this is Page2!" />
    <Button text="Go back to Page1" tap="buttonBackTap" />
  </StackPanel>
</Page>
```

**page2.js**
```
var frameModule = require("ui/frame");
function buttonBackTap(args) {
    var topmost = frameModule.topmost();
    topmost.goBack();
}
exports.buttonBackTap = buttonBackTap;
```

* **b**. Sync your changes and open the app. Your main page should be the first page you see. From there you can navigate to Page1. Once on Page1, you have the option to go to Page2 or go back to the main page.

<hr data-action="end" />

### Step 8. Passing information between page navigation

In case you want to pass some state information, i.e. some kind of context, to the page you are navigating to, you should use the overload which accepts a NavigationEntry object. You can specify the page you want to navigate to by using the moduleName string property of the NavigationEntry class. 

<hr data-action="start" />

#### Action

* **a**. Modify buttonForwardTap in main.js to pass information to Page1.

```
function buttonForwardTap(args) {
    var topmost = frameModule.topmost();
    var navigationEntry = {
        moduleName: "app/tutorials/navigation/page1",
        context: {
            info: {
                    name: "John",
                    age: "22"
                },
        },
        animated: false
    };
    topmost.navigate(navigationEntry);
}
exports.buttonForwardTap = buttonForwardTap;
```

* **b**. Open page1.xml and subscribe for the navigatedTo event of the page.

```
<Page loaded="onPageLoaded" navigatedTo="pageNavigatedTo">
  ...
</Page>
```

* **c**. Open page1.js and add a navigatedTo event handler.

```
var dialogs = require("ui/dialogs");
function pageNavigatedTo(navigationEntry) {
    var name = navigationEntry.context.info.name;
    var age = navigationEntry.context.info.age;
    dialogs.alert(name + ", age " + age);
}
exports.pageNavigatedTo = pageNavigatedTo;
```

<hr data-action="end" />

Great job! In just a few minutes, you've mastered some of the critical basics to building native apps with NativeScript and the Telerik Platform. With these skills in hand, creating the navigation, layout and major views in your next mobile app should be a breeze!