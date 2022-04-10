# Structured Streaming

Structured Streaming 은 데이터 스트림을 테이블로 관리한다. 매 미니매치 마다 새로운 데이터 스트림 유입되면 테이블에 추가된다.

## Structured Streaming 으로 스트림 데이터 처리 예시

### 샘플 데이터 스트림

```
{"time":1469501675,"action":"Open"}
{"time":1469501678,"action":"Close"}
{"time":1469501680,"action":"Open"}
{"time":1469501685,"action":"Open"}
{"time":1469501686,"action":"Open"}
{"time":1469501689,"action":"Open"}
{"time":1469501691,"action":"Open"}
{"time":1469501694,"action":"Open"}
{"time":1469501696,"action":"Close"}
{"time":1469501702,"action":"Open"}
{"time":1469501703,"action":"Open"}
{"time":1469501704,"action":"Open"}
```

### 스트림 초기화

샘플 데이터의 스키마를 정의하고 

```python
from pyspark.sql.types import *
from pyspark.sql.functions import *

inputPath = "/databricks-datasets/structured-streaming/events/"

# Define the schema to speed up processing
jsonSchema = StructType([ StructField("time", TimestampType(), True), StructField("action", StringType(), True) ])

streamingInputDF = (
  spark
    .readStream
    .schema(jsonSchema)               # Set the schema of the JSON data
    .option("maxFilesPerTrigger", 1)  # Treat a sequence of files as a stream by picking one file at a time
    .json(inputPath)
)

streamingCountsDF = (
  streamingInputDF
    .groupBy(
      streamingInputDF.action,
      window(streamingInputDF.time, "1 hour"))
    .count()
```



```python
query = (
  streamingCountsDF
    .writeStream
    .format("memory")        # memory = store in-memory table (for testing only)
    .queryName("counts")     # counts = name of the in-memory table
    .outputMode("complete")  # complete = all the counts should be in the table
    .start()
)
```



```sql
%sql select action, date_format(window.end, "MMM-dd HH:mm") as time, count from counts order by time, action
```





### 참고자료

Azure Databricks Doucmentation, https://docs.microsoft.com/ko-kr/azure/databricks/getting-started/spark/streaming
