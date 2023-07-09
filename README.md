# spark-k8s-jupyterhub
Spark in k8s on jupyterhub materials

Blog post https://dev.to/akoshel/spark-on-k8s-in-jupyterhub-1da2

```
spark-submit \
  --master k8s://https://192.168.49.2:8443 \
  --deploy-mode cluster \
  --driver-memory 1g \
  --conf spark.kubernetes.memoryOverheadFactor=0.5 \
  --name sparkpi-test1 \
  --class org.apache.spark.examples.SparkPi \
  --conf spark.kubernetes.container.image=ducdn01/spark:latest \
  --conf spark.kubernetes.driver.pod.name=spark-test1-pi \
  --conf spark.kubernetes.namespace=spark \
  --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark \
  --verbose \
  local:///opt/spark/examples/jars/spark-examples_2.12-3.2.2.jar 1000
```
---
```
spark = (
    SparkSession.builder.appName("hello").master("k8s://https://192.168.49.2:8443") # Your master address name
    .config("spark.kubernetes.container.image", "ducdn01/pyspark:latest") # Spark image name
    .config("spark.driver.port", "2222") # Needs to match svc
    .config("spark.driver.blockManager.port", "7777")
    .config("spark.driver.host", "driver-service.jupyterhub.svc.cluster.local") # Needs to match svc
    .config("spark.driver.bindAddress", "0.0.0.0")
    .config("spark.kubernetes.namespace", "spark")
    .config("spark.kubernetes.authenticate.driver.serviceAccountName", "spark")
    .config("spark.kubernetes.authenticate.serviceAccountName", "spark")

    .config("spark.executor.instances", "2")
    .config("spark.kubernetes.container.image.pullPolicy", "IfNotPresent")
    .getOrCreate()
)
```
---
### Actice anaconda eviroment
- Create enviroment 
```
conda create --name <env_name>
```
- Activate env
```
source activate <env_name>
```
- Install ipykernel
```
conda install ipykernel
```
- References: https://towardsdatascience.com/get-your-conda-environment-to-show-in-jupyter-notebooks-the-easy-way-17010b76e874
---
#### Uninstall kernel not uninstall env anaconda
```
jupyter kernelspec uninstall new-env
```