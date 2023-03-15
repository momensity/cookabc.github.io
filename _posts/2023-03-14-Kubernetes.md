---
layout: post
title:  "Kubernetes Architecture"
date:   2023-03-14 20:45:12 +0800
categories: kubernetes
---

<div style="width: 100%; height: 650px; display: flex; justify-content: center; align-items: center;">
    <img alt="Kubernetes Architecture Diagram" style="max-width: 150%" src="/assets/k8s/architecture.png">
</div>

> Source: _[Kubernetes Architecture Explained](https://devopscube.com/kubernetes-architecture-explained/)_
>
> The following Kubernetes architecture diagram shows all the components of the Kubernetes cluster and how external systems connect to the Kubernetes cluster.

![Kubernetes Architecture](https://images.clickittech.com/2020/wp-content/uploads/2022/04/13202329/Diagram-55.jpg)

-  ### Control Plane Components
<br/>
    ***1. kube-apiserver***
    > The kube-api server is the central hub of the Kubernetes cluster that exposes the Kubernetes API. Kubernetes api-server is responsible for the following:
    > - `API management`: Exposes the cluster API endpoint and handles all API requests.
    > - `Authentication` (Using client certificates, bearer tokens, and HTTP Basic Authentication) and `Authorization` (ABAC and RBAC evaluation)
    > - Processing API requests and validating data for the API objects like pods, services, etc. (Validation and Mutation Admission controllers)
    > - It is the only component that communicates with etcd.
    > - api-server coordinates all the processes between the control plane and worker node components.
    
    ![kube-apiserver](/assets/k8s/apiserver.png)

    ***2. etcd***
    >  Etcd acts as both a backend service discovery and a database. You can call it the brain of the Kubernetes cluster.
    > - etcd stores all configurations, states, and metadata of Kubernetes objects (pods, secrets, daemonsets, deployments, configmaps, statefulsets, etc).
    > - etcd allows a client to subscribe to events using Watch() API . Kubernetes api-server uses the etcd’s watch functionality to track the change in the state of an object.
    > - etcd exposes key-value API using gRPC. Also, the gRPC gateway is a RESTful proxy that translates all the HTTP API calls into gRPC messages. It makes it an ideal database for Kubernetes.
    > - etcd stores all objects under the /registry directory key in key-value format. For example, information on a pod named Nginx in the default namespace can be found under /registry/pods/default/nginx
    
    ![etcd](/assets/k8s/etcd.png)
    
    ***3. kube-scheduler***
    > The kube-scheduler is responsible for scheduling pods on worker nodes. Its primary task is to identify the create request and choose the best node for a pod that satisfies the requirements.
    
    ![kube-scheduler](/assets/k8s/scheduler.png)
    
    ***4. Kube Controller Manager***
    > Kube controller manager is a component that manages all the Kubernetes controllers. Kubernetes resources/objects like pods, namespaces, jobs, replicaset are managed by respective controllers. Also, the kube scheduler is also a controller managed by Kube controller manager.
    > - It manages all the controllers and the controllers try to keep the cluster in the desired state.
    > - You can extend kubernetes with custom controllers associated with a custom resource definition.
    
    ![Kube Controller Manager](/assets/k8s/kube_controller_manager.png)
    
    ***5. Cloud Controller Manager (CCM)***
    > When kubernetes is deployed in cloud environments, the cloud controller manager acts as a bridge between Cloud Platform APIs and the Kubernetes cluster.
    > - Deploying Kubernetes Service of type Load balancer. Here Kubernetes provisions a Cloud-specific Loadbalancer and integrates with Kubernetes Service.
    > - Provisioning storage volumes (PV) for pods backed by cloud storage solutions.
    
    ![Cloud Controller Manager](/assets/k8s/cloud_controller_manager.png)

<br/>

- ### Worker Node Components
<br/>
    ***1. Kubelet***
    > The kubelet in Kubernetes is the main service on a node responsible for ensuring pods and their containers are healthy and running in the desired state, regularly taking in new or modified pod specifications from the kube-apiserver, and reporting to the master on the health of the host where it is running.
    >
    > To put it simply, kubelet is responsible for the following.
    > - Creating, modifying, and deleting containers for the pod.
    > - Responsible for handling liveliness, readiness, and startup probes.
    > - Responsible for Mounting volumes by reading pod configuration and creating respective directories on the host for the volume mount.
    > - Collecting and reporting Node and pod status via calls to the API server.
    
    ![Kubelet](/assets/k8s/kubelet.png)
    
    ***2. Kube proxy***
    > The Kubernetes kube-proxy is a proxy service running on each worker node, responsible for dealing with individual host subnetting and forwarding requests to the correct pods/containers across isolated networks in a cluster, as well as exposing services to the external world.
    
    ![Kube proxy](/assets/k8s/kube_proxy.png)
    
    ***3. Container Runtime***
    > The container runtime in Kubernetes runs on all nodes in the cluster and is responsible for managing the entire lifecycle of a container on a host, including pulling images from registries, allocating and isolating resources for containers, and running containers.
    
    ![Container Runtime](/assets/k8s/container_runtime.png)

<br/>

- ### Addon Components
<br/>
    ***1. CNI Plugin (Container Network Interface)***
    > The CNI plugin is a Kubernetes networking plugin that defines how containers should be networked together within a Kubernetes cluster, responsible for providing networking capabilities to pods by setting up the network interfaces, assigning IP addresses, and managing routing and network policies.
    
    ![CNI Plugin](/assets/k8s/CNI_plugin.png)
    
    ***2. CoreDNS (For DNS server)***:
    > CoreDNS is a DNS server within Kubernetes that enables DNS-based service discovery when enabled as an addon.
    
    ***3. Metrics Server (For Resource Metrics)***:
    > The Metrics Server in Kubernetes enables the collection of performance data and resource usage metrics for nodes and pods within the cluster.
    
    ***4. Web UI (Kubernetes Dashboard)***:
    > Enabling the Kubernetes Web UI provides access to the dashboard for managing objects through a web-based user interface.