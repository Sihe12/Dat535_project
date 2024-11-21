Dupliserte hver linjen 150 ganger for å gjøre det enda mer data.

Endret da fra map til flatMap

Så fra 11.6Mb til nå 1.7 G  5.1 G  /project/raw_temperature_data


Data cleaning tidligere:
Execution time: 0.0023403167724609375
[Stage 3:>                                                          (0 + 1) / 1]
('2000-01-01', (-6.3, 0.02, 1.0))
('2000-01-01', (-4.0, 0.02, 1.0))
('2000-01-01', (-7.0, 0.02, 1.0))
('2000-01-02', (-4.4, 0.03, 1.0))
('2000-01-02', (2.1, 0.03, 1.0))
('2000-01-02', (-0.8, 0.03, 1.0))
('2000-01-03', (0.4, 0.05, 1.0))
('2000-01-03', (3.2, 0.05, 1.0))
('2000-01-03', (2.5, 0.05, 1.0))
('2000-01-04', (2.2, 0.07, 1.0))

Data cleaning nå:




Hi Everyone,

    if you have small dataset, make sure to make these changes in the file:

    /usr/local/hadoop/etc/hadoop/hdfs-site.xml

    in place of ´dfs.replication´ use the value ´3´ instead of ´2´.

    

    This will make sure, all the datanodes have copy of the data and all datanodes should do the job.

    It might still not work, if your data smaller after pre-processing.



-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



Dokumentasjon

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Spark Configuration Information

Each node has a total memory of 8gb and 4 cores. 
So since each node needs a 10% overhead we set the total memory to 7gb and 4 cores.
This will keep the total memory requested per executor to approximately 7.7 GB (7 GB + 700 MB overhead), which fits under the 8 GB max allocation.

We set the number of executors to 3 since we have 3 nodes in our cluster.

We set the number of cores per executor to 4 since each node has 4 cores.

We experiment with the number of tasks per core to see how it affects the performance of our spark jobs.

We set the dynamic allocation to false since we want to control the number of executors and cores.

We set the replication factor to 3 in the hdfs-site.xml file to ensure that all datanodes have a copy of the data and all datanodes should do the job.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Data Ingestion
Since we want to simulate a real world scenario and profile and tune our spark jobs, we will multiply each entry in the dataset by 150. This will increase the size of the dataset from 11.6Mb to 1.7 G  5.1 G  /project/raw_temperature_data. This will allow us to more easily test the performance of our spark jobs. 


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Data Cleaning

1. Zero parrallelism. Only one task per datanode. Therefore, the task per cpu is set to 4 since 4 cores are available on each node. Therefore the processing is done sequentially per node, but in 3 in parallel across nodes.
sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

spark.master yarn
spark.driver.memory 2g
spark.yarn.am.memory 2g
spark.executor.memory=7g
spark.executor.cores=4
spark.executor.instances=3
spark.task.cpus=4
spark.dynamicAllocation.enabled=false

EOL

    Total time: 2.8 min
    Group by function: 
        Total time: 2 min
        Total time across all tasks:3.8 min

        Ony two datanodes are used.


2. The executors with each having max parallelism . One task per core. Therefore, the task per cpu is set to 1 since 4 cores are available on each node. Therefore the processing is also done in parallel per node.

sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

spark.master yarn
spark.driver.memory 2g
spark.yarn.am.memory 2g
spark.executor.memory=7g
spark.executor.cores=4
spark.executor.instances=3
spark.task.cpus=1
spark.dynamicAllocation.enabled=false

EOL

    Total time: 1.5 min
    Group by function: 
        Total time: 43 sec
        Total time across all tasks: 4.2 min
        The total time across all tasks is the sum of all tasks done in parallel across all nodes. This is a greate example of how parallelism can speed up the processing time of a spark job.

        Only two datanodes are used.

1.2 Explanation: Only 2 datanodes are used 

3. Max parallelism.
sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

spark.master yarn
spark.driver.memory 2g
spark.yarn.am.memory 2g
spark.executor.memory=1000m
spark.executor.cores=1
spark.executor.instances=12
spark.task.cpus=1
spark.dynamicAllocation.enabled=false

EOL

    Total time: 1.3 min
    Group by function: 
        Total time: 44 sec
        Total time across all tasks: 4.5 min
        The total time across all tasks is the sum of all tasks done in parallel across all nodes. This is a greate example of how parallelism can speed up the processing time of a spark job.

        All three datanodes are used.
        Total number of executors: 10. This is due to the hardware constraints of the datanodes.

     Aggregated Metrics by Executor

4. Best performance.
sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

spark.master yarn
spark.driver.memory 2g
spark.yarn.am.memory 2g
spark.executor.memory=2500m
spark.executor.cores=4
spark.task.cpus=2
spark.executor.instances=5
spark.dynamicAllocation.enabled=true
spark.dynamicAllocation.minExecutors=3
spark.dynamicAllocation.maxExecutors=10
spark.dynamicAllocation.initialExecutors=5
spark.dynamicAllocation.executorIdleTimeout=20s
spark.dynamicAllocation.schedulerBacklogTimeout=1s
spark.dynamicAllocation.sustainedSchedulerBacklogTimeout=10s
spark.dynamicAllocation.executorAllocationRatio=1.0

EOL

    Total time: 1.2 min
    Group by function: 
        Total time: 39 sec
        Total time across all tasks: 4.3 min
        The total time across all tasks is the sum of all tasks done in parallel across all nodes. This is a greate example of how parallelism can speed up the processing time of a spark job.

        All three datanodes are used.
        Total number of executors: 5.

        This combination of executor memory, cores and instances gives the best performance for our spark job. This is due to the hardware constraints of the datanodes.


Different configurations of spark jobs can have a significant impact on the performance of the job. By experimenting with different configurations, we can optimize the performance of our spark jobs and reduce the time it takes to process our data. This can be especially important when working with large datasets, where even small improvements in performance can lead to significant time savings. By understanding the hardware constraints of our cluster and the characteristics of our data, we can choose the configuration that best fits our needs and maximize the performance of our spark jobs.

Different Algorithm

Initial Algo used on the above tuning: daily_avg_rdd = processed_rdd.groupByKey().mapValues(lambda temps: list(temps)) \
    .mapValues(lambda temps: (round(statistics.mean([t[0] for t in temps])), temps[0][1], temps[0][2]))

New Algo: daily_avg_rdd = processed_rdd \
    .map(lambda t: (t[0], (t[1][0], t[1][1], t[1][2], 1))) \
    .reduceByKey(lambda a, b: (a[0] + b[0], a[1], a[2], a[3] + b[3])) \
    .mapValues(lambda v: (round(v[0] / v[3]), v[1], v[2]))

    Total time: 1.1 min 
    Group by function
        Total time: 41 sec
        Total time across all tasks: 4.3 min

        All three datanodes are used.
        Total number of executors: 5.


        We thouth the new algorithm would be faster since it uses the reduceByKey function which is optimized for aggregating data. However, the performance of the new algorithm is a bit slower compared to the old algorithm. This is because the old algorithm is already optimized for the data and the new algorithm does not provide any significant performance improvements. This shows that it is important to test different algorithms and configurations to find the best combination for our spark jobs.



-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Model Training

LSTM Model

sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

spark.master yarn
spark.driver.memory 2g
spark.yarn.am.memory 2g
spark.executor.memory=7g
spark.executor.cores=4
spark.executor.instances=3
spark.task.cpus= 2
spark.dynamicAllocation.enabled=false

EOL
