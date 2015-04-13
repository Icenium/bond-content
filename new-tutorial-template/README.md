## Tutorial Folder ##

Each tutorial folder should contains the following items:

- The `metadata.json` file containing the **metadata** used to create and display the **tutorial** in the **Telerik Platform**:
	- `title` (*string*) - the title of the tutorial.
	- `description` (*string*) - the description of the tutorial.
	- `estimateTime` (*number*) - the estimate time for completing the tutorial.
	- `land` (*string*, *optional*) - specifies the first page that should be loaded after starting the tutorial. The available values are:
		- **`home`** (*default*) - the default page of the Telerik Platform will be loaded (the workspaces one).
		- **`workspace`** - available for tutorials creating a workspace. The newly created workspace will be loaded.
		- **`project`** - available for tutorials creating a project. The default page of the newly created project will be loaded.
	- `workspace` (*object*, *optional*) - object describing a workspace that should be created along with the tutorial.
		- `name` (*string*) - the name of the workspace.
		- `description` (*string*) - the description of the workspace.
		- `project` (*object*, *optional*) - object describing a project that should be created in the specified workspace along with the tutorial.
			- `type` - the service type of the project (e.g. appbuilder) 
			- the other properties are depending on the service type (appBuilder, backendServices, analytics etc.). They can be found in the **metadata-serviceType.json files**.
	
	> Each tutorial should have **only one metadata file** named `metadata.json`. The metadata files in this folder are examples for the structure of the project object depending on the service type.

- `lesson` files - the tutorial content is split on multiple parts (lessons) that should be executed successively. This folder contains 3 sample lessons - `1-lesson-one.md`, `2-lesson-two.md` and `3-lesson-three.md`. 

	>The only requirement for the **file names** is that their **alphabetical order** should be **the same as** the **logical order** of the lessons.

	Each lesson contains the following information:
	-  `title` (*format:## Lesson &lt;Number&gt;. &lt;Title&gt;*) - the title of the lesson e.g. *## Lesson 1. Build a photo album*.
	-  `introduction` (*string*, *optional*) - information about the goal of the current lesson.
	-  `steps` - each lesson is split on multiple parts (steps) that should be executed  successively. Each step contains the following information:
		- `title` (*format:### Step &lt;Number&gt;. &lt;Title&gt;*) - the title of the step
		- `introduction` (*string*, *optional*) - information about the goal of the current step.
		- `actions` - each step is split on multiple parts (actions) that should be executed  successively.
		- `summary`  (*string*, *optional*) - more information about the achieved goals and a conclusion
	- `summary` (*string*, *optional*) - more information about the achieved goals and a conclusion. 

- `images` folder - containing the image files used in the tutorial.

> Please follow [these steps](http://tap.telerik.com/process/tutorials#sync-tutorials "Update Telerik Platform tutorials based on the repository") in order to get the **Telerik Platform tutorials updated** after a **repository change**.
