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
[Configuration File Syntax](#conf-file-syntax)
[Configuration File Demo](#conf-file-demo)
[K8s Namespaces](#k8s-namespaces)

 
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

 ---
 
 <h3>Debugging Pods</h3>
 
 1- Get the pod name via `kubectl get pod`
 
 2- run `kubectl logs [pod name]`
 
 3- Voila !
 
 <p align="center"> 
 <img width="550" alt="Screen Shot 2021-06-28 at 9 01 17 AM" src="https://user-images.githubusercontent.com/31994778/123587827-86950200-d7ef-11eb-98ed-03e81a6636e5.png">
</p>

4- If you want to see more information about the pod, run `kubectl describe pod [pod name]`

<p align="center">
 <img width="550" alt="Screen Shot 2021-06-28 at 9 04 22 AM" src="https://user-images.githubusercontent.com/31994778/123588052-dd024080-d7ef-11eb-86e8-a809369d3572.png">
 </p>
 
 This also will give us the lifecycle information of the pod.
 
 ```bash
 Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  9h     default-scheduler  Successfully assigned default/mongo-depl-5fd6b7d4b4-hnkg9 to minikube
  Normal  Pulling    5m39s  kubelet            Pulling image "mongo"
  Normal  Pulled     3m10s  kubelet            Successfully pulled image "mongo" in 2m28.975403524s
  Normal  Created    3m8s   kubelet            Created container mongo
  Normal  Started    3m8s   kubelet            Started container mongo
```

5- We can also get inside the pod terminal via `kubectl exec -it [pod name] -- bin/bash`

<p align="center">
<img width="550" alt="Screen Shot 2021-06-28 at 9 13 44 AM" src="https://user-images.githubusercontent.com/31994778/123588938-2ef79600-d7f1-11eb-8eb9-68aea47da80c.png">
 </p>

with this, we can get inside the pod container and do some testing and debugging, very useful.

---

<h3>Delete Deployment</h3>

`kubectl delete deployment [name]`

1- run `kubectl get deployment` to check out deployments

2- run `kubectl delete deployment [name]` to delete

Results in:
 
 ```bash
 ???  front ??? master ??? kubectl get deployment
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
mongo-depl   1/1     1            1           10h
nginx-depl   1/1     1            1           11h
???  front ??? master ??? kubectl delete deployment mongo-depl
deployment.apps "mongo-depl" deleted
???  front ??? master ??? kubectl get pod
NAME                          READY   STATUS        RESTARTS   AGE
mongo-depl-5fd6b7d4b4-hnkg9   0/1     Terminating   0          10h
nginx-depl-7fc44fc5d4-wvv6h   1/1     Running       0          11h
???  front ??? master ??? 

 ```
 
 <b>CRUD operations is done at `Deployment` level, and everything underneath follows automatically.</b>
 
 ---
  
<h3>Creating a Configuration File</h3>

As we have seen before, creating a deployment manually is a bit laborious. It has the following form:
```
kubectl create deployment mongo-depl --image=mongo option1 option2 option3
```

Imagine doing this manually everytime. Would be very tedious, especially if you're working with multiple pods.

Instead, we can create a config-file and use it to create deployments with `kubectl apply -f [filename]`

Let's create a configuration file:

1- `touch nginx-deployment.yaml`

2- 
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.6
          ports:
            - containerPort: 80
```

here,

- under metadata, `name: nginx-deployment` is the name of the deployment.
- under spec, `replicas: 1` is how many pod replicas we want to create.
- template and spec are the blueprint of the pods.
- kind: Deployment specifies what we want to create with this .yaml file.

<b>This means that "I want a pod with a single replica. This pod will run a container with name: nginx, image: nginx: 1.6 and bind that to port 80.</b>

<p align="center">
 <img width="500" alt="Screen Shot 2021-06-28 at 9 50 02 AM" src="https://user-images.githubusercontent.com/31994778/123592938-95cb7e00-d7f6-11eb-9c41-e31dee946cd9.png">
 </p>
 
(Later on, I changed nginx: 1.6 to nginx: latest due to some error...)

Let's apply the configuration..

`kubectl apply -f nginx-deployment.yaml`

Results in:

```bash
???  front ??? master ??? kubectl apply -f nginx-deployment.yaml
deployment.apps/nginx-deployment created

???  front ??? master ??? kubectl get pod
NAME                                READY   STATUS    RESTARTS   AGE
mongo-depl-5fd6b7d4b4-gpxpt         1/1     Running   0          134m
nginx-deployment-75b69bd684-mm6lw   1/1     Running   0          2m2s

???  front ??? master ??? kubectl describe pod nginx-deployment-75b69bd684-mm6lw
Name:         nginx-deployment-75b69bd684-mm6lw
Namespace:    default
Priority:     0
Node:         minikube/192.168.64.2
Start Time:   Mon, 28 Jun 2021 11:43:03 +0300
Labels:       app=nginx
              pod-template-hash=75b69bd684
Annotations:  <none>
Status:       Running
IP:           172.17.0.5
IPs:
  IP:           172.17.0.5
Controlled By:  ReplicaSet/nginx-deployment-75b69bd684
Containers:
  nginx:
    Container ID:   docker://97fc037029179e168b2a478f438b767c0d4ab37cc1fc5dd20e5c02c64eaa4b45
    Image:          nginx:latest
    Image ID:       docker-pullable://nginx@sha256:47ae43cdfc7064d28800bc42e79a429540c7c80168e8c8952778c0d5af1c09db
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Mon, 28 Jun 2021 11:43:04 +0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-gkcb9 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-gkcb9:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-gkcb9
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  42s   default-scheduler  Successfully assigned default/nginx-deployment-75b69bd684-mm6lw to minikube
  Normal  Pulled     41s   kubelet            Container image "nginx:latest" already present on machine
  Normal  Created    41s   kubelet            Created container nginx
  Normal  Started    41s   kubelet            Started container nginx
```

If you want to update, say replica to 2, then all you have to do is 

`vim nginx-deployment.yaml` and change the necessary field.

<p align="center">
 <img width="357" alt="Screen Shot 2021-06-28 at 11 49 51 AM" src="https://user-images.githubusercontent.com/31994778/123608056-18f4d000-d807-11eb-9fd2-8b5a61fdf76d.png">
 </p>

and then `kubectl apply nginx-deployment.yaml` again,

```bash
???  front ??? master ??? vim nginx-deployment.yaml 
???  front ??? master ??? kubectl apply -f nginx-deployment.yaml                
deployment.apps/nginx-deployment configured
???  front ??? master ??? kubectl get pod
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-75b69bd684-dlwfq   1/1     Running   0          12s
nginx-deployment-75b69bd684-mm6lw   1/1     Running   0          8m26s
```

As you can see, we have two replicas now!

<b>You can use kubectl commands with any other component, such as `services`, `volumes`, just like we did it with `deployment`.</b>

<p id="conf-file-syntax">
<h2>Configuration File Syntax</h2>
</p>

<b><i>"Main tool for creating and configuring components in the Kubernetes cluster."</b></i>

<h3>3 Parts of K8s Configuration File</h3>

Every configuration file in K8s has 3 parts.

1- <b>Metadata</b>

2- <b>Specification</b>

3- <b>Status</b>

---

<h3>Format</h3>

Format of K8s configuration file is .yaml, which is:

- Very strict about indentation.
- Easy to read.
- You can use online Yaml validators, especially if the configuration file is pretty large.
- Store the config file with your code. 

<h3>Connecting Services to Deployments</h3>

<b><i>"Services act as a gateway to pods, hence, we need to configure deployments and services accordingly."</b></i>

Configuration file for deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 8080
```

Configuration file for service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

Here, `containerPort` should be the same as `targetPort` of the service. This is so because a service needs to forward request to the pod, which runs the container inside. If the service doesn't know the container port, then it can't forward requests to the pod.

Also, `port: 80` of the service means that other pods, such as a database pod will talk to the service through port 80.

<p align="center">
 <img width="351" height="426" alt="Screen Shot 2021-06-28 at 3 37 18 PM" src="https://user-images.githubusercontent.com/31994778/123637730-04c0cb00-d827-11eb-9d7b-c0b81badd744.png">
 <img width="320" height="426" alt="Screen Shot 2021-06-28 at 3 37 36 PM" src="https://user-images.githubusercontent.com/31994778/123637718-fecaea00-d826-11eb-96f7-b95b95db5baf.png">
 </p>

<p align="center">
<img width="800" alt="Screen Shot 2021-06-28 at 3 45 19 PM" src="https://user-images.githubusercontent.com/31994778/123639050-7ea58400-d828-11eb-9fa7-c736ba2ab61d.png">
</p>

---

<h3>Deleting Configuration Files</h3>

If you delete a configuration file through kubectl, everything underneath is gone.

```bash
???  Azure ??? master ??? kubectl delete -f nginx-deployment.yaml 
deployment.apps "nginx-deployment" deleted
???  Azure ??? master ??? kubectl delete -f nginx-service.yaml 
service "nginx-service" deleted
???  Azure ??? master ??? kubectl get deployment
No resources found in default namespace.
???  Azure ??? master ??? kubectl get pod
No resources found in default namespace.
???  Azure ??? master ??? kubectl get service
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   18h
```

---

<p id="conf-file-demo">
<h2>Configuration File Demo</h2>
</p>

We'll build a K8s cluster with mongodb and mongo-express deployments.

1- We need to prepare both mongodb and mongo-express configuration files.

Starting with mongodb:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb-deployment
  labels:
    app: mongodb
spec:
  replicas: 2
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
        - name: mongodb
          image: mongo:latest
          ports:
            - containerPort: 27017
          env:
            - name: MONGO_INITDB_ROOT_USERNAME
              valueFrom:
                secretKeyRef:
                  name: 
                  key: 
            - name: MONGO_INITDB_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: 
                  key: 
```

Here, as you may have noticed, we did not fill `name` and `key` fields yet. We need a `Secret` file first. Remember that `Secret` file contains sensitive data, such as username and password.

2 - Create `mongodb-secret.yaml` file

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mongodb-secret
type: Opaque
data:
  username: YWRtaW4=
  password: cGFzc3dvcmQ=
```

You may wonder how we got username and password as `YWRtaW4=` and `cGFzc3dvcmQ=`..

I used the following command to encode username and password values to Base64.

```
???  Azure ??? master ??? echo -n 'admin' | base64
YWRtaW4=

???  Azure ??? master ??? echo -n 'password' | base64
cGFzc3dvcmQ=
```

3- run `kubectl apply -f mongodb-secret.yaml` in order to create secret component.

<b>Secret should be created prior to deployment file! Otherwise deployment file won't see the secret file and cannot retrieve username, password data.</b>

4- edit your `mongodb-deployment.yaml` file `env` part as follows

```yaml
env:
  - name: MONGO_INITDB_ROOT_USERNAME
    valueFrom:
      secretKeyRef:
        name: mongodb-secret
        key: username
  - name: MONGO_INITDB_ROOT_PASSWORD
    valueFrom:
      secretKeyRef:
        name: mongodb-secret
        key: password
```

5- run `kubectl apply -f mongodb-deployment.yaml`

6- Test!

```yaml
???  Azure ??? master ??? kubectl get deployment
NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
mongodb-deployment   2/2     2            2           6s

???  Azure ??? master ??? kubectl get all       
NAME                                      READY   STATUS    RESTARTS   AGE
pod/mongodb-deployment-6c4cc48696-7fgjz   1/1     Running   0          24s
pod/mongodb-deployment-6c4cc48696-8j547   1/1     Running   0          24s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   22h

NAME                                 READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongodb-deployment   2/2     2            2           24s

NAME                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/mongodb-deployment-6c4cc48696   2         2         2       24s

???  Azure ??? master ??? kubectl get secret    
NAME                  TYPE                                  DATA   AGE
default-token-gkcb9   kubernetes.io/service-account-token   3      22h
mongodb-secret        Opaque                                2      14m

```

As you can see, everything is working fine and dandy.

Now, let's create an internal service so that other pods can talk to mongodb pod.

<b>In yaml, you can put multiple documents in a single file with `---`</b>

We can put Service and Deployment file in the same configuration file because they belong together.

<p align="center">
 <img width="550" alt="Screen Shot 2021-06-28 at 9 17 30 PM" src="https://user-images.githubusercontent.com/31994778/123684657-68aeb800-d856-11eb-824a-fdb30d9bb721.png">
 </p>


7- Create a service configuration

```yaml
???  Azure ??? master ??? kubectl apply -f mongodb-deployment.yaml
deployment.apps/mongodb-deployment unchanged
service/mongodb-service created

???  Azure ??? master ??? kubectl describe service mongodb-service
Name:              mongodb-service
Namespace:         default
Labels:            <none>
Annotations:       <none>
Selector:          app=mongodb
Type:              ClusterIP
IP:                10.105.106.70
Port:              <unset>  8081/TCP
TargetPort:        27017/TCP
Endpoints:         172.17.0.3:27017,172.17.0.4:27017
Session Affinity:  None
Events:            <none>
```

8- Do the same thing for mongo-express application

I added both files [mongo-express-depl.yaml](https://github.com/burakhanaksoy/Kubernetes-Study/blob/main/mongo-express-depl.yaml) and [mongodb-deployment.yaml](https://github.com/burakhanaksoy/Kubernetes-Study/blob/main/mongodb-deployment.yaml) to the repo...

Here, `mongo-express-depl.yaml` has 

```yaml
type: LoadBalancer
  ports:
    - protocol: TCP
      port: 8081
      targetPort: 8081
      nodePort: 30000
```

<b>type is set to `LoadBalancer` since it is `Ingress`, which acts as a gateway to external requests. And `nodePort` acts as the port which we use to connect mongo-express by using browser, in short, it's the external port.</b>

9- Create a configmap so that mongo-express deployment can retrieve database_url.

mongo-configmap.yaml

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mongodb-configmap
data:
  database_url: mongodb-service
```

10- firstly, apply the configmap by `kubectl apply -f mongo-configmap.yaml`.

Then, create both service and deployment components by `kubectl apply -f mongo-express-depl.yaml`.

```bash
???  Azure ??? master ??? kubectl apply -f mongo-configmap.yaml
configmap/mongodb-configmap created
???  Azure ??? master ??? kubectl apply -f mongo-express-depl.yaml
deployment.apps/mongo-express created
```

<p align="center">
<img width="550" alt="Screen Shot 2021-06-29 at 9 21 48 AM" src="https://user-images.githubusercontent.com/31994778/123747447-a5f96100-d8bb-11eb-9e3e-35997db8bc0e.png">
 </p>

 We ran `minikube service mongo-express-service` to get an external IP address for mongo-express-service to connect to.
 
<p align="center">
 <img width="600" alt="Screen Shot 2021-06-28 at 10 47 23 PM" src="https://user-images.githubusercontent.com/31994778/123695123-d9f46800-d862-11eb-917e-fd6f4f4f8654.png">
 </p>
 
 Schematic is as follows:
 
 <p align="center">
 <img width="350" height="400" alt="Screen Shot 2021-06-28 at 10 56 31 PM" src="https://user-images.githubusercontent.com/31994778/123696276-3c019d00-d864-11eb-9b9b-28756a199a2b.png">
 </p>
 
 <b>Again, it's important that we apply configmap.yaml before we apply deployment.yaml</b>
 
 ---
 
 <p id="k8s-namespaces">
 <h2>K8s Namespaces</h2>
</p>
 
 <b><i>"We can think of a namespace as a virtual cluster inside a cluster."</b></i>
 
 using a namespace is not necessary for small, mid scale projects since in this kind of projects, resources inside the cluster can be managed easily, without complexities.
 
 However, as the projects get larger and more complex, we might have a situation like the following:
 
 <p align="center">
 <img width="550" alt="Screen Shot 2021-06-29 at 9 41 21 AM" src="https://user-images.githubusercontent.com/31994778/123749503-2de06a80-d8be-11eb-8532-28a75402d7c4.png">
 </p>
 
 <p align="center">
 A complex project figure
 </p>
 
In this case, we better use namespaces. Image running `kubectl get all` with a namespace having least 15 components, multiple configmaps and etc. That would be a mess!

 With using namespaces, we can group related components in their respective namespaces as follows:
 
 <p align="center">
<img width="550" alt="Screen Shot 2021-06-29 at 9 45 31 AM" src="https://user-images.githubusercontent.com/31994778/123750046-c971db00-d8be-11eb-9aae-3b3d8e94f0d8.png">
 </p>

<b>Another use case of namespaces is multiple teams using the same cluster.</b>

<p align="center">
 <img width="600" alt="Screen Shot 2021-06-29 at 9 47 29 AM" src="https://user-images.githubusercontent.com/31994778/123750504-54eb6c00-d8bf-11eb-83f2-42c57228bb19.png">
 </p>
 
  <p align="center">
 Teams having a conflict when using a single cluster.
 </p>
 
 Solution

<p align="center">
 <img width="600" alt="Screen Shot 2021-06-29 at 9 47 52 AM" src="https://user-images.githubusercontent.com/31994778/123750383-308f8f80-d8bf-11eb-9e13-1789447e456d.png">
 </p>

<b>Another use case of namespaces is resource sharing in staging and development.</b>

When we have staging and development deployments inside the same cluster, it's better to use them in different namespaces. In this way, they can share the resources inside the cluster, and we do not need to deploy the resources twice for staging and development deployments.

<p align="center">
 <img width="600" alt="Screen Shot 2021-06-29 at 9 57 26 AM" src="https://user-images.githubusercontent.com/31994778/123751481-76992300-d8c0-11eb-8db4-74e5d131c704.png">
 </p>
 
 <b>Another use case of namespaces is limiting resource access for different teams.</b>
 
 <p align="center">
<img width="600" alt="Screen Shot 2021-06-29 at 10 18 30 AM" src="https://user-images.githubusercontent.com/31994778/123754276-620a5a00-d8c3-11eb-8217-db66c87115d4.png">

 </p>
  
 <p align="center">
restricting namespaces for different teams
 </p>
 
 In this way, we eliminate the possibility of one team interfering accidentally with another team's namespace and resources.
 
 Also, in this way, we can limit CPU, RAM, Storage usage for each team per namespace.
 
  <p align="center">
<img width="600" alt="Screen Shot 2021-06-29 at 10 21 11 AM" src="https://user-images.githubusercontent.com/31994778/123754614-c3cac400-d8c3-11eb-95ff-ef4a606f738c.png">
 </p>
  
 <p align="center">
portioning physical resources for different teams
 </p>
 
 <h3>Namespace Usecase Summary</h3>
 
 - Structuring components
 - Avoid conflicts between teams
 - Sharing services between different environments
 - Access and resource limits on namespaces level

<b>Each namespace should define its own ConfigMap, even if it has the same reference. This is so because one namespace cannot use another namespace's ConfigMap.</b>

<b>Each namespace should define its own Secret, even if it has the same reference. This is so because one namespace cannot use another namespace's Secret.</b>

<b>However, namespaces can share service.</b>

  <p align="center">
<img width="671" alt="Screen Shot 2021-06-29 at 10 28 36 AM" src="https://user-images.githubusercontent.com/31994778/123755658-cf6aba80-d8c4-11eb-9a93-409ead2c89a4.png">
 </p>
  
 <p align="center">
 Different namespaces can share Service component
 </p>
 
 <b>Some components cannot be created in a namespace, they must be global in the cluster. Examples of such components are `volume` and `node`.</b>
 
 ```bash
 ???  Azure ??? master ??? kubectl api-resources --namespaced=false
NAME                              SHORTNAMES   APIGROUP                       NAMESPACED   KIND
componentstatuses                 cs                                          false        ComponentStatus
namespaces                        ns                                          false        Namespace
nodes                             no                                          false        Node
persistentvolumes                 pv                                          false        PersistentVolume
mutatingwebhookconfigurations                  admissionregistration.k8s.io   false        MutatingWebhookConfiguration
validatingwebhookconfigurations                admissionregistration.k8s.io   false        ValidatingWebhookConfiguration
customresourcedefinitions         crd,crds     apiextensions.k8s.io           false        CustomResourceDefinition
apiservices                                    apiregistration.k8s.io         false        APIService
tokenreviews                                   authentication.k8s.io          false        TokenReview
selfsubjectaccessreviews                       authorization.k8s.io           false        SelfSubjectAccessReview
selfsubjectrulesreviews                        authorization.k8s.io           false        SelfSubjectRulesReview
subjectaccessreviews                           authorization.k8s.io           false        SubjectAccessReview
certificatesigningrequests        csr          certificates.k8s.io            false        CertificateSigningRequest
flowschemas                                    flowcontrol.apiserver.k8s.io   false        FlowSchema
prioritylevelconfigurations                    flowcontrol.apiserver.k8s.io   false        PriorityLevelConfiguration
ingressclasses                                 networking.k8s.io              false        IngressClass
runtimeclasses                                 node.k8s.io                    false        RuntimeClass
podsecuritypolicies               psp          policy                         false        PodSecurityPolicy
clusterrolebindings                            rbac.authorization.k8s.io      false        ClusterRoleBinding
clusterroles                                   rbac.authorization.k8s.io      false        ClusterRole
priorityclasses                   pc           scheduling.k8s.io              false        PriorityClass
csidrivers                                     storage.k8s.io                 false        CSIDriver
csinodes                                       storage.k8s.io                 false        CSINode
storageclasses                    sc           storage.k8s.io                 false        StorageClass
volumeattachments                              storage.k8s.io                 false        VolumeAttachment
```

```bash
???  Azure ??? master ??? kubectl api-resources --namespaced=true 
NAME                        SHORTNAMES   APIGROUP                    NAMESPACED   KIND
bindings                                                             true         Binding
configmaps                  cm                                       true         ConfigMap
endpoints                   ep                                       true         Endpoints
events                      ev                                       true         Event
limitranges                 limits                                   true         LimitRange
persistentvolumeclaims      pvc                                      true         PersistentVolumeClaim
pods                        po                                       true         Pod
podtemplates                                                         true         PodTemplate
replicationcontrollers      rc                                       true         ReplicationController
resourcequotas              quota                                    true         ResourceQuota
secrets                                                              true         Secret
serviceaccounts             sa                                       true         ServiceAccount
services                    svc                                      true         Service
controllerrevisions                      apps                        true         ControllerRevision
daemonsets                  ds           apps                        true         DaemonSet
deployments                 deploy       apps                        true         Deployment
replicasets                 rs           apps                        true         ReplicaSet
statefulsets                sts          apps                        true         StatefulSet
localsubjectaccessreviews                authorization.k8s.io        true         LocalSubjectAccessReview
horizontalpodautoscalers    hpa          autoscaling                 true         HorizontalPodAutoscaler
cronjobs                    cj           batch                       true         CronJob
jobs                                     batch                       true         Job
leases                                   coordination.k8s.io         true         Lease
endpointslices                           discovery.k8s.io            true         EndpointSlice
events                      ev           events.k8s.io               true         Event
ingresses                   ing          extensions                  true         Ingress
ingresses                   ing          networking.k8s.io           true         Ingress
networkpolicies             netpol       networking.k8s.io           true         NetworkPolicy
poddisruptionbudgets        pdb          policy                      true         PodDisruptionBudget
rolebindings                             rbac.authorization.k8s.io   true         RoleBinding
roles                                    rbac.authorization.k8s.io   true         Role
```
