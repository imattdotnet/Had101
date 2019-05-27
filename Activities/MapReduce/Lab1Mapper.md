MapReduce Development With Java
In this lab, you will run a MapReduce application using the Hadoop Java API. The "Hello World" of MapReduce is traditionally a word count program. You will perform a word count on the text of A Tale of Two Cities, a classic novel by Charles Dickens.

Objectives
Become familiar with the Mapper and Reducer phase of a MapReduce job.
Submit the job on the Hortonworks virtual machine and examine the output.
Prerequisites
This lab assumes that the student has a working Hortonworks virtual machine environment.

Windows users may need to use PuTTY in order to run scp and ssh commands.

Instructions
We will be executing the MapReduce job on the Hortonworks virtual machine (VM) so let's first start by copying our data file and Java program to the VM. Open a terminal and in the lesson-160-mapreduce-development-with-javadirectory, execute the following commands.
scp lab/a-tale-of-two-cities.txt root@sandbox.hortonworks.com:/tmp
scp lab/WordCount.java root@sandbox.hortonworks.com:/tmp
Use ssh to login to the VM.
ssh root@sandbox.hortonworks.com
Now we need to add the data file to HDFS. Run the following command on the VM.
[root@sandbox ~]# sudo -u hdfs hadoop fs -put /tmp/a-tale-of-two-cities.txt /tmp/a-tale-of-two-cities.txt
Now let's compile our Java class and prepare a JAR file to be executed by Hadoop. Run the following commands on the VM.
[root@sandbox ~]# sudo su - hdfs
[hdfs@sandbox ~]$ mkdir -p lesson-160/com/example
[hdfs@sandbox ~]$ cp /tmp/WordCount.java lesson-160/com/example/
[hdfs@sandbox ~]$ cd lesson-160/
[hdfs@sandbox lesson-160]$ javac -cp `hadoop classpath` com/example/WordCount.java
[hdfs@sandbox lesson-160]$ jar cvf wordcount.jar com/
hadoop classpath is a command that prints the class path needed to get the Hadoop jar and the required libraries.

We are all set to execute the MapReduce job now but before we do so, open lab/WordCount.java locally in a text editor of your choice and let's take a look at what is going on. You can see that we have a public Java class called WordCount with two inner classes named Map and Reduce.
public static class Map extends Mapper<LongWritable, Text, Text, IntWritable> {
    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
        String line = value.toString();
        StringTokenizer tokenizer = new StringTokenizer(line);
        while (tokenizer.hasMoreTokens()) {
            word.set(tokenizer.nextToken());
            context.write(word, one);
        }
    }
}
The Map class extends Mapper which is part of the Hadoop API. Within the angle brackets are the data types for the input key and value and the emitted key and value. You then are required to override a method called map that performs the actual work of the mapper function.
