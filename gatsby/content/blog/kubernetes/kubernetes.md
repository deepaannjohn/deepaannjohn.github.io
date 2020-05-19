---
title: Getting started with Kubernetes
date: "2020-05-18T12:12:03.284Z"
description: "Kubernetes Intro"
---

**Kuberenetes Basics**

Kubernetes is an open source container orchestration engine for **automating deployment, scaling, and management of containerized applications**. The open source project is hosted by the Cloud Native Computing Foundation (CNCF).

A Kubernetes cluster consists of two types of resources:

The **Master** coordinates the cluster

**Nodes** are the workers that run applications


The **Master** is responsible for managing the cluster. The master coordinates all activities in your cluster, such as 

_scheduling applications, 

maintaining applications' desired state, 

scaling applications, 

and rolling out new updates.
_

A **node** is a VM or a physical computer that serves as a worker machine in a Kubernetes cluster. 

_Each node has a Kubelet, which is an agent for managing the node and communicating with the Kubernetes master. 

The node should also have tools for handling container operations, such as Docker or rkt. _

When you deploy applications on Kubernetes, you tell the master to start the application containers. The master schedules the containers to run on the cluster's nodes. The nodes communicate with the master using the Kubernetes API, which the master exposes. End users can also use the Kubernetes API directly to interact with the cluster.

A Kubernetes cluster can be deployed on either physical or virtual machines.

Create a k8s cluster:

The following command starts a cluster with a single master node:

`minikube start`

Minikube will start a vm and run a k8s cluster now.

The command line interface is called kubectl.

To view the cluster details, run:

`kubectl custer-info`

To view the nodes, run:

`kubectl get nodes`


**Deploying on Kubernetes**

Once you have a running Kubernetes cluster, you can deploy your containerized applications on top of it.

To do so, you create a Kubernetes Deployment configuration. The Deployment instructs Kubernetes how to create and update instances of your application. Once you've created a Deployment, the Kubernetes master schedules the application instances included in that Deployment to run on individual Nodes in the cluster.

When you create a Deployment, you'll need to specify the container image for your application and the number of replicas that you want to run. You can change that information later by updating your Deployment.

A **Kubernetes Pod** is a group of one or more Containers, tied together for the purposes of administration and networking. A Pod is the basic execution unit of a Kubernetes application - the smallest and simplest unit in the Kubernetes object model that you create or deploy. A Pod encapsulates an application’s container (or, in some cases, multiple containers), storage resources, a unique network identity (IP address), as well as options that govern how the container(s) should run. A Pod represents a unit of deployment: a single instance of an application in Kubernetes, which might consist of either a single container or a small number of containers that are tightly coupled and that share resources. Each Pod is meant to run a single instance of a given application. If you want to scale your application horizontally (to provide more overall resources by running more instances), you should use multiple Pods, one for each instance. In Kubernetes, this is typically referred to as replication. Pods that are running inside Kubernetes are running on a private, isolated network. By default they are visible from other pods and services within the same kubernetes cluster, but not outside that network. Pods are the atomic unit on the Kubernetes platform. When we create a Deployment on Kubernetes, that Deployment creates Pods with containers inside them (as opposed to creating containers directly). Each Pod is tied to the Node where it is scheduled, and remains there until termination (according to restart policy) or deletion. In case of a Node failure, identical Pods are scheduled on other available Nodes in the cluster.

A Pod is a group of one or more application containers (such as Docker or rkt) and includes shared storage (volumes), IP address and information about how to run them.


```
Containers should only be scheduled together in a single Pod if they are tightly coupled and need to share resources such as disk.

```
A **Kubernetes Deployment** checks on the health of your Pod and restarts the Pod’s Container if it terminates. **Deployments** are the recommended way to manage the creation and scaling of Pods.

`creating a deployment:

kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4

Viewing a deployment:

kubectl get deployments

View the pod:

kubectl get pods`


By default, the Pod is only accessible by its internal IP address within the Kubernetes cluster. To make the Container accessible from outside the Kubernetes virtual network, you have to expose the Pod as a K**ubernetes Service.**

`kubectl expose deployment hello-node --type=LoadBalancer --port=8080`

The --type=LoadBalancer flag indicates that you want to expose your Service outside of the cluster.

`kubectl get services`

 The common format of a kubectl command is: **kubectl action resource**. 


**Nodes**

A Pod always runs on a Node. A Node is a worker machine in Kubernetes and may be either a virtual or a physical machine, depending on the cluster. Each Node is managed by the Master. A Node can have multiple pods, and the Kubernetes master automatically handles scheduling the pods across the Nodes in the cluster. The Master's automatic scheduling takes into account the available resources on each Node.

Every Kubernetes Node runs at least:

Kubelet, a process responsible for communication between the Kubernetes Master and the Node; it manages the Pods and the containers running on a machine.

A container runtime (like Docker, rkt) responsible for pulling the container image from a registry, unpacking the container, and running the application.

```

kubectl get - list resources
kubectl describe - show detailed information about a resource
kubectl logs - print the logs from a container in a pod
kubectl exec - execute a command on a container in a pod
```

Although each Pod has a unique IP address, those IPs are not exposed outside the cluster without a **Service**. Services allow your applications to receive traffic. Services can be exposed in different ways by specifying a type in the ServiceSpec:

ClusterIP (default) - Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster.

NodePort - Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using <NodeIP>:<NodePort>. Superset of ClusterIP.

LoadBalancer - Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service. Superset of NodePort.

ExternalName - Exposes the Service using an arbitrary name (specified by externalName in the spec) by returning a CNAME record with the name. No proxy is used. This type requires v1.7 or higher of kube-dns.

A Service routes traffic across a set of Pods. Services are the abstraction that allow pods to die and replicate in Kubernetes without impacting your application. Discovery and routing among dependent Pods is handled by Kubernetes Services.

Services match a set of Pods using labels and selectors, a grouping primitive that allows logical operation on objects in Kubernetes. 

To create a new service and expose it to external traffic we’ll use the expose command.

> kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080


The Deployment created automatically a label for our Pod. With describe deployment command we can see the name of the label:

kubectl describe deployment

Let’s use this label to query our list of Pods. We’ll use the kubectl get pods command with -l as a parameter, followed by the label values:

kubectl get pods -l app=kubernetes-bootcamp

You can do the same to list the existing services:

kubectl get services -l app=kubernetes-bootcamp

To apply a new label we use the label command followed by the object type, object name and the new label:

kubectl label pod $POD_NAME app=v1 --overwrite=true

**Scaling** is accomplished by changing the number of **replicas** in a Deployment. Scaling out a Deployment will ensure new Pods are created and scheduled to Nodes with available resources. Running multiple instances of an application will require a way to distribute the traffic to all of them. Services have an** integrated load-balancer **that will distribute network traffic to all Pods of an exposed Deployment. Services will monitor continuously the running Pods using endpoints, to ensure the traffic is sent only to available Pods.

To see the ReplicaSet created by the Deployment, run 
> kubectl get rs

> kubectl scale deployments/kubernetes-bootcamp --replicas=4

Use the same command to scale up and down.

**Rolling updates** allow Deployments' update to take place with zero downtime by incrementally updating Pods instances with new ones.

To update the image of the application to version 2, use the set image command, followed by the deployment name and the new image version:

> kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2

> kubectl rollout status deployments/kubernetes-bootcamp

Kubernetes provides you with:

**Service discovery and load balancing**

Kubernetes can expose a container using the DNS name or using their own IP address. If traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that the deployment is stable.

**Storage orchestration**

Kubernetes allows you to automatically mount a storage system of your choice, such as local storages, public cloud providers, and more.

Automated rollouts and rollbacks

You can describe the desired state for your deployed containers using Kubernetes, and it can change the actual state to the desired state at a controlled rate. For example, you can automate Kubernetes to create new containers for your deployment, remove existing containers and adopt all their resources to the new container.

Automatic bin packing

You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can fit containers onto your nodes to 
make the best use of your resources.

Self-healing

Kubernetes restarts containers that fail, replaces containers, kills containers that don’t respond to your user-defined health check, and doesn’t advertise them to clients until they are ready to serve.

Secret and configuration management

Kubernetes lets you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your container images, and without exposing secrets in your stack configuration.
