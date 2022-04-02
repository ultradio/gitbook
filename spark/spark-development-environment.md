# Spark Development Environment



Scala  설

```
sudo apt-get install scala

scala

export SCALA_HOME=/usr/bin/scala
export PATH=$SCALA_HOME/bin:$PATH
```

Spark 설

```
wget https://archive.apache.org/dist/spark/spark-2.4.4/spark-2.4.4-bin-hadoop2.7.tgz

tar xvf spark-2.4.5-bin-hadoop2.7.tgz

sudo mv spark-2.4.5-bin-hadoop2.7/ /usr/spark

export SPARK_HOME=/usr/spark
export PATH=$SPARK_HOME/bin:$PATH

spark-shell
```





참고자료

\
\


