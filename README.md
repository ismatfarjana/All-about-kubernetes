# What is kubernetes

## Ans:

Kubernete is a tool for running diferent containes togather.

Kubernete automaticaly:

- creates the containers
- manages the containers
- creates communications between the containers

# how to set up kubernetes?

## ans:

### Docker for Mac/windows

set prefference kubenetes enable in prefferenace
![](https://i.imgur.com/HBsdFSO.png)
![](https://i.imgur.com/jMsXNJP.png)

# checking

| Column 1                   | Column 2                | Column 3                                    |
| -------------------------- | ----------------------- | ------------------------------------------- |
| in the new terminal window | **_`kubectl version`_** | will show client version and server version |

---

# Walk through kubernetes

**what do we need to start**

- service sourcecode
- service Dockerfile
- ready to go images
- create containers from the images
- deploy the containers on kubernetes cluster

![](https://i.imgur.com/UAiGP1A.png)

- kubernetes cluster will contain different <span style="color:blue">nodes</span> (nodes are virtual machines)
- its actually the computer that will run different containers for us
- for example: i am using my own computer, so bu default I am running one node, one virtual machine

### To create a container out of an image

- config file - holds directions for kubernetes
  - set up networking accessibility - make some copies of the service, make these copies accessible from network

| Column 1                           | Column 2  | Column 3                                                                                  |                                                                                     |
| ---------------------------------- | --------- | ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| give the config file to kubernetes | `kubeclt` | interact with kubernetes cluster                                                          |
|                                    |           | kubernetes cluster reads the config                                                       |
|                                    |           | kubernetes try to find a copy of these image in the Docker daemon of my personal computer |
|                                    |           | if not availabe localy, it looks in Docker hub                                            |
|                                    |           | after finding                                                                             | kubernetes creates required amount of containers                                    |
|                                    |           |                                                                                           | randomly distributes the containers among the <span style="color:blue">nodes</span> |
|                                    |           |                                                                                           | each container will be hosted inside a <span style="color:green">pod</span>         |

**_<span style="color:green">pod</span>_**: it wraps up container and can have multiple containers in it

| config file steps                                                 | to do so                                                             | what we get                                                                                                                                |
| ----------------------------------------------------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| to manage the <span style="color:green">pod</span>s               | kubernete will create a <span style="color:purple">deployment</span> | <span style="color:purple">deployment</span> will manage the <span style="color:green">pod</span>s                                         |
|                                                                   |                                                                      | <span style="color:purple">deployment</span> recreates the <span style="color:green">pod</span> if anything happens                        |
| access networking form other <span style="color:green">pod</span> | kubernete creats <span style="color:orange">**_Service_**</span>     | <span style="color:orange">**_Service_**</span> gives access to running <span style="color:green">pod</span> inside the kubernetes cluster |

<span style="color:orange">**_Service_**</span> removes the difficulties of what ip or on what port which service is running on

<span style="color:orange">**_Service_**</span> does the work for **event-bus**(previously, event-bus had to reach out to all different copies of the comment services figuer out it exists and also in which port to reach out)

So when <span style="color:orange">**_Service_**</span> is created to kuberbetes, it takes the request and forwords it to appropriate service

So , when the **event-bus** service is created, rather then going to different services(which is very confusing) it will reach out to <span style="color:orange">**_Service_**</span> and <span style="color:orange">**_Service_**</span> will forward the request to acqurate <span style="color:green">pod</span> / container
