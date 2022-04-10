# Structured Streaming

Structured Streaming 은 데이터 스트림을 테이블로 관리한다. 매 미니매치 마다 새로운 데이터 스트림 유입되면 테이블에 추가된다.

### Basic Concepts <a href="#basic-concepts" id="basic-concepts"></a>

![출처: https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html](<../.gitbook/assets/image (45).png>)

![출처: https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html](<../.gitbook/assets/image (42) (1).png>)



### **Types of time windows**

![출처: https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html](<../.gitbook/assets/image (46) (1).png>)



### **Handling Late Data and Watermarking**

![출처: https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html](<../.gitbook/assets/image (43).png>)



```scala
import spark.implicits._

val words = ... // streaming DataFrame of schema { timestamp: Timestamp, word: String }

// Group the data by window and word and compute the count of each group
val windowedCounts = words
    .withWatermark("timestamp", "10 minutes")
    .groupBy(
        window($"timestamp", "10 minutes", "5 minutes"),
        $"word")
    .count()
```

###

### 참고자료

Spark Documentation, [https://docs.microsoft.com/ko-kr/azure/databricks/getting-started/spark/streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html)
