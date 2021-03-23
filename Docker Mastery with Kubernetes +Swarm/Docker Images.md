Image
  - An Image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime.

Image Layers
  - sha helps system identify layers, each layer has a sha.
  - The starting layer is called scratch.
  - Each layer is uniquely identified and only stored once on a host.

Image Tags
  - Can have multiple tags for image id.
  - docker image tag nginx isamo45/nginx
  - latest is default tag.
  - docker image push isamo45/nginx => uploads changed layers to an image registry
  - docker login
  - docker logout
  - ALWAYS LOGOUT FROM SHARED MACHINES OR SERVERS WHEN DONE, TO PROTECT YOUR ACCOUNT
  - docker image tag nginx isamo45/nginx
  - Can make a new repo based on tag.
  - Can add additional tags
  - docker image tag isamo45/nginx isamo45/nginx:testing
  - docker image push isamo45/nginx:testing
  - Won't bother reuploading duplicate layers.
  - Latest tag is the latest stable version usually.

DockerFile
  - Recipe for creating your image.
  - FROM
    - all images must have a FROM
    - usually from a minimal Linux distribution like debian or (even better) alpine
    - if you truly want to start with an empty container, use FROM scratch
    - We inherit everything from this dockerfile.
  - ENV
    - Way to set environment values
    - optional environment variable that's used in later lines and set as envvar when container is running
  - RUN
    - optional commands to run at shell inside container at build time
  - EXPOSE
    - expose these ports on the docker virtual network
    - you still need to use -p or -P to open/forward these ports on host
  - CMD
    - required: run this command when container is launched
    - only one CMD allowed, so if there are multiple, last one wins
  - WORKDIR
    - Change working directory to root of nginx webhost
    - using WORKDIR is prefered to using 'RUN cd /some/path'
  - COPY
    - copy source code to directory
  - Each one of commands creates a new layer.
  - && lets you have multiple commands in one layer.
  - docker image build -t customnginx .
    - . means build this dockerfile in this directory.
  - lines in dockerfile that hasn't changed does not get rebuilt. So you will have less build time.
  - Ordering lines in dockerfile is important so that you don't have to rebuild multiple layers.
  - Things that change the most should be at the bottom of your dockerfile.
  - docker build -f some-dockerfile
  - Dockerfiles are part process workflow and part art.

Prune
  - You can use "prune" commands to clean up images, volumes, build cache, and containers. Examples include:
  - docker image prune to clean up just "dangling" images
  - docker system prune will clean up everything
  - The big one is usually docker image prune -a which will remove all images you're not using. Use docker system df to see space usage.
  - docker system df

---------------------------

External Notes

---------------------------

DockerFile Reference Doc : https://docs.docker.com/engine/reference/builder/
  - A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession.
  - The docker build command builds an image from a Dockerfile and a context.
    - The build’s context is the set of files at a specified location PATH or URL. The PATH is a directory on your local filesystem. The URL is a Git repository location.
    - The first thing a build process does is send the entire context (recursively) to the daemon. In most cases, it’s best to start with an empty directory as context and keep your Dockerfile in that directory. Add only the files needed for building the Dockerfile.
    - To increase the build’s performance, exclude files and directories by adding a .dockerignore file to the context directory.
  - Traditionally, the Dockerfile is called Dockerfile and located in the root of the context. You use the -f flag with docker build to point to a Dockerfile anywhere in your file system.
  - The Docker daemon runs the instructions in the Dockerfile one-by-one, committing the result of each instruction to a new image if necessary, before finally outputting the ID of your new image. The Docker daemon will automatically clean up the context you sent.
  - Whenever possible, Docker will re-use the intermediate images (cache), to accelerate the docker build process significantly.
  - Note that each instruction is run independently, and causes a new image to be created - so RUN cd /tmp will not have any effect on the next instructions.
  - If you wish to use build cache of a specific image you can specify it with --cache-from option. Images specified with --cache-from do not need to have a parent chain and may be pulled from other registries.
  - The BuildKit backend provides many benefits compared to the old implementation. For example, BuildKit can:
    - Detect and skip executing unused build stages
    - Parallelize building independent build stages
    - Incrementally transfer only the changed files in your build context between builds
    - Detect and skip transferring unused files in your build context
    - Use external Dockerfile implementations with many new features
    - Avoid side-effects with rest of the API (intermediate images and containers)
    - Prioritize your build cache for automatic pruning
  - Docker runs instructions in a Dockerfile in order. A Dockerfile must begin with a FROM instruction. This may be after parser directives, comments, and globally scoped ARGs. The FROM instruction specifies the Parent Image from which you are building. FROM may only be preceded by one or more ARG instructions, which declare arguments that are used in FROM lines in the Dockerfile.
  - Docker treats lines that begin with # as a comment, unless the line is a valid parser directive. A # marker anywhere else in a line is treated as an argument. Line continuation characters are not supported in comments.
  - Parser Directives:
    - Parser directives are optional, and affect the way in which subsequent lines in a Dockerfile are handled. Parser directives do not add layers to the build, and will not be shown as a build step. Parser directives are written as a special type of comment in the form # directive=value. A single directive may only be used once.
    - Once a comment, empty line or builder instruction has been processed, Docker no longer looks for parser directives. Instead it treats anything formatted as a parser directive as a comment and does not attempt to validate if it might be a parser directive. Therefore, all parser directives must be at the very top of a Dockerfile.
    - Custom Dockerfile implementation allows you to:
      - Automatically get bugfixes without updating the daemon
      - Make sure all users are using the same implementation to build your Dockerfile
      - Use the latest features without updating the daemon
      - Try out new experimental or third-party features
    - Stable channel follows semantic versioning. For example:
      - docker/dockerfile:1.0.0 - only allow immutable version 1.0.0
      - docker/dockerfile:1.0 - allow versions 1.0.*
      - docker/dockerfile:1 - allow versions 1.*.*
      - docker/dockerfile:latest - latest release on stable channel
    - The experimental channel uses incremental versioning with the major and minor component from the stable channel on the time of the release. For example:
      - docker/dockerfile:1.0.1-experimental - only allow immutable version 1.0.1-experimental
      - docker/dockerfile:1.0-experimental - latest experimental releases after 1.0
      - docker/dockerfile:experimental - latest release on experimental channel  
    - You should choose a channel that best fits your needs. If you only want bugfixes, you should use docker/dockerfile:1.0. If you want to benefit from experimental features, you should use the experimental channel. If you are using the experimental channel, newer releases may not be backwards compatible, so it is recommended to use an immutable full version variant.
    - The escape directive sets the character used to escape characters in a Dockerfile. If not specified, the default escape character is \.
    - Setting the escape character to \` is especially useful on Windows, where \ is the directory path separator. \` is consistent with Windows PowerShell.
  - Environment replacement :
    - The ${variable_name} syntax also supports a few of the standard bash modifiers as specified below:
      - ${variable:-word} indicates that if variable is set then the result will be that value. If variable is not set then word will be the result.
      - ${variable:+word} indicates that if variable is set then word will be the result, otherwise the result is the empty string.
    - Environment variables are supported by the following list of instructions in the Dockerfile:
      - ADD
      - COPY
      - ENV
      - EXPOSE
      - FROM
      - LABEL
      - STOPSIGNAL
      - USER
      - VOLUME
      - WORKDIR
      - ONBUILD
    - Environment variable substitution will use the same value for each variable throughout the entire instruction. In other words, in this example:
    ```dockerfile
    ENV abc=hello
    ENV abc=bye def=$abc
    ENV ghi=$abc
    ```
      - will result in def having a value of hello, not bye. However, ghi will have a value of bye because it is not part of the same instruction that set abc to bye.
  - .dockerignore file
    - Before the docker CLI sends the context to the docker daemon, it looks for a file named .dockerignore in the root directory of the context. If this file exists, the CLI modifies the context to exclude files and directories that match patterns in it. This helps to avoid unnecessarily sending large or sensitive files and directories to the daemon and potentially adding them to images using ADD or COPY.
    - The CLI interprets the .dockerignore file as a newline-separated list of patterns similar to the file globs of Unix shells. For the purposes of matching, the root of the context is considered to be both the working and the root directory. For example, the patterns /foo/bar and foo/bar both exclude a file or directory named bar in the foo subdirectory of PATH or in the root of the git repository located at URL. Neither excludes anything else.
    - Lines starting with ! (exclamation mark) can be used to make exceptions to exclusions.
    ```dockerfile
    *.md
    !README*.md
    README-secret.md
    ```
      - No markdown files are included in the context except README files other than README-secret.md.
  - FROM
    - The FROM instruction initializes a new build stage and sets the Base Image for subsequent instructions. As such, a valid Dockerfile must start with a FROM instruction. The image can be any valid image – it is especially easy to start by pulling an image from the Public Repositories.

      - ARG is the only instruction that may precede FROM in the Dockerfile. See Understand how ARG and FROM interact.
      - FROM can appear multiple times within a single Dockerfile to create multiple images or use one build stage as a dependency for another. Simply make a note of the last image ID output by the commit before each new FROM instruction. Each FROM instruction clears any state created by previous instructions.
      - Optionally a name can be given to a new build stage by adding AS name to the FROM instruction. The name can be used in subsequent FROM and COPY --from=<name> instructions to refer to the image built in this stage.
      - The tag or digest values are optional. If you omit either of them, the builder assumes a latest tag by default. The builder returns an error if it cannot find the tag value.
    - The optional --platform flag can be used to specify the platform of the image in case FROM references a multi-platform image. For example, linux/amd64, linux/arm64, or windows/amd64. By default, the target platform of the build request is used. Global build arguments can be used in the value of this flag, for example automatic platform ARGs allow you to force a stage to native build platform (--platform=$BUILDPLATFORM), and use it to cross-compile to the target platform inside the stage.
    - An ARG declared before a FROM is outside of a build stage, so it can’t be used in any instruction after a FROM. To use the default value of an ARG declared before the first FROM use an ARG instruction without a value inside of a build stage.
  - RUN
    - RUN has 2 forms:
      - RUN <command> (shell form, the command is run in a shell, which by default is /bin/sh -c on Linux or cmd /S /C on Windows)
      - RUN ["executable", "param1", "param2"] (exec form)
    - The RUN instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile.
    - The exec form is parsed as a JSON array, which means that you must use double-quotes (“) around words not single-quotes (‘).
    - In the JSON form, it is necessary to escape backslashes. This is particularly relevant on Windows where the backslash is the path separator.
    - The cache for RUN instructions isn’t invalidated automatically during the next build. The cache for an instruction like RUN apt-get dist-upgrade -y will be reused during the next build. The cache for RUN instructions can be invalidated by using the --no-cache flag, for example docker build --no-cache.
  - CMD
    - The CMD instruction has three forms:
      - CMD ["executable","param1","param2"] (exec form, this is the preferred form)
      - CMD ["param1","param2"] (as default parameters to ENTRYPOINT)
      - CMD command param1 param2 (shell form)
    - There can only be one CMD instruction in a Dockerfile. If you list more than one CMD then only the last CMD will take effect.
    - The main purpose of a CMD is to provide defaults for an executing container. These defaults can include an executable, or they can omit the executable, in which case you must specify an ENTRYPOINT instruction as well.
    - Unlike the shell form, the exec form does not invoke a command shell. This means that normal shell processing does not happen. For example, CMD [ "echo", "$HOME" ] will not do variable substitution on $HOME. If you want shell processing then either use the shell form or execute a shell directly, for example: CMD [ "sh", "-c", "echo $HOME" ]. When using the exec form and executing a shell directly, as in the case for the shell form, it is the shell that is doing the environment variable expansion, not docker.
    - If you would like your container to run the same executable every time, then you should consider using ENTRYPOINT in combination with CMD.
    - Do not confuse RUN with CMD. RUN actually runs a command and commits the result; CMD does not execute anything at build time, but specifies the intended command for the image.
  - LABEL
    - The LABEL instruction adds metadata to an image. A LABEL is a key-value pair. To include spaces within a LABEL value, use quotes and backslashes as you would in command-line parsing. A few usage examples:
    - Labels included in base or parent images (images in the FROM line) are inherited by your image. If a label already exists but with a different value, the most-recently-applied value overrides any previously-set value.
    - To view an image’s labels, use the docker image inspect command. You can use the --format option to show just the labels;
    ```cmd
    docker image inspect --format='' myimage
    ```
  - MAINTAINER (deprecated)
    - The MAINTAINER instruction sets the Author field of the generated images. The LABEL instruction is a much more flexible version of this and you should use it instead, as it enables setting any metadata you require, and can be viewed easily, for example with docker inspect. To set a label corresponding to the MAINTAINER field you could use:
    ```dockerfile
    LABEL maintainer="SvenDowideit@home.org.au"
    ```
  - EXPOSE
    - The EXPOSE instruction informs Docker that the container listens on the specified network ports at runtime. You can specify whether the port listens on TCP or UDP, and the default is TCP if the protocol is not specified.
    - The EXPOSE instruction does not actually publish the port. It functions as a type of documentation between the person who builds the image and the person who runs the container, about which ports are intended to be published. To actually publish the port when running the container, use the -p flag on docker run to publish and map one or more ports, or the -P flag to publish all exposed ports and map them to high-order ports.
    - By default, EXPOSE assumes TCP. You can also specify UDP: EXPOSE 80/udp
    - To expose on both TCP and UDP, include two lines.
  - ENV
    - The ENV instruction sets the environment variable <key> to the value <value>. This value will be in the environment for all subsequent instructions in the build stage and can be replaced inline in many as well. The value will be interpreted for other environment variables, so quote characters will be removed if they are not escaped. Like command line parsing, quotes and backslashes can be used to include spaces within values.
    - The environment variables set using ENV will persist when a container is run from the resulting image. You can view the values using docker inspect, and change them using docker run --env <key>=<value>.
  - ADD
    - ADD has two forms:
      - ADD [--chown=<user>:<group>] <src>... <dest>
      - ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]
    - The ADD instruction copies new files, directories or remote file URLs from <src> and adds them to the filesystem of the image at the path <dest>.
    - The <dest> is an absolute path, or a path relative to WORKDIR, into which the source will be copied inside the destination container.
    - When adding files or directories that contain special characters (such as [ and ]), you need to escape those paths following the Golang rules to prevent them from being treated as a matching pattern.
    - The first encountered ADD instruction will invalidate the cache for all following instructions from the Dockerfile if the contents of <src> have changed.
    - ADD obeys the following rules:
      - The <src> path must be inside the context of the build; you cannot ADD ../something /something, because the first step of a docker build is to send the context directory (and subdirectories) to the docker daemon.
      - If <src> is a URL and <dest> does not end with a trailing slash, then a file is downloaded from the URL and copied to <dest>.
      - If <src> is a URL and <dest> does end with a trailing slash, then the filename is inferred from the URL and the file is downloaded to <dest>/<filename>. For instance, ADD http://example.com/foobar / would create the file /foobar. The URL must have a nontrivial path so that an appropriate filename can be discovered in this case (http://example.com will not work).
      - If <src> is a directory, the entire contents of the directory are copied, including filesystem metadata.
      - If <src> is a local tar archive in a recognized compression format (identity, gzip, bzip2 or xz) then it is unpacked as a directory. Resources from remote URLs are not decompressed. When a directory is copied or unpacked, it has the same behavior as tar -x, the result is the union of:
        - Whatever existed at the destination path and
        - The contents of the source tree, with conflicts resolved in favor of “2.” on a file-by-file basis.
        If <src> is any other kind of file, it is copied individually along with its metadata. In this case, if <dest> ends with a trailing slash /, it will be considered a directory and the contents of <src> will be written at <dest>/base(<src>).
      - If <src> is any other kind of file, it is copied individually along with its metadata. In this case, if <dest> ends with a trailing slash /, it will be considered a directory and the contents of <src> will be written at <dest>/base(<src>).
      - If multiple <src> resources are specified, either directly or due to the use of a wildcard, then <dest> must be a directory, and it must end with a slash /.
      - If <dest> does not end with a trailing slash, it will be considered a regular file and the contents of <src> will be written at <dest>.
      - If <dest> doesn’t exist, it is created along with all missing directories in its path.
  - COPY
    - COPY has two forms:
      - COPY [--chown=<user>:<group>] <src>... <dest>
      - COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]
    - The COPY instruction copies new files or directories from <src> and adds them to the filesystem of the container at the path <dest>.
    - COPY takes in a src and destination. It only lets you copy in a local file or directory from your host (the machine building the Docker image) into the Docker image itself.
    - ADD lets you do that too, but it also supports 2 other sources. First, you can use a URL instead of a local file / directory. Secondly, you can extract a tar file from the source directly into the destination.
  - ENTRYPOINT
    - ENTRYPOINT has two forms:
      - The exec form, which is the preferred form:
        - ENTRYPOINT ["executable", "param1", "param2"]
      - The shell form:
        - ENTRYPOINT command param1 param2
    - Command line arguments to docker run <image> will be appended after all elements in an exec form ENTRYPOINT, and will override all elements specified using CMD. This allows arguments to be passed to the entry point, i.e., docker run <image> -d will pass the -d argument to the entry point. You can override the ENTRYPOINT instruction using the docker run --entrypoint flag.
    - The shell form prevents any CMD or run command line arguments from being used, but has the disadvantage that your ENTRYPOINT will be started as a subcommand of /bin/sh -c, which does not pass signals. This means that the executable will not be the container’s PID 1 - and will not receive Unix signals - so your executable will not receive a SIGTERM from docker stop <container>.
    - If you need to write a starter script for a single executable, you can ensure that the final executable receives the Unix signals by using exec and gosu commands.
    - Lastly, if you need to do some extra cleanup (or communicate with other containers) on shutdown, or are co-ordinating more than one executable, you may need to ensure that the ENTRYPOINT script receives the Unix signals, passes them on, and then does some more work:
    - You can override the ENTRYPOINT setting using --entrypoint, but this can only set the binary to exec (no sh -c will be used).
    - The exec form is parsed as a JSON array, which means that you must use double-quotes (“) around words not single-quotes (‘).
    - Both CMD and ENTRYPOINT instructions define what command gets executed when running a container. There are few rules that describe their co-operation.
      - Dockerfile should specify at least one of CMD or ENTRYPOINT commands.
      - ENTRYPOINT should be defined when using the container as an executable.
      - CMD should be used as a way of defining default arguments for an ENTRYPOINT command or for executing an ad-hoc command in a container.
      - CMD will be overridden when running the container with alternative arguments.
    - If CMD is defined from the base image, setting ENTRYPOINT will reset CMD to an empty value. In this scenario, CMD must be defined in the current image to have a value.
  - VOLUME
    - The VOLUME instruction creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers.
  - USER
    - The USER instruction sets the user name (or UID) and optionally the user group (or GID) to use when running the image and for any RUN, CMD and ENTRYPOINT instructions that follow it in the Dockerfile.
  - WORKDIR
    - The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile. If the WORKDIR doesn’t exist, it will be created even if it’s not used in any subsequent Dockerfile instruction.
    - The WORKDIR instruction can resolve environment variables previously set using ENV. You can only use environment variables explicitly set in the Dockerfile
  - ARG
    - The ARG instruction defines a variable that users can pass at build-time to the builder with the docker build command using the --build-arg <varname>=<value> flag. If a user specifies a build argument that was not defined in the Dockerfile, the build outputs a warning.
    - It is not recommended to use build-time variables for passing secrets like github keys, user credentials etc. Build-time variable values are visible to any user of the image with the docker history command.
    - If an ARG instruction has a default value and if there is no value passed at build-time, the builder uses the default.
    - An ARG variable definition comes into effect from the line on which it is defined in the Dockerfile not from the argument’s use on the command-line or elsewhere.
    - An ARG instruction goes out of scope at the end of the build stage where it was defined. To use an arg in multiple stages, each stage must include the ARG instruction.
    - ou can use an ARG or an ENV instruction to specify variables that are available to the RUN instruction. Environment variables defined using the ENV instruction always override an ARG instruction of the same name.
    - ARG variables are not persisted into the built image as ENV variables are. However, ARG variables do impact the build cache in similar ways. If a Dockerfile defines an ARG variable whose value is different from a previous build, then a “cache miss” occurs upon its first usage, not its definition. In particular, all RUN instructions following an ARG instruction use the ARG variable implicitly (as an environment variable), thus can cause a cache miss. All predefined ARG variables are exempt from caching unless there is a matching ARG statement in the Dockerfile.
  - ONBUILD
    - The ONBUILD instruction adds to the image a trigger instruction to be executed at a later time, when the image is used as the base for another build. The trigger will be executed in the context of the downstream build, as if it had been inserted immediately after the FROM instruction in the downstream Dockerfile.
    - This is useful if you are building an image which will be used as a base to build other images, for example an application build environment or a daemon which may be customized with user-specific configuration.
  - STOPSIGNAL
    - The STOPSIGNAL instruction sets the system call signal that will be sent to the container to exit. This signal can be a valid unsigned number that matches a position in the kernel’s syscall table, for instance 9, or a signal name in the format SIGNAME, for instance SIGKILL.
  - HEALTHCHECK
    - The HEALTHCHECK instruction has two forms:
      - HEALTHCHECK [OPTIONS] CMD command (check container health by running a command inside the container)
      - HEALTHCHECK NONE (disable any healthcheck inherited from the base image)
    - The HEALTHCHECK instruction tells Docker how to test a container to check that it is still working. This can detect cases such as a web server that is stuck in an infinite loop and unable to handle new connections, even though the server process is still running.
    - The options that can appear before CMD are:
      - --interval=DURATION (default: 30s)
      - --timeout=DURATION (default: 30s)
      - --start-period=DURATION (default: 0s)
      - --retries=N (default: 3)
      - There can only be one HEALTHCHECK instruction in a Dockerfile. If you list more than one then only the last HEALTHCHECK will take effect.
  - SHELL
    - The SHELL instruction allows the default shell used for the shell form of commands to be overridden. The default shell on Linux is ["/bin/sh", "-c"], and on Windows is ["cmd", "/S", "/C"]. The SHELL instruction must be written in JSON form in a Dockerfile.
    - The SHELL instruction is particularly useful on Windows where there are two commonly used and quite different native shells: cmd and powershell, as well as alternate shells available including sh
    - The SHELL instruction can appear multiple times. Each SHELL instruction overrides all previous SHELL instructions, and affects all subsequent instructions.

About storage drivers : https://docs.docker.com/storage/storagedriver/
  - To use storage drivers effectively, it’s important to know how Docker builds and stores images, and how these images are used by containers. You can use this information to make informed choices about the best way to persist data from your applications and avoid performance problems along the way.
  - Storage drivers allow you to create data in the writable layer of your container. The files won’t be persisted after the container is deleted, and both read and write speeds are lower than native file system performance.
  - Images and layers
    - A Docker image is built up from a series of layers. Each layer represents an instruction in the image’s Dockerfile. Each layer except the very last one is read-only.
    - Each layer is only a set of differences from the layer before it. The layers are stacked on top of each other.
    - When you create a new container, you add a new writable layer on top of the underlying layers. This layer is often called the “container layer”.
    - All changes made to the running container, such as writing new files, modifying existing files, and deleting files, are written to this thin writable container layer.
    - A storage driver handles the details about the way these layers interact with each other. Different storage drivers are available, which have advantages and disadvantages in different situations.
  - Container and layers
    - The major difference between a container and an image is the top writable layer. All writes to the container that add new or modify existing data are stored in this writable layer. When the container is deleted, the writable layer is also deleted. The underlying image remains unchanged.
    - Because each container has its own writable container layer, and all changes are stored in this container layer, multiple containers can share access to the same underlying image and yet have their own data state.
    - If you need multiple images to have shared access to the exact same data, store this data in a Docker volume and mount it into your containers.
    - Docker uses storage drivers to manage the contents of the image layers and the writable container layer. Each storage driver handles the implementation differently, but all drivers use stackable image layers and the copy-on-write (CoW) strategy.
  - The copy-on-write (CoW) strategy
    - Copy-on-write (COW), sometimes referred to as implicit sharing or shadowing, is a resource-management technique used in computer programming to efficiently implement a "duplicate" or "copy" operation on modifiable resources. If a resource is duplicated but not modified, it is not necessary to create a new resource; the resource can be shared between the copy and the original. Modifications must still create a copy, hence the technique: the copy operation is deferred until the first write. By sharing resources in this way, it is possible to significantly reduce the resource consumption of unmodified copies, while adding a small overhead to resource-modifying operations
    - Copy-on-write is a strategy of sharing and copying files for maximum efficiency. If a file or directory exists in a lower layer within the image, and another layer (including the writable layer) needs read access to it, it just uses the existing file. The first time another layer needs to modify the file (when building the image or running the container), the file is copied into that layer and modified. This minimizes I/O and the size of each of the subsequent layers.
    - The <missing> lines in the docker history output indicate that those layers were built on another system and are not available locally. This can be ignored.
    - When you start a container, a thin writable container layer is added on top of the other layers. Any changes the container makes to the filesystem are stored here. Any files the container does not change do not get copied to this writable layer. This means that the writable layer is as small as possible.
    - When an existing file in a container is modified, the storage driver performs a copy-on-write operation. The specifics steps involved depend on the specific storage driver. For the aufs, overlay, and overlay2 drivers, the copy-on-write operation follows this rough sequence:
      - Search through the image layers for the file to update. The process starts at the newest layer and works down to the base layer one layer at a time. When results are found, they are added to a cache to speed future operations.
      - Perform a copy_up operation on the first copy of the file that is found, to copy the file to the container’s writable layer.
      - Any modifications are made to this copy of the file, and the container cannot see the read-only copy of the file that exists in the lower layer.
    - Containers that write a lot of data consume more space than containers that do not. This is because most write operations consume new space in the container’s thin writable top layer.
    - Note: for write-heavy applications, you should not store the data in the container. Instead, use Docker volumes, which are independent of the running container and are designed to be efficient for I/O. In addition, volumes can be shared among containers and do not increase the size of your container’s writable layer.
    - Not only does copy-on-write save space, but it also reduces start-up time. When you start a container (or multiple containers from the same image), Docker only needs to create the thin writable container layer.
    - If Docker had to make an entire copy of the underlying image stack each time it started a new container, container start times and disk space used would be significantly increased. This would be similar to the way that virtual machines work, with one or more virtual disks per virtual machine.
  - Container size on disk
    - To view the approximate size of a running container, you can use the docker ps -s command. Two different columns relate to size.
      - size: the amount of data (on disk) that is used for the writable layer of each container.
      - virtual size: the amount of data used for the read-only image data used by the container plus the container’s writable layer size. Multiple containers may share some or all read-only image data. Two containers started from the same image share 100% of the read-only data, while two containers with different images which have layers in common share those common layers. Therefore, you can’t just total the virtual sizes. This over-estimates the total disk usage by a potentially non-trivial amount.
    - The total disk space used by all of the running containers on disk is some combination of each container’s size and the virtual size values. If multiple containers started from the same exact image, the total size on disk for these containers would be SUM (size of containers) plus one image size (virtual size- size).
    - This also does not count the following additional ways a container can take up disk space:
      - Disk space used for log files if you use the json-file logging driver. This can be non-trivial if your container generates a large amount of logging data and log rotation is not configured.
      - Volumes and bind mounts used by the container.
      - Disk space used for the container’s configuration files, which are typically small.
      - Memory written to disk (if swapping is enabled).
      - Checkpoints, if you’re using the experimental checkpoint/restore feature.
