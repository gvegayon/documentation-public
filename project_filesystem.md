# Project Filesystem Status

Starting around mid-March, users began reporting multiple issues due to high load on our filesystem that hosts /rcf-proj2 and /rcf-proj3 . This has caused problems for users trying to log in, access files, or run interactive jobs.

We've since stabilized the filesystem but have made it read-only to decrease the load and prevent further file access issues. The filesystem will remain in this state until June when we can deploy a new, previously planned filesystem and migrate all project data.

**In the meantime, we are requesting that users use the /staging filesystem for their production runs.**

# How to use staging

Your staging directory is located at:

    /staging/<proj_name>/<user_name>

Users involved in multiple projects will have multiple staging directories they can write to, in the same way that they have different project directories for the different projects they belong to.

## What data should move to staging?
Staging has much less available disk space than the project filesystem, so please **only copy necessary files to your staging directory**. In general, if it doesn't need to be edited or updated, keep it in your read-only project directory.

Most programs have a method to use input files even if they aren't in the working directory. If you need help getting your program to read files from your project directory and write out in your staging directory, please contact us at hpc@usc.edu

## Backups

Staging has always been intended as a temporary filesystem and there is currently **no backup process** to protect data in the event of accidental deletion or hardware failure. We have disabled the automatic data purging policy on /staging so your files will not be automatically deleted, but please **copy important files out of staging** as soon as possible to protect them.

# Installing software

Installing software on staging should be as easy as installing it to your project directory. However, there are some special cases. If you require further assistance, please contact us at hpc@usc.edu.

## Python

Python allows you to install and load packages from arbitrary directories.

Assume you have a directory of the form:

    package_root=/staging/<proj_name>/<user_name>/python_packages

You can install packages like so:

    pip3 install <package_name> --root=$package_root

To load a package, ensure you have appended your `PYTHONPATH` environment variable like so:

    export PYTHONPATH=${package_root}/lib/python3.7/site-packages:${PYTHONPATH}

## R

R allows you to install and load packages from arbitrary directories.

To install an R package, run a command of the form:

    install.packages("package_name", lib.loc="package_path")

Where `package_path` is the absolute path to where you want your R packages to be installed. It can be something of the form:

    /staging/<proj_name>/<user_name>/r_packages/3.6

*Note: We recommend keeping packages separated by R version (Major.Minor) to avoid compatibility issues.*

To load an R package, run a command of the form:

    library("package_name",lib.loc="package_path")
