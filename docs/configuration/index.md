## Configuration

### Create a Pipeline

Once you have logged into the system, you will be redirected to the page which displays all the Pipelines you have permission for. Additionally, if you have permission for adding Pipelines, there will be "Add" button within each Pipeline Group, which will open modal window, where the process of creation will begin.

 ![Screenshot](../img/pipelines-screen.jpg)

#### The creation of Pipeline can be accomplished in 3 basic steps:

>Step 1: Setup

 ![Screenshot](../img/create-pipeline-setup.png)

###### * In this example **Automatic Scheduling** will be used

 1. *Fill in the Pipeline name*

 2. *Choose whether the pipeline will be automatically scheduled (this means that if the material that the pipeline uses is polling for changes, each time there is a registered change in the material, the pipeline will automatically run)*

>Step 2: Materials

 ![Screenshot](../img/create-pipeline-materials.png)

 In this step you can use either ***Existing Material***, or ***New Material***, which the pipeline will use. If you choose the first option, a dropdown list with all the materials will be provided for you and you will be able to choose any one of them.

###### * In this example **New Material** will be used

![Screenshot](../img/create-pipeline-materials-new-material.png)
###### * In this example **Poll for changes** will be used

1. *Fill the material name*

2. *Fill the git repo url*

3. *Choose whether the material will be polling for changes (this means that if the pipeline that uses the material is set to automatically schedule, each time there is a registered change in the material, the pipeline will automatically run)*

4. *Choose whether you will enter your git credentials (username and password). In the cases when the git repo is private, the credentials are obligatory, while when it is public, they are not*

>Step 3: Review

![Screenshot](../img/create-pipeline-review.png)

Here, you can review the pipeline you just created and if everything is correct, to submit the Pipeline

### Manage Pipeline run
### Add a new agent
### Setup resources
### Create & Authorize users
### Manage  pipeline groups
### Configure Server
