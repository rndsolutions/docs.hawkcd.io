# Deployment of a Pipeline
## Create a Pipeline
### # 1: Specify pipeline name
Click <b><i>+Add</i></b><br />
![Screenshot](../img/hawkCDSteps/2AddPipeline.png)
<br />
<br />

This will open a pop-up window.<br />
Enter pipeline name by your choice and click <b><i>Continue</i></b>
![Screenshot](../img/hawkCDSteps/pipelineName.png)
<br />
<br />

### # 2: Choose material source
Specify your material and click <b><i>Continue</i></b>.<br />
You have two options:

> * Existing Material - (choose material from..)
> * New Material - (adds new material from to..)

![Screenshot](../img/hawkCDSteps/chooseMaterial.png)
<br />Choose <b><i>New Material</i></b>
<br />
<br />

### # 3: Add material
<p>
In order to continue you have to fill out correctly two required fields: <b> Material Name</b> and <b>Git Url</b>.</br>
Unspecified <b>Git Branch</b> will choose <b>master</b> branch by default.
Marking <b>Poll for changes</b> will automatically fetch any new changes on your github repository.<br />
</p>
![Screenshot](../img/hawkCDSteps/addMaterial.png)
<b>CAUTION:</b> In order for <b>Poll for changes</b> to work <a href="3"> Automatic scheduling</a> must be checked.<br />
<b>Credentials</b> must be provided only if the git repository is private.

![Screenshot](../img/hawkCDSteps/credentials.png)
<br />


### # 4: Submit
Click <b> Submit </b> to save or <b> Back</b> to make changes.
</p>

![Screenshot](../img/hawkCDSteps/pipelineReview.png)


<br />
<br />
<center>
![Screenshot](../img/hawkCDSteps/tada.png)
<h2>Your Pipeline is created successfully.</h2>
</center>
<br />
![Screenshot](../img/hawkCDSteps/firstPipeline.png)

## Configure Pipeline
### Update Pipeline Name
To update Pipeline name simply click the 'config' button (the one in the middle).
![Screenshot](../img/hawkCDSteps/config.png)

This will open the config screen, General Options tab is selected by default. <br />
Enter the new name and select <b> Update </b><br />
![Screenshot](../img/hawkCDSteps/newName.png)
<br />

<b>DONE ! </b><br />
Pipeline name changed.<br />
![Screenshot](../img/hawkCDSteps/updatedName.png)
<br />
<br />
<br />





### Automatic Scheduling
### Add / Delete Stage
#### Configure Stage
### Add / Delete Task
#### Configure Task
#### Add / Delete Exec Task
##### Configure Exec Task
#### Add / Delete Upload Artifact
##### Configure Upload Artifact
#### Add / Delete Fetch Artifact Task
##### Configure Fetch Artifact Task
#### Add / Delete Fetch Material Task

## Delete Pipeline

# Pipeline Groups
## Add new Pipeline Group
### Steps
## Manage Pipeline Group
### Steps
## Delete Pipeline Group
### Steps
# Materials
## Add new Material
### Steps
## Manage Material
### Steps
## Delete Material
### Steps
