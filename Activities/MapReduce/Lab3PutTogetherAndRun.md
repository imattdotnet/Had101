Now that you've seen how the Mapper and Reducer are written, let's run the example.

In the VM terminal, run the following command.
[hdfs@sandbox lesson-160]$ yarn jar wordcount.jar com.example.WordCount /tmp/a-tale-of-two-cities.txt /tmp/lesson-160
After the job completes running, use the Ambari HDFS Explorer to navigate to /tmp/lesson-160. Inside that directory, you should see a file named part-r-00000 which contains the results of the word count analysis. Download it to your local machine and take a look!

Congratulations, this lab is complete!
