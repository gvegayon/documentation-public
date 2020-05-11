# Using the Command Line to Transfer Files to Discovery

Command line interface data-transfer utilities (clients) are available on MacOS and Linux systems, and through applications such as PuTTY and Cygwin on Windows. The common utilities, sftp, rsync and scp, are described on this page.

## SFTP: Secure File Transfer (Command-line-based)

SFTP is a command-line-based transfer tool that is the equivalent of a GUI-based sftp client. Like SSH, SFTP requires an initial login and authentication, and your session will remain open until you exit or are disconnected. You will remain connected to HPC and able to upload(put) and download(get) files without further login or authentication.

From your laptop, login to discovery.usc.edu and authenticate via DUO.
```
sftp ttrojan@discovery.usc.edu
```
If it is your first time logging into the discovery HPC node you will be asked Are you sure you want to continue connecting (yes/no)?. Answer “yes”. You will see the following once you are connected:
```
Connected to discovery.usc.edu.
sftp> 
```
The help command will display the available sftp commands.
```
sftp> help
Available commands:
<lots of options>
```
Use commands like pwd, ls, and cd, and their local equivalents lpwd, lls, and lcd, to navigate to the source and destination directories for file transfer.

### Navigate locally
```
sftp> lpwd
Local working directory: /home1/tommy
sftp> lcd myimages
sftp> lls
myplot1.jpg	myplot2.jpg
```
### Navigate remotely
```
sftp> pwd
Remote working directory: /home1/ttrojan
sftp> cd /scratch/tt/ttrojan/images
```
### Upload file from local to remote computer
```
sftp> put myplot1.jpg myplot.jpg
Uploading myplot1.jpg to /scratch/tt/ttrojan/myplot.jpg
myplot1.jpg                                 100%   10KB   2.4MB/s   00:00    
```
## RSync: Remote Synchronization (Command-line-based)

Rsync is a fast and versatile transfer utility for synchronizing files and directories. It is a good choice for manually transferring large amounts of data on or to HPC because only new or updated files are copied.

The command synchronizes the directory on the local computer, /source/path, with the directory on the remote computer, /destination/path. If /destination/path doesn’t exist, it will be created. Of course, standard Linux file and directory permissions apply.
```
rsync </source/path> <hostname:/destination/path>
```
Note: Unlike SFTP, login and authentication are requested for each use of the command.

The following example copies a file from a local computer to a directory on discovery. The command was typed into the terminal window on a local computer.
```
rsync -av --progress myplot1.jpg ttrojan@discovery.usc.edu:/scratch/ttrojan/
```
### login and authenticate
```
building file list ... 
1 file to consider
myplot1.jpg
       10079 100%    0.00kB/s    0:00:00 (xfer#1, to-check=0/1)

sent 10208 bytes  received 42 bytes  1205.88 bytes/sec
total size is 10079  speedup is 0.98
```
The a option specifies that it will be an archive (and so preserves permissions and modification times, and recursively transfers directories), the v option turns on verbose mode, and the progress option shows progress. Rsync provides many options—see the manual page (“man rsync”) for details.

## SCP: Secure Copy Protocol (Command-line-based)
scp is another convenient way to transfer a single file or directory. The following description assumes that scp is being used to transfer files between your local computer and HPC. Due to security risks, please be mindful of the type of information being transferred. Where possible, omit all information that may be considered confidential. For examples of confidential information that requires additional consideration visit http://itservices.usc.edu/security/sensitive-info.

Note: Unlike SFTP, login and authentication are requested for each use of the command. If you plan to do multiple transfers it is recommended that you use sftp — either the command line version or a GUI-based client, which will keep your session open so that multiple authentications are not required.

You can copy a file(s) from a local directory (/source/path) to a remote directory (/destination/path), and vice-versa.
```
scp </source/path> <hostname:/destination/path>

scp <hostname:/source/path> </destination/path>
```
For example:
```
scp myplot1.jpg ttrojan@discovery.usc.edu:/scratch/ttrojan

scp myplot1.jpg myplot2.jpg ttrojan@discovery.usc.edu:/scratch/ttrojan

scp *.jpg ttrojan@discovery.usc.edu:/scratch/ttrojan
```
Similar to the copy (cp) command, you can copy whole directories using the -r option. The source directory will be copied (created) too.
```
scp -r ./myplots ttrojan@discovery.usc.edu:/scratch/ttrojan

scp -r ttrojan@discovery.usc.edu:/scratch/ttrojan/myplots .
```
The a option specifies that it will be an archive (and so preserves permissions and modification times, and recursively transfers directories), the v option turns on verbose mode, and the progress option shows progress. Rsync provides many options—see the manual page ("man rsync") for details.


			
			
