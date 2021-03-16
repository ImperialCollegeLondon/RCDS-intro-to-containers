# Research computing: Reproducible and scalable research computing with containers

This is an introduction to containers course at Imperial College London

3 × 2 hour classes

## The Graduate School logo
<img src="/images/grad-school-logo.png">

## Prerequisites

**NOTE: You are expecting to have some (not necessarily to have many or a lot) basic experience with bash commands, and Python or R programming. We will use basic bash command lines, simple coding in Python and R in the course.** 

## Description

Scientific software is often dependent on other packages and libraries (and specific versions of these).  The dependencies are hard to reproduce upon redeployment by collaborators and users who may not share the same system setup.  This creates an unnecessary barrier to sharing many excellent software packages making them less shareable and reproducible.

Containers are an effective solution to this problem. The course teaches the fundamentals of creating, deploying and managing containers. You will learn about the essential commands and scripts, about Docker Hub and Singularity (HPC-friendly solution). The workshop will be delivered through a combination of slides, demonstrations and hands-on practice.

This course consists of 3 parts as follows.

- **Part 1**: Introducing containers, Docker command lines.
  - In this part, you will learn why containers are used, what containers are, and you will get hands-on practice with Docker (the most popular container tool) command lines.

- **Part 2**: Create containers with examples. 
  - In this part, you will get familiar with creating container images in Docker environment. You will learn to compose Dockerfile and build container images through examples including bash, Python and R. You will get hands-on practice as well.

- **Part 3**: Share container images and scale up with singularity.
  - In this part, you will practice how to share your own container images with others using either Docker Hub distribution or other ways. You will also learn how to run images using singularity on high performance computing cluster or cloud.

## On completion of this course you will be able to: 

- Understand the benefits of using containers for research 
- Use the Docker commands and basic Singularity commandsCompose Dockerfiles to build docker containers
- Manage Docker Hub repository
- Share Docker container images
- Interpret common errors and use these to help debug a container
- Use singularity to scale up containers on HPC 

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
We should certainly see people using the same containers on macOS and Windows today.

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

- **Docker Hub**: the Docker Hub is a storage resource and associated website where a vast collection of preexisting container images are documented and stored, and are made available for your use.

### Basic docker commands

1. Once your Docker application is running, open a shell (terminal) window, and run the following command to check that Docker is installed and the command line tools are working correctly. 

     `docker --version`
  
2. A command that checks that the virtual machine host is running is the Docker container list command.

      `docker container ls`

3. One of the simplest Docker container images just allows you to create containers that print a welcome message.

To create and run containers from named Docker images you use the docker run command. Open a shell window if you do not already have one open and try the following docker run invocation. Note that it does not matter what your current working directory is.

`docker run hello-world`
      
Try: 

run the above command again and observe the difference.

* The “digest” is a secure fingerprint (a “hash”) of the particular version of the container image that you now have.

4. List of docker images on your computer

Run:

`docker image ls`
      
5. Remove docker image(s)

If you need to reclaim disk space, you can remove image files. The images and their corresponding containers can start to take up a lot of disk space if you don’t clean them up occasionally. If you want to remove an image, you will need to find out details such as the image ID or name of the repository. 

`docker image rm hello-world`
    
if you get the following error message (example):

`Error response from daemon: conflict: unable to remove repository reference "hello-world" (must force) - container a6d14ab49884 is using its referenced image bf756fb1ae65`



