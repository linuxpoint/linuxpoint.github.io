---
layout: post
title: "Setup a local Kubernetes cluster in FIVE minutes!"
subtitle: "Quick K8s cluster setup with kind and Docker"
date: 2024-08-08
author: "Navneet Nishant"
header-img: "img/post-bg/setup-a-local-kubernetes-cluster-in-five-minutes.jpg"
tags: [Kubernetes, Docker, kind]
header-mask: 0.7
---

### What is *kind*?

This post assumes that you have a basic idea about what Kubernetes is and what it does.

Let's try to setup a local Kubernetes cluster.

Today we are looking at a tool called [***kind***.](https://kind.sigs.k8s.io/)

This tool spins up a Kubernetes cluster on your local machine that you can use for testing/development/experimental purposes. 

If you are looking for a Kubernetes distro for running in production. There are better options available for you in the market.

To get started with ***kind***, we would first need to install ***Docker***. This is a prerequisite as the Nodes for your Kubernetes cluster will be hosted in Docker containers.

If you already have Docker installed on your machine, you can move on to the next step.

### Installing Docker on your machine (the hard way)

There a tons of ways to get Docker on your machine which can be found [**here**.](https://docs.docker.com/engine/install/)

For this post, I am using a Debian/Ubuntu based distro on the local machine. So all commands you will see in this post should work in Debian/Ubuntu based GNU/Linux distros. 

I'll manually download the **.deb** files for Docker and then use **dpkg** to install them.

You will need to download three .deb files to get Docker:

- docker-ce
- docker-ce-cli
- containerd.io


To download the binaries, you need to know a couple of things.

First is your distro's code name.

This is pretty straight forward.

Type the below command in your shell program.

> **$ cat /etc/os-release**

It should give you a output like this.

> ![download-docker](/img/in-post/post-setup-a-local-kubernetes-cluster-in-five-minutes/download-docker.jpg)	

Looking at the output, it is evident that my code name is *jammy*.

Next, we need to find the architecture of your machine.

To check your computer's architecture, run the below command.

> **$ arch**

If output of the *arch* command is *x86_64*. Your's is a 64-bit machine. So arch = amd64.

Now that we know our code-name and arch, we need to go to the the below url.

> **https://download.docker.com/linux/ubuntu/dists/{code-name}/pool/stable/{arch}/**

In our case this will be [https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/](https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/)

This is where we will find our three binaries *docker-ce*, *docker-ce-cli* and *containerd.io*

Download the latest versions of the three and you should be okay.

Now to install the three .deb files that we just downloaded, place the three .deb in a new folder and run the below command.

> **$ dpkg -i /path/to/your/deb/*.deb**

There's one last step to Docker installation and that is to add your linux user to ***docker*** group.

This will enable our user to run docker commands without ***sudo***.

To do this, use the command

> **$ sudo usermod -aG docker $USER**

***OOF!*** That was some work.

### Installing *kind*

Now that we are done with the Docker installation, we can safely move to installation of ***kind***.

Head over to [https://github.com/kubernetes-sigs/kind/releases](https://github.com/kubernetes-sigs/kind/releases) and download the latest binary of ***kind***.

Once downloaded, rename the file to *kind*.

Now to make the binary available for use as a command, first we will make it executable using

> **$ chmod +x ./kind**

And then move it to /usr/local/bin/

> $ **sudo mv ./kind /usr/local/bin/kind**

Now we can test the kind command with

> **$ kind version**

### Installing kubectl

We are almost ready to create our local Kubernetes cluster.

But wait!

There is still a missing piece of the puzzle left to be found.

We have no means to interact with the cluster that we will create.

***kubectl*** comes to our rescue. It is a tool provided by Kubernetes to communicate with any Kubernetes cluster's control plane.

This is similar to what we did for installation of ***kind***.

Download the binary file for kubectl

> **$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"**

Make it executable

> **$ chmod +x ./kubectl**

Move it to /usr/local/bin/ so that it is accessible from anywhere on the terminal.

> **$ sudo mv ./kubectl /usr/local/bin/kubectl**



### Creating a cluster

This is the moment that we have been waiting for. Isn't it?

To create a single node cluster

> **$ kind create cluster --name {cluster-name}**

Let's name our first cluster as ***cluster-1***.

> **$ kind create cluster --name cluster-1**


> ![creating-a-cluster.jpg](/img/in-post/post-setup-a-local-kubernetes-cluster-in-five-minutes/creating-a-cluster.jpg)


### Interacting with the cluster using kubectl

To get some details about the cluster

> **$ kubectl cluster-info**

> ![interact-with-cluster.jpg](/img/in-post/post-setup-a-local-kubernetes-cluster-in-five-minutes/interact-with-cluster.jpg)

To list existing Kubernetes services

> **$ kubectl get services**

With this we were successfully able to create a kubernetes cluster using ***kind*** and ***Docker*** and interact with it using ***kubectl***.




