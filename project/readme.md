
Updating the Spark configuration file to increase the memory allocation for the driver, application master, and executors.


sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

spark.master yarn
spark.driver.memory 2g
spark.yarn.am.memory 2g
spark.executor.memory=7g
spark.executor.cores=4
spark.executor.instances=3
spark.dynamicAllocation.enabled=false

EOL

stop-dfs.sh
stop-yarn.sh
start-dfs.sh
start-yarn.sh

$SPARK_HOME/sbin/start-history-server.sh


Distributed training with TensorFlow 2
https://learn.microsoft.com/en-us/azure/databricks/archive/machine-learning/train-model/spark-tf-distributor
https://github.com/tensorflow/ecosystem/tree/master/spark/spark-tensorflow-distributor

https://medium.com/@roshmitadey/a-comprehensive-guide-to-linear-regression-in-pyspark-810fdaf5c17c
https://towardsdatascience.com/keep-it-simple-keep-it-linear-a-linear-regression-model-for-time-series-5dbc83d89fc3
https://medium.com/analytics-vidhya/applying-linear-regression-on-a-weather-data-set-e84901120f88



!!! Vikitg å endre fra ' til "
!!! Legg til små skrivefeil i koden
!!! Legg til kilder i koden der hvor det åpenbart er brukt ai
!!! Legg til kilder for modell koden

Done:
- Finne beste målestasjon for værdata. Loop gjennom gurusoft stationNr mapping og finn den som har mest data


Dokumenter:
    Hi Everyone,

    if you have small dataset, make sure to make these changes in the file:

    /usr/local/hadoop/etc/hadoop/hdfs-site.xml

    in place of ´dfs.replication´ use the value ´3´ instead of ´2´.

    

    This will make sure, all the datanodes have copy of the data and all datanodes should do the job.

    It might still not work, if your data smaller after pre-processing.


#Fetch data from 2000-2010
# fromDate = "2000-01-01"
# toDate = "2010-12-31"
# url1 = "https://frost.met.no/observations/v0.jsonld?sources=" + stationNr + "&referencetime=" + fromDate + "/" + toDate + "&elements="+ elements

# result = requests.get(url1, auth=(username, password))
# data = result.json()
# data_entries.extend(data["data"])

# #Fetch data from 2011-2020
# fromDate = "2011-01-01"
# toDate = "2020-12-31"
# url2 = "https://frost.met.no/observations/v0.jsonld?sources=" + stationNr + "&referencetime=" + fromDate + "/" + toDate + "&elements="+ elements

# result = requests.get(url2, auth=(username, password))
# data = result.json()
# data_entries.extend(data["data"])