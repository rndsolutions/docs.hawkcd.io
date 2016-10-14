
Continuous Delivery Overview
==============================
The `Continuous Delivery` (CD) approach to shipping software has been around for few years now, implemented in teams allows them to produce software in rapid cycles, ensuring that the software can be reliably released at any time. It aims at building, testing, and releasing software faster and more frequently. The approach helps reduce the cost, time, and risk of delivering changes by allowing for more incremental updates to applications in production. `HawkCD` helps teams to adopt CD practices in the Software Development Life Cycle (SDLC) by giving them the freedom to create CD Pipelines to model their release processes.

Anatomy of a CD Pipeline
--------------------------

*The Deployment Pipeline* is a central concept in the CD approach to shipping software. At an abstract level, a deployment Pipeline is an automated implementation of the software release process for getting software from version control to the market. Usually every change in the software goes through a complex process on its way to being released. The process may involve building the source code, followed by a progression of these builds through numerous Stages where the product quality gets assessed. The Deployment Pipeline becomes a central collaboration hub for the software delivery team. Ability to automate the process is crucial for the team productivity.

The CD Pipeline breaks down a software delivery process into Stages. The aim of each Stage is to verify the software quality from a different angle, validate the functionality and prevent errors from affecting users. The CD Pipeline provides feedback to the team and visibility into the flow of changes to everyone involved in the delivery.

There is no such thing as *Standard Deployment Pipeline*, however a typical CD Pipeline will include some, or all, of the following stages: Check-in, Acceptance, Performance Tests, Production Deployment.

![Screenshot](../img/CD_Pipeline1.png)

Server Objects & Concepts
=========================
The following section represents a deep dive into the `HawkCD` Server concepts, components and objects.

Task
--------------
### Overview

A Task is an action that is performed on a server/machine or inside a container where a `HawkCD` Agent is installed. Tasks are contained inside Jobs and are always executed in the order added to the Job. `HawkCD` offers four types of Tasks - `Exec`, `Fetch Material`, `Upload Artifact` and `Fetch Artifact`.

### Configuration Options

All Tasks have a `Run If Condition` option. `Passed` (set by default), `Failed` or `Any` are the selectable choices.

* `Passed` - if selected, the Task will be executed only in case the one before it completed successfully. If the Task is to be executed first the option is ignored.
* `Failed` - if selected, the Task will be executed only ...
* `Any` - if selected, the Task will be executed regardless of the status of the previous one.

Exec
--------------
### Overview
The ``Exec`` task is the most universal type of tasks HawkCD provides. It allows to do just anything you can think of on a server where the task is executed on. You can run script, e.g. ``PowerShell``, ``Shell``, execute commands etc.

The `Exec Task` is the most universal type of Task `HawkCD` offers. It allows you to do just about anything you can think of on a given Server where the Task is executed. You can run scripts (e.g., PowerShell, Shell), execute commands, etc.

### How does it work?

The `Exec Task` contains the following attributes: `Command`, `Arguments` and `Working Dir`.

* `Command` - executable name to run, usually for Linux -`/bin/bash`, for Windows `cmd`
* `Arguments` - arguments to be passed to the executable (e.g., Linux - `-c cp -r dir dir1` , Windows `/c echo %PAHT%`)
* `Working Dir` - the directory in which the process will run

Example commands:

```bash

  #copy all files

  /bin/bash -c cp -r dir1/  dir2

```
The above command runs on Linux based systems the `bash` executable and passes the `cp` command with arguments to it

  <div class="admonition note">
  <p class="admonition-title">Note</p>
  <p>
  You can think of `HawkCD` `Exec Task` as a universal command executor. In fact it can run any command via `cmd` or `/bin/bash`, `/bin/sh` as long as it's in the environment path (system | user) of the system, otherwise the `Command` value must be a direct path to the targeted executable on the system.
  </p>
  </div>

### Configuration Options

The `Exec Task` provides a single configuration option:

* ``Ignore Errors ``

 ``Ignore Errors`` - Ignore errors, if there are any, sets task status to ``Passed``.

 <div class="admonition warning">
 <p class="admonition-title">WARNING</p>
 <p>
 Be aware that``FAILED`` task will run only in case the previous task exits with status failed code, different than ``0``
 </p>
 </div>

### Exec Tasks Scenarios

* [Add ](/configuration/#add-task)
* [Delete ](/configuration/#delete-task)
* [Configure](/configuration/#configure-task)


Fetch Material
---------------

###Overview

The ``Fetch Material`` task allows to fetch already defined materials with the system. At the moment ``HawkCD`` supports only materials of type ``git`` meaning that you can define and fetch materials of type git only as input to your pipelines. Future versions it may support other types e.g. ``TFVC``, ``SVN`` etc. A common use case is when you need to build your source code but before doing it you need to fetch it on an agent first.

### How does it work?

The ``Fetch Material`` task clones git repository directly from projects source into ``HawkCD`` Server and Agent. <br />
``Fetch Materials`` task contains one attribute - [``material``](/#materials). <br /> </br >
At the moment ``HawkCD`` supports only one material per pipeline. Multiple materials (e.g. sources) may be added into feature versions.

### Configuration Options
``Fetch Material`` task provides one configuration option - [Run If Condition](/concepts/#run-if-condition)

### Fetch Material Tasks Scenarios

* [Add ](/configuration/#)
* [Delete ](/configuration/#)
* [Configure](/configuration/#)


Upload Artifact
----------------

### Overview
The ``Upload Artifacts`` task allows uploading build artifacts to HawkCD server. A common use case is when source code is compiled and the build output is stored to the server via the ``Upload Artifacts`` task, then using ``Fetch Artifacts`` to deploy artifacts to appropriate agent.

### How does it works?


The ``Upload Artifact`` task provides two attributes: ``Source`` and ``Destination``.   

``Source``  - Path to ``Artifact``.  
``Destination`` - Path to server destination where artifacts to be stored (optional).

Upload Artifact task uses relative paths to the agent sandbox.
Artifact must be  must be on the agents directory in order
to be uploaded to the ``Server``.   

Full path to ``Source`` - ``Agent/Pipelines/<PipelineName>/``.   
Full path to ``Destination`` - ``Server/Arttifacts/<PipelineName>/<PipelineRun>/``.



### Configuration Options

The ``Upload Artifact`` task provides one configuration option - [``Run If Condition``]()

 ``Run If Condition`` - runs under three different scenarios: ``Passed``, ``Failed`` and ``Any``.
 If option ``Passed`` (default) is chosen, the execution of the current task will be continued only in case the previous task completed successfully - ``Passed``
 If the option ``Failed`` is chosen it will be run only in case the previous task is marked as ``Failed``.
 A task set to ``Any`` will always run, regardless of previous task status (``Passed`` | ``Failed``).

### Upload Artifact Tasks Scenarios

 * [Add ](/configuration/#)
 * [Delete ](/configuration/#)
 * [Configure](/configuration/#)




Fetch Artifacts
---------------
### Overview
The ``Fetch Artifact`` task allows users to download artifacts from the server repository into a ``HawkCD`` agent sandbox.

### How does it work?
``Fetch Artifact`` downloads artifact from ``Server`` to agent sandbox.  
The fetch artifacts contains the following attributes: ``Pipeline``, ``Run``, ``Source`` and ``Destination``.

* ``Pipeline`` - Which pipeline.
* ``Run`` - Which pipeline run.
* ``Source``  - Path to ``Artifact``.  
* ``Destination`` - Path to server destination where artifacts to be stored (optional).

### Configuration options
The ``Fetch Artifact`` task provides one configuration option - ``Run If Condition``

 ``Run If Condition`` - runs under three different scenarios: ``Passed``, ``Failed`` and ``Any``.
 If option ``Passed`` (default) is chosen, the execution of the current task will be continued only in case the previous task completed successfully - ``Passed``
 If the option ``Failed`` is chosen it will be run only in case the previous task is marked as ``Failed``.
 A task set to ``Any`` will always run, regardless of previous task status (``Passed`` | ``Failed``).

### Fetch Artifact Tasks Scenarios

* [Add ](/configuration/#)
* [Delete ](/configuration/#)
* [Configure](/configuration/#)

Job
-----

### Overview

A ``job`` consists of multiple ``tasks``, each of which executes in order. If ``task`` inside a ``job`` fails, then the ``job`` is considered failed, and unless specified otherwise, the rest of the tasks in the will not be run.

### How does it work?

Unlike ``Tasks`` and ``Stages``, which are always executed in sequence, ``Jobs`` are executed in ``parallel`` among Agent's grid.
``Resources``, also called ``tags``, can be used to route jobs to specific agents. When job doesn't have a concrete resource assigned, it considers all available agents registered with the server for execution.

While one Job is executing, another may wait for the same [Agent](#agent) or for an eligible [Agent](#agent) which may not even be registered with the server. In this scenario, the Pipeline it belongs to is set to status ``AWAITING`` and will resume execution as soon as an eligible [Agent](#agent) becomes registered and enabled with HawkCD.

Job without resources can be executed from all agents. Job with a specific
[resource](#resource) may be executed only from an [agent](#agent) with the same resource assigned. Also a``Job`` may have more than one resource assigned.

Jobs may contain [Environment Variables](#environment-variables). They become available during task execution and can be read from task by using percentage symbol notation e.g. ``%EnvironmentVariable%``

To learn more about  how environment variables work, check the [Environment Variables](#environment-variables) section.

### Configuration Options
``Job`` may be created first and decided to be configured later. Its configuration options include updating job name, adding, editing or deleting a task list.
Overriding environment variable, adding resource

* [add/delete job](/configuration/#add-delete-job)
* [configure job](/configuration/#configure-job)

Stage
-------
### Overview
A ``Stage`` can be thought as a container for ``Jobs``. While ``Jobs`` are run in parallel, ``Stages`` are always run in sequence. If a ``Job`` from particular stage fails, then the Stage is considered failed as well.
However, since Jobs are independent of each other, all other Jobs in the Stage will also be run. Stages that belong to a certain pipeline are always run in sequence.

### How does it work?
Stages are major component when comes to automation release processing. Each step of building new feature into a large project
may be divided into few steps/stages:

* Check-in    
* Assemble    
* Acceptance
* Performance
* Production Deployment

Since stages run in a sequence, each of the previous stages (e.g. steps) must complete successfully
in order next stage to start. If a ``Stage`` fails next ``Stage`` does not start, ``Pipeline`` is set to FAILED.

### Configure Options

Each `Stage` has a ``Stage Trigger`` reason: ``Manual`` and ``On Success``.

When ``Manual`` stage  trigger is selected, stage pauses and awaits to be run manually. Pipeline status is set to AWAITING.

``On Success`` start stage when previous stage has ``PASSED`` successfully. If previous stage fails, next stage does not start executing.
Pipeline status is set to FAILED.

Stages also have ``Environment Variables``, which can be overridden by jobs environment variables. To see how environment variables work, please
check [``environment variables section``](/concepts/#environment-variables).


### Stage Scenarios

* [Add ](/configuration/#add-stage)
* [Delete ](/configuration/#delete-stage)
* [Configure](/configuration/#configure-stage)

Pipeline
---------

### Overview
A ``Pipeline`` consists of multiple Stages, each of which is run in order. If a Stage fails, then the Pipeline is considered failed and the rest of the stages will not be run. The ``Pipeline`` server object allows crafting the entire application release process

###How does it work?

We briefly view  [Anatomy of a Pipeline](/concepts/#anatomy-of-a-cd-pipeline) .<br />
All Pipelines have a set of Stages which have a set of Jobs which have a set of Tasks.

There are four different types of tasks in ``HawkCD`` - [Exec](/concepts/#exec), [Fetch Artifact](/), [Fetch Material](/) and [Upload Artifact](/). In order Pipeline run to be set to
status _passed_, task action must complete successfully, unless the task is marked otherwise with [Ignore Errors](/) option.

If all tasks in a certain ``Job`` complete successfully, the ``Job`` is set to _passed_, all jobs must be with status _passed_ for a ``Stage`` to pass. All Stages must pass for ``Pipeline``
run to complete successfully. If a ``Task`` fails the ``Job`` is set to _failed_ respectively ``Pipeline`` status is set to _failed_.

<div class="admonition note">
<p class="admonition-title">Note</p>
<p>
All Stages and Tasks runs in sequence, except Jobs. Execution starts in order, if one Task, Job, or Stage fails the next Task/Job/Stage will not run.
The Pipeline will be set to FAILED.
</p>
</div>

### Configuration Options
In HawkCD Pipelines can be managed very easily.   


You can [update pipeline name](/) or select [Automatic pipeline scheduling]. You can [add Stage](/), [add Job](/) or
[add Task](/) to already created Pipeline. Delete each one of them is also pretty straightforward, with just one click. Each ``Pipeline``, ``Stage``, ``Job`` or ``Task`` can be configured in details.   


Click [Configure Pipeline](/configuration/#pipeline-configuration) to see how to configure [Environment Variables](/) or how to add [Resources](/).


### Pipeline Scenarios

* [Add new Pipeline](/)
* [Configure Pipeline](/)
* [Delete Pipeline](/)




Pipeline Group
---------------

#### Overview
``Pipeline Group`` can be thought as a container for Pipelines. Each ``Pipeline`` belongs to a group.


#### How it works?
Grouping pipelines helps managing, sharing and restricting pipelines among teams.  
Each ``Pipeline Group`` can be configured to belong to a specif QA, Software Engineers,  
DevOps or any other team. Each rule of the ``Pipeline Group`` applies to all the  ``Pipelines`` in the group.  


Pipeline Group example:


//

#### Configuration Options
There are two options available to  a ``Pipeline Group``:    

 * ``Assign Pipeline`` - Assigns unassigned  or pipeline from another pipeline group to the current pipeline group.   

 * ``UnAssign Pipeline``. Unassigns pipeline. Pipeline moves to ``UnassignedPipelines`` pipeline group.

### Pipeline Scenarios
 * [Add new Pipeline Group](/)
 * [Assign Pipeline Group](/)
 * [Unassign Pipeline](/)
 * [Delete Pipeline Group](/)

 <div class="admonition note">
 <p class="admonition-title">Note</p>
 <p>
 ``Pipeline Group`` can be deleted only if there is no ``Pipeline`` assigned to it.
 </p>
 </div>




Materials
---------
grouping pipelines helps your work manage, share and restrict pipelines among teams.
#### Overview
Materials represent ``code`` (git) ``artifact`` (Nuget) repositories. For every pipeline there
should be at least one material defined.

<div class="admonition note">
<p class="admonition-title">Note</p>
<p>
At the moment HawkCD supports only materials of type git, but feature version will provide
support for other types of materials.
</p>
</div>


#### How it works?
A ``Material`` is cause for a Pipeline to run. ``HawkCD`` automatically tracks your material and fetches the latest
version of it. At the moment ``HawkCD`` supports only one ``material`` per pipeline, but in feature version multiple
materials could be assigned to a single pipeline.

#### Configuration Options
Materials contains the following attributes: ``Material Name``, ``Git Url``, ``Git Branch`` and ``Credentials``.

 * ``Material Name`` - with which identifies on the HawkCD server.
 * ``Git Url`` - URL to project git repository.

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

####Groups

A ``group`` is a set of claims (scope + permissions) that are grouped together. A ``group`` would ease the authorization management across groups of people. E.g. if we have 3 teams - dev, qa & ops, rather than assigning permissions individually to each team member, we would create a group and add scope and permissions to it, then add the members to the group, so that they inherit all of the ``group’s`` permissions.

#### Permission Inheritance

If a user is assigned a ``pipeline group`` scope and an ``admin permission`` type that would mean that all resources that are children of the current ``pipeline group`` (scope) e.g. one or more pipelines, will obey the permission assigned to their parent - pipeline group.

#### Overriding Permissions

This is the case when we want to give a user permissions at a given scope e.g. "pipeline group", however we need to either restrict or broaden the rights to one or more child resources, e.g. Pipelines.
Given is a Pipeline group named "Dev pipelines" and we want to have one of our teams to have view rights for the group. Combining the Pipeline Group scope and the view permission type would allow anyone of the team to see all pipelines. However, if we want the Development Lead of the team to be able to administer one or more pipelines from the group, but not all of them, we would assign in addition to its view rights inherited from the pipeline group scope, a pipeline scope with admin permission for a concrete pipelines that he needs administration rights for. In fact we'll override the inherited rights he received as part of the Pipeline group scope.
