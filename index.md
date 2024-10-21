# How to Build and Submit your First CUDA Job using PACE

## Table of Contents
1. [Introduction](#introduction)
2. [Logging onto PACE](#logging-onto-pace)
3. [SSH](#ssh)
   - [Login](#login)
   - [Interactive Jobs](#interactive-jobs)
     - [Requesting a Node](#requesting-a-node)
   - [Batch Jobs](#batch-jobs)
     - [Writing a batch script](#writing-a-batch-script)
4. [Loading Modules](#loading-modules)
5. [Building and Running Programs](#building-and-running-programs)
6. [OnDemand](#ondemand)
   - [Accessing OnDemand](#accessing-ondemand)
   - [Interactive Apps](#interactive-apps)

## Introduction

Congratulations, you have just been granted access to Georgia Tech’s compute resources! 🎉

Welcome to your first step in submitting a job using Georgia Tech’s **Partnership for an Advanced Computing Environment (PACE)**! Whether you’re accessing PACE through **SSH** or the web-based **OnDemand** platform, the following guide will walk you through everything you need to know to successfully run your first job on PACE. In this tutorial, you’ll learn how to log into PACE, either through SSH or the OnDemand interface, and how to request compute resources. You’ll also discover how to load the necessary modules necessary to run your programs, so you’ll be fully prepared to execute your job on the cluster. Along the way, we’ll cover writing and submitting batch jobs using SLURM, as well as exploring the OnDemand interface for a more intuitive way to utilize compute resources through programs like RStudio, MATLAB, and Jupyter Notebooks. By the end of this guide, you’ll be fully equipped to build and submit your first CUDA job on PACE. Whether you’re a seasoned command-line user or a beginner, we’ve got you covered. Let’s dive in and get started! 🎯

## Logging onto PACE

There are two ways to log onto a PACE cluster, either through SSH or through Georgia Tech’s OnDemand instance (ice, phoenix) for your desired cluster. To access your cluster through either of these channels, **you will first need to log onto the campus VPN**.

## SSH
**SSH (Secure Shell)** is a fast and light-weight means of accessing the PACE compute recourses. As a command line interface, SSH access does not provide users with a graphical interface. However unlike OnDemand, it enables direct file transfer, batch job scripting, and better network resource efficiency. 

### Login 

To login via SSH, open a new terminal and type the following command, replacing `(your_gt_username)` and `(cluster_url)` with your Georgia Tech username and the cluster you're trying to access:

```
ssh (your_gt_username)@login-(cluster_url).pace.gatech.edu
```

Upon logging in you should be prompted for a password, type your GT password and press enter to login. 

> **Note:** When typing passwords in Linux and Windows systems, the command line will be blank and will not show characters as you press them.

Once logged in, you should receive a prompt acknowledging your access to a Georgia Institute of Technology system and terms of use. Although it may vary from cluster to cluster login text may look similar to this and signifies successful access to the cluster:

<div align="center">
  <img src="https://gatech.service-now.com/ssh_success.pngx" alt="Login Prompt" />
</div>


### Interactive Jobs

Interactive jobs on PACE are jobs that allow you to actively engage with the compute node in real-time, similar to how you would work on your local machine. Instead of submitting a script for batch processing and waiting for results, interactive jobs give you direct, live access to a compute node.

#### Requesting a Node

When you first log into the cluster, you will be interacting with the “login” node. Your shell prompt should look something like this, where the word “login” is visible in the prompt:

```bash
[(your_gt_username)@login-(cluster_url).pace.gatech.edu] $
```


This node is the shared access point for everyone using the cluster. **Do not build or run demanding programs on this node.** This slows down the experience for everyone involved. To gain access to a free node with available compute resources, you will need to submit a request using SLURM.

Our goal for this tutorial is to build and run a CUDA program that runs on one NVIDIA GPU, so we want to request the first available node that has an NVIDIA V100 available.

> **Sidenote:** It is possible to request multiple nodes or multiple GPUs, if your job demands it.

To request a node with a GPU, run:

```
salloc --gres=gpu:V100:1 -t1:00:00
```

This command requests any available node with one available V100 and will reserve it for you for one hour. If everything went well, you should see a message:

```salloc: Pending job allocation [job number]```

```salloc: job [job number] queued and waiting for resources```

The cluster may be busy at the moment you submit the job, so be ready to wait a few minutes before canceling the request. If you find the scheduler is taking too long, press the `CTRL` and `C` keys at the same time to go back to the login node.

Once you have access, you should see a prompt (without the word “login”), meaning you now have the node!

For any reason if you may need to cancel a job, you can run:

```scancel <job number>```


To find the job number, run `squeue`.

To exit interactive mode prior to the allotted session time, run `exit`.

### Batch Jobs

Batch jobs on PACE are jobs that allow you to submit one or several scripts to the cluster at a given time. This enables automated requests for compute resources and running of programs simultaneously across several compute nodes.

#### Writing a batch script

(Instructions on writing a batch script)

## Loading Modules

To make dependency management easier, PACE clusters use a tool called **Lua Modules**, where each “module” corresponds to a package. To build the attached CUDA program, we will first need to load the correct modules.

## Building and Running Programs

(Instructions on building and running your CUDA program)

## OnDemand

### Accessing OnDemand

Georgia Tech’s **Open OnDemand** is a web-based interface for accessing the PACE clusters. It provides an easier alternative to the command line for running jobs, submitting batch scripts, and accessing files on the cluster. OnDemand is available for both the Phoenix, ICE, and Hive clusters, and it can be accessed through the following URLs:

- **Phoenix**: [https://phoenix-ondemand.pace.gatech.edu](https://ondemand-phoenix.pace.gatech.edu/)
- **ICE**: [https://ondemand-ice.pace.gatech.edu](https://ondemand-ice.pace.gatech.edu/)
- **Hive**: [https://ondemand-hive.pace.gatech.edu/](https://ondemand-hive.pace.gatech.edu/)

You must be connected to the **Georgia Tech VPN** to access these interfaces.

Once logged in, you will be able to:

- Launch **interactive apps** such as terminal sessions, Jupyter notebooks, or MATLAB GUIs without having to configure your environment manually.
- Submit and monitor batch jobs through a graphical interface instead of using command-line SLURM.

### Interactive Apps

1. **Login** to the appropriate OnDemand URL for your cluster.
2. Navigate to the **My Interactive Sessions** tab.
3. Select your interactive app of choice.
4. Fill in the required details such as resources needed (e.g., GPUs, memory) and run time.
5. Once you’re ready, click **Submit**. You will then be able to access the given app and compute resources from within the browser.



