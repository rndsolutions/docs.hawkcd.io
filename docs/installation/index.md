Installation
===============

<div class="admonition note">
<p class="admonition-title">Prerequisites</p>
<p>
  Java 1.8
</p>
</div>


Linux
------
For Linux-like operating systems `HawkCD` is distributed as a self containing package. It ships with embedded Redis database.

###Install & Run the Server

```sh
wget http://www.hawkcd.io/downloads/latest/hawkcd.tar.gz
tar zxvf hawkcd.tar.gz
cd Server
./hawkcd.sh
```

###Install & Run the the Agent

```sh
wget http://www.hawkcd.io/downloads/latest/hawkcd-agent.tar.gz
tar zxvf hawkcd-agent.tar.gz
cd Agent
./agent.sh
```

Windows
--------

We provide msi installers for windows OS

###Install & run the Server

Download the latest installers (``link``)

* Run the HawckCD Server installer.

![Alt text](/img/win/Server_1.png)

* Once the welcome screen is shown click on the Next button.

![Alt text](/img/win/Server_2.png)

* Either choose the default installation directory or a new install path.

![Alt text](/img/win/Server_3.png)

* Enter the server host name, default value is ``localhost``

![Alt text](/img/win/Server_4.png)


<div class="admonition warning">
<p class="admonition-title">WARNING</p>
<p>
The server installer ships Redis for windows. It installs Redis as windows service ``hawkcdredis``.  By default, the redis port is 6379. If the port is already in use the will let you choose another port.
</p>
</div>


###Install & Run the the agent
