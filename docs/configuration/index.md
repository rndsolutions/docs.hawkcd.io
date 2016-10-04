## Configuration

### Create a Pipeline

<p>
To create a pipeline, we must first understand the Continuous Delivery methodology/process. <br />
</p>

<div class="questionTitles">What is Continuous Delivery and why do we use it? </div>
[useful links or our explanation]<br />

<p>
The core idea of Continuous Delivery is to create a repeatable, reliable and incrementally improving process for taking software from concept to customer. The Continuous Delivery pipeline is what makes it all happen.
</p>

<div class="questionTitles">What is a Continuous Delivery Pipeline?</div>

<p>
The continuous delivery pipeline breaks down the software delivery process into stages. Each stage is aimed at verifying the quality of new features from a different angle to validate the new functionality and prevent errors from affecting your users. <br /><br /> TO BE explained in details.... about stages, jobs tasks and accompanied materials.
</p><br />

<div class="questionTitles">Creating Pipeline</div>


<p>
Creating a pipeline in HawkCD is very easy and its separated in few simple steps.
We'll explain it in details shortly. </br />
Before creating a pipeline we must first choose the pipeline group in which we want our pipeline to be created.<br />
In order to create a pipeline, we must have a pipeline group. That is why HawkCD comes with<br />
a <b> defaultPipelineGroup</b>.<br /><br />
</p>

<p>
What are pipline groups? Click <a href="#"> here </a> to find out.
</p>

<p> This is how pipeline groups look like. </p>
![Screenshot](../img/hawkCDSteps/pipelineGroups.png)

<p>
HawkCD comes with a <b>defaultPipelineGroup</b>.
You can use the <b>defaultPipelineGroup</b> to create your first pipeline or you can
create your own pipeline group and delete <b>defaultPipelineGroup</b>.<br /><br />
<b>NOTE: A pipeline group must be empty in order to be deleted.</b>
</p>

<p>Here are the few simple steps to create your own pipeline.</p>

<b>Step 1: Specify pipeline name</b> <br />
<i>Click <b>+Add</b> to create a new Pipeline Definition within the pipeline group you've chosen. </i>
</p>


![Screenshot](../img/hawkCDSteps/2AddPipeline.png)

<p>
This will open a pop-up window.<br />
<i>Enter pipeline name by your choice and click <b>continue</b></i>
</p>

![Screenshot](../img/hawkCDSteps/PipelineName.png)

<p>
<b>Step 2: Choose material source</b><br />
Specify your material and click <b>continue</b>. You have two options - to choose an existing material or to add a new one.<br />
</p>

![Screenshot](../img/hawkCDSteps/materials.png)

<p>
In the following example we demonstrate creating pipeline definition with a new material.
<br /> So choosing <b>New Material</b> would be the right step.
</p>

<p>
<b> Step 3: Add material.</b>
</p>

![Screenshot](../img/hawkCDSteps/newMaterial.png)

<p>
In order to continue you have to fill out correctly two required fields: <b> Material Name</b> and <b>Git Url</b>.</br>
Unspecified <b>Git Branch</b> will choose <b>master</b> branch by default.
Marking <b>Poll for changes</b> will automatically fetch any new changes on your github repository.
(<b>NOTE:</b> in order for <b>Poll for changes</b> to work <a href="3"> Automatic scheduling</a> must be checked.) <br />
</p>

<p>
<b>Credentials</b> must be provided if the git repository is private, otherwise your material will be invalid and Pipeline<br />
run will be set to status FAILED.   (more about pipeline statuses <a href= "#"> here </a>)
</p>

![Screenshot](../img/hawkCDSteps/credentials.png)



<p>
<b> Step 4: Submit </b><br />
Click <b> Submit </b> to save or <b> Back</b> to make some changes.
</p>

![Screenshot](../img/hawkCDSteps/pipelineReview.png)


<br />
<br />
<center>
![Screenshot](../img/hawkCDSteps/tada.png)
<h2>Your Pipeline is created successfully.</h2>
</center>
<br />
![Screenshot](../img/hawkCDSteps/pipelineCreated.png)

### Create Pipeline Group
#### TODO
### Monitor Pipeline run
### Add a new agent
### Setup resources
### Create & Authorize users
### Manage pipeline groups
### Configure Server
### Create materials
### Run multiple agents on a Server
### Running shell/bash and PowerShell scripts
### Uploading Artifacts
### Fetch Artifacts
### Fetch Materials
