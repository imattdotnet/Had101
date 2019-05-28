

# Permissions

In this lab we'll learn about how to set Linux permissions on files and directories. Permissions specify what a particular person may or may not do with respect to a file or directory. As such, permissions are important in creating a secure environment. For instance you don't want other people to be changing your files and you also want system files to be safe from damage (either accidental or deliberate). Luckily, permissions in a Linux system are quite easy to work with.

## So what are they?

Linux permissions dictate 3 things you may do with a file, read, write and execute. They are referred to in Linux by a single letter each.

-   **r** read - you may view the contents of the file.
-   **w** write - you may change the contents of the file.
-   **x** execute - you may execute or run the file if it is a program or script.

For every file we define 3 sets of people for whom we may specify permissions.

-   **owner** - a single person who owns the file. (typically the person who created the file but ownership may be granted to some one else by certain users)
-   **group** - every file belongs to a single group.
-   **others** - everyone else who is not in the group or the owner.

Three persmissions and three groups of people. That's about all there is to permissions really. Now let's see how we can view and change them.

View Permissions

To view permissions for a file we use the long listing option for the command ls.

```
ls -l [path]

```

```
[hdfs@sandbox linuxtutorialwork]$ ls -l /home/hdfs/linuxtutorialwork/frog.png
-rwx-----x 1 hdfs hadoop 2817927 Feb 17 15:01 /home/hdfs/linuxtutorialwork/frog.png
[hdfs@sandbox linuxtutorialwork]$

```

In the above example the first 10 characters of the output are what we look at to identify permissions.

-   The first character identifies the file type. If it is a dash ( - ) then it is a normal file. If it is a d then it is a directory.
-   The following 3 characters represent the permissions for the owner. A letter represents the presence of a permission and a dash ( - ) represents the absence of a permission. In this example the owner has all permissions (read, write and execute).
-   The following 3 characters represent the permissions for the group. In this example the group has the ability to read but not write or execute. Note that the order of permissions is always read, then write then execute.
-   Finally the last 3 characters represent the permissions for others (or everyone else). In this example they have the execute permission and nothing else.

## Change Permissions

To change permissions on a file or directory we use a command called `chmod` It stands for change file mode bits which is a bit of a mouthfull but think of the mode bits as the permission indicators.

```
chmod [permissions] [path]

```

`chmod` has permission arguments that are made up of 3 components

-   Who are we changing the permission for? [ugoa] - user (or owner), group, others, all
-   Are we granting or revoking the permission - indicated with either a plus ( + ) or minus ( - )
-   Which permission are we setting? - read ( r ), write ( w ) or execute ( x )

The following examples will make their usage clearer.

Grant the execute permission to the group. Then remove the write permission for the owner.

```
[hdfs@sandbox linuxtutorialwork]$ ls -l frog.png
-rwx-----x 1 hdfs hadoop 2817927 Feb 17 15:01 frog.png
[hdfs@sandbox linuxtutorialwork]$ chmod g+x frog.png
[hdfs@sandbox linuxtutorialwork]$ ls -l frog.png
-rwx--x--x 1 hdfs hadoop 2817927 Feb 17 15:01 frog.png
[hdfs@sandbox linuxtutorialwork]$
[hdfs@sandbox linuxtutorialwork]$ chmod u-w frog.png
[hdfs@sandbox linuxtutorialwork]$ ls -l frog.png
-r-x--x--x 1 hdfs hadoop 2817927 Feb 17 15:01 frog.png
[hdfs@sandbox linuxtutorialwork]$

```

Don't want to assign permissions individually? We can assign multiple permissions at once.

```
[hdfs@sandbox linuxtutorialwork]$ ls -l frog.png
-rwx-----x 1 hdfs hadoop 2817927 Feb 17 15:01 frog.png
[hdfs@sandbox linuxtutorialwork]$
[hdfs@sandbox linuxtutorialwork]$ chmod g+wx frog.png
[hdfs@sandbox linuxtutorialwork]$ ls -l frog.png
-rwx-wx--x 1 hdfs hadoop 2817927 Feb 17 15:01 frog.png
[hdfs@sandbox linuxtutorialwork]$
[hdfs@sandbox linuxtutorialwork]$ chmod go-x frog.png
[hdfs@sandbox linuxtutorialwork]$ ls -l frog.png
-rwx-w---- 1 hdfs hadoop 2817927 Feb 17 15:01 frog.png
[hdfs@sandbox linuxtutorialwork]$

```

It may seem odd that as the owner of a file we can remove our ability to read, write and execute that file but there are valid reasons we may wish to do this. Maybe we have a file with data in it we wish not to accidentally change for instance. While we may remove these permissions, we may not remove our ability to set those permissions and as such we always have control over every file under our ownership.

## Setting Permissions Shorthand

The method outlined above is not too hard for setting permissions but it can be a little tedious if we have a specific set of permissions we sould like to apply regularly to certain files. Luckily, there is a shorthand way to specify permissions that makes this easy.

To understand how this shorthand method works we first need a little background in number systems. Our typical number system is decimal. It is a base 10 number system and as such has 10 symbols (0 - 9) used. Another number system is octal which is base 8 (0-7). Now it just so happens that with 3 permissions and each being on or off, we have 8 possible combinations (2^3). Now we can also represent our numbers using binary which only has 2 symbols (0 and 1). The mapping of octal to binary is in the table below.

**Octal** | **Binary**
----------------|----------------
**0** | 0 0 0
**1** | 0 0 1
**2** | 0 1 0
**3** | 0 1 1
**4** | 1 0 0
**5** | 1 0 1
**6** | 1 1 0
**7** | 1 1 1

Now the interesting point to note is that we may represent all 8 octal values with 3 binary bits and that every possible combination of 1 and 0 is included in it. So we have 3 bits and we also have 3 permissions. If you think of 1 as representing on and 0 as off then a single octal number may be used to represent a set of permissions for a set of people. Three numbers and we can specify permissions for the user, group and others. Let's see some examples. (refer to the table above to see how they match)

```
[hdfs@sandbox linuxtutorialwork]$ ls -l frog.png
-rw-r----x 1 hdfs hadoop 2817927 Feb 17 15:01 frog.png
[hdfs@sandbox linuxtutorialwork]$ chmod 751 frog.png
[hdfs@sandbox linuxtutorialwork]$ ls -l frog.png
-rwxr-x--x 1 hdfs hadoop 2817927 Feb 17 15:01 frog.png
[hdfs@sandbox linuxtutorialwork]$
[hdfs@sandbox linuxtutorialwork]$ chmod 240 frog.png
[hdfs@sandbox linuxtutorialwork]$ ls -l frog.png
--w-r----- 1 hdfs hadoop 2817927 Feb 17 15:01 frog.png
[hdfs@sandbox linuxtutorialwork]$

```

People often remember commonly used number sequences for different types of files and find this method quite convenient. For example **755** or **750** are commonly used for scripts.

## Permissions for Directories

The same series of permissions may be used for directories but they have a slightly different behaviour.

-   **r** - you have the ability to read the contents of the directory (ie do an `ls`)
-   **w** - you have the ability to write into the directory (ie create files and directories)
-   **x** - you have the ability to enter that directory (ie `cd`)

Let's see some of these in action

```
[hdfs@sandbox linuxtutorialwork]$ ls testdir
file1  file2  file3
[hdfs@sandbox linuxtutorialwork]$
[hdfs@sandbox linuxtutorialwork]$ chmod 400 testdir
[hdfs@sandbox linuxtutorialwork]$ ls -ld testdir
dr-------- 2 hdfs hadoop 4096 Mar 23 23:55 testdir
[hdfs@sandbox linuxtutorialwork]$
[hdfs@sandbox linuxtutorialwork]$ cd testdir
-bash: cd: testdir: Permission denied
[hdfs@sandbox linuxtutorialwork]$ ls testdir
ls: cannot access testdir/file3: Permission denied
ls: cannot access testdir/file2: Permission denied
ls: cannot access testdir/file1: Permission denied
file1  file2  file3
[hdfs@sandbox linuxtutorialwork]$
[hdfs@sandbox linuxtutorialwork]$ chmod 100 testdir
[hdfs@sandbox linuxtutorialwork]$
[hdfs@sandbox linuxtutorialwork]$ cd testdir
[hdfs@sandbox testdir]$ ls testdir
ls: cannot access testdir: No such file or directory
[hdfs@sandbox testdir]$

```

The **-d** option for `ls` stands for directory. Normally if we give `ls` an argument which is a directory it will list the contents of that directory. In this case however we are interested in the permissions of the directory directly and the **-d** option allows us to obtain that.

These permissions can seem a little confusing at first. What we need to remember is that these permissions are for the directory itself, not the files within. So, for example, you may have a directory which you don't have the read permission for. It may have files within it which you do have the read permission for. As long as you know the file exists and it's name you can still read the file.

```
[hdfs@sandbox linuxtutorialwork]$ ls -ld testdir
d--x------ 2 hdfs hadoop 4096 Mar 23 23:55 testdir
[hdfs@sandbox linuxtutorialwork]$
[hdfs@sandbox linuxtutorialwork]$ cd testdir
[hdfs@sandbox testdir]$
[hdfs@sandbox testdir]$ ls testdir
ls: cannot access testdir: No such file or directory
[hdfs@sandbox testdir]$
[hdfs@sandbox testdir]$ cat samplefile.txt
Kyle 20
Stan 11
Kenny 37
[hdfs@sandbox testdir]$

```

## The root user

On a Linux system there are only 2 people usually who may change the permissions of a file or directory. The owner of the file or directory and the root user. The root user is a superuser who is allowed to do anything and everything on the system. Typically the administrators of a system would be the only ones who have access to the root account and would use it to maintain the system. Typically normal users would mostly only have access to files and directories in their home directory and maybe a few others for the purposes of sharing and collaborating on work and this helps to maintain the security and stability of the system.

## Basic Security

Your home directory is your own personal space on the system. You should make sure that it stays that way.

Most users would give themselves full read, write and execute permissions for their home directory and no permissions for the group or others however some people for various reasons may have a slighly different set up.

Normally, for optimal security, you should not give either the group or others write access to your home directory, but execute without read can come in handy sometimes. This allows people to get into your home directory but not allow them to see what is there. An example of when this is used is for personal web pages.

It is typical for a system to run a webserver and allow users to each have their own web space. A common set up is that if you place a directory in your home directory called public_html then the webserver will read and display the contents of it. The webserver runs as a different user to you however so by default will not have access to get in and read those files. This is a situation where it is necessary to grant execute on your home directory so that the webserver user may access the required resources.

## Summary

-   `chmod` - Change permissions on a file or directory.
-   `ls -ld` - View the permissions for a specific directory.
-   **Security** - Correct permissions are important for the security of a system.
-   **Usage** - Setting the right permissions is important in the smooth running of certain tasks on Linux.

## Activities

Let's play with some permissions.

-   First off, take a look at the permissions of your home directory, then have a look at the permissions of various files in there.
-   Now let's go into your linuxtutorialwork directory and change the permissions of some of the files in there. Make sure you use both the shorthand and longhand form for setting permissions and that you also use a variety of absolute and relative paths. Try removing the read permission from a file then reading it. Or removing the write permission and then opening it in `vi`.
-   Let's play with directories now. Create a directory and put some files into it. Now play about with removing various permissions from yourself on that directory and see what you can and can't do.
-   Finally, have an explore around the system and see what the general permissions are for files in other system directories such as /etc and /bin
