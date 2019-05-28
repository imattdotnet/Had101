## Lesson Plan - Intro to Hadoop 101

### Overview
In this lesson, we'll learn a little about Hadoop, why it exists, and how it relates to more traditional modes of storing data. We'll get some experience moving files around in a typical Hadoop environment. Then we'll run some big data search jobs, first using a simple Java program, and then using some of the applications built on top of Hadoop - Hive, and Pig if time permits.

##### Instructor Priorities

* Students new to Big Data concepts should understand what makes NOSQL stores like Hadoop different from traditional structured data storage.
* Students should get practice executing commands in the  Hadoop Distributed File System (HDFS).
* Students should be able to define and run at least one Map Reduce job to access and process data in Hadoop.

##### Instructor Notes

* This session will be a combination of acclimation and practice. It is especially important not to allow students to get bogged down doing installation. With about 10 minutes of account setup and configuration, they should be able to go right into hands on practice in the file system, and then run the sample code to build and execute a Map Reduce job against the sample data.

* Leading up to this exercise, you as Instructors and TAs will be responsible for helping students get their environments set up, and sample data loaded.
- - -

### Class Objectives

* To be able to explain basic concepts around Big Data:
* The difference between SQL and NOSQL (non-relational) data bases, CAP theorem, and the main elements in the Hadoop ecosystem.
* The fundamentals of MapReduce.
* To successfully install and configure the text environment.
* To gain familiarity with HDFS file commands.
* To write a simple Mapper and Reducer in Java
* If time permits, you'll also run a map reduce jobs using Hive and Pig
- - -

## 0. Instructor Do: Welcome Students (0:01)

* Take the first few minutes of class to welcome students back to class. Give them a heads-up that today's class will move them one step closer to one of the hottest areas in computing.

## 1. Students Do: Setup   (0:10)

[Instructions on setting up Horton VM](/Activities/Setup/SetupHortonVM.md)

## 2. Everyone Do: Unix File System (0:05)

* Take a few moments to preview this activity, explaining that if they are already super-comfortable using Unix commands, they can check out the more advanced labs on Permissions and Cron Jobs. Students should only spend a few minutes on this section.

* [Lab 1: Basic Navigation](/Activities/UnixFileSystem/Lab1BasicNavigation.md)
* [Lab 2: File Manipulation](/Activities/UnixFileSystem/Lab2FileManipulation.md)
* [Lab 3: Permissions](/Activities/UnixFileSystem/Lab3Permissions.md)
* [Lab 4: Cron Jobs](/Activities/UnixFileSystem/Lab4CronJobs.md)
  
* Answer any remaining questions before proceeding to the next section.

## 3. Instructor Do: Introducing Hadoop   (0:10)

### Overview of Hadoop

Many of you have probably worked on large databases before and probably traditional databases such as Relational Databases.

This approach has some limitations. For instance, a single database is limited to what you can place on a single machine. In the big data space, this is often much more than what Oracle can handle (they may be able to store the data, but often have severe problems processing such data). 

So, when is data big? 
Clearly a Megabyte (MB) is not big, right? How about a Terabyte (TB), is a TB big? Today you can buy a TB of storage for less than $100, so TBs are not that impressive anymore. How about Petabyte (PB)? A PB is 1,000 TB. That would mean 1,000 TB drives. You can buy harddrives that holds 8 TB today for less than $300. That means, we could buy a PT of storage for less than $40,000. Clearly that is in range. We would have to buy 125 disks and perhaps with data redundancy as much as twice that. Today, some companies are talking about Exabytes of data. One Exabyte is 1,000 PB. This may still be achievable, but clearly BIG. There are also those that are talking about the next level up, Zetabytes. A Zetabyte (you guessed it) is 1,000 Exabytes. To illustrate how big that is, using current technologies to build out an 1 Exabyte drive, that drive would be as big as Antarctica (or as large as the US and Mexico combined).

I hope you agree that the idea of building a hard disk as large as Antarctica is a bad idea. Although, we are pretty certain the data density will improve over time (the data storage is getting denser and denser), at least in the near future, we have to find ways to partition the data across multiple drives. This often also means that we want to store the data on multiple machines. This fragmentation of data we call partitions. 

Another reason to store data across multiple machines is to scale up computation. We really don't have much of an option. If we have say 1000 disks, we may want to have 1000 CPU's working over those disks as well. Now we can process 1000 different things in parallel! 

When we partition the data, run into another problem. This problem was described by Eric Brewer (A professor at UC Berkeley and vice-president of infrastructure at Google). 

This theorem, called Brewer's Theorem or CAP theorem states that when we partition the data, we will have to compromise either Availability or Consistency. We'll look at the CAP theorem next. 

The CAP Theorem states that it is impossible to simultaneously provide more than two of these three guarantees: 
Consistency Availability Partition tolerance 

Here is a good video illustrating the CAP theorem and discussing the proof: 

https://www.youtube.com/watch?v=Jw1iFr4v58M

What is important to take with you from the video is that IF scalability is a major concern, you can never sacrifice P(artition-tolerance).

Many large companies in the FinTech space have run into this problem and we often then have to rely on some form of sharding of the databases. However, the sharding means that we have to implement quite a bit of code to either achieve consistency or availability.

It is this realization that help realize our need for other database technologies where we embrace partition tolerance but sacrifice some of the other guarantees. Here are an example of two such databases: 

Cassandra (Partition Tolerant, Available, but sacrifices Consistency) HBase (Partition Tolerant, Consistent, but sacrifices Availability).

In the big data world, we need to partition the data to handle the larger data.

### Enter NoSQL Databases
To be able to handle the partition tolerance in large data sets, various new database types have emerged and often grouped under the name NoSQL databases. There are some disagreement of what NoSQL stands for (it was a tag used to promote a conference on Twitter), but let's just say it stands for Not Only SQL or perhaps it should be, databases that are not relational.

So, if databases are not relational, what are they? 

Probably the most commonly discussed types used for storing Big Data are as follows: 

Type | Examples 
------------|----------
|Key Value Store | Riak, Redis, Aerospike, Voldemort, MemcacheDB
|Columnar Store | HBase, Cassandra, DyamoDB, Bigtable, Druid, Hypertable
|Document Store | MongoDB, CouchDB, Couchbase, BaseX

* Key-Value store:
Simplest. Typically, the users can store and retrieve data based on some key stored in lexicographical order There is no structure in the data (no relationship), so partitioning is relatively easy, e.g., we can assign a range of keys to individual machines. 
* Columnar Store:
May look a lot like a relational database (it has tables and columns), but the Columnar store has no concepts of relationships. A columnar store typically supports what's called wide-columns, that is they can store a very large number of columns and each row can have a different number of columns. Because the columnar store doesn't support relationship, the rows can typically easily be partitioned across multiple machines.
* Document Store:
A document store can store complex data called documents. Often the documents can be of an arbitrary shape (although, strict schema definitions are possible). The structure of the document is similar to an XML/YAML/JSON/BSON. The documents may have pointers to other documents, but there are no enforced relationships making each document its own 'island'. Partitioning is relatively trivial here also.

### Hadoop 
Where does Hadoop fit into the Big Data space and how is it related to NoSQL stores? 

Hadoop is a distributed data and processing system. 

By itself, it's quite a primitive software solution that allows you to create cohesive clusters of data and processing nodes. Hadoop is not a NoSQL database, but some of the NoSQL solutions may use Hadoop to implement their solution. 
HBase, based on Hadoop, is a columnar store that is Partition Tolerant and Consistent (giving up Availability). 

### Hadoop as a File System
Instructor: Poll your students to see how many of them feel super-confident moving files around using Unix commands. If more than half of them say they are confident, skip this section and go right to the HDFS labs, where you'll use Unix-like commands to move files around within the Hadoop file system.

In this Optional lab, we'll learn the basics of moving around the system. Many tasks rely on being able to get to, or reference the correct location in the system. As such, this stuff really forms the foundation of being able to work effectively in Linux. Make sure you understand it well. The output of the commands in this lab may be different, depending on the system you are executing them. 

Reading, Copying, Moving Files Inside HDFS

## 4. Students Do: Hadoop Labs   (0:05)

* [Lab 1: Hadoop Commands](/Activities/Hadoop/LabHadoopCommands.md)
* [Cheatsheet: Hadoop HDFS Commands](/Activities/Hadoop/hdfs-cheatsheet.pdf)

## 5. Instructor Do: Understanding Map / Reduce   (0:10)

Given the way Hadoop stores data on multiple machines, we want to be able to process the data using the computational power of the machines that we use to store the data. How can we do that?

To take advantage of a distributed cluster of computational nodes, we need to come up with algorithms that allows us to run things in parallel. Also, we would want to make sure that we delegate the processing to the node that actually have the data if possible.

There is a saying in Big Data:

> Don't send a terabyte to the algorithms, send the algorithms to the terabyte!

MapReduce is a programming model which allows us to process massive data sets in parallel. 

![Map Reduce](/Images/Map_Reduce.png)

The algorithm works like this:

1.  Map Phase: The data is converted into a set of Key-Value pairs. Hadoop ensures that the mapper is running on the machine where the data is. The mapper can run in parallel with other mappers. If the data is processed across multiple machines, the mappers can run in total isolation.

2.  Shuffle Phase: Hadoop redistributes the key-value pairs you created from the map phase such that all key-value pairs with the same key is sent to the same machine in the cluster. Good news: Hadoop does this without any help from your programming!

3.  Reduce Phase: In this phase, we get to see all the key-value pairs and we can apply some algorithm to combine the set of values.

Next, we'll review a Mapper, the first half of this mechanism.

### 6.  Students Do: Review Mapper and Reducer(0:10)

[Download:](/Activities/MapReduce/mapreducejava_690976.zip)
Unzip this file and you should have all the files you need to complete this lab.

In this lab, you will run a MapReduce application using the Hadoop Java API. The "Hello World" of MapReduce is traditionally a word count program. You will perform a word count on the text of A Tale of Two Cities.

* [Lab 1: Review Mapper](/Activities/MapReduce/Lab1Mapper.md)
* [Lab 2: Review Reducer](/Activities/MapReduce/Lab2Reducer.md)

### 7. Students Do: Run Map / Reduce Job   (0:05)

* Now task students with running their own Map / Reduce jobs. Slack out the following:
* [Lab 3: Run and review output](/Activities/MapReduce/Lab3PutTogetherAndRun.md)

### 8. Instructor Do: Run Word Count in Hive (0:05)

* If time permits, run Demo2 by submitting the Hive command, showing results.
* [Lab 1: WordCount](/Activities/Hive/Lab1WordCount.md)

### 9. Students Do: Hive and Pig   (0:10)

* Time for students to get a taste of pig and hive. Slack out the following:
* [Lab 1: WordCount](/Activities/Hive/Lab1WordCount.md)
* [Advanced Hive Labs](/Activities/Hive/AdvancedLabs1_4.md)
* [Lab 1: HelloWorld](/Activities/Pig/Lab1HelloWorld.md)
* [Advanced Pig Labs](/Activities/Pig/AdvancedPigLabs.md)
