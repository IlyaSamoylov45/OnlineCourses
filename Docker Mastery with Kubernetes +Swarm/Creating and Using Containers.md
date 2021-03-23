Docker commands
  - docker version -> verified cli can talk to engine
  - docker info -> most config values of engine

Image vs Container
  - An image is the application we want to run
  - Container is a running instance of the image.
  - docker container run <image name>
  - docker container run --publish 80:80 nginx
  - docker container stop <container id / name>
  - docker ps (old way)
  - docker container ls (new way)
  - docker container logs <name / container id>
  - docker container rm 63f 690 0de
  - docker container rm -f 63f
  - docker container ls -a (Show all containers)
  - docker start <container_id / name>
  - docker container stats => show live performance data for all containers.
  - docker container top <name> => process list in one container
  - docker container inspect <name> => details of on container config
  - docker container run -it => start new container interactively
  - docker container exec -it => run additional command in existing container.
  - docker container run -it --name proxy nginx bash
  - docker container exec -it <container name / id> bash
  - exec runs an additional process on an existing docker container
  - docker image ls
  - docker container run -it alpine bash
    - Not in the container... bash is not on alpine but has sh
  - flag -p allows you to specify ports
  - docker container port <container> => quick port check

Docker Network Defaults :
  - Each container connected to a private virtual network "bridge"
  - Each virtual network routes through NAT firewall on host IP
  - All containers on a virtual network can talk to each other without -p
  - Best practice is to create a new virtual network for each app:
    - network "my_web_app" for mysql and php/apache containers
    - network "my_api" for mongo and nodejs containers
  - Defaults work well in many cases, but easy to swap out parts to customize it
  - Can make new Virtual Networks
  - Attach containers to more than one virtual network (or none)
  -  Skip virtual networks and use host IP (--net=host)
  - Use different Docker network drivers to gain new abilities
  - docker container port <name / id>
  - docker container inspect --format '{{ .NetworkSettings.IPAddress }}' <name / id>

Docker Networks : CLI Management
  - docker network ls => show networks
  - docker network inspect => Inspect a network
  - docker network create --driver => Create a network  (Spawns a new virtual network for you to attach containers to.)
  - docker network connect => Attach a  network to a container (Dynamically creates a NIC in a container on an existing virtual network)
  - docker network disconnect => Detach a network from container
  - --network bridge => default docker virtual network which is NAT'ed behind the Host IP.
  - docker network create --help
  - Docker DNS : Docker daemon has a built-in DNS server that containers use by default.
  - docker defaults the hostname to containers name, but you may set aliases
  - docker container exec -it my_nginx ping new_nginx
  - docker container --link => can be used to manually link containers.
  - Containers shouldn't rely on IP's for inter-communication
  - DNS for friendly names is built-in for you to use custom networks.
  - The default bridge network driver allow containers to communicate with each other when running on the same docker host.
  - By using the 'docker exec -it <container> sh' (or bash) command on a container, we can connect to a shell from inside it.
  - To start a container in detached mode, you use -d=true or just -d option. By design, containers started in detached mode exit when the root process used to run the container exits, unless you also specify the --rm option. If you use -d with --rm, the container is removed when it exits or when the daemon exits, whichever happens first.
  - docker container run -d --net dude --net-alias search elasticsearch:2


---------------------------

External Notes

---------------------------



Additional information :
  - With:
    - cgroup: Control Groups provide a mechanism for aggregating/partitioning sets of tasks, and all their future children, into hierarchical groups with specialized behaviour.
    - namespace: wraps a global system resource in an abstraction that makes it appear to the processes within the namespace that they have their own isolated instance of the global resource.
  - In short:
    - Cgroups = limits how much you can use;
    - namespaces = limits what you can see (and therefore use)
  - Cgroups determines how much host machine resources to be given to containers.
  - Namespaces: provides process isolation, complete isolation of containers, separate file system.
