# Research computing: Reproducible and scalable research computing with containers

This is an introduction to containers course at Imperial College London

3 × 2 hour classes

## The ECRI logo

<img src="/images/ecri.webp" width="400" />

## Prerequisites

**NOTE: You are expecting to have some (not necessarily to have many or a lot) basic experience with bash commands, and Python or R programming. We will use basic bash command lines, simple coding in Python, R and C++ in the course. And you are also expecting to have access to [HPC](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-access/)** 

## Description

Scientific software is often dependent on other packages and libraries (and specific versions of these).  The dependencies are hard to reproduce upon redeployment by collaborators and users who may not share the same system setup.  This creates an unnecessary barrier to sharing many excellent software packages making them less shareable and reproducible.

Containers are an effective solution to this problem. The course teaches the fundamentals of creating, deploying and managing containers. You will learn about the essential commands and scripts, about Docker Hub and Singularity (HPC-friendly solution). The workshop will be delivered through a combination of slides, demonstrations and hands-on practice.

This course consists of 3 parts as follows.

- **Part 1**: Introducing containers, Docker command lines.
  - In this part, you will learn why containers are used, what containers are, and you will get hands-on practice with Docker (the most popular container tool) command lines.

- **Part 2**: Create container images with examples. 
  - In this part, you will get familiar with creating container images in Docker environment. You will learn to compose Dockerfile and build container images through examples including bash, Python, R and C++. You will get hands-on practice as well.

- **Part 3**: Share container images and scale up with singularity.
  - In this part, you will practice how to share your own container images with others using either Docker Hub distribution or other ways. You will also learn how to run images using singularity on high performance computing cluster or cloud.

## Intended learning outcomes (ILOs) 

On completion of this course you will be able to: 

- Remember the benefits of using containers for research 
- Use the common Docker commands
- Compose Dockerfiles to build docker containers
- Manage Docker Hub repository
- Share Docker container images
- Interpret common errors and use these to help debug a container
- Use basic singularity commands to scale up containers on HPC 

## Pre-course activities:

Prerequisites: **Docker engine and Docker Hub**

Please try to complete the following setup tasks ahead of the workshop. If you run into problems, please contact the workshop leader at the e-mail address on the home page. We will also provide installation and setup help at the workshop if required, but you will get more out of the training if you can complete them ahead of time.

There are four steps to the setup:

1. Create a Docker Hub account

You should setup a free account on the Docker Hub ahead of the workshop:

- The [Docker Hub](http://hub.docker.com/). We will use the Docker Hub to download pre-built container images, and for you to upload and download container images that you create, as explained in the relevant lesson episodes.
Please seek help at the start of the lesson if you have not been able to setup a Docker Hub account.

2. Install the Docker software on your system

Please try to install the appropriate software from the list below depending on the operating system that your laptop is running:

- Microsoft Windows, either:
  - Try this first, although it won’t work with Windows 10 Home Edition. Install the [Docker Desktop (Windows)](https://hub.docker.com/editions/community/docker-ce-desktop-windows), [YouTube video](https://youtu.be/shVTL3JsBr8)

  or failing that;

  - Install the [Docker Toolbox (Windows)](https://docs.docker.com/toolbox/toolbox_install_windows/).
  
- Apple macOS, either:
  - Try this first, although it will not work with older versions of macOS. Install the [Docker Desktop (Mac)](https://hub.docker.com/editions/community/docker-ce-desktop-mac), [YouTube video](https://youtu.be/BJxGr2Xa6Zk)
  or failing that:

  - Install the [Docker Toolbox (Mac)](https://docs.docker.com/toolbox/toolbox_install_mac/).
  
- Linux: there are too many varieties of Linux to give precise instructions here, but hopefully you can locate documentation for getting Docker installed on your Linux distribution. It may already be installed. If it is not already installed on your system, see:
  - [Docker Engine on CentOS](https://docs.docker.com/install/linux/docker-ce/centos/)
  - [Docker Engine on Debian](https://docs.docker.com/install/linux/docker-ce/debian/)
  - [Docker Engine on Fedora](https://docs.docker.com/install/linux/docker-ce/fedora/)
  - [Docker Engine on Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
  - Note that on Linux, you will either need to run Docker commands as root using `sudo` or [create a docker group to run without sudo](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user).

3. Use online Docker lab environment [Lab Environment](https://www.docker.com/play-with-docker)
4. Use Docker in Docker environment on GitHub by creating / launching codespaces at [Docker GitHub](https://github.com/jianlianggao/codespace_docker)

## Part1: Container and Docker commands page options

### Why container?

**The fundamental problem: software has dependencies that are difficult to manage**
Consider Python: a widely used programming language for analysis. Many Python users install the language and tools using something such as [Anaconda](https://anaconda.org/) and give little thought to the underlying software dependencies allowing them to use libraries such as [Matplotlib](https://matplotlib.org/) or [Pandas](https://pandas.pydata.org/) on their computer. Indeed, in an ideal world none of us would need to think about fixing software dependencies, but we are far from that world. For example:

  - Python 2.7 and Python 3 programs are generally **incompatible**: Not all packages can be installed in the same Python environment at their most up to date versions due to conflicts in the shared dependencies
    - Python 2.7 has now been deprecated but there are still many legacy programs out there
    
  - Different versions of Python tools may give slightly different outputs and/or results
  
All of the above discussion is **just** about ***Python***. Many people use many different tools and pieces of software during their research workflow all of which may have dependency issues. Some software may just depend on the version of the operating system you’re running or be more like Python where the languages change over time, and depend on an enormous set of software libraries written by unrelated software development teams.

**What if** you wanted to distribute a software tool that automated interaction between R and Python. Both of these language environments have independent version and software dependency lineages. As the number of software components such as R and Python increases, this can rapidly lead to a combinatorial explosion in the number of possible configurations, only some of which will work as intended. This situation is sometimes informally termed “dependency hell”.

The situation is often mitigated in part by factors such as:

- an acceptance of inherent software and hardware obsolesce so it’s not expected that all versions of software need to be supported forever;

- some inherent synchronisation in the reasons for making software changes (e.g., the shift from 32-bit to 64-bit software), so not all versions will be expected to interact with all other versions.

Although we have highlighted the dependency issue above, there are other, related problems that multiple versions of tools and software can cause:

- Reproducibility: we want to make sure that we (and others) can reproduce our research outputs and results. What if, a year after we have run an analysis pipeline and produced some results on a laptop, we need to run the same pipeline and reproduce the results but our old laptop has now been replaced by a new one (perhaps with a different operating system)? How can we make sure our software environment is equivalent to that on which we performed the original work?

- Workload: most people use multiple computers for their research (either simultaneously, or, over a period of time, due to upgrades or hardware failures). Installing the software and tools we need on all these systems and keeping everything up to date and in sync is a large burden of additional work we could do without. This problem may be even more severe if we have to use shared advanced computing facilities where we may not have administrator access to install software.

Thankfully there are ways to get underneath (a lot of) this mess: containers to the rescue! Containers provide a way to package up software dependencies and access to resources such as data in a uniform and portable manner that allows them to be shared and reused across many different computer resources.

### What are containers?

The term “container” can be usefully considered with reference to shipping containers. Before shipping containers were developed, packing and unpacking cargo ships was time consuming, and error prone, with high potential for different clients’ goods to become mixed up. 

<img src="/images/ancient_dock1.png">

<img src="/images/ancient_dock2.png">

Since shipping containers were introduced, the shipping has become much better organized.

<img src="/images/modern_dock1.png">

<img src="/images/modern_dock2.png">

Software containers standardise the packaging of a complete software system (the lightweight virtual machine): you can drop a container into a container host, and it should “just work”.

<img src="/images/vm_containers.png">

On this course, we will be using Linux containers - all of the containers we will meet are based on the Linux operating system in one form or another. However, the same Linux containers we create can run on:

- MacOS;
- Microsoft Windows;
- Linux;

and

- The Cloud


### Containers file systems

One complication with using a virtual environment such as a container is that the file systems (i.e. the directories that the container sees) can now potentially come from two different locations:

- **Internal file systems**: these provide directories that are part of the container itself and are not visible on the host outside the container. These directories can have the same location as directories on the host but the container will see its internal version of the directories rather than the host versions.

- **Host file systems**: these are directories mapped from the host into the container to allow the container to access data on the host system. Some container systems (e.g. Singularity) map particular directories into the container by default while others (e.g. Docker) do not generally do this (needs to be mapped manually). Note that the location (or path) to the directories in the container is not necessarily the same as that in the host. The command you use to start the container will usually provide a way to map host directories to directories in the container.

This is illustrated in the diagram below:
<img src="/images/container_file_system.png">

### Docker and its terminology

#### Docker:

[Docker](https://www.docker.com/) is software that manages containers and the resources that containers need. While Docker is a leader in the container space, there are many similar technologies available and the concepts we learn in this workshop will allow us to use other container platforms even if their command syntax will be a little different.

#### Terminology:

- **Image**: this is the term that Docker uses to describe the template for the virtual hard disk contents (files and folders) from which live instances of containers will be created. The term “container image” may sometimes be used to emphasise that the “image” relates to software containers and not, say, the sense of an “image” when discussing VMs or cute kitten pictures (without loss of generality).

- **Container**: this is an instance of a lightweight virtual machine created by Docker from a (container) image.
  - If you are interested in more technical details, Docker actually creates images by combining together multiple**layers**, although you can profitably use Docker without knowing much about layers. As a quick summary, each layer is a given set of files and folders. The combination of layers essentially involves a set-wise union of the files and folders in the layers, except that there is also a way for upper layers to hide files from lower layers (which has the appearance of deleting those files). Layers facilitate efficient storage space use, by allowing container images to share and reuse sets of files and folders, while still allowing individual container images to have their own specific files and folders.
 
<img src="/images/images.png" width="600" />

- **Docker Hub**: the Docker Hub is a storage resource and associated website where a vast collection of preexisting container images are documented and stored, and are made available for your use.

### Basic docker commands

1. Once your Docker application is running, open a shell (terminal) window, and run the following command to check that Docker is installed and the command line tools are working correctly. 

     `docker --version`
  
2. A command that checks that the virtual machine host is running is the Docker container list command.

      `docker container ls`  (or `docker ps`)

3. One of the simplest Docker container images just allows you to create containers that print a welcome message.

To create and run containers (instances) from named container images you use the docker run command. Open a shell window if you do not already have one open and try the following docker run invocation. Note that it does not matter what your current working directory is.

`docker run hello-world`
      
Try: 

run the above command again and observe the difference.

* The “digest” is a secure fingerprint (a “hash”) of the particular version of the container image that you now have.
* If you have any trouble, try to login to docker by running `docker login` (Thanks Gordon for this tip).

4. List of docker images on your computer

Run:

`docker image ls`  (or `docker images`)
      
5. Remove docker image(s)

If you need to reclaim disk space, you can remove image files. The images and their corresponding containers can start to take up a lot of disk space if you don’t clean them up occasionally. If you want to remove an image, you will need to find out details such as the image ID or name of the repository. 

`docker image rm hello-world` (or `docker rmi hello-world`) (or `docker rmi <image ID>`)
    
if you get the following error message (example):

`Error response from daemon: conflict: unable to remove repository reference "hello-world" (must force) - container a6d14ab49884 is using its referenced image bf756fb1ae65`


that is because there are containers created that depend on this image.

Then you need find what containers are depending on the image(s) to remove it from the containers list first.

`docker container ls` or `docker container ls --all`
then
`docker container rm ID` (or `docker rm <container ID>`) or `docker container rm container_name`

NOTE: 

If you want to remove all exited containers at once you can use the docker containers prune command.

Be careful with this command. If you have containers you may want to reconnect to, you should not use this command. It will ask you if to confirm you want to remove ALL stopped (exited) containers, see output below. If successful it will print the full CONTAINER IDs back to you.



Finally, you are able to remove the unwanted docker image(s).

6. Remove all container instances and images.

for removing all exited container instances, use
`docker rm $(docker ps -a -q)`

for removing all images, use

`docker image rm $(docker image ls -q)` (this command will fail for the images sharing the same image IDs)

This command assumes you are using a bash (or compatible) shell. If you happen to be using another shell such as csh or tcsh, this command will fail and you should instead wrap the docker image ls -q part of the command in “backticks”, e.g. \`docker image ls -q\`

### More interaction with Docker

Often, you will need to use containers to run specific commands, analyses or tasks (as we have seen with the “hello-world” example above). Sometimes, however, you would prefer to have a container running and to get interactive access so you can run commands directly within the container yourself, or debug your docker image. 

To do so, you need to add flag(s) to a docker image when you run it.


1.  Run docker container in interactive mode

`docker run -t -i ubuntu /bin/bash`

  - `-t` flag: Allocate a psuedo-tty to allow us to access the container interactively
  - `-i` flag: interactive mode

2. Remove container automatically after exited

docker run --rm -t -i ubuntu /bin/bash

To get out of the container image, we use the exit command.

**NOTE:**

For the above approach to work the image has to run a shell by default (most Linux OS images do this). Some minimal OS images may also need you to add the `--entrypoint "/bin/bash"`  option to docker run (`docker run --rm -t -i --entrypoint "/bin/bash" ubuntu`), or to place "/bin/bash" at the end of the docker run command, depends on use of ENTRYPOINT or CMD in the Dockerfile.

What’s the difference between the –entrypoint option and placing /bin/sh at the end of the command? Some images are pre-configured with an entrypoint - the command to run by default within a container started from the image. If the default command that runs when you start the container is not the command you want to run, you can override it using the `--entrypoint` option. If an image does not have an entrypoint pre-configured, you tell Docker what command to run when starting a container from the image at the end of the `docker run` command.

3. Run docker container in background and allow to interact later

`docker run -t -d ubuntu`

  - `-d`  flag: Detach the container and run in the background
We can get the image ID and see the image is running in the background with `docker container ls` .

**Now we have** a running container with an open shell sitting waiting for input. The open shell and the fact that we assigned a pseudo-tty to connect to the shell’s input channel prevent the container from exiting straight away as it would have if we ran a command that exited immediately (for example echo Hello World!).

Then, let’s start an interactive shell directly in the container image using the `docker exec`  command with the container image ID or container image name:

`docker exec -i -t <container name> bash`

To get out of the container, use exit command.

Finally, we need to stop the container (it is still running in the background). We use the `docker stop`  command with the container `name`  or `ID`  to do this.

4. Mount local directories to container

A docker container is by default isolated from the host system. If you want to keep some files (e.g., data analysis results), you don't want them disappeared when the container exited. Therefore it is likely that you will want to mount a local directory from the host system in the container that you start in order to make files from the host system available within the running container. For now, as a quick example, modify the `docker run`  command above so that the container will have the current directory that you are in on the host machine mounted at `/data`  in the container:

`docker run --rm -t -i -v ${PWD}:/data ubuntu "/bin/bash"`

5. More advanced interactions

For more advanced options, please refer to [docker run documentation](https://docs.docker.com/engine/reference/commandline/run/)

This is an example for running a web service (HackMD, an online doc editor) using docker engine. Thanks to [HackMD](https://hackmd.io/@MaxWu/Hk39SWD0x?type=view)


First, setup the environment. 

```
docker network create backend

docker volume create database #(optional)
```
Second, start database engine with postgreSQL.

```
docker run -d --name database --network backend -e POSTGRES_USER=hackmd -e POSTGRES_PASSWORD=hackmdpass -e POSTGRES_DB=hackmd -v ${PWD}/database:/var/lib/postgresql/data postgres:9.6-alpine
```

Finally, start HackMD.

```
docker run -d --name app --network backend -e HMD_DB_URL=postgres://hackmd:hackmdpass@database:5432/hackmd -p 3000:3000 --restart always --link database hackmdio/hackmd:1.2.0
```

- NOTE:
  - `--name` is for specifying application name instead of being randomly asssigned by docker engine.
  - `-p` is for mapping the port from host to container instance <host port>:<container port>
  - `-e` is for setting environment variable(s)



To test your local HackMD service, please visit 127.0.0.1:3000 on your computer (*NOTE: if you want to change the port number for accessing the containerized service, you need to change the 3000 before the : to another port number as you wish*)

or 

\<your computer IP\>:3000 on your mobile devices / another computer which are connected to the same WiFi.
  
Try: 

- Stop the HackMD service and remove the containers after testing.
- Backup the database directory.
- Redo running HackMD service by mounting to the backup directory.

*Windows GUI application*:

Try:
`docker run -it --rm -p 8006:8006 --device=/dev/kvm --device=/dev/net/tun --cap-add NET_ADMIN --stop-timeout 120 dockurr/windows`

- `--cap-add NET_ADMIN` for creating bridge connection from host to the container.
- `--device=/dev/kvm` for improving performance with KVM acceleration.
- `--device=/dev/net/tun` for the container to use the TUN (network tunnel) device, which is often required for VPNs and other network-related tasks.
- more details can be found at https://github.com/dockur/windows

*Another example of application*:

Setup GitHub codespaces R environment. [R Application Example](https://github.com/jianlianggao/R_app_example)



## Part2: Generate container images
 
### Create own container images

Where to get help: the [Docker Community Forums](https://forums.docker.com/), [the Docker Community Slack](https://dockercommunity.slack.com/), or [Stack Overflow](https://stackoverflow.com/search?tab=newest&q=docker).

To create your own container images, you need to compose Dockerfiles.

A Dockerfile contains a set of instructions with options to the instructions. There are many different instructions available, we only cover a few here. See [https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/) for a full description.

In our simple Dockerfile, we have the following six instructions:

- `FROM`: Initialises the build and specifies the base image for subsequent instructions - all Dockerfiles start with this instruction.
- `RUN`: Runs a command (using /bin/sh on Linux).
- `ADD`: Copies new files, directories from <src> and adds them to the filesystem of the image at the path <dest>.
- `CMD`: The default computational work that will be preformed when the container is executed using `docker run`. There can be only one CMD instruction in a Dockerfile.
- `ENTRYPOINT`: An ENTRYPOINT allows you to configure a container that will run as an executable. The best use for ENTRYPOINT is to set the image’s main command, allowing that image to be run as though it was that command (and then use CMD as the default flags).
- `WORKDIR`: The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile. If the WORKDIR doesn’t exist, it will be created even if it’s not used in any subsequent Dockerfile instruction.

To build a container image:

- Make sure in the same directory as where the Dockerfile is saved.
- Run the following command line:  "docker build -t <container image name[:tag]> ."
    -  **NOTE:**
        - No quotation marks, and do NOT miss the . at the end. :tag is optional, and it is "latest" by default
        - Naming rule for a container image name: start with an alphanumeric character, followed by a-z0-9, _ (underscore), . (period) or - (hyphen), or / (forward slash).


### Create own container images - Example 1

Example 1: Compose one Dockerfile and one bash script

- **Dockerfile** contains:

```
FROM ubuntu 

ADD print.sh ./ 

RUN chmod a+x ./print.sh 

CMD ["./print.sh"]
```

- print.sh (start without shebang line) contains:

```
#!/bin/bash

echo "This is a print test. Happy Year of the Snake!"

```

NOTE: shebang line (#!/bin/bash) means running the file using Bash Shell.

- **Multi platform image build**

Try:
```
docker buildx build --platform=linux/amd64,linux/arm64 -t shell_test_multi_platform .
```
If an error occurs like:
ERROR: Multi-platform build is not supported for the docker driver.
Switch to a different driver, or turn on the containerd image store, and try again.

Then you need to run: 
```
docker buildx create --name=container --driver=docker-container --use --bootstrap
```
and rerun the previous command.

If the following warning 
```
WARNING: No output specified with docker-container driver. Build result will only remain in the build cache. To push result image into registry use --push or to load image into docker use --load
```
is displayed. The docker driver has been setup correctly.
Then you can try:
```
docker buildx build --platform=linux/amd64,linux/arm64 -t <docker_hub_username>/shell_test_multi_platform --push .
```
**NOTE:** Docker hub login is required.


- Try to run a slightly more complicated shell script:
```
#!/bin/bash

echo "This is a Docker test. Happy Year of the Snake!"

for i in {1..10}
do
  echo $i > /data/file_$i.txt
done
```
NOTE: Build an image and test it with a mounted directory.

### Create own container images - Example 2

Example 2: Compose one Dockerfile with a simple Python code.

- Dockerfile contains:

```
FROM ubuntu

RUN apt update && apt install -y python3 

ADD py_test.py ./ 

CMD ["python", "py_test.py"]
```

- py_test.py  contains:

```
print("This is a Python test in docker!")
```
TIP: You need to interact with the container instance to debug error(s) you meet.
NOTE: Packages need to be installed using RUN.



### Create own container images - Example 3


Example 3: Compose one Dockerfile, and a Python code with a sample pandas dataset.

- Dockerfile contains:
```
FROM ubuntu:20.04

RUN apt update && apt install -y python3 python3-pip && pip3 install pandas

ADD py_test.py ./

CMD ["python3", "py_test.py"]
```
**NOTE:** for using the latest Ubuntu, the first two lines in the Dockerfile should be:
```
FROM ubuntu
RUN apt update && apt install -y python3 python3-pip && \
        python3 -m pip config set global.break-system-packages true && \
        pip3 install pandas
```

- py_test.py  contains:

```
import pandas as pd

data = { 'Company' : ['VW','Toyota','Renault','KIA','Tesla'], 'Cars Sold (millions)' : [10.8,10.7,10.3,7.4,0.25], 'Best Selling Model' : ['Golf','RAV4','Clio','Forte','Model 3']}

frame = pd.DataFrame(data)

frame.info()

print("This is a Python test in docker!")
```

Try: build an image with a tag (not the default one "latest").



### Create own container images - Example 4


Example 4: Compose one Dockerfile with a R code. This example helps to understand installing R packages in R environment.

- Dockerfile contains:
```
FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive 

RUN apt update && apt install -y r-base && \

    R -e "install.packages('dplyr', repos='http://cran.r-project.org')"

ADD test.r ./

CMD ["Rscript", "test.r"]
```

- test.r  contains:
```
library(dplyr)

print("This is a R test in docker!")
```

**NOTE**: ENV DEBIAN_FRONTEND=noninteractive for disabling region/country selection when installing packages.



### Create own container images - Example 5

Attached Files:
File [CW_example_data.csv](https://raw.githubusercontent.com/jianlianggao/course-intro-to-containers/main/sample_data/CW_example_data.csv) (18.961 KB, right click and save link as CW_example_data.csv to your local drive)


Example 5: Compose one Dockerfile with a R code. This example helps to understand how to access data on host directory.

- Dockerfile contains:
```
FROM ubuntu

ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /usr/local/src

RUN apt update && apt install -y r-base && \

    R -e "install.packages('dplyr', repos='http://cran.r-project.org')" 

ADD test.r ./

ENTRYPOINT ["Rscript", "test.r"]
```

- test.r  contains:
```
library(dplyr)

args <- commandArgs(trailingOnly = TRUE)

print(args[1])

print(args[2])

data <- read.csv(args[1], stringsAsFactors = F)

data_new <- data %>% filter(GENDER == 'MALE')

if (!dir.exists(args[2])) {

  dir.create(args[2])

}

write.csv(data_new, paste(args[2],'output.csv',sep=''))

print("This is a R test in docker!")
```

Try:

```
docker run --rm -v ${PWD}:/data <container image name> /data/dataset/CW_example_data.csv /data/output/
```

**NOTE**:

1. Download the attached dataset to your local directory.
ENV DEBIAN_FRONTEND=noninteractive for disabling region/country selection when installing packages.
When taking input parameters from docker run, need to use ENTRYPOINT

2. If you run docker online (play-with-docker), you will need to generate key pairs on your computer before you can upload data file using `scp`.
   To generate key pairs for connecting to your docker online instance, run the command `ssh-keygen -t ed25519 -P "" -f ~/.ssh/id_ed25519` in your terminal window (tested in a MacOS terminal window. should work in a Ubuntu terminal window. For a Windows command window, not the PowerShell window, `ssh-keygen` may exist in `C:\Windows\System32\OpenSSH\` and you may need to add the full path to `ssh-keygen.exe` in order to run the command)


### Create own container images - Example 6

Attached Files:
File [CW_example_data.csv](https://raw.githubusercontent.com/jianlianggao/course-intro-to-containers/main/sample_data/CW_example_data.csv) (18.961 KB, right click and save link as CW_example_data.csv to your local drive)


- Dockerfile contains:

```
FROM ubuntu

WORKDIR /usr/local/src

RUN apt update && apt install -y python3 python3-pip && pip3 install pandas

ADD py_test.py ./

ENTRYPOINT ["python3", "py_test.py"]
```

- py_test.py  contains:

```
import sys,os

import pandas as pd

 

args = sys.argv

input = args[1]

output = args[2]+"output.csv"

 

df = pd.read_csv(input)

print(df)

 

data = { 'Company' : ['VW','Toyota','Renault','KIA','Tesla'], 'Cars Sold (millions)' : [10.8,10.7,10.3,7.4,0.25], 'Best Selling Model' : ['Golf','RAV4','Clio','Forte','Model 3']}

frame = pd.DataFrame(data)

# Thanks to Chen Chen for suggesting the following 2 lines to avoid partail failure of the image run.
if not os.path.exists(args[2]):
    os.makedirs(args[2])

frame.to_csv(output)

print("This is a Python test in docker!")

```

Try:

```
docker run --rm -v ${PWD}:/data  <container image name> /data/dataset/CW_example_data.csv /data/output/
```

**NOTE**:

1. Download the attached dataset to your local directory.
2. When taking input parameters from docker run, need to use ENTRYPOINT
3. If you encounter an error like `ERROR: failed to solve: process "/bin/sh -c apt update && apt install -y python3 python3-pip && pip3 install pandas" did not complete successfully: exit code: 1` when building an image from this example, please add `--break-system-packages` to `pip3 install`, for this example, `pip3 install --break-system-packages pandas` and try again to build the image
4. If you want to have package(s) install in the Dockerfile with non-root user(s), you can create a non-root user in the Dockerfile. For example
```
FROM ubuntu

WORKDIR /usr/local/src

ADD requirements.txt ./

RUN apt update && apt install -y python3 python3-pip python3-venv

RUN useradd -m -s /bin/bash appuser && chown -R appuser:appuser /usr/local/src

# switch to the user added
USER appuser
 
RUN python3 -m venv /home/appuser/venv && \
    /home/appuser/venv/bin/pip install --no-cache-dir -r requirements.txt #--break-system-packages

ADD py_test.py ./

ENTRYPOINT ["/home/appuser/venv/bin/python3", "py_test.py"]
```

**Tips**:
1. You can try to build your container image(s) based-on an existing image on your computer, that will speed up your image(s) creating. (Thanks to Drake for this tip).

### Create own container images - Example 7

Try: Create a new container image based on the existing image from example 6. In the new docker image, try to install a new Python package, for example: numpy. Then test the numpy package in Python script.
  
### Create own container images - Example 8

Example 8: Compose one Dockerfile with a shell script and a simple C++ code.

- Dockerfile contains:

```
FROM ubuntu
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && \
    apt-get -y install g++ mono-mcs && \
    rm -rf /var/lib/apt/lists/*
WORKDIR /usr/local/src
ENV NAME VAR1
ENV NAME VAR2
ENV NAME VAR3
ENV OUT_DIR=/output_dir/
ADD run_helloC.sh ./
ADD helloworld.cpp ./
RUN g++ -o helloworld helloworld.cpp

CMD ["/bin/sh", "./run_helloC.sh"]
```

- helloworld.cpp  contains:

```
#include <iostream>
#include <string>
#include <fstream> 
using namespace std;

int main(int argc, char **argv)
{
  cout << "Hello world example, with a directory and arguments!" << endl;

  ofstream out;
  string val;
  if (argc >= 2){
        val = argv[1];
        cout << "Directory is : " << val << endl;

        string filename = val + "/test_write.txt";
        out.open(filename.c_str());
        out << "Hello World C++ example!" << endl;
         for (int i = 2; i < argc; i++){
                val = argv[i];
                cout << "Argument " << i << " " << val << endl;
                out << "Argument " << i << " " << val << endl;
        }
        out.close();
  } else


  return 0;
}
```

- run_helloC.sh  contains:
                                                             
```
#!/bin/sh

./helloworld $OUT_DIR $VAR1 $VAR2 $VAR3
```
                                                             
- To test the container image, please try:
```
docker run -it --rm -v $PWD/output_dir:/output_dir -e VAR1=15 -e VAR2=20 hello_c
```
                                                             
*Thanks to Amy's work [Amy Tabb](https://amytabb.com/tips/tutorials/2018/07/28/docker-tutorial-c-plus-plus/) .*
                                                             
### After building container image(s): release hard drive space

- prune

To clean up all build cache, run `docker builder prune -a` 



## Part3: Share containers and scale up

### Share Docker images

Docker engine saves its virtual image data in intricate locations. For example 

in MacOS,

~/Library/Containers/com.docker.docker/Data/vms/0

and the data in the directory is also hard to understand because the data look like:

<img src="/images/docker_image_location_mac.png">

It's incredibly difficult to know what needs to be copied in order to use your own docker images on another computer. 

Solution: use `docker save` command.

Examples:

1. `docker save -o r_docker.tar r_docker`  (fast but no compression)

2. `docker save r_docker | gzip > r_docker.tar.gz` (slow but with compression)

Now try:

remove the existing image from the virtual environment (hint: from docker image list)

Then:

load saved docker image.

command:  `docker load -i <image_name>`

### Share docker images -- group activity

Group activity: (in pairs)

Step 1. create your own docker image.

Step 2. send file to your peer. (By fileexchange at Imperial College, or Google Drive, Dropbox, OneDrive etc).

Step 3. Your peer load received image into docker and test run. 

### Docker hub

Share docker images via Docker Hub.

1. Upload docker images onto Docker Hub

- Login Docker hub via command line
    - `docker login`
- Create docker images with `<docker hub account name>/<docker image name>:[tag]` ([tag] is optional)
    - for example: `docker build -t jianlianggao/r_docker2 .` or `docker build -t jianlianggao/r_docker2:20210423 .`
- Or change docker image's name with docker tag <existing name> <new name:[tag]>
    - for example: `docker tag r_docker2 jianlianggao/r_docker2:20210423`
- Push docker images onto Docker Hub
    - for example: `docker push jianlianggao/r_docker2`
    
2. Download docker images from Docker Hub

- Use command: `docker pull` 
    - for example: `docker pull jianlianggao/r_docker2` 
    
NOTE: be careful about the tags.


### Docker Hub -- group activity

Group activity: (in pairs)

Step 1. Create your own docker image.

Step 2. Push docker image to Docker Hub.

Step 3. Your peer downloads your image from Docker Hub and tests run. 

### Singularity (Apptainer)

HPC application:

(detailed instruction for using HPC can be found at [https://imperialcollegelondon.app.box.com/s/kwjxbd5bc87w296wo0m7fdwo9jct5vvs](https://imperialcollegelondon.app.box.com/s/kwjxbd5bc87w296wo0m7fdwo9jct5vvs) ) and [Imperial HPC wiki](https://wiki.imperial.ac.uk/display/HPC/Running+your+first+job)

- HPC check available Singularity modules
    - `module avail`
- Load module
    - for example: `module load singular/3.1.1`
- Download docker images
    - for example: `singularity pull --name pypd_docker.simg docker://jianlianggao/pypd_docker:202106`
    - NOTE: be careful about tags
- Compose .pbs script for one single job
    - for example: singularity_test1.pbs

```
#PBS -l walltime=00:20:00

#PBS -l select=1:ncpus=2:mem=8gb

module load singular/3.1.1

singularity run -B /rds/general/user/jgao/home/singularity_test:/data /rds/general/user/jgao/home/singularity_test/pypd_docker.simg /data/dataset/CW_example_data.csv /data/
```

  - submit the job by running `qsub singularity_test1.pbs`

To check the status of the submitted job, run the following command

**Tips:**
- Use `cd $PBS_O_WORKDIR` to avoid using absolute path to the .pbs script above. (*Thanks to Purwanto, Haditya Kukuh (known as Kukuh, PhD cadidate in Process System Engineering at Imperial)*)

```
#PBS -l walltime=00:20:00

#PBS -l select=1:ncpus=2:mem=8gb

module load singular/3.1.1

cd $PBS_O_WORKDIR

singularity run -B ./:/data pypd_docker.simg /data/dataset/CW_example_data.csv /data/
```


```
qstat <job ID>
```


NOTE: **singularity may not like the `WORKDIR` setting in the Dockefile. A full path should then be added to the `ENTRYPOINT` setting and rebuild the container image**. if something goes wrong, to debug singularity images, run the following command

```
singularity shell -C <image name>
```


- Compose .pbs script for parallel jobs
    - for example: singularity_par_test1.pbs

```
#PBS -l walltime=00:20:00

#PBS -l select=1:ncpus=2:mem=8gb

#PBS -J 1-5

module load singular/3.1.1

singularity run -B /rds/general/user/jgao/home/singularity_test:/data /rds/general/user/jgao/home/singularity_test/pypd_docker.simg /data/dataset/CW_example_data_${PBS_ARRAY_INDEX}.csv /data/output${PBS_ARRAY_INDEX}/
```

To view the status of parallel jobs, please use the following command:

  
```
qstat -rt <job ID>[]
```


## Part4: Acknowledgement

The materials in this lesson are inspired by

D. M. Eyers, S. L. R. Stevens, A. Turner, C. Koch and J. Cohen. "Reproducible computational environments using containers: Introduction to Docker".
Version 2020.09a (4a93bd67aa), September 2020. Carpentries Incubator. https://github.com/carpentries-incubator/docker-introduction
  
Many thanks to 
[Dr. Jeremy Cohen](https://www.imperial.ac.uk/people/jeremy.cohen), who kindly provided feedback and ideas. If you are interested in more courses about containers, please click on Dr. Cohen's name to find his contact.

Dr. Katerina Michalickova and Dr. Magdalena Jara, who discussed with me and provided comments on the design of this course.

### Concetps not included

#### Docker-compose
  
This workshop does not include docker-compose. If you are interested in docker-compose, please have a look at 

https://docs.docker.com/compose/gettingstarted/

and do more research if needed. Or feel free to speak to me by email/Teams
  
#### Nextflow
  
You may have also heard about nextflow. It is not covered in this course. If you are keen to know about it, please feel free to start from [Jack Gisby's case study](https://github.com/ImperialCollegeLondon/ReCoDE_rnaseq_pipeline).

#### Container Application for NVIDIA CUDA

Please read this article [Enabling your AI and ML projects with GPU acceleration on Linux--By Dr Benjamin J  Scharpf](https://www.linkedin.com/posts/activity-7215832323945050112-x28H/?utm_source=share&utm_medium=member_desktop)

# Feedback form
If you're taking this course through the ECRI, please fill out [the feedback form](https://imperial.eu.qualtrics.com/jfe/form/SV_9HzWTxus5jKCyxg).
