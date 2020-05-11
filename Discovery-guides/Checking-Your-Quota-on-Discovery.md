## Checking Your Account Storage Quota on the Discovery HPC Login Node

The command "lfs" provides information on storage you have available in your home directory. After logging in, run it like this:

```-bash-4.2$ /usr/bin/lfs quota -h -u christay /home1```

It will produce something similar to the following:

```
Disk quotas for usr christay (uid 375879):
     Filesystem    used   quota   limit   grace   files   quota   limit   grace
         /home1  3.547M    100G    101G       -      62  307200  312320       -
uid 375879 is using default block quota setting
uid 375879 is using default file quota setting
```

In this example, I'm using about 3.5M of my allotted 100G, and 62 out of the 307200 files I can store. These are soft limits. If you exceed the soft limit your account will still function normally, you should take actions to delete files before you exceed the hard limit, at which point your account will not function normally.

The /scratch filesystem is available for temporary large files, datasets and intensive data IO, and there's no quota on /scratch. Be sure to copy and back up your job results or any important data from /scratch as there's no backup for /scratch. You should also not use /scrach for long term data storage.

