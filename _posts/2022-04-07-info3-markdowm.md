---
layout: post
title: 3. 데이터 화면 입출력 구현
subtitle: ★★★ 
categories: markdown
tags: [정보처리기사_실기]
---

# 3. 데이터 입출력 구현
## 3-1. 논리 데이터 저장소 확인
### * 데이터 모델(Data Model) : 현실 세계의 정보를 인간과 컴퓨터가 이해할 수 있도록 추상화하여 표현한 모델
- 데이터 모델 *절차* : (개논물)
    1. 개념적 데이터 모델 : 현실세계에 대한 인식을 추상적&개념적으로 표현하여 개념적 구조를 도출하는 데이터 모델
        - 트랜잭션 모델링, View 통합방법 및 Attribute 합성 고려
        - DB 종류와 관계 X 
        > [산출물] 개체관계 다이어그램 - 도출된 엔티티간의 관계를 이해하기 쉽게 도식화한 다이어그램
    2. 논리적 데이터 모델 : 업무의 모습을 모델링 표기법으로 형상화해 사람이 이해하기 쉽게 표현한 데이터 모델 
        - 스키마 설계, 트랜잭션 인터페이스 설계
        - 정규화를 수행
    3. 물리적 데이터 모델 : 논리 데이터 모델을 특정 DBMS의 특성 및 성능을 고려해 물리적인 스키마를 만드는 일련의 데이터 모델 
        - 테이블, 인덱스, 뷰, 파티션 등 객체를 생성
        - 반정규화를 수행 
        - 레코드 집중의 분석 및 설계
        - 저장 레코드 양식 설계

### * 논리 데이터 모델 검증
- 논리 데이터 모델링 *종류*
    |종류|설명| 구성요소
    |-----|---|---|
    | **관계** 데이터 모델 | - 논리적 구조가 **2차원 테이블 형태**로 구성된 모델 <br> - 기본키(PK)와 이를 참조하는 외래키(FK)로 관계 표현 <br> - 1:1 / 1:N / N:M 관계를 자유롭게 표현 [사진첨부] <br> - E.F.Codd 박사가 제안한 모델| - **릴레이션** : 행(Row), 열(Column)로 구성된 **테이블** <br> - **튜플**(Tuple) : 행(Row) <br> - **속성**(Attribute) : 열(Column) <br> - **카디널리티**(Cardinality) : 튜플의 수 <br> - **차수**(Degree) : 속성의 수 <br> - **스키마**(Schema) : **DB의 구조, 제약조건 등의 정보를 담고 있는** 기본적인 구조 <br> - **인스턴스**(Instance) : 정의된 스키마에 따라 생성된 테이블에 **실제 저장된 데이터의 집합** |
    | 계층 데이터 모델 | - 논리적 구조가 **트리 형태**로 구성된 모델 <br> - 상하관계 有 (부모-자식) <br> - 1:N 관계만 허용 [사진첨부] |
    | 네트워크 데이터 모델 | - 논리적 구조가 **그래프 형태**로 구성된 모델 <br> - CODASYL DBTG 모델이라고 불림 <br> - 상위, 하위 레코드 사이에 N:M 관계를 만족하는 구조 [사진첨부] |

- 논리 데이터 모델링 *속성* : 개체 / 속성 / 관계
    |속성|설명| 피터 챈 모델 (Peter Chen Model) | 까마귀발 모델(Crow's Foot Model) |
    |-----|---|---|---|
    |개체 (Entity)| 실체 / 사물 or 사건| 사각형 (▭) | '표' 형식 |
    |속성 (Attributes)| 개체가 가지고 있는 요소 or 성질 <br> 속성명은 단수형으로 하되 개체명을 사용하지 X , 속성이 필수사항(NOT NULL)인지 아닌지(NULL)를 고려해 작성 | 타원형 (〇) | 표 '내부'에 표시 |
    |관계 (Relationship)| 두 개체 간의 관계를 정의 | 마름모 (◇) | - 1:1 → ———— <br> - 1:N → ———∈ <br> - M:N → ∋——∈ |
    1. 개체 (Entity) : 실체 
        - 사물 or 사건
        - 피터 챈 모델 (Peter Chen Model) → 개체 (ㅁ) 
        - 까마귀발 모델(Crow's Foot Model) → 개체 (표 형식)
    2. 속성 (Attributes) : 구체적 항목
    3. 관계 (Relationship) : 개체 간의 대응 관계

- 관계 *대수* : 관계형 데이터베에서 원하는 정보와 그 정보를 어떻게 유도하는지에 대해 기술하는 **절차적 정형 언어**
    - 관계 대수 연산자 → 일반 집합 연산자, 순수 관계 연산자로 분류
    [사진첨부]
- 관계 *해석* : 튜플 관계 혜석과 도메인 관계 해석을 하는 **비절차적 언어** / 프레티킷 해석에 기반한 언어\

### * 정규화 (Normalization) : 관계형 데이터 모델에서 데이터의 중복성을 제거해 이상현상을 방지하고 데이터의 일관성, 정확성을 유지하기 위해 무손실 분해하는 과정
- 이상현상 (Anomaly) : 데이터의 중복성으로 릴레이션을 조작할 때 발생하는 비합리적현상
    > [종류] 삽입, 삭제, 갱신 이상 <br> 1. 삽입 이상 : 정보를 저장할 때 해당 정보의 불필요한 세부정보를 입력해야 하는 경우 <br> 2. 삭제 이상 : 정보 삭제할 경우 원치않는 정보가 같이 삭제되는 경우 <br> 3. 갱신 이상 : 중복 데이터 중 특정 부분만 수정되어 중복된 값이 모순을 일으키는 경우
- 정규화 *단계* 
    ```
    ① 1정규형(1NF) | 원자값으로 구성 (테이블내 속성값은 원자값이어야함) | ex) 고객 ID = 1 , 이메일 = aaa@aaa , bbb@bbb (총2개) 이므로 원자값이 X → 따라서 속성 1개만 가지도록하면 1차 정규화 O 
    ② 2정규형(2NF) | 부분 함수 종속 제거(완전 함수적 종속 관계)
    ③ 3정규형(3NF) | 이행 함수 종속 제거 | A → B이고, B → C 이면서 A → C인 관계가 같이 있는 경우를 이행함수 종속관계라함 ex) <책번호> → <출판사> / <출판사> → <홈페이지> 에 영향을 줌 
    ④ 보이스-코드 정규형 (BCNF) | 결정자 후보키가 아닌 함수 종속 제거 
    ⑤ 4정규형(4NF) | 다치(다중 값) 종속 제거
    ⑥ 5정규형(5NF) | 조인 종속 제거

    ```
### * 반정규화 (De-Normalization) : 정규화된 엔티티, 속성, 관계에 대해 성능 향상과 개발 운영의 단순화를 위해 중복 & 통합 & 분리등을 수행하는 데이터 모델링의 기법 
- 반정규화 *특징*
    - 장점 : 성능 향상과 관리의 효율성 ↑
    - 단점 : 데이터의 일관성 및 정합성(논리적 모순이 없는 것) ↓ / 유지비 발생 / 성능에 나쁜 영향을 미침
- 반정규화 *기법*
    | 구분 | 수행방법 | 설명 |
    |-----|---|---|
    | 테이블 | 병합 | - 1:1, 1:M 관계를 통합해 조인 횟수를 줄여 성능 ↑ <br> - 슈퍼타입(공통된 데이터로 구성), 서브타입 테이블(나머지 테이터로 구성) 통합을 통해 성능 ↑ |
    ||분할| - 테이블을 수직 or 수평으로 분할하는 것 = **파티셔닝** <br> 1. 수평 분할 : 테이블 분할에 레코드를 기준으로 활용 <br> 2. 수직 분할 : 하나의 테이블이 가지는 컬럼의 갯수가 증가하는 경우 활용 / 갱신 위주의 속성 분할, 자주 조회되는 속성 분할, 크기가 큰 속성 분할, 보안을 적용해야는 속성 분할 |
    || 중복 | 대량의 데이터들에 대한 집계함수(GROUP BY, SUM 등)를 사용해 실시간 통계정보를 계산하는 경우에 효과적인 수행을 위해 별토의 통계 테이블을 두거나 중복 테이블을 추가 <br> 1. 집계 테이블 추가 : 집계 테이블을 위한 테이블 생성 → 각 원본 테이블에 트리거를 설정해 사용 (※ 트리거의 오버헤드 유의<br> 2. 진행 테이블 추가 : 이력관리 등의 목적으로 추가하는 테이블 / 기본키를 적절히 설정 <br> 3. 특정부분만을 포함하는 데이터 추가: 데이터가 많은 테이블의 특정부분만을 사용하는 경우 해당 부분만으로 새로우 테이블 생성 |
    | 컬럼 | 컬럼 중복화 | 조인 성능 향상을 위해 중복 허용|
    | 관계 | 중복관계 추가 | 조인으로 인해 발생하수 있는 성능 저하 예방을 위해 추가적 관계를 맺는 방법 |
## 3-2. 물리 데이터 저장소 설계
### * 물리 데이터 모델 설계 
- 물리데이터 모델링은 논리 모델을 적용하고자 하는 기술에 맞도록 **상세화해 가는 과정** (찐 DB 테이블 작성하는 과정)
- 물리데이터 모델링 *절차*
    ```
    ① 개체를 테이블로 변환
    ② 속성을 컬럼으로 변환 
    ③ UID를 기본키로 변환 : Not Null, Unique등 제약조건을 추가로 정의 
    ④ 관계를 외래키로 변환
    ⑤ 컬럼유형과 길이 정의
        - CHAR : 2000byte, 고정길이 문자열
        - VARCHAR2 : 4000byte , 가변길이 문자열
        - NUMBER : 38자릿수의 숫자
        - DATA : 날짜
        - BLOB, CLOB : 바이너리(Binary), 텍스트 데이터 최대 4GB
    ⑥ 반정규화 수행 : 중복테이블 추가, 테이블 조합 & 분할 & 제거 , 컬럼 중복화
    ```

### * 물리 데이터 저장소 구성 
- 인덱스 (Index) : 검색연산의 최적화를 위해 DB내 열에 대한 정보를 구성한 데이터 구조
- View 속성 : REPLACE, FORCE, NOFORCE 등
    > [View] 사용자에게 접근이 허용된 자료만 제한적으로 보여주기 위해 만들어진 가상 테이블

    | 속성 | 설명 |
    |----|---|
    | REPLACE | 뷰가 이미 존재하는 경우 재생성 |
    | FORCE | 기존 테이블의 존재 여부에 관계없이 뷰 생성|
    | NOFORCE | 기존 테이블이 존재할 때 뷰 생성 |
    | WITH CEHCK OPTION | 서브쿼리 내의 조건을 만족하는 행만 변경 |
    | WITH READ ONLY | 데이터조작어(DDM ; SELECT, INSERT, DELETE, UPDATE) 작업 불가 |
- 클러스터(Cluster)
    > [Cluster] 여러개의 객체를 하나로 모으는것, 대상이되는 범위의 요소를 모은 집합 <br> 컴퓨터 클러스터라면 여러대의 컴퓨터들이 연결되어 하나의 시스템처럼 동작하는 컴퓨터의 집합 / DB에서 클러스터는 마찬가치로 여러개의 서버가 연결되어 하나의 시스템처럼 동작하는 서버들의 집합

- 파티션(Partition) 종류 : 레인지, 해시, 리스트, 컴포지트 파티셔닝 등 
    > [Partition]

    1. 레인지 파티셔닝 (Range Partitioning) : 연속적인 숫자나 날짜를 기준으로 하는 파티셔닝 / 관리가 쉽고 관리 시간 단축이 가능
        ```
        [EX] 우편번호, 일별, 월별, 분기별등
        ```
    2. 해시 파티셔닝 (Hash Partitioning) : 파티션 키 + 해시 함수값에 의한 파티셔닝 / 균등한 데이터 분할 O , 질의 성능 향상
        ```
        파티션을 위한 범위가 없는 데이터에 적합 
        ```
    3. 리스트 파티셔닝 (List Partitioning) : 특정 파티션에 저장 될 테이터에 대한 **명시적 제어**가 가능한 파티셔닝 / 분포도가 비슷하고 컬럼의 조건이 많이 들어오는 경우 유용
        ```
        [EX] 
        한국 , 일본, 중국 → 아시아 
        노르웨이, 스웨덴, 핀란드 → 북유럽
        ```
    4. 컴포지트 파티셔닝 (Composite Partitioning) : 2개 이상의 파티셔닝을 결합하는 파티셔닝
        ```
        큰 파티션에 대하 I/O 요청을 여러 파티션으로 분산 가능
        파티션이 너무 커서 효과적으로 관리할 수 없을 때 유용
        ```
    - 파티션의 *장점* : 성능향상, 가용성 향상, 백업 가능, 경합 감소('디스크스트라이핑'으로 입출력 성능을 향상) 
        > [Disk Striping] 성능 향상을 위해 데이터를 1개 이상의 디스크 드라이브에 저장하여 **드라이버를 병렬로 사용할 수 있는** 기술
## 3-3. 데이터베이스 기초 활용
### * 데이터베이스 
- 정의 : 데이터베이스란 다수의 인원, 시스템 or 프로그램이 사용할 목적으로 **통합하여 관리되는 데이터의 집합**
    - 데이터 효과적 관리를 위해 ***자료의 중복성, 무결성 확보, 일성 유지, 유용성 보장***이 중요
    - 데이터 베이스 = 통합된 데이터(자료 중복 배제한 데이터 모임) + 저장된 데이터 + 운영 데이터 (조직업무를 수행하는데 필요한 데이터) + 공용 데이터 (공동으로 사용되는 데이터) 
- 데이터베이스 *특성* 
    1. 실시간 접근성 (Real-Time Accessibility) : 쿼리에 대해 실시간 응답 가능해야함 
    2. 계속적인 변화 (Continuous Evolution) : 새로운 데이터의 삽입, 삭제, 갱신으로 항상 최신의 데이터를 유지함 
    3. 동시공용 (Concurrent Sharing) : 다수의 사용자가 동시에 같은 내용의 데이터를 이용할 수 있어야함
    4. 내용 참조 (Content Reference) : 데이터를 참조할 때 사용자가 요구하는 데이터 내용으로 데이터를 찾음

- 데이터베이스 *종류*
    1. 파일 시스템 : 파일에 이름을 부여하고 저장이나 검색을 위해 어디에 위치시켜야 하는지 정의한 후 관리하는 데이터 베이스 전 단계의 데이터 관리 방식
    >[파일 시스템 종류] <br> - ISAM (**I**ndexed **S**equential **A**ccess **M**ethod) : 자료 내용은 주 저장부 , 자료 색인은 자료가 기록된 위치와 함께 색인부에 기록되는 시스템 <br> - VSAM (**V**irtual **S**torage **A**ccess **M**ethod) : 대형 운영체제에서 사용되는 파일 관리시스템
    2. 관계형 데이터베이스 관리시스템 (RDBMS; Relational Database Management System) : 관계형 모델을 기반으로 하는 가장 보편화된 데이터베이스 관리시스템
    >> [RDBMS 종류] Oracle, SQL Server, MySQL, Maria DB등
    3. 계층형 데이터베이스 관리시스템 (HDBMS; Hierachical Database Management System) : 데이터를 **상하 종속적인 관계로 계층화해** 관리하는 데이터베이스
    >> [HDBMS 종류] IMS, System2000등
    4. 네트워크 데이터베이스 관리시스템 (NDBMS; Network Database Management System) : 데이터 구조를 **네트워크상의 망상형태로** 표현한 데이터 모델
    >> [NDBMS 종류] IDS, IDMS

### * DBMS 
- DBMS는 데이터 추가, 변경, 검색등의 기능을 지원하는 소프트웨어
- DBMS *유형* : 관리하는 데이터 형태 및 관리 방식에 따하 나뉨
    | 유형 | 설명 |
    |----|---|
    | 키-값(Key-Value) DBMS | - Key 기반 Get, Put, Delete 제공 / 빅데이터 처리가 가능한 DBMS <br> - ex) Redis, DynamoDB |
    | 컬럼 기반 데이터 저장 DBMS | - Key 안에 (컬럼, 값) 조합으로 된 여러개의 필드를 갖는 DBMS / 구글의 빅데이터 기반으로 구현 <br> - ex) HBase, Cassandra |
    | 문서 저장 DBMS  | 값(Value)의 데이터 타입이 문서라는 타입으로 사용하는 DBMS / 복잡한 계층 구현 표현 가능 (문서타입은 XML, JSON과 같이 구조화된 데이터 타입) <br> - ex) MongoDB, Couchbase |
    | 그래프 DBMS | - 시멘틱웹과 온톨로지 분야에서 활용되는 그래프로 데이터를 표현하는 DBMS / 노드간 관계를 구조화하여 저장 <br> - ex) Neo4j, AllegroGraph |
- DBMS *특징*
    1. 무결성 : 동일한 내용에 대해 서로 다른 데이터가 저장되는 것을 허용하지 X
    2. 일관성 : 삽입, 삭제, 갱신, 생성 후에도 저장된 데이터가 변함없이 일정
    3. 회복성 : 장애가 발생했을 때 특정 상태로 복구되어야 함 
    4. 보안성 : 불법적인 노출, 변경, 손실로부터 보호되어야 함
    5. 효율성 : 응답시간, 저장 공간 활용등이 최적화되어 사용자, 시스템등의 요구조건을 만족시켜야함
### * 데이터베이스 *기술 트랜드*
- 빅데이터 : 빅데이터는 수십 페타바이트(PB)크기의 비정형 데이터
    - 특성 : 양, 다양성, 속도**
        1. 데이터의 **양** (Volume) : 페타파이트 수준의 대규모데이터, / 디지털 정보량이 폭증하는것을 의미
        2. 데이터의 **다양성** (Variety) : 정형, 비정형, 반정형의 다양한 데이터 / 데이터 유형이 다양해지는 것을 의미
        3. 데이터의 **속도** (Velocity) : 빠르게 증가하고 수집되며 처리되는 데이터 / 실시간 분석이 중요해지는것을 의미 
    - 빅데이터 **수집 & 저장 & 처리** 기술
        | 구분 | 설명 |
        |----|---|
        | 비정형 / 반정형 데이터 수집 | 내&외부 **정제되지 않은 데이터**를 확보해 필요한 정보 추출해 활용하기 위해 효과적으로 수집 및 전송하는 기술 <br> ex) 척와(Chukwa), 플럼(Flume), 스크라이브(Scribe) |
        | 정형 데이터 수집 | 내&외부 **정제된 대용량 데이터**의 수집 및 전송 기술 <br> ex) ETL, FTP, 스쿱(Sqoop) , 하이호(Hiho) |
        | 분산데이터 저장 / 처리 | **대용량 파일**의 효과적인 분산 저장 및 처리 기술 <br> ex) HDFS , 맵 리듀스 <br> - HDFS (Hadoop Distributed File System) : 대용량 데이터의 집합을 처리하는 응용 프로그램에 적합하도록 설계된 하둡 분산 파일 시스템 <br> - 맵 리듀스 (Map Reduce) : 구글에서 대용량 데이터 처리를 분산 병렬 컴퓨팅에서 처리하기 위한 목적으로 제작해 2004년에 발표한 소프트웨어 프레임워크
        | 분산데이터 베이스 | HDFS의 컬럼 기반 데이터베이스로 실시간 랜덤 조회 및 업데이트가 가능한 기술 <br> ex) HBase |
    - 빅데이터 **분석, 실시간 처리 및 시각화**를 위한 주요 기술
        | 구분 | 설명 |
        |----|---|
        | 빅데이터 **분석** | - 데이터의 가공과 분류, 클러스터링, 패턴 분석을 처리하는 기술 <br> - 데이터 **가공**을 위한 대표적인 솔루션 → 피그(Pig) , 하이브(Hive) <br> - 데이터 **마이닝**을 위한 대표적인 솔루션 → 머하웃(Mahout) |
        | 빅데이터 **실시간 처리** | - 하둡 기반의 실시간 SQL 질의 처리와 요청된 작업을 최적화하기 위한 워크플로우 관리 기술 <br> - **실시간 SQL 질의**를 위한 대표적인 솔루션 → 임팔라(Impala) <br> - **워크플로우 관리**를 위한 대표적인 솔루션 → 우지(Oozie) |
        | **분산 코디네이션** | 분산 환경에서 서버들 간에 상호조정이 필요한 다양한 서비스를 분산 및 동시처리 제공 기술 <br> - 분산코디네이션을 위한 대표적인 솔루션 → 주키퍼(Zookeeper) |
        | **분석 및 시각화** | 분석된 데이터의 의미와 가치를 시각적으로 표현하기 위한 기술 <br> - 분석 및 시각화를 위한 대표적인 솔루션 → 알(R)
- NoSQL : RDBMS 와 다른 DBMS를 지칭하기 위한 용어, 데이터 저장에 고정된  테이블 스키마가 필요 X , 조인연산 X, 수평적으로 확장이 가능한 DBMS
    - 특성 : 
        1. Basically Available : 언제든지 데이터는 접근할 수 있어야함 / 가용성 중시 
        2. Soft-State : 노드의 상태는 외부에서 전송된 정보를 통해 결정됨 / 특정시점에서는 데이터의 일관성이 보장되지 X 
        3. Eventually Consistency : 일정 시간이 지나면 데이터의 일관성이 유지됨 
    - 유형 → DBMS 유형 참조(동일)
- 데이터 마이닝 (Data Mining) : 대규모로 저장된 데이터 안에서 체계적이고 자동적으로 통계적 규칙이나 패턴을 찾아내는 기술
    - 데이터 마이닝 *절차* 
        > 목적 설정 → 데이터 준비 → 가공 → 마이닝 기법 적용 → 정보 검증
    - 데이터 마이닝 *주요기법*
        | 주요기법 | 설명 |
        |----|---|
        | 분류 규칙 (Classification) | - 과거 데이터로부터 특성을 찾아내 분류모형을 만든 후 이를 토대로 새로운 레코드의 결과 값을 예측하는 기법 <br> 마케팅, 고객 신용평가 모형에 활용 |
        | 연관 규칙 (Association) | 데이터 안에 존재하는 항목들 간의 종속관계를 찾아내는 기법 |
        | 연속 규칙 (Sequence) | 연관규칙 + 시간관련 정보 |
        | 데이터 군집화 (Clustering) | - 대상 레코드들을 유사한 특성을 지닌 소그룹으로 분할하는 작업으로 분류규칙과 유사 <br> - 정보가 없는 상태에서 데이터를 분류하는 기법 |  