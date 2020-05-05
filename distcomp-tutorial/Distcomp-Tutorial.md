# Hadoop and Spark Tutorial for Statisticians


## Install Hadoop

### Prerequisites

#### SSH

``` {.bash org-language="sh"}
fli@carbon:~$ sudo apt-get install openssh-server
fli@carbon:~$ ssh-keygen -t rsa
fli@carbon:~$ cat ~/.ssh/id_rsa.pub >> authorized_keys
```

#### JDK

``` {.bash org-language="sh"}
fli@carbon:~$ sudo apt-get install openjdk-7-jdk
fli@carbon:~$ java -version
```

#### Get Hadoop

Visit [Hadoop homepage](http://hadoop.apache.org/releases.html) to
download the latest version of Hadoop for Linux.

## Configuring Hadoop


### Core configuration files

The configuration files for Hadoop is at `etc/hadoop`. You have to set
the at least the four core configuration files in order to start Hadoop
properly.

``` {.example}
mapred-site.xml
hdfs-site.xml
core-site.xml
hadoop-env.sh
```

### Important environment variables

You have to set the following environment variables by either editing
your Hadoop `etc/hadoop/hadoop-env.sh` file.

``` {.bash org-language="sh"}
export HADOOP_HOME=~/li_feng/hadoop # This is your Hadoop installation directory
export JAVA_HOME=/usr/lib/jvm/default-java/ #location to Java
# export HADOOP_CONF_DIR=$HADOOP_HOME/lib/native
# export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"
```

-   Single node mode
-   Pseudo mode
-   Cluster mode

## Start and stop Hadoop

### Format HDFS

``` {.bash org-language="sh"}
fli@carbon:~/hadoop/bin$ hdfs namenode -format
```

### Start/Stop HDFS


``` {.bash org-language="sh"}
fli@carbon:~/hadoop/sbin$ start-dfs.sh
```

Namenode information then is accessible from <http://localhost:50070> .
However `sbin/stop-dfs.sh` will stop HDFS.

###　Start/Stop MapReduce


``` {.bash org-language="sh"}
fli@carbon:~/hadoop/sbin$ start-yarn.sh
```

Hadoop administration page then is accessible from
<http://localhost:8088/>. However `sbin/stop-yarn.sh` will stop
MapReduce.

##　Basic Hadoop shell commands


### Create a directory in HDFS

``` {.bash org-language="sh"}
fli@carbon:~/hadoop/bin$ hadoop fs -mkdir /test
```

### Upload a local file to HDFS

``` {.bash org-language="sh"}
fli@carbon:~/hadoop/bin$ hadoop fs -put ~/StudentNameList.xls /test
```

### Check files in HDFS

``` {.example}
fli@carbon:~/hadoop/bin$ hadoop fs -ls /test
```

Type `hadoop fs` to check other basic HDFS data operation commands

``` {.bash org-language="sh"}
fli@carbon:~/hadoop/bin$ hadoop fs
Usage: hadoop fs [generic options]
    [-appendToFile <localsrc> ... <dst>]
    [-cat [-ignoreCrc] <src> ...]
    [-checksum <src> ...]
    [-chgrp [-R] GROUP PATH...]
    [-chmod [-R] <MODE[,MODE]... | OCTALMODE> PATH...]
    [-chown [-R] [OWNER][:[GROUP]] PATH...]
    [-copyFromLocal [-f] [-p] <localsrc> ... <dst>]
    [-copyToLocal [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
    [-count [-q] <path> ...]
    [-cp [-f] [-p | -p[topax]] <src> ... <dst>]
    [-createSnapshot <snapshotDir> [<snapshotName>]]
    [-deleteSnapshot <snapshotDir> <snapshotName>]
    [-df [-h] [<path> ...]]
    [-du [-s] [-h] <path> ...]
    [-expunge]
    [-get [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
    [-getfacl [-R] <path>]
    [-getfattr [-R] {-n name | -d} [-e en] <path>]
    [-getmerge [-nl] <src> <localdst>]
    [-help [cmd ...]]
    [-ls [-d] [-h] [-R] [<path> ...]]
    [-mkdir [-p] <path> ...]
    [-moveFromLocal <localsrc> ... <dst>]
    [-moveToLocal <src> <localdst>]
    [-mv <src> ... <dst>]
    [-put [-f] [-p] <localsrc> ... <dst>]
    [-renameSnapshot <snapshotDir> <oldName> <newName>]
    [-rm [-f] [-r|-R] [-skipTrash] <src> ...]
    [-rmdir [--ignore-fail-on-non-empty] <dir> ...]
    [-setfacl [-R] [{-b|-k} {-m|-x <acl_spec>} <path>]|[--set <acl_spec> <path>]]
    [-setfattr {-n name [-v value] | -x name} <path>]
    [-setrep [-R] [-w] <rep> <path> ...]
    [-stat [format] <path> ...]
    [-tail [-f] <file>]
    [-test -[defsz] <path>]
    [-text [-ignoreCrc] <src> ...]
    [-touchz <path> ...]
    [-usage [cmd ...]]

Generic options supported are
-conf <configuration file>     specify an application configuration file
-D <property=value>            use value for given property
-fs <local|namenode:port>      specify a namenode
-jt <local|jobtracker:port>    specify a job tracker
-files <comma separated list of files>    specify comma separated files to be copied to the map reduce cluster
-libjars <comma separated list of jars>    specify comma separated jar files to include in the classpath.
-archives <comma separated list of archives>    specify comma separated archives to be unarchived on the compute machines.

The general command line syntax is
bin/hadoop command [genericOptions] [commandOptions]
```

### Hadoop task managements

``` {.example}
fli@carbon:~/hadoop/bin$ mapred job
Usage: CLI <command> <args>
    [-submit <job-file>]
    [-status <job-id>]
    [-counter <job-id> <group-name> <counter-name>]
    [-kill <job-id>]
    [-set-priority <job-id> <priority>]. Valid values for priorities are: VERY_HIGH HIGH NORMAL LOW VERY_LOW
    [-events <job-id> <from-event-#> <#-of-events>]
    [-history <jobHistoryFile>]
    [-list [all]]
    [-list-active-trackers]
    [-list-blacklisted-trackers]
    [-list-attempt-ids <job-id> <task-type> <task-state>]. Valid values for <task-type> are REDUCE MAP. Valid values for <task-state> are running, completed
    [-kill-task <task-attempt-id>]
    [-fail-task <task-attempt-id>]
    [-logs <job-id> <task-attempt-id>]

Generic options supported are
-conf <configuration file>     specify an application configuration file
-D <property=value>            use value for given property
-fs <local|namenode:port>      specify a namenode
-jt <local|jobtracker:port>    specify a job tracker
-files <comma separated list of files>    specify comma separated files to be copied to the map reduce cluster
-libjars <comma separated list of jars>    specify comma separated jar files to include in the classpath.
-archives <comma separated list of archives>    specify comma separated archives to be unarchived on the compute machines.

The general command line syntax is
bin/hadoop command [genericOptions] [commandOptions]
```

### Getting help from from Hadoop

Use your web browser to open the file
`hadoop/share/doc/hadoop/index.html` which will guide you to the
document entry for current Hadoop version.

## Hadoop Streaming

### A very simple word count example

``` {.bash org-language="sh"}
fli@carbon:~$ hadoop/bin/hadoop jar \
              ~/hadoop/share/hadoop/tools/lib/hadoop-streaming-2.5.2.jar \
              -input /stocks.txt \
              -output wcoutfile \
              -mapper "/bin/cat" \
              -reducer "/usr/bin/wc" \
```

### Hadoop Streaming with R


#### Write an R script that accepts standard input and output.

See such example `stock_day_avg.R`

``` {.r org-language="R"}
#! /usr/bin/env Rscript

sink("/dev/null")

input <- file("stdin", "r")
while(length(currentLine <- readLines(input, n=1, warn=FALSE)) > 0)
{
    fields <- unlist(strsplit(currentLine, ","))
    lowHigh <- c(as.double(fields[3]), as.double(fields[6]))
    stock_mean <- mean(lowHigh)
    sink()
    cat(fields[1], fields[2], stock_mean, "\n", sep="\t")
    sink("/dev/null")
}

close(input)
```

And you input data file `stocks.txt` looks like the following format.
The complete dataset can be downloaded from <http://finance.yahoo.com/>.

``` {.bash org-language="sh"}
AAPL,2009-01-02,85.88,91.04,85.16,90.75,26643400,90.75
AAPL,2008-01-02,199.27,200.26,192.55,194.84,38542100,194.84
AAPL,2007-01-03,86.29,86.58,81.90,83.80,44225700,83.80
AAPL,2006-01-03,72.38,74.75,72.25,74.75,28829800,74.75
AAPL,2005-01-03,64.78,65.11,62.60,63.29,24714000,31.65
AAPL,2004-01-02,21.55,21.75,21.18,21.28,5165800,10.64
AAPL,2003-01-02,14.36,14.92,14.35,14.80,6479600,7.40
AAPL,2002-01-02,22.05,23.30,21.96,23.30,18910600,11.65
AAPL,2001-01-02,14.88,15.25,14.56,14.88,16161800,7.44
AAPL,2000-01-03,104.87,112.50,101.69,111.94,19144400,27.99
CSCO,2009-01-02,16.41,17.00,16.25,16.96,40980600,16.96
CSCO,2008-01-02,27.00,27.30,26.21,26.54,64338900,26.54
CSCO,2007-01-03,27.46,27.98,27.33,27.73,64226000,27.73
CSCO,2006-01-03,17.21,17.49,17.18,17.45,55426000,17.45
CSCO,2005-01-03,19.42,19.61,19.27,19.32,56725600,19.32
CSCO,2004-01-02,24.36,24.53,24.16,24.25,29955800,24.25
CSCO,2003-01-02,13.11,13.69,13.09,13.64,61335700,13.64
CSCO,2002-01-02,18.44,19.30,18.26,19.23,55376900,19.23
CSCO,2001-01-02,38.13,38.50,32.63,33.31,17384600,33.31
CSCO,2000-01-03,109.94,110.25,103.56,108.06,53076000,54.03
GOOG,2009-01-02,308.60,321.82,305.50,321.32,3610500,321.32
GOOG,2008-01-02,692.87,697.37,677.73,685.19,4306900,685.19
GOOG,2007-01-03,466.00,476.66,461.11,467.59,7706500,467.59
GOOG,2006-01-03,422.52,435.67,418.22,435.23,13121200,435.23
GOOG,2005-01-03,197.40,203.64,195.46,202.71,15844200,202.71
MSFT,2009-01-02,19.53,20.40,19.37,20.33,50084000,19.86
MSFT,2008-01-02,35.79,35.96,35.00,35.22,63004200,33.79
MSFT,2007-01-03,29.91,30.25,29.40,29.86,76935100,28.26
MSFT,2006-01-03,26.25,27.00,26.10,26.84,79973000,25.04
MSFT,2005-01-03,26.80,26.95,26.65,26.74,65002900,24.65
MSFT,2004-01-02,27.58,27.77,27.33,27.45,44487700,22.64
MSFT,2003-01-02,52.30,53.75,51.71,53.72,67025200,21.95
MSFT,2002-01-02,66.65,67.11,65.51,67.04,48124000,27.40
MSFT,2001-01-02,44.13,45.00,42.88,43.38,82413200,17.73
MSFT,2000-01-03,117.37,118.62,112.00,116.56,53228400,47.64
YHOO,2009-01-02,12.17,12.85,12.12,12.85,9514600,12.85
YHOO,2008-01-02,23.80,24.15,23.60,23.72,25671700,23.72
YHOO,2007-01-03,25.85,26.26,25.26,25.61,26352700,25.61
YHOO,2006-01-03,39.69,41.22,38.79,40.91,24227700,40.91
YHOO,2005-01-03,38.36,38.90,37.65,38.18,25482800,38.18
YHOO,2004-01-02,45.50,45.83,45.12,45.40,16480000,22.70
YHOO,2003-01-02,16.59,17.66,16.50,17.60,19640400,8.80
YHOO,2002-01-02,18.14,18.69,17.68,18.63,21903600,9.31
YHOO,2001-01-02,30.31,30.37,27.50,28.19,21939200,14.10
YHOO,2000-01-03,442.92,477.00,429.50,475.00,38469600,118.75
```

#### Your script has to be executable

``` {.bash org-language="sh"}
fli@carbon:~$ chmod +x stock_day_avg.R
```

And very importantly, you have to have your R installed on every worker
node and the necessary R packages should be installed as well.

#### Quick test your file and mapper function

``` {.bash org-language="sh"}
fli@carbon:~$ cat stocks.txt  | stock_day_avg.R
```

#### Upload the data file to HDFS

``` {.bash org-language="sh"}
fli@carbon:~$ hadoop/bin/hadoop fs -put stocks.txt /
```

#### Submitting tasks

``` {.bash org-language="sh"}
fli@carbon:~$ hadoop/bin/hadoop \
              jar ~/hadoop/share/hadoop/tools/lib/hadoop-streaming-2.5.2.jar \
              -input /stocks.txt \
              -output output \
              -file stock_day_avg.R\
              -mapper "stock_day_avg.R"
```

#### View your result

You can either view your result from the web interface or use the
following HDFS command

``` {.bash org-language="sh"}
fli@carbon:~$ hadoop/bin/hdfs dfs -cat /user/fli/output/part-00000
```

### Hadoop Streaming Documentation


The complete Hadoop Streaming Documentation can be found from Hadoop
Installation directory
`share/doc/hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/HadoopStreaming.html`

## Hadoop with Java API


We have the following Jave WordCount version MapReduce program that
counts the number of occurrences of each word in a given input set. This
works with a local-standalone, pseudo-distributed or fully-distributed
Hadoop installation.

``` {.java}
import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class WordCount {

  public static class TokenizerMapper
       extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
      }
    }
  }

  public static class IntSumReducer
       extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                       Context context
                       ) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      result.set(sum);
      context.write(key, result);
    }
  }

  public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    Job job = Job.getInstance(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
  }
}
```

Before we compile our java program. Make sure the following environment
variables are set properly.

``` {.bash org-language="sh"}
export JAVA_HOME=/usr/lib/jvm/default-java/
export PATH=$JAVA_HOME/bin:$PATH
export HADOOP_CLASSPATH=$JAVA_HOME/lib/tools.jar
```

You can check them from the terminal as

``` {.bash org-language="sh"}
echo $HADOOP_CLASSPATH
```

Now we can compile `WordCount.java` and create a jar file

``` {.bash org-language="sh"}
fli@carbon:~/hadoop$ ~/hadoop/bin/hadoop com.sun.tools.javac.Main WordCount.java
fli@carbon:~/hadoop$ jar cf wc.jar WordCount*.class
```

Then you will find a `wc.jar` at the same directory with
`WordCount.java`.

Now let\'s upload some files to HDFS. We make an input directory named
`input` that contains all our files to be counted. We would like to
write all the output to `output` directory.

``` {.bash org-language="sh"}
fli@carbon:~/hadoop$ bin/hadoop fs -mkdir -p WordCount/input
fli@carbon:~/hadoop$ bin/hadoop fs -ls WordCount/input
Found 3 items
-rw-r--r--   1 fli supergroup      15458 2014-12-08 09:45 WordCount/input/LICENSE.txt
-rw-r--r--   1 fli supergroup        101 2014-12-08 09:45 WordCount/input/NOTICE.txt
-rw-r--r--   1 fli supergroup       1366 2014-12-08 09:45 WordCount/input/README.txt
```

Please note that in above commands we have omitted the absolute path. So
`WordCount/input` really means `/user/fli/WordCount/input` in HDFS.

We are going to submit our WordCount program to Hadoop

``` {.bash org-language="sh"}
fli@carbon:~/hadoop$ bin/hadoop jar wc.jar WordCount\
                     WordCount/input \
                     WordCount/output
```

Check the command output message, you will see a line like `Job
job_local1195814039_0001 completed successfully` and you can find the
output at HDFS

``` {.bash org-language="sh"}
fli@carbon:~/hadoop$ ~/hadoop/bin/hadoop fs -cat WordCount/output/*
```

## Statistical Modeling with Hadoop


### Linear Regression Models.


The core algorithm for linear regression modeling is to code up a
mapreduce procedure for X\'Y and X\'X. One can decompose this into many
submatrix multiplications and sum them over in the end. See the lecture
notes for details.

### Logistic Regression Models

You will need to code up your own algorithm for estimating the
coefficients in the model. You can use the RHadoop API or Mahout.

### RHadoop

RHadoop is a collection of five R packages that allow users to manage
and analyze data with Hadoop. Examples and helps can be found from
<https://github.com/RevolutionAnalytics/RHadoop/wiki>


### Via approximations.

See lecture notes.

## Statistical Learning with Mahout

### Quick Install Mahout

#### Use the binary release

Please visit <https://mahout.apache.org/> to download the latest binary
version (currently 0.9 is the release version) of Mahout. But remember
that this version does not work well with Hadoop 2.5.2.

#### Compile your mahout that matches your hadoop

Instead of using the binary version, one may need to compile mahout to
match the system hadoop (version 2.x). NOTE: If you don\'t have the
newest maven installed, you have to install maven firstly. Please refer
to the maven guide.

Make sure you have `maven` and `git` installed in your system

``` {.bash org-language="sh"}
fli@carbon:~$ sudo apt-get install maven git
```

You need to clone the newest mahout from the repository with `git`

``` {.bash org-language="sh"}
fli@carbon:~$ git clone --branch master git://github.com/apache/mahout.git mahout
```

Now compile and pack mahout with Hadoop 2.x. This take a while

``` {.bash org-language="sh"}
fli@carbon:~$ cd mahout
fli@carbon:~/mahout$ mvn -Dhadoop2.version=2.5.2 clean compile
fli@carbon:~/mahout$ mvn -Dhadoop2.version=2.5.2 -DskipTests=true clean package
fli@carbon:~/mahout$ mvn -Dhadoop2.version=2.5.2 -DskipTests=true
clean install
```

#### Set up the necessary environment variables


Make sure the following environment variables are set properly

``` {.bash org-language="sh"}
export MAHOUT_HOME=$HOME/mahout/
export MAHOUT_CONF_DIR=$MAHOUT_HOME/conf/
```

To integrate Mahout with Hadoop, make sure your Hadoop is installed
properly and the following environment variables are correctly
specified.

``` {.bash org-language="sh"}
export HADOOP_HOME=$HOME/hadoop/
export HADOOP_CLASSPATH=$JAVA_HOME/lib/tools.jar
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop/
```

Note: There is a special environment variable `MAHOUT_LOCAL`. If it is
set to not empty value. Mahout will run locally.

After installation, you will find all possible algorithms in your
version.

``` {.bash org-language="sh"}
fli@carbon:~/mahout$ bin/mahout
MAHOUT_LOCAL is not set; adding HADOOP_CONF_DIR to classpath.
Running on hadoop, using /home/fli/hadoop//bin/hadoop and HADOOP_CONF_DIR=/home/fli/hadoop//etc/hadoop/
MAHOUT-JOB: /home/fli/mahout/mahout-examples-0.9-job.jar
An example program must be given as the first argument.
Valid program names are:
  arff.vector: : Generate Vectors from an ARFF file or directory
  baumwelch: : Baum-Welch algorithm for unsupervised HMM training
  canopy: : Canopy clustering
  cat: : Print a file or resource as the logistic regression models would see it
  cleansvd: : Cleanup and verification of SVD output
  clusterdump: : Dump cluster output to text
  clusterpp: : Groups Clustering Output In Clusters
  cmdump: : Dump confusion matrix in HTML or text formats
  concatmatrices: : Concatenates 2 matrices of same cardinality into a single matrix
  cvb: : LDA via Collapsed Variation Bayes (0th deriv. approx)
  cvb0_local: : LDA via Collapsed Variation Bayes, in memory locally.
  evaluateFactorization: : compute RMSE and MAE of a rating matrix factorization against probes
  fkmeans: : Fuzzy K-means clustering
  hmmpredict: : Generate random sequence of observations by given HMM
  itemsimilarity: : Compute the item-item-similarities for item-based collaborative filtering
  kmeans: : K-means clustering
  lucene.vector: : Generate Vectors from a Lucene index
  lucene2seq: : Generate Text SequenceFiles from a Lucene index
  matrixdump: : Dump matrix in CSV format
  matrixmult: : Take the product of two matrices
  parallelALS: : ALS-WR factorization of a rating matrix
  qualcluster: : Runs clustering experiments and summarizes results in a CSV
  recommendfactorized: : Compute recommendations using the factorization of a rating matrix
  recommenditembased: : Compute recommendations using item-based collaborative filtering
  regexconverter: : Convert text files on a per line basis based on regular expressions
  resplit: : Splits a set of SequenceFiles into a number of equal splits
  rowid: : Map SequenceFile<Text,VectorWritable> to {SequenceFile<IntWritable,VectorWritable>, SequenceFile<IntWritable,Text>}
  rowsimilarity: : Compute the pairwise similarities of the rows of a matrix
  runAdaptiveLogistic: : Score new production data using a probably trained and validated AdaptivelogisticRegression model
  runlogistic: : Run a logistic regression model against CSV data
  seq2encoded: : Encoded Sparse Vector generation from Text sequence files
  seq2sparse: : Sparse Vector generation from Text sequence files
  seqdirectory: : Generate sequence files (of Text) from a directory
  seqdumper: : Generic Sequence File dumper
  seqmailarchives: : Creates SequenceFile from a directory containing gzipped mail archives
  seqwiki: : Wikipedia xml dump to sequence file
  spectralkmeans: : Spectral k-means clustering
  split: : Split Input data into test and train sets
  splitDataset: : split a rating dataset into training and probe parts
  ssvd: : Stochastic SVD
  streamingkmeans: : Streaming k-means clustering
  svd: : Lanczos Singular Value Decomposition
  testnb: : Test the Vector-based Bayes classifier
  trainAdaptiveLogistic: : Train an AdaptivelogisticRegression model
  trainlogistic: : Train a logistic regression using stochastic gradient descent
  trainnb: : Train the Vector-based Bayes classifier
  transpose: : Take the transpose of a matrix
  validateAdaptiveLogistic: : Validate an AdaptivelogisticRegression model against hold-out data set
  vecdist: : Compute the distances between a set of Vectors (or Cluster or Canopy, they must fit in memory) and a list of Vectors
  vectordump: : Dump vectors from a sequence file to text
  viterbi: : Viterbi decoding of hidden states from given output states sequence
```

#### Run a Mahout Job


-   Let Hadoop/HDFS up and run
-   Upload data to HDFS
-   Run the example

Assume you have uploaded a text data
<http://archive.ics.uci.edu/ml/databases/synthetic_control/synthetic_control.data>
to HDFS\'s user directory `testdata`

You may run the command by calling Mahout directly which invokes Hadoop
from the back,

``` {.bash org-language="sh"}
fli@carbon:~$ mahout/bin/mahout org.apache.mahout.clustering.syntheticcontrol.canopy.Job
```

Or one can call Mahout from Hadoop

``` {.bash org-language="sh"}
fli@carbon:~$ hadoop/bin/hadoop jar \
              $MAHOUT_HOME/examples/target/mahout-examples-1.0-SNAPSHOT-job.jar \
              org.apache.mahout.clustering.syntheticcontrol.canopy.Job
```

The output will be at your `output` directory under your HDFS user
directory. For more information about this example, please visit
<https://mahout.apache.org/users/clustering/canopy-clustering.html>

#### Mahout build-in examples


There are a lot ready-to-use examples at `mahout/examples/bin`
directory. Just run e.g.

``` {.bash org-language="sh"}
fli@carbon:~ mahout/examples/bin/classify-20newsgroups.sh
```

#### Classification with random forests

We will run the random forests algorithm with Mahout 1.0 and Hadoop
2.5.2.

#### Upload the data to HDFS\'s directory

``` {.bash org-language="sh"}
fli@carbon:~$ ~/hadoop/bin/hadoop fs -put KDD* testdata
fli@carbon:~$ ~/hadoop/bin/hadoop fs -ls testdata
Found 2 items
-rw-r--r--   1 fli supergroup    3365886 2014-12-14 17:32 testdata/KDDTest+.arff
-rw-r--r--   1 fli supergroup   18742306 2014-12-14 17:32 testdata/KDDTrain+.arff
```

#### Generate the dataset description

``` {.bash org-language="sh"}
fli@carbon:~$ ~/hadoop/bin/hadoop jar \
              $MAHOUT_HOME/examples/target/mahout-examples-1.0-SNAPSHOT-job.jar \
              org.apache.mahout.classifier.df.tools.Describe \
              -p testdata/KDDTrain+.arff \
              -f testdata/KDDTrain+.info  \
              -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L
```

where the \"N 3 C 2 N C 4 N C 8 N 2 C 19 N L\" string describes all the
attributes of the data. In this cases, it means 1 numerical(N)
attribute, followed by 3 Categorical(C) attributes, ...L indicates the
label.

A file named `KDDTrain+.info` will be generated and stored in `testdata`
directory. Check it with

``` {.bash org-language="sh"}
fli@carbon:~$ ~/hadoop/bin/hadoop fs -cat testdata/*.info
```

#### Build the model

We will try to build 100 trees (-t argument) using the partial
implementation (-p). Each tree is built using 5 random selected
attribute per node (-sl argument) and the example outputs the decision
tree in the \"nsl-forest\" directory (-o).

The number of partitions is controlled by the -Dmapred.max.split.size
argument that indicates to Hadoop the max. size of each partition, in
this case 1/10 of the size of the dataset. Thus 10 partitions will be
used. IMPORTANT: using less partitions should give better classification
results, but needs a lot of memory.

``` {.bash org-language="sh"}
fli@carbon:~$ ~/hadoop/bin/hadoop jar \
              $MAHOUT_HOME/examples/target/mahout-examples-1.0-SNAPSHOT-job.jar \
              org.apache.mahout.classifier.df.mapreduce.BuildForest \
              -Dmapred.max.split.size=1874231 \
              -d testdata/KDDTrain+.arff \
              -ds testdata/KDDTrain+.info \
              -sl 5 -p -t 100 -o nsl-forest
```

A directory named `nsl-forest` will be generated that contains all the
model parameters.

#### Use the model to classify new data

Now we can compute the predictions of \"KDDTest+.arff\" dataset (-i
argument) using the same data descriptor generated for the training
dataset (-ds) and the decision forest built previously (-m). Optionally
(if the test dataset contains the labels of the tuples) run the analyzer
to compute the confusion matrix (-a), and you can also store the
predictions in a text file or a directory of text files(-o). Passing the
(-mr) parameter will use Hadoop to distribute the classification.

``` {.bash org-language="sh"}
fli@carbon:~$ ~/hadoop/bin/hadoop jar \
              $MAHOUT_HOME/examples/target/mahout-examples-1.0-SNAPSHOT-job.jar \
              org.apache.mahout.classifier.df.mapreduce.TestForest \
              -i testdata/KDDTest+.arff \
              -ds testdata/KDDTrain+.info \
              -m nsl-forest  \
              -a -mr \
              -o predictions
```

which will return the following summary (as below) and the result will
be stored in the `predictions` directory.

``` {.bash org-language="sh"}
=======================================================
Summary
-------------------------------------------------------
Correctly Classified Instances          :      17162       76.1267%
Incorrectly Classified Instances        :       5382       23.8733%
Total Classified Instances              :      22544

=======================================================
Confusion Matrix
-------------------------------------------------------
a       b       <--Classified as
8994    717      |  9711    a     = normal
4665    8168     |  12833   b     = anomaly

=======================================================
Statistics
-------------------------------------------------------
Kappa                                        0.536
Accuracy                                   76.1267%
Reliability                                52.0883%
Reliability (standard deviation)            0.4738
Weighted precision                          0.8069
Weighted recall                             0.7613
Weighted F1 score                           0.7597
```

If you have any question concerning with random forests, read Chapter 15
of [The Elements of Statistical
Learning](http://statweb.stanford.edu/~tibs/ElemStatLearn/)

## Introduction to Spark

# Spark Shell

## Interactive Analysis with the Spark Shell

-   Spark\'s shell provides a simple way to learn the API, as well as a
    powerful tool to analyze data interactively. It is available in
    either Scala (which runs on the Java VM and is thus a good way to
    use existing Java libraries) or Python.

-   Start the Python version with exactly 4 cores by running the
    following in the Spark directory:

``` {.bash org-language="sh"}
./bin/pyspark --master local[4]
```

To find a complete list of options, run `pyspark --help`.

-   Start the Scala version by running the following in the Spark
    directory:

``` {.bash org-language="sh"}
./bin/spark-shell
```

-   All examples based on this section will be based on Python. One may
    also check out the Scala version at
    <http://spark.apache.org/docs/latest/programming-guide.html>

-   Spark\'s primary abstraction is a distributed collection of items
    called a Resilient Distributed Dataset (RDD). RDDs can be created
    from Hadoop InputFormats (such as HDFS files) or by transforming
    other RDDs.

-   To make a new RDD from the text of the README file in the Spark
    source directory:

``` {.bash org-language="sh"}
>>> textFile = sc.textFile("README.md")
```

-   RDDs have actions, which return values, and transformations, which
    return pointers to new RDDs.

``` {.bash org-language="sh"}
>>> textFile.count() # Number of items in this RDD
126

>>> textFile.first() # First item in this RDD
u'# Apache Spark'

```

-   RDD actions and transformations can be used for more complex
    computations. Let's say we want to find the line with the most
    words:

``` {.bash org-language="sh"}
>>> textFile.map(lambda line: len(line.split())).reduce(lambda a, b: a if (a > b) else b)
15
```

-   Spark also supports pulling data sets into a cluster-wide in-memory
    cache. This is very useful when data is accessed repeatedly

``` {.bash org-language="sh"}
>>> linesWithSpark.cache()

>>> linesWithSpark.count()
15

>>> linesWithSpark.count()
15
```

## Standalone Applications


-   Assume we like to write a program that just counts the number of
    lines containing \'a\' and the number containing \'b\' in the Spark
    README.

## The Python version

``` {.bash org-language="sh"}
"""SimpleApp.py"""
from pyspark import SparkContext

logFile = "YOUR_SPARK_HOME/README.md"  # some file on system
sc = SparkContext("local", "Simple App")
logData = sc.textFile(logFile).cache()

numAs = logData.filter(lambda s: 'a' in s).count()
numBs = logData.filter(lambda s: 'b' in s).count()

print "Lines with a: %i, lines with b: %i" % (numAs, numBs)
```

## The Java version

``` {.java}
/* SimpleApp.java */

import org.apache.spark.api.java.*;
import org.apache.spark.SparkConf;
import org.apache.spark.api.java.function.Function;

public class SimpleApp {
  public static void main(String[] args) {
    String logFile = "YOUR_SPARK_HOME/README.md"; // Should be some file on your system
    SparkConf conf = new SparkConf().setAppName("Simple Application");
    JavaSparkContext sc = new JavaSparkContext(conf);
    JavaRDD<String> logData = sc.textFile(logFile).cache();

    long numAs = logData.filter(new Function<String, Boolean>() {
      public Boolean call(String s) { return s.contains("a"); }
    }).count();

    long numBs = logData.filter(new Function<String, Boolean>() {
      public Boolean call(String s) { return s.contains("b"); }
    }).count();

    System.out.println("Lines with a: " + numAs + ", lines with b: " + numBs);
  }
}
```

## The Scala version

``` {.java}
/* SimpleApp.scala */
import org.apache.spark.SparkContext
import org.apache.spark.SparkContext._
import org.apache.spark.SparkConf

object SimpleApp {
  def main(args: Array[String]) {
    val logFile = "YOUR_SPARK_HOME/README.md" // Should be some file on your system
    val conf = new SparkConf().setAppName("Simple Application")
    val sc = new SparkContext(conf)
    val logData = sc.textFile(logFile, 2).cache()
    val numAs = logData.filter(line => line.contains("a")).count()
    val numBs = logData.filter(line => line.contains("b")).count()
    println("Lines with a: %s, Lines with b: %s".format(numAs, numBs))
  }
}
```

### Submitting Applications to Spark


#### Bundling Your Application\'s Dependencies

-   If your code depends on other projects, you will need to package
    them alongside your application in order to distribute the code to a
    Spark cluster.

-   To do this, to create an assembly jar containing your code and its
    dependencies. When creating assembly jars, list Spark and Hadoop as
    provided dependencies; these need not be bundled since they are
    provided by the cluster manager at runtime.

-   For Python, you can use the `--py-files` argument of `spark-submit`
    to add .py, .zip or .egg files to be distributed with your
    application. If you depend on multiple Python files, pack them into
    a .zip or .egg.

-   Once a user application is bundled, it can be launched using the

`bin/spark-submit` script.

#### Run Your Application

-   Run application locally on 8 cores

``` {.bash org-language="sh"}
./bin/spark-submit \
  --class org.apache.spark.examples.SparkPi \
  --master local[8] \
  /path/to/examples.jar \
  100
```

-   Run on a Spark standalone cluster

``` {.bash org-language="sh"}
./bin/spark-submit \
  --class org.apache.spark.examples.SparkPi \
  --master spark://207.184.161.138:7077 \
  --executor-memory 20G \
  --total-executor-cores 100 \
  /path/to/examples.jar \
  1000
```

-   Run on a Hadoop YARN cluster

``` {.bash org-language="sh"}
export HADOOP_CONF_DIR=XXX
./bin/spark-submit \
  --class org.apache.spark.examples.SparkPi \
  --master yarn-cluster \  # can also be `yarn-client` for client mode
  --executor-memory 20G \
  --num-executors 50 \
  /path/to/examples.jar \
  1000
```

-   Run a Python application on a cluster

``` {.bash org-language="sh"}
./bin/spark-submit \
  --master spark://207.184.161.138:7077 \
  examples/src/main/python/pi.py \
  1000
```
