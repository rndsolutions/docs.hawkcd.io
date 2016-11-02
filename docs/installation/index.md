Requirements
===============

You can download the latest versions of the `HawkCD` Server and Agent for your OS from [here](http://downloads.hawkcd.io/).

<div class="admonition note">
<p class="admonition-title">Prerequisites</p>
<p>
  Java 1.8
</p>
</div>

Linux
------

For Linux-like operating systems `HawkCD` is distributed as a self containing package. It ships with embedded Redis database.

### Install & Run

Navigate to the directory where you downloaded the arhive.</br>
Run the following commands for the Server:

```sh
tar zxvf hawkcd.tar.gz
cd Server
chmod +x hawkcd.sh
./hawkcd.sh
```

Run the following commands for the Agent:
```sh
tar zxvf hawkcd-agent.tar.gz
cd Agent
chmod +x agent.sh
./agent.sh
```

### Upgrade

Before upgrading, make sure the Server is stopped.</br>
</br>
Download the archive where the Server folder is located and run the commands from above. After that navigate inside the folder and delete the old JAR file with the `rm` command.</br>
</br>
The same applies for the Agent.

Windows
--------

We provide MSI installers for Windows OS.

### Install & run the Server

The Windows version of HawkCD ships with a .msi and .exe. You will need to download both to install HawkCD. Run the HawckCD Server .exe (not the .msi) installer and follow the steps below:

* Once the welcome screen is shown click on the Next button.

![Alt text](/img/win/Server_1.png)

* Either choose the default installation directory or a new install path.

![Alt text](/img/win/Server_2.png)

* Enter the server host name, default value is `localhost`.

![Alt text](/img/win/Server_3.png)

<div class="admonition warning">
<p class="admonition-title">WARNING</p>
<p>
The server installer ships Redis for windows. It installs Redis as Windows service `hawkcdredis`.  By default, the Redis port is 6379. If the port is already in use the will let you choose another port.
</p>
</div>

![Alt text](/img/win/Server_6.png)

* The configuration is over, click Install.

![Alt text](/img/win/Server_4.png)

* Click Finish to complete the installation.

![Alt text](/img/win/Server_5.png)

### Install & Run the the Agent

Run the HawckCD Agent installer and follow the steps below:

* Once the welcome screen is shown click on the Next button.

![Alt text](/img/win/Agent_1.png)

* Either choose the default installation directory or a new install path.

![Alt text](/img/win/Agent_2.png)

* Enter the Server's host name and port number

![Alt text](/img/win/Agent_3.png)

<div class="admonition warning">
<p class="admonition-title">WARNING</p>
<p>
If not set properly the Agent will not be able to connect to the Server.
</p>
</div>

* The configuration is over, click Install.

![Alt text](/img/win/Agent_4.png)

* Click Finish to complete the installation.

![Alt text](/img/win/Server_5.png)

### Upgrade

* Running a newer version of the Server/Agent installer will skip the configuration steps and send you straight to the window with the Install button.
* After that click Finish and your product will be upgraded.
