What is Kubernetes
  - Kubernetes = popular container orchestrator
  - Container Orchestration = Make many servers act like one.
  - Released in 2015 by google, maintained by large community.
  - Runs on top of Docker (usually) as a set of APIs in containers.
  - Provides API / CLI to manage containers across servers
  - Many clouds provide it for you.
  - Many vendors make a distribution of kubernetes.

Why Kubernetes
  - Orchestration : Next logical step in journey to faster DevOps.
  - Not every solution needs orchestration
  - Servers + Change Rate = Benefit of orchestration.
  - There are many vendor solutions to kubernetes.

Kubernetes vs Swarm
  - Kubernetes or Swarm
    - Kubernetes and swarm are both container orchestrators.
    - Both are sold platforms with vendor backing.
    - Swarm: Easier to deploy/manage.
    - Kubernetes : More features and flexibility.
  - Advantages of Swarm :
    - Comes with Docker, single vendor container platform.
    - Easiest orchestrator to deeploy / manage yourself.
    - Follows 80 / 20 rule, 20 % of features for 80% use cases.
    - Runs anywhere Docker does :
      - local, cloud, datacenter.
      - ARM, Windows 32-bit
    - Secure by default.
    - Easier to troubleshoot.

Advantages of Kubernetes
  - Clouds will deploy / manage Kubernetes for you.
  - Infrastructure vendors are making their own distributions.
  - Widest adoption and community.
  - Flexible : Covers widest set of use cases
  - "Kubernetes" first vendor support

Kubernetes Terminology
  - Basic Terms : System Parts.
    - Kubernetes: The whole orchestration system.
      - K8s "k-eights" or Kube for short.
    - Kubectl : CLI to configure Kubernetes and manage apps.
      - Using "cube control" official pronunciation.
    - Node : Single server in the Kubernetes cluster.
    - Kubelet : Kubernetes agent running on nodes.
    - Control Plane : Set of containers that manage the cluster.
      - Includes API server, scheduler, controller manager, etcd, and more
      - Sometimes called "master"

Kubernetes Container Abstractions
  - Pod : one or more containers running together on one Node.
    - Basic unit of deployment.
  - Controller : For creating/updating pods and other objects.
    - Many types of Controllers inc. Deployment, ReplicaSet, StatefulSet, DaemonSet, Job, CronJob, etc.
  - Service : network endpoints to connect to a pod.
  - Namespace : Filtered group of objects in cluster.
  - Secrets, ConfigMaps, and more.

Kubernetes Run, Create, and Apply
  - Kubernetes is evovling, and so is the CLI.
  - We get three ways to create pods from the kubectl CLI.
    - kubectl run (changing to be only for pod creation) in 1.18
    - kubectl create (create some resources via CLI or YAML)
    - kubectl apply (create/update anything via YAML)
  - For now we'll just use run or create CLI.
  - Later we'll learn YAML and pros/cons of each.

Creating Pods with kubectl  
  - kubectl version
  - Two ways to deploy pds (containers) : via commands or via YAML
  - kubectl run my-nginx --image nginx
  - kubectl get pods
  - kubectl get all
  - Pods -> ReplicaSet -> Deployment
  - kubectl delete deployment my-nginx
  - Starting in version 1.18 (released March 2020), the kubectl run command only does one thing: create single pods. There were many reasons for this, but the big ones were to reduce the complexity of how the run command worked and to move other resource creation to the kubectl create command. The idea is that kubectl run is now similar to docker run. It creates a single pod, where docker run creates a single container.
    - kubectl create deployment nginx --image nginx

Scaling ReplicaSets
  - Start a new deployment for one replica/pod
    - kubectl run my-apache --image httpd
  - Let scale up to two:
    - kubectl scale deploy/my-apache --replicas 2
    - kubectl scale deployment my-apache --replicas 2
    - The above are the same.
    - deploy = deployment = deployments

Inspecting Deployment Objects
  - Get container logs :
    - kubectl logs deploymeny/my-apache --follow --tail 1
  - Get a bunch of details about an object, including events!
    - kubectl logs -l run=my-apache
  - kubernetes can pull up to 5 pods at a time when it uses the selector.
  - Lookup the Stern tool for better log tailing.
  - Get a bunch of details about an object, including events!
    - kubectl describe pod/my-apache-xxxx-yyyy
    - kubectl describe pod/my-apache-54dc6c76f6-4qsv8
  - Watch a command (without needing watch)
    - kubectl get pods -w

Service Types
  - Exposing Containers
    - kubectl expose creates a service for existing pods.
    - A service is a stable address for pod(s)
    - If we want to connect pod(s), we need a service.
    - CoreDNS allows us to resolve services b name.
    - There are different types of services
      - ClusterIP
      - NodePort
      - LoadBalancer
      - ExternalName
  - Basic Service Types
    - ClusterIP (default)
      - Single, internal virtual IP allocated
      - Only reachable from within cluster (nodes and pods)
      - Pods can reach service on apps port number.
      - Good in the cluster
    - NodePort
      - High port allocated on each node.
      - Anyone can connect (if they can reach node)
    - LoadBalancer
      - Mostly used in cloud.
      - Controls a LB endpoint external to the cluster.
      - Only available when infra provider gives you a LB (AWS ELB etc)
    - ExternalName
      - Adds CNAME DNS record to CoreDNS only
      - Can be used when you are doing migrations.
      - Not used for Pods, but for giving pods a DNS name to use something outside Kubernetes.
    - Kubernetes Ingress
      - Designed for http traffic.

Creating a ClusterIP Service
  - kubectl get pods -w
  - kubectl create deployment httpenv --image=bretfisher/httpenv
  - Scale it to 5 replicas
    - kubectl scale deployment/httpenv --replicass=5
  - Let's create a ClusterIP service (default)
    - kubectl expose deployment/httpenv --port 8888
  - Inspecting ClusterIP Service
    - Look up what IP was allocated
      - kubectl get service
    - Remember this IP is cluster internal only, how do we curl it?
    - If you are on DockerDesktop (Host OS is not container OS)
      - k run --generator=run-prod/v1 temp-shell --rm -it --image bretfisher/netshoot -- bash
      - curl httpenv:8888
    - If on Linux Host :
      - curl [ip of service]:8888

Creating a NodePort Service
  - Expose a NodePort so we can access it via the host IP (including localhost on Windows/Linux/macOS)
    - kubectl expose deployment/httpenv --port 8888 --name httpenv-np --type NodePort
  - NodePort service also creates a ClusterIP!
  - These three service types are additive, each one creates the one above it :
    - ClusterIP
    - NodePort
    - LoadBalancer
  - curl localhost:NodePort
  - On minikube :
    - curl minikubeIp:NodePort

Add a LoadBalancer Service
  - If you're on DockerDesktop
    - kubectl expose deployment/httpenv --port 8888 --name httpenv-lb --type LoadBalancer
    - curl localhost:8888
  - If you're on kubeadm, minikube or microk8s
    - No built in LB
    - You can till run the command, but it will just stay at "pending"

Cleanup
  - k delete service/httpenv service/httpenv-np
  - k delete service/httpenv-lb deployment/httpenv

Kubernetes Services DNS
  - Starting with 1.11, internal DNS is provided by CoreDNS
    - Like swarm, this is a DNS-Based Service Discovery
  - So far we've been using hostnames to access Services.
    - curl <hostname>
  - But this only works for services in the same namespace.
    - kubectl get namespaces
  - Services also have a FQDN
    - curl <hostname>.<namespace>.svc.cluster.local

---------------------------

External Notes

---------------------------

Kubernetes : https://en.wikipedia.org/wiki/Kubernetes
  - Kubernetes is loosely coupled and extensible to meet different workloads. This extensibility is provided in large part by the Kubernetes API, which is used by internal components as well as extensions and containers that run on Kubernetes.
  - The Kubernetes master is the main controlling unit of the cluster, managing its workload and directing communication across the system. The Kubernetes control plane consists of various components, each its own process, that can run both on a single master node or on multiple masters supporting high-availability clusters.
  - The various components of the Kubernetes control plane are as follows:
    - <b>etcd</b>: etcd[28] is a persistent, lightweight, distributed, key-value data store developed by CoreOS that reliably stores the configuration data of the cluster, representing the overall state of the cluster at any given point of time.
    - <b>API server</b> : The API server is a key component and serves the Kubernetes API using JSON over HTTP, which provides both the internal and external interface to Kubernetes. The API server processes and validates REST requests and updates state of the API objects in etcd, thereby allowing clients to configure workloads and containers across Worker nodes.
    - <b>Scheduler</b>: The scheduler is the pluggable component that selects which node an unscheduled pod (the basic entity managed by the scheduler) runs on, based on resource availability. The scheduler tracks resource use on each node to ensure that workload is not scheduled in excess of available resources. For this purpose, the scheduler must know the resource requirements, resource availability, and other user-provided constraints and policy directives such as quality-of-service, affinity/anti-affinity requirements, data locality, and so on. In essence, the scheduler's role is to match resource "supply" to workload "demand".
    - <b>Controller manager</b> : A controller is a reconciliation loop that drives actual cluster state toward the desired cluster state, communicating with the API server to create, update, and delete the resources it manages (pods, service endpoints, etc.). The controller manager is a process that manages a set of core Kubernetes controllers. One kind of controller is a Replication Controller, which handles replication and scaling by running a specified number of copies of a pod across the cluster. It also handles creating replacement pods if the underlying node fails.
  - Nodes
    - A Node, also known as a Worker or a Minion, is a machine where containers (workloads) are deployed. Every node in the cluster must run a container runtime such as Docker, as well as the below-mentioned components, for communication with the primary for network configuration of these containers.
      - Kubelet: Kubelet is responsible for the running state of each node, ensuring that all containers on the node are healthy. It takes care of starting, stopping, and maintaining application containers organized into pods as directed by the control plane. Kubelet monitors the state of a pod, and if not in the desired state, the pod re-deploys to the same node. Node status is relayed every few seconds via heartbeat messages to the primary. Once the primary detects a node failure, the Replication Controller observes this state change and launches pods on other healthy nodes.
      - Kube-proxy: The Kube-proxy is an implementation of a network proxy and a load balancer, and it supports the service abstraction along with other networking operation.  It is responsible for routing traffic to the appropriate container based on IP and port number of the incoming request.
      - Container runtime: A container resides inside a pod. The container is the lowest level of a micro-service, which holds the running application, libraries, and their dependencies. Containers can be exposed to the world through an external IP address. Kubernetes has supported Docker containers since its first version, and in July 2016 the rkt container engine was added.
  - Pods
    - The basic scheduling unit in Kubernetes is a pod. A pod is a grouping of containerized components. A pod consists of one or more containers that are guaranteed to be co-located on the same node.
  - ReplicaSets
    - A ReplicaSet’s purpose is to maintain a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability of a specified number of identical Pods.
  - Services
    - A Kubernetes service is a set of pods that work together, such as one tier of a multi-tier application. The set of pods that constitute a service are defined by a label selector.
    - Kubernetes provides two modes of service discovery, using environmental variables or using Kubernetes DNS.
    - Service discovery assigns a stable IP address and DNS name to the service, and load balances traffic in a round-robin manner to network connections of that IP address among the pods matching the selector (even as failures cause the pods to move from machine to machine).
  - Volumes
    - A Kubernetes Volume provides persistent storage that exists for the lifetime of the pod itself.
    - The same volume can be mounted at different points in the filesystem tree by different containers.
  - Namespaces
    - Kubernetes provides a partitioning of the resources it manages into non-overlapping sets called namespaces.
  - ConfigMaps and Secrets
    - Kubernetes provides two closely related mechanisms to deal with this need: "configmaps" and "secrets", both of which allow for configuration changes to be made without requiring an application build. The data from configmaps and secrets will be made available to every single instance of the application to which these objects have been bound via the deployment.
    - Once the pod that depends on the secret or configmap is deleted, the in-memory copy of all bound secrets and configmaps are deleted as well.
    - he data is accessible to the pod through one of two ways: a) as environment variables (which will be created by Kubernetes when the pod is started) or b) available on the container filesystem that is visible only from within the pod.
    - The biggest difference between a secret and a configmap is that the content of the data in a secret is base64 encoded.
  - StatefulSets
    - Stateful workloads are much harder, because the state needs to be preserved if a pod is restarted, and if the application is scaled up or down, then the state may need to be redistributed. Databases are an example of stateful workloads. When run in high-availability mode, many databases come with the notion of a primary instance and secondary instance(s).
  - DaemonSets
    - Normally, the locations where pods are run are determined by the algorithm implemented in the Kubernetes Scheduler. For some use cases, though, there could be a need to run a pod on every single node in the cluster. This is useful for use cases like log collection, ingress controllers, and storage services. The ability to do this kind of pod scheduling is implemented by the feature called DaemonSets.
  - Labels and selectors
    - Kubernetes enables clients (users or internal components) to attach keys called "labels" to any API object in the system, such as pods and nodes.
    - "label selectors" are queries against labels that resolve to matching objects.
    - simply changing the labels of the pods or changing the label selectors on the service can be used to control which pods get traffic and which don't, which can be used to support various deployment patterns like blue-green deployments or A-B testing.
  - Replication Controllers and Deployments
    - A ReplicaSet declares the number of instances of a pod that is needed, and a Replication Controller manages the system so that the number of healthy pods that are running matches the number of pods declared in the ReplicaSet
  - Add-ons
    - Add-ons operate just like any other application running within the cluster: they are implemented via pods and services, and are only different in that they implement features of the Kubernetes cluster. The pods may be managed by Deployments, ReplicationControllers, and so on.
  - Storage
    - Historically Kubernetes was suitable only for stateless services. However, many applications have a database, which requires persistence, which leads to the creation of persistent storage for Kubernetes. Implementing persistent storage for containers is one of the top challenges of Kubernetes administrators, DevOps and cloud engineers.
  - API
    - The design principles underlying Kubernetes allow one to programmatically create, configure, and manage Kubernetes clusters. This function is exposed via an API called the Cluster API.
    - The API has two pieces - the core API, and a provider implementation. The provider implementation consists of cloud-provider specific functions that let Kubernetes provide the cluster API in a fashion that is well-integrated with the cloud-provider's services and resources.
  - Resource sharing and communication
    - Pods enable data sharing and communication among their constituent containers.
    - Storage in Pods
      - A Pod can specify a set of shared storage volumes. All containers in the Pod can access the shared volumes, allowing those containers to share data. Volumes also allow persistent data in a Pod to survive in case one of the containers within needs to be restarted.
    - Pod networking
      - Each Pod is assigned a unique IP address for each address family. Every container in a Pod shares the network namespace, including the IP address and network ports.
      -  Inside a Pod (and only then), the containers that belong to the Pod can communicate with one another using localhost.
      - When containers in a Pod communicate with entities outside the Pod, they must coordinate how they use the shared network resources (such as ports).
      - The containers in a Pod can also communicate with each other using standard inter-process communications like SystemV semaphores or POSIX shared memory.
      - Containers in different Pods have distinct IP addresses and can not communicate by IPC without special configuration. Containers that want to interact with a container running in a different Pod can use IP networking to communicate.
  - Privileged mode for containers
    - Any container in a Pod can enable privileged mode, using the privileged flag on the security context of the container spec. This is useful for containers that want to use operating system administrative capabilities such as manipulating the network stack or accessing hardware devices.
    - Note: Your container runtime must support the concept of a privileged container for this setting to be relevant.
  - Static Pods
    - Static Pods are managed directly by the kubelet daemon on a specific node, without the API server observing them. Whereas most Pods are managed by the control plane for static Pods, the kubelet directly supervises each static Pod (and restarts it if it fails).
    - Static Pods are always bound to one Kubelet on a specific node. The main use for static Pods is to run a self-hosted control plane: in other words, using the kubelet to supervise the individual control plane components.
    - The kubelet automatically tries to create a mirror Pod on the Kubernetes API server for each static Pod. This means that the Pods running on a node are visible on the API server, but cannot be controlled from there.

Namespaces : https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
  - Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces.
  - When to Use Multiple Namespaces
    - Namespaces are intended for use in environments with many users spread across multiple teams, or projects. For clusters with a few to tens of users, you should not need to create or think about namespaces at all. Start using namespaces when you need the features they provide.
    - Namespaces provide a scope for names. Names of resources need to be unique within a namespace, but not across namespaces. Namespaces cannot be nested inside one another and each Kubernetes resource can only be in one namespace.
  - Working with Namespaces
    - Note: Avoid creating namespace with prefix kube-, since it is reserved for Kubernetes system namespaces.
    - You can list the current namespaces in a cluster using:

    ```bash
    kubectl get namespace

    NAME              STATUS   AGE
    default           Active   1d
    kube-node-lease   Active   1d
    kube-public       Active   1d
    kube-system       Active   1d
    ```
    - Kubernetes starts with four initial namespaces:
      - default The default namespace for objects with no other namespace
      - kube-system The namespace for objects created by the Kubernetes system
      - kube-public This namespace is created automatically and is readable by all users (including those not authenticated). This namespace is mostly reserved for cluster usage, in case that some resources should be visible and readable publicly throughout the whole cluster. The public aspect of this namespace is only a convention, not a requirement.
      - kube-node-lease This namespace for the lease objects associated with each node which improves the performance of the node heartbeats as the cluster scales.
    - Setting the namespace for a request :
      - To set the namespace for a current request, use the --namespace flag.

      ```bash
      kubectl run nginx --image=nginx --namespace=<insert-namespace-name-here>
      kubectl get pods --namespace=<insert-namespace-name-here>
      ```
    - Setting the namespace preference :
      - You can permanently save the namespace for all subsequent kubectl commands in that context.

      ```bash
      kubectl config set-context --current --namespace=<insert-namespace-name-here>
      # Validate it
      kubectl config view --minify | grep namespace:
      ```
  - Namespaces and DNS
    - When you create a Service, it creates a corresponding DNS entry. This entry is of the form <service-name>.<namespace-name>.svc.cluster.local, which means that if a container just uses <service-name>, it will resolve to the service which is local to a namespace. This is useful for using the same configuration across multiple namespaces such as Development, Staging and Production. If you want to reach across namespaces, you need to use the fully qualified domain name (FQDN).
  - Not All Objects are in a Namespace
    - Most Kubernetes resources (e.g. pods, services, replication controllers, and others) are in some namespaces. However namespace resources are not themselves in a namespace. And low-level resources, such as nodes and persistentVolumes, are not in any namespace.
    - To see which Kubernetes resources are and aren't in a namespace:

    ```bash
    # In a namespace
    kubectl api-resources --namespaced=true

    # Not in a namespace
    kubectl api-resources --namespaced=false
    ```

Kubernetes Components : https://kubernetes.io/docs/concepts/overview/components/#master-components
  - A Kubernetes cluster consists of a set of worker machines, called nodes, that run containerized applications. Every cluster has at least one worker node.
  - The worker node(s) host the Pods that are the components of the application workload. The control plane manages the worker nodes and the Pods in the cluster. In production environments, the control plane usually runs across multiple computers and a cluster usually runs multiple nodes, providing fault-tolerance and high availability.
  - Control Plane Components
    - The control plane's components make global decisions about the cluster, s well as detecting and responding to cluster events :
      - Ex. scheduling, starting new pods if failure occurs.
    - Control plane components can be run on any machine in the cluster. However, for simplicity, set up scripts typically start all control plane components on the same machine, and do not run user containers on this machine.
    - kube-apiserver
      - The API server is a component of the Kubernetes control plane that exposes the Kubernetes API. The API server is the front end for the Kubernetes control plane.
    - etcd
      - Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data. If your Kubernetes cluster uses etcd as its backing store, make sure you have a back up plan for those data.
    - kube-scheduler
      - Control plane component that watches for newly created Pods with no assigned node, and selects a node for them to run on.
      - Factors taken into account for scheduling decisions include: individual and collective resource requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, and deadlines.
    - kube-controller-manager
      - Control Plane component that runs controller processes.
      - Logically, each controller is a separate process, but to reduce complexity, they are all compiled into a single binary and run in a single process.
      - Some types of these controllers are:
        - Node controller: Responsible for noticing and responding when nodes go down.
        - Job controller: Watches for Job objects that represent one-off tasks, then creates Pods to run those tasks to completion.
        - Endpoints controller: Populates the Endpoints object (that is, joins Services & Pods).
        - Service Account & Token controllers: Create default accounts and API access tokens for new namespaces.
    - cloud-controller-manager
      - A Kubernetes control plane component that embeds cloud-specific control logic.
      - The cloud controller manager lets you link your cluster into your cloud provider's API, and separates out the components that interact with that cloud platform from components that just interact with your cluster.
      - The cloud-controller-manager only runs controllers that are specific to your cloud provider. If you are running Kubernetes on your own premises, or in a learning environment inside your own PC, the cluster does not have a cloud controller manager.
      - As with the kube-controller-manager, the cloud-controller-manager combines several logically independent control loops into a single binary that you run as a single process. You can scale horizontally (run more than one copy) to improve performance or to help tolerate failures.
      - The following controllers can have cloud provider dependencies:
        - Node controller: For checking the cloud provider to determine if a node has been deleted in the cloud after it stops responding
        - Route controller: For setting up routes in the underlying cloud infrastructure
        - Service controller: For creating, updating and deleting cloud provider load balancers
  - Node Components
    - Node components run on every node, maintaining running pods and providing the Kubernetes runtime environment.
    - kubelet
      - An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod.
      - The kubelet takes a set of PodSpecs that are provided through various mechanisms and ensures that the containers described in those PodSpecs are running and healthy.
    - kube-proxy
      - kube-proxy is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept.
      - kube-proxy maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.
    - Container runtime
      - The container runtime is the software that is responsible for running containers.
  - Addons
    - Addons use Kubernetes resources (DaemonSet, Deployment, etc) to implement cluster features.
    - DNS
      - While the other addons are not strictly required, all Kubernetes clusters should have cluster DNS.
      - Containers started by Kubernetes automatically include this DNS server in their DNS searches.
    - Web UI (Dashboard)
      - Dashboard is a general purpose, web-based UI for Kubernetes clusters.
    - Container Resource Monitoring
      - Container Resource Monitoring records generic time-series metrics about containers in a central database, and provides a UI for browsing that data.
    - Cluster-level Logging
      - A cluster-level logging mechanism is responsible for saving container logs to a central log store with search/browsing interface.

Pods : https://kubernetes.io/docs/concepts/workloads/pods/
  - Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.
  - A Pod (as in a pod of whales or pea pod) is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers.
  - A Pod's contents are always co-located and co-scheduled, and run in a shared context.
  -  A Pod's contents are always co-located and co-scheduled, and run in a shared context. A Pod models an application-specific "logical host": it contains one or more application containers which are relatively tightly coupled.
  - In non-cloud contexts, applications executed on the same physical or virtual machine are analogous to cloud applications executed on the same logical host.
  - What is a Pod?
    - The shared context of a Pod is a set of Linux namespaces, cgroups, and potentially other facets of isolation - the same things that isolate a Docker container.
    - In terms of Docker concepts, a Pod is similar to a group of Docker containers with shared namespaces and shared filesystem volumes.
  - Using Pods
    - Usually you don't need to create Pods directly, even singleton Pods. Instead, create them using workload resources such as Deployment or Job.
    - If your Pods need to track state, consider the StatefulSet resource.
    - Pods in a Kubernetes cluster are used in two main ways:
      - Pods that run a single container. The "one-container-per-Pod" model is the most common Kubernetes use case; in this case, you can think of a Pod as a wrapper around a single container; Kubernetes manages Pods rather than managing the containers directly.
      - Pods that run multiple containers that need to work together. A Pod can encapsulate an application composed of multiple co-located containers that are tightly coupled and need to share resources. These co-located containers form a single cohesive unit of service.
    - Note: Grouping multiple co-located and co-managed containers in a single Pod is a relatively advanced use case. You should use this pattern only in specific instances in which your containers are tightly coupled.
    - Each Pod is meant to run a single instance of a given application. If you want to scale your application horizontally (to provide more overall resources by running more instances), you should use multiple Pods, one for each instance.
    - In Kubernetes, this is typically referred to as replication. Replicated Pods are usually created and managed as a group by a workload resource and its controller.
  - How Pods manage multiple containers
    - Pods are designed to support multiple cooperating processes (as containers) that form a cohesive unit of service. The containers in a Pod are automatically co-located and co-scheduled on the same physical or virtual machine in the cluster.
    - The containers can share resources and dependencies, communicate with one another, and coordinate when and how they are terminated.
    - Some Pods have init containers as well as app containers. Init containers run and complete before the app containers are started.
    - Pods natively provide two kinds of shared resources for their constituent containers: networking and storage.
  - Working with Pods
    - You'll rarely create individual Pods directly in Kubernetes—even singleton Pods.
    - When a Pod gets created (directly by you, or indirectly by a controller), the new Pod is scheduled to run on a Node in your cluster.
    - The Pod remains on that node until the Pod finishes execution, the Pod object is deleted, the Pod is evicted for lack of resources, or the node fails.
    - Note: Restarting a container in a Pod should not be confused with restarting a Pod. A Pod is not a process, but an environment for running container(s). A Pod persists until it is deleted.
  - Pods and controllers
    - You can use workload resources to create and manage multiple Pods for you. A controller for the resource handles replication and rollout and automatic healing in case of Pod failure.
  - Pod templates
    - Controllers for workload resources create Pods from a pod template and manage those Pods on your behalf.
    - PodTemplates are specifications for creating Pods, and are included in workload resources such as Deployments, Jobs, and DaemonSets.
    - Each controller for a workload resource uses the PodTemplate inside the workload object to make actual Pods.
    - The PodTemplate is part of the desired state of whatever workload resource you used to run your app.
    - Modifying the pod template or switching to a new pod template has no direct effect on the Pods that already exist. If you change the pod template for a workload resource, that resource needs to create replacement Pods that use the updated template.
    - For example, the StatefulSet controller ensures that the running Pods match the current pod template for each StatefulSet object. If you edit the StatefulSet to change its pod template, the StatefulSet starts to create new Pods based on the updated template. Eventually, all of the old Pods are replaced with new Pods, and the update is complete.
    - On Nodes, the kubelet does not directly observe or manage any of the details around pod templates and updates; those details are abstracted away.
  - Pod update and replacement
    - Kubernetes doesn't prevent you from managing Pods directly. It is possible to update some fields of a running Pod, in place.
    - However, Pod update operations like patch, and replace have some limitations:
    Most of the metadata about a Pod is immutable. For example, you cannot change the namespace, name, uid, or creationTimestamp fields; the generation field is unique. It only accepts updates that increment the field's current value.
      - If the metadata.deletionTimestamp is set, no new entry can be added to the metadata.finalizers list.
      - Pod updates may not change fields other than spec.containers[\*].image, spec.initContainers[\*].image, spec.activeDeadlineSeconds or spec.tolerations. For spec.tolerations, you can only add new entries.
      - When updating the spec.activeDeadlineSeconds field, two types of updates are allowed:
        - setting the unassigned field to a positive number;
        - updating the field from a positive number to a smaller, non-negative number.

Service : https://kubernetes.io/docs/concepts/services-networking/service/
  - An abstract way to expose an application running on a set of Pods as a network service.
  -  Kubernetes gives Pods their own IP addresses and a single DNS name for a set of Pods, and can load-balance across them.
  - Motivation
    - Kubernetes Pods are created and destroyed to match the state of your cluster. Pods are nonpermanent resources. If you use a Deployment to run your app, it can create and destroy Pods dynamically.
    - Each Pod gets its own IP address, however in a Deployment, the set of Pods running in one moment in time could be different from the set of Pods running that application a moment later.
    - if some set of Pods (call them "backends") provides functionality to other Pods (call them "frontends") inside your cluster services are what keep track of which IP to connect to.
  - Service resources
    - The set of Pods targeted by a Service is usually determined by a selector.
    - The Service abstraction enables decoupling.
    - Cloud-native service discovery
      - If you're able to use Kubernetes APIs for service discovery in your application, you can query the API server for Endpoints, that get updated whenever the set of Pods in a Service changes.
      - For non-native applications, Kubernetes offers ways to place a network port or load balancer in between your application and the backend Pods.
  - Defining a Service
    - A Service in Kubernetes is a REST object, similar to a Pod. Like all of the REST objects, you can POST a Service definition to the API server to create a new instance.
    - Kubernetes assigns a named Services an IP address (sometimes called the "cluster IP"), which is used by the Service proxies
    - The controller for the Service selector continuously scans for Pods that match its selector.
    - Note: A Service can map any incoming port to a targetPort. By default and for convenience, the targetPort is set to the same value as the port field.
    - Port definitions in Pods have names, and you can reference these names in the targetPort attribute of a Service.
    - As many Services need to expose more than one port, Kubernetes supports multiple port definitions on a Service object. Each port definition can have the same protocol, or a different one.
    - Services without selectors
      - Services most commonly abstract access to Kubernetes Pods, but they can also abstract other kinds of backends. For example:
        - You want to have an external database cluster in production, but in your test environment you use your own databases.
        - You want to point your Service to a Service in a different Namespace or on another cluster.
        - You are migrating a workload to Kubernetes. While evaluating the approach, you run only a portion of your backends in Kubernetes.
      - If a Service has no selector, the corresponding Endpoints object is not created automatically. You can manually map the Service to the network address and port where it's running, by adding an Endpoints object manually.
        - The name of the Endpoints object must be a valid DNS subdomain name.
        - The endpoint IPs must not be: loopback (127.0.0.0/8 for IPv4, ::1/128 for IPv6), or link-local (169.254.0.0/16 and 224.0.0.0/24 for IPv4, fe80::/64 for IPv6).
        - Accessing a Service without a selector works the same as if it had a selector.
      - An ExternalName Service is a special case of Service that does not have selectors and uses DNS names instead.
    - EndpointSlices : Kubernetes v1.17 [beta]
      - EndpointSlices are an API resource that can provide a more scalable alternative to Endpoints. Although conceptually quite similar to Endpoints, EndpointSlices allow for distributing network endpoints across multiple resources.
      - By default, an EndpointSlice is considered "full" once it reaches 100 endpoints, at which point additional EndpointSlices will be created to store any additional endpoints.
    - Application protocol : Kubernetes v1.20 [stable]
      - The appProtocol field provides a way to specify an application protocol for each Service port. The value of this field is mirrored by the corresponding Endpoints and EndpointSlice objects.
  - Virtual IPs and service proxies
    - Every node in a Kubernetes cluster runs a kube-proxy. kube-proxy is responsible for implementing a form of virtual IP for Services of type other than ExternalName.
    - Why not use round-robin DNS?
      - There are a few reasons for using proxying for Services:
        - There is a long history of DNS implementations not respecting record TTLs, and caching the results of name lookups after they should have expired.
        - Some apps do DNS lookups only once and cache the results indefinitely.
        - Even if apps and libraries did proper re-resolution, the low or zero TTLs on the DNS records could impose a high load on DNS that then becomes difficult to manage.
    - User space proxy mode
      - In this mode, kube-proxy watches the Kubernetes control plane for the addition and removal of Service and Endpoint objects. For each Service it opens a port (randomly chosen) on the local node. Any connections to this "proxy port" are proxied to one of the Service's backend Pods.
      - Lastly, the user-space proxy installs iptables rules which capture traffic to the Service's clusterIP (which is virtual) and port.
    - iptables proxy mode
      - In this mode, kube-proxy watches the Kubernetes control plane for the addition and removal of Service and Endpoint objects. For each Service, it installs iptables rules, which capture traffic to the Service's clusterIP and port, and redirect that traffic to one of the Service's backend sets. For each Endpoint object, it installs iptables rules which select a backend Pod.
      - Using iptables to handle traffic has a lower system overhead, because traffic is handled by Linux netfilter without the need to switch between userspace and the kernel space. This approach is also likely to be more reliable.
      - If kube-proxy is running in iptables mode and the first Pod that's selected does not respond, the connection fails. This is different from userspace mode: in that scenario, kube-proxy would detect that the connection to the first Pod had failed and would automatically retry with a different backend Pod.
    - IPVS proxy mode
      - In ipvs mode, kube-proxy watches Kubernetes Services and Endpoints, calls netlink interface to create IPVS rules accordingly and synchronizes IPVS rules with Kubernetes Services and Endpoints periodically.
      - This control loop ensures that IPVS status matches the desired state. When accessing a Service, IPVS directs traffic to one of the backend Pods.
      - The IPVS proxy mode is based on netfilter hook function that is similar to iptables mode, but uses a hash table as the underlying data structure and works in the kernel space.
      - That means kube-proxy in IPVS mode redirects traffic with lower latency than kube-proxy in iptables mode, with much better performance when synchronising proxy rules.
      - Compared to the other proxy modes, IPVS mode also supports a higher throughput of network traffic.
      - IPVS provides more options for balancing traffic to backend Pods; these are:
        - rr: round-robin
        - lc: least connection (smallest number of open connections)
        - dh: destination hashing
        - sh: source hashing
        - sed: shortest expected delay
        - nq: never queue
      - Note :
        - To run kube-proxy in IPVS mode, you must make IPVS available on the node before starting kube-proxy.
        - When kube-proxy starts in IPVS proxy mode, it verifies whether IPVS kernel modules are available. If the IPVS kernel modules are not detected, then kube-proxy falls back to running in iptables proxy mode.
      - In these proxy models, the traffic bound for the Service's IP:Port is proxied to an appropriate backend without the clients knowing anything about Kubernetes or Services or Pods.
      - If you want to make sure that connections from a particular client are passed to the same Pod each time, you can select the session affinity based on the client's IP addresses by setting service.spec.sessionAffinity to "ClientIP" (the default is "None").
  - Multi-Port Services
    - For some Services, you need to expose more than one port. Kubernetes lets you configure multiple port definitions on a Service object. When using multiple ports for a Service, you must give all of your ports names so that these are unambiguous.
    - Note: As with Kubernetes names in general, names for ports must only contain lowercase alphanumeric characters and -. Port names must also start and end with an alphanumeric character.
  - Choosing your own IP address
    - You can specify your own cluster IP address as part of a Service creation request. To do this, set the .spec.clusterIP field.
    - The IP address that you choose must be a valid IPv4 or IPv6 address from within the service-cluster-ip-range CIDR range that is configured for the API server.
      - If you try to create a Service with an invalid clusterIP address value, the API server will return a 422 HTTP status code to indicate that there's a problem.
  - Discovering services
    - Kubernetes supports 2 primary modes of finding a Service - environment variables and DNS.
    - Environment variables
      - When a Pod is run on a Node, the kubelet adds a set of environment variables for each active Service.
      - It supports both Docker links compatible variables (see makeLinkVariables) and simpler {SVCNAME}_SERVICE_HOST and {SVCNAME}_SERVICE_PORT variables, where the Service name is upper-cased and dashes are converted to underscores.
      - Note: When you have a Pod that needs to access a Service, and you are using the environment variable method to publish the port and cluster IP to the client Pods, you must create the Service before the client Pods come into existence. Otherwise, those client Pods won't have their environment variables populated.
    - DNS
      - You can (and almost always should) set up a DNS service for your Kubernetes cluster using an add-on.
      - A cluster-aware DNS server, such as CoreDNS, watches the Kubernetes API for new Services and creates a set of DNS records for each one. If DNS has been enabled throughout your cluster then all Pods should automatically be able to resolve Services by their DNS name.
      - For example, if you have a Service called my-service in a Kubernetes namespace my-ns, the control plane and the DNS Service acting together create a DNS record for my-service.my-ns. Pods in the my-ns namespace should be able to find the service by doing a name lookup for my-service (my-service.my-ns would also work).
  - Headless Services
    - Sometimes you don't need load-balancing and a single Service IP. In this case, you can create what are termed "headless" Services, by explicitly specifying "None" for the cluster IP (.spec.clusterIP).
    - You can use a headless Service to interface with other service discovery mechanisms, without being tied to Kubernetes' implementation.
    - For headless Services, a cluster IP is not allocated, kube-proxy does not handle these Services, and there is no load balancing or proxying done by the platform for them.
    - With selectors
      - For headless Services that define selectors, the endpoints controller creates Endpoints records in the API, and modifies the DNS configuration to return A records (IP addresses) that point directly to the Pods backing the Service.
    - Without selectors
      - For headless Services that do not define selectors, the endpoints controller does not create Endpoints records.
      - However, the DNS system looks for and configures either:
        - CNAME records for ExternalName-type Services.
        - A records for any Endpoints that share a name with the Service, for all other types.
  - Publishing Services (ServiceTypes)
    - For some parts of your application (for example, frontends) you may want to expose a Service onto an external IP address, that's outside of your cluster.
    - Kubernetes ServiceTypes allow you to specify what kind of Service you want. The default is ClusterIP.
    - Type values and their behaviors are:
      - ClusterIP: Exposes the Service on a cluster-internal IP. Choosing this value makes the Service only reachable from within the cluster. This is the default ServiceType.
      - NodePort: Exposes the Service on each Node's IP at a static port (the NodePort). A ClusterIP Service, to which the NodePort Service routes, is automatically created. You'll be able to contact the NodePort Service, from outside the cluster, by requesting <NodeIP>:<NodePort>.
      - LoadBalancer: Exposes the Service externally using a cloud provider's load balancer. NodePort and ClusterIP Services, to which the external load balancer routes, are automatically created.
      - ExternalName: Maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value. No proxying of any kind is set up.
        - Note: You need either kube-dns version 1.7 or CoreDNS version 0.0.8 or higher to use the ExternalName type.
      - You can also use Ingress to expose your Service. Ingress is not a Service type, but it acts as the entry point for your cluster. It lets you consolidate your routing rules into a single resource as it can expose multiple services under the same IP address.
    - Type NodePort
      - If you set the type field to NodePort, the Kubernetes control plane allocates a port from a range specified by --service-node-port-range flag (default: 30000-32767). Each node proxies that port (the same port number on every Node) into your Service. Your Service reports the allocated port in its .spec.ports[\*].nodePort field.
      - If you want to specify particular IP(s) to proxy the port, you can set the --nodeport-addresses flag in kube-proxy to particular IP block(s). This flag takes a comma-delimited list of IP blocks (e.g. 10.0.0.0/8, 192.0.2.0/25) to specify IP address ranges that kube-proxy should consider as local to this node.
      - If you want a specific port number, you can specify a value in the nodePort field.
      - The control plane will either allocate you that port or report that the API transaction failed. This means that you need to take care of possible port collisions yourself. You also have to use a valid port number, one that's inside the range configured for NodePort use.
      - Using a NodePort gives you the freedom to set up your own load balancing solution, to configure environments that are not fully supported by Kubernetes, or even to just expose one or more nodes' IPs directly.
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: my-service
      spec:
        type: NodePort
        selector:
          app: MyApp
        ports:
            # By default and for convenience, the `targetPort` is set to the same value as the `port` field.
          - port: 80
            targetPort: 80
            # Optional field
            # By default and for convenience, the Kubernetes control plane will allocate a port from a range (default: 30000-32767)
            nodePort: 30007
      ```
    - Type LoadBalancer
      - On cloud providers which support external load balancers, setting the type field to LoadBalancer provisions a load balancer for your Service. The actual creation of the load balancer happens asynchronously, and information about the provisioned balancer is published in the Service's .status.loadBalancer field.
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: my-service
      spec:
        selector:
          app: MyApp
        ports:
          - protocol: TCP
            port: 80
            targetPort: 9376
        clusterIP: 10.0.171.239
        type: LoadBalancer
      status:
        loadBalancer:
          ingress:
          - ip: 192.0.2.127
      ```
      - Traffic from the external load balancer is directed at the backend Pods. The cloud provider decides how it is load balanced.
      - Some cloud providers allow you to specify the loadBalancerIP. If you specify a loadBalancerIP but your cloud provider does not support the feature, the loadbalancerIP field that you set is ignored.
      - On Azure, if you want to use a user-specified public type loadBalancerIP, you first need to create a static type public IP address resource. This public IP address resource should be in the same resource group of the other automatically created resources of the cluster.
      - Load balancers with mixed protocol types
        - By default, for LoadBalancer type of Services, when there is more than one port defined, all ports must have the same protocol, and the protocol must be one which is supported by the cloud provider.
        - If the feature gate MixedProtocolLBService is enabled for the kube-apiserver it is allowed to use different protocols when there is more than one port defined.
        - Note: The set of protocols that can be used for LoadBalancer type of Services is still defined by the cloud provider.
      - Disabling load balancer NodePort allocation
        - Starting in v1.20, you can optionally disable node port allocation for a Service Type=LoadBalancer by setting the field spec.allocateLoadBalancerNodePorts to false.
        - This should only be used for load balancer implementations that route traffic directly to pods as opposed to using node ports.
      - Internal load balancer
        - In a mixed environment it is sometimes necessary to route traffic from Services inside the same (virtual) network address block.
        - In a split-horizon DNS environment you would need two Services to be able to route both external and internal traffic to your endpoints.
        - To set an internal load balancer, you must add  annotations to your Service depending on the cloud Service provider.
      - TLS support on AWS
        - For partial TLS / SSL support on clusters running on AWS, you can add three annotations to a LoadBalancer service:
        ```yaml
        metadata:
          name: my-service
          annotations:
            service.beta.kubernetes.io/aws-load-balancer-ssl-cert: arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012
            service.beta.kubernetes.io/aws-load-balancer-backend-protocol: (https|http|ssl|tcp)
        ```
        - The first specifies the ARN of the certificate to use. It can be either a certificate from a third party issuer that was uploaded to IAM or one created within AWS Certificate Manager.
        - The second annotation specifies which protocol a Pod speaks. For HTTPS and SSL, the ELB expects the Pod to authenticate itself over the encrypted connection, using a certificate.
          - HTTP and HTTPS selects layer 7 proxying: the ELB terminates the connection with the user, parses headers, and injects the X-Forwarded-For header with the user's IP address (Pods only see the IP address of the ELB at the other end of its connection) when forwarding requests.
          - TCP and SSL selects layer 4 proxying: the ELB forwards traffic without modifying the headers.
        - In a mixed-use environment where some ports are secured and others are left unencrypted, you can use the following annotations:
        ```yaml
        metadata:
           name: my-service
           annotations:
             service.beta.kubernetes.io/aws-load-balancer-backend-protocol: http
             service.beta.kubernetes.io/aws-load-balancer-ssl-ports: "443,8443"
        ```
        - In the above example, if the Service contained three ports, 80, 443, and 8443, then 443 and 8443 would use the SSL certificate, but 80 would just be proxied HTTP.
        - From Kubernetes v1.9 onwards you can use predefined AWS SSL policies with HTTPS or SSL listeners for your Services.
        - You can then specify any one of those policies using the "service.beta.kubernetes.io/aws-load-balancer-ssl-negotiation-policy"
      - PROXY protocol support on AWS
        - To enable PROXY protocol support for clusters running on AWS :
        ```yaml
        metadata:
           name: my-service
           annotations:
             service.beta.kubernetes.io/aws-load-balancer-proxy-protocol: "*"
        ```
      - ELB Access Logs on AWS
        - The annotation service.beta.kubernetes.io/aws-load-balancer-access-log-enabled controls whether access logs are enabled.
        - The annotation service.beta.kubernetes.io/aws-load-balancer-access-log-emit-interval controls the interval in minutes for publishing the access logs. You can specify an interval of either 5 or 60 minutes.
        - The annotation service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-name controls the name of the Amazon S3 bucket where load balancer access logs are stored.
        - The annotation service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-prefix specifies the logical hierarchy you created for your Amazon S3 bucket.
        ```yaml
        metadata:
          name: my-service
          annotations:
            service.beta.kubernetes.io/aws-load-balancer-access-log-enabled: "true"
            # Specifies whether access logs are enabled for the load balancer
            service.beta.kubernetes.io/aws-load-balancer-access-log-emit-interval: "60"
            # The interval for publishing the access logs. You can specify an interval of either 5 or 60 (minutes).
            service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-name: "my-bucket"
            # The name of the Amazon S3 bucket where the access logs are stored
            service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-prefix: "my-bucket-prefix/prod"
            # The logical hierarchy you created for your Amazon S3 bucket, for example `my-bucket-prefix/prod`
        ```
      - Connection Draining on AWS
        - Connection draining for Classic ELBs can be managed with the annotation service.beta.kubernetes.io/aws-load-balancer-connection-draining-enabled set to the value of "true". The annotation service.beta.kubernetes.io/aws-load-balancer-connection-draining-timeout can also be used to set maximum time, in seconds, to keep the existing connections open before deregistering the instances.
        ```yaml
        metadata:
          name: my-service
          annotations:
            service.beta.kubernetes.io/aws-load-balancer-connection-draining-enabled: "true"
            service.beta.kubernetes.io/aws-load-balancer-connection-draining-timeout: "60"
        ```
      - Other ELB annotations
      ```yaml
      metadata:
        name: my-service
        annotations:
          service.beta.kubernetes.io/aws-load-balancer-connection-idle-timeout: "60"
          # The time, in seconds, that the connection is allowed to be idle (no data has been sent over the connection) before it is closed by the load balancer

          service.beta.kubernetes.io/aws-load-balancer-cross-zone-load-balancing-enabled: "true"
          # Specifies whether cross-zone load balancing is enabled for the load balancer

          service.beta.kubernetes.io/aws-load-balancer-additional-resource-tags: "environment=prod,owner=devops"
          # A comma-separated list of key-value pairs which will be recorded as
          # additional tags in the ELB.

          service.beta.kubernetes.io/aws-load-balancer-healthcheck-healthy-threshold: ""
          # The number of successive successful health checks required for a backend to
          # be considered healthy for traffic. Defaults to 2, must be between 2 and 10

          service.beta.kubernetes.io/aws-load-balancer-healthcheck-unhealthy-threshold: "3"
          # The number of unsuccessful health checks required for a backend to be
          # considered unhealthy for traffic. Defaults to 6, must be between 2 and 10

          service.beta.kubernetes.io/aws-load-balancer-healthcheck-interval: "20"
          # The approximate interval, in seconds, between health checks of an
          # individual instance. Defaults to 10, must be between 5 and 300

          service.beta.kubernetes.io/aws-load-balancer-healthcheck-timeout: "5"
          # The amount of time, in seconds, during which no response means a failed
          # health check. This value must be less than the service.beta.kubernetes.io/aws-load-balancer-healthcheck-interval
          # value. Defaults to 5, must be between 2 and 60

          service.beta.kubernetes.io/aws-load-balancer-security-groups: "sg-53fae93f"
          # A list of existing security groups to be added to ELB created. Unlike the annotation
          # service.beta.kubernetes.io/aws-load-balancer-extra-security-groups, this replaces all other security groups previously assigned to the ELB.

          service.beta.kubernetes.io/aws-load-balancer-extra-security-groups: "sg-53fae93f,sg-42efd82e"
          # A list of additional security groups to be added to the ELB

          service.beta.kubernetes.io/aws-load-balancer-target-node-labels: "ingress-gw,gw-name=public-api"
          # A comma separated list of key-value pairs which are used
          # to select the target nodes for the load balancer
      ```
      - Network Load Balancer support on AWS
        - To use a Network Load Balancer on AWS, use the annotation service.beta.kubernetes.io/aws-load-balancer-type with the value set to nlb.
        - Unlike Classic Elastic Load Balancers, Network Load Balancers (NLBs) forward the client's IP address through to the node. If a Service's .spec.externalTrafficPolicy is set to Cluster, the client's IP address is not propagated to the end Pods.
        - By setting .spec.externalTrafficPolicy to Local, the client IP addresses is propagated to the end Pods, but this could result in uneven distribution of traffic. Nodes without any Pods for a particular LoadBalancer Service will fail the NLB Target Group's health check on the auto-assigned .spec.healthCheckNodePort and not receive any traffic.
          - In order to achieve even traffic, either use a DaemonSet or specify a pod anti-affinity to not locate on the same node.
        - In order to limit which client IP's can access the Network Load Balancer, specify loadBalancerSourceRanges.
        ```yaml
        spec:
          loadBalancerSourceRanges:
            - "143.231.0.0/16"
        ```
        - If .spec.loadBalancerSourceRanges is not set, Kubernetes allows traffic from 0.0.0.0/0 to the Node Security Group(s). If nodes have public IP addresses, be aware that non-NLB traffic can also reach all instances in those modified security groups.
    - Type ExternalName
      - Services of type ExternalName map a Service to a DNS name, not to a typical selector such as my-service or cassandra. You specify these Services with the spec.externalName parameter.
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: my-service
        namespace: prod
      spec:
        type: ExternalName
        externalName: my.database.example.com
      ```
      - Note: ExternalName accepts an IPv4 address string, but as a DNS names comprised of digits, not as an IP address. ExternalNames that resemble IPv4 addresses are not resolved by CoreDNS or ingress-nginx because ExternalName is intended to specify a canonical DNS name. To hardcode an IP address, consider using headless Services.
    - External IPs
      - If there are external IPs that route to one or more cluster nodes, Kubernetes Services can be exposed on those externalIPs. Traffic that ingresses into the cluster with the external IP (as destination IP), on the Service port, will be routed to one of the Service endpoints. externalIPs are not managed by Kubernetes and are the responsibility of the cluster administrator.
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: my-service
      spec:
        selector:
          app: MyApp
        ports:
          - name: http
            protocol: TCP
            port: 80
            targetPort: 9376
        externalIPs:
          - 80.11.12.10
      ```
      - In the Service spec, externalIPs can be specified along with any of the ServiceTypes
  - Shortcomings
    - Using the userspace proxy for VIPs works at small to medium scale, but will not scale to very large clusters with thousands of Services.
    - Using the userspace proxy obscures the source IP address of a packet accessing a Service. This makes some kinds of network filtering (firewalling) impossible. The iptables proxy mode does not obscure in-cluster source IPs, but it does still impact clients coming through a load balancer or node-port.
    - The Type field is designed as nested functionality - each level adds to the previous. This is not strictly required on all cloud providers but the current API requires it.
  - Virtual IP implementation
    - Avoiding collisions
      - One of the primary philosophies of Kubernetes is that you should not be exposed to situations that could cause your actions to fail through no fault of your own
      - In order to allow you to choose a port number for your Services, we must ensure that no two Services can collide. Kubernetes does that by allocating each Service its own IP address.
      - To ensure each Service receives a unique IP, an internal allocator atomically updates a global allocation map in etcd prior to creating each Service. The map object must exist in the registry for Services to get IP address assignments, otherwise creations will fail with a message indicating an IP address could not be allocated.
      - Kubernetes also uses controllers to check for invalid assignments (eg due to administrator intervention) and for cleaning up allocated IP addresses that are no longer used by any Services.
    - Service IP addresses
      - Unlike Pod IP addresses, which actually route to a fixed destination, Service IPs are not actually answered by a single host. Instead, kube-proxy uses iptables (packet processing logic in Linux) to define virtual IP addresses which are transparently redirected as needed.
      - When clients connect to the VIP, their traffic is automatically transported to an appropriate endpoint. The environment variables and DNS for Services are actually populated in terms of the Service's virtual IP address.
      - kube-proxy supports three proxy modes—userspace, iptables and IPVS—which each operate slightly differently.
      - Userspace
        - As an example, When the backend Service is created, the Kubernetes master assigns a virtual IP address, for example 10.0.0.1. Assuming the Service port is 1234, the Service is observed by all of the kube-proxy instances in the cluster. When a proxy sees a new Service, it opens a new random port, establishes an iptables redirect from the virtual IP address to this new port, and starts accepting connections on it.
        - When a client connects to the Service's virtual IP address, the iptables rule kicks in, and redirects the packets to the proxy's own port. The "Service proxy" chooses a backend, and starts proxying traffic from the client to the backend.
        - This means that Service owners can choose any port they want without risk of collision. Clients can connect to an IP and port, without being aware of which Pods they are actually accessing.
      - iptables
        - When the backend Service is created, the Kubernetes control plane assigns a virtual IP address, for example 10.0.0.1. Assuming the Service port is 1234, the Service is observed by all of the kube-proxy instances in the cluster. When a proxy sees a new Service, it installs a series of iptables rules which redirect from the virtual IP address to per-Service rules. The per-Service rules link to per-Endpoint rules which redirect traffic (using destination NAT) to the backends.
        - When a client connects to the Service's virtual IP address the iptables rule kicks in. A backend is chosen (either based on session affinity or randomly) and packets are redirected to the backend. Unlike the userspace proxy, packets are never copied to userspace, the kube-proxy does not have to be running for the virtual IP address to work, and Nodes see traffic arriving from the unaltered client IP address.
        - This same basic flow executes when traffic comes in through a node-port or through a load-balancer, though in those cases the client IP does get altered.
      - IPVS
        - iptables operations slow down dramatically in large scale cluster e.g 10,000 Services. IPVS is designed for load balancing and based on in-kernel hash tables. So you can achieve performance consistency in large number of Services from IPVS-based kube-proxy. Meanwhile, IPVS-based kube-proxy has more sophisticated load balancing algorithms (least conns, locality, weighted, persistence).
  - Supported protocols
    - TCP
      - default
    - UDP
      - You can use UDP for most Services. For type=LoadBalancer Services,  UDP support depends on the cloud provider offering this facility.
    - SCTP
      - When using a network plugin that supports SCTP traffic, you can use SCTP for most Services. For type=LoadBalancer Services, SCTP support depends on the cloud provider offering this facility.
      - Most don't support it.
      - SCTP is not supported on Windows based nodes.
      - Warning: The kube-proxy does not support the management of SCTP associations when it is in userspace mode.
      - Warning: The support of multihomed SCTP associations requires that the CNI plugin can support the assignment of multiple interfaces and IP addresses to a Pod.
    - HTTP
      - If your cloud provider supports it, you can use a Service in LoadBalancer mode to set up external HTTP / HTTPS reverse proxying, forwarded to the Endpoints of the Service.
    - PROXY protocol
      - If your cloud provider supports it, you can use a Service in LoadBalancer mode to configure a load balancer outside of Kubernetes itself, that will forward connections prefixed with PROXY protocol.
      - The load balancer will send an initial series of octets describing the incoming connection.

Using Source IP - https://kubernetes.io/docs/tutorials/services/source-ip/
  - Applications running in a Kubernetes cluster find and communicate with each other, and the outside world, through the Service abstraction.
  - Terminology
    - NAT
      - network address translation
    - Source NAT
      - replacing the source IP on a packet; in this page, that usually means replacing with the IP address of a node.
    - Destination NAT
      - replacing the destination IP on a packet; in this page, that usually means replacing with the IP address of a Pod
    - VIP
      - a virtual IP address, such as the one assigned to every Service in Kubernetes
    - kube-proxy
      - a network daemon that orchestrates Service VIP management on every node
  - Source IP for Services with Type=ClusterIP
    - Packets sent to ClusterIP from within the cluster are never source NAT'd if you're running kube-proxy in iptables mode, (the default). You can query the kube-proxy mode by fetching http://localhost:10249/proxyMode on the node where kube-proxy is running.
  - Source IP for Services with Type=NodePort
    - Packets sent to Services with Type=NodePort are source NAT'd by default.
    - To avoid this, Kubernetes has a feature to preserve the client source IP. If you set service.spec.externalTrafficPolicy to the value Local, kube-proxy only proxies proxy requests to local endpoints, and does not forward traffic to other nodes. This approach preserves the original source IP address. If there are no local endpoints, packets sent to the node are dropped, so you can rely on the correct source-ip in any packet processing rules you might apply a packet that make it through to the endpoint.
  - Source IP for Services with Type=LoadBalancer
    - Packets sent to Services with Type=LoadBalancer are source NAT'd by default, because all schedulable Kubernetes nodes in the Ready state are eligible for load-balanced traffic. So if packets arrive at a node without an endpoint, the system proxies it to a node with an endpoint, replacing the source IP on the packet with the IP of the node.
    - If you're running on Google Kubernetes Engine/GCE, setting the same service.spec.externalTrafficPolicy field to Local forces nodes without Service endpoints to remove themselves from the list of nodes eligible for loadbalanced traffic by deliberately failing health checks.
    - A controller running on the control plane is responsible for allocating the cloud load balancer. The same controller also allocates HTTP health checks pointing to this port/path on each node.

kubectl for Docker Users : https://kubernetes.io/docs/reference/kubectl/docker-cli-to-kubectl/
  - The following sections show a Docker sub-command and describe the equivalent kubectl command.
  ```bash
  docker run -d --restart=always -e DOMAIN=cluster --name nginx-app -p 80:80 nginx

  # start the pod running nginx
  kubectl create deployment

  # add env to nginx-app
  kubectl set env deployment/nginx-app  DOMAIN=cluster --image=nginx nginx-app

  # expose a port through with a service
  kubectl expose deployment nginx-app --port=80 --name=nginx-http
  ```
  - Note: kubectl commands print the type and name of the resource created or mutated, which can then be used in subsequent commands. You can expose a new Service after a Deployment is created.
  - By using kubectl, you can create a Deployment to ensure that N pods are running nginx, where N is the number of replicas stated in the spec and defaults to 1. You can also create a service with a selector that matches the pod labels.
  - By default images run in the background, similar to docker run -d .... To run things in the foreground, use kubectl run to create pod:
  ```bash
  kubectl run [-i] [--tty] --attach <name> --image=<image>
  ```
  - Unlike docker run ..., if you specify --attach, then you attach stdin, stdout and stderr. You cannot control which streams are attached (docker -a ...). To detach from the container, you can type the escape sequence Ctrl+P followed by Ctrl+Q.
  ```bash
  docker ps -a
  kubectl get po
  ```
  - To attach a process that is already running in a container :
  ```bash
  docker ps
  docker attach 55c103fa1296
  kubectl get pods
  kubectl attach -it nginx-app-5jyvm
  ```
  - To execute a command in a container :
  ```bash
  docker exec 55c103fa1296 cat /etc/hostname
  kubectl exec nginx-app-5jyvm -- cat /etc/hostname
  ```
  - Logs :
  ```bash
  docker logs -f a9e

  kubectl logs -f nginx-app-zibvs
  ```
  - There is a slight difference between pods and containers; by default pods do not terminate if their processes exit. Instead the pods restart the process. This is similar to the docker run option --restart=always with one major difference. In docker, the output for each invocation of the process is concatenated, but for Kubernetes, each invocation is separate.
  - docker stop and docker rm
  ```bash
  docker ps
  docker stop a9ec34d98787
  docker rm a9ec34d98787

  kubectl get deployment nginx-app
  kubectl get po -l run=nginx-app
  kubectl delete deployment nginx-app
  kubectl get po -l run=nginx-app
  ```
  - Note: When you use kubectl, you don't delete the pod directly. You have to first delete the Deployment that owns the pod. If you delete the pod directly, the Deployment recreates the pod.
  - docker info :
  ```bash
  docker info
  kubectl cluster-info
  ```

Install Tools : https://kubernetes.io/docs/tasks/tools/#install-kubectl-on-windows
  - kubectl
    - The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters.
  - kind
    - kind lets you run Kubernetes on your local computer. This tool requires that you have Docker installed and configured.
  - minikube
    - Like kind, minikube is a tool that lets you run Kubernetes locally. minikube runs a single-node Kubernetes cluster on your personal computer.
  - kubeadm
    - You can use the kubeadm tool to create and manage Kubernetes clusters. It performs the actions necessary to get a minimum viable, secure cluster up and running in a user friendly way.

Microk8s : https://github.com/ubuntu/microk8s
  - Single-package fully conformant lightweight Kubernetes that works on 42 flavours of Linux. Perfect for:
    - Developer workstations
    - IoT
    - Edge
    - CI/CD

stern : https://github.com/wercker/stern
  - Stern allows you to tail multiple pods on Kubernetes and multiple containers within the pod. Each result is color coded for quicker debugging.
  - The query is a regular expression so the pod name can easily be filtered and you don't need to specify the exact id (for instance omitting the deployment id). If a pod is deleted it gets removed from tail and if a new pod is added it automatically gets tailed.
  - When a pod contains multiple containers Stern can tail all of them too without having to do this manually for each one. Simply specify the container flag to limit what containers to show. By default all containers are listened to.

Kubernetes DNS-Based Service Discovery : https://github.com/kubernetes/dns/blob/master/docs/specification.md
  - While service discovery in Kubernetes may be provided via other protocols and mechanisms, DNS is very commonly used and is a highly recommended add-on.
  - Resource Records : Any DNS-based service discovery solution for Kubernetes must provide the resource records (RRs)
    1. Definitions
      - Values not in angle brackets, ```< >```, are literals. The meaning of the values in angle brackets are defined below or in the description of the specific record.
        - ```<zone>``` = configured cluster domain, e.g. cluster.local
        - ```<ns>``` = a Namespace
        - ```<ttl>``` = the standard DNS time-to-live value for the record
      - hostname
        - In order of precedence, the hostname of an endpoint is:
          - The value of the endpoint's hostname field.
          - A unique, system-assigned identifier for the endpoint. The exact format and source of this identifier is not prescribed by this specification. However, it must be possible to use this to identify a specific endpoint in the context of a Service. This is used in the event no explicit endpoint hostname is defined.
      - ready
        - An endpoint is considered ready if its address is in the addresses field of the EndpointSubset object, or the corresponding service has the service.alpha.kubernetes.io/tolerate-unready-endpoints annotation set to true.
    2. Record for Schema Version
      - There must be a TXT record named ```dns-version.<zone>```. that contains the semantic version of the DNS schema in use in this cluster.
      - Record Format:
        - ```dns-version.<zone>. <ttl> IN TXT <schema-version>```
      - Question Example:
        - ```dns-version.cluster.local. IN TXT```
      - Answer Example:
        - ```dns-version.cluster.local. 28800 IN TXT "1.1.0"```
    3. Records for a Service with ClusterIP
      - Given a Service named ```<service>``` in Namespace ```<ns>``` with ClusterIP ```<cluster-ip>```, the following records must exist.
      - I. A/AAAA Record
        - If the ```<cluster-ip>``` is an IPv4 address, an A record of the following form must exist.
          - Record Format:
            - ```<service>.<ns>.svc.<zone>. <ttl> IN A <cluster-ip>```
          - Question Example:
            - kubernetes.default.svc.cluster.local. IN A
          - Answer Example:
            - kubernetes.default.svc.cluster.local. 4 IN A 10.3.0.1
        - If the ```<cluster-ip>``` is an IPv6 address, an AAAA record of the following form must exist.
        - Record Format:
          - ```<service>.<ns>.svc.<zone>. <ttl> IN AAAA <cluster-ip>```
        - Question Example:
          - kubernetes.default.svc.cluster.local. IN AAAA
        - Answer Example:
          - kubernetes.default.svc.cluster.local. 4 IN AAAA 2001:db8::1
      - II. SRV Records
        - For each port in the Service with name ```<port>``` and number ```<port-number>``` using protocol ```<proto>```, an SRV record of the following form must exist.
          - Record Format:
_             - ```<port>.<proto>.<service>.<ns>.svc.<zone>. <ttl> IN SRV <weight> <priority> <port-number> <service>.<ns>.svc.<zone>.```
        - The priority ```<priority>``` and weight ```<weight>``` are numbers as described in RFC2782 and whose values are not prescribed by this specification.
        - Unnamed ports do not have an SRV record.
          - Question Example:
            - ```_https._tcp.kubernetes.default.svc.cluster.local. IN SRV```
          - Answer Example:
            - ```_https._tcp.kubernetes.default.svc.cluster.local. 30 IN SRV 10 100 443 kubernetes.default.svc.cluster.local.```
      - III. PTR Record
        - Given an IPv4 Service ClusterIP <a>.<b>.<c>.<d>, a PTR record of the following form must exist.
          - Record Format:
            - ```<d>.<c>.<b>.<a>.in-addr.arpa. <ttl> IN PTR <service>.<ns>.svc.<zone>.```
          - Question Example:
            - ```1.0.3.10.in-addr.arpa. IN PTR```
          - Answer Example:
            - ``` 1.0.3.10.in-addr.arpa. 14 IN PTR kubernetes.default.svc.cluster.local. ```
        - Given an IPv6 Service ClusterIP represented in hexadecimal format without any simplification :  ```<a1a2a3a4:b1b2b3b4:c1c2c3c4:d1d2d3d4:e1e2e3e4:f1f2f3f4:g1g2g3g4:h1h2h3h4>```, a PTR record as a sequence of nibbles in reverse order of the following form must exist.
          - Record Format:
            - ```h4.h3.h2.h1.g4.g3.g2.g1.f4.f3.f2.f1.e4.e3.e2.e1.d4.d3.d2.d1.c4.c3.c2.c1.b4.b3.b2.b1.a4.a3.a2.a1.ip6.arpa <ttl> IN PTR <service>.<ns>.svc.<zone>.```
          - Question Example:
            - ``` 1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.b.d.0.1.0.0.2.ip6.arpa. IN PTR```
          - Answer Example:
            - ```1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.b.d.0.1.0.0.2.ip6.arpa. 14 IN PTR kubernetes.default.svc.cluster.local. ```
    4. Records for a Headless Service
      - Given a headless Service ```<service>``` in Namespace ```<ns>``` (i.e., a Service with no ClusterIP), the following records must exist.
        - I. A/AAAA Records
        - II. SRV Records
        - III. PTR Records
    5. Records for External Name Services
      - Given a Service named ```<service>``` in Namespace ```<ns>``` with ExternalName ```<extname>```, a CNAME record named ```<service>.<ns>.svc.<zone>``` pointing to ```<extname>``` must exist.
        - If the IP family of the service is IPv4:
          - Record Format:
            - ```<service>.<ns>.svc.<zone>. <ttl> IN CNAME <extname>.```
          - Question Example:
            - foo.default.svc.cluster.local. IN A
          - Answer Example:
            - foo.default.svc.cluster.local. 10 IN CNAME www.example.com.
            - www.example.com. 28715 IN A 192.0.2.53
        - If the IP family of the service is IPv6:
          - Record Format:
            - ```<service>.<ns>.svc.<zone>. <ttl> IN CNAME <extname>.```
          - Question Example:
            - foo.default.svc.cluster.local. IN AAAA
          - Answer Example:
            - foo.default.svc.cluster.local. 10 IN CNAME www.example.com.
            - www.example.com. 28715 IN AAAA 2001:db8::1

- Using a Service to Expose Your App : https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/
  - Overview of Kubernetes Services
    - Kubernetes Pods are mortal. Pods in fact have a lifecycle. When a worker node dies, the Pods running on the Node are also lost.
    - A ReplicaSet might then dynamically drive the cluster back to desired state via creation of new Pods to keep your application running.
    - Although each Pod has a unique IP address, those IPs are not exposed outside the cluster without a Service.
    - Services allow your applications to receive traffic. Services can be exposed in different ways by specifying a type in the ServiceSpec:
      - ClusterIP (default) - Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster.
      - NodePort - Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using <NodeIP>:<NodePort>. Superset of ClusterIP.
      - LoadBalancer - Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service. Superset of NodePort.
      - ExternalName - Exposes the Service using an arbitrary name (specified by externalName in the spec) by returning a CNAME record with the name. No proxy is used. This type requires v1.7 or higher of kube-dns.
  - Services and Labels
    - A Service routes traffic across a set of Pods.
    - Services are the abstraction that allow pods to die and replicate in Kubernetes without impacting your application.
    - Services match a set of Pods using labels and selectors, a grouping primitive that allows logical operation on objects in Kubernetes. Labels are key/value pairs attached to objects and can be used in any number of ways:
      - Designate objects for development, test, and production
      - Embed version tags
      - Classify an object using tags
