# Spark Configurations

## Configurations for Pre-Processing and Linear Regression
1. Config 1
```
    sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

    spark.master yarn
    spark.driver.memory 2g
    spark.yarn.am.memory 2g
    spark.executor.memory=4g
    spark.executor.cores=4
    spark.executor.instances=3
    spark.task.cpus=4
    spark.dynamicAllocation.enabled=false

    EOL
```

2. Config 2
```
    sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

    spark.master yarn
    spark.driver.memory 2g
    spark.yarn.am.memory 2g
    spark.executor.memory=4g
    spark.executor.cores=4
    spark.executor.instances=3
    spark.task.cpus=1
    spark.dynamicAllocation.enabled=false

    EOL
```

3. Config 3
```
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
```

4. Config 4
```
    sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

    spark.master yarn
    spark.driver.memory 2g
    spark.yarn.am.memory 2g
    spark.executor.memory=4g
    spark.executor.cores=4
    spark.task.cpus=2
    spark.executor.instances=3
    spark.dynamicAllocation.enabled=false

    EOL
```

## Configurations for LSTM and CNN
1. Config 1
```
    sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

    spark.master yarn
    spark.driver.memory 4g
    spark.yarn.am.memory 2g
    spark.executor.memory=4g
    spark.executor.cores=4
    spark.executor.instances=3
    spark.task.cpus=2
    spark.dynamicAllocation.enabled=false

    EOL
    # Number of slots: 3
    runner = MirroredStrategyRunner(num_slots=3, use_gpu=False)

```
2. Config 2
```
    sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

    spark.master yarn
    spark.driver.memory 4g
    spark.yarn.am.memory 2g
    spark.executor.memory=4g
    spark.executor.cores=4
    spark.executor.instances=3
    spark.task.cpus=1
    spark.dynamicAllocation.enabled=false

    EOL
    # Number of slots: 9
    runner = MirroredStrategyRunner(num_slots=9, use_gpu=False)

```

3. Config 3
```
    sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

    spark.master yarn
    spark.driver.memory 4g
    spark.yarn.am.memory 2g
    spark.executor.memory=1000m
    spark.executor.cores=2
    spark.executor.instances=6
    spark.task.cpus=1
    spark.dynamicAllocation.enabled=false

    EOL
    # Number of slots: 6
    runner = MirroredStrategyRunner(num_slots=6, use_gpu=False)

```


