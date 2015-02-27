## Tutorials Root Folder ##

This folder contains the following items:

- **System folders** like `new-tutorial-template`.

  > Take a look at the `new-tutorial-template` folder containing the basic information about the structure of each tutorial and some sample files. It can be used as a start point for the new tutorials

- The `metadata.json` file - it contains information about tutorial **types** and **keys of the active tutorials per type in their order** in the Telerik Platform.

  > The **first tutorial** in the metadata file is taken as a **default** one and directly displayed for the new users.
  
  > Note that each **type** must specify **id**, **shortName**, **longName** and **orderedTutorials** list

- A **separate folder** for **each tutorial** like `quick-start`. Bear in mind that the **name** of each **tutorial folder** is taken as its **key**.

> Please follow [these steps](http://tap.telerik.com/process/tutorials#sync-tutorials "Update Telerik Platform tutorials based on the repository") in order to get the **Telerik Platform tutorials updated** after a **repository change**.
