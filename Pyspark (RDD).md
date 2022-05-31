# Pyspark Code(1)

### Spark) Context 설정

- ```python
  from pyspark import SparkConf, SparkContext
  ```

  - `SparkConf` 는 스파크 콘텍스트를 설정하는 객체 

- ```python
  conf = SparkConf().setMaster("local").setAppName("country-student-count")
  sc = SparkContext(conf = conf)
  ```

  - 클러스터가 아니라 local에서 실행하겠다는 뜻

  - `SparkConf` 객체로 스파크 콘텍스트 객체를 만들고 **sc**로 정의한 코드 

     

### Spark) RDD 생성

- ```pyth
  directory = "/home/tutor/SparkCourse/data"
  file  = "xAPI-Edu-Data.csv"
  lines = sc.textFile(f"file:///{directory}/{file}")
  ```

  - 파일 경로 설정을 한 후 `sc.textFile`로 **RDD**생성
  - 데이터를 담고 있는 file을 `textFile`이 한줄씩 쪼개는데 이 줄 하나하나가 모두 **RDD**의 값이 된다

- ```python
  rows = lines.filter(lambda row : row != header) # 데이터의 첫 줄에 컬럼정보가 담겨있기 때문에 필요없어서 제거
  countries = rows.map(lambda row : row.split(",")[2])
  
  # 첫번째 코드 실행 후 rows가 담고있는 데이터 형태 
  # 'M,KW,KuwaIT,lowerlevel,G-04,A,IT,F,Father,15,16,2,20,Yes,Good,Under-7,M'
  ```

  - `rows = lines.filter(lambda row : row != header)`:
    - `filter()`(**Transformation**)
      - filter method안에 column에 대한 조건을 명시하면 해당 조건을 만족하는 row만 뽑아낼 수 있다.
    - lines자체가 변형이 된 것이 아니기 때문에 변형된 값을 옮겨야 한다.
  - `countries = rows.map(lambda row : row.split(",")[2])` 
    - `map()` (**Transformation**)
      1. 함수를 인자로 가지고
      2. 리스트와 같은 iterable 자료 구조의 모든 요소에 그 함수를 적용시킨  후
      3. 그 결과로 구성된 map객체를 돌려준다.
    - 모든 줄에서 2번째 필드의 나라 데이터를 꺼내 countries 라는 새 RDD를 불러온다)   

- ```python
  countries.collect()[:3]
  ```

  - `collect()`(**Action**)
    - RDD의 데이터를 출력한다 

- ```python
  result = countries.countByValue()
  ```

  - `countByValue()`(**Action**)
    - value에 대한 unique set의 count가 key value로 저장이 됨
  - countries RDD에는 영향을 끼치지 않는다.





