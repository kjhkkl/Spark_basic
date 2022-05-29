

# Batch Processing 과 Stream Processing

### Batch Processing (일괄 처리)

- What?
  -  `많은 양의 데이터`를 `정해진 시간`에 `한꺼번에 처리`하는 것!

- When?
  1. 실시간성을 보장하지 않아도 될 때
  2. 데이터를 한꺼번에 처리할 수 있을 때
  3. 무거운 처리를 할 때
- Example
  - 매일 아침 웹 스크래핑/크롤링
  - 매주 사이트에서 관심을 보인 유저들에게 마케팅 이메일 전송
  - 월 별 1일마다 수요와 공급을 예측

### Stream Processing

- What?
  -  `실시간으로 쏟아지는 데이터`를 `계속` 처리하는 것
- When?
  - 실시간성을 보장 해야 될 때
  - 데이터가 여러 소스로부터 들어올 때
  - 데이터가 가끔 들어오거나 지속적으로 들어올 때
  - 가벼운 처리를 할 때
- Example
  - 사기 거래 탐지
  - 이상 탐지
  - 실시간 알림
  - 비즈니스 모니터링
  - 실시간 기능이 들어가는 어플

### Batch VS Stream

![Batch vs Stream](https://k21academy.com/wp-content/uploads/2020/11/BPSP_Diagram_2.png)



- 배치 처리 플로우
  1. 데이터를 모아서
  2. 데이터베이스에서 읽어서 처리 한 다음
  3. 다시 데이터베이스에 담는다.
- 스트림 처리 플로우
  1. 데이터가 들어로 때마다 ( Ingest )
  2. 쿼리 / 처리 후 State를 업데이트 한 다음
  3. DB에 담는다.

---

# Spark?

### Spark의 등장 배경

- 과거와 달리 빅데이터 처리를 실시간으로 필요한 서비스가 증가 

- DISK I/O 기반인 하둡으로는 효과적인 비즈니스를 하기 힘들었다(느림)
- ![](https://phoenixnap.com/kb/wp-content/uploads/2021/04/hadoop-spark-data-processing.png)
- **Hadoop**의 분산처리 단계
  1. 클러스터에서 데이터 읽기
  2. 동작 실행
  3. 클러스터에 결과 기록
  4. 데이터 업데이트 된 내용 읽기
  5. 다음 동작 실행
  6. 클러스터에 결과 기록
- **Spark**의 분산처리 단계
  1. 클러스터에서 데이터 읽기
  2. 애널리틱스 운영 수행 및 결과값 클러스터 입력 동작과 같은 전 과정이 동시 진행

- 단점을 보완하고자 In-Memory 기반의 빠른 처리가 장점인 Spark가 등장하게 됨

---

### Spark의 기본 동작 원리 및 아키텍쳐

- **Spark Cluster**의 구조는 크게 **Master node**와 **Worker node**로 구성된다.
- **Master node**는 전체 클러스터를 관리하고 분석 프로그램을 수행하는 역할을 한다.
  - 사용자가 만든 분석 프로그램을 **Driver Program**이라고 한다.
- 이 분석 프로그램을 **Spark Cluster**에 실행하게 되면 하나의 **JOB**이 생성된다.

![](https://blog.kakaocdn.net/dn/b4IwAS/btrjkiCyCnc/2JQtCkT7XopNoK0xkO9qJ1/img.png)

- 이렇게 생성된 **JOB**이 외부 저장소(HDFS 같은 파일 시스템이나 외부 데이터 베이스)로 부터 데이터를 로딩하는 경우
- 이 데이터는 **Spark Cluster**의 **Worker node**로 로딩이 된다
- 로딩된 데이터는 여러 서버의 메모리에 분산되어 로딩이 된다.
- 이렇게 스파크 메모리에 저장된 데이터 객체를 RDD라고 한다.

![](https://blog.kakaocdn.net/dn/lEDkS/btrjjNpiLda/cLKtcKE75vbcVMEkZPLFI0/img.png)

- 이렇게 로딩된 데이터는 어플리케이션 로직에 의해서 처리된다.
- 하나의 **JOB**이 여러 **Worker node**에 분산된 데이터를 이용해서 분산되어 실행된다.
- 하나의 **JOB**은 여러개의 **Task**로 분리되어 실행이 된다.
- 이렇게 나눠진 **Task**를 실행하는 것을 **Executor**라고 한다.

---

### Spark 의 Components정리

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile27.uf.tistory.com%2Fimage%2F251A9E48575E7117041AAC)

- **Driver Program**: main 함수를 실행시키고 그 안에서` SparkContext`를 생성하는 메인 프로세스

- **SparkContext**: Driver Program에서 Job을 Executor에 실행하기 위한 Endpoint
- **Cluster Manager**: Application 자원을 할당, 제거하는 등 Cluster 자원을 관리하는 서비스
- **Worker Node**: 클러스터에 있는 executor를 포함한 실제 작업을 하는 노드
- **Executor**: Application에서 Driver Program이 요청한 Task들의 연산을 실제로 수행하는 프로세스
- **Job**: Application에서 Spark에 요청하는 일련의 작업. 여러 개의 Task로 나뉘어 실행됨
- **Task**: Spark Executor에서 수행되는 최소 작업 단위

---

# Spark RDD

### Spark 의 데이터 구조

- **Spark**의 데이터 구조는 아래와 같이 크게 3가지 종류로 분류할 수 있다
  - RDD (Resillient Distributed Data)
  - Dataframe
  - Dataset
  - 그 중, RDD는 Spark에 가장 처음으로 도입된 데이터 구조 이다

---

### RDD 개념

- Resilient Distributed Dataset(RDD)는 아래와 같은 의미를 가진다.
   	1. **Resillient(회복력 있는, 변하지 않는)**
       - 메모리 내부의 데이터가 손실되었을 때, 유실된 파티션을 재연산해 데이터를 복구할 수 있다.
   	2. **Distributed(분산된)**
       - Spark Cluster를 통해 메모리에 분산되어 저장된다.
   	3. **Data**
- 즉, RDD는 **여러 분산 노드에 걸쳐 저장되는, 변경이 불가능한 데이터의 집합**이다.
- 따라서, RDD를 변경하기 위해선 새로운 RDD를 생성하는 방법뿐이며
- RDD는 2가지 **Operation**을 사용해 조작할 수 있다.

---

### Operation

- RDD 는 2가지 Operation (Transformation, Action) 을 지원합니다.
  - **Transformation**
    - 기존의 RDD 를 변경하여 새로운 RDD 를 생성하는 것입니다.
    - **즉, 리턴값이 RDD 입니다.**
    - ex) map(), filter()...
  - **Action**
    - RDD 값을 기반으로 무엇인가를 계산해서, 결과를 생성하는 것입니다.
    - **즉, 리턴값이 데이터 또는 실행 결과입니다.**
    - ex) collect(), count()...
- 이러한, RDD 동작 원리의 핵심은 **Lazy Evaluation (느긋한 연산)** 입니다.
- **즉, RDD 는 Action 연산자를 만나기 전까지는, Transformation 연산자가 아무리 쌓여도 처리하지 않습니다.**

![](https://mblogthumb-phinf.pstatic.net/20160809_26/tajogood_1470717511901n4rMY_PNG/spark.PNG?type=w800)

---

### DAG (Directed Acyclic Graph)

- 위처럼, RDD 를 변경하는 순서를 Lineage 라고 하며

- Lineage 는 DAG(Directed Acyclic Graph)의 형태를 가집니다.

- DAG 의 형태는 아래와 같습니다.

  

  ![img](https://blog.kakaocdn.net/dn/zaYOy/btrazNjsx1w/vBjz6FZdOTMQMouPK9hS3k/img.png)

  

  - 노드간의 순환이 없으며, 일정한 방향성을 가지기 떄문에
  - 노드 간의 순서가 중요한 형태입니다.

- 따라서, RDD 연산 과정에서 특정 RDD 관련 정보가 메모리에서 유실됐을 경우,

- DAG 그래프를 복기하여 다시 계산하고, 복구할 수 있습니다.

- Spark 는 이러한 특성 때문에 **Fault-tolerant(장애허용)** 를 잘 보장합니다.





















