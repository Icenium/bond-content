## Tutorial Folder ##

This folder contains the following items:

- The `metadata.json` file - it contains the **metadata information** used to create and display the tutorial in the Telerik Platform:
	- `title` (*string*) - the title of the tutorial.
	- `description` (*string*) - the description of the tutorial.
	- `estimateTime` (*number*) - the estimate time for completing the tutorial.
	- `land` (*string*, *optional*) - specifies the first page that should be loaded after starting the tutorial. The available vlaues are:
		- **`home`** (*default*) - the default page of the Telerik Portal will be loaded (the workspaces one).
		- **`workspace`** - available for tutorials creating a workspace. The newly created workspace will be loaded.
		- **`project`** - available for tutorials creating a project. The default page of the newly created project will be loaded.
	- `workspace` (*object*, *optional*) - object describing a workspace that should be created along with the tutorial.
		- `name` (*string*) - the name of the workspace.
		- `description` (*string*) - the description of the workspace.
		- `project` (*object*, *optional*) - object describing a project that should be created in the specified workspace along with the tutorial.
			- `type` - the service type of the project (e.g. appbuilder) 
			- the other properties are depending on the service type (appBuilder, backendServices, analytics etc.). They can be found in the **metadata-*serviceType*.json files** demonstrating each of the service **models**.

> Please follow [these steps](http://tap.telerik.com/process/tutorials, "Update Telerik Platform tutorials based on the repository") in order to get the **Telerik Platform tutorials updated** after a **repository change**.
