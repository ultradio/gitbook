---
description: Parquet, ORC, Avro
---

# Columnar Database Storage

### Columnar Database Storage

![http://www.hanaexam.com/p/column-vs-row-data-storage.html](<../.gitbook/assets/columnar_database_storage.png>)

![http://www.hanaexam.com/p/column-vs-row-data-storage.html](<../.gitbook/assets/columnar_database_storage2.png>)

### Columnar Database Storage Formats

ORC, Parquet, Avro 는 데이터를 저장하는 방식에 차이가 있다. Parquet와 ORC를 데이터를 column 기반으로 저장하기 때문에 특정 필드에 자주 접근할 때 유용하다. Avro는 Row 기반으로 저장하기 때문에 모든 필드에 골고루 접근할 때 유용하다. **ORC는 Hive에, Parquet는 Spark에 최적화된 포맷**이다.

&#x20;CSV 파일을 예로 Row기반 저장포맷과 Column기반 저장 포맷을 비교해보자.&#x20;

#### Row 기반 저장 포맷

{% code title="people" %}
```
ID,FIRST_NAME,LAST_NAME,AGE,COOL,FAVORITE_FRUIT
1, 몽룡, 이, 18, True, ['바나나', '사과']
2, 춘향, 성, 16, True,
```
{% endcode %}

#### &#x20;Column 기반 저장 포맷&#x20;

Column 기반 저장 포맷은 다음과 같은 형식이다.

```
Field Name/Field Type/Number of Characters:[data]
```

{% code title="people" %}
```
ID/INT/3:1,2
FIRST_NAME/STRING/5:몽룡,춘향
LAST_NAME/STRING/3:이,성
AGE/INT/5:18,16
COOL/BOOL/3:1,1
FAVORITE_FRUIT/ARRAY[STRING]/11:[바나나,사과],[]
```
{% endcode %}

#### &#x20;Column기반 저장 포맷 이점

```
SELECT COUNT(1) from people where last_name = "Rathbone"
```

LAST\_NAME이 "이" 인 데이터를 검색할 때 Row 기반 저장 포맷은 모든 Row를 스캔하여 찾는다. 반면 Column 기반 저장 포맷은 2개 필드는 스킵하고 LAST\_NAME 필드에서 찾기 때문에 검색 성능이 빠르다.

### 참고자료

[https://blog.matthewrathbone.com/2019/11/21/guide-to-columnar-file-formats.html](https://blog.matthewrathbone.com/2019/11/21/guide-to-columnar-file-formats.html)&#x20;

