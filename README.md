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
  - Try this first, although it won’t work with Windows 10 Home Edition. Install the [Docker Desktop (Windows)](https://hub.docker.com/editions/community/docker-ce-desktop-windows),


  or failing that;

  - Install the [Docker Toolbox (Windows)](https://docs.docker.com/toolbox/toolbox_install_windows/).
  
- Apple macOS, either:
  - Try this first, although it will not work with older versions of macOS. Install the [Docker Desktop (Mac)](https://hub.docker.com/editions/community/docker-ce-desktop-mac),
  or failing that:

  - Install the [Docker Toolbox (Mac)](https://docs.docker.com/toolbox/toolbox_install_mac/).
  
- Linux: there are too many varieties of Linux to give precise instructions here, but hopefully you can locate documentation for getting Docker installed on your Linux distribution. It may already be installed. If it is not already installed on your system, see:
  - [Docker Engine on CentOS](https://docs.docker.com/install/linux/docker-ce/centos/)
  - [Docker Engine on Debian](https://docs.docker.com/install/linux/docker-ce/debian/)
  - [Docker Engine on Fedora](https://docs.docker.com/install/linux/docker-ce/fedora/)
  - [Docker Engine on Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
  - Note that on Linux, you will either need to run Docker commands as root using `sudo` or [create a docker group to run without sudo](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user).
