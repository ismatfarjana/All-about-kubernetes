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

# Terminology

| Column 1               | Column 2                                                                         |
| ---------------------- | -------------------------------------------------------------------------------- |
| **Kubernetes cluster** | a collection of nodes and a master to manage the nodes                           | Text |
| **Node**               | a virtual machine that runs the containers                                       |  |
| **Pod**                | a hosted running container/containers                                            |  |
| **Deployment**         | monitors the pod, if crashes , restarts them                                     |  |
| **Service**            | a akubernetes service gives a easy to remember URL to access a running container |  |

# Config Files

- we create config files to create and configure objects(deployments, pods, services)
- written in YAML syntax
- These files are the documentations about what the kubernetes clusters are doing, so we will store the config files with our sourcecode(projects), this will be commited to git to store them in source control. It will let other engineers know about different diployment, services and pods i have created
- DO NOT CREATE A RESOURCE OR OBJECT DIRECTLY AT THE TERMINAL FOR PRODUCTION ENVIRONMNET

### commands:

| Column 1                                       | Column 2                                   | Column 3                                                                                                                                                                                               |
| ---------------------------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| to create an object with kubernetes            | **`kubectl apply -f posts.yaml`**          | tells kubernetes to process the config                                                                                                                                                                 |
| to see all the pods running inside the cluster | **`kubectl get pods`**                     | information about all the running pods![](https://i.imgur.com/YSMLHhp.png)                                                                                                                             |
| to run a command inside a container            | **`kubectl exec -it [pod_name] -- [cmd]`** | (startup a shell inside a container that is bein graninside the pod, add `sh` at the end ).execute a given command in a running pod ![](https://i.imgur.com/bT3hUCX.png) write `exit` to end the shell |
| to print all the logs tied to a container      | **`kubectl logs [pod_name]`**              | prints logs from given pod ![](https://i.imgur.com/YhCom2d.png)                                                                                                                                        |
| to get the information about running pod       | **`kubectl describe pod [pod_name]`**      | it will help debugging, for that look at the events log ![](https://i.imgur.com/Z1GyL7h.png)                                                                                                           |
| to delete a pod                                | **`kubectl delete pod [pod_name]`**        |                                                                                                                                                                                                        |
|                                                |                                            |                                                                                                                                                                                                        |

# Deployment

Pods are created with Deployment. Deployment is a kubernetes object that manage a set of pods

## commands:

| Column 1                                                          | Column 2                                      | Column 3                                                                      |
| ----------------------------------------------------------------- | --------------------------------------------- | ----------------------------------------------------------------------------- |
| Creating Deployment                                               | **`kubectl apply -f posts-depl.yaml`**        | ![](https://i.imgur.com/jQpftyt.png)                                          |
| list of running deployment                                        | **`kubectl get deployments`**                 | information about all pods in deployment ![](https://i.imgur.com/XqvNPum.png) |
| deleting pod created by kubernetes : kubernees creats another one |                                               | ![](https://i.imgur.com/koPQyZE.png)                                          |
| print details about deployment for debugging                      | **`kubectl describe deployment [depl-name]`** |                                                                               |
| delete a deployment                                               | **`kubectl delete deployment [depl-name]`**   | deletes deplyment with all associated pods.not recreated anymore              |

---

# Update Image Used by Deployment:

| Column 1     | Column 2                                                                                              | Advantage/ disadvantage                                                                                                                                                          |
| ------------ | ----------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Method 1** | \* change to project code                                                                             | everytime any new version created we need to come back and change the configuration in kubernetes deployment file and overtime this becomes hard cause this file will be so long |
|              | _ rebuild the image , specifying a new image verison_ ![](https://i.imgur.com/jo8Ak4T.png)            |
|              | \* in deployment config file , update the image version                                               |
|              | \* run the command **`kubectl apply -f [depl file name]`** whe kubernetes deployment is build         |
|              |                                                                                                       |                                                                                                                                                                                  |
| **Method 2** | * the deployment must use *latest\* tage in the pod ![](https://i.imgur.com/8CqJIpw.png)              |                                                                                                                                                                                  |
|              | \* Make an update to code ![](https://i.imgur.com/7AqvmeF.png)                                        |                                                                                                                                                                                  |
|              | \* build image ![](https://i.imgur.com/fb0pRAi.png)                                                   |                                                                                                                                                                                  |
|              | push image to docker hub ![](https://i.imgur.com/VFLT1yd.png)                                         |                                                                                                                                                                                  |
|              | run command **`kubectl rollout restart deployment [depl-name]`** ![](https://i.imgur.com/hKbvYhi.png) |                                                                                                                                                                                  |
|              |                                                                                                       |                                                                                                                                                                                  |

# the whole process of updating file, image building and deploying into docker hub

![](https://i.imgur.com/lV1oOEe.png)

![](https://i.imgur.com/VEOvtKW.png)

---

# Networking with Services

### Types of services

| Column 1      | Column 2                                                          | Column 3                                              |
| ------------- | ----------------------------------------------------------------- | ----------------------------------------------------- |
| Cluster IP    | setup communication between different pods inside clusters        | will use regularly                                    |
| Node Port     | this is used to access a pod from outside of cluster              | only for development purpuses                         |
| Load Balancer | makes pod accessible from outside the cluster, like outside world | nearly similer to node port, but action is different. |
| External Name | redirects an in-cluster req to a CNAME url.. dont know what it is |                                                       |

#### Difference between Node Port and Load Balancer

| Node Port                                                                   | Load Balancer                        |
| --------------------------------------------------------------------------- | ------------------------------------ |
| access a pod from outside of cluster: Development purpose, only for testing | access a pod from outside of cluster |

## Creating NodePort and accessing Node Port services

| Column 1              | Column 2                                                                       |
| --------------------- | ------------------------------------------------------------------------------ |
| create a service      | **`kubectl apply -f [service-name]`**                                          |  |
| get all services      | **`kubectl get services`**![](https://i.imgur.com/fv9SFNR.png)                 |
| details about service | **`kubectl describe services posts-srv`** ![](https://i.imgur.com/qwDxFHo.png) |

## Create ClusterIP services

| Column 1                              | Column 2                             |
| ------------------------------------- | ------------------------------------ |
| **`kubectl apply -f [service-name]`** | ![](https://i.imgur.com/eZB4ZvH.png) |
| **`kubectl get services`**            | ![](https://i.imgur.com/QINs87E.png) |
|                                       |                                      |

## communicate between services

write a url of http with the name of the clusterip service

| Column 1                                                 | Column 2 | Column 3 |
| -------------------------------------------------------- | -------- | -------- |
| change the localhost to <service name> in files index.js | Text     | Text     |
| update image by deployment                               |          |          |

##### test on postman

# Overall creating service and deployment

![](https://i.imgur.com/ZUS1fny.png)

# apply all

![](https://i.imgur.com/eIDoNqY.png)

![](https://i.imgur.com/TfxQlB7.png)

# ERROR HANDLING

| ERROR                                                    | WHY THIS ERROR                                   | SOLUTION                                                                   |
| -------------------------------------------------------- | ------------------------------------------------ | -------------------------------------------------------------------------- |
| Cannt push On Docker![](https://i.imgur.com/a2KjQMC.png) | didnot used my dockerid while creating the image | used my dockerid while creating image ![](https://i.imgur.com/6r1Z8wF.png) |

---

[Microservice project](https://github.com/Microservice-With-React-and-NodeJS)
