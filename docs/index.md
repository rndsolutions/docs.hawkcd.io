
HawkCD
=======
Lightweight ``Continuous Delivery`` Server

``HawkCD`` is automation solution for building ``Continuous Delivery`` pipelines.
The project development is inspired by the ``Continuous Delivery`` principles and practices. ``HawkCD`` allows automating the software release process - from code check-in through build and automated testing to production deployments.

The product is in ``Alpha`` phase

Installation
------------

``HawkCD`` is being tested on Linux & Windows operating systems

#### Linux
For Linux like system ``HawkCD`` is distributed as a self containing package, it ships with embedded Redis database.

Install & run the Server

```sh
wget http://www.hawkcd.io/downloads/latest/hawkcd.tar.gz
tar zxvf hawkcd.tar.gz
cd Server
./hawkcd.sh
```

Install & Run the the agent

```sh
wget http://www.hawkcd.io/downloads/latest/hawkcd-agent.tar.gz
tar zxvf hawkcd-agent.tar.gz
cd Agent
./agent.sh
```


#### Windows

Soon to come :)

Getting Started
----------------

* [Continuous Delivery & Server concepts](/concepts)
* [Setup your first pipeline](configuration/#set-up-a-pipeline)
* [Pipeline Configuration](/configuration/#pipeline-configuration)
* [Running a pipeline](/configuration/#running-a-pipeline)
