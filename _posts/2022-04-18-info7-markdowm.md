---
layout: post
title: 7. SQL 응용
subtitle: ★★★ 
categories: markdown
tags: [정보처리기사_실기]
---

# 7. SQL 응용 
## 7-1. 데이터베이스 기본
### * 트랜잭션 (Transaction) : **인가받지 않은 사용자로부터 데이터를 보장**하기 위해 DBMS가 가져야하는 특성, 하나의 논리적 기능을 수행하기 위한 **작업의 기본 단위** 
- 특성 : (ACID)
    | 특성 | 설명 | 주요기법
    |-----|----|---
    | 원자성 <br> (**A**tomicity) | - 분해가 불가능한 작업의 최소단위 <br> - 연산 전체가 성공 or 실패 (All or Noting) <br> - **하나라도 실패 → 전체 취소** | Commit, Rollback, 회복성 보장 
    | 일관성 <br> (**C**onsistency) | 트랜잭션이 실행 or 성공 후 **항상 일관된 데이터 상태를 보존**해야함 | **[병행제어]** <br>  무결성 제약조건, 동시성 제어  
    | 격리성 <br> (**I**solation) | 트랜잭션이 실행 중 생성하는 연산 중간의 결과를 다른 트랜잭션이 **접근 불가**한 특성 | **[데이터베이스 고립화]** <br> - Read Uncommited <br> ``` 커밋되지 않은 데이터를 다른 트랙잰션이 읽는 것 O , 연산 X    ``` <br> - Read Commited <br> ``` 커밋된 데이터만 읽기 O ``` <br> - Repeatable Read <br> ``` 트랜잭션 종료시까지 해당 데이터에 대한 갱신,삭제 X ``` <br> - Serializable <br> ``` 해당 데이터 영역 전체 제한 ```  
    | 영속성 <br> (**D**urability) | 성공으로 끝난 트랜잭션의 결과는 영속적으로 데이터베이스에 저장함 | **[회복기법]**
- 트랜잭션 *상태 변화* : (활부완실철) 
    > **활동** 상태 → **부분 완료** 상태 → **완료** 상태 → **실패** 상태 → **철회** 상태
    ```
    1. 활동 상태 (Activity) : 초기 상태 
    2. 부분 완료 상태 (Partially Committed) : 마지막 명령문이 실행 된 후 가지는 상태
    3. 완료 상태 (Committed) : 트랜잭션이 성공적으로 완료된 상태 
    4. 실패 상태 (Failed) : 정상적인 실행이 더 이상 진행될 수 없는 상태
    5. 철회 상태 (Aborted) : 트랜잭션 취소 후 데이터베이스가 트랜잭션 시작 전 상태로 돌아간 상태
    ```
- 트랜잭션 *제어* : (커롤체)
    - 트랜잭션의 제어언어(TCL; Transaction Control Language)는 트랜잭션의 결과를 **허용 or 취소**하는 목적으로 사용되는 언어 
        1. 커밋 (COMMIT) : 트랜잭션 **확정**, 트랜잭션을 메모리에 영구적으로 저장하는 명령어 
        2. 롤백 (ROLLBACK) :** 트랜잭션 **취소**, 트랜잭션 저장을 무효화시키는 명령어
        3. 체크포인트 (CHECKPOINT) : **저장 시기 설정**, ROLLBACK을 위한 시점을 지정하는 명령어
    
--------

- 병행 제어 (Concurrency Control) : 여러 트랜잭션을 수행할 때 **데이터 베이스 일관성 유지를 위해** 상호작용을 제어하는 기법
    - 목적 : 데이터베이스 공유 & 시스템 활용도 최대화 , 데이터베이스 일관성 유지, 사용자 응답시간 최소화 
    - 병행제어 미보장했을 경우 **(갱현모연)** 이 일어남 
        > ① 갱신 손실 (Lost Update) : 먼저 실행된 트랜잭션의 결과를 나중에 실행된 트랜잭션이 덮어쓸 때 발생하는 오류 <br> ② 현황 파악 오류 (Dirty Read) : 트랜잭션의 중간 결과를 다른 트랜잭션이 참조하여 발생하는 오류 <br> ③ 모순성 (Inconsistency) : 두 트랜잭션이 동시에 실행돼 데이터베이스 일관성이 결여되는 오류 <br> ④ 연쇄복귀 (Casecading Rollback) : 특정 트랜잭션이 처리를 취소할 경우 트랜잭션이 그 부분을 취소하지 못하는 오류 
    - 병행 제어 기법 *종류* : (로 낙탄다; **오** **낙타** 탄**다**) 
        | 기법| 설명 
        |----|---
        | 로킹 (Locking) | - 트랜잭션의 **순차적 진행을 보장하는 직렬화** 기법 <br> - 데이터베이스, 파일 , 레코드 등은 로킹 단위가 될 수 O <br> - 로킹 단위가 작아짐 → 데이터베이스 공유도 & 로킹 오버헤드 ↑ <br> - 한꺼번에 로킹할 수 있는 객체의 크기 = 로킹단위 
        | 낙관적 검증 | 트랜잭션이 어떤 검증도 하지 않고 트랜잭션을 수행한 후 종료 했을 때 검증을 수행해 데이터베이스에 반영하는 기법 <br> (어떻게든되겠지~ 마지막에혀) 
        | 타임 스탬프 순서 (Time Stamp Ordering) | 트랜잭션 실행 전 타임 스탬프를 부여해 부여된 시간에 따라 트랜잭션을 수행하는 기법 
        | 다중버전 동시성 제어 <br> (MVCC; Multi Version Councurency Control) | 트랜잭션의 타임스탬프와 접근하려는 데이터의 타임스탬프를 비교해 직렬가능성이 보장되는 버전을 선택해 접근하게 하는 기법
---

- 데이터베이스 고립화 수준 : 다른 트랜잭션이 현재 데이터에 대한 **무결성을 해치지않게 잠금을 설정하는 정도**
    - 종류 : Read Uncommitted / Read Committed / Rapeatable Read / Serializable Read

---
- 회복 기법 : 트랜잭션을 수행하는 도중 **장애로 인해 손상된 데이터베이스를 손상되기 이전 상태로 복구시키는 작엄**
    - 종류 : (회로체크) ; 회복하는데 로로체크
        1. **로그기반** 회복 기법 
            ```
            - 지연 갱신 회복 기법 (Deferred Update) : 트랜잭션이 완료되기 전 까지 DB에 기록하지 X
            - 즉각 갱신 회복 기법 (Immediate Update) : 트랜잭션 수행 중 갱신 결과를 바로 DB에 반영하는 기법
            ```
        2. **체크 포인트** 회복 기법 (Checkpoint Recovery) : 검사한 지점 이후에 처리된 트랜잭션에 대해서만 장애 발생 전 상태로 복원시키는 회복 기법
        3. **그림자 페이징** 회복 기법 (Shadow Paging Recovery) : 트랜잭션 실행 시 **복제본을 만들어** 복제본을 이용해 복구하는 회복 기법

### * DDL (Data Definition Language) : 데이터를 정의하는 언어 
- DDL 대상 : (도스테뷰인) ; **도**를 아는 **스**님 **테**라스에 앉아 **뷰**를 **인**어본다
    1. 도메인 : 하나의 속성이 가질 수 있는 **원자값들의 집합** / 속성의 데이터 타입과 크기, 제약조건 등의 정보
    2. 스키마 : DB의 구조, 제약조건등의 정보를 담고있는 **기본적인 구조**
        - 외부 스키마 (서브스키마)  : **사용자나 개발자 관점**에서 필요로 하는 **DB의 논리적 구조** , 사용자 뷰를 나타냄
        - 내부 스키마 : 물리적 저장장치의 관점에서 보는 DB 구조 / 실제 DB에 저장될 레코드 형식 정의
        - 개념 스키마 : DB의 **전체적인** 논리적 구조 / 전체적인 뷰를 나타냄 / 개체간의 관계, 제약조건, 접근 권한, 무결성, 보안에 대해 정의
    3. 테이블 : 데이터 **저장 공간**
        - 생성 ***"CREATE"*** 
            ```SQL
            CREATE TABLE 테이블명
            (
                컬럼명 데이터타입 PRIMARY KEY,
                컬럼명 VARCHAR2(100) FOREIGN KEY REFERENCES 참조테이블(기본키),
                컬럼명 NUMBER(50) UNIQUE, --UNIQUE : 유일값
                컬럼명 VARCHAR2(10) NOT NULL,
                컬럼명 VARCHAR2(10) CHECK (성별 = 'M' OR 성별 = 'F') , --제약조건 설정 (조건식)
                컬럼명 DATE DEFAULT SYSDATE -- DEFAULT : INSERT할 때 값을 넣지 않았응 경우 기본값으로 설정해주는 제약조건 
            );
            ```
        - 수정 ***"ALTER"***
            ```SQL
            --컬럼 '추가'
            ALTER TABLE 테이블명 ADD 컬럼명 데이터타입 [제약조건];
            ALTER TABLE 사원 ADD 전화번호 VARCHAR2(10) UNIQUE;

            -- 컬럼 '수정'
            ALTER TABLE 테이블명 MODIFY 컬럼명 데이터타입 [제약조건];
            ALTER TABLE 사원 MODIFY 이름 VARCHAR2(30) NOT NULL;

            -- 컬럼 '삭제'
            ALTER TABLE 테이블명 DROP 컬럼명;
            ALTER TABLE 사원 DROP 생년월일;
            ```
        - 삭제 ***"DROP(테이블) / TRUNCATE(데이터)"***
            ```SQL
            DROP TABLE 테이블명 [CASECADE | RESTRICT]; -- CASECADE : 참조하는 테이블까지 연쇄적으로 제거 / RESTRICT : 다른테이블이 삭제할 테이블을 참조하면 제거하지 X
            DROP TABLE 사원;

            TRUNCATE TABLE 테이블명;
            TRUNCATE TABLE 사원;
            ```
    4. 뷰 : 하나 이상의 물리 테이블에서 유도되는 **가상 테이블**
        - 생성 ***"CREATE"***
            ```SQL 
            CREATE VIEW 뷰이름 AS 조회쿼리;
            
            CREATE VIEW 사원뷰 AS
            SELECT 사번.이름 FROM 사원 WHERE 성별 = 'M'; 
            --VIEW TABLE 에는 UNION(중복행이 제거된 쿼리 겨로가 집합) or ORDER BY 절 사용 X
            ```
        - 교체 ***"CREATE OR REPLACE"***
            ```SQL
            CREATE OR REPLACE VIEW 뷰이름 AS 조회쿼리; --뷰를 교체하는 명령
            ```
        - 삭제 ***"DROP"***
            ```SQL
            DROP VIEW 뷰이름;
            ```
        - 뷰 장단점 
            |구분|장단점|설명|
            |----|---|---
            |장점|논리적 독립성 제공 | DB에 영향을 주지않고 어플리키에션이 원하는 형태로 데이터 접근 O 
            ||사용자 데이터 관리 용이 | 여러 종류의 데이터에 대해 단순한 질의어 사용 O
            ||데이터 보안의 용이 | 특정 필드만 선택해 뷰를 생성한 경우 선택되지 않은 테이블 접근 X 
            |단점|뷰 자체 인덱스 X| 인덱스는 물리적으로 저장된 데이터를 상대로 하기 때문에 논리적 구성인 뷰 자체는 인덱스를 가지지 못함 
            ||뷰 정의 변경 X | 정의 변경을 하려면 뷰 삭제하고 재생성 
            ||데이터 변경 제약 존재| 뷰의 내용에 대한 삽입, 삭제, 변경 제약이 有
    5. 인덱스 : 검색을 빠르게 하기 위한 데이터 구조
        - 생성 ***"CREATE"***
            ```SQL 
            -- UNIQUE 생략 O, 중복값 허용하지 X 
            CREATE [UNIQUE] INDEX 인덱스명 ON 테이블명(컬럼명1, 컬럼명2,...);
            CREATE INDEX 사번인덱스 ON 사원(사번);
            ```
        - 수정 ***"ALTER"***
            ```SQL
            ALTER [UNIQUE] INDEX 인덱스명 ON 테이블명(컬럼명1, 컬럼명2,...);
            ALTER INDEX 사번인덱스 ON 사원(사번);
            ```
        - 삭제 ***"DROP"***
            ```SQL
            DROP INDEX 인덱스명;
            ```
        - 인덱스 종류 : (순해비함 단결클) ; **순해**보이지만 **비함**지르면 **단결**하는 **클**라스
            |종류|설명
            |----|---
            |순서 인덱스 (Ordered Index) | 데이터가 정렬된 순서로 생성되는 인덱스 / B-Tree 알고리즘 활용(오름차순&내림차순 활용 O)
            |해시 인덱스 (Hash Index) | 데이터에 키-값으로 접근하는 인덱스 / 데이터에 접근비용이 균일 / 튜플 양에 무관
            |비트맵 인덱스 (Bitmap Index) | 각 컬럼에 적은 갯수 값이 저장된 경우 선택하는 인덱스 / 수정변경이 적을 경우 용이(ex. 생년월일 or 상품번호 등)
            |함수기반 인덱스 (Functional Index) | 수식이나 함수를 적용하는 인덱스
            |단일 인덱스 (Singled Index) | 하나의 컬럼으로 구성한 인덱스
            |결합 인덱스 (Concatenated Index) |  두 개 이상의 컬럼으로 구성한 인덱스 / WHERE 조건으로 사용하는 빈도가 높은 경우 사용 
            |클러스터드 인덱스 (Clustered Index) | PK 기준으로 레코드(행)를 묶어서 저장하는 인덱스
### * DCL (Data Control Language) : 관리자가 사용하는 제아용 언어
1. GRANT : 권한 부여
    ```SQL
    GRANT 권한 ON 테이블 TO 사용자;
    GRANT UPDATE ON 사원 TO 장길산;
    ```
2. REVOKE : 권한 회수
    ```SQL
    REVOKE 권한 ON 테이블 FROM 사용자;
    ```
## 7-2. 응용 SQL 작성
### * 집계성 SQL 작성 
- 데이터 분석 함수 : 데이터 분석을 우해 복수 행 기준의 데이터를 모아 처리하는것을 목적으로 하는 다중 행 항수
    - 종류 : 집계, 그룹, 윈도 함수 
- **집계** 함수 : 여러 행 or 테이블 전체 행으로부터 **하나의 결과값으 반환**하는 함수
    >[종류] COUNT, SUM, AVG, MAX, MIN, STDDEV(표준편차), VARIAN(분산)
- **그룹** 함수 : **그룹별로 결과를 출력**하는 함수
    1. ***ROLLUP*** : 소그룹의 합계 등 **중간 집계 값을 출력**하기 위한 그룹 함수 
        - 순서가 바뀌면 수행결과도 바뀜 
        - 대상이 되는 컬럼 = ROLLUP 뒤 / 아닌 경우 = GROUP BY 뒤 
        ```SQL
        SELECT DEPT, JOB, SUM(SALARY) FROM DEPT_SALARY 
            GROUP BY ROLLUP(DEPT, JOB);
        ```
    2. ***CUBE*** :  결합 가능한 모든 값에 대한 집계를 생성하는 그룹 함수 
        - 연산이 많아 시스템에 부담 
        ```SQL
        SELECT DEPT, JOB, SUM(SALARY) FROM DEPT_SALARY
            GROUP BY CUBE(DEPT, JOB);
        ```
    3. ***GROUPING SETS*** : 컬럼들에 대한 **개별집계**, ROLLUP&CUBE와 달리 컬럼 간 **순서와 무관한 결과** 출력이 가능한 그룹 함수
        ```SQL
        SELECT DEPT, JOB, SUM(SALARY) FROM DEPT_SALARY 
            GROUP BY GROUPING SETS(DEPT, JOB, ());
        ```
- **윈도** 함수 (= OLAP; 의사결정 지원 시스템) : 온라인 분석 처리 용도
    ```SQL 
    SELECT 함수명(파라미터)    
        OVER                                -- OVER 필수
        ([PARTITION BY 컬럼1, ... ]         -- 선택항목 , PARTITION BY를 통해 구분된 레코드의 집합 = 윈도
        [ORDER BY 컬럼A, ...])              -- 어떤 열을 어떤 순서로 정할지 결정
        FROM 테이블명
    ```
    1. 순위 함수 : 레코드의 순위를 계산 
        > RANK, DENSE_RANKM, ROW_NUMBER
        ```SQL
        SELECT NAME, SALARY, 
            RANK() OVER (ORDER BY SALARY DESC) A,
            DENSE_RANK() OVER (ORDER BY SALARY DESC) B,
            ROW_NUMBER() OVER (ORDER BY SALATY DESC) C
        FROM EMPLOYEE;
        ``` 
    2. 행 순서 함수 : 레코드에서 이전/이후의 값들을 출력하는 함수
        > FIRST_VALUE(MIN과 비슷), LAST_VALUE, LAG, LEAD
        ```SQL
        SELECT NAME, SALARY,
            FIRST_VALUE(NAME) OVER (ORDER BY SALARY DESC) A,         -- 내림차순(DESC) → MAX / 오름차순(ASC) → MIN 
            LAST_VALUR(NAME) OVER (ORDER BY SALARY DESC) B,         -- 내림차순(DESC) → MIN / 오름차순(ASC) → MAX
            LAG(NAME) OVER (ORDER BY SALARY DESC) C,        -- 이전 로우 값
            LEAD(NAME) OVER (ORDER BY SALARY DESC) D        -- 이후 로우 값
        FROM EMPLOYEE;
        ``` 
    3. 그룹 내 비율 함수 : 백분율, 통계 
        > RATIO_TO_REPORT, PERCENT_RANK 
        ```SQL
        SELECT NAME, SALARY,
            RATIO_TO_REPORT(SALARY) OVER A,
            PERCENT_RANK() OVER (ORDER BY SALARY DESC) B
        FROM EMPLOYEE;
        ```
## 7-3. 절차형 SQL 활용
### * 절차형 SQL : 개발 언어처럼 SQL 언어에서도 절차 지향적인 프로그램이 가능하도록 하는 트랜잭션 언어
- 종류 : 프로시저, 사용자정의함수, 트리거
    1. 프로시저 (Procedure) : 일련의 쿼리들을 하나의 함수처럼 실행하기 위한 **쿼리의 집합**
        > [프로시저 구성] 디비컨 SET : 선언부(DECLARE) , 시작/종료부(BEGIN/END) , 제어부(CONTROL; 조건문과 반복문을 이용해 문장처리) , SQL , 예외부(EXCEPTION), 실행부(TRANSACTION) 
        ```SQL
        # 프로시저 '문법'

        CREATE [OR REPLACE] PROCEDURE 프로시저명 
            (파라미터명 [IN | OUT | INOUT] 데이터타입, ...) IS 변수 선언        -- 변수의 입출력 구분 (IN : 운영체제 → 프로시저로 값 전달모드 / OUT : 프로시저 → 운영체제로 값 전달 모드 / INOUT : IN + OUT)
            BEGIN
                명령어;
            [COMMIT | ROLLBACK]
            END;

        # 프로시저 '호출문'

        EXECUTE 프로시저명 (파라미터1, 파라미터2, ...);
        ```
    2. 사용자 정의 함수 (User-Defined Function) : SQL 처리를 수행하고 수행 결과를 단일 값으로 반환할 수 있는 절차형 SQL
        - 프로시저와 동일하나 **반환 부분만 다름(단일값 반환)**
        > 디비컨 SE**R** : DECALRE, BEGIN/END, CONTOL, SQL, EXCEPTION, **RETURN**
        ```SQL
        CREATE [OR REPLACE] FUNCTION 함수명 
            (파라미터 명 IN 데이터 타입, ...)
            RETURN 데이터 타입
            IS 변수 선언
            BEGIN 명령어;
            RETURN 변수;                        -- 호출문에 대한 함수값 반환 
        END;
        ```
    3. 트리거 (Trigger) : DB 시스템에서 삽입, 갱신, 삭제등 이벤트가 발생할때마다 자동으로 수행되는 절차형 SQL 
        - ***'행'*** 트리거 : **데이터 변화**가 생길때마다 실행
        - ***'문장'*** 트리거 : 트리거에 의해 **단 한번** 실행 
        > 디**이**비컨 SE : DECLARE, **EVEN**, BEGIN/END, CONTROL, SQL, EXCEPTION
        ```SQL
        ** TCL(트랜잭션 제어어; COMMIT, ROLLBACK ) 사용 X , 오류에 주의 **

        CREATE [OR REPLACE] TRIGGER 트리거명 
            [BEFORE | AFTER] 유형 ON 테이블명       -- BEFORE : INSERT/UPDATE/DELETE를 수행하기 전 실행 | AFTER : 수행 후 실행 | 유형 : INSERT/UPDATE/DELETE 
            [FOR EACH ROW]                         --  매번 변경되는 데이터 행의 수만큼 실행
            BEGIN
        END;
        ```
- DBMS_OUTPUT : 메세지를 버퍼에 저장하고 버퍼로부터 메세지를 읽어보기 위한 인터페이스 패키지 
    - SQL이 정상적으로 구현됐는지 테스트하는 목적으로 多 사용
    ```SQL
    DBMS_OUTPUT.PUT(문자열);        -- 개행 없이 문자열 출력
    DBMS_OUTPUT.PUT_LINE(문자열)    -- 문자열 출력 후 개행
    ```
- 조건문 
    1. IF 
        ```SQL
        IF 조건 THEN 문장;
        ELSIF 조건 THEN 문장;
        ELSE 문장;
        END IF;
        
        ```
    2. CASE
        ```SQL
        CASE 변수 
            WHEN 값1 THEN SET 명령어;
            WHEN 값2 THEN SET 명령어;
            ELSE SET 명령어;
        END CASE;

        CASE 
            WHEN 조건1 THEN SET 명령어;
            WHEN 조건2 THEN SET 명령어;
            ELSE SET 명령어;
        END CASE;
        ```
- 반복문
    1. LOOP : 특정 조건이 만족할때까지 반복
        ```SQL
        LOOP 
            문장;
            EXIT WHEN 탈출조건;
        END LOOP;
        ```
    2. WHILE : 조건 참 = 반복 / 거짓 or EXTI WHEN = 반복문 탈출!
        ```SQL
        WHILE 반복 조건 LOOP 
            문장;
        EXIT WHEN 탈출조건;
        END LOOP;
        ```
    3. FOR LOOP : 시작값, 끝값 지정해 해당값이 그 구간내에 있으면 반복
        ```SQL
        FOR 인덱스 IN 시작값 .. 종료값
        LOOP 
            문장;
        END LOOP;
        ```
- 예외문 : 실행 중 발생 가능한 예외상황 수행 
    ```SQL
    EXCEPTION 
        WHEN 조건 THEN SET 명령어;
    ```
## 7-4. 데이터 조작 프로시저 최적화
### * 데이터 조작 프로시저 *성능 개선* : 최소의 시간으로 원하는 결과를 얻도록 프로시저를 수정하는 작업 
- 성능 개선 ***절차***
    1. 문제있는 SQL 식별 : APM등 활용 
        > [APM?] 성능 모니터링 도구
    2. 옵티마이저 통계 확인 
        > [옵티마이저?] 작성된 SQL을 가장 빠르고 효율적으로 수행할 **최적의 처리경로를 생성해주는 DB 핵심모듈** <br> 옵티마이저가 생성한 SQL 처리경로 = 실행계획(Execution Plan)
        - 유형 : RBO (Rule Based Optimizer; 규칙기반) / CBO (Cost Based Optimizer; 비용기반)
            | 비교 | RBO | CBO 
            |-----|----|---
            | 개념 | **통계정보가 없는 상태에서 사전 등록된 규칙에 따라** 질의 실행 계획을 선택하는 옵티마이저 | **통계정보로부터**1 모든 접근 경로를 고려한 질의 실행 계획을 선택하는 옵티마이저
            | 핵심 | 규칙(우선순위)기반 | 비용(수행시간) 기반
            | 평가기준 | 인덱스 구조, 연산자, 조건절 형태 등 | 레코드 개수, 블록 개수, 평균 행 길이 등 
            | 장점 | 사용자가 원하는 처리 경로로 유도하기 쉬움 | 옵티마이저 이해도가 낮아도 성능 보장 가능
        - 역할 : 쿼리변환, 비용 산저, 계획 생성  
            | 서브 엔진 | 역할
            |----|---
            | 쿼리변환 (Query Transformer) | SQL을 더 일반적이고 표준화된 형태로 변환
            | 비용산정 (Estimator) | - 각 단계의 선택도, 카디널리티, 비용을 계산 <br> ```[선택도] 전체 대상 레코드 중 특정 조건에 의해 선택될 것으로 예쌍되는 레코드 비율 ```<br> - 실행계획 전체에 대한 총 비용 계산 
            | 계획 생성 (Plan Generator) | 하나의 쿼리 실행 시 후보군이 될만한 실행계획들을 생성해내는 역할 
        - Hint : Optimizer가 항상 최선의 실행계획을 수립할 수 없어 Hint를 통해 실행계획을 변경 
            > [Hint] 실행하려는 SQL문에 사전 정보를 줘 SQL문 실행에 빠른 결과를 가져오는 효과를 만드는 문법
            ```SQL
            SELECT /*+ RULE */ ENAME, SAL FROM EMP WHERE EMPNO > 9000;
            -- 비용 기반 옵티마이저에서 규칙기반 옵티마이저로 변경
            ```
            | 힌트 | 설명 
            |-----|----
            | /*+ RULE */ | 규칙 기반 접근방식을 사용하도록 지정
            | /*+ CHOOSE */ | 오라클 옵티마이저 디폴트값에 따름
            | /*+ INDEX(테이블명 인덱스명) */ | 지정된 인덱스를 강제적으로 사용하도록 지정
            | /*+ USE_HASH(테이블명) */ | 지정된 테이블들의 조인이 *Hash Join* 형식으로 일어나도록 유도
            | /*+ USE_MERGE(테이블명) */ | 지정된 테이블들의 조인이 *Sort Merge* 형식으로 일어나도록 유도
            | /*+ USE_NL(테이블명) */ | 지정된 테이블들의 조인이 *Nested Loop*형식으로 일어나도록 유도
    3. SQL문 재구성 
        > [구성가이드 ex] 특정 값 지정 / 별도의 SQL 사용 / Hint 사용 / HAVING 미사용 / 인덱스만 질의 사용 
    4. 인덱스 재구성
        > [인덱스 재구성 가이드 ex] 자주 쓰는 컬럼 선정 / SORT 명령어 생략 / 분포도를 고려 / 변경 적은 컬럼 선정 / 결합 인덱스 사용
    5. 실행계획 유지관리
    