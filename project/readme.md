
Updating the Spark configuration file to increase the memory allocation for the driver, application master, and executors.

```bash
sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

spark.master yarn
spark.driver.memory 2g
spark.yarn.am.memory 2g
spark.executor.memory 2g
spark.dynamicAllocation.enabled=false  # Disable dynamic allocation
spark.executor.instances=3  # Set a fixed number of executors
spark.executor.cores=4      # Number of cores per executor (or adjust based on your node resources)
spark.task.cpus=1           # Number of CPUs per task (adjust based on your workload)

EOL
```

sudo tee -a /usr/local/spark/conf/spark-defaults.conf > /dev/null << EOL

spark.master yarn
spark.driver.memory 2g
spark.yarn.am.memory 2g
spark.executor.memory 2g
spark.dynamicAllocation.enabled=false
spark.executor.instances=3

EOL

stop-dfs.sh
stop-yarn.sh
start-dfs.sh
start-yarn.sh

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