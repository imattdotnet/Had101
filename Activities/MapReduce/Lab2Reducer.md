You then are required to override a method called map that performs the actual work of the mapper function.

public static class Reduce extends Reducer<Text, IntWritable, Text, IntWritable> {

    public void reduce(Text key, Iterable<IntWritable> values, Context context)
            throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
            sum += val.get();
        }
        context.write(key, new IntWritable(sum));
    }
}
The Reduce class extends Hadoop's Reducer API. Again, within the angle brakcets are the data types first for the input key and value followed by the emitted key and value. The reduce method is overridden for the reducer work.

After that, the main method just sets up the configuration of the Hadoop Job object which is the driver of the whole operation. There are two parameters required which are the HDFS location of the input file and then the HDFS outputlocation.

/**
 * args[0] - HDFS input path
 * args[1] - HDFS output path
 */
public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();

    Job job = Job.getInstance(conf, "WordCount");

    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);

    job.setMapperClass(Map.class);
    job.setReducerClass(Reduce.class);

    job.setInputFormatClass(TextInputFormat.class);
    job.setOutputFormatClass(TextOutputFormat.class);

    FileInputFormat.addInputPath(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));

    job.setJar("wordcount.jar");
    job.waitForCompletion(true);
}

