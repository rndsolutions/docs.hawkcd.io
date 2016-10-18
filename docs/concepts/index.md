
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
A ``Material`` can be set to trigger your Pipeline to run automatically. ``HawkCD`` automatically tracks your Material and fetches the latest
version of it.
<!-- if ``Poll for changes`` is selected. -->

#### Configuration Options
Materials contains the following attributes: ``Material Name``, ``Git URL``, ``Git Branch`` and ``Credentials``.

 * ``Material Name`` - with which identifies on the ``HawkCD`` server.
 * ``Git URL`` - URL to project git repository.

Resource | Tags
---------------

Resources are used to route jobs to agents. A common use case is when you have multiple agents installed on various server environments and you want to specify a certain job to be executed on concrete agent. Imagine you have a build server and application server,  of course you don't want when you kick off a "Build Stage" -> "Build job" to be routed to the Application Server, so applying a resource to the job, as well as to the agent would allow HawkCD to correctly assign jobs only to the matching agents

The resources assigned to an agent and job must match 100% to get jobs routed correctly

> Assign resource to a job

![Screenshot](../img/assign_resource_to_job.png)

> Assign resource to an agent

![Screenshot](../img/assign_resource_to_agent.png)

Environment Variables
---------------------

###Overview

###How does it work?

Agent
------

### Overview

``HawkCD`` Agents are the workers that execute ``Jobs/Tasks``. All Tasks configured in the system run on ``HawkCD`` Agents. A common workflow is: The ``HawkCD`` Server checks for changes in Materials and when a change is detected a Pipeline gets triggered, the corresponding Jobs are assigned to eligible Agents and all tasks of theirs are executed.

###How does it work?
In the agent ``install dir`` a folder called ``Pipelines``, referred also as agent ``sandbox`` is created to store data for each pipeline. If we execute jobs from two pipelines on a specific agent we would have the following directory structure  ``InstallDir/Pipelines/Pipeline1`` and ``InstallDir/Pipelines/Pipeline2``

When an agent registers for first time with the server it is ``disabled`` meaning that, the server knows about it but unless it's enabled explicitly by the server Admin, jobs will be not be distributed to it  


#### Agent Statuses

* Idle - ready to accept jobs
* Running - executing job
* Disconnected - used to be connected but no longer available with the server


Security
--------
### Overview
The server has the notion for ``scope`` and ``permission type``. ``Scope`` represents a certain level from the server where specific rights can be applied. On the other hand, ``permission types`` define rights - what a user can do in concrete ``scope``. Combining both concepts (``scope`` & ``permission type``) provides a flexible authorization model.

#### Permission Scopes

* ``Server`` - global server scope
* ``Pipeline group`` - pipeline group level
* ``Pipeline`` - pipeline level scope

#### Permission Types

* ``Viewer``  - a user can only view a given resource and its child resources
* ``Operator`` - a user can view and operate (run, re-run, pause, stop, etc.) a given resource (e.g. Pipeline & Stage) and its child resources
* ``Admin``

#### User Groups

A ``group`` is a set of claims (scope + permissions) that are grouped together. A ``group`` would ease the authorization management across groups of people. E.g. if we have 3 teams - dev, qa & ops, rather than assigning permissions individually to each team member, we would create a group and add scope and permissions to it, then add the members to the group, so that they inherit all of the ``group’s`` permissions.

#### Permission Inheritance

If a user is assigned a ``pipeline group`` scope and an ``admin permission`` type that would mean that all resources that are children of the current ``pipeline group`` (scope) e.g. one or more pipelines, will obey the permission assigned to their parent - pipeline group.

#### Overriding Permissions

This is the case when we want to give a user permissions at a given scope e.g. "pipeline group", however we need to either restrict or broaden the rights to one or more child resources, e.g. Pipelines.
Given is a Pipeline group named "Dev pipelines" and we want to have one of our teams to have view rights for the group. Combining the Pipeline Group scope and the view permission type would allow anyone of the team to see all pipelines. However, if we want the Development Lead of the team to be able to administer one or more pipelines from the group, but not all of them, we would assign in addition to its view rights inherited from the pipeline group scope, a pipeline scope with admin permission for a concrete pipelines that he needs administration rights for. In fact we'll override the inherited rights he received as part of the Pipeline group scope.
