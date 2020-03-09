# Discovery User Guide

## Introduction
Thank you for volunteering to try out our new test cluster, discovery.usc.edu.  Almost every aspect of this system is different from the current one. Your feedback will be invaluable to helping strengthen it and improve the experience for the rest of the community when it goes into production.
## Getting Help
Please direct any questions regarding the test system to hpc-support@usc.edu. All emails sent here will be sent to a separate ticketing system. You are also encouraged to provide feedback on the test ticketing system as we will be using it when we launch the new cluster.

## Logging in
You can access the test cluster at the hostname `discovery.usc.edu`. Authentication should be the same as `hpc-login3.usc.edu`.

If you encounter issues connecting to `discovery` while off campus, it may be necessary to first connect to a VPN. Please see this page for information on setting it up: https://itservices.usc.edu/vpn/.

## Storing data
Every user will have access to 3 different storage locations, home, proj, and scratch.

###  Home directory
Your home directory is of the form:

    /home1/<user_name>

/home1 is a high-performance parallel file system, you can run IO intensive jobs directly from `/home1`. Quota for each user on `/home1` is
100GB and 300k files. We keep 14 days of snapshots of `/home1` which can be think of as semi-backups, meaning if you accidentally deleted some data we are able to recover it within 14 days. You should always keep extra backups of your important
data since snapshots or semi-backups are not real backups.

To check your current usage of quota on /home1 use:
```
lfs quota -h -u $USER /home1
```
this check is also included in /usr/local/bin/myquota on `discovery.usc.edu`

###  Current Project directory (Read only)
To make it easier to start testing, your project directory is available on a read-only basis to `discovery.usc.edu`

###  Scratch directory
Your can find your scratch directory at

    /scratch/<user_name>

/scratch is a high-performance parallel file system, you can run IO intensive jobs directly from /scratch. No quota is currently enforced
on /scratch. There's no backup of any sort on /scratch.

### Storage Summary
|PATH|Quota|No. File quota|High Performance IO|
|-|-|-|-|
|/home1|100GB|300k|Yes|
|/scratch|none|none|Yes|

### Transfering data
You can use `discovery.usc.edu` directly as data transfer node. scp and sftp are recommended.

## Software Modules (lmod)
One of the biggest user-facing changes to the test cluster is trading the setup.sh method of using software for the module system. Lmod has the ability to track software dependencies to ensure you have better control of your environment. (No more logging out and then back in to reset your environment!)

The official documentation for Lmod can be found here: https://lmod.readthedocs.io/en/latest/010_user.html

Here are some of the basics.

###  Checking available software
To see what modules you can load into your environment, run the command `module avail`. You should see something similar to:

    --------/spack/apps/lmod/linux-centos7-x86_64/Core --------
    gcc/4.9.4     gcc/9.2.0    (D)    intel/19.0.4 (D)
    gcc/8.3.0    intel/18.0.4        usc

    Where:
    D:  Default Module

    Use "module spider" to find all possible modules and extensions.
    Use "module keyword key1 key2 ..." to search for all possible modules
    matching any of the "keys".


If you have a fresh environment only `Core` modules are available. These are usually compilers but can be applications like Matlab that are prebuilt and have no dependencies.
###  Loading/Unloading software

Typically loading modules is as simple as typing `module load <software_name>`. `<software_name>` must be visible when you run module avail.

By running

    module load <software_name>

Lmod will set your environment such that <software_name> will be placed in your path. To see what environment variables get set, you can run:

    module show <software_name>

If there are multiple versions of `<software_name>` you can specify a version like so:

    module load <software_name>/<version>

For example to load gcc version 8.3.0:
```
   $ module load gcc/8.3.0
```
If no version is specified:
```
   $ module load gcc
```
the default version will be loaded. The default version is indicated with a (D) next to it.


To unload a specific module you can run:

```
    module unload <software>
```

To totally clear your environment you can run:

```
    module purge
```


###  Finding software
The first time you log in and run `module avail`, it may not seem like much software is available. This is actually a safety feature that prevents you from loading incompatible modules. If you would like to explore the software tree, you can start loading modules and new ones will unlock. To see all applications built with a certain compiler, you can load that module. Everything built with that compiler will become visible in module avail.

The current naming scheme for modules is `<software_name>/<version>-xxxx`.

Where `xxxx` is a 4 character hash (random looking characters). We plan to remove the hash from the module naming scheme in the production environment.

#### Module spider
If you know the name of a software package you can use the `module spider` command to find out if it exist and how to load it.

For example:
```
  $ module spider samtools

    ------------------------------------------------------
    samtools: samtools/1.9-rq3g
    ------------------------------------------------------

        You will need to load all module(s) on any one of the lines below before the "samtools/1.9-rq3g" module is available to load.

        gcc/8.3.0

        Help:
        SAM Tools provide various utilities for manipulating alignments in the
        SAM format, including sorting, merging, indexing and generating
        alignments in a per-position format
```

### Environment management

One of the nicest features of Lmod is that it can track software dependencies automatically. For example lets say you want to use the `jellyfish` package compiled with `gcc/8.3.0`. You would load like like so:

```
module load gcc/8.3.0
module load jellyfish
```

If for some reason you need to use the `intel` compiler set, you can use the `module swap` command like so:

```
module swap gcc intel

The following have been reloaded with a version change:
  1) jellyfish/2.2.7-oauz => jellyfish/2.3.0-2hxl
```

Lmod automatically changes the `jellyfish` module to one that was compiled with `intel`.

## Running jobs
The new system will continue to use the SLURM job scheduler so it shouldn't be necessary to make changes to your job scripts (unless you request specific partitions or hardware not available in the test cluster).

As a reminder, you can find our documenation on Slurm here:
https://hpcc.usc.edu/support/documentation/slurm/

Using the job scheduler should be the same but there are few user-facing changes of note:

* Max wall-time of 2 days in `main` partition
* Each user can use 96 CPU cores in `main` partition regardless of number of jobs
* Each user can submit 2000 jobs in `main` partition
* If you don't specify any partition, it will default to `main`
* New `oneweek` partition for runs of up to 1 week, each user can use 12 CPU cores regardless of number of jobs and submit 50 jobs.

### Resources and limits summary

|partition|Nodes|CPUS|RAM(GB)|MaxCPUsPerUser|MaxSubmitJobsPerUser|
|---|---|---|---|---|---|
|main |11 | 24|95 |96|2000|
|oneweek |2 | 24|95 |12|50|

### Example Job script

For your convenience, here's a example job script for a serial running job:

```
#!/bin/bash
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=8
#SBATCH --mem-per-cpu=2GB
#SBATCH --time=1:00:00

module purge
module load gcc/8.3.0
module load python

python3 script.py
```

for a parallel running job:

```
#!/bin/bash
#SBATCH --ntasks=64
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=2GB
#SBATCH --time=1:00:00

module purge
module load usc  
srun ./mpi_hello.out
```
