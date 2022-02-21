---
description: RDD, Lazy Execution, Distributed Data Processing, Transformation
---

# Spark Core

### RDD (Resilient Distributed Dataset)

RDD 는 분산환경에서 워커노드에 파티션되어 있는 분산객체이다. 변경 불가능하고, 연산 순서를 기억하고 있어서 파티션이 깨지면 다시 복구할 수 있는 장애 복구 기능을 가지고 있다.

RDD 는 내고장성(fault-tolerant) 데이터셋으로 메모리에 중간산출물을 저장할 수 있고, RDD 에서 제공하는 API 로 데이터를 조작할 수 있다. 보통 Low-level transformation API, Action API 사용해야 하거나, 비정형 데이터(unstructured data)를 처리할 때 RDD를 사용한다. 정형 데이터(structured data)를 처리할 때는 Dataset을 사용한다.

![](https://blog.kakaocdn.net/dn/bSzuwX/btq8dtHha8L/uISrAgCpGihBQ2l0WpZZp1/img.png)

### Lazy Execution

Lazy Execution은 RDD 가 어떻게 만들어지는지 Lineage 정보를 먼저 만들고 Job 을 실행하기 때문에 자원을 고려해서 최적의 Job을 실행할 수 있다.

Driver 코드를 작성하고 Spark 애플리케이션을 실행하면, Driver 는 코드를 분석하여 Transformation 코드로 Job 실행계획(Execution Plan, Lineage 정보)을 세운 후, Action 코드를 수행하여 Job 을 실행한다.

![](https://blog.kakaocdn.net/dn/YIU99/btq79k5YWlH/BuzF4aGKzdJeLJuuoCMQg0/img.png)

### Distributed Data Processing

Spark에서 파일시스템으로 HDFS와 같은 분산파일시스템을 사용하면, 데이터가 여러 파티션으로 쪼개져서 분산되어 저장되기 때문에 파티션 마다 Transformation 작업을 수행한다. 작업은 Data Locality를 고려해 파티션 별로 수행되며, Shuffle이 필요한 경우에 데이터가 다른 머신으로 전송된다.

![](https://blog.kakaocdn.net/dn/bko5QZ/btq8a7KNMKj/JhfykqMxCRQWURQcBjl1g0/img.png)

### Narrow vs Wide transformation

RDD 는 Transformation API(map, filter, join, etc)를 사용하여 분산되어 있는 대용량 데이터를 조작할 수 있다.

Narrow transformation API 는 셔플링하지 않고 작업 노드에 있는 데이터만 가지고 작업하기 때문에 빠르며,파티션이 깨져도 작업노드에서 복원이 가능하다.

Wide transformation 은 셔플링이 필요하며, 데이터가 깨졌을 때 Network IO 비용이 발생한다. Wide transformation API를 사용할 경우, 체크포인트 해주는 것이 좋을 수 있다.

성능을 최적화하기 위해서는 Wide transformation API 는 주의 깊게 사용해야 하며 Narrow transformation API를 최대한 활용해야 한다.

Narrow, Wide transformation 과 action API(Operation)에 대해 알아보자.

![](https://blog.kakaocdn.net/dn/wyBSU/btq8cF2JOSI/4J9ktx3QYqIIx9Zx8DRE10/img.png)

### 참고자료

[http://blog.cloudera.com/blog/2015/03/how-to-tune-your-apache-spark-jobs-part-1/](http://blog.cloudera.com/blog/2015/03/how-to-tune-your-apache-spark-jobs-part-1/)

