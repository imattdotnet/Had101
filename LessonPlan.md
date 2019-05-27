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
* - - -

### 0. Instructor Do: Welcome Students (0:01)

* Take the first few minutes of class to welcome students back to class. Give them a heads-up that today's class will move them one step closer to one of the hottest areas in computing.

### 1. Students Do: Setup   (0:10)

[Instructions on setting up Horton VM](/Activities/Setup/SetupHortonVM.md)

### 2. Everyone Do: Unix File System (0:10)

* Take a few moments to preview this activity.  Have students crack open the DEMO1 file and follow along.
  
* Answer any remaining questions before proceeding to the next section.

### 3. Instructor  Do: Understanding Map / Reduce   (0:10)

We've seen how Hadoop stores data on multiple machines. We want to be able to process the data using the computational power of the machines that we use to store the data. How can we do that?

To take advantage of a distributed cluster of computational nodes, we need to come up with algorithms that allows us to run things in parallel. Also, we would want to make sure that we delegate the processing to the node that actually have the data if possible.

There is a saying in Big Data:

> Don't send a terabyte to the algorithms, send the algorithms to the terabyte!

MapReduce is a programming model supported by Hadoop. The programming model allows us to process massive data sets in parallel. Hadoop supports the orchestration of a MapReduce program where the program containing the MapReduce job (containing something we'll learn to know under the name _**Mappers**_ and **_Reducers_**) as well as some orchestration of what order to run the mappers and reducers.

![Map Reduce](../Images/Map_Reduce.png)

The algorithm works as follows:

1.  Map Phase: The data is converted into a set of Key-Value pairs. Hadoop ensures that the mapper is running on the machine where the data is. A key thing about the mapper is that it can run in parallel with other mappers. That is, if the data that is processed is processed across multiple machines, the mappers caHere we come to one of the great things about Hadoop, in addition to the distributed storage, we also have a mechanism to send a program to each of the machines in our cluster. That is, we can send data around (this is called shuffling) AND we can send programs to the nodes.

The problem we now need to solve is, what kind of algorithms can we send to the nodes that works in parallel and that distributes well. This is an area of computer science that have been deeply explored by the functional programming community and Hadoop decided to combine two of common rfun in total isolation.**

2.  Shuffle Phase: This phase is performed by Hadoop and as a programmer, you're not directly involved. However, it is important to understand this phase as well. What Hadoop will do is to redistribute the key-value pairs you created from the map phase such that all key-value pairs with the same key is sent to the same machine in the cluster.

3.  Reduce Phase: In this phase, we get to see all the key-value pairs and we can apply some algorithm to combine the set of values.

Next, we'll write a Mapper, the first half of this mechanism.

### 4.  Do: Review Mapper (0:10)


### 5. Instructor Do: Review Reducer (0:15)


### 7. EveryoneStudents Do: Run Map / Reduce Job   (0:05)

* Now task students with running their own Map / Reduce jobs. Slack out the following:

* **File:**

* **Instructions:**

### 8. Instructor Do: Demo2 - Run Word Count in Hive

* If time permits, run Demo2 by submitting the Hive command, showing results.


