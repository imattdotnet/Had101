

# Lab Environment Setup with Hortonworks Virtual Machine

## Instructions

1.  Many of the labs in this course require the use of [Hortonworks Sandbox](http://hortonworks.com/products/hortonworks-sandbox) which is a virtual machine that has been configured with a Hadoop environment. It is the quickest way to get started with Big Data.
    
    At the time of this writing, the current version of Hortonworks Sandbox is **2.3.2**. Download the virtual machine file. It is about 9GB.
    
2.  The lab instructions have been written and tested using the [VirtualBox](https://www.virtualbox.org/) version of Hortonworks Sandbox. Please download and install VirtualBox for your operating system [at this link](https://www.virtualbox.org/wiki/Downloads).
3.  Start the VirtualBox and choose _Import Appliance_ from VirtualBox. (This is "Import", not "Open".) _Do not_ power on your virtual machine after you have finished importing.
    
NOTE: Windows screens are going to be a bit different, but all the commands and content in this section are equivalent. We have specific Windows instructions in the text where needed.
    
4.  We need to do a bit of network setup. The virtual machine must be running with a _Host-only Adapter_ configuration in VirtualBox. We will set this up in two steps:
    
    -   Set up a Host-only Adapter in Virtual Box
    -   Select it to be used with our virtual machine.
    
    We will do this next.
    
5.  Go to the menu of Virtual Box and choose _VirtualBox > Preferences_ and then navigate to _Network_. Click on the _Host-only Networks_. If you don't see _vboxnet0_ as an entry, click on the '+' button to create it.

Now click on the screwdriver button and then click on _DHCP Server_.
        
    Make note of the value of the _Lower Address Bound_ IP address. In this case, for our machine, the IP address in question is 192.168.56.101. Note the value you have for your machine.
    
6.  Click on "OK" until you exit the preferences.
7.  The virtual machine must be running with a _Host-only Adapter_ configuration in VirtualBox. Select the _Settings_ of the Hortonworks virtual machine and choose the _Network_ tab to change from _NAT_ to _Host-only Adapter_. Make sure the _Enable Network Adapter_ is checked.
    
    That's it for the network setup.
    
8.  Many of the labs assume that `sandbox.hortonworks.com` resolves to the virtual machine. We will set this up now.
    
    #### Mac & Linux Setup:
    
    If you are on Mac OS X or Linux, open a terminal and use the _Lower Address Bound_ IP address from the previous step to make an entry to `/etc/hosts`. In this case, that IP is 192.168.56.101, but it could be different on your own computer.
    
    <pre>$ sudo sh -c "echo '192.168.56.101 sandbox.hortonworks.com' >> /etc/hosts"</pre>
    
    #### Windows Setup:
    
    If you are on Windows, you will need to edit the hosts file. On Windows, hosts file is located at `C:\Windows\System32\Drivers\etc\hosts`
    
    Open the file in an editor and add the line with the IP for the _Lower Address Bound_. For example:
    
    `192.168.56.101 sandbox.hortonworks.com`
    
    That's it.
    
9.  Boot the virtual machine.

## Accessing the Sandbox

Once started, you can access the virtual machine either through the [Ambari web UI](http://sandbox.hortonworks.com:8080/) or through the terminal.

-   To access via the terminal, use ssh to log into the virtual machine.
    
    $ ssh root@sandbox.hortonworks.com
    
    Windows users can use [PuTTY](http://www.putty.org/) to use secure shell.
    
    The default password is **hadoop** but you will be required to change it after logging in the first time.
    
    > Note: Hortonworks VM is using CentOS. If CentOS is not happy with your new password it will just exist without changing the old password. Make sure you use a password which is not easy to guess.
    
-   To access Hadoop admin dashboard via Ambari, use the following URL: [http://sandbox.hortonworks.com:8080](http://sandbox.hortonworks.com:8080/)

-   _User Name:_ admin
-   _Password:_ admin

Happy Hadooping!

