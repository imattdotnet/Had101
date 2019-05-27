Essential HDFS Commands
This lab serves as a quick hands-on guide to the most useful HDFS commands for managing HDFS files from the command line.

Introduction
The Hadoop File System is a distributed file system that is the heart of the storage for Hadoop. There are many ways to interact with HDFS including Hue, Ambari, HDFS Web UI, WebHDFS and the command line. The first way most people interact with HDFS is via the command line tool called hdfs. This is a runner that runs other commands including dfs. This replaces the old Hadoop fs in the newer Hadoop. The HDFS client can be installed on Linux, Windows, and Macintosh and be utilized to access your remote or local Hadoop clusters.

The one universal and fastest way to check things is with the shell or CLI. The following are always helpful and usually hard or slower to do in a graphical interface. Try them out as they are described on your own Hadoop installation.

To List All the Files in the HDFS Root Directory
The most basic command is to get a list of directories from the root. This gives you the lay of the land.

Usage: hdfs dfs [generic options] -ls [-C] [-d] [-h] [-q] [-R] [-t] [-S] [-r] [-u] [<path> ...]
Example:
hdfs dfs -ls /
Found 35 items
drwxrwxrwx   - yarn   hadoop          0 2016-10-06 16:05 /app-logs
drwxrwxrwx   - hdfs   hdfs            0 2016-11-04 11:56 /apps
drwxr-xr-x   - yarn   hadoop          0 2016-09-15 21:02 /ats
drwxrwxrwx   - hdfs   hdfs            0 2016-10-05 21:07 /banking
...
You can choose any path from the root down, just like regular Linux file system. -h shows in human readable sizes, recommended. -R is another great one to drill into subdirectories. Often you won't realize how many files and directories you actually have in HDFS. Many tools including Hive, Spark history and BI tools will create directories and files as logs or for indexing.

Copying Files
There are a few different ways to copy files:

copyFromLocal - Copy the file from Local file system to HDFS
copyToLocal - Copy the file from HDFS to Local File System
put - Copy single source, or multiple sources from local file system to the destination file system
Usage: hdfs dfs -copyFromLocal <localsrc> <hdfs destination>
Command: hdfs dfs –copyFromLocal /home/user/test /new_user

Usage: hdfs dfs -copyToLocal <hdfs source> <localdst>
Command: hdfs dfs –copyToLocal /new_user/test /home/user

Usage: hdfs dfs -put <localsrc> <destination>
Command: hdfs dfs –put /home/user/test /user
Create an Empty File in an HDFS Directory
Usage: hadoop fs [generic options] -touchz <path> ...
Example:
hdfs dfs -touchz /test2/file1.txt
This works the same as Linux touch command. This is useful to initialize a file. Sometimes you want to test a user's permissions and want to quickly do a write. This is the quickest path for you. You can also bulk upload a chunk of files via: hdfs dfs -put *.txt /test1/. The reason I want to do this so I can show you a very interesting command called getmerge.

Concatenate all the Files into a Directory into a Single File
Usage:  hdfs dfs [generic options] -getmerge [-nl] <src> <localdst>
Example:
hdfs dfs -getmerge -nl /test1 file1.txt
This will create a new file on your local directory that contains all the files from a directory and concatenates all them together. The -nl option adds newlines between files. This is often nice when you wish to consolidate a lot of small files into an extract for another system. This is quick and easy and doesn't require using a tool like Apache Flume or Apache NiFi. Of course, for regular production jobs and for larger and greater number of files you will want a more powerful tool like the two mentioned. For a quick extract that someone wants to see in Excel, concatenating a few dozen CSVs from a directory into one file is helpful.

Change the Permissions of a /new-dir
Usage: hdfs dfs [generic options] -chmod [-R] <MODE[,MODE]... | OCTALMODE> PATH...
Example:
hdfs dfs -chmod -R 777 /new-dir
The chmod patterns follow the standard Linux patterns, where 777 gives every user read-write-execute for user-group-other.

Change the Owner and Group of a New Directory: /new-dir
Usage: hdfs dfs [generic options] -chown [-R] [OWNER][:[GROUP]] PATH...
Example:
hdfs dfs -chown -R admin:hadoop /new-dir
Change the ownership of a directory to the admin user and the Hadoop group. You must have permissions to give this to that user and that group. Also, the user and group must exist. For changing permissions, it is best to sudo to the hdfs user which is the root user for HDFS. Linux root user is not the root owner of the HDFS file system.

Delete all the ORC files forever, skipping the temporary trash holding
Usage:  hdfs dfs [generic options] -rm [-f] [-r|-R] [-skipTrash] [-safely] <src> ...
Example:
hdfs dfs -rm -R -f -skipTrash /dir/*.orc
Deleted /dir/a.orc
We want to skipTrash to destroy that file immediately and free up our space, otherwise, it will go to a trash directory and wait for a configured period of time before it was deleted. You use -f to force the deletion.

Move A Directory From Local To HDFS and Delete Local
Usage: hdfs dfs [generic options] -moveFromLocal <localsrc> ... <dst>
Example:
hdfs dfs -moveFromLocal /tmp/tmp2 /tmp2
[hdfs@tspanndev10 /]$ hdfs dfs -ls /tmp2
Found 2 items
-rw-r--r--   3 hdfs hdfs          0 2016-11-18 15:55 /tmp2/a.txt
-rw-r--r--   3 hdfs hdfs          5 2016-11-18 15:55 /tmp2/b.txt
[hdfs@tspanndev10 /]$ ls -lt /tmp/tmp2
ls: cannot access /tmp/tmp2: No such file or directory
If you want to move a local directory up to HDFS and remove the local copy, the command is moveFromLocal.

Show Disk Usage in Megabytes for the Directory: /dir
Usage: hdfs dfs [generic options] -du [-s] [-h] <path> ...
Example:
hdfs dfs -du -s -h /dir
2.1 G  /dir
The -h flag gives you a human readable output of size, for example Gigabytes.

Getting Help
When in doubt of what command you want to use or what to do next, just type help. You will also get a detailed list for each individual command.

hdfs dfs -help
Usage: hadoop fs [generic options]
[-appendToFile <localsrc> ... <dst>]
[-cat [-ignoreCrc] <src> ...]
[-checksum <src> ...]
[-chgrp [-R] GROUP PATH...]
[-chmod [-R] <MODE[,MODE]... | OCTALMODE> PATH...]
[-chown [-R] [OWNER][:[GROUP]] PATH...]
[-copyFromLocal [-f] [-p] [-l] <localsrc> ... <dst>]
[-copyToLocal [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
[-count [-q] [-h] [-v] [-t [<storage type>]] <path> ...]
[-cp [-f] [-p | -p[topax]] <src> ... <dst>]
[-createSnapshot <snapshotDir> [<snapshotName>]]
[-deleteSnapshot <snapshotDir> <snapshotName>]
[-df [-h] [<path> ...]]
[-du [-s] [-h] <path> ...]
[-expunge]
[-find <path> ... <expression> ...]
[-get [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
[-getfacl [-R] <path>]
[-getfattr [-R] {-n name | -d} [-e en] <path>]
[-getmerge [-nl] <src> <localdst>]
[-help [cmd ...]]
[-ls [-C] [-d] [-h] [-q] [-R] [-t] [-S] [-r] [-u] [<path> ...]]
[-mkdir [-p] <path> ...]
[-moveFromLocal <localsrc> ... <dst>]
[-moveToLocal <src> <localdst>]
[-mv <src> ... <dst>]
[-put [-f] [-p] [-l] <localsrc> ... <dst>]
[-renameSnapshot <snapshotDir> <oldName> <newName>]
[-rm [-f] [-r|-R] [-skipTrash] [-safely] <src> ...]
[-rmdir [--ignore-fail-on-non-empty] <dir> ...]
[-setfacl [-R] [{-b|-k} {-m|-x <acl_spec>} <path>]|[--set <acl_spec> <path>]]
[-setfattr {-n name [-v value] | -x name} <path>]
[-setrep [-R] [-w] <rep> <path> ...]
[-stat [format] <path> ...]
[-tail [-f] <file>]
[-test -[defsz] <path>]
[-text [-ignoreCrc] <src> ...]
[-touchz <path> ...]
[-truncate [-w] <length> <path> ...]
[-usage [cmd ...]]
...
You can also use the older format of: hadoop fs. This will work on older Hadoop installations as well.

HDFS DFS Administration
If you are logged in as the hdfs super user, you can also use the HDFS Admin commands.

Usage: hdfs dfsadmin
Note: Administrative commands can only be run as the HDFS superuser.
[-report [-live] [-dead] [-decommissioning]]
[-safemode <enter | leave | get | wait>]
[-saveNamespace]
[-rollEdits]
[-restoreFailedStorage true|false|check]
[-refreshNodes]
[-setQuota <quota> <dirname>...<dirname>]
[-clrQuota <dirname>...<dirname>]
[-setSpaceQuota <quota> [-storageType <storagetype>] <dirname>...<dirname>]
[-clrSpaceQuota [-storageType <storagetype>] <dirname>...<dirname>]
[-finalizeUpgrade]
[-rollingUpgrade [<query|prepare|finalize>]]
[-refreshServiceAcl]
[-refreshUserToGroupsMappings]
[-refreshSuperUserGroupsConfiguration]
[-refreshCallQueue]
[-refresh <host:ipc_port> <key> [arg1..argn]
[-reconfig <namenode|datanode> <host:ipc_port> <start|status|properties>]
[-printTopology]
[-refreshNamenodes datanode_host:ipc_port]
[-deleteBlockPool datanode_host:ipc_port blockpoolId [force]]
[-setBalancerBandwidth <bandwidth in bytes per second>]
[-fetchImage <local directory>]
[-allowSnapshot <snapshotDir>]
[-disallowSnapshot <snapshotDir>]
[-shutdownDatanode <datanode_host:ipc_port> [upgrade]]
[-getDatanodeInfo <datanode_host:ipc_port>]
[-metasave filename]
[-triggerBlockReport [-incremental] <datanode_host:ipc_port>]
[-help [cmd]]
Generic options supported are
-conf <configuration file>     specify an application configuration file
-D <property=value>            use value for given property
-fs <local|namenode:port>      specify a namenode
-jt <local|resourcemanager:port>    specify a ResourceManager
-files <comma separated list of files>    specify comma separated files to be copied to the map reduce cluster
-libjars <comma separated list of jars>    specify comma separated jar files to include in the classpath.
-archives <comma separated list of archives>    specify comma separated archives to be unarchived on the compute machines.
The general command line syntax is
hdfs command [genericOptions] [commandOptions]
There are number of commands that you may need to use for administrating your cluster if you are one of the administrators for your cluster. If you are running your own personal cluster or Sandbox, these are also good to know and try. Do Not Try These In Production if you are not the owner and fully understand the dire consequences of these actions. These commands will be affecting the entire Hadoop cluster distributed file system. You can shutdown data nodes, add quotas to directories for various users and other administrative features.

WARNING: Enter Safemode for Your Cluster
Usage: hdfs dfsadmin [-safemode enter | leave | get | wait | forceExit]
Example:
hdfs dfsadmin -safemode enter
Safe mode is ON
Do not do this unless you need to do cluster maintenance such as adding nodes. You will be entering read-only mode. You need to do safemode leave to get out of this. These commands may take time as they wait for things to write and jobs not accessing the servers.

To Get a Report of Your Cluster
Usage: hdfs dfsadmin -report
Example:
hdfs dfsadmin -report
Configured Capacity: 75149430272 (69.99 GB)
Present Capacity: 55889761113 (52.05 GB)
DFS Remaining: 26116294782 (24.32 GB)
DFS Used: 29773466331 (27.73 GB)
DFS Used%: 53.27%
Under replicated blocks: 1295883
Blocks with corrupt replicas: 0
Missing blocks: 0
Missing blocks (with replication factor 1): 0
-------------------------------------------------
Live datanodes (1):
Name: 111.11.111.11:50010 (dataflowdeveloper.com)
Hostname: dataflowdeveloper.com
Decommission Status : Normal
Configured Capacity: 75149430272 (69.99 GB)
DFS Used: 29773466331 (27.73 GB)
Non DFS Used: 19259669159 (17.94 GB)
DFS Remaining: 26116294782 (24.32 GB)
DFS Used%: 39.62%
DFS Remaining%: 34.75%
Configured Cache Capacity: 0 (0 B)
Cache Used: 0 (0 B)
Cache Remaining: 0 (0 B)
Cache Used%: 100.00%
Cache Remaining%: 0.00%
Xceivers: 8
Last contact: Fri Nov 18 16:28:59 UTC 2016
