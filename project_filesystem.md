# Project Filesystem Status

Starting around mid-March, users began reporting multiple issues due to high load on our filesystem that hosts /rcf-proj2 and /rcf-proj3 . This has caused problems for users trying to log in, access files, or run interactive jobs.

We've since stabilized the filesystem but have made it read-only to decrease the load and prevent further file access issues. The filesystem will remain in this state until June when we can deploy a new, previously planned filesystem and migrate all project data.

## /scratch - an all new high-performing parallel file system 

We have brought up the /scratch file system, which is 806TB (compared to 266TB /staging) and IO performance is twice more than /staging. Each user account is limited to 10TB quota on /scratch. If you need more, please contact us.

Your scratch directory is located at:

    /scratch/<username>

### What data should move to scratch?

If your job is IO intensive, you should put your input and output data on /scratch. Your scratch directory is accessible only to you.

### NOTE: /staging is going away soon !

Your staging directory is located at:

    /staging/<proj_name>/<user_name>

because of small capacity of /staging, HPC is planning to replace it with a much bigger and newer file system. So please migrate your data to /scratch at your earliest convenience. 

### Where should I make the data migration?

On hpc-transfer, hpc-transfer is recently upgraded with new hardware and new 40GB links, it can migrate data in and out much faster than login nodes. Please note, regenerating data is much faster than copying around data. So if you have a large custom python or R installtion under /staging, don't copy that but just re-install from scratch under /scratch.

## Backups

There are no backups for either /scratch or /staging. Please keep additional copies of your important data to prevent accidental data loss. (If your PhD thesis relies on your data, keep at least tripple copies.)

## Installing software

Installing software on staging and scratch should be as easy as installing it to your project directory. However, there are some special cases. If you require further assistance, please contact us at hpc@usc.edu.

### Python

Python allows you to install and load packages from arbitrary directories.

Assume you have a directory of the form:

    package_root=/scratch/<username>/python_packages

You can install packages like so:

    pip3 install <package_name> --root=$package_root

To load a package, ensure you have appended your `PYTHONPATH` environment variable like so:

    export PYTHONPATH=${package_root}/lib/python3.7/site-packages:${PYTHONPATH}

### R

R allows you to install and load packages from arbitrary directories.

To install an R package, run a command of the form:

    install.packages("package_name", lib.loc="package_path")

Where `package_path` is the absolute path to where you want your R packages to be installed. It can be something of the form:

    /scratch/<username>/r_packages/3.6

*Note: We recommend keeping packages separated by R version (Major.Minor) to avoid compatibility issues.*

To load an R package, run a command of the form:

    library("package_name",lib.loc="package_path")
