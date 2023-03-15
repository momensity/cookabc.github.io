---
layout: post
title:  "Kubernetes Architecture"
date:   2023-03-14 20:45:12 +0800
categories: kubernetes
---

![Kubernetes Architecture](https://images.clickittech.com/2020/wp-content/uploads/2022/04/13202329/Diagram-55.jpg)

> Source: _[Kubernetes Architecture Explained](https://devopscube.com/kubernetes-architecture-explained/)_
>
> The following Kubernetes architecture diagram shows all the components of the Kubernetes cluster and how external systems connect to the Kubernetes cluster.

![Kubernetes Architecture](/assets/images/k8s-architecture.png)

-  ### Control Plane Components
<br/>
    ***1. kube-apiserver***
    > Kubernetes API server is the central management entity that receives all REST requests for modifications (to pods, services, replication sets/controllers and others), serving as frontend to the cluster. Also, this is the only component that communicates with the etcd cluster, making sure data is stored in etcd and is in agreement with the service details of the deployed pods.
    ![](https://devopscube.com/wp-content/uploads/2022/10/kube-api-server.drawio-1.png.webp)

    ***2. etcd***
    >  a simple, distributed key value storage which is used to store the Kubernetes cluster data (such as number of pods, their state, namespace, etc), API objects and service discovery details. It is only accessible from the API server for security reasons. etcd enables notifications to the cluster about configuration changes with the help of watchers. Notifications are API requests on each etcd cluster node to trigger the update of information in the node’s storage.
    ![](https://devopscube.com/wp-content/uploads/2023/01/image-5-1160x1074.png)
    
    ***3. kube-scheduler***
    > helps schedule the pods (a co-located group of containers inside which our application processes are running) on the various nodes based on resource utilization. It reads the service’s operational requirements and schedules it on the best fit node. For example, if the application needs 1GB of memory and 2 CPU cores, then the pods for that application will be scheduled on a node with at least those resources. The scheduler runs each time there is a need to schedule pods. The scheduler must know the total resources available as well as resources allocated to existing workloads on each node.
    ![](https://devopscube.com/wp-content/uploads/2023/01/image-8.png)
    
    ***4. Kube Controller Manager***
    > runs a number of distinct controller processes in the background (for example, replication controller controls number of replicas in a pod, endpoints controller populates endpoint objects like services and pods, and others) to regulate the shared state of the cluster and perform routine tasks. When a change in a service configuration occurs (for example, replacing the image from which the pods are running, or changing parameters in the configuration yaml file), the controller spots the change and starts working towards the new desired state.
    ![](https://devopscube.com/wp-content/uploads/2023/01/image-9-1160x1723.png)
    
    ***5. Cloud Controller Manager (CCM)***
    > is responsible for managing controller processes with dependencies on the underlying cloud provider (if applicable). For example, when a controller needs to check if a node was terminated or set up routes, load balancers or volumes in the cloud infrastructure, all that is handled by the cloud-controller-manager.
    ![](https://devopscube.com/wp-content/uploads/2023/01/Cloud-Controller-Manager-1078x1536.png)

<br/>

- ### Worker Node Components
<br/>
    ***1. Kubelet***
    > the main service on a node, regularly taking in new or modified pod specifications (primarily through the kube-apiserver) and ensuring that pods and their containers are healthy and running in the desired state. This component also reports to the master on the health of the host where it is running.
    ![](https://devopscube.com/wp-content/uploads/2023/01/kubelet-architecture-1245x1536.png)
    
    ***2. Kube proxy***
    > a proxy service that runs on each worker node to deal with individual host subnetting and expose services to the external world. It performs request forwarding to the correct pods/containers across the various isolated networks in a cluster.
    ![](https://devopscube.com/wp-content/uploads/2023/01/image-14-1160x1587.png)
    
    ***3. Container Runtime***
    > Container runtime runs on all the nodes in the Kubernetes cluster. It is responsible for pulling images from container registries, running containers, allocating and isolating resources for containers, and managing the entire lifecycle of a container on a host.
    ![](https://devopscube.com/wp-content/uploads/2022/12/image-5.png)

<br/>

- ### Addon Components
<br/>
    ***1. CNI Plugin (Container Network Interface)***
    > CNI concerns itself only with network connectivity of containers and removing allocated resources when the container is deleted. 
    ![](https://devopscube.com/wp-content/uploads/2023/01/image-18-1160x1223.png)
    
    ***2. CoreDNS (For DNS server)***:
    > CoreDNS acts as a DNS server within the Kubernetes cluster. By enabling this addon, you can enable DNS-based service discovery.
    
    ***3. Metrics Server (For Resource Metrics)***:
    > This addon helps you collect performance data and resource usage of Nodes and pods in the cluster.
    
    ***4. Web UI (Kubernetes Dashboard)***:
    > This addon enables the Kubernetes dashboard for managing the object via web UI.