Docker Swarm
  - Containers Everywhere = New Problems
    - How do we automate container lifecycle
    - How can we easily scale out/in/up/down?
    - How can we ensure our containers are re-created if they fail?
    - How can we replace containers without downtime (blue/green deployment)?
    - How can we control/track where containers get started?
    - How can we store secret, keys, passwords etc. and get them to the right container (and only that container)?

Swarm Mode : Built-In Orchestration
  - Swarm Mode is a clustering solution built inside Docker.
  - Not related to Swarm "classic" for pre-1.12 versions
  - Added in 1.12 (Summer 2016) via SwarmKit toolkit
  - Enhanced in 1.13 (Jan. 2017) via Stacks and Secrets.
  - Docker swarm not enabled by default, new commands once enables:
    - docker swarm
    - docker node
    - docker service
    - docker stack
    - docker secret
  - Managers and workers
  - Control plain is how orders get sent around the swarm for taking actions.
  - Managers are workers with permissions to control the swarm.
  - docker swarm command replaces the docker run command. For swarms
  - A single service can have multiple tasks and each one of those tasks will launch a container.
  - docker swarm init => to initialize swarm
    - Lots of PKI and security automation
      - Root signing Certificate created for our swarm
      - Certificate is issued for first manager node
      - join tokens created.
    - Raft database created to store root CA, configs and secrets
      - Encrypted by default on disk (1.13+)
      - No need for another key/value system to hold orchestration/secrets
      - Replicates logs amongst Managers via mutual TLS (Transport Layer Security) in "control plane".
  - docker service --help
  - docker service replaces docker run in a swarm
  - docker service create alpine ping 8.8.8.8
  - docker service ps frosty_newton
    - will show the tasks or containers of this service.
  - docker service update <ID> --replicas 3
  - docker update --help
  - The swarm will update containers in a way to have consistent availability.
  - docker container rm -f <name>.1.<ID>
  - docker swarm init --advertise-addr <IP Address>
  - sudo docker-machine ssh <node>
  - docker node update --role manager <node>
  - docker swarm join-token manager


Swarm Basic Features and how to use them in your workflow
  - Scaling out with overlay networking :
    - Just choose --driver overlay when creating network.
    - For container-to-container traffic inside a single Swarm
    - Optional IPSec (AES) encryption on network creation.
    - Each service can be connected to multiple networks
      - (E.g. front-end, back-end)
    ```bash
    docker network create --driver overlay mydrupal
    docker service create --name psql --network mydrupal -e POSTGRES_PASSWORD=mypass postgres
    docker service create --name drupal --network mydrupal -p 80:80 drupal
    ```
  - Routing mesh:
    - Routes ingress (incoming) packets for a service to proper task.
    - spans all nodes in swarm
    - uses IPVS from Linux Kernel
    - Load balances Swarm services across their tasks.
    - Two ways this works:
      - Container-to-container in a overlay network (uses VIP).
      - External traffic incoming traffic incoming to published ports (all nodes listen).
    - This is a stateless load balancing.
    - This LB is at OSI Layer 3 (TCP), not Layer (DNS).
    - Both of the limitations can be overcome with :
      - Nginx or HAProxy LB proxy, or :
      - Docker Enterprise Edition, which comes with built-in L4 web proxy.

Stacks : Production Grade Compose:
  - In 1.13 Docker adds a new layer of abstraction to Swarm called Stacks.
  - Stacks accept compose files as their declarative definition for services, networks and volumes.
    - Also includes secrets!
  - We use docker stack deploy rather than docker service create
  - Stacks manages all those objects for us, including overlay network per stack. Adds stack name to start of their name.
  - New deploy: key in Compose file. Can't do build:
  - Compose now ignores deploy:, Swarm ignores build:
    - Building is not something that should happen on your production swarm.
  - docker-compse cli not needed on Swarm server.
  - Stack manages the swarm
  - docker stack deploy -c example-voting-app-tack.yml voteapp
  - docker stack --help
  - docker stack ls
  - docker ps voteapp
    - You can see which node they're working on.
  - docker stack services <name>
  - docker stack ps <name>
  - docker network ls
  - docker visualizer : 8080
    - See which nodes have what.
  - If you use a config file to manage your service
    - You just have to deploy the config file again.

Secrets Storage for Swarm: Protecting Your Environment Variables
  - Secrets Storage :
    - Easiest"secure" solution for storing secrets in Swarm.
      - Built into swarm and out of the box.
      - Version 1.13 of swarm or higher.
      - Encrypted in transit.
    - What is a secret?
      - usernames and passwords.
      - TLS certificates and keys.
      - SSH keys.
      - Any data you would prefer not be "on front page of news".
      - Anything to allow connectivity between things.
    - Supports generic strings or binary content up to 500Kb in size.
    - Doesn't require apps to be rewritten in order to get these.
    - As of Docker 1.13.0 Swarm Raft DM is encryted on disk.
    - Only stored on disk on Manager nodes.
    - Default is managers and workers "control plane" is TLS + Mutual Auth.
    - Secrets are first stored in Swarm DB using docker secrets commands, then assigned to a Service(s).
    - Only containers in assigned Service(s) can see them.
    - They look like files in container but are actually in-memory fs.
    - /run/secrets/<secret_name> or /run/secrets/<secret_alias>
    - Can have multiple names for the same key.
    - Local docker-compose can use file-based secrets, but not secure.
    - Secrets are a swarm only thing.
    - Two ways to create a secret within swarm.
      1. Give it a file
        - docker secret create psql_user psql_user.txt
        - You are storing the password on the hard drive on the host/server.
        - If someone was able to get root they would be able to get these files out.
      2. Give it a value within a command line.
    - docker secret ls
    - docker secret inspect psql_user
      - won't give us the information.
    - only the containers and services that have been assigned to the secret will be able to see the decrypted secret.
    - docker service create --name psql --secret psql_user --secret psql_pass -e POSTGRES_PASSWORD_FILE=/run/secrets/psql_pass -e POSTGRES_USER_FILE=/run/secrets/psql_user postgres
    - docker exec -it <container name> bash
    - docker logs <container name>
    - docker service update --secret-rm (If you remove a secret it will redeploy the container.)

Secrets with stacks:
  - docker service create --name search --replicas 3 -p 9200:9200 elasticsearch:2
  - to use secrets we need 3.1 or newer.
  - Can have files with secrets defined. Or define them in DockerCompose file.
  - Tell the compose file the secrets and where they are defined.
  - Can define the permissions and users that are allowed to use the standard linux mode.
  - docker stack deploy -c docker-compose.yml mydb

Swarm App LifeCycle
  - Compose in not a prod tool.
  - docker-compose up -d
  - docker-compose exec psql cat /run/secrets/{secret}
  - We can still use secrets in test and dev!
  - Local docker-compose up for dev env
  - Remote docker-compose up CI for env
  - Remote docker stack deploy for prod env
  - Can have docker compose build on itself:
    - usually if you dont specify anything and just use docker-compose up and have a docker-compose.yml and docker-compose.override.yml, override will be the final compose.
    - If you want to use different one in order of base to final. :  docker-compose.yml -f docker-compose.test.yml up -d

Swarm App Service Updates:
  - Provides rolling replacement of tasks/containers in a service.
  - Limits downtime (be careful with "prevents" downtime).
  - Will replace containers for most changes
  - Has many many cli options to control the update.
  - Create options will usually change, adding -add or -rm to them.
  - Includes rollback and healthcheck options.
  - Also has scale & rollback subcommand for quicker access.
    - docker service scale web = 4 and docker service rollback web.
  - A stack deploy, when pre-existing, will issue service updates.
  - Examples :
    - Just update the image to used to a newer version.
      - docker service update --image myapp:1.2.1 <servicename>
    - Adding an environment variable and remove a port
      - docker service update --env-add NOD_ENV=production --publish-rm 8080
    - Change number of replicas of two services
      - docker service scale web=8 api=6
  - Updates in Stack files:
    - Same command. Just edit the YAML file, then
      - docker stack deploy -c fileyml stackname

  ```bash
  docker service update --image nginx:1.13.6 web

  docker service update --publish-rm 8088 --publish-add 9090:80 web

  docker service update --force web

  docker service rm web
  ```

Swarm App Healthchecks
  - HEALTHCHECK was added in 1.12
  - Supported in Dockerfile, Compose YAML, docker run and Swarm Services.
  - When going production try to work with HEALTHCHECK
  - Docker engine will exec the command in the container
    - e.g curl localhost
  - It expects exit 0 (OK) or exit 1 (Error)
  - Three container states : starting, healthy, unhealthy.
  - Much better than "is binary still running?"
  - Not an external monitoring replacement.
  - Healthcheck status shows up in docker container ls
  - Check last 5 healthchecks with docker container inspect.
  - Docker run does nothing with healthchecks.
  - Services will replace tasks if they fail healthcheck.
  - Service updates wait for them before continuing.
    - Options for healthcheck in Dockerfile:
      - --interval=DURATION (default : 30s)
      - --timeout=DURATION (default: 30s)
      - --start-period=DURATION (default : 0s) (17.09+)
      - --retries=N (default: 3)
    - Basic command using default options:
      - HEALTHCHECK curl -f https://localhost/ || false
    - Custom options with the command:
      - HEALTHCHECK --timeout=2s --inerval=3s  --retries=3 \
      - CMD curl -f http://localhost/ || exit 1
    - Static website running in Ngnix, just test default URL

---------------------------

External Notes

---------------------------

Heart of the SwarmKit: Store, Topology & Object Model : https://www.youtube.com/watch?v=EmePhjGnCXY&ab_channel=Docker

How Raft works (Understandable Distributed Consensus
) : http://thesecretlivesofdata.com/raft/
  - So What is Distributed Consensus?
    - A distributed consensus ensures a consensus of data among nodes in a distributed system or reaches an agreement on a proposal.
  - Raft is a protocol for implementing distributed consensus.
    - A node can be in 1 of 3 states:
      - The Follower state
      - The Candidate state
      - The Leader state
    - All our nodes start in the follower state. If followers don't hear from a leader then they can become a candidate. If followers don't hear from a leader then they can become a candidate. The candidate then requests votes from other nodes. Nodes will reply with their vote. The candidate becomes the leader if it gets votes from a majority of nodes. This process is called Leader Election. All changes to the system now go through the leader. Each change is added as an entry in the node's log.
    - To commit the entry the node first replicates it to the follower nodes then the leader waits until a majority of nodes have written the entry. The entry is now committed on the leader node and the node state is "5". The leader then notifies the followers that the entry is committed. The cluster has now come to consensus about the system state. This process is called Log Replication.
    - Leader Election
      - In Raft there are two timeout settings which control elections.
        - First is the election timeout. The election timeout is the amount of time a follower waits until becoming a candidate. The election timeout is randomized to be between 150ms and 300ms. After the election timeout the follower becomes a candidate and starts a new election term votes for itself and sends out Request Vote messages to other nodes.
        - If the receiving node hasn't voted yet in this term then it votes for the candidate and the node resets its election timeout.
        - Once a candidate has a majority of votes it becomes leader.
      - The leader begins sending out Append Entries messages to its followers. These messages are sent in intervals specified by the heartbeat timeout.
        - Followers then respond to each Append Entries message.
        - This election term will continue until a follower stops receiving heartbeats and becomes a candidate.
        - Requiring a majority of votes guarantees that only one leader can be elected per term.
        - If two nodes become candidates at the same time then a split vote can occur.
    - Log Replication
      - Replication in computing involves sharing information so as to ensure consistency between redundant resources, such as software or hardware components, to improve reliability, fault-tolerance, or accessibility.
      - Once we have a leader elected we need to replicate all changes to our system to all nodes.
      - This is done by using the same Append Entries message that was used for heartbeats.
      - First a client sends a change to the leader.
      - The change is appended to the leader's log then the change is sent to the followers on the next heartbeat.
      - An entry is committed once a majority of followers acknowledge it and a response is sent to the client.
      - Raft can even stay consistent in the face of network partitions.

Docker Swarm Firewall Ports : https://www.bretfisher.com/docker-swarm-firewall-ports/
  - Docker Swarm Mode is a built-in solution with built-in key/value store.
  - Inbound Traffic for Swarm Management
    - TCP port 2377 for cluster management & raft sync communications
    - TCP and UDP port 7946 for "control plane" gossip discovery communication between all nodes
    - UDP port 4789 for "data plane" VXLAN overlay network traffic
    - IP Protocol 50 (ESP) if you plan on using overlay network with the encryption option
  - AWS Tip: You should use Security Groups in AWS's "source" field rather then subnets, so SG's will all dynamically update when new nodes are added.

How To Configure Custom Connection Options for your SSH Client : https://www.digitalocean.com/community/tutorials/how-to-configure-custom-connection-options-for-your-ssh-client
  - The Location of the SSH Client Config File : touch ~/.ssh/config
  - your config file should follow the simple rule of having the most specific configurations at the top. More general definitions should come later on in order to apply options that were not defined by the previous matching sections.

Microsoft Hyper-V : https://docs.docker.com/machine/drivers/hyper-v/


Deploy services to a swarm : https://docs.docker.com/engine/swarm/services/
  - Swarm services use a declarative model, which means that you define the desired state of the service, and rely upon Docker to maintain this state.
  - The state includes information such as (but not limited to):
    - the image name and tag the service containers should run
    - how many containers participate in the service
    - whether any ports are exposed to clients outside the swarm
    - whether the service should start automatically when Docker starts
    - the specific behavior that happens when the service is restarted (such as whether a rolling restart is used)
    - characteristics of the nodes where the service can run (such as resource constraints and placement preferences)
  - Create a service
    - To create a single-replica service with no extra configuration, you only need to supply the image name.
    ```bash
    docker service create nginx
    ```
    - The service is scheduled on an available node. To confirm that the service was created and started successfully, use the docker service ls command
    - Created services do not always run right away. A service can be in a pending state if its image is unavailable, if no node meets the requirements you configure for the service, or other reasons.
    - To provide a name for your service, use the --name flag:
  - gMSA for Swarm
    - Swarm now allows using a Docker Config as a gMSA credential spec - a requirement for Active Directory-authenticated applications. This reduces the burden of distributing credential specs to the nodes they’re used on.
    - To use a Config as a credential spec, first create the Docker Config containing the credential spec:
      ```bash
      docker config create credspec credspec.json
      ```
      - Now, you should have a Docker Config named credspec, and you can create a service using this credential spec. To do so, use the --credential-spec flag with the config name, like this:
      ```bash
      docker service create --credential-spec="config://credspec" <your image>
      ```
      - Your service will use the gMSA credential spec when it starts, but unlike a typical Docker Config (used by passing the --config flag), the credential spec will not be mounted into the container.

Use swarm mode routing mesh
  - Docker Engine swarm mode makes it easy to publish ports for services to make them available to resources outside the swarm. All nodes participate in an ingress routing mesh. The routing mesh enables each node in the swarm to accept connections on published ports for any service running in the swarm, even if there’s no task running on the node. The routing mesh routes all incoming requests to published ports on available nodes to an active container.
  - To use the ingress network in the swarm, you need to have the following ports open between the swarm nodes before you enable swarm mode:
    - Port 7946 TCP/UDP for container network discovery.
    - Port 4789 UDP for the container ingress network.
  - You must also open the published port between the swarm nodes and any external resources, such as an external load balancer, that require access to the port.
  - Publish a port for a service
    - Use the --publish flag to publish a port when you create a service. target is used to specify the port inside the container, and published is used to specify the port to bind on the routing mesh. If you leave off the published port, a random high-numbered port is bound for each service task. You need to inspect the task to determine the port.
    ```bash
    docker service create \
    --name <SERVICE-NAME> \
    --publish  published=<PUBLISHED-PORT>,target=<CONTAINER-PORT> \
    <IMAGE>
    ```
      - Note: The older form of this syntax is a colon-separated string, where the published port is first and the target port is second, such as -p 8080:80. The new syntax is preferred because it is easier to read and allows more flexibility.
      - The <PUBLISHED-PORT> is the port where the swarm makes the service available. If you omit it, a random high-numbered port is bound. The <CONTAINER-PORT> is the port where the container listens. This parameter is required.
    - For example, the following command publishes port 80 in the nginx container to port 8080 for any node in the swarm:
    ```bash
    docker service create \
    --name my-web \
    --publish published=8080,target=80 \
    --replicas 2 \
    nginx
    ```
      - When you access port 8080 on any node, Docker routes your request to an active container. On the swarm nodes themselves, port 8080 may not actually be bound, but the routing mesh knows how to route the traffic and prevents any port conflicts from happening.
  - docker service inspect to to view the service’s published port.
  - Bypass the routing mesh
    - You can bypass the routing mesh, so that when you access the bound port on a given node, you are always accessing the instance of the service running on that node. This is referred to as host mode. There are a few things to keep in mind.
      - If you access a node which is not running a service task, the service does not listen on that port. It is possible that nothing is listening, or that a completely different application is listening.
      - If you expect to run multiple service tasks on each node (such as when you have 5 nodes but run 10 replicas), you cannot specify a static target port. Either allow Docker to assign a random high-numbered port (by leaving off the published), or ensure that only a single instance of the service runs on a given node, by using a global service rather than a replicated one, or by using placement constraints.
    - To bypass the routing mesh, you must use the long --publish service and set mode to host. If you omit the mode key or set it to ingress, the routing mesh is used. The following command creates a global service using host mode and bypassing the routing mesh.

How services work : https://docs.docker.com/engine/swarm/how-swarm-mode-works/services/
  - To deploy an application image when Docker Engine is in swarm mode, you create a service. Frequently a service is the image for a microservice within the context of some larger application. Examples of services might include an HTTP server, a database, or any other type of executable program that you wish to run in a distributed environment.
  - When you create a service, you specify which container image to use and which commands to execute inside running containers.
  - You also define options for the service including:
    - the port where the swarm makes the service available outside the swarm
    - an overlay network for the service to connect to other services in the swarm
    - CPU and memory limits and reservations
    - a rolling update policy
    - the number of replicas of the image to run in the swarm
  - Services, tasks, and containers
    - When you deploy the service to the swarm, the swarm manager accepts your service definition as the desired state for the service. Then it schedules the service on nodes in the swarm as one or more replica tasks. The tasks run independently of each other on nodes in the swarm.
    - A container is an isolated process. In the swarm mode model, each task invokes exactly one container.
  - Tasks and scheduling
    - A task is the atomic unit of scheduling within a swarm.
    - When you declare a desired service state by creating or updating a service, the orchestrator realizes the desired state by scheduling tasks.
    - For instance, you define a service that instructs the orchestrator to keep three instances of an HTTP listener running at all times. The orchestrator responds by creating three tasks. Each task is a slot that the scheduler fills by spawning a container. The container is the instantiation of the task. If an HTTP listener task subsequently fails its health check or crashes, the orchestrator creates a new replica task that spawns a new container.
    - The underlying logic of Docker swarm mode is a general purpose scheduler and orchestrator. The service and task abstractions themselves are unaware of the containers they implement.
  - Pending services
    - A service may be configured in such a way that no node currently in the swarm can run its tasks. In this case, the service remains in state pending.
    - If your only intention is to prevent a service from being deployed, scale the service to 0 instead of trying to configure it in such a way that it remains in pending.
      - If all nodes are paused or drained, and you create a service, it is pending until a node becomes available. In reality, the first node to become available gets all of the tasks, so this is not a good thing to do in a production environment.
      - You can reserve a specific amount of memory for a service. If no node in the swarm has the required amount of memory, the service remains in a pending state until a node is available which can run its tasks. If you specify a very large value, such as 500 GB, the task stays pending forever, unless you really have a node which can satisfy it.
      - You can impose placement constraints on the service, and the constraints may not be able to be honored at a given time.
    - Replicated and global services
      - There are two types of service deployments, replicated and global.
        - For a replicated service, you specify the number of identical tasks you want to run. For example, you decide to deploy an HTTP service with three replicas, each serving the same content.
        - A global service is a service that runs one task on every node. There is no pre-specified number of tasks. Each time you add a node to the swarm, the orchestrator creates a task and the scheduler assigns the task to the new node. Good candidates for global services are monitoring agents, an anti-virus scanners or other types of containers that you want to run on every node in the swarm.

How nodes work : https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/
  - Docker Engine 1.12 introduces swarm mode that enables you to create a cluster of one or more Docker Engines called a swarm.
  - A swarm consists of one or more nodes: physical or virtual machines running Docker Engine 1.12 or later in swarm mode.
  - There are two types of nodes: managers and workers.
    - Manager nodes
      - Manager nodes handle cluster management tasks:
        - maintaining cluster state
        - scheduling services
        - serving swarm mode HTTP API endpoints
        - Using a Raft implementation, the managers maintain a consistent internal state of the entire swarm and all the services running on it. For testing purposes it is OK to run a swarm with a single manager. If the manager in a single-manager swarm fails, your services continue to run, but you need to create a new cluster to recover.
        - To take advantage of swarm mode’s fault-tolerance features, Docker recommends you implement an odd number of nodes according to your organization’s high-availability requirements. When you have multiple managers you can recover from the failure of a manager node without downtime.
          - A three-manager swarm tolerates a maximum loss of one manager.
          - A five-manager swarm tolerates a maximum simultaneous loss of two manager nodes.
          - An N manager cluster tolerates the loss of at most (N-1)/2 managers.
          - Docker recommends a maximum of seven manager nodes for a swarm.
        - Important Note: Adding more managers does NOT mean increased scalability or higher performance. In general, the opposite is true.
    - Worker nodes
      - Worker nodes are also instances of Docker Engine whose sole purpose is to execute containers. Worker nodes don’t participate in the Raft distributed state, make scheduling decisions, or serve the swarm mode HTTP API.
      - You can create a swarm of one manager node, but you cannot have a worker node without at least one manager node. By default, all managers are also workers. In a single manager node cluster, you can run commands like docker service create and the scheduler places all tasks on the local Engine.
      - To prevent the scheduler from placing tasks on a manager node in a multi-node swarm, set the availability for the manager node to Drain. The scheduler gracefully stops tasks on nodes in Drain mode and schedules the tasks on an Active node. The scheduler does not assign new tasks to nodes with Drain availability.
      - You can promote a worker node to be a manager by running docker node promote. For example, you may want to promote a worker node when you take a manager node offline for maintenance.

Manage sensitive data with Docker secrets : https://docs.docker.com/engine/swarm/secrets/
  - About secrets
    - In terms of Docker Swarm services, a secret is a blob of data, such as a password, SSH private key, SSL certificate, or another piece of data that should not be transmitted over a network or stored unencrypted in a Dockerfile or in your application’s source code.
    - You can use Docker secrets to centrally manage this data and securely transmit it to only those containers that need access to it.
    - Secrets are encrypted during transit and at rest in a Docker swarm. A given secret is only accessible to those services which have been granted explicit access to it, and only while those service tasks are running.
    - You can use secrets to manage any sensitive data which a container needs at runtime but you don’t want to store in the image or in source control, such as:
      - Usernames and passwords
      - TLS certificates and keys
      - SSH keys
      - Other important data such as the name of a database or internal server
      - Generic strings or binary content (up to 500 kb in size)
    - Docker secrets are only available to swarm services, not to standalone containers. To use this feature, consider adapting your container to run as a service. Stateful containers can typically run with a scale of 1 without changing the container code.
    - Another use case for using secrets is to provide a layer of abstraction between the container and a set of credentials.
    - You can also use secrets to manage non-sensitive data, such as configuration files. However, Docker supports the use of configs for storing non-sensitive data. Configs are mounted into the container’s filesystem directly, without the use of a RAM disk.
  - Windows Support:
    - Docker includes support for secrets on Windows containers. Where there are differences in the implementations.
  - How Docker manages secrets:
    - When you add a secret to the swarm, Docker sends the secret to the swarm manager over a mutual TLS connection. The secret is stored in the Raft log, which is encrypted. The entire Raft log is replicated across the other managers, ensuring the same high availability guarantees for secrets as for the rest of the swarm management data.
    - When you grant a newly-created or running service access to a secret, the decrypted secret is mounted into the container in an in-memory filesystem. The location of the mount point within the container defaults to /run/secrets/<secret_name> in Linux containers, or C:\ProgramData\Docker\secrets in Windows containers. But you can specify the location.
    - When a container task stops running, the decrypted secrets shared to it are unmounted from the in-memory filesystem for that container and flushed from the node’s memory.
    - If a node loses connectivity to the swarm while it is running a task container with access to a secret, the task container still has access to its secrets, but cannot receive updates until the node reconnects to the swarm.
    - You cannot remove a secret that a running service is using.
    - To update or roll back secrets more easily, consider adding a version number or date to the secret name.
  - Secret commands
    - docker secret create
    - docker secret inspect
    - docker secret ls
    - docker secret rm
    - --secret flag for docker service create
    - --secret-add and --secret-rm flags for docker service update
  - Defining and using secrets in compose files
    - Both the docker-compose and docker stack commands support defining secrets in a compose file.
    - Example : Get started with secrets :
      1. Add a secret to Docker. The docker secret create command reads standard input because the last argument, which represents the file to read the secret from, is set to -.
      ```bash
      printf "This is a secret" | docker secret create my_secret_data -
      ```
      2. Create a redis service and grant it access to the secret. By default, the container can access the secret at /run/secrets/<secret_name>, but you can customize the file name on the container using the target option.
      ```bash
      docker service  create --name redis --secret my_secret_data redis:alpine
      ```
      3. Verify that the task is running without issues using docker service ps.
      4. Get the ID of the redis service task container using docker ps , so that you can use docker container exec to connect to the container and read the contents of the secret data file, which defaults to being readable by all and has the same name as the name of the secret.
      5. Verify that the secret is not available if you commit the container.
      6. Try removing the secret. The removal fails because the redis service is running and has access to the secret.
      7. Remove access to the secret from the running redis service by updating the service.
      ```bash
      docker service update --secret-rm my_secret_data redis
      ```
      8. Repeat steps 3 and 4 again, verifying that the service no longer has access to the secret. The container ID is different, because the service update command redeploys the service.
      9. top and remove the service, and remove the secret from Docker.
      ```bash
      docker service rm redis
      docker secret rm my_secret_data
      ```

Share Compose configurations between files and projects : https://docs.docker.com/compose/extends/#multiple-compose-files
  - Compose supports two methods of sharing common configuration:
    1. Extending an entire Compose file by using multiple Compose files
    2. Extending individual services with the extends field (for Compose file versions up to 2.1)
  - Using multiple Compose files enables you to customize a Compose application for different environments or different workflows.
  - By default, Compose reads two files, a docker-compose.yml and an optional docker-compose.override.yml file. By convention, the docker-compose.yml contains your base configuration. The override file, as its name implies, can contain configuration overrides for existing services or entirely new services.
  - To use multiple override files, or an override file with a different name, you can use the -f option to specify the list of files. Compose merges files in the order they’re specified on the command line.
  - When you use multiple configuration files, you must make sure all paths in the files are relative to the base Compose file (the first Compose file specified with -f).
  - Tracking which fragment of a service is relative to which path is difficult and confusing, so to keep paths easier to understand, all paths must be defined relative to the base file.
  - Example
    - docker-compose.yml

    ```yml
    web:
      image: example/my_web_app:latest
      depends_on:
        - db
        - cache

    db:
      image: postgres:latest

    cache:
      image: redis:latest
    ```
    - docker-compose.override.yml

    ```yml
    web:
      build: .
      volumes:
        - '.:/code'
      ports:
        - 8883:80
      environment:
        DEBUG: 'true'

    db:
      command: '-d'
      ports:
        - 5432:5432

    cache:
      ports:
        - 6379:6379
    ```
    - When you run docker-compose up it reads the overrides automatically.
    - docker-compose.prod.yml

    ```yml
    web:
      ports:
        - 80:80
      environment:
        PRODUCTION: 'true'

    cache:
      environment:
        TTL: '500'
    ```
    - To deploy with this production Compose file you can run :
    ```bash  
    docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
    ```
  - ADMINISTRATIVE TASKS
    - Another common use case is running adhoc or administrative tasks against one or more services in a Compose app.
    - docker-compose :

    ```yml
    web:
      image: example/my_web_app:latest
      depends_on:
        - db

    db:
      image: postgres:latest
    ```
    - docker-compose.admin.yml :

    ```yml
    dbadmin:
      build: database_admin/
      depends_on:
        - db
    ```
    - To start a normal environment run docker-compose up -d. To run a database backup, include the docker-compose.admin.yml as well.
    ```bash
    docker-compose -f docker-compose.yml -f docker-compose.admin.yml \
 run dbadmin db-backup
    ```
  - Extending Services
    - The extends keyword is supported in earlier Compose file formats up to Compose file version 2.1
    - Docker Compose’s extends keyword enables the sharing of common configurations among different files, or even different projects entirely. Extending services is useful if you have several services that reuse a common set of configuration options. Using extends you can define a common set of service options in one place and refer to it from anywhere.
    - Keep in mind that volumes_from and depends_on are never shared between services using extends. These exceptions exist to avoid implicit dependencies; you always define volumes_from locally. This ensures dependencies between services are clearly visible when reading the current file. Defining these locally also ensures that changes to the referenced file don’t break anything.
    - Understand the extends configuration
    ```yml
    services:
      web:
        extends:
          file: common-services.yml
          service: webapp
    ```
    - Extending an individual service is useful when you have multiple services that have a common configuration.
    - Ex :
      - common.yml :
      ```yml
      services:
        app:
          build: .
          environment:
            CONFIG_FILE_PATH: /code/config
            API_KEY: xxxyyy
          cpu_shares: 5
      ```
      - docker-compose.yml :
      ```yml
      services:
        webapp:
          extends:
            file: common.yml
            service: app
          command: /code/run_web_app
          ports:
            - 8080:8080
          depends_on:
            - queue
            - db

        queue_worker:
          extends:
            file: common.yml
            service: app
          command: /code/run_worker
          depends_on:
            - queue
      ```
  - Adding and overridng configuration
    - Compose copies configurations from the original service over to the local one. If a configuration option is defined in both the original service and the local service, the local value replaces or extends the original value.
    - For the multi-value options ports, expose, external_links, dns, dns_search, and tmpfs, Compose concatenates both sets of values:
    - In the case of environment, labels, volumes, and devices, Compose “merges” entries together with locally-defined values taking precedence. For environment and labels, the environment variable or label name determines which value is used.
    - Entries for volumes and devices are merged using the mount path in the container:

Use Compose in production : https://docs.docker.com/compose/production/
  - When you define your app with Compose in development, you can use this definition to run your application in different environments such as CI, staging, and production.
  - The easiest way to deploy an application is to run it on a single server, similar to how you would run your development environment. If you want to scale up your application, you can run Compose apps on a Swarm cluster.
  - Modify your Compose file for production
    - You probably need to make changes to your app configuration to make it ready for production. These changes may include:
      -  Removing any volume bindings for application code, so that code stays inside the container and can’t be changed from outside
      - Binding to different ports on the host
      - Setting environment variables differently, such as reducing the verbosity of logging, or to specify settings for external services such as an email server
      - Specifying a restart policy like restart: always to avoid downtime
      - Adding extra services such as a log aggregator
    - For this reason, consider defining an additional Compose file, say production.yml, which specifies production-appropriate configuration. This configuration file only needs to include the changes you’d like to make from the original Compose file. The additional Compose file can be applied over the original docker-compose.yml to create a new configuration.
    ```bash
    docker-compose -f docker-compose.yml -f production.yml up -d
    ```
    - When you make changes to your app code, remember to rebuild your image and recreate your app’s containers. To redeploy a service called web, use:
    ```bash
    docker-compose build web
    docker-compose up --no-deps -d web
    ```
      - This first rebuilds the image for web and then stop, destroy, and recreate just the web service. The --no-deps flag prevents Compose from also recreating any services which web depends on.
    - You can use Compose to deploy an app to a remote Docker host by setting the DOCKER_HOST, DOCKER_TLS_VERIFY, and DOCKER_CERT_PATH environment variables appropriately.

docker service update : https://docs.docker.com/engine/reference/commandline/service_update/
  - Updates a service as described by the specified parameters. The parameters are the same as docker service create. Refer to the description there for further information.
  - Normally, updating a service will only cause the service’s tasks to be replaced with new ones if a change to the service requires recreating the tasks for it to take effect. For example, only changing the --update-parallelism setting will not recreate the tasks, because the individual tasks are not affected by this setting. However, the --force flag will cause the tasks to be recreated anyway. This can be used to perform a rolling restart without any changes to the service parameters.
  - Update a service:
  ```bash
  docker service update --limit-cpu 2 redis
  ```
  - Perform a rolling restart with no parameter changes :
  ```bash
  docker service update --force --update-parallelism 1 --update-delay 30s redis
  ```
    - In this example, the --force flag causes the service’s tasks to be shut down and replaced with new ones even though none of the other parameters would normally cause that to happen. The --update-parallelism 1 setting ensures that only one task is replaced at a time (this is the default behavior). The --update-delay 30s setting introduces a 30 second delay between tasks, so that the rolling restart happens gradually.
  - Add or remove mounts
    - Use the --mount-add or --mount-rm options add or remove a service’s bind mounts or volumes.

    ```bash
    $ docker service create \
        --name=myservice \
        --mount type=volume,source=test-data,target=/somewhere \
        nginx:alpine

    $ docker service update \
        --mount-add type=volume,source=other-volume,target=/somewhere-else \
        myservice

    $ docker service update --mount-rm /somewhere myservice

    ```
  - Add or remove published service ports
    - Use the --publish-add or --publish-rm flags to add or remove a published port for a service. You can use the short or long syntax.
    - Use the --network-add or --network-rm flags to add or remove a network for a service. You can use the short or long syntax.

    ```bash
    $ docker service update \
      --publish-add published=8080,target=80 \

    $ docker service update \
      --network-rm my-network \
      --network-add name=my-network,alias=web1 \

    ```
  - Roll back to the previous version of a service
    - Use the --rollback option to roll back to the previous version of the service.
    - This will revert the service to the configuration that was in place before the most recent docker service update command.

    ```bash
    $ docker service update --replicas=5 web

    $ docker service update --rollback web

    $ docker service update \
      --rollback \
      --update-delay 0s
      web
    ```
    - Services can also be set up to roll back to the previous version automatically when an update fails. To set up a service for automatic rollback, use --update-failure-action=rollback. A rollback will be triggered if the fraction of the tasks which failed to update successfully exceeds the value given with --update-max-failure-ratio.
    - The rate, parallelism, and other parameters of a rollback operation are determined by the values passed with the following flags:
      - --rollback-delay
      - --rollback-failure-action
      - --rollback-max-failure-ratio
      - --rollback-monitor
      - --rollback-parallelism
  - Add or remove secrets
    - Use the --secret-add or --secret-rm options add or remove a service’s secrets.
    ```bash
    $ docker service update \
        --secret-add source=ssh-2,target=ssh-2 \
        --secret-rm ssh-1 \
    ```
  - Updating Jobs
    - When a service is created as a job, by setting its mode to replicated-job or to global-job when doing service create, options for updating it are limited.
    - Updating a Job immediately stops any Tasks that are in progress. The operation creates a new set of Tasks for the job and effectively resets its completion status. If any Tasks were running before the update, they are stopped, and new Tasks are created.
    - Jobs cannot be rolled out or rolled back. None of the flags for configuring update or rollback settings are valid with job modes.
    - To run a job again with the same parameters that it was run previously, it can be force updated with the --force flag.
