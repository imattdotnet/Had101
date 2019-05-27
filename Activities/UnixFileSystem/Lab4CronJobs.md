

# Cron - Task Scheduling

Cron stands for Command Run ON. It is a mechanism that allows you to tell the system to run certain commands at certain times. So for instance, I could craft a script to check how much space is left on the disk that holds the home directories and send an email to certain people if the free disk space gets below a certain amount. Then I would use cron to get this script to run automatically every hour.

Cron allows a fair bit of control in terms of when commands are scheduled to run. Like most things in Linux, it is managed by way of a text file. The layout of the text file can be a little daunting but once you get the hang of it it's not that bad. There are many websites out there with good references to make it easy as well.

## Basics

The file that manages cron is called a crontab. Each user may have their own crontab and within it each line represents a schedule for a particular command as follows:

```
* * * * * command to execute

```

Where the *'s represent (in order from left to right:

-   Minutes (0 - 59)
-   Hours (0 - 23)
-   Day of Month (1 - 31)
-   Months (1 - 12)
-   Day of week (0 - 7) (0 and 7 are Sunday)

A * in any of those places means every one of those increments.

Some examples:

```
* * * * * /bin/myscript.sh

```

Execute `myscript.sh` every minute.

```
30 3 * * 4 /bin/myscript.sh

```

Execute `myscript.sh` every Thursday at 3:30am.

## Ranges

You can use ranges to define cron jobs. For example:

-   1,2,5,9 - means every first, second, fifth, and ninth (minute, hour, month, ...).
-   0-4,8-12 - means all (minutes, hours, months,...) from 0 to 4 and from 8 to 12.
-   */5 - means every fifth (minute, hour, month, ...).
-   1-9/2 is the same as 1,3,5,7,9.

Ranges or lists of names are not allowed (if you are using names instead of numbers for months and days - e.g., Mon-Wed is not valid).

```
1,7,25,47 */2 * * * command

```

Run `command` every second hour in the first, seventh, 25th, and 47th minute.

## Your Crontab

To view a list of what tasks you currently have scheduled you may run the following command:

```
crontab -l

```

To edit your scheduled tasks, run the following command. It will open up in your default text editor which is normally Vi.

```
crontab -e

```

## Activities

Try making a cron job that outputs to a file every minute. Save the following script to a file named `helloworld.sh` and then use crontab to output to a file in your home directory.

#!/bin/bash
echo Hello World

In order to be able to run the file, you need to change the permissions with `chmod`. Add the executable right for the owner:

```
chmod u+x helloworld.sh

```

To send the output of the script to a file, use the following syntax to append. This way you can see multiple "Hello World" messages appear.

```
./helloworld.sh >> /home/your-userid/helloworld.log

```

If your script does not have the executable privilege, cron will send you an email. It will look like this:

```
You have mail in /var/mail/<user>

```

Enter `mail` to view it:

```
$ mail
Mail version 8.1 6/6/93.  Type ? for help.
"/var/mail/<user>": 17 messages 17 new
>N  1 <user>@LM-SJN-21  Fri Apr 14 18:00  19/829   "Cron <<user>@LM-SJN-XXXXXXXX> helloworld.sh >> ~/helloworld.log"

```

Enter `1` to view the first message:

```
? 1
Message 1:
From <user>@LM-SJN-XXXXXXXX.localdomain  Fri Apr 14 18:00:03 2017
X-Original-To: <user>
Delivered-To: <user>@LM-SJN-XXXXXXXX.localdomain
From: <user>@LM-SJN-XXXXXXXX.localdomain (Cron Daemon)
To: <user>@LM-SJN-XXXXXXXX.localdomain
Subject: Cron <<user>@LM-SJN-XXXXXXXX> helloworld.sh >> ~/helloworld.log
X-Cron-Env: <SHELL=/bin/sh>
X-Cron-Env: <PATH=/usr/bin:/bin>
X-Cron-Env: <LOGNAME=<user>>
X-Cron-Env: <USER=<user>>
X-Cron-Env: <HOME=/Users/<user>>
Date: Fri, 14 Apr 2017 18:00:02 -0700 (PDT)

/bin/sh: helloworld.sh: command not found

```

Command not found! This is a common mistake and it is easy to fix with the `chmod u+x` command.

> You can delete all message in mail by entering `d *` and then exit mail with `q`.