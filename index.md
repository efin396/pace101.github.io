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
   
---

## Introduction

Congratulations, you have just been granted access to Georgia Tech’s compute resources! 🎉

Welcome to your first step in submitting a job using Georgia Tech’s **Partnership for an Advanced Computing Environment (PACE)**! In this tutorial, you’ll learn how to log into PACE, either through SSH or the OnDemand interface, and how to request compute resources.By the end of this guide, you’ll be fully equipped to build and submit your first CUDA job on PACE. Whether you're an experienced programmer or novice, this guide is tailored to help you take your first steps on PACE. Please note that this guide assumes a basic understanding of programming.

## Prerequisites

Before starting this guide, please ensure that:
- You have been granted access to a PACE cluster by Georgia Tech.
- You are are familar with a programming langauge.

## Safety and Etiquette Guidelines

Here are a few essential safety and etiquette guidelines for using PACE compute resources:

1. **Avoid Heavy Processing on Login Nodes**: The login nodes are shared access points, so avoid running compute-intensive programs here, as it slows down the experience for everyone. Use compute nodes instead by submitting jobs or starting interactive sessions.

2. **Monitor Resource Usage**: Only request the amount of compute resources (CPUs, GPUs, memory) you need. Overestimating can delay your job and those of other users. For test runs, start with small allocations, then scale up as necessary.

3. **Data Security**: Do not store sensitive or personally identifiable information (PII) on PACE unless you have permission and are following Georgia Tech’s data security guidelines. 

4. **Save Your Work**: Regularly back up your data, as storage on compute nodes is not permanent. Use your allocated storage for active work, but move completed projects to more permanent storage solutions.

5. **Cancel Idle Jobs**: If you no longer need a job, cancel it to free up resources for other users. You can use the `scancel` command for this.

6. **Abide by Usage Policies**: Georgia Tech has specific policies for using PACE resources, including respecting the intended use for research and academic purposes.

## Overview of PACE Resources

If you are unfamiliar with PACE, here is a brief overview of the types of clusters that are available. Each of the clusters operate independently and have separate access permissions.

1. _**Phoenix Cluster:**_
The Phoenix Cluster is the flagship compute cluster of PACE. It ranked #463 in the Top500 supercomputer ranking in November 2022. A variety of compute nodes are available, some of which include access to NVIDIA H100, A100, L40, and RTX-6000 GPUs, with Intel Xeon Gold CPUs. There are also 5 petabytes (PB) of project storage and 1.5 PB of scratch storage. Over 1300 nodes available in total. This cluster excels at general high-performance computing and AI workloads.

2. _**ICE Cluster:**_
The ICE (Instructional Cluster Environment) Cluster is tailored for instructional use by certain courses. Like the other clusters, there are CPU and GPU nodes available by request.

3. _**HIVE Cluster:**_
The HIVE Cluster is a smaller cluster with a similar blend of CPU-only and GPU-enabled compute nodes. The cluster has 16 GPU nodes, each with 4 NVIDIA V100 GPUs.

---

## Logging onto PACE
There are two ways to log onto a PACE cluster, either through SSH or through Georgia Tech’s OnDemand instance (ice, phoenix) for your desired cluster. To access your cluster through either of these channels, **you will first need to log onto the campus VPN**.

## OnDemand

Georgia Tech’s **Open OnDemand** is a web-based interface for accessing the PACE clusters. It provides an easier alternative to the command line for running jobs, submitting batch scripts, and accessing files on the cluster. For those interested in accessing PACE compute resources through SSH, please continue to the [SSH](#ssh) section.

### Accessing OnDemand

OnDemand is available for both the Phoenix, ICE, and Hive clusters, and it can be accessed through the following URLs:

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

---

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

### Module Loading and Dependency Management

To make dependency management easier, PACE clusters use a tool called **Lua Modules**, where each “module” corresponds to a package. To build the attached CUDA program, we will first need to load the correct modules. On most GT clusters, the `nvhpc` module will load everything you need to build a CUDA program.

To load a list of desired modules, run:

```bash
module load <module1> <module2> ...
```

For our CUDA program, the easiest option is to load the `nvhpc` module. If everything went well, you will either see no message or a message of the form:

```plaintext
Lmod is automatically replacing "gcc/12.3.0" with "nvhpc-nvc/24.5".

Inactive Modules:
  1) libiconv/1.17 	2) libpciaccess/0.17 	3) libxml2/2.10.3 	4) mvapich2 	
  5) pmix/4.2.6 	6) slurm/23.11.1 	7) xz/5.4.1 	8) zlib/1.2.13
```

This log shows that two modules (`gcc/12.3.0` and `nvhpc-nvc/24.5`) are mutually exclusive, so it disabled `gcc`. Now that you have the appropriate modules loaded, you should be able to build your CUDA program!

If your project requires other dependencies, type `module list` to see a full list of all available modules on the system, and `module spider <module name>` to check if a certain module is on the system.

### Building and Running

1. Make a new directory for your CUDA project’s source code.
2. An example program which multiplies two matrices can be found here: [Example Program](https://github.com/CoffeeBeforeArch/cuda_programming/blob/master/02_matrix_mul/baseline/mmul.cu).

To build this program, run:

```bash
nvcc matmul.cu
```

If you are building a project with multiple source files, pass all of them to `nvcc`.

---

### SSH - Batch Jobs

Batch jobs on PACE are jobs that allow you to submit one or several scripts to the cluster at a given time. This enables automated requests for compute resources and running of programs simultaneously across several compute nodes.

For jobs you wish to perform multiple times, we suggest you write a batch script. A batch script is a shell script with additional comments at the top of the file that specify the arguments to SLURM.

#### Writing a Batch Script

A batch script must begin with `#!/bin/bash`. It must also include the following lines:

- **Tracking account**: This is the project supervisor for a job run. The account username will have the form `gts-<username>`, with the username being for the PI.

  ```bash
  #SBATCH --account=gts-gburdell3
  ```

- **Job partition**: Each partition has a certain hardware spec, which you can find [here](https://gatech.service-now.com/home?id=kb_article_view&sysparm_article=KB0042005). The default `phoenix` partition is provided based on your charge account and the most significant resources requested.

  ```bash
  #SBATCH -p phoenix
  ```

Additional options can be found [here](https://gatech.service-now.com/home?id=kb_article_view&sysparm_article=KB0042003).

For the purpose of this guide, we suggest creating two batch scripts – one to build the program and one to run it.

### Conclusion
This concludes our guide on how to write and run a PACE CUDA job on ICE or Phoenix clusters. If any issues arise, here are a list of potential resources:

- [PACE Consulting Sessions](https://gatech.service-now.com/home?id=kb_article_view&sysparm_article=KB0042280)
- [PACE Training Sessions](https://gatech.service-now.com/home?id=kb_article_view&sysparm_article=KB0042298)
- [Other Pace Documentation Resources](https://gatech.service-now.com/technology?id=kb_article_view&sysparm_article=KB0042503)

## References
TODO: Convert to APA format
https://gatech.service-now.com/home?id=kb_article_view&sysparm_article=KB0041976

https://gatech.service-now.com/home?id=kb_article_view&sysparm_article=KB0041969

https://gatech.service-now.com/home?id=kb_article_view&sysparm_article=KB0041998

https://gatech.service-now.com/home?id=kb_article_view&sysparm_article=KB0041998

https://arnon.dk/matching-sm-architectures-arch-and-gencode-for-various-nvidia-cards/
(For the `sm` argument)



