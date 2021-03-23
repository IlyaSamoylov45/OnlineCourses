Docker Compose
  - Why : configure relationships between containers
  - Why : save our run settings in easy-to-read file.
  - Why : create one-liner developer environment startups
  - Comprised of 2 separate but related things.
    1. YAML-formatted file that describes our solution options for:
      - containers
      - networks
      - volumes
    2. A CLI tool docker-compose used for local dev/test automation with those YAML files
  - docker-compose.yml
    - Compose YAML format has it's own versions 1, 2, 2.1, 3, 3.1
    - YAML file can be used with docker-compose command for local docker automation or..
    - With docker directly in production with Swarm
    - docker-compose --help
    - docker-compse.yml is default filename, but any can be used with docker-compose -f
  - Basic Docker compose file syntax :
  ```YAML
  version: '3.1'  # if no version is specified then v1 is assumed. Recommend v2 minimum

  services:  # containers. same as docker run
    servicename: # a friendly name. this is also DNS name inside network
      image: # Optional if you use build:
      command: # Optional, replace the default CMD specified by the image
      environment: # Optional, same as -e in docker run
      volumes: # Optional, same as -v in docker run
    servicename2:

  volumes: # Optional, same as docker volume create

  networks: # Optional, same as docker network create
  ```

  - Basic Docker compose file example :
  ```YAML
  version: '3'

  services:
    ghost:
      image: ghost
      ports:
        - "80:2368"
      environment:
        - URL=http://localhost
        - NODE_ENV=production
        - MYSQL_HOST=mysql-primary
        - MYSQL_PASSWORD=mypass
        - MYSQL_DATABASE=ghost
      volumes:
        - ./config.js:/var/lib/ghost/config.js
      depends_on:
        - mysql-primary
        - mysql-secondary
    proxysql:
      image: percona/proxysql
      environment:
        - CLUSTER_NAME=mycluster
        - CLUSTER_JOIN=mysql-primary,mysql-secondary
        - MYSQL_ROOT_PASSWORD=mypass

        - MYSQL_PROXY_USER=proxyuser
        - MYSQL_PROXY_PASSWORD=s3cret
    mysql-primary:
      image: percona/percona-xtradb-cluster:5.7
      environment:
        - CLUSTER_NAME=mycluster
        - MYSQL_ROOT_PASSWORD=mypass
        - MYSQL_DATABASE=ghost
        - MYSQL_PROXY_USER=proxyuser
        - MYSQL_PROXY_PASSWORD=s3cret
    mysql-secondary:
      image: percona/percona-xtradb-cluster:5.7
      environment:
        - CLUSTER_NAME=mycluster
        - MYSQL_ROOT_PASSWORD=mypass

        - CLUSTER_JOIN=mysql-primary
        - MYSQL_PROXY_USER=proxyuser
        - MYSQL_PROXY_PASSWORD=s3cret
      depends_on:
        - mysql-primary
  ```

Docker Compose CLI
  - docker-compose cli is separate tool.
  - CLI tool comes with Docker for Windows/Mac, but separate download for Linux
  - Not a good production-grade tool, but ideal for local development and test.
  - Two most common commands are :
    - docker-compose up #setup volumes/networks and start all containers
    - docker-compose down # stop all containers and remove cont/vol/net
  - If all your projects had a Dockerfile and docker-compose.yml then "new developer onboarding" would be:
    - git clone github.com/some/software
    - docker-compose up
Using Compose to Build
  - Compose can also build your custom images
  - Will build then with docker-compose up if not found in cache
  - Also rebuild with docker-compose build
  - Great for complex builds that have lots of vars or build args

---------------------------

External Notes

---------------------------

YAML Syntax : https://yaml.org/refcard.html

Compose file versions and upgrading : https://docs.docker.com/compose/compose-file/compose-versioning/
  - There are several versions of the Compose file format – 1, 2, 2.x, and 3.x
  - There are three legacy versions of the Compose file format:
    - Version 1. This is specified by omitting a version key at the root of the YAML.
    - Version 2.x. This is specified with a version: '2' or version: '2.1', etc., entry at the root of the YAML.
    - Version 3.x, designed to be cross-compatible between Compose and the Docker Engine’s swarm mode. This is specified with a version: '3' or version: '3.1', etc., entry at the root of the YAML.
  - Note: When specifying the Compose file version to use, make sure to specify both the major and minor numbers. If no minor version is given, 0 is used by default and not the latest minor version.
  - Note: If you’re using multiple Compose files or extending services, each file must be of the same version - you cannot, for example, mix version 1 and 2 in a single project.
  - We recommend against using --compatibility mode in production. Because the resulting configuration is only an approximate using non-Swarm mode properties, it may produce unexpected results.

Compose file : https://docs.docker.com/compose/compose-file/#links
