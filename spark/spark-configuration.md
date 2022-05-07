# Spark Configuration


{% tabs %}
{% tab title="spark-shell, spark-submit" %}
```bash
# Run on a YARN cluster in cluster deploy mode
# --num-executors: Executor 개수
# --executor-cores: Executor별 Core 개수
# --executor-memory: Executor별 메모리

./bin/spark-submit \
  --class org.apache.spark.examples.SparkPi \
  --master yarn \
  --deploy-mode cluster \
  --executor-memory 20G \
  --executor-cores 4 \
  --num-executors 50 \
  /path/to/examples.jar \
  1000
```
{% endtab %}

{% tab title="spark-defaults.conf" %}
```bash
# Default system properties included when running spark-submit.
# This is useful for setting default environmental settings.
spark.executor.instances          50
spark.executor.cores              4
spark.executor.memory             20G
#spark.dynamicAllocation.enabled    true
```
{% endtab %}
{% endtabs %}

## 참고자료

Spark Configuration, [https://spark.apache.org/docs/latest/configuration.html](https://spark.apache.org/docs/latest/configuration.html) \
Spark Submitting Applications, [https://spark.apache.org/docs/latest/submitting-applications.html](https://spark.apache.org/docs/latest/submitting-applications.html)
