#### 基于 docker `1.6.2`

```shell
docker help #列出所有可用的命令与简介
docker help $command 或者 docker $command --help #列出某个命令的帮助
```
#### 容器运行相关

|命令|用法|备注|
|---|-------|----------|
|attach|`attach [OPTIONS] $container`|Attach to a running container|
|cp|`cp $container:path $hostdir or -`|Copy files/folders from a container's filesystem to the host path|
|events|`events [OPTIONS]`|Get real time events from the server|
|exec|`exec [OPTIONS] CONTAINER COMMAND [ARG...]`|Run a command in a running container|
|kill|`kill [OPTIONS] CONTAINER [CONTAINER...]`|Kill a running container|
|rename|`rename OLD_NAME NEW_NAME`|Rename an existing container|
|run|`run [OPTIONS] IMAGE [COMMAND] [ARG...]`|Run a command in a new container|
|pause|`pause CONTAINER [CONTAINER...]`|Pause all processes within a container|
|unpause|`unpause CONTAINER [CONTAINER...]`|Unpause a paused container|
|start|`start [OPTIONS] CONTAINER [CONTAINER...]`|Start a stopped container|
|stop|`stop [OPTIONS] CONTAINER [CONTAINER...]`|Stop a running container|
|restart|`restart [OPTIONS] CONTAINER [CONTAINER...]`|Restart a running container|
|wait|`wait [OPTIONS] CONTAINER [CONTAINER...]`|Block until a container stops, then print its exit code|
|create|`create [OPTIONS] IMAGE [COMMAND] [ARG...]`|Create a new container|
|rm|`rm [OPTIONS] CONTAINER [CONTAINER...]`|Remove one or more containers|
|stats|`stats [OPTIONS] CONTAINER [CONTAINER...]`|Display a stream of a containers' resource usage statistics|
|ps|`ps [OPTIONS]`|List containers|
|top|`top [OPTIONS] CONTAINER [ps OPTIONS]`|Lookup the running processes of a container|
|inspect|`inspect [OPTIONS] CONTAINER or IMAGE [CONTAINER or IMAGE...]`|Return low-level information on a container or image|
|logs|`logs [OPTIONS] CONTAINER`|Fetch the logs of a container|
|port|`port [OPTIONS] CONTAINER [PRIVATE_PORT[/PROTO]]`|Lookup the public-facing port that is NAT-ed to PRIVATE_PORT|
|diff|`diff [OPTIONS] CONTAINER`|Inspect changes on a container's filesystem|

#### image 打包相关

|命令|用法|备注|
|---|-------|----------|
|build|`build [OPTIONS] PATH or URL or -`|Build an image from a Dockerfile|
|commit|`commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]`|Create a new image from a container's changes|
|images|`images [OPTIONS] [REPOSITORY]`|List images|
|history|`history [OPTIONS] IMAGE`|Show the history of an image|
|rmi|`rmi [OPTIONS] IMAGE [IMAGE...]`|Remove one or more images|
|export|`export [OPTIONS] CONTAINER`|Stream the contents of a container as a tar archive|
|import|`import [OPTIONS] URL or - [REPOSITORY[:TAG]]`|Create a new filesystem image from the contents of a tarball|
|load|`load [OPTIONS]`|Load an image from a tar archive|
|save|`save [OPTIONS] IMAGE [IMAGE...]`|Save an image to a tar archive|
|tag|`tag [OPTIONS] IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG]`|Tag an image into a repository|


#### Register 相关

|命令|用法|备注|
|---|-------|----------|
|login|`login [OPTIONS] [SERVER]`|Register or log in to a Docker registry server|
|logout|`logout [SERVER]`|Log out from a Docker registry server|
|search|`search [OPTIONS] TERM`|Search for an image on the Docker Hub|
|push|`push [OPTIONS] NAME[:TAG]`|Push an image or a repository to a Docker registry server|
|pull|`pull [OPTIONS] NAME[:TAG or @DIGEST]`|Pull an image or a repository from a Docker registry server|

#### 系统相关

|命令|用法|备注|
|---|-------|----------|
|info|`info`|Display system-wide information|
|version|`version`|Show the Docker version information|

#### 容器资源配置相关选项
```shell
-a, --attach=[]            Attach to STDIN, STDOUT or STDERR
--add-host=[]              Add a custom host-to-IP mapping (host:ip)
-c, --cpu-shares=0         CPU shares (relative weight)
--cap-add=[]               Add Linux capabilities
--cap-drop=[]              Drop Linux capabilities
--cgroup-parent=           Optional parent cgroup for the container
--cidfile=                 Write the container ID to the file
--cpuset-cpus=             CPUs in which to allow execution (0-3, 0,1)
-d, --detach=false         Run container in background and print container ID
--device=[]                Add a host device to the container
--dns=[]                   Set custom DNS servers
--dns-search=[]            Set custom DNS search domains
-e, --env=[]               Set environment variables
--entrypoint=              Overwrite the default ENTRYPOINT of the image
--env-file=[]              Read in a file of environment variables
--expose=[]                Expose a port or a range of ports
-h, --hostname=            Container host name
--help=false               Print usage
-i, --interactive=false    Keep STDIN open even if not attached
--ipc=                     IPC namespace to use
-l, --label=[]             Set meta data on a container
--label-file=[]            Read in a line delimited file of labels
--link=[]                  Add link to another container
--log-driver=              Logging driver for container
--lxc-conf=[]              Add custom lxc options
-m, --memory=              Memory limit
--mac-address=             Container MAC address (e.g. 92:d0:c6:0a:29:33)
--memory-swap=             Total memory (memory + swap), '-1' to disable swap
--name=                    Assign a name to the container
--net=bridge               Set the Network mode for the container
-P, --publish-all=false    Publish all exposed ports to random ports
-p, --publish=[]           Publish a container's port(s) to the host
--pid=                     PID namespace to use
--privileged=false         Give extended privileges to this container
--read-only=false          Mount the container's root filesystem as read only
--restart=no               Restart policy to apply when a container exits
--rm=false                 Automatically remove the container when it exits
--security-opt=[]          Security Options
--sig-proxy=true           Proxy received signals to the process
 -t, --tty=false            Allocate a pseudo-TTY
 -u, --user=                Username or UID (format: <name|uid>[:<group|gid>])
 --ulimit=[]                Ulimit options
 -v, --volume=[]            Bind mount a volume
 --volumes-from=[]          Mount volumes from the specified container(s)
 -w, --workdir=             Working directory inside the container
```
