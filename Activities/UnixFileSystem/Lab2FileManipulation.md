# File Manipulation

In this lab, we'll learn how to make some files and directories and move them around.

## Making a Directory

Linux organizes its file system in a hierarchical way. Over time you'll tend to build up a fair amount of data (storage capacities are always increasing). It's important that we create a directory structure that will help us organize that data in a manageable way. Develop the habit of organizing your stuff into an elegant file structure now and you will thank yourself for years to come.

Creating a directory is pretty easy. The command we are after is `mkdir` which is short for Make Directory.

```
mkdir [options] <Directory>

```

In it's most basic form we can run `mkdir` supplying only a directory and it will create it for us.

```
[hdfs@sandbox ~]$ pwd
/home/hdfs
[hdfs@sandbox ~]$ ls
example_directory  pig_1490221969660.log
[hdfs@sandbox ~]$ mkdir linuxtutorialwork
[hdfs@sandbox ~]$ ls
example_directory  linuxtutorialwork  pig_1490221969660.log
[hdfs@sandbox ~]$

```

Let's break it down:

-   `pwd` - Let's start off by making sure we are where we think we should be. (In the example above I am in my home directory)
-   `ls` - We'll do a quick listing so we know what is already in our directory.
-   `mkdir linuxtutorialwork` - Run the command `mkdir` and create a directory linuxtutorialwork (a nice place to put further work we do relating to this tutorial just to keep it separate from our other stuff).

Remember that when we supply a directory in the above command we are actually supplying a path. Is the path we specified relative or absolute? Here are a few more examples of how we can supply a directory to be created

-   mkdir /home/hdfs/foo
-   mkdir ./blah
-   mkdir ../dir1
-   mkdir ~/linuxtutorialwork/dir2

There are a few useful options available for `mkdir`.

The first one is **-p** which tells `mkdir` to make parent directories as needed (demonstration of what that actually means below). The second one is **-v** which makes `mkdir` tell us what it is doing (as you saw in the example above, it normally does not).

```
[hdfs@sandbox ~]$ mkdir -p linuxtutorialwork/foo/bar
[hdfs@sandbox ~]$ cd linuxtutorialwork/foo/bar/
[hdfs@sandbox bar]$ pwd
/home/hdfs/linuxtutorialwork/foo/bar
[hdfs@sandbox bar]$

```

And now the same command but with the -v option

```
[hdfs@sandbox ~]$ mkdir -pv linuxtutorialwork/foo/bar
mkdir: created directory `linuxtutorialwork/foo'
mkdir: created directory `linuxtutorialwork/foo/bar'
[hdfs@sandbox ~]$ cd linuxtutorialwork/foo/bar/
[hdfs@sandbox bar]$ pwd
/home/hdfs/linuxtutorialwork/foo/bar
[hdfs@sandbox bar]$

```

## Removing a Directory

Creating a directory is pretty easy. Removing or deleting a directory is easy too. One thing to note, however, is that there is no undo when it comes to the command line on Linux (Linux GUI desktop environments typically do provide an undo feature but the command line does not). Just be careful with what you do. The command to remove a directory is `rmdir`, short for remove directory.

```
rmdir [options] <Directory>

```

Two things to note. Firstly, `rmdir` supports the **-v** and **-p** options similar to `mkdir`. Secondly, a directory must be empty before it may be removed (later on we'll see a way to get around this).

```
[hdfs@sandbox ~]$ rmdir linuxtutorialwork/foo/bar/
[hdfs@sandbox ~]$ ls linuxtutorialwork/foo/

```

## Creating a Blank File

A lot of commands that involve manipulating data within a file have the nice feature that they will create a file automatically if we refer to it and it does not exist. In fact we can make use of this very characteristic to create blank files using the command `touch`.

```
touch [options] <filename>

```

```
[hdfs@sandbox linuxtutorialwork]$ pwd
/home/hdfs/linuxtutorialwork
[hdfs@sandbox linuxtutorialwork]$ ls
foo
[hdfs@sandbox linuxtutorialwork]$ touch example1
[hdfs@sandbox linuxtutorialwork]$ ls
example1  foo
[hdfs@sandbox linuxtutorialwork]$

```

`touch` is actually a command we may use to modify the access and modification times on a file (normally not needed but sometimes when you're testing a system that relies on file access or modification times it can be useful). What we are taking advantage of here is that if we touch a file and it does not exist, the command will do us a favor and automatically create it for us.

Many things in Linux are not done directly but by knowing the behaviour of certain commands and aspects of the system and using them in creative ways to achieve the desired outcome.

## Copying a File or Directory

There are many reasons why we may want to make a duplicate of a file or directory. Often before changing something, we may wish to create a duplicate so that if something goes wrong we can easily revert back to the original. The command we use for this is cp which stands for copy.

```
cp [options] <source> <destination>

```

There are quite a few options available to `cp`. It's worth checking out the `man` page for `cp` to see what else is available.

```
[hdfs@sandbox linuxtutorialwork]$ ls
example1  foo
[hdfs@sandbox linuxtutorialwork]$ cp example1 barney
[hdfs@sandbox linuxtutorialwork]$ ls
barney  example1  foo
[hdfs@sandbox linuxtutorialwork]$

```

Note that both the source and destination are paths. This means we may refer to them using both absolute and relative paths. Here are a few examples:

-   cp /home/hdfs/linuxtutorialwork/example2 example3
-   cp example2 ../../backups
-   cp example2 ../../backups/example4
-   cp /home/hdfs/linuxtutorialwork/example2 /otherdir/foo/example5

When we use `cp` the destination can be a path to either a file or directory. If it is to a file then it will create a copy of the source but name the copy the filename specified in destination. If we provide a directory as the destination then it will copy the file into that directory and the copy will have the same name as the source.

In it's default behaviour `cp` will only copy a file (there is a way to copy several files in one go but we'll get to that in section 6. Wildcards). Using the **-r** option, which stands for recursive, we may copy directories. Recursive means that we want to look at a directory and all files and directories within it, and for subdirectories, go into them and do the same thing and keep doing this.

```
[hdfs@sandbox linuxtutorialwork]$ ls
barney  example1  foo
[hdfs@sandbox linuxtutorialwork]$ cp foo foo2
cp: omitting directory `foo'
[hdfs@sandbox linuxtutorialwork]$ cp -r foo foo2
[hdfs@sandbox linuxtutorialwork]$ ls
barney  example1  foo  foo2
[hdfs@sandbox linuxtutorialwork]$

```

In the above example any files and directories within the directory foo will also be copied to foo2.

## Moving a File or Directory

To move a file we use the command `mv` which is short for move. It operates in a similar way to `cp`. One slight advantage is that we can move directories without having to provide the **-r** option.

```
mv [options] <source> <destination>

```

```
[hdfs@sandbox linuxtutorialwork]$ ls
barney  example1  foo  foo2
[hdfs@sandbox linuxtutorialwork]$ mkdir backups
[hdfs@sandbox linuxtutorialwork]$ mv foo2 backups/foo3
[hdfs@sandbox linuxtutorialwork]$ mv barney backups
[hdfs@sandbox linuxtutorialwork]$ ls
backups  example1  foo
[hdfs@sandbox linuxtutorialwork]$

```

Let's break it down:

-   `mkdir backups` We created a new directory called backups.
-   `mv foo2 backups/foo3` We moved the directory foo2 into the directory backups and renamed it as foo3
-   `mv barney backups` We moved the file barney into backups. As we did not provide a destination name, it kept the same name. Note that again the source and destination are paths and may be referred to as either absolute or relative paths.

### Renaming Files and Directories

Now just as above with the command `touch`, we can use the basic behaviour of the command `mv` in a creative way to achieve a slighly different outcome. Normally `mv` will be used to move a file or directory into a new directory. As we saw on line 4 above, we may provide a new name for the file or directory and as part of the move it will also rename it. Now if we specify the destination to be the same directory as the source, but with a different name, then we have effectively used `mv` to rename a file or directory.

```
[hdfs@sandbox linuxtutorialwork]$ ls
backups  example1  foo
[hdfs@sandbox linuxtutorialwork]$ mv foo foo3
[hdfs@sandbox linuxtutorialwork]$ ls
backups  example1  foo3
[hdfs@sandbox linuxtutorialwork]$ cd ..
[hdfs@sandbox ~]$ mkdir linuxtutorialwork/testdir
[hdfs@sandbox ~]$ mv linuxtutorialwork/testdir /home/hdfs/linuxtutorialwork/fred
[hdfs@sandbox ~]$ ls linuxtutorialwork
backups  example1  foo3  fred
[hdfs@sandbox ~]$

```

Let's break it down:

-   `mv foo foo3` - We renamed the file foo to be foo3 (both paths are relative).
-   `cd ..` - We moved into our parent directory. This was done only so in the next line we can illustrate that we may run commands on files and directories even if we are not currently in the directory they reside in.
-   `mv linuxtutorialwork/testdir /home/hdfs/linuxtutorialwork/fred` - We renamed the directory testdir to fred (the source path was a relative path and the destination was an aboslute path).

## Removing a File (and non empty Directories)

As with `rmdir`, removing a file is an action that may not be undone so be careful. The command to remove or delete a file is `rm` which stands for remove.

```
rm [options] <file>

```

```
[hdfs@sandbox linuxtutorialwork]$ ls
backups  example1  foo3  fred
[hdfs@sandbox linuxtutorialwork]$ rm example1
[hdfs@sandbox linuxtutorialwork]$ ls
backups  foo3  fred
[hdfs@sandbox linuxtutorialwork]$

```

### Removing non empty Directories

Like several other commands introduced in this lab, `rm` has several options that alter it's behaviour. I'll leave it up to you to look at the `man` page to see what they are but I will introduce one particularly useful option which is **-r**. Similar to `cp` it stands for recursive. When `rm` is run with the **-r** option it allows us to remove directories and all files and directories contained within.

```
[hdfs@sandbox linuxtutorialwork]$ ls
backups  foo3  fred
[hdfs@sandbox linuxtutorialwork]$ rmdir backups
rmdir: failed to remove `backups': Directory not empty
[hdfs@sandbox linuxtutorialwork]$ rm backups
rm: cannot remove `backups': Is a directory
[hdfs@sandbox linuxtutorialwork]$ rm -r backups
[hdfs@sandbox linuxtutorialwork]$ ls
foo3  fred
[hdfs@sandbox linuxtutorialwork]$

```

A good option to use in combination with **r** is **i** which stands for interactive. This option will prompt you before removing each file and directory and give you the option to cancel the command.

## One Final Note

I know I've stressed the point but by now hopefully you can appreciate that whenever we refer to a file or directory on the command line it is in fact a path. As such it may be specified as either an absolute or relative path. This is pretty much always the case so remember this important point. Remember to experiment with both absolute and relative paths in the commands as sometimes they give subtle but useful differences in output. (The output of the command may be slightly different but the action of the command will always be the same.)

## Summary

-   `mkdir` - Make Directory - ie. Create a directory.
-   `rmdir` - Remove Directory - ie. Delete a directory.
-   `touch` - Create a blank file.
-   `cp` - Copy - ie. Copy a file or directory.
-   `mv` - Move - ie. Move a file or directory (can also be used to rename).
-   `rm` - Remove - ie. Delete a file.
-   **No undo** - The Linux command line does not have an undo feature. Perform destructive actions carefully.
-   **Command line options** - Most commands have many useful command line options. Make sure you skim the man page for new commands so you are familiar with what they can do and what is available.

## Activities

We now have at our disposal, various commands to actually interact with the system. Let's put them into practice. Have a go at the following:

-   Start by creating a directory in your home directory in which to experiment.
-   In that directory, create a series of files and directories (and files and directories in those directories).
-   Now rename a few of those files and directories.
-   Delete one of the directories that has other files and directories in them.
-   Move back to your home directory and from there copy a file from one of your subdirectories into the initial directory you created.
-   Now move that file back into another directory.
-   Rename a few files
-   Next, move a file and rename it in the process.
-   Finally, have a look at the existing directories in your home directory. You probably have a Documents, Downloads, Music and Images directory etc. Think about what other directories may help you keep your account organised and start setting this up.