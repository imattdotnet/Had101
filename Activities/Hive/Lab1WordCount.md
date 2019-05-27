# Hello World, Hive

In this lab, you will do a simple search in Hive by performing a word count of _Mary Had a Little Lamb_.

## Objectives

1.  Upload the source data file to HDFS.
2.  Run the provided Hive script.
3.  Observe the word count results.

## Prerequisites

This lab assumes that the student is familiar with the course environment, in particular, the Hortonworks virtual machine.

## Instructions

1.  The first thing we need to do is get our data set into HDFS. From your local computer, run the following command from the `lesson-200-hive-overview` directory:
    
    $ scp lab/mary.txt root@sandbox.hortonworks.com:/tmp
    
2.  Use ssh to login to the virtual machine:
    
    $ ssh root@sandbox.hortonworks.com
    
3.  Now we need to add the data file to HDFS. Run the following command on the virtual machine:
    
    [root@sandbox ~]# sudo -u hdfs hadoop fs -put /tmp/mary.txt /tmp/mary.txt
    [root@sandbox ~]# sudo -u hdfs hadoop fs -chown admin /tmp/mary.txt
    
4.  Click on the _Hive_ button from the Off-canvas menu (in the header bar on top of the page) in the Ambari UI. Select the `default` database if it is not already selected.
5.  Just in case you are not familiar with this nursery rhyme, here is the text of _Mary Had a Little Lamb_.
    
    > Mary had a little lamb its fleece was white as snow and everywhere that Mary went the lamb was sure to go.
    
    Let's see how this word count query looks in Hive.
    
    DROP TABLE IF EXISTS mary;
    DROP TABLE IF EXISTS word_counts;
    
    CREATE TABLE mary (line STRING);
    LOAD DATA INPATH '/tmp/mary.txt' OVERWRITE INTO TABLE mary;
    CREATE TABLE word_counts AS
      SELECT word, count(1) AS count FROM
        (SELECT explode(split(line, ' ')) AS word FROM mary) tempTable
        GROUP BY word;
    
    First, we clean up any existing tables with the two `DROP TABLE` statements. Next, the input data we have is in sentences so we need to convert it to words by using the `split` function with a space as the delimiter. This will result in an array of strings like:
    
    ```
    ['Mary', 'had', 'a', 'little', 'lamb']
    
    ```
    
    To convert the array into multiple rows, the `explode` function is used and the output is created in an intermediate table `tempTable`. The final step is applying the `GROUP BY` clause and `count` function to count word occurrences.
    
    Copy and paste the query into the Hive editor and execute it.
    
6.  Now execute the following query to look at the results.
    
    SELECT * FROM word_counts;
    
    You should see the following results.
    
```markdown
| word_counts.word | word_counts.count |
|------------------|-------------------|
| Mary             | 2                 |
| a                | 1                 |
| and              | 1                 |
| as               | 1                 |
| everywhere       | 1                 |
| fleece           | 1                 |
| go               | 1                 |
| had              | 1                 |
| its              | 1                 |
| lamb             | 1                 |
| little           | 1                 |
| snow             | 1                 |
| sure             | 1                 |
| that             | 1                 |
| the              | 1                 |
| to               | 1                 |
| was              | 2                 |
| went             | 1                 |
| white            | 1                 |
```
    

Congratulations, this lab is complete!
