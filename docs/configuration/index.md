HawkCD Configuration
==================

Accessing the server (Login)
--------
Accessing a fresh installation of `HawkCD` comes with a pre-defined administrator account.

* `user`: admin@admin.com,
* `password`: admin
* `server url`: http://localhost:8080


<div class="admonition warning">
<p class="admonition-title">WARNING</p>
<p>
  It is highly recommended to change the default password of the administrator account after the first login.
</p>
</div>

Add Pipeline
-----------------
* Chose a Pipeline Group
* Click the `Add` button
* Fill in `Pipeline Name` and press `Continue`
* Choose either existing or create a new Material
* Review your Pipeline settings and press the `Submit` button

<div class="admonition note">
<p class="admonition-title">Note</p>
<p>
  For complete configuration of the pipeline definition navigate to the pipeline config screen where additional jobs and stages can be created to suit your deployment process
</p>
</div>


Configure Pipeline
-----------------------
Once a Pipeline is created additional configuration is required to get it building and deploying your code. The default Pipeline wizard creates a Pipeline with default Stage, Job and Task.

* Go to Pipeline Screen
* Choose a Pipeline
* Press `Configure` button (represented by the cogwheel icon).
* Select `Pipeline Settings` to edit `Pipeline Name` and `Automatic Pipeline scheduling` options
* Select `Stages` tab to add, edit and delete Stages
* Select `Environment Variables` tab to add, edit and delete Environment Variables

<div class="admonition note">
<p class="admonition-title">Note</p>
 If a Pipeline has only one Stage it cannot be deleted.
</div>

Here's an example Pipeline that can be set-up with `HawkCD`:

Pipeline Name: HawkCD
Stages: Check-in, Assemble, Acceptance, Performance, Production Deployment

![Screenshot](../img/hawkcd-config.png)

### Configure Stage

* Go to Pipeline Screen
* Choose a Pipeline
* Press `Configure` button (represented by the cogwheel icon).
* Choose a Stage
* Select `Stage Settings` to edit `Stage Name` and `Stage Trigger` options
* Select `Jobs` tab to add, edit and delete Jobs
* Select `Environment Variables` tab to add, edit and delete Environment Variables

<div class="admonition note">
<p class="admonition-title">Note</p>
 If a Stage has only one Job it cannot be deleted.
</div>

### Configure Job

* Go to Pipeline Screen
* Choose a Pipeline
* Press `Configure` button (represented by the cogwheel icon).
* Choose a Stage
* Choose a Job
* Select `Job Settings` to edit `Job Name`
* Select `Tasks` tab to add, edit and delete Tasks
* Select `Environment Variables` tab to add, edit and delete Environment Variables
* Select `Resources` tab to add, edit and delete Resources

<div class="admonition note">
<p class="admonition-title">Note</p>
 If a Job has only one Task it cannot be deleted.
</div>

Running a Pipeline
--------------------


Manage Pipeline Groups
----------------------
### Add new Pipeline Group

* Go to Admin Screen
* Select `Pipeline Groups` tab and press the `Add` button
* Fill in the group name and press `OK` button

### Assign Pipeline to Pipeline Group
* Go to the Admin Screen
* Select `Pipeline Groups` tab and choose a Pipeline Group
* Press the `Assign pipeline` button
* All Pipelines `assigned` to other groups and all `unassigned` Pipelines will be displayed
* Select a Pipeline and press the `OK` button

### Delete Pipeline Group

* Go to the Admin Screen
* Select `Pipeline Groups` section and choose a group
* Press the bin button in the right corner of the group

<div class="admonition warning">
  <p class="admonition-title">WARNING</p>
    <p>A Pipeline Group can be deleted only if it doesn't have any pipelines assigned to it.</p>
</div>

Manage Materials
----------------
### Add new Material

* Go to Admin Screen
* Select `Materials` section
* Press the `Add` button
* Fill in the fields and press `OK`

### Update Existing Material

* Go to Admin Screen
* Select `Materials` section
* Press the `Edit` button
* Update the Material's fields and press `OK`

### Delete Existing Material

* Go to Admin Screen
* Select `Materials` section
* Press the `Delete` button and press `OK`

Manage User Groups
---------------------
### Add User Group

* Go to Admin Screen
* Select `User Groups` section
* Press `Add` button
* Fill in the group name and click ``OK``

### Manage User Group permissions

* Go to Admin Screen
* Select `User Groups` section
* Choose a ``User Group`` by clicking the ``cog`` button
* Choose to add/update/delete the group's `scope` and `permissions` and click `OK`

### Assign/Remove users to/from a group

* Go to the Admin Screen
* Select ``User Groups`` menu
* Expand a group and press the ``manage`` button
* Select a user to be assigned to the group
* Press the ``OK`` Button

### Delete a User Group
* Go to the Admin Screen
* Select ``User Groups`` menu
* Click the ``bin`` icon from the header of the group
* Confirm the action by pressing ``ok`` button

<div class="admonition warning">
<p class="admonition-title">WARNING</p>
<p>
  Deleting a user group is possible if it doesn't have any users assigned to it
</p>
</div>

### Create a new user

* Go to the Admin Screen
* Select ``Users`` menu
* Press the ``Add`` button
* Enter user's email and password
* Press the ``OK`` button


### Edit User
* Go to the Admin Screen
* Select ``Users`` menu
* Press the ``cog`` button

<div class="admonition note">
<p class="admonition-title">Note</p>
<p>
  At the moment HawkCD supports only resetting user password and changing email
</p>
</div>


### Manage permissions for user

* Go to the Admin Screen
* Select ``Users`` menu
* Choose a ``User`` by clicking the ``manage`` button
* From the pop-up window ``add/update/delete``  ``scope`` and ``permissions`` for the user


<div class="admonition note">
<p class="admonition-title">Note</p>
<p>
  Note that if the user has any inherited permissions from a group it participate in, they will be overwritten.
</p>
</div>

### Enable/Disable Users

* Go to the Admin Screen
* Select ``Users`` menu
* Press the ``enable/disable`` button for concrete user

<div class="admonition note">
<p class="admonition-title">Note</p>
<p>
  Disabled users will no longer be able to access (login) to the system
</p>
</div>

Manage My Profile
---------------

The logged in user into the server is allowed to change his password


Agents Management
-----------------

``HawkCD`` [Agents](/concepts/#agent) are the workers that execute Jobs/Tasks.

### Enable/Disable Agent

* Go to the Agents screen
* Press the ``enable/disable`` toggle button

<div class="admonition note">
<p class="admonition-title">Note</p>
<p>
  When an Agent connects to the server for first time it is disabled, it requires server administrator to enable it -  becomes available for job
</p>
</div>

### Add/Remove agent resources

* Go to the Agents screen
* Press the ``edit resources`` button
* ``Add/Update/Delete`` agent [resources](/concepts/#resource-tags)

<div class="admonition note">
<p class="admonition-title">Note</p>
<p>
  Resources are used to route jobs to agents
</p>
</div>

Artifacts Repository
--------------------

### Download Artifacts
