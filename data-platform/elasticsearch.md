# #01 Data Platform

분석할 브랜드와 제품을 선정하고 데이터 수집하여 검색엔진에 저장한다. 저장한 데이터를 탐색적 분석을 수행하여 불용어 사전, 키워드 사전과 및 분석방법론을 정의한다. 분석방법론에 맞는 분석함수를 만들어 데이터 분석을 수행한다. 최종적으로 분석 결과를 마케팅 전략 수립에 활용한다.

![](<../.gitbook/assets/image (46).png>)

### Data Collection

특정 브랜드로 출시한된 제품에 반응을 분석하기 위해 제품리뷰 사이트와 같은 소셜미디어에서 데이터를 수집한다.

데이터는 Chrome extension 기반 web scraper와 웹 스크래핑 라이브러리 scrapy를 사용하여 수집하고 Elasticsearch에 저장한다.

**모듈 및 라이브러리**

scrapy 라이브러리\
웹사이트 스크래핑으로 데이터를 추출하고 저장하는 라이브러리이다.

selenium\
webdriver 모듈을 사용하여, 동적 웹페이지와 로그인이 필요한 웹페이지를 스크래핑하는 라이브러리이다.

web scraper\
chrome extension 기반 웹페이지 스크래핑 도구이다.

![](<../.gitbook/assets/image (13).png>)

### Data Analytics

수집한 데이터를 분석하는 과정이다. 탐색적 데이터 분석 과정에서 만든 키워드 사전와 정의한 분석 방법론을 사용하여 데이터를 분석한다.&#x20;

### 감정분류**기** 모델

키워드 사전 쿼리로 분류한 문장을 감정분류기를 사용하여 긍정/부정/중립으로 문장을 분류할 수 있다.

![](<../.gitbook/assets/image (20).png>)

**모듈 및 라이브러**

nltk\
문장 분리에 사용한다.

scikit-learn\
머신러닝 라이브러리로 데이터 전처리에 사용한다.

tensorflow\
감성 분석에 사용할 딥러링 라이브러리이다.

Sentence Extractor\
키워드 사전에 정의한 키워드가 포함된 분석 대상 문장을 추출한다.

TextCNN\
감성분석 딥러닝 알고리즘이다.\


### 문장분류기

키워드 분석 쿼리와 Elasticsearch Percolator 기능을 사용하여 문장의 카테고리와 감정을 분류한다.

![](<../.gitbook/assets/image (22).png>)

**모듈 및 라이브러리**

Elasticsearch Percolator\
Elasticsearch에 키워드 분석 쿼리를 저장하고 저장할 데이터를 리버스 서치하여 문장을 분류하고 저장한다.

Text Translator\
텍스트를 번역한다.

Sentence Extractor\
키워드 사전에 정의한 키워드를 포함한 분석 대상 문장을 추출한다.

Sentiment Classifier\
감성분류 모델을 사용하여 긍정, 부정, 중립을 분류한다.

### Data Storage

메타데이터, 원본 데이터, 분석 데이터를 Elasticsearch에 저장한다.

![](<../.gitbook/assets/image (17).png>)

**모듈 및 라이브러리**

pandas\
데이터를 다루는 라이브러리로 데이터 전처리 및 탐색적 데이터 분석에 사용하는 라이브러리이다.

elasticsearch\
ES에 데이터를 저장할 때 사용한다.

pandasticsearch\
ES에서 데이터를 읽어올 때 사용한다.

### Data Visualization

Kibana를 사용해 데이터 검색하고 분석할 수 있으며 대쉬보드를 구성하여 실시간으로 현황을 파악할 수 있다. 대쉬보드는 설정만으로 브랜드 및 제품에 따라 구성이 가능하다.

![](<../.gitbook/assets/image (18).png>)







