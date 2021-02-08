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
