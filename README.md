<h1>Kubernetes Study</h1>

<p align="center">
<img width="400" height="200" src="https://user-images.githubusercontent.com/31994778/123542315-b7bef500-d751-11eb-8bf6-dd6c87a7fabb.png">
</p>

---


<b>Table Of Contents</b> |
------------ | 
[What is Kubernetes?](#what-is-kubernetes)
[Kubernetes Components](#k8s-components)
[Kubernetes Architecture](#k8s-architecture)
[Minikube and Kubectl](#k8s-minikube-kubectl)
[Main Kubectl Commands](#kubectl-commands)

 
 <p id="what-is-kubernetes">
 <h2>What is Kubernetes?</h2>
 </p>
 
 <p>
  <b><i>"k8s is an open source orchestration tool. In foundation, it manages containers."</b></i>
 </p>
 
 <p align="center">
  <img width="473" alt="Screen Shot 2021-06-27 at 2 30 27 PM" src="https://user-images.githubusercontent.com/31994778/123542831-56e4ec00-d754-11eb-8b3a-5923fbab33a3.png">
  </p>
  
  This means that K8s helps us manage applications that made up of 10s or 100s of containers.
  
  <h3>What Problems K8s Solve?</h3>
  
Amongst switching from `monolithic` architectures to `microservices`, applications might have 10s or 100s of containers running. Since the number of containers increase, it becomes much harder to manually manage them. K8s helps us in this area.


<p align="center">
  <img width="550" alt="Screen Shot 2021-06-27 at 2 34 07 PM" src="https://user-images.githubusercontent.com/31994778/123542965-15a10c00-d755-11eb-86bf-a2218c442c38.png">
  </p>
  
  <p align="center">
    <b>Microservices picturized</b>
  </p>
 
 <h3>What Features Does K8s Offer?</h3>
 
 - <b>High Availability</b>: When a containers fails, kubernetes restarts it automatically to eliminate downtime.
 - <b>Scalability</b>: Kubernetes helps containers to load faster which increases the response rate.
 - <b>Disaster Recovery</b>: When backend infrastructure fails, i.e., loses data, K8s helps it backup and restore the data.

---

<p id="k8s-components">
<h2>Kubernetes Components</h2>
</p>

<p align="center">
<img width="550" height="350" alt="Screen Shot 2021-06-27 at 2 46 44 PM" src="https://user-images.githubusercontent.com/31994778/123543324-901e5b80-d756-11eb-8827-308fd1445836.png">
  </p>

<b><i>"Kubernetes has tons of components, but for most of the time we are going to be working with just a handful of them."</b></i>

<h3>Node</h3>

Node in kubernetes is a server. It's either on a physical machine or a VM. `Nodes` contain `pods`. Each node in k8s has necessary resources to run pods.

---

<h3>Pod</h3>

A pod is the smallest unit in K8s. <b>Pod is an abstraction over container.</b>

What this means is that a pod creaters a running environment, another layer, over a, say Docker, container and it abstracts this container so that <b>you only have to deal with K8s, not the container.</b>

<p align="center">
  <img width="300" height="350" alt="Screen Shot 2021-06-27 at 3 10 28 PM" src="https://user-images.githubusercontent.com/31994778/123544050-1ee0a780-d75a-11eb-9f72-76ccc0d724ba.png">
  </p>

- A pod is meant to run one application(container) inside of it.
- In my website project, I have backend, frontend, db applications. With k8s, they all run inside pods. One pod for each, total => 3 pods.

<p align="center">
  <img width="300" height="350" alt="Screen Shot 2021-06-27 at 3 16 52 PM" src="https://user-images.githubusercontent.com/31994778/123544229-fdcc8680-d75a-11eb-925d-69fc78f8d912.png">
  </p>
  
  <b>Kubernetes has its own virtual network and pods talk to each other using the IP addresses assigned to them inside this virtual network.</b>
  
  <p align="center">
  <img width="300" height="350" alt="Screen Shot 2021-06-27 at 3 21 54 PM" src="https://user-images.githubusercontent.com/31994778/123544325-75021a80-d75b-11eb-8e91-83fced5101b4.png">
  </p>
  
  When a pod dies out, kubernetes replaces it with another one, however, the IP address of the dead port changes on restart. This is bad since other pod won't be aware of the new IP address. To solve it, we have another component called `Service`.
  
  ---

  <h3>Service</h3>
  
  <b><i>"Service is a component that provides a permanent IP address with pods. So each `pod` has its own `service`. However, Service and Pod are not the same thing so we should think Service as a gateway to a Pod that helps a Pod to obtain a permanent IP address. "</b></i>
  
  <p align="center">
  <img width="300" height="300" alt="Screen Shot 2021-06-27 at 3 31 28 PM" src="https://user-images.githubusercontent.com/31994778/123544611-c959ca00-d75c-11eb-98ec-d33c7ff0c659.png">
  </p>
  
  - Life cycle of a Pod and Service are not connected. This means `Service` will stay intact even if `Pod` dies out.
  - For our app to be accessed by a browser, we should have an `External Service` that will handle the communication between the browser and our app.
  - But of course, we don't want our applications, such as db, to be open to the outside sources, so we need to create something called `Internal Service`.
  - This, opening our app to outside world, is done with `Ingress`.
  - So, instead of `Service`, an external request first goes through `Ingress`, and `Ingress` does the forwarding to the `Service`.

<p align="center">
<img width="500" alt="Screen Shot 2021-06-27 at 3 41 18 PM" src="https://user-images.githubusercontent.com/31994778/123544888-22762d80-d75e-11eb-9c98-c59908f57142.png">
  </p>
  
<p align="center">
  <img width="350" height="300" alt="Screen Shot 2021-06-27 at 3 43 07 PM" src="https://user-images.githubusercontent.com/31994778/123544955-71bc5e00-d75e-11eb-95c7-922fd129880f.png">
  </p>
  
  ---
  
  <h3>ConfigMap</h3>
  
  <b><i>"ConfigMap is a k8s component in which we store endpoint information. It is an API object used to store non-confidential data in key-value pairs."</b></i>
  
Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume.

<h4>Motivation</h4>

<b><i>"Use a ConfigMap for setting configuration data separately from application code."</b></i>

When we are working on an application, we make connections of different components with endpoints. For example 
```python
class DB(metaclass=Singleton):
    _database = None
    local = "mongodb://admin:password@localhost:27017"  # Use for development
    dockerUrl = "mongodb://admin:password@mongodb"  # Use for production
    MONGO_DETAILS = dockerUrl
```

Here, we write down `dockerUrl` and `localUrl` so that our backend application can connect to the DB application.

Imagine, that we already created container images and suddenly, we want to change the endpoint with which backend connects to DB. Without configmap, it is done in the following order:

1- Change the hardcoded endpoint in the IDE.
2- Recreate the Image.
3- Push the Image to Hub.
4- Run `docker-compose -f app.yml up`

As you can see, this is very tedious for something as simple as changing the endpoint.

Thanks to `ConfigMap`, we can easily solve this problem without having to do these steps.

<b>We just put/adjust the endpoint inside the `ConfigMap`, and our pod will know what to do with it.</b>

<p align="center">
  <img width="550" alt="Screen Shot 2021-06-27 at 4 37 12 PM" src="https://user-images.githubusercontent.com/31994778/123546616-01b1d600-d766-11eb-9371-47898c6ce93b.png">
  </p>

In other words, <b>A ConfigMap allows you to decouple environment-specific configuration from your container images, so that your applications are easily portable.</b>

---

<h3>Secret</h3>

<b><i>"A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. You can put everything that cannot be put inside a ConfigMap for security reasons inside a Secret."</b></i>

<b>Both ConfigMap and Secret is connected to a pod.</b>

<p align="center">
<img width="550" alt="Screen Shot 2021-06-27 at 4 40 02 PM" src="https://user-images.githubusercontent.com/31994778/123546691-5d7c5f00-d766-11eb-93ca-9f6d720dc0d0.png">
  </p>
  
  ---

  <h3>Volumes</h3>
  
  By default, Kubernetes pods are like Docker containers, in that they do not preserve data when they are restarted. Volumes in k8s, help us out in this field.
  
  <b><i>"K8s volumes attaches a physical hard-drive to a pod that's meant to be stateful. The storage can either be on local machine or a remote storage."</b></i>
  
  <p align="center">
  <img width="550" alt="Screen Shot 2021-06-27 at 4 46 18 PM" src="https://user-images.githubusercontent.com/31994778/123546944-6883bf00-d767-11eb-8a87-008cb7915283.png">
  </p>
  
  ---
  
  <h3>Deployment & Stateful Sets</h3>
  
  <b><i>"Deployment and Stateful Sets are pod blueprints with replicating mechanisms."</b></i>
  
  Let's say that everything is working fine. But what happens when one of our pods dies?
  
  <p align="center">
  <img width="550" alt="Screen Shot 2021-06-27 at 5 03 14 PM" src="https://user-images.githubusercontent.com/31994778/123547467-94a03f80-d769-11eb-9064-3f301d2f680e.png">

</p>
  
  In this case, the user will experience downtime until the pod is replaced, and this is not a good user experience.
  
  To fix this problem, we need to `replicate` our pods so that when the main pod is down, a replicated pod can be used instead of it.
  
  - The replica is also connected to a `service`.
  - Here, service will have another functionality on top of providing static IP address, which is `LoadBalancer`.
  - So, when one pod dies, the service will take the request and direct it to another alive pod.
  - In order to create a replica of the pod, you need to define a blueprint, in other words `Deployment`, and specify how many replicas of that pod to be run.
  - In practice, you'll not be working with pods, you'll be creating `Deployments`.
  - Before we said that `Pod` is an abstraction on top of container.
  - `Deployment` is an abstraction on top of `Pods`.

<p align="center">
  <img width="550" alt="Screen Shot 2021-06-27 at 5 10 22 PM" src="https://user-images.githubusercontent.com/31994778/123547730-98809180-d76a-11eb-84fe-928bfb853742.png">

  </p>
  
  But, what about the Database?
  
  Unfortunately, using `Deployment` is only for `Stateless` applications. In other words, if your application hold data(state), then it's better to use `Stateful Sets`. The reason why we can't use `Deployment` is because the replicas of the database would likely to have problem of data inconsistency, since they all share a common data resource.
  
  <p align="center">
  <img width="550" alt="Screen Shot 2021-06-27 at 5 18 51 PM" src="https://user-images.githubusercontent.com/31994778/123548022-ca462800-d76b-11eb-9846-ede56e600b56.png">

  </p>
  
<b>Here, we need to make sure that database read/write is synchronized through all replicas to avoid data inconsistency.</b>

<b>Note that deploying db applications using stateful set is not easy. So it's common practice to host db applications outside k8s cluster and just have the stateless applications that replicate and scale with no problem inside of the k8s cluster.</b>
  
  <p align="center">
  <img width="250" height="350" alt="Screen Shot 2021-06-27 at 4 55 47 PM" src="https://user-images.githubusercontent.com/31994778/123547205-8aca0c80-d768-11eb-973e-dde70a0e14b7.png">
</p>

---

<p id="k8s-architecture">
<h2>Kubernetes Architecture</h2>
</p>

<h3>Node Processes</h3>

Nodes are one of the most important components in K8s architecture since they run the pods inside them. This is one of the reasons why they are called `Worker Nodes`.

<p align="center">
 <img width="550" alt="networking-overview" src="https://user-images.githubusercontent.com/31994778/123550396-e77ff400-d775-11eb-898d-70112a170aaa.png"> 
 </p>

<p align="center">
 Nodes are cluster servers that do the actual work
 </p>
 
 Every node must have at least 3 processes:
 
 - Container Runtime: Because pods have containers running inside, `Container Runtime` should be installed on every node.
 - Kubelet: Interacts with both the container and the node. Kubelet starts a pod with a container inside. Also does resource assignments.(CPU, RAM, Storage)
 - Kube Proxy: Forwards requests in an intelligent manner to avoid network overhead.

<p align="center">
 <img width="350" height="300" alt="Screen Shot 2021-06-27 at 6 37 33 PM" src="https://user-images.githubusercontent.com/31994778/123550567-cec40e00-d776-11eb-82db-8743e1a11325.png">
 </p>
 
 <p align="center">
 Kube Proxy forwarding a request to the most logical pod to prevent network overhead
 </p>
 
 ---

<h3>Master Processes</h3>

<b><i>Master Nodes are responsible from controlling the state of worker nodes, and have completely different processes running inside.</b></i>

<p align="center">
 <img width="550" alt="Screen Shot 2021-06-27 at 6 37 33 PM" src="https://user-images.githubusercontent.com/31994778/123550733-a852a280-d777-11eb-869b-515e5a2e9757.png">
</p>

1- <b>API Server</b>: Acts as a gateway between k8s cluster and control plane. k8s operator(this is you) observes the state of cluster and pods on frontend side with the information provided by the API Server. In other words, it's the gateway for the cluster. This means that when you want to schedule new pods, deploy new applications, create new service or any other components, you need to talk to the API server on the master node and the API Server will validate your request and if everything's fine it'll forward your request to other processes.

<p align="center">
 <img width="264" alt="Screen Shot 2021-06-27 at 6 59 13 PM" src="https://user-images.githubusercontent.com/31994778/123551288-d0430580-d779-11eb-93eb-9aa806e5e5d9.png">
 </p>

---

2- <b>Scheduler</b>: Responsible for `intelligently` selecting a node to start a requested pod. It selects the node by looking at many things, such as how much resource available on the node and etc.

<p align="center">
 <img width="286" alt="Screen Shot 2021-06-27 at 7 02 10 PM" src="https://user-images.githubusercontent.com/31994778/123551398-34fe6000-d77a-11eb-8a12-9c4aa0fda8a6.png">
 </p>
 
 <b>Scheduler only decides which node that the pod is scheduled to. Kubelet is the one who starts the pod.</b>
 
 ---
 
 3- <b>Controller Manager</b>: If happens, detects the dead pods on any node and reschedules those nodes as soon as possible.
 
 In other words, watches for state changes in the cluster, i.e., pods crashing and etc., and tries to recover the state. To do that it talks to the scheduler and scheduler does its thing.
 
 
<p align="center">
<img width="272" alt="Screen Shot 2021-06-27 at 7 08 49 PM" src="https://user-images.githubusercontent.com/31994778/123551581-22d0f180-d77b-11eb-8f5f-7b3b7b98d959.png">
 </p>
 
 ---
 
 4- <b>etcd</b>: etcd is the key-value store that stores the data regarding the cluster's state. We can think of it as the `Cluster brain`. Every change inside the cluster is persisted into this store and all of the processes we mentioned above works because of the data inside etcd. 
 
 <b>Application data is not stored in etcd.</b>
 
 <b>There could be multiple master nodes</b>
 
 <p align="center">
<img width="500" alt="Screen Shot 2021-06-27 at 7 18 27 PM" src="https://user-images.githubusercontent.com/31994778/123551920-8f002500-d77c-11eb-81a5-0a2921d15add.png">
 </p>

<p id="k8s-minikube-kubectl">
 <h2>Minikube and Kubectl</h2>
 </p>
 
 <b><i>"Minikube is a 1 Node k8s cluster to be used for testing purposes."</b></i>
 
 <p align="center">
 <img width="450" height="377.08" alt="Screen Shot 2021-06-27 at 7 25 28 PM" src="https://user-images.githubusercontent.com/31994778/123552169-92e07700-d77d-11eb-89d8-3def555ceef3.png">
 <img width="450" alt="Screen Shot 2021-06-27 at 7 25 41 PM" src="https://user-images.githubusercontent.com/31994778/123552164-8bb96900-d77d-11eb-81da-b6e51cdcfd32.png">
 </p>
 
 ---
 
 <h3>Kubectl</h3>
 
 <b><i>"Kubectl is a command line tool for K8s cluster."</b></i>
 
  <p align="center">
 <img width="600" alt="Screen Shot 2021-06-27 at 7 30 55 PM" src="https://user-images.githubusercontent.com/31994778/123552306-3af64000-d77e-11eb-89fb-af89065d055c.png">
 </p>
 
 ---
 
 <h3>Creating and Starting a Cluster</h3>
 
 <h4>Start a Cluster</h4>
 
 `minikube start --vm-driver=hyperkit`
 
 Here, hyperkit is a hypervisor, other alternatives can be used as well.
 
 <p align="center">
 <img width="550" alt="Screen Shot 2021-06-27 at 9 42 11 PM" src="https://user-images.githubusercontent.com/31994778/123555788-8e718980-d790-11eb-9732-46a57bcb9757.png">
 </p>
 
 
<b>Even if you don't have Docker on your machine, this is still going to work because minikube has `Docker Runtime` pre-installed.</b>
 
 ---
 
 <h4>Get Status of Nodes</h4>
 
`kubectl get nodes`

`minikube status`

<p align="center">
 <img width="500" alt="Screen Shot 2021-06-27 at 9 43 12 PM" src="https://user-images.githubusercontent.com/31994778/123555812-b5c85680-d790-11eb-8a85-2e06ad85f4c4.png">
 </p>
 
 ---
 
 <h4>Version</h4>
 
 <p align="center">
 <img width="500" alt="Screen Shot 2021-06-27 at 9 45 19 PM" src="https://user-images.githubusercontent.com/31994778/123555862-00e26980-d791-11eb-9519-2ef646e5c7ec.png">
 </p>
 
 ---
 
 <p id="kubectl-commands">
 <h3>Main Kubectl Commands</h3>
 <p>
 
 <p align="center">
  <img width="550" alt="Screen Shot 2021-06-27 at 9 47 38 PM" src="https://user-images.githubusercontent.com/31994778/123555927-5585e480-d791-11eb-9db2-773040f28fbe.png">
  </p>
  
  
  <h4>Get Pods</h4>
  
  `kubectl get pod`
  
<p align="center">
<img width="550" alt="Screen Shot 2021-06-27 at 9 56 32 PM" src="https://user-images.githubusercontent.com/31994778/123556217-96cac400-d792-11eb-94ed-6b1583a119e4.png">
 </p>
  
  ---
  
  <h4>Get Services</h4>
  
  `kubectl get services`
  
  <p align="center">
 <img width="550" alt="Screen Shot 2021-06-27 at 9 51 05 PM" src="https://user-images.githubusercontent.com/31994778/123556039-cb8a4b80-d791-11eb-8ebc-c95584dc9bc9.png">
 </p>
  
  ---
  
  <h4>Create Components</h4>
  
  `kubectl create ..`
  
  `kubectl create deployment nginx-depl --image=nginx`
  
  creates deployment of nginx using its image on DockerHub.
  
   <p align="center">
<img width="550" alt="Screen Shot 2021-06-27 at 9 55 15 PM" src="https://user-images.githubusercontent.com/31994778/123556180-6551f880-d792-11eb-9cbd-8ad6d6a08854.png">
 </p>
 
- <b>replicaset manages the replicas of a pods.</b>
- <b>Deployment is a blueprint for creating pods.</b>


<p align="center">
<img width="550" alt="Screen Shot 2021-06-27 at 10 01 52 PM" src="https://user-images.githubusercontent.com/31994778/123556332-5750a780-d793-11eb-92f6-8bd8e2a5b439.png">
 </p>
 
 ---
 
 <h4>Changing Deployment with kubectl edit</h4>
 
 `kubectl edit deployment <deployment-name>`
 
 <p align="center">
 <img width="500" alt="Screen Shot 2021-06-27 at 10 07 22 PM" src="https://user-images.githubusercontent.com/31994778/123556467-14db9a80-d794-11eb-9545-63551de30b88.png">
 </p>
 
 Navigates us to configuration file, and we can change it however we want. For example, I want to change the image version of the pod, so I make it nginx:1.16. The only thing I have to do is change it and save it. The rest is K8s magic.
 
 <p align="center">
 <img width="550" alt="Screen Shot 2021-06-27 at 10 09 30 PM" src="https://user-images.githubusercontent.com/31994778/123556549-77cd3180-d794-11eb-8452-d48ca0d398cd.png">
 </p>
 
 As you can see here, K8s is automatically creating another pod for the nginx:1.16 container. Very convenient and fast.
 
 Also, note that a new replicaset is created.
 
 <p align="center">
 <img width="550" alt="Screen Shot 2021-06-27 at 10 14 45 PM" src="https://user-images.githubusercontent.com/31994778/123556662-21acbe00-d795-11eb-886d-4a340b1bb596.png">
<p>
