# Basic Navigation

In this lab, we'll learn the basics of moving around the system. Many tasks rely on being able to get to, or reference the correct location in the system. As such, this stuff really forms the foundation of being able to work effectively in Linux. Make sure you understand it well.

> The output of the commands in this lab may be different, depending on the system you are executing them.

## So where are we?

The first command we are going to learn is pwd which stands for Print Working Directory. (You'll find that a lof of commands in linux are named as an abbreviation of a word or words describing them. This makes it easier to remember them.) The command does just that. It tells you what your current or present working directory is. Give it a try now.

```
[hdfs@sandbox ~]$ pwd
/home/hdfs
[hdfs@sandbox ~]$

```

A lot of commands on the terminal will rely on you being in the right location. As you're moving around, it can be easy to lose track of where you are at. Make use of this command often so as to remind yourself where you presently are.

## What's in Our Current Location?

It's one thing to know where we are. Next we'll want to know what is there. The command for this task is ls. It's short for list. Let's give it a go.

```
[hdfs@sandbox ~]$ ls
example_directory  pig_1490221969660.log
[hdfs@sandbox ~]$

```

Whereas pwd is just run by itself with no arguments, ls is a little more powerful. We have run it here with no arguments in which case it will just do a plain listing of our current location. We can do more with ls however. Below is an outline of it's usage:

```
ls [options] [location]

```

In the above example, the square brackets ( [ ] ) mean that those items are optional, we may run the command with or without them. In the terminal below I have run `ls` in a few different ways to demonstrate.

```
[hdfs@sandbox ~]$ ls
pig_1490221969660.log
[hdfs@sandbox ~]$ mkdir example_directory
[hdfs@sandbox ~]$ ls
example_directory  pig_1490221969660.log
[hdfs@sandbox ~]$ ls -l
total 8
drwxr-xr-x 2 hdfs hadoop 4096 Mar 23 22:10 example_directory
-rw-r--r-- 1 hdfs hadoop 2615 Mar 22 22:32 pig_1490221969660.log
[hdfs@sandbox ~]$ ls /etc
adjtime                   DIR_COLORS               hosts.deny      mime.types      puppet            skel
adjtime.rpmsave           DIR_COLORS.256color      hst             modprobe.d      ranger            slider
aliases                   DIR_COLORS.lightbgcolor  httpd           motd            ranger-admin      spark
alternatives              drirc                    hue             mtab            ranger-tagsync    spark2
ambari-agent              environment              idmapd.conf     my.cnf          ranger-usersync   sqoop
ambari-infra-solr         ethers                   init            my.cnf.d        rc                ssh
...
[hdfs@sandbox ~]$ ls -l /etc
total 1520
-rw-r--r--  1 root     root         12 Jul 12  2016 adjtime
-rw-r--r--  2 root     root         16 Jun  2  2016 adjtime.rpmsave
-rw-r--r--  2 root     root       1512 Jan 12  2010 aliases
drwxr-xr-x  2 root     root       4096 Oct 25 08:00 alternatives
drwxr-xr-x  3 root     root       4096 Oct 25 07:24 ambari-agent
drwxr-xr-x  3 root     root       4096 Oct 25 07:46 ambari-infra-solr
drwxr-xr-x  3 root     root       4096 Oct 25 07:38 ambari-metrics-collector
drwxr-xr-x  3 root     root       4096 Oct 25 07:39 ambari-metrics-grafana
drwxr-xr-x  3 root     root       4096 Oct 25 07:38 ambari-metrics-monitor
drwxr-xr-x  1 root     root       4096 Oct 25 07:23 ambari-server
drwxr-xr-x  3 root     root       4096 Oct 25 07:38 ams-hbase
-rw-------  1 root     root        541 Aug 23  2016 anacrontab
-rw-r--r--  1 root     root        148 Jan 12  2016 asound.conf
-rw-r--r--  1 root     root          1 Feb 19  2015 at.deny
...
[hdfs@sandbox ~]$

```

Let's break it down:

-   `ls` - We ran ls in it's most basic form. It listed the contents of our current directory.
-   `ls -l` - We ran ls with a single command line option ( -l ) which indicates we are going to do a long listing. A long listing has the following:
    -   First character indicates whether it is a normal file ( - ) or directory ( d )
    -   Next 9 characters are permissions for the file or directory
    -   The next file is the number of blocks (don't worry too much about this).
    -   The next field is the owner of the file or directory (hdfs in this case).
    -   The next field is the group the file or directory belongs to (hadoop in this case).
    -   Following this is the file size.
    -   Next up is the file modification time.
    -   Finally we have the actual name of the file or directory.
-   `ls /etc` - We ran ls with a command line argument ( /etc ). When we do this it tells ls not to list our current directory but instead to list that directories contents.
-   `ls -l /etc` - We ran ls with both a command line option and argument. As such it did a long listing of the directory /etc.
-   The `...` just indicate that I have cut out some of the commands normal output for brevities sake. When you run the commands you will see a longer listing of files and directories.

## Paths

In the previous commands we started touching on something called a path. I would like to go into more detail on them now as they are important in being proficient with Linux. Whenever we refer to either a file or directory on the command line, we are in fact referring to a path. ie. A path is a means to get to a particular file or directory on the system.

### Absolute and Relative Paths

There are 2 types of paths we can use, **absolute** and **relative**. Whenever we refer to a file or directory we are using one of these paths. Whenever we refer to a file or directory, we can, in fact, use either type of path (either way, the system will still be directed to the same location).

To begin with, we have to understand that the file system under linux is a hierarchical structure. At the very top of the structure is what's called the **root** directory. It is denoted by a single slash ( / ). It has subdirectories, they have subdirectories and so on. Files may reside in any of these directories.

Absolute paths specify a location (file or directory) in relation to the root directory. You can identify them easily as they always begin with a forward slash ( / )

Relative paths specify a location (file or directory) in relation to where we currently are in the system. They will not begin with a slash.

Here's an example to illustrate:

```
[hdfs@sandbox ~]$ pwd
/home/hdfs
[hdfs@sandbox ~]$ ls example_directory/
file1.txt  file2.txt
[hdfs@sandbox ~]$ ls /home/hdfs/example_directory/
file1.txt  file2.txt
[hdfs@sandbox ~]$

```

-   `pwd` - We ran pwd just to verify where we currently are.
-   `ls example_directory/` - We ran ls providing it with a relative path. Documents is a directory in our current location. This command could produce different results depending on where we are. If we had another user on the system, bob, and we ran the command when in their home directory then we would list the contents of their Documents directory instead.
-   `ls /home/hdfs/example_directory/` - We ran ls providing it with an absolute path. This command will provide the same output regardless of our current location when we run it.

### More on Paths

You'll find that a lot of stuff in Linux can be achieved in several different ways. Paths are no different. Here are some more building blocks you may use to help build your paths.

-   ~ (tilde) - This is a shortcut for your home directory. eg, if your home directory is /home/hdfs then you could refer to the directory Documents with the path /home/hdfs/Documents or ~/Documents
-   . (dot) - This is a reference to your current directory. eg in the example above we referred to Documents on line 4 with a relative path. It could also be written as ./Documents (Normally this extra bit is not required but in later sections we will see where it comes in handy).
-   .. (dotdot)- This is a reference to the parent directory. You can use this several times in a path to keep going up the heirarchy. eg if you were in the path /home/hdfs you could run the command ls ../../ and this would do a listing of the root directory.

So now you are probably starting to see that we can refer to a location in a variety of different ways. Some of you may be asking the question, which one should I use? The answer is that you can use any method you like to refer to a location. Whenever you refer to a file or directory on the command line you are actually referring to a path and your path can be constructed using any of these elements. The best approach is whichever is the most convenient for you. Here are some examples:

```
[hdfs@sandbox ~]$ pwd
/home/hdfs
[hdfs@sandbox ~]$ ls example_directory/
file1.txt  file2.txt
[hdfs@sandbox ~]$ ls /home/hdfs/example_directory/
file1.txt  file2.txt
[hdfs@sandbox ~]$ pwd
/home/hdfs
[hdfs@sandbox ~]$ ls ~/example_directory/
file1.txt  file2.txt
[hdfs@sandbox ~]$ ls ./example_directory/
file1.txt  file2.txt
[hdfs@sandbox ~]$ ls /home/hdfs/example_directory/
file1.txt  file2.txt
[hdfs@sandbox ~]$ ls ../../
bin   cgroups_test  etc     home        lib    lost+found  mnt        opt           proc  sbin     srv  tmp  vagrant
boot  dev           hadoop  kafka-logs  lib64  media       nohup.out  packer-files  root  selinux  sys  usr  var
[hdfs@sandbox ~]$ ls /
bin   cgroups_test  etc     home        lib    lost+found  mnt        opt           proc  sbin     srv  tmp  vagrant
boot  dev           hadoop  kafka-logs  lib64  media       nohup.out  packer-files  root  selinux  sys  usr  var
[hdfs@sandbox ~]$

```

After playing about with these on the command line yourself they will start to make a bit more sense. Make sure you understand how all of these elements of building a path work as you'll use all of them in future sections.

## Let's Move Around a Bit

In order to move around in the system we use a command called cd which stands for change directory. It works as follows:

```
cd [location]

```

** Shortcut: ** If you run the command cd without any arguments then it will always take you back to your home directory.

The command cd may be run without a location as we saw in the shortcut above but usually will be run with a single command line argument which is the location we would like to change into. The location is specified as a path and as such may be specified as either an absolute or relative path and using any of the path building blocks mentioned above. Here are some examples.

```
[hdfs@sandbox ~]$ pwd
/home/hdfs
[hdfs@sandbox ~]$ cd example_directory/
[hdfs@sandbox example_directory]$ ls
file1.txt  file2.txt
[hdfs@sandbox example_directory]$ cd /
[hdfs@sandbox /]$ pwd
/
[hdfs@sandbox /]$ ls
bin   cgroups_test  etc     home        lib    lost+found  mnt        opt           proc  sbin     srv  tmp  vagrant
boot  dev           hadoop  kafka-logs  lib64  media       nohup.out  packer-files  root  selinux  sys  usr  var
[hdfs@sandbox /]$ cd ~/example_directory/
[hdfs@sandbox example_directory]$ pwd
/home/hdfs/example_directory
[hdfs@sandbox example_directory]$ cd ../../
[hdfs@sandbox home]$ pwd
/home
[hdfs@sandbox home]$ cd
[hdfs@sandbox ~]$ pwd
/home/hdfs
[hdfs@sandbox ~]$

```

### Tab Completion

Typing out these paths can become tedious. If you're like me, you're also prone to making typos. The command line has a nice little mechanism to help us in this respect. It's called Tab Completion.

When you start typing a path (anywhere on the command line, you're not just limited to certain commands) you may hit the Tab key on your keyboard at any time which will invoke an auto complete action. If nothing happens then that means there are several possibilities. If you hit Tab again it will show you those possibilities. You may then continue typing and hit Tab again and it will again try to auto complete for you.

It's kinda hard to demonstrate here so it's probably best if you try it yourself. If you start typing cd /hTab/Tab you'll get a feel for how it works.

## Summary

-   `pwd` - Print Working Directory - ie. Where are we currently.
-   `ls` - List the contents of a directory.
-   `cd` - Change Directories - ie. move to another directory.
-   **Relative path** - A file or directory location relative to where we currently are in the file system.
-   **Absolute path** - A file or directory location in relation to the root of the file system.

## Activities

Have a go at the following:

-   Let's start by getting familiar with moving around. Use the commands cd and ls to explore what directories are on your system and what's in them. Make sure you use a variety of relative and absolute paths. Some interesting places to look at are:
    -   /etc - Stores config files for the system.
    -   /var/log - Stores log files for various system programs. (You may not have permission to look at everything in this directory. Don't let that stop you exploring though. A few error messages never hurt anyone.)
    -   /bin - The location of several commonly used programs
    -   /usr/bin - Another location for programs on the system.
    -   Now go to your home directory using 4 different methods.
    -   Make sure you are using Tab Completion when typing out your paths too. Why do anything you can get the computer to do for you?
