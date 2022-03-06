# StackExchangeInvertedIndex
<h1> Overview </h1>
<p>This is an inverted index of PostID, Tags, Scores, and ClosedDates queried from StackExchange, 
run through PySpark,
report written in Jupyter Notebooks 
via GCP Dataproc, VMInstances, and Google Storage Bucket 😁🍸. </p>

<h2> Updates </h2>
<p><ul>
  <li>Update 3/3/2022 - I initially chose GCP GCloud but there were many bugs. I decided to shift my work over to AWS EMR.</li> 
  <li>Update 3/4/2022 - Decided I did not want to deal with the cloud today. Switched to local via hortonworks and docker.</li>
  <li>Update 3/5/2022 - <a href=https://www.youtube.com/c/Codible/about>This wonderful youtuber</a> set me on the right path with GCP</li>
    <ul>
        <li>- Watch this video by Codible for help Connecting to a Hadoop Cluster on Google Dataproc with Jupyter Notebook: <a href=https://youtu.be/SN2VDCBzSlE>Click Here</a></li>
        <li>- Watch this video by Codible for help Using PySpark on Dataproc Hadoop Cluster to process large CSV file: <a href=https://youtu.be/Y6OXGc0mmYM>Click Here</a></li>
  </ul>
  </ul></p>
  
 <h2> Execution </h2>
  
  <p> First, I went to <a href=data.stackexchange.com>data.stackexchange.com</a> and clicked <b>Compose Query</b>. <br>
  <br>
  I typed in the following sql-like code into the textbox, ran and downloaded the query as a csv. <br>
  <br>
  <code>
    select Id, Tags, Score, ClosedDate
    from Posts
    where CreationDate like '%2021%';
  </code></p>
  <p> After setting up this GitHub repository I committed <code> QueryResults.csv </code> here.
  
  <p> Then, I set up a Google Cloud Platform Dataproc with 1 Master Node and 2 Worker Nodes. I loaded it with an Ubuntu 1.5 image, alloting 50 GB to each node. I included with Jupyter and Anaconda for this project. I also created a new Google Storage bucket in order to save my jupyter-notebook and inverted indexes for downloading later. </p>
  <p> After my VMInstances provisioned I opened the SSH to my Worker Node and the Terminal from JupyterLab. From the terminal I downloaded the csv to my VMInstance: <br>
  <code> wget https://github.com/kscully-dotcom/StackExchangeInvertedIndex/raw/main/QueryResults.csv </code><br>
  <br> 
  and saved it to my Google Storage Bucket: <br> 
  <code> gsutil cp QueryResults.csv gs://usr-1-google-storage-bucket-path/data </code><br>
  <br>
  Then I started a new jupyter-lab project and used the PySpark terminal pre-built into my VMInstance. Here are some useful links that helped me throughout this project:<br>
<ul>
  <li><b><a href=https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html>PySpark Spark SQL Documentation</a></b></li>
  <li><b><a href=https://spark.apache.org/docs/latest/api/python/reference/pyspark.html>PySpark Spark Core Documentation</a></b></li>
  <li><b><a href=https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.RDD.html#pyspark.RDD>PySpark RDD Documentation</a></b></li>
  <li><b><a href=https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.RDD.reduceByKey.html#pyspark.RDD.reduceByKey>PySpark reduceByKey Documentation</a></b></li>
  <li><b><a href=https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.withColumn.html#pyspark.sql.DataFrame.withColumn>PySpark withColumn Documentation</a></b></li>
  <li><b><a href=https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.regexp_replace.html#pyspark.sql.functions.regexp_replace>Pyspark regexp_replace Documentation</a></b></li>
  <li><b><a href=https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.RDD.map.html>PySpark map Documentation</a></b></li>
  <li><b><a href=https://stackoverflow.com/questions/56676701/pyspark-applying-reduce-by-key-to-the-values-of-an-rdd>StackOverflow Explanation of PySpark Reduce</a></b><</li>
  <li><b><a href=https://github.com/MingChen0919/learning-apache-spark/blob/master/notebooks/01-data-strcture/1.3-conversion-between-rdd-and-dataframe.ipynb>MingChen0919 Provides Useful Tips on PySpark SparkContext and Shifting between RDDs and Dataframes</a></b></li>
  <li><b><a href=http://hadooptutorial.info/hadoop-output-formats/>Hadoop Output Formats</a></b></li>
  <li><b><a href=https://stackoverflow.com/questions/31385363/how-to-export-a-table-dataframe-in-pyspark-to-csv>Saving Pyspark RDD as a CSV File</a></b> <br>This is not always recommended if the dataset is very large and will put too much weight on your CPU. In my case the InvertedIndex CSV files were not astronomically large. I would still recommend parsing the information needed from your output file before saving a TextFile to your computer system. Here is an easy one-liner to save your RDD as a CSV. </li>
  </ul>
  </p>
 <h2> Summary </h2>
  <p> This exercise was a great way to introduce myself (a new data scientist) to the Data Architecture of large amounts of scalable data and how to parse through and extract the answers I -or a client- would need. I scraped web data in one very simple way via StackExhange. A similar approach would have been to use Google's BigQuery. For other examples I may use an API or the web's html. I also used Google Cloud Platform's Dataproc and Google Storage Bucket to organize this project. Another approach would have been to use AWS EMR or Locally hosting using a Virtual Machine. I decided to use Google Cloud Platform because I had already been familiar with it's tools and User Interface. AWS EMR seemed very clear and straightforward but needed more setup. I do recommend using a cloud service because of the capacity and demand of the data transactions on a local CPU. For example, I tried to set up a local Virtual Machine to runPyspark and the Virtual Machine alone was recommended to allot 10GB of my disk space to the virtual machine. This was not a safe amount of storage to take away from my CPU and 8GB was enough to shut down my Virtual Machine after installing Anaconda and Hadoop. Keep in mind I have 16GB of RAM (not a lot but will be upgrading soon) and 23GB of Virtual Memory on a  custom built PC with no pre-installed bloatware. </p>
<h2> Purpose </h2>
 <p> The purpose of this exercise was to get familiar with the Apache Hadoop and Spark Data Engineering Tools. One of the first exercises many Data Scientists will do with MapReduce is a simple WordCount exercise. This InvertedIndex exercise is an extension off of the MapReduce WordCount exercise. </p>
  <h2> What we Learned </h2>
  <p>
  We learned how to setup Hadoop Clusters and run MapReduce transactions on RDD's with PySpark. We also learned how to organize a datalake. We practiced webscraping. We learned the expense on a local computer's memory of an organized datalake and running transactions and why it is important to utilize a portion of the computer's disk space to run these programs.
  </p>
<h2> What can be better </h2>
<p> The code can be cleaner, and with more time it will be. We can use our InvertedIndexes to plot data visualizations and answer data questions. The Hadoop cluster organization can also be improved.</p>
<h2> Why is this useful </h2>
<p>
When we work for a large corporation, extract large amounts of data, have constantly scaling and updating data (i.e., weather data, financial transactions, energy data, etc.) we won't have the human or machine capacity to hold and organize it all. That's why we need ways to reduce and minimize the data across nodes, develop data lakes, and have them work efficiently enough that we can query that data efficiently and without expensive transactions to the system. Having development tools like Spark, Docker, Hadoop, Cassandra, etc. allows the data scientists, engineers, and business executives the ability to run programs in the background that organizes the data, collects the data for analysis, and to quickly query data to answer impactful business questions for clients and stakeholders. These tools allow an inexpensive and efficient solution for all of these necessary-evil problems.</p>

