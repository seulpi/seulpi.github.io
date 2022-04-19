---
layout: post
title: 7. SQL 응용
subtitle: ★★★ 
categories: markdown
tags: [정보처리기사_실기]
---

# 8. 서버프로그램 구현 
## 8-1. 개발환경 구축
### * 개발 도구의 *분류* : (**빌구테형**)
| 구분| 설명 
|----|---
| ***빌드*** 도구 | 작성한 코드의 빌드 및 배포를 수행하는 도구 <br> ```Ant , Maven, Gradle ```
| ***구현*** 도구 | 개발자의 코드 작성, 디버깅, 수정등과 같은 작업을 지원하는 도구 <br> ```Eclipse, InteliJ, Spring Tool Suite, NetBeans, Visual Studio```
| ***테스트*** 도구 | 코드의 기능 검증과 전체 품질을 높이기 위해 사용되는 도구 <br> ```xUnit, PMD, Findbugs, Cpcheck, Sonar```
| ***형상***도구 | 개발자들이 작성한 코드와 리소스 등 산출물에 대한 버전 관리를 위한 도구 <br>  ```CVS, Git, Subversion```

### * 개발환경 *구성요소* 
- **하드웨어** 개발환경 
    - 서버 하드웨어 개발환경 : 웹서버 | 웹 어플리케이션 | 데이터베이스 서버 | 파일 서버 
    ```
    웹 클라이언트 ↔ 웹서버 ↔ 웹어플리케이션 서버 ← DB 
    
    - 웹서버 : HTTP를 이용한 요청/응답 처리 , 정적컨텐츠(CSS, JavaScript, Image) 처리
    - 웹 어플리케이션 서버 : 동적 컨텐츠(JSP, Servlet) 처리 
    - 데이터베이스 서버 : 데이터의 수집, 저장을 위한 용도로 사용 
    - 파일 서버 : 파일 저장 하드웨어로 물리 저장장치를 활용한 서버
    ```

- **소프트웨어** 개발환경 
    | 구분 | 설명 | 종류
    |-----|----|---
    | 운영체제 | 서버의 하드웨어를 사용자 관점에서 편리하고 유용하게 사용하기 위한 S/W | Windows, Unix, Linux
    | 미들웨어 | 컴퓨터 간 연결을 쉽고 안전하게 할 수 있게 해주고 관리를 도와주는 S/W | Weblogic, Websphere, Jeus, Tomcat 
    | DBMS | 사용자와 DB 사이에서 사용자의 요구에 따라 정보를 생성하고 DB 관리해주는 S/W | Oracle, MySQL, MS-SQL, PostgreSQL 

- **형상관리** : 개발 전과정에서 발생하는 모든 항목의 변경 사항을 관리하기 위한 활동
    > [베이스라인?] 개발 과정의 각 단계의 산출물을 검토, 평가, 조정, 처리 등 변화를 통제하는 시점의 기준
    - 절차 : (**식통감기**)
        > 식별 (ID와 관리번호 부여) - 통제 (형상통제위원회 CCB 운영) - 감사(베이스라ㅏ의 무결성 평가) - 기록 (형상결과 보고서 작성) 
    - 형상관리 *도구 유형* : (**공클분**)
        | 유형 | 종류 | 설명
        |----|----|---
        | 공유 폴더 방식 | RCS, SCCS | 매일 개발 완료된 파일은 약속된 위치의 공유폴더에 복사하는 방식  
        | 클라이언트/서버 방식 | - CSV : 서버와 클라이언트로 구성 <br> - SVN : 하나의 서버에서 소스를 쉽고 유용하게 관리할 수 있게 해주는 도구 | 중앙에 버전 관리 시스템을 항상 동작시키는 방식 
        | 분산 저장소 방식 | Git | 로컬 저장소와 원격 저장소로 분리되어 분산 저장하는 방식 
    - 주요 명령어 
        | 동작 | Git | SVN(Subversion)
        |-----|----|---
        | **저장소 생성** | git **ini**t | svn **import**
        | **저장소 복제** | git **clone** | svn **checkout**
        | 커밋 | git commit | svn commit
        | 변경 내용 확인 | git diff | svn diff
        | 추가 | git add | svn add
        | 이동 | git mv | svn mv
        | 삭제 | git rm | svn rm 
        | **브랜치 생성** | git **branch** | svn **copy**
        | 병합 | git merge | svn merge 
        | **원격 저장소 반영** | git **push** | svn **commit**
        | **설정 확인** | git **config** | svn **info**
## 8-2. 공통 모듈 구현
### * 공통 모듈 : **모듈**은 그 자체로 하나의 **완전한 기능을 수행할 수 있는 독립된 실체**
- 특징 
    1. 독립성 (독립성은 응집도+결합도로 결정 ▶ 독립성 ↑ [결합도 ↓ && 응집도 ↑ && 모듈크기 小 ])
    2. 모듈 내부에 모듈을 하나로 통합 가능한 많은 조합 존재
    3. 단독 컴파일&재사용 O
- **모듈화** : S/W의 기능향상 or 시스템의 수정,재사용 등 관리가 용이하게 **기능 단위의 모듈로 분해하는 설계 및 구현 기밥**
    ```
    # 모듈화 기법
    - 루틴 (Routine) : 기능을 가진 명령들의 모임
    - 메인 루틴 (Main Routine) : 대략적으로 전체 동작 절차를 표시하도록 만들어진 루틴 , 서브 루틴 호출
    - 서브 루틴 (Subroutine) : 메인루틴에 의해 필요할때마다 호출되는 루틴 
    ```
### * S/W 모듈 ***응집도*** : 응집도는 모듈의 독립성을 나타내는 정도 / 모듈 **내부** 구성요소 간 연관 정도 
- 응집도 *유형* : (**우논시절 통순기**)
    |응집도|유형|설명
    |-----|----|---
    |낮 | **우연적** 응집도 <br> (Coincidental Cohesion) |  **모듈 내부의 구성 요소**가 각 연관이 없을 경우 
    | ㅣ| **논리적** 응집도 <br> (Logical Cohesion) | **유사한 성격**을 갖거나 **특정 형태**로 분류되는 처리요소들이 한 모듈에서 처리되는 경우
    | ｜| **시간적** 응집도 <br> (Temporal Cohesion)| **특정 시간**에 처리되어야 하는 활동들을 한 모듈에서 처리할 경우
    | ｜| **절차적** 응집도 <br> (Procedural Cohesion)| 모듈 안의 구성요소들이 기능을 **순차적으로** 수행할 경우
    | ｜| **통신적** 응집도 <br> (Communication Cohesion) | **동일한 입출력**을 사용해 다른 기능을 수행하는 활동들이 모여있을 경우  
    | ▼ | **순차적** 응집도 <br> (Sequential Cohesion) | 한 활동으로부터 나온 출력값을 **다른 활동이 사용**할 경우
    |높 | **기능적** 응집도 <br> (Functional Cohesion) | **모든 기능**이 단일 목적을 위해 수행되는 경우
### * S/W 모듈 ***결합도*** : **외부**의 모듈과의 연관도 / 모듈 간의 상호의존성 
- 결합도 *유형* : (**내공외제스자**)
    |결합도|유형|설명
    |-----|----|---
    |높|**내용** 결합도 <br> (Content Coupling)| 다른 모듈 내부에 있는 변수나 기능을 **다른 모듈에서 사용**하는 경우
    |▲|**공통** 결합도 <br> (Common Coupling)| 모듈 밖에 있는 **전역변수**를 참조하고 전역변수를 갱신하는 식으로 상호작용하는 경우
    |｜|**외부** 결합도 <br> (External Coupling)| 두 개의 모듈이 **외부에서 도입된** 데이터 포맷, 통신 프로토콜 or 디바이스 인터페이스를 공유할 경우 
    |｜|**제어** 결합도 <br> (Control Coupling)| 값만 전달하는 게 아닌 어떻게 처리해야한다는 **제어요소**가 전달되는 경우
    |｜|**스탬프** 결합도 (Stamp Coupling)| **배열,객체, 구조 등이 전달**되는 경우
    |낮|**자료** 결합도 (Data Coupling)| 모듈 간의 인터페이스로 전달되는 **파라미터를 통해서만** 모듈 간의 상호작용이 일어나는 경우

### * 팬인(Fan-In) / 팬아웃(Fan-Out) 
- 팬인(Fan-In) : 어떤 모듈을 제어(호출) **하는** 모듈의 수 
    - 자신을 기준으로 모듈에 **들어오면** Fan-In
    - Fan-In ↑ → 비용 ↑ , 재사용 측면 설계 Good, but 단일 장애점 발생 O 
- 팬아웃(Fan-Out) : 어떤 모듈에 **의해** 제어(호출) **되는** 모듈의 수 
    - 자신을 기준으로 모듈에서 **나가면** Fan-Out
    - Fan-Out ↑ → 불필요한 모듈 호출&단순화 여부 검토 필요
>▶ FOR 시스템 복잡도 최적화 → Fan-In ↑ Fan-Out ↓
---
### * 공통 모듈 ***테스트*** : **IDE(Intergrated Development Environment)**을 도구를 활용해 개별 공통 모듈에 대한 **디버깅**을 수행하는 것 
- 공통 모듈 테스트는 **화이트 박스 기법**을 활용 , jUnit(단위테스트도구)을 활용해 테스트코드 구현  
- 공통 모듈 테스트 *종류* 
    1. **화이트 박스** 테**스트 : 프로그램의 로직을 이해하고 **내부 구조와 동작을 검사**하는 S/W 테스트 방식 <br> 소스 코드를 보면서 테스트 케이스를 다양하게 만들어 테스트를 수행
    2. **메서드 기반** 테스트 : 공통 모듈의 **외부에 공개된** 메서드 기반 테스트 <br> 메서드에 서로 다른 파라미터 값을 호출하면서 다양한 테스트를 수행 
    3. **화면 기반** 테스트 : 사용자용 화면이 있는 경우, 개발 후 **각 화면 단위로 화면에 직접 데이터를 입력**하여 테스트 하는 경우 
    4. **테스트 드라이버(Driver) / 테스트 스텁(Stub)** : 기능을 테스트 할 수 있는 화면 or 하위모듈이 구현되지 않은 경우에 수행 
        - 테스트 드라이버 : 하위 모듈 有 , 상위 모듈 X 
        - 테스트 스텁 : 하위 모듈 X, 상위 모듈 有
### * 서버 프로그램 구현 : 업무 프로세스를 기반으로 개발언어와 도구를 이용해 **서버에서 서비스 제공에 필요한 기능을 구현하는 활동**
- 서버 프로그램 세부 구현 
    > [**디스 다써클**] DTO/VO - SQL문 - DAO - Service - Controller
## 8-4. 배치 프로그램 구현 
### * 배치 프로그램 (Batch Program) : 일련의 작업들을 묶어 정기적으로 반복 수행하거나 정해진 규칙에 따라 일괄처리하는 방법
- 배치 프로그램 *유형* : (**이온정**)
    - **이벤트** 배치 : 사전에 정의해 둔 조건 충족 시 자동으로 실행 
    - **온디맨드** 배치 : 사용자의 명시적 요구가 있을때마다 실행
    - **정기** 배치 : 정해진 시점에 정기적으로 실행

- 배치 *스케줄러* : 배치 프로그램을 지원한는 도구 
    - 스프링배치 (Spring Batch) : 스프링 프레임워크의 3대요소(DI, AOP, 서비스 추상화등)를 모두 사용할 수 있는 대용량처리를 제공하는 스케줄러 배치 어플리케이션 
    - 쿼츠 스케줄러 (Quartz Scheduler) : 스프링 프레임워크에 플러그인되어 수행하는 작업과 실행 스케줄을 정의하는 트리거를 분리해 유연성을 제공하는 오픈 소스 기반 스케줄러
- ***Cron 표현식*** : (**초분시일월요연**) 
    > 초 ,분 , 시간, 일, 월, 요일, 연도
    
    |기호|의미
    |-----|----
    |*| 모든 수 <br>```[ex] 0 0 * * ? → 매일 12시에 실행```
    |?| 해당 항목 미사용 <br>```[ex] 0 15 10 * * ? → 매일 오전 10시 15분에 실행```
    |-| 기간 설정 <br>```[ex] 0 0 20 ? * MON-FRI → 매주 월요일과 금요일 사이 20시에 실행 ```
    |,| 특정 기간 설정 <br>```[ex] 0 0/5 14,20 * * ? → 매일 14시에 시작해 14시 55분까지 5분마다 실행 , 20시 정각부터 20시 55분마다 실행``` 
    |/| 시작 시간과 반복 간격 설정 <br>```[ex] 0 0/5 14,20 * * ? → 매일 14시에 시작해 14시 55분까지 5분마다 실행 , 20시 정각부터 20시 55분마다 실행```
    |L| 마지막 기간에 동작 <br>```[ex] 0 15 10 L * ? → 매달 마지막 날 10시 15분에 실행```
    |W| 가장 가까운 평일에 동작
    |#| 몇번째 주, 요일 설정