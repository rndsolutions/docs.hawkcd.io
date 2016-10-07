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

### Add / Delete Job
Choose Pipeline you wish to add Job to and click *config* button (the one in the middle).
![Screenshot](../img/hawkCDSteps/add-exec-choose.png) <br /> <br />
Navigate to the Stage you wish to add Job to.
Jobs tab is selected by default.  click <strong>*Add Job*</strong>.
![Screenshot](../img/hawkCDSteps/add-job-navToStage.png) <br /> <br />


Enter Job name, select job's task, fill task's form template add click <strong>*Add Job*</strong>.
![Screenshot](../img/hawkCDSteps/add-job-fillTmp.png) <br /> <br />


Done! <br />
*MyJob added*. <br />
![Screenshot](../img/hawkCDSteps/add-job-done.png) <br /> <br />

##### Delete Job
Choose Pipeline where to delete Job from and click *config* button (the one in the middle).
![Screenshot](../img/hawkCDSteps/delete-job-chooseP.png) <br /> <br />

Navigate to Stage where the Job is placed and click *Delete*.
![Screenshot](../img/hawkCDSteps/delete-job.png) <br /> <br />

Confirm removal by clicking *Delete* button.
![Screenshot](../img/hawkCDSteps/delete-job-delete.png) <br /> <br /



#### Configure Job

### Add / Delete Task
#### Configure Task
#### Add / Delete Exec Task
##### # Add Exec Task
Choose pipeline and click *config* button (the one in the middle)<br />
![Screenshot](../img/hawkCDSteps/add-exec-choose.png) <br /> <br />

Navigate to the [Job](/) to which you want to add [Task](/) <br />
*Tasks* tab is selected by default. Click on *Add Task*.<br />
![Screenshot](../img/hawkCDSteps/add-exec-add.png) <br /> <br />

Choose *Exec* on the *Task Type*, fill out the form and click *Add Task*. <br />
![Screenshot](../img/hawkCDSteps/add-exec-task-submit.png) <br /> <br />

<strong> DONE! </strong> <br />
![Screenshot](../img/hawkCDSteps/add-exec-task-done.png) <br /> <br />

##### # Delete Exec Task

Navigate to the [Job](/concepts/#job) from where you want to delete the [Task](/concepts/#task) <br />
![Screenshot](../img/hawkCDSteps/exec-task-navigate-delete.png) <br /> <br />

Click *Delete* to remove selected task.<br />
![Screenshot](../img/hawkCDSteps/exec-task-delete.png)

##### Configure Exec Task

There are two options that can be configured on *Exec Task*.
<br />
<br />

First, navigate to selected [Job](/concepts/#job) and click *Edit* on the [Task](/concepts/#task)  to configure.<br />
![Screenshot](../img/hawkCDSteps/exec-task-edit.png) <br /> <br />

Configure the desired behavior for your [Task](/concepts/#task) and click *Edit Task*. <br /> <br />
![Screenshot](../img/hawkCDSteps/exec-task-configure.png) <br /> <br />



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
