
Continuous Delivery Overview
==============================
The `Continuous Delivery` (CD) approach to shipping software has been around for a few years now, when implemented in teams it allows them to produce software in rapid cycles, ensuring that the software can be reliably released at any time. It aims at building, testing, and releasing software faster and more frequently. The approach helps reduce the cost, time, and risk of delivering changes by allowing for more incremental updates to applications in production. `HawkCD` helps teams adopt CD practices in the Software Development Life Cycle (SDLC) by giving them the freedom to create CD Pipelines to model their release processes.

Anatomy of a CD Pipeline
--------------------------

*The Deployment Pipeline* is a central concept in the CD approach to shipping software. At an abstract level, a deployment Pipeline is an automated implementation of the software release process for getting software from version control to the market. Usually every change in the software goes through a complex process before being released. The process may involve building the source code, followed by a progression of these builds through numerous Stages where the product quality gets assessed. The Deployment Pipeline becomes a central collaboration hub for the software delivery team. Ability to automate the process is crucial for overall team productivity.

The CD Pipeline breaks down a software delivery process into Stages. The aim of each Stage is to verify the software quality from a different angle, validate the functionality and prevent errors from affecting users. The CD Pipeline provides feedback to the team and visibility into the flow of changes to everyone involved in the delivery.

There is no such thing as *Standard Deployment Pipeline*, however a typical CD Pipeline will include some, or all, of the following stages: Check-in, Acceptance, Performance Tests, Production Deployment.

![Screenshot](../img/CD_Pipeline1.png)

Server Objects & Concepts
=========================
The following section represents a deep dive into the `HawkCD` Server concepts, components and objects.

Pipeline
---------
### Overview

As discussed previously the Pipeline allows crafting the entire application release process from start to finish. A Pipeline consists of Stages, which in turn consist of Jobs, which consist of Tasks.

### How does it work?

After a Pipeline is configured it is ready to be executed. Upon each execution a new Pipeline run is created with its own Status and RunId (starting from 1). When started, a Pipeline always begins with Status `IN_PROGRESS`. Upon successful completion its Status is set to `PASSED` and if something goes wrong and it doesn't manage to complete its Status is set to `FAILED`. </br>
A Pipeline can be paused or canceled during its execution setting its Status to `PAUSED` or `CANCELED` respectively. </br>
There is also the `AWAITING` Status, which means that an action by the user must be taken to continue the execution of the Pipeline. Usually this is caused by a Stage, which is set to be Manually Triggered or there being no Assignable Agents.

### Configuration Options

The Pipeline provides the following configuration options:

* `Automatic pipeline scheduling` - If selected, the Pipeline will trigger automatically, creating a new run, when its Material is updated.

Pipelines also have `Environment Variables`, which can be overridden by Stage `Environment Variables`. To see how they work, please see [Environment Variables section](/concepts/#environment-variables).

### Pipeline Scenarios

* [Add new Pipeline](/configuration/#add-pipeline)
* [Configure Pipeline](/configuration/#manage-pipelines)
* [Delete Pipeline](/)

Stage
-------
### Overview

A Stage can be thought of as a container for Jobs. Stages are a major component when it comes to automation release processing. Each step of building a new feature into a large project can be separated into Stages.

### How does it work?

As mentioned before a Pipeline consists of multiple Stages, all of each are executed in sequence, which means the previous Stage must complete successfully in order for the next to start. If all Stages pass, the Pipeline's Status is set to `PASSED`. If one fails, all Stages after it will not be run and the Pipeline's Status is set to `FAILED`. </br>
Stages have the same Statuses as Pipelines: `PASSED`, `FAILED`, `IN_PROGRESS`, `PAUSED`, `CANCELED`, `AWAITING`.

<!-- * Check-in    
* Assemble    
* Acceptance
* Performance
* Production Deployment -->

### Configure Options

The Stage provides the following configuration options:

* `Stage Trigger`
    * `On Success` - set by default. If selected the Stage will trigger automatically if the one before it completed successfully (has Status`PASSED`).
    * `Manual` - if selected, the execution of the Pipeline will stop at this Stage. Both Pipeline and Stage will be with Status `AWAITING` until the user decides to continue the process and manually triggers the Stage.

Stages also have `Environment Variables`, which can be overridden by Jobs `Environment Variables`. To see how they work, please see [Environment Variables section](/concepts/#environment-variables).

### Stage Scenarios

* [Add ](/configuration/#add-stage)
* [Delete ](/configuration/#delete-stage)
* [Configure](/configuration/#configure-stage)

Job
-----
### Overview

A Job consists of multiple Tasks. Jobs are assigned to Agents and then executed by them.

### How does it work?

Unlike Stages, which are always executed in sequence, Jobs are executed in ``parallel`` among a grid of Agents.</br>
`Resources`, also called `Tags`, can be used to route Jobs to specific Agents. A Job with a specific `Resource` can be executed only by an Agent with the same `Resource` assigned. Also a``Job`` may have more than one resource assigned.

When a Job doesn't have a concrete Resource assigned, it considers all available Agents registered with the Server for execution.

While one Job is executing, another may wait for the same [Agent](#agent) or for an eligible [Agent](#agent) which may not even be registered with the server. In this scenario, the Pipeline it belongs to is set to status ``AWAITING`` and will resume execution as soon as an eligible [Agent](#agent) becomes registered and enabled with HawkCD.

Jobs may contain [Environment Variables](#environment-variables). They become available during task execution and can be read from task by using percentage symbol notation e.g. ``%EnvironmentVariable%``

To learn more about  how environment variables work, check the [Environment Variables](#environment-variables) section.

### Configuration Options
``Job`` may be created first and decided to be configured later. Its configuration options include updating job name, adding, editing or deleting a task list.
Overriding environment variable, adding resource

* [add/delete job](/configuration/#add-delete-job)
* [configure job](/configuration/#configure-job)

Task
--------------
### Overview

A Task is an action that is performed on a server/machine or inside a container where a `HawkCD` Agent is installed. Tasks are contained inside Jobs and are always executed in the order defined in the Job. `HawkCD` offers 4 types of Tasks - `Exec`, `Fetch Material`, `Upload Artifact` and `Fetch Artifact`.

### Configuration Options

All Tasks have a `Run If Condition` option. Available options are `Passed` (set by default), `Failed` and `Any`.

* `Passed` - if selected, the Task will be executed only if the previous one completed successfully. If the Task is first in order, then the `Run If Condition` is ignored.
* `Failed` - if selected, the Task will be executed only if the previous one failed to complete successfully.
* `Any` - if selected, the Task will be executed regardless of the status of the previous one.

### Task Scenarios

* [Add](/configuration/#add-task)
* [Configure](/configuration/#configure-task)
* [Delete](/configuration/#delete-task)

Exec
--------------
### Overview
The ``Exec`` task is the most universal type of task `HawkCD` provides. It allows you to do just about anything you can think of on the server the task is executed on. You can run scripts (e.g. ``PowerShell``, ``Shell``) or execute any console (e,g, ``Command Prompt``, ``Terminal``) command.

### How does it work?

The `Exec` Task contains the following attributes: `Command`, `Arguments` and `Working Dir`.

* `Command` - executable name to run, usually for Linux -`/bin/bash`, for Windows `cmd`.
* `Arguments` - arguments to be passed to the executable (e.g., Linux - `-c cp -r dir dir1` , Windows `/c echo %PATH%`).
* `Working Dir` - the directory in which the process will run, relative to the root directory of the Server.

Example commands:

On Linux:

```bash

  #copy all files

  /bin/bash -c cp -r dir1/  dir2

```
The above command runs the `bash` executable, creates a new process with `-c` and passes the `cp` command with the `-r dir1/ dir2` arguments to it.

On Windows:

```bash

   #display %PATH%

   cmd /c echo %PATH%

```
The above command runs the `cmd` executable, creates a new process with `/c` and passes the `echo` command with the `%PATH%` argument to it.


  <div class="admonition note">
  <p class="admonition-title">Note</p>
  <p>
  You can think of the `HawkCD` `Exec` Task as a universal command executor. In fact it can run any command via `cmd` or `/bin/bash`, `/bin/sh` as long as it's in the environment path (system | user) of the system. Otherwise, the `Command` value must be a direct path to the targeted executable on the system.
  </p>
  </div>

### Configuration Options

The `Exec` Task provides 1 configuration option:

 * ``Ignore Errors`` - If selected, the Task's status is set to `PASSED`, regardless if it completed successfully or not.

Fetch Material
---------------
###Overview

The `Fetch Material` Task allows the user to fetch [Materials](/concepts/#materials) already defined within the system. At the moment the only type of Material `HawkCD` supports is `Git`, meaning that you can define and fetch only Materials of type `Git` as input for your Pipelines. Future versions may support other types (e.g., `TFVC`, `SVN`, etc.). A common use case is when you need to build your source code, but before doing so you need to fetch it on an Agent first.

### How does it work?

The `Fetch Material` Task clones a Git repository directly from the project's source to a `HawkCD` Agent.

### Configuration Options

``Fetch Material`` Task provides one configuration option:

* `Material` - the predefined material to be fetched. At the moment `HawkCD` supports only one Material per Pipeline. Support for multiple Materials will be added to future versions.

Upload Artifact
----------------
### Overview

The `Upload Artifact` Task allows users to upload build artifacts to a `HawkCD` Server. A common use case is when source code is compiled and the build output is stored on the Server via the `Upload Artifact` Task, then using `Fetch Artifact` to deploy artifacts to the appropriate Agent.

### How does it works?

The `Upload Artifact` Task takes a file or a folder (usually created by an Agent), archives it by creating a .zip file and sends it to the Server. Once on the Server the archive is unzipped and stored in the Artifacts folder.

### Configuration Options

The `Upload Artifact` Task provides 2 configuration options:

* `Source` - path to the Artifact starting at `../Agent/Pipelines/<PipelineName>/`. If no `Source` is selected the entire contents of the folder are uploaded.
* `Destination` - a folder or folders to be created where the Artifact is stored. If no `Destination` is selected the Artifact is saved in `../Server/Arttifacts/<PipelineName>/<PipelineRun>/` with no additional folders.

  <div class="admonition note">
  <p class="admonition-title">Note</p>
  <p>
  The `Upload Artifact` Task uses paths relative to the Agent sandbox. Artifacts must be in `../Agent/Pipelines/<PipelineName>/` folder to be uploaded to the `Server`.   
  </p>
  </div>

Fetch Artifact
---------------
### Overview

The `Fetch Artifact` Task allows users to download Artifacts from the Server repository to a `HawkCD` Agent sandbox.

### How does it work?

The `Fetch Artifact` Task takes a file or a folder previously stored on the Server, archives it by creating a .zip file and sends it to the Agent. Once on the Agent the archive is unzipped and stored in the currently executing Pipeline's folder.

### Configuration options

The `Fetch Artifact` Task provides 4 configuration options:

* `Pipeline` - the name of the Pipeline which previously uploaded the Artifact. The user can select any Pipeline for which he/she has at least permission type `Viewer`.
* `Run` - the specific Pipeline run which previously uploaded the Artifact. The `latest` option is useful when the user wants to fetch an Artifact the will be uploaded with the current execution of the Pipeline.
* ``Source``  - path to the Artifact starting at `../Server/Artifacts/<PipelineName>/<PipelineRun>/`. If no `Source` is selected, the entire contents of the folder are fetched.   
* ``Destination`` - a folder or folders to be created where the Artifact is fetched. If no `Destination` is selected the Artifact is saved in `../Agent/Pipelines/<PipelineName>/` with no additional folders.

Pipeline Group
---------------

#### Overview
``Pipeline Group`` can be thought as a container for Pipelines. Each Pipeline belongs to a group (unless it's unassigned).


#### How it works?
Grouping pipelines helps managing, sharing and restricting Pipelines among teams.  
Each ``Pipeline Group`` can be configured to belong to a specific ``User Group`` (e.g. QA, Software Engineers, DevOps, etc). To learn more, see [User Group section](/concepts/#user-groups).<br>
Each permission rule of the ``Pipeline Group`` applies to all of the Pipelines in the group, unless the Pipeline has its own permission rule. When it does, it overrides the ``Pipeline Group``'s rule.


#### Configuration Options
There are 2 options available for each ``Pipeline Group``:    

 * ``Assign Pipeline`` - Assigns a Pipeline to the current Pipeline Group regardless of whether it's already assigned or not.   

 * ``Unassign Pipeline``. Unassigns a pipeline from the current Pipeline Group. The unassigned Pipeline moves to the ``UnassignedPipelines`` Pipeline Group.

### Pipeline Scenarios
 * [Add Pipeline Group](/)
 * [Assign Pipeline](/)
 * [Unassign Pipeline](/)
 * [Delete Pipeline Group](/)

 <div class="admonition note">
 <p class="admonition-title">Note</p>
 <p>
A ``Pipeline Group`` can only be deleted if there are no Pipelines assigned to it.
 </p>
 </div>




Materials
---------
#### Overview
A ``Material`` represents a branch of a ``Git`` repository. It is used to define which part of your project(s) the Pipeline will work with. For every Pipeline there
must be at least one ``Material`` defined.

  <div class="admonition note">
  <p class="admonition-title">Note</p>
  <p>
  At the moment ``HawkCD`` only supports ``Git`` for its ``Materials``, but future versions will provide
  support for other types of materials.
  </p>
  </div>


#### How it works?
A Material is assigned to a Pipeline the moment it is created. When a Pipeline is run it is updated locally on the Server to the latest Git commit. A ``Material`` can be set to trigger your Pipeline to run automatically using Automatic Pipeline Scheduling. When selected, ``HawkCD`` automatically tracks your Material and fetches the latest
version of it. When a change is made (e.g. a commit is pushed) ``HawkCD`` will automatically trigger your Pipeline.
<!-- if ``Poll for changes`` is selected. -->

#### Configuration Options
Materials contain the following options:

 * ``Material Name`` - the name of the Material. Once the Material is created the name cannot be changed.
 * ``Git URL`` - URL to your GitHub project repository (the one you would use to clone a repository). This option requires that the URL is valid and ends with ".git" (e.g., https://github.com/rndsolutions/hawkcd.git). SSH URLs are currently not supported.
 * ``Git Branch`` - the git branch to track from (e.g. master). If nothing is entered it will default to "master".
 * ``Credentials`` - your GitHub credentials. If the git repository is private, you will need to provide your GitHub credentials, so that ``HawkCD`` can access your repository.

Resource | Tags
---------------

Resources are used to assign Jobs to Agents. A common use case is when you have multiple Agents installed on various server environments and you want to specify a certain Job to be executed on a specific Agent. Imagine you have a Build Server and an Application Server. You wouldn't want your build to be assigned to the Application Server when you kick off a "Build Stage" -> "Build Job", so applying a Resource to the Job, as well as to the Agent, would allow ``HawkCD`` to correctly assign Jobs only to the matching Agents.

The Server tries to assign Jobs to Agents with the exact Resources the Job has. It prioritizes Agents that have the least amount of extra resources. Agents must have at least all of the Job's Resources to be assigned that Job. If a Job has no Resources, it can be assigned to any Agent.

<div class="admonition note">
<p class="admonition-title">Note</p>
<p>
If there is no suitable Agent for a Job, the Server sets the Pipeline's Status to AWAITING.
</p>
</div>

Environment Variables
---------------------

###Overview

###How does it work?

Agent
------

### Overview

``HawkCD`` Agents are the workers that execute Jobs and their Tasks. All Tasks configured in the ``HawkCD`` Server run on ``HawkCD`` Agents. A common workflow is when the ``HawkCD`` Server detects a change in the Material and a Pipeline is triggered, the corresponding Jobs are assigned to eligible Agents and all Tasks in that Job are executed by that Agent. The Agent then returns the result of the Job back to the Server.

###How does it work?
In the directory the Agent was installed (the ``InstallDir``), a folder called ``Pipelines`` (also referred to as the Agent's ``Sandbox``) is created to store data for each Pipeline. If we execute Jobs from two Pipelines named ``Pipeline1`` and ``Pipeline2`` on a specific Agent, we would have the following directory structure:<br>
``InstallDir/Pipelines/Pipeline1`` and ``InstallDir/Pipelines/Pipeline2``

When an Agent registers for first time with the Server, it defaults to ``Disabled``. Meaning that the Server knows about it, but unless it's enabled explicitly by the Server Admin, Jobs will not be assigned to it.  

  <div class="admonition note">
  <p class="admonition-title">Note</p>
  <p>
  If an Agent hasn't reported for a certain amount of time it's considered ``Disconnected``.
  </p>
  </div>


#### Agent Statuses

* ``Idle`` - the Agent is ready to accept Jobs.
* ``Running`` - the Agent is currently executing a Job. Further Jobs cannot be assigned to it until it completes the Job.


Security
--------
### Overview
The server has the notion of ``Permission Scope``, ``Permission Type`` and ``Permission Entity``. <br>

* ``Permission Scope`` represents the object level at which specific permissions can be applied. <br>
* ``Permission Types`` define permissions - what a user can do in specific ``Permission Scope``. <br>
* ``Permission Entity`` is the specific object(s) for which the Permissions are applied. <br>

 Combining all 3 concepts (``Permission Scope``, ``Permission Type`` and ``Permission Entity``) provides a flexible authorization model.

#### Permission Scopes

* ``Server`` - applies at all levels (all Pipeline Groups and Pipelines).
* ``Pipeline Group`` - applies at the Pipeline Group level.
* ``Pipeline`` - applies at the Pipeline level.

#### Permission Types

* ``Viewer``  - can only view a given resource and its child resources (e.g. if you're a Pipeline ``Viewer`` you can view all Stages, Jobs and Tasks in that Pipeline aswell).
* ``Operator`` - can view and operate (run, re-run, pause, stop, etc.) a given resource and its child resources (e.g. if you're a Pipeline ``Operator`` you can also run its Stages aswell).
* ``Admin`` - can view, operate and configure (edit) a given resource and its child resources (e.g. if you're a Pipeline ``Admin`` you can configure its Stages, Jobs and Tasks aswell).

#### Permission Entities

There are 2 types of ``Permission Entities`` - specific and generic. <br>
Specific ``Permission Entities`` refer to a single object in a given ``Permission Scope`` (e.g. a Pipeline or Pipeline Group), while generic ``Permission Entities`` refer to all objects in a given ``Permission Scope`` (e.g. all Pipelines or all Pipeline Groups).

#### Permission Inheritance and Overriding

Permissions in ``HawkCD`` follow a set of inheritance rules. Objects higher in the hierarchy (e.g. Pipeline Groups are on a higher level than Pipelines) apply their permissions to all objects that belong to it, but can be overriden by permissions for objects that are lower in the hierarchy, even if they belong to the higher object.

For example, let's say that you have a ``Viewer`` permission for a specific Pipeline Group. That means you can view, but not operate or configure all of the Pipelines inside that Pipeline Group aswell. However, if you have an ``Admin`` permission for a Pipeline inside that Pipeline Group, you will be able to configure that Pipeline specifically, because that permission is on a lower level than the Pipeline Group permission you have. In other words, it's more specific, thus more important.

<div class="admonition note">
<p class="admonition-title">Note</p>
<p>
The Pipeline level overrides all other levels. The Pipeline Group level overrides the Server level. The Server level doesn't override any other level.
</p>
</div>

#### Users and User Groups

Users can be assigned to User Groups. User Groups aim to make managing the Permissions of multiple Users at the same time easier, because both Users and User Groups have Permissions. This provides a lot of flexibility to how you build your Permissions. Users' Permissions override the Permissions of his User Group.

For example, let's say you have 2 developer teams. We'll call them "DevTeam1" and "DevTeam2". DevTeam1 has their own set of Pipelines and Pipeline Groups, however so does DevTeam2. In this case you can create 2 User Groups called "DevTeam1" and "DevTeam2". You can assign all of the members of your two developer teams to their respective User Groups and manage all of their Permissions at once by setting the Permissions on the User Group itself, rather than on every member individually. This makes changes to the Permissions of each team very fast and easy. Let's say that both teams have a Development Lead and they need to be able to edit certain Pipelines, but you don't want the whole team being able to edit those Pipelines, just operate them. You can set the 2 User Groups to have a ``Viewer`` Permission for their Pipeline Groups and ``Operator`` Permissions for the Pipelines they need to be able to run. Then, you can go to each of the individual Development Leads and give them an ``Admin`` Permission for each of the Pipelines or Pipeline Groups they need to be able to edit. This will override their ``Viewer`` Permission from the User Group. In doing so, all of your developers can view, run, re-run, pause and cancel their Pipelines and your Development Leads can additionally edit some or all of those Pipelines too.
