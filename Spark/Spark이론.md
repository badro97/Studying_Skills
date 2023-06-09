# Spark 이론

## Spark란?

Spark 는 데이터 엔지니어링, 데이터 사이언스, 머신러닝등의 작업을 실행할 수 있는 multi-langauge 엔진/프레임워크입니다. 단일 노드 뿐만 아니라 cluster 형태로 대량의 컴퓨팅 자원을 사용할 수 있습니다. 대량의 데이터에 대해서 분산된 컴퓨팅 처리를 신뢰성 있게 처리할 수 있습니다.

---  

## Spark의 주요 기능

**Batch/streaming data**

하나의 프로그래밍 모델과 프레임워크로 Batch 처리와 real-time streaming 을 통합 개발 할 수 있다. Python, SQL, Scala, Java , R의 다양한 언어로 개발할 수 있다.

**SQL analytics**

반복된 쿼리, ad-hoc 쿼리 모두 ANSI SQL로 빠르게 분산된 처리결과를 얻을 수 있다. 기존의 다양한 data warehouse 솔루션(MapReduce, Hive, Impala)보다 빠르다. 

**Data science at scale**

petabyte-scale 데이터에 대해 downsampling없이도 Exploratory Data Analysis (EDA) 를 수행할 수 있다.

- hive : 24-48hrs
- Spark : 1-2 hrs

**Machine learning**

ML algorithm 을 학습시킬 수 있고, 로컬에서 작성하고 수행한 코드가 수천대의 클러스터에서도 동일하게 동작한다. 

---  

## Spark 가 풀고자 하는 문제

### MapReduce 의 한계 극복

대량의 데이터에 대한 처리 작업은 MapReduce 라는 모델이 처리하는 것이 효율적이고 강력하다는 것이 Hadoop MapReduce 에서 검증이 되었다.

Hadoop MapReduce 는 이전에는 처리할 수 없던 대량의 데이터를 처리할 수 있게 한 의미가 있다. 하지만, Hadoop MapReduce 는 빈번한 자원의 할당과 해제의 문제, 중간 결과 파일을 외부 스토리지를 사용함으로서 발생하는 지연시간과 부하 문제, 과도한 shuffle 문제 등으로 인해 실제 데이터 처리 작업보다 부가적인 작업에 의해 시간을 너무 많이 소모하고 자원을 낭비하는 문제가 있었다.

Spark 는 분산 데이터 모델인 RDD와 in-memory cache 의 활용으로 Hadoop MapReduce 의 한계를 대부분 해결했고, 그 결과 Hadoop MapReduce 보다 100배 이상의 빠른 성능을 보였다. 

### API로 정형화된 데이터 처리 모델

MapReduce를 토대로 데이터를 처리하는 과정을 정형화될 수 있었다. Spark는 데이터를 변환하는 것은 map function, 데이터를 걸러내는 것은 filter function, 데이터를 재배치하는 것은 group by, 데이터를 집계하는 것은 reduce/aggregate function 이런식이다.

Spark 는 scala 기반의 functional programming 의 type 안정성과 완결성을 토대로 데이터 처리모델을 API로 정형화 시켰다. API를 통해 기존의 데이터 처리 작업을 작성하는 것보다 코드를 작성하는 것이 쉬워질 뿐만 아니라, 이 토대 하에서 데이터 처리작업을 코드수준에서 무결성을 보장해서 전체적인 개발 생산성을 높일 수 있었다.

### Multi Language 로 확장성

데이터를 활용하는 분야에서는 전문 소프트웨어 엔지니어가 아닌 개발자가 많고, 이들은 Java, Scala 등의 정적 타입언어에 대한 어려움이 있었다. 이들은 Pyhon, R 과 같은 언어를 선호했는데, Spark는 정형화된 데이터 처리 모델과 API를 토대로 다른 언어도 Spark API를 이용해서 코드를 작성해도 대용량 분산처리를 가능하게 만들었다.

### 더 쉽고, 더 폭넓은 사용

Spark RDD로 분산 데이터 모델을 빠르고 안정적으로 사용할 수 있게 만들고, Spark의 API로 데이터 처리작업에 대한 정형화를 이루었다. 이를 토대로해서 사용성에서의 추상화를 할 수 있었다. Spark API를 몰라도 ANSI SQL만으로도 대용량 처리작업에 대해서 Spark 로 처리가 가능하게 만들었다. 또한 Spark 의 데이터 처리작업을 몰라도 복잡한 ML 연산을 쉽게 제공하도록 MLib 라이브러리로 대용량 데이터에 대해서 빠른 ML연산이 가능하게 만들었다.

이것에서 영감을 받아 많은 OLAP 처리장치가 내부 실행엔진으로 spark 를 사용해서 처리하고 있다. Hive도 execution engine 으로 spark를 사용할 수 있다.

이러한 작업은 여러 프로젝트에서 계속 진행중이다.

--- 

# Spark 구성요소와 아키텍처

## Spark Architecture

![image](https://github.com/badro97/Project1/assets/49307262/84f85a2f-e8d6-47f0-8e01-81fd6e815c5c)

https://static.javatpoint.com/tutorial/spark/images/spark-architecture.png


<img width="715" alt="Screenshot 2023-05-28 at 4 50 54 PM" src="https://github.com/badro97/Project1/assets/49307262/b85c5f97-b99d-4034-a539-424c8f24629d">  

--- 

### Spark Driver  

Spark Driver 는 Spark 프로그램의 중앙 처리장치이다. Spark Context 를 시작하고, 제출된 어플리케이션의 실행을 담당한다. 어플리케이션의 실행은 코드 내용을 보고 job 의 순서(DAG 형식)로 나눈다. 각 job 은 stage로 나누고, 각 stage 는 작은 task 단위로 나뉜다. Task 가 task scheduler로 전달된다.

Spark Application code 가 submit 되면, driver 는 코드를 분석한뒤, DAG라고 불리는 logical plan 을 작성한다. 여기서 데이터의 흐름은 task 와 transformation 위주로 최적화된다. 이 logical plan 은 실제로 최적화된 일련의 physical plan 이된다. physical code 는 stages 로 나뉘고, 각 stage 는 클러스터에 제출될 tasks 들로 구성된다. 

Spark Driver 는 cluster manager 와 상호작용해서 task를 수행할 리소스를 얻는다. submit 된 코드와 physical execution model 은 클러스터 매니저가 알려준 데이터의 위치를 기반으로  할당 된다. 또한 executor의 future task도 스케줄 시킬 수 있다. 마지막으로 최종 output 을 수집하고 그 결과를 Spark submit을 수행한 Client에게 전달한다.

---  

### Cluster Manager

Spark driver 는 실제 task를 실행할 executor(또는 worker node)의 가용한 자원을 Cluster Manager 에 요청 한다. ClusterManager 는 Spark Driver 가 요청한 리소스를 할당하고, driver 가 요청한 프로그램을 worker node 에서 실행할 수 있는 instrcution을 제공한다. ClusterManager는 executor 의 자원사용, 성능을 트래킹하고, job 이 끝나면 driver 에게 작업의 종료를 알린다. 작업도중 worker node 가 실패하거나 삭제되면, Cluster manager 는 같은 작업을 다른 노드에 할당해서 작업이 이어지도록 한다.

### Executor

Executor 는 실제로 데이터에 접근할 수 있고, 할당된 task를 수행할수 있는 worker 이다. Spark job 을 submit 할때 executor 의 수를 지정할 수 있고, Cluster Manager의 허용치 안에서 dynamic 하게 allocation할 수도 있다. 각 executor 마다 몇개의 (virtual) core를 사용할지, 얼마의 RAM을 사용할지 지정할 수 있다.. executor 는 수행할 복수개의 task 를 할당받아 수행하고, 수행 결과는 driver 에게 전달한다.  

## Spark on Yarn Architecture

아래 그림은 master=yarn 으로 했을때, Yarn 아키텍처 하에서 Spark 의 각 component 의 아키텍처가 어떻게 되는지 나타낸 그림이다. 

<img width="683" alt="Screenshot 2023-05-28 at 4 54 55 PM" src="https://github.com/badro97/Project1/assets/49307262/44adacdc-9ca3-4041-897d-2fa5df3199a5">  

## Spark Software Architecture

![image](https://github.com/badro97/Project1/assets/49307262/abb5c90c-b753-4cf5-a54a-e4bb6d4fce10)  
https://dwgeek.com/wp-content/uploads/2018/07/Apache-Spark-Architecture.jpg

---  

## Spark 사용시 주의할 점

위에서 설명한 Architecture 의 구조적인 제약에 따라 Spark 사용시 주의할 사항이 몇가지 있다.

### Driver 가 SPOF(Single Point Of Failure, 단일 실패점) 이다.

(Client-mode의 경우)

Spark Driver는 하나의 JVM이고, executor 는 cluster manager와 driver에 의해서 fault-tolerance 가 보장되지만 Driver 는 그렇지 않다. 그렇다고 Driver 가 하는 일이 적은 것도 아니다, 각 executor 가 수행하는 task의 결과가 모두 Driver로 모이므로 task의 결과 값 데이터가 큰 경우 Driver 가 오버헤드 (OOM(Out of Memory)이 가장 많음)로 죽는 경우가 발생할 수 있다. 이때 Driver를 자동으로 살려주는 방법은 없다. Driver 프로세스를 모니터링하고 Driver 에 문제가 생길경우 대처할 방법이 필요하다.

(cluster-mode의 경우)

cluster-mode 의 경우 Spark Driver 가 뜬 container 가 죽으면 (기본적으로) Driver 가 다시 시작한다. 이때 그 과정에서 executor들이 전달한 데이터가 유지된다는 보장이 없다. 이때도 역시 Driver 가 SPOF인 것은 변함이 없고 재시작에 대한 고려를 해야한다.

### Executor의 수와 리소스

executor의 수를 늘리수록 parallelism 이 증가한다. executor의 수가 증가하면 in-memory 에 의한 병렬연산 속도가 빨라지는 것을 기대할 수 있다. 다만 데이터의 양 대비 많은 executor 를 이용하면 불필요하게 network를 많이 사용하게 될 수 있다. (element가 10개인 배열을 10개의 executor 에서 수행할 필요가 있을까?) 또한 executor의 수가 많아지면 shuffle에서 이동하는 데이터가 많아지므로 오히려 전체 수행시간이 느려지고 시스템부하는 높아질 수도 있다.  

### Idempotent(멱등성 → 언제 실행하더라도 같은 결과물을 만들어야된다.)

Executor 는 Cluster Manager 와 Driver 에 의해서 언제든 재시작될 수 있다 . Driver 도 SPOF이기 때문에 fast-fail 전략을 쓴다면 프로그램을 처음부터 다시 시작하는 경우가 꽤 발생한다. 따라서 Spark Job 은 언제든 재처리할 수 있다는 것을 가정하고 작성하는 것이 좋다. 하나의 작업을 idempotent 하게 작성해야 실제 데이터가 오염되거나 잘못처리되는 일이 없다.

Batch로 Spark를 사용하는 것과 달리 context가 필요한 streaming 작업의 경우는 context 가 필요한 경우가 많기 때문에, 시스템 설계에서부터 at least once, at most once, exactly once 를 고려해야한다. checkpoint 기능을 잘 활용해서 context를 잘 유지하는 방법을 쓰는 것이 좋다. checkpoint를 쓰더라도 streaming application 역시 최대한 idempotent 하게 만드는 게 좋다.