# Project Filesystem Status

Starting around mid-March, users began reporting multiple issues due to high load on our filesystem that hosts /rcf-proj2 and /rcf-proj3. This has caused problems for users trying to log in, access files, or run interactive jobs.

We have since stabilized the filesystem but have made it read-only to decrease the load and prevent further file access issues. The filesystem will remain in this state until June when we can deploy a new, previously planned filesystem and migrate all project data.

Previously, users were instructed to run their jobs in the /staging filesystem until our new project filesystem was up and running. However, /staging nearly reached its limited capacity within a few weeks. As a result, we have created a new, high-performing parallel filesystem for production runs called **/scratch**, which has a much larger capacity than /staging.

## /scratch: an all new, high-performing parallel filesystem

Your scratch directory is located at:

    /scratch/<username>

You should now be using /scratch for your production runs instead of /staging.

The new /scratch filesystem has a capacity of 806TB (compared to 266TB on /staging) and it can sustain 25GB/s of reading and writing performance, which is almost twice as much as /staging.

Your scratch directory is accessible only to you, and each user account is limited to a 10TB quota. If you need more space than this, please contact us at hpc@usc.edu.

## /staging: soon to be decommissioned

Your staging directory is located at:

    /staging/<proj_name>/<user_name>

We are preparing to replace the current /staging filesystem with another 1PB high-performing parallel file system in the next few weeks. Once the upgraded file system comes up, each user will receive another 10-20TB quota.

In order to upgrade the system, we ask that you **migrate your data on /staging to /scratch** as soon as possible.

### How should I perform the data migration from /staging to /scratch?

Migrate data on the hpc-transfer node, which has been recently upgraded with new hardware and new 40GB links. It can migrate data in and out much faster than the login nodes. Instructions on how to transfer files using hpc-transfer can be found here: https://github.com/uschpc/documentation-public/blob/master/hpc-transfer-guide.md

Please note: regenerating data is much faster than copying data. For example, if you have a large custom Python or R installation under /staging, simply re-install under /scratch rather than copying.

## Backups

There are no backups for either /scratch or /staging. Please keep additional copies of your important data elsewhere to prevent accidental data loss. (If your PhD thesis relies on your data, keep at least three copies.)

## Installing software

Installing software on /scratch should be as easy as installing it to your project directory. However, there are some special cases where it is more difficult. If you require further assistance, please contact us at hpc@usc.edu.

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

    library("package_name", lib.loc="package_path")
