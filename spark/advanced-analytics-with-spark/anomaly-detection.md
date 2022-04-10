# Anomaly Detection in Network Traffic with K-means

### Anomaly Detection

이상 탐지는 지금까지 알려지지 않은 새로운 네트워크 공격이나 서버 장애, 공장 설비 등 새로운 종류의 이상을 탐지하는 것이다. 이상 탐지는 비지도 학습 알고리즘 K-Means를 사용하여 탐지할 수 있다. 정상 데이터를 학습하여 데이터를 여러 개의 클러스터로 클러스터링하고 새로운 데이터가 클러스터에 포함되지 않으면 이상으로 탐지할 수 있다.

### K-Means

K-Means 는 데이터를 K개로 클러스터링하는 ML 모델이다. K 값은 사용자가 정의하는 하이퍼파라미터이다. 클러스터 중심을 centroid라고 하는데 클러스터에 속한 모든 데이터의 산술평균으로 구한다. 그래서 K-Means라고 한다. **K-Means 모델은 가장 좋은 K값을 찾는 것이 핵심**이다.

### Network Anomaly Detection

아직까지 네트워크 공격으로 알려지지는 않았지만, 과거 네트워크 연결과 다른 양상을 찾아 내는 것이 네트워크 이상 탐지이다.

### Anomaly Detection in Network Traffic with K-means <a href="#anomaly-detection-in-network-traffic-with-k-means" id="anomaly-detection-in-network-traffic-with-k-means"></a>

* Spark 설치한다.
* Spark Session 설정한다.
* 데이터 로드한다.
* 데이터 탐색한다.
  * 결측치, 수치형, 범주형 데이터, 레이블 고유값 점검한다.
* 모델링한다.
  * StringIndexer와 OneHotEncoder로 범주형 데이터를 수치형으로 변환한다.
  * VectorAssembler로 feature vector를 생성한다.
  * StandardScaler로 feature를 Standard Score로 바꾸어 정규화한다.
  * K-Means 모델 생성한다.
  * Pipeline으로 연결한다.
* 다양한 K 값으로 모델을 학습한다.
  * K 값이 커질 수록 각 클러스터 데이터가 Centroid와 가까워야 한다.
  * K 값이 큰데 거리가 멀 경우 Local Optimum에 도달하기 전에 학습을 종료했을 수도 있으니 반복 횟수를 늘려서 모델 성능을 개선한다.
  * K 값이 증가해도 평가 점수가 크게 떨어지지 않는 지점(Elbow)이 적당한 K 값 일 수 있다.

#### Anomaly Detection  <a href="#anomaly-detection-in-network-traffic-with-k-means" id="anomaly-detection-in-network-traffic-with-k-means"></a>

**이상 탐지는 새로운 데이터에서 가장 가까운 군집  중심과의 거리를 측정하는 방식으로 진행한다.  거리가 정의한 Threshold 값을 넘어서면 이상한 데이터로 간주한다.**&#x20;

```python
import warnings
warnings.filterwarnings(action='default')

#install Apache Spark
!pip install pyspark --quiet

from pyspark.ml import PipelineModel, Pipeline
from pyspark.ml.clustering import KMeans, KMeansModel
from pyspark.ml.feature import OneHotEncoder, VectorAssembler, StringIndexer, StandardScaler
from pyspark.ml.linalg import Vector, Vectors
from pyspark.sql import DataFrame, SparkSession
from pyspark.sql.functions import col, asc, desc
import random

#Building Spark Session
#spark = SparkSession.builder.appName("kmeans").master("local[*]").getOrCreate()
spark = (SparkSession.builder
                  .appName('Anomaly Detection in Network Traffic with K-means')
                  .config("spark.executor.memory", "1G")
                  .config("spark.executor.cores","4")
                  .getOrCreate())
                  
# Setting Spark Log Level
spark.sparkContext.setLogLevel('ERROR')
spark.version

# Load Data

url = '/kaggle/input/sparkml/kddcup.data_10_percent'

data = spark.read.\
    option("inferSchema", True).\
    option("header", False).\
    csv(url).\
    toDF(
        "duration", "protocol_type", "service", "flag",
        "src_bytes", "dst_bytes", "land", "wrong_fragment", "urgent",
        "hot", "num_failed_logins", "logged_in", "num_compromised",
        "root_shell", "su_attempted", "num_root", "num_file_creations",
        "num_shells", "num_access_files", "num_outbound_cmds",
        "is_host_login", "is_guest_login", "count", "srv_count",
        "serror_rate", "srv_serror_rate", "rerror_rate", "srv_rerror_rate",
        "same_srv_rate", "diff_srv_rate", "srv_diff_host_rate",
        "dst_host_count", "dst_host_srv_count",
        "dst_host_same_srv_rate", "dst_host_diff_srv_rate",
        "dst_host_same_src_port_rate", "dst_host_srv_diff_host_rate",
        "dst_host_serror_rate", "dst_host_srv_serror_rate",
        "dst_host_rerror_rate", "dst_host_srv_rerror_rate",
        "label")

data.cache() #for faster re-use

## Data Exploration
# 23개 레이블이 있고 smurf와 neptune 빈도가 높다. 데이터에 23개 레이블이 있기 때문에 k는 최소 23이라고 추측할 수 있다.

countByLabel = data.select("label").groupBy("label").count().orderBy(col("count").desc())
countByLabel.show(25)
countByLabel.count()

# 데이터는 수치형와 범주형로 이루어져 있다. 범주형 데이터는 수치형으로 엔코딩이 필요하다.
data.show(5)

class RunKMeans:
    def __init__(self, spark: SparkSession):
        self.spark = spark
    
    def oneHotPipeline(self, inputCol: str):
        # 문자열을 0, 1, 2 등의 정수 인덱스로 변환한다.
        indexer = StringIndexer().setInputCol(inputCol).setOutputCol(inputCol+"_indexed")
        # 정수 인덱스를 One-Hot Encoding 한다.
        encoder = OneHotEncoder().setInputCol(inputCol + "_indexed").setOutputCol(inputCol + "_vec")
        # One-Hot Encoding 파이프라인을 생성한다.
        pipeline = Pipeline().setStages([indexer, encoder])

        return pipeline, inputCol + "_vec"

    def fitPipeline(self, data: DataFrame, k: int):
        
        # 범주형 데이터를 One-Hot Encoding을 사용하여 수치형으로 변환한다.
        protoTypeEncoder, protoTypeVecCol = self.oneHotPipeline("protocol_type")
        serviceEncoder, serivceVecCol = self.oneHotPipeline("service")
        flagEncoder, flagVecCol = self.oneHotPipeline("flag")
        
        # VectorAssembler는 feature vector를 생성한다.
        assembleCols = (set(data.columns) - set(["label", "protocol_type", "service", "flag"])).\
            union(set([protoTypeVecCol, serivceVecCol, flagVecCol]))

        assembler = VectorAssembler().\
            setInputCols(list(assembleCols)).\
            setOutputCol("featureVector")

        # feature를 Standard Score로 바꾸어 정규화한다.(데이터를 같은 방향으로 같은 크기만큼 이동한다는 듯)
        scaler = StandardScaler().\
            setInputCol("featureVector").\
            setOutputCol("scaledFeatureVector").\
            setWithStd(True)

        # K-Means 모델을 생성한다.
        kmeans = KMeans().\
            setSeed(random.randint(1, 10000000)).\
            setK(k).\
            setPredictionCol("cluster").\
            setFeaturesCol("scaledFeatureVector").\
            setMaxIter(40).\
            setTol(1.0e-5)
        
        # Pipeline 으로 연결한다.
        pipeline = Pipeline().setStages(
            [protoTypeEncoder, serviceEncoder, flagEncoder, assembler, scaler, kmeans])

        pipelineModel = pipeline.fit(data)
        
        return pipelineModel


    def clusteringSocre(self, data: DataFrame, k: int):
        pipelineModel = self.fitPipeline(data, k)
        kmeansModel = pipelineModel.stages[-1]
        
        return kmeansModel.summary.trainingCost
        
runKMeans = RunKMeans(spark)
# K 값이 커질 수록 각 클러스터 데이터가 Centroid와 가까워야 한다.
# K 값이 큰데 거리가 멀 경우 Local Optimun에 도달하기 전에 학습을 종료했을 수도 있으니 반복 횟수를 늘려서 모델 성능을 개선한다.
# K 값이 증가해도 평가 점수가 크게 떨어지지 않는 지점(Elbow)이 적당한 K 값 일 수 있다.

for k in range(60, 270, 30):
    print(k, runKMeans.clusteringSocre(data, k))
```



### 참고자료

9가지 사례로 익히는 고급 스파크 분석(2판) 도서 (한빛미디어)





