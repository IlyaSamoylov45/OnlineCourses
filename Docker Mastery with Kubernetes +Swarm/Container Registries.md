Container Registries
  - An image registry needs to be part of your container plan.
  - More Docker Hub details including auto-build.
  - How Docker Store (store.docker.com) is different then Hub
  - How Docker Cloud (cloud.docker.com) is different then Hub
  - Use new Swarms feature in Cloud to connect Mac/Win to swarm.
  - Install and use Docker Registry as private image store.
  - 3rd party registry options.

Docker Hub : Digging Deeper
  - The most popular public image registry.
  - It's really Docker Registry plus lightweight image building.
  - Let's explore more of the features of Docker Hub.
  - Link GitHub/BitBucket to Hub and auto-build images on commit.
  - Chain image building together.

Running Docker Registry
  - A private image registry for your network.
  - Part of the docker/distribution Github repo.
  - The de facto in private container registries.
  - Not as full featured as Hub or others, no web UI, basic auth only.
  - At its core : a web API and storage system, written in Go.
  - Storage supports local, S3/Azure/Alibaba/Google Cloud, and OpenStack Swift.
  - Secure your registry with TLS
  - Storage cleanup via Garbage Collection.
  - Enable Hub caching via "--registry-mirror"

Registry and Proper TLS
  - "Secure by Default" : Docker won't talk to registry without HTTPS
    - Except localhost (127.0.0.0/8)
    - Port 5000
  - For remote self-signed TLS, enable "insecure-registry" in engine.

Private Docker Registry
  - Run the registry image
  ```bash
 docker container run -d -p 5000:5000 --name registry registry
 ```
  - Re-tag an existing image and push it to your new registry
  ```bash
  docker tag hello-world 127.0.0.1:5000/hello-world
  ```
  ```bash
  docker push 127.0.0.1:5000/hello-world
  ```
  - Remove that image from local cache and pull it from new registry
  ```bash
  docker image remove hello-world
  ```
  ```bash
  docker image remove 127.0.0.1:5000/hello-world
  ```
  ```bash
  docker pull 127.0.0.1:5000/hello-world
  ```
  - Re-create registry using a bind mount and see how it stores data
  ```bash
  docker container run -d -p 5000:5000 --name registry -v $(pwd)/
  registry-data:/var/lib/registry registry
  ```

Private Docker Registry with Swarm
  - Works the same way as localhost.
  - Because of routing Mesh all nodes can see 127.0.0.1:5000
  - Remember to decide how to store images (volume driver)
  - NOTE : ALL NODES MUST BE ABLE TO ACCESS THE IMAGES.
  - ProTip: Use a hosted SaaS registry if possible.

---------------------------

External Notes

---------------------------

Configuring a registry : https://docs.docker.com/registry/configuration/
  - Override specific configuration options
    - In a typical setup where you run your Registry from the official image, you can specify a configuration variable from the environment by passing -e arguments to your docker run stanza or from within a Dockerfile using the ENV instruction.
    - To override a configuration option, create an environment variable named REGISTRY_variable where variable is the name of the configuration option and the _ (underscore) represents indention levels. For example, you can configure the rootdirectory of the filesystem storage backend:
    ```yaml
    storage:
      filesystem:
        rootdirectory: /var/lib/registry
    ```
    - To override this value, set an environment variable like this:
      - REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY=/somewhere
  - Overriding the entire configuration file
    - If the default configuration is not a sound basis for your usage, or if you are having issues overriding keys from the environment, you can specify an alternate YAML configuration file by mounting it as a volume in the container.
  - version
    - The version option is required. It specifies the configuration’s version. It is expected to remain a top-level field, to allow for a consistent version check before parsing the remainder of the configuration file
  - log
    - The log subsection configures the behavior of the logging system. The logging system outputs everything to stdout. You can adjust the granularity and format with this configuration section.
  - accesslog
    - Within log, accesslog configures the behavior of the access logging system. By default, the access logging system outputs to stdout in Combined Log Format. Access logging can be disabled by setting the boolean flag disabled to true.
  - hooks
    - The hooks subsection configures the logging hooks’ behavior. This subsection includes a sequence handler which you can use for sending mail, for example.
  - storage
    - The storage option is required and defines which storage backend is in use. You must configure exactly one backend. If you configure more, the registry returns an error.
    - For testing only, you can use the inmemory storage driver. If you would like to run a registry from volatile memory, use the filesystem driver on a ramdisk.
    - If you are deploying a registry on Windows, a Windows volume mounted from the host is not recommended. Instead, you can use a S3 or Azure backing data-store. If you do use a Windows volume, the length of the PATH to the mount point must be within the MAX_PATH limits (typically 255 characters), or this error will occur:
  - maintenance
    - Currently, upload purging and read-only mode are the only maintenance functions available.
    - uploadpurging
      - Upload purging is a background process that periodically removes orphaned files from the upload directories of the registry. Upload purging is enabled by default.
    - readonly
      - If the readonly section under maintenance has enabled set to true, clients will not be allowed to write to the registry. This mode is useful to temporarily prevent writes to the backend storage so a garbage collection pass can be run.
  - delete
    - Use the delete structure to enable the deletion of image blobs and manifests by digest. It defaults to false, but it can be enabled by writing the following on the configuration file:
  - cache
    - Use the cache structure to enable caching of data accessed in the storage backend. Currently, the only available cache provides fast access to layer metadata, which uses the blobdescriptor field if configured.
  - redirect
    - The redirect subsection provides configuration for managing redirects from content backends.
    -  In certain deployment scenarios, you may decide to route all data through the Registry, rather than redirecting to the backend.
  - auth
    - The auth option is optional.
    - silly
      - The silly authentication provider is only appropriate for development. It simply checks for the existence of the Authorization header in the HTTP request. It does not check the header’s value. If the header does not exist, the silly auth responds with a challenge response, echoing back the realm, service, and scope for which access was denied.
    - token
      - Token-based authentication allows you to decouple the authentication system from the registry. It is an established authentication paradigm with a high degree of security.
    - htpasswd
      - The htpasswd authentication backed allows you to configure basic authentication using an Apache htpasswd file. The only supported password format is bcrypt. Entries with other hash types are ignored. The htpasswd file is loaded once, at startup. If the file is invalid, the registry will display an error and will not start.
  - middleware
    - The middleware structure is optional. Use this option to inject middleware at named hook points.
    - Each middleware must implement the same interface as the object it is wrapping. For instance, a registry middleware must implement the distribution.Namespace interface, while a repository middleware must implement distribution.Repository, and a storage middleware must implement driver.StorageDriver.
  - redirect
    - You can use the redirect storage middleware to specify a custom URL to a location of a proxy for the layer stored by the S3 storage driver.
  - reporting
    - The reporting option is optional and configures error and metrics reporting tools. At the moment only two services are supported:
  - http
    - The http option details the configuration for the HTTP server that hosts the registry.
    - tls
      - The tls structure within http is optional. Use this to configure TLS for the server. If you already have a web server running on the same host as the registry, you may prefer to configure TLS on that web server and proxy connections to the registry server.
    - letsencrypt
      - The letsencrypt structure within tls is optional. Use this to configure TLS certificates provided by Let’s Encrypt.
    - debug
      - The debug option is optional . Use it to configure a debug server that can be helpful in diagnosing problems. The debug endpoint can be used for monitoring registry metrics and health, as well as profiling. Sensitive information may be available via the debug endpoint. Please be certain that access to the debug endpoint is locked down in a production environment.
    - prometheus
      - The prometheus option defines whether the prometheus metrics is enable, as well as the path to access the metrics.
    - headers
      - The headers option is optional . Use it to specify headers that the HTTP server should include in responses. This can be used for security headers such as Strict-Transport-Security.
    - http2
      - The http2 structure within http is optional. Use this to control http2 settings for the registry.
  - notifications
    - The notifications option is optional and currently may contain a single option, endpoints.
    - endpoints
      - The endpoints structure contains a list of named services (URLs) that can accept event notifications.
  - redis
    - Declare parameters for constructing the redis connections. Registry instances may use the Redis instance for several applications. Currently, it caches information about immutable blobs. Most of the redis options control how the registry connects to the redis instance. You can control the pool’s behavior with the pool subsection.
    - pool
      - Use these settings to configure the behavior of the Redis connection pool.
  - health
    - The health option is optional, and contains preferences for a periodic health check on the storage driver’s backend storage, as well as optional periodic checks on local files, HTTP URIs, and/or TCP servers. The results of the health checks are available at the /debug/health endpoint on the debug HTTP server if the debug HTTP server is enabled (see http section).
    - storagedriver
      - The storagedriver structure contains options for a health check on the configured storage driver’s backend storage. The health check is only active when enabled is set to true.
    - file
      - The file structure includes a list of paths to be periodically checked for the\ existence of a file. If a file exists at the given path, the health check will fail. You can use this mechanism to bring a registry out of rotation by creating a file.
    - http
      - The http structure includes a list of HTTP URIs to periodically check with HEAD requests. If a HEAD request does not complete or returns an unexpected status code, the health check will fail.
    - tcp
      - The tcp structure includes a list of TCP addresses to periodically check using TCP connection attempts. Addresses must include port numbers. If a connection attempt fails, the health check will fail.
  - proxy
    - The proxy structure allows a registry to be configured as a pull-through cache to Docker Hub.
    - Note: These private repositories are stored in the proxy cache’s storage. Take appropriate measures to protect access to the proxy cache.
  - compatibility
    - Use the compatibility structure to configure handling of older and deprecated features. Each subsection defines such a feature with configurable behavior.
  - validation
    - disabled
      - The disabled flag disables the other options in the validation section. They are enabled by default. This option deprecates the enabled flag.
    - manifests
      - Use the manifests subsection to configure validation of manifests. If disabled is false, the validation allows nothing.
    - URLS
      - The allow and deny options are each a list of regular expressions that restrict the URLs in pushed manifests.
      - If allow is unset, pushing a manifest containing URLs fails.
      - If allow is set, pushing a manifest succeeds only if all URLs match one of the allow regular expressions and one of the following holds:
        - deny is unset.
        - deny is set but no URLs within the manifest match any of the deny regular expressions.

Registry as a pull through cache : https://docs.docker.com/registry/recipes/mirror/
  - If you have multiple instances of Docker running in your environment, such as multiple physical or virtual machines all running Docker, each daemon goes out to the internet and fetches an image it doesn’t have locally, from the Docker repository. You can run a local registry mirror and point all your daemons there, to avoid this extra internet traffic.
  - Alternatively, if the set of images you are using is well delimited, you can simply pull them manually and push them to a simple, local, private registry.
  - It’s currently not possible to mirror another private registry. Only the central Hub can be mirrored.
  - The Registry can be configured as a pull through cache. In this mode a Registry responds to all normal docker pull requests but stores all content locally.
  - The first time you request an image from your local registry mirror, it pulls the image from the public Docker registry and stores it locally before handing it back to you. On subsequent requests, the local registry mirror is able to serve the image from its own storage.
  - When a pull is attempted with a tag, the Registry checks the remote to ensure if it has the latest version of the requested content. Otherwise, it fetches and caches the latest content.
  - In environments with high churn rates, stale data can build up in the cache. When running as a pull through cache the Registry periodically removes old content to save disk space. Subsequent requests for removed content causes a remote fetch and local re-caching. To ensure best performance and guarantee correctness the Registry cache should be configured to use the filesystem driver for storage.
  - The easiest way to run a registry as a pull through cache is to run the official Registry image. At least, you need to specify proxy.remoteurl within /etc/docker/registry/config.yml.
  - Multiple registry caches can be deployed over the same back-end. A single registry cache ensures that concurrent requests do not pull duplicate data, but this property does not hold true for a registry cache cluster.
  - To configure a Registry to run as a pull through cache, the addition of a proxy section is required to the config file.
  - To access private images on the Docker Hub, a username and password can be supplied.
  ```yml
  proxy:
    remoteurl: https://registry-1.docker.io
    username: [username]
    password: [password]
  ```
    - Warning: If you specify a username and password, it’s very important to understand that private resources that this user has access to Docker Hub is made available on your mirror. You must secure your mirror by implementing authentication if you expect these resources to stay private!
    - Warning: For the scheduler to clean up old entries, delete must be enabled in the registry configuration.
  - Configure the Docker daemon
    - Either pass the --registry-mirror option when starting dockerd manually, or edit /etc/docker/daemon.json and add the registry-mirrors key and value, to make the change persistent.
    ```yml
    {
      "registry-mirrors": ["https://<my-docker-mirror-host>"]
    }
    ```

Garbage collection : https://docs.docker.com/registry/garbage-collection/
  - About garbage collection
    - In the context of the Docker registry, garbage collection is the process of removing blobs from the filesystem when they are no longer referenced by a manifest. Blobs can include both layers and manifests.
    - Registry data can occupy considerable amounts of disk space. In addition, garbage collection can be a security consideration, when it is desirable to ensure that certain layers no longer exist on the filesystem.
  - Garbage collection in practice
    - Filesystem layers are stored by their content address in the Registry. This has many advantages, one of which is that data is stored once and referred to by manifests.
    - Layers are therefore shared amongst manifests; each manifest maintains a reference to the layer. As long as a layer is referenced by one manifest, it cannot be garbage collected.
    - Manifests and layers can be deleted with the registry API. This API removes references to the target and makes them eligible for garbage collection. It also makes them unable to be read via the API.
    - If a layer is deleted, it is removed from the filesystem when garbage collection is run. If a manifest is deleted the layers to which it refers are removed from the filesystem if no other manifests refers to them.
    - Garbage collection runs in two phases. First, in the ‘mark’ phase, the process scans all the manifests in the registry. From these manifests, it constructs a set of content address digests. This set is the ‘mark set’ and denotes the set of blobs to not delete. Secondly, in the ‘sweep’ phase, the process scans all the blobs and if a blob’s content address digest is not in the mark set, the process deletes it.
    - Note: You should ensure that the registry is in read-only mode or not running at all. If you were to upload an image while garbage collection is running, there is the risk that the image’s layers are mistakenly deleted leading to a corrupted image.
    - This type of garbage collection is known as stop-the-world garbage collection.
  - Run garbage collection
    - bin/registry garbage-collect [--dry-run] /path/to/config.yml
      - The garbage-collect command accepts a --dry-run parameter, which prints the progress of the mark and sweep phases without removing any data. Running with a log level of info gives a clear indication of items eligible for deletion.
    - config.yml file :
    ```yml
    version: 0.1
      storage:
        filesystem:
          rootdirectory: /registry/data
    ```
