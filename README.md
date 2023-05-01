# platys-trial-workspace
Gitpod for experimenting with Platys - a tool for generating and provisioning Modern Data Platforms based on Docker and Docker Compose from Trivadis. Its main use is for small-scale Data Lab projects, Proof-of-Concepts (PoC) or Proof-of-value (PoV) projects as well as trainings.

The user of platys can choose which services to use from a list of supported services and generate a fully working docker-compose.yml file including all necessary configuration files.


[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/lucasjellema/platys-trial-workspace)


## Installation of platys

This installation is performed when the Gitpod workspace is first started (based on [Installing Platys](https://github.com/TrivadisPF/platys/blob/master/documentation/install.md)). See the actual commands in file `.gitpod.yml`

The steps are:
* download tar ball to /tmp
* extract and move to /usr/local/bin
* make root the owner
* remove the archive
* verify installation with `platys version`

Note: only when you see the output from this last command - somethink like:
```
Platys - Trivadis Platform in a Box - v 2.4.3
https://github.com/trivadispf/platys
Copyright (c) 2018-2020, Trivadis AG 
```

is the environment ready for the next steps
## First steps with platys

These first steps are taken from [Getting started with Platys](https://github.com/TrivadisPF/platys-modern-data-platform/blob/master/documentation/getting-started.md).

To create and run a simple first platform, let's first create a target directory.

```
mkdir platys-demo-platform
cd platys-demo-platform
```

Now let's use platys to initialise the new and now current directory to use the Modern Data Analytics Platform Stack.

We specify the platform stack name trivadis/platys-modern-data-platform to use as well as the stack version 1.15.0 (the current version of this platform stack at the time of writing).  With the -n option we give the platform a meaningful name. It will be used as the name of the docker network, so it is important that it is unique, if you use multiple platforms on the same machine.

```
platys init -n demo-platform --stack trivadis/platys-modern-data-platform --stack-version 1.15.0 --structure flat
```

Running this command takes several dozens of seconds. It generates a config.yml file, if it does not exist already, with all the services which can be configured for the platform. This config file is now to be configured - to specify the services to be included in the *demo-platform*. For example: enable Apache Kafka, MySQL, SuperSet, RabbitMQ. And set additional configuration settings - see [the documentation of the configuration settings](https://github.com/TrivadisPF/platys-modern-data-platform/blob/master/documentation/configuration.md)


A very simple first platform composition could include services PostgreSQL (open source database) and nocodb ( a no-code database platform that allows teams to collaborate and build applications with ease of a familiar and intuitive spreadsheet interface) . This requires us to enable two services. With this next command (or simply from the file navigator in VS Code) open the `config.yml` file.

```
gp open config.yml
``` 

Locate the line with `POSTGRESQL_enable: false`. Change *false* to *true* in these two lines:

``` 
      # ===== PostgreSQL ========
      #
      POSTGRESQL_enable: false
      POSTGRESQL_volume_map_data: false
``` 

Also locate the lines 
``` 
      NOCODB_enable: false
      NOCODB_volume_map_data: false
``` 
and change *false* to *true*. By default, *nocodb* is configured to work with the default PostgreSQL database (demodb) and user (demo) that will be created by platsys in the docker-compose,.yml file.   

To generate the docker-compose.yml based on your updated version of the config.yml file:

```
platys gen -c ${PWD}/config.yml
```

The file docker-compose.yml is generated in the current directory. By running this file with Docker Compose, all configured services - if they fit in your machine - are pulled and started and configured.

Before running Docker Compose, first export 
* DOCKER_HOST_IP - the IP address of the network interface of the Docker Host
* PUBLIC_IP - the IP address of the public network interface of the Docker Host (different to DOCKER_HOST_IP if in a public cloud environment

```
export DOCKER_HOST_IP=127.0.0.1
```
(I am not entirely sure about the PUBLIC_IP)

To now run the Docker Compose:

```
docker-compose up -d
```

The images for the selected services will be pulled (downloaded and subsequently the services are started. At port 80 you will find an overview of the installed services and their relevant port details.

Check the docker-compose.yml file to see which port has been mapped to *nocodb*. 

In my case:
``` 
    ports:
      - 28276:8080
``` 

On port 28276 I can open the `nocodb` web ui. After creating an account, I can enter the tool - that is connected to the PostgreSQL Database. I can define new tables, enter data and create a view on that data. I can even share that view (a web page) with anyone on the public internet.

One way to look inside the PostgreSQL database is with the CLI *pgcli*. You can install it with:

```
pip install -U pgcli
```

and the run and connect to the *demo* user in the *demodb* with:

```
pgcli 'postgres://demo:abc123!@localhost:5432/demodb'
```

# Cookbook with Trino and Kafka

see [original cookbook by Guido Schmutz](https://github.com/TrivadisPF/platys-modern-data-platform/blob/master/cookbooks/recipes/querying-kafka-with-trino/README.md)

# Resources

[Introduction to Platys](https://github.com/TrivadisPF/platys)
[Getting started with Platys](https://github.com/TrivadisPF/platys-modern-data-platform/blob/master/documentation/getting-started.md)
[Platys Modern Data Platform - Overview of supported Services ](https://github.com/TrivadisPF/platys-modern-data-platform)
[Installing Platys](https://github.com/TrivadisPF/platys/blob/master/documentation/install.md)
[Cookbooks for trying out aspects of the modern data platform](https://github.com/TrivadisPF/platys-modern-data-platform/tree/master/cookbooks)