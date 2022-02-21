---
description: Data Serialization, Memory Tuning, Garbage Collection Tuning
---

# Spark Tuning

### Data Serialization

객체를 직렬화할 때 너무 느리면 애플리케이션 전체 성능이 크게 저하될 수 있다. Spark 와 같은 분산 환경에서 데이터 직렬화는 성능에 많은 영향을 미친다.

Spark는 워커 노드들 사이에 데이터를 셔플링 할 때, 혹은 RDD를 디스크에 저장할 때 직렬화한다. Spark는 직렬화 성능 향상을 위해 Kryo Serializer를 사용할 수 있으며, Java Serializer 사용할 때 보다 10배 이상 성능 향상이 가능하다. Kryo Serializer 사용하려면 Spark 설정 파일에서 Kryo 직렬화 설정하면 된다.

Kyro 사용 설정

{% code title="spark-defaults.conf" %}
```bash
spark.serializer org.apache.spark.serializer.KryoSerializer
```
{% endcode %}



Kryo에 사용자 클래스 등록 Kryo Serializer는 전체 클래스 이름을 저장하기 때문에 자원 낭비가 심하므로 사용자가 원하는 클래스를 등록해서 저장해야 한다.

```scala
val conf = new SparkConf().setMaster(...).setAppName(...)
conf.registerKryoClasses(Array(classOf[MyClass1], classOf[MyClass2]))
val sc = new SparkContext(conf)
```

### Memory Tuning

직렬화를 잘 수행하면 메모리 최적화에도 도움이 된다. Spark Storage Level 설정을 StorageLevel.MEMORY\_ONLY\_SER 로 설정하면 데이터를 직렬화해서 메모리에 저장할 수 있다. RDD 파티션을 바이트 배열 하나로 만들어 저장하기 때문에 메모리 사용량을 줄일 수 있다.

하지만 데이터를 직렬화하면 메모리를 줄일 수는 있으나, 데이터에 접근할 때 역직렬화 해야 하기 때문에 데이터 접근 시간이 더 오래 걸릴 수 있다.

기본 Storage Level은 StorageLevel.MEMORY\_ONLY 를 사용하며,  RDD를 역직렬화하여 JVM에 저장한다.

### Garbage Collection Tuning

Java 는 메모리를 확보하기 위해 주기적으로 오래된 객체를 추적해서 제거하는 GC 작업을 수행하는데, 이 과정에서 GC 오버헤드가 발생한다.

GC 비용을 낮추는 방법에는 크게 두가지가 있다.

첫번째는 GC 비용이 Java 객체 수와 비례하기 때문에객체 수를 줄이면 GC 비용을 낮출 수 있다. 두번째는 객체를 직렬화해서 저장하면 RDD 파티션 마다 바이트 배열 객체 하나로 만들 수 있다.

GC 가 문제가 되는지 디버깅하려면 Spark 애플리케이션을 실행할 때 Java 옵션에 -verbose:gc -XX:+PrintGCDetails -XX:+PrintGCTimeStamp 설정하면 GC가 발생할 때 마다 로그를 출력할 수 있다.

#### **GC 디버깅 옵션**

```bash
spark.driver.extraJavaOptions -Dhdp.version=2.2.0.0–2041 -verbose:gc -XX:+PrintGCDetails -XX:+PrintGCTimeStamps 
spark.executor.extraJavaOptions -Dhdp.version=2.2.0.0–2041 -verbose:gc -XX:+PrintGCDetails -XX:+PrintGCTimeStamps
```



GC 문제를 점검하고 최적화하는 방법은 크게 2가지 방법이 있다.

1. Full GC 가 배치 작업 완료 전에 여러번 일어난다면 작업을 실행할 메모리가 부족하다는 것이다.
2. Heap에 OldGen 이 거의 꽉 찼다면 캐쉬 메모리 크기를 줄여라. 작업이 느려지는 것보다 캐쉬를 더 적게 하는 것이 더 유리하다.

### 참고자료

[http://spark.apache.org/docs/latest/tuning.html](http://spark.apache.org/docs/latest/tuning.html)\
[https://github.com/apache/spark/blob/master/core/src/test/scala/org/apache/spark/SparkConfSuite.scala](https://github.com/apache/spark/blob/master/core/src/test/scala/org/apache/spark/SparkConfSuite.scala)\
[https://ogirardot.wordpress.com/2015/01/09/changing-sparks-default-java-serialization-to-kryo/](https://ogirardot.wordpress.com/2015/01/09/changing-sparks-default-java-serialization-to-kryo/)\
[http://helloworld.naver.com/helloworld/textyle/1329](http://helloworld.naver.com/helloworld/textyle/1329)
