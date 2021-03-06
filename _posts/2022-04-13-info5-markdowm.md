---
layout: post
title: 5. 인터페이스 구현
subtitle: ★★★ 
categories: markdown
tags: [정보처리기사_실기]
---

# 5. 인터페이스 구현
## 5-1. 인터페이스 설계 확인
### * 외부 및 내부 모듈 간 인터페이스 데이터 표준 확인
- 인터페이스 데이터 표준 확인 : 상호 연계하고자 하는 시스템 간 **인터페이스가 되어야 할 범위의 데이터 형식과 표준을 정의하는 활동**
```
# 인터페이스 데이터 형태가 같을 경우 → 그대로 전송 / ≠ → 데이터 변환해 전송 
```
- 송수신 시스템 간 인터페이스 데이터 표준 확인 절차
    1. **식별된 데이터 인터페이스를 통해** 인터페이스 데이터 표준 확인 
    > ① 데이터 인터페이스 입출력 의미 파악 <br> 
    ② 데이터 인터페이스 입출력 의미 파악을 통한 테이터 표준 확인
    2. **인터페이스 기능을 통한** 인터페이스 데이터 항목 식별
    3. 데이터 표준 최종 확인 
## 5-2. 인터페이스 기능 구현
### * 인터페이스 기능 구현 *정의*
- 모듈 간 세부 설계서 확인
    1. 컴포넌트 명세서 : 컴포넌트의 개요, 내부 클래스의 동작, 인터페이스를 통해 외부와 통신하는 명세를 정의함 
    > [컴포넌트?] 다른 부품과 조립되어 응용시스템을 구축하기 위해 사용되는 소프트웨어 프로그램
    2. 인터페이스 명세서 : 컴포넌트 명세서에 명시된 인터페이스 클래스의 세부적인 조건 및 기능을 명시한 명세서 
 
### * 인터페이스 기능 *구현*
- 사전에 정의된 기능 구현 정의 내용을 토대로 어떻게 구현할지 분석함
- 인터페이스 기능 구현 *기술*
    1. JSON (Javascript Object Notation) : '속성-값' or '키-값' 으로 이루어진 데이터 오브젝트를 전달하기 위해 인간이 읽을 수 있는 텍스트를 사용하는 개방형 표준 포맷 
        - 특징 : AJAX에서 사용 多 / XML을 대체하는 주요 데이터 포맷 
        - 표현 자료형 : 숫자(number) , 문자(String; " "), 배열(Array; []) , 객체(Object; { })
        - 문법 :
        ```js
        {

            "이름": "홍길동",
            "주소": "서울시 00동",
            "나이": 30,
            "특징": ["A", "B", "C"]
        }

        ====================================
        # 도구
        - Parser : JSON text 파일을 해석&자바 오브젝트로 변환
        - Renderer : 자바를 text로 표현
        - Serializer : POJO를 JSON 표현으로 직렬화
        - Mapper : POJO와 JSON을 매핍
        - Validator : JSON 스키마를 이용해 파일 내용 유효성 체크
        
         # 장점
         - XML보다 가볍고 빠름
         - 자료 종류에 큰 제한 X
         - Javascript 코드 안에서 JSON 객체에 접근이 쉬움

         # 단점
         - 태그 X → 가독성 ↓
         - DTD 같은 것이 X ∴ 데이터 형식이 틀렸을 경우 체크가 쉽지 X
        ```
    2. XML (Extensible Markup Language) : HTML의 단점을 보완한 인터넷 언어로 특수한 목적을 갖는 마크업 언어 
        - 특징 : XML은 전송된 데이터 구조를 동일한 형태로 정의함
        ```html
        <?xml version="1.0" encoding="UTF-8"?>
            <shop city="인천" type="전자상가">
                <smart device>
                    <name>갤럭시S22</name>
                    <cost>1,000,000</cost>
                </smart device>
            </shop>
        ```
    3. AJAX (Asynchronous Javascript And XML) : js를 사용해 웹서버와 클라이언트 간 **비동기적으로** XML 데이터를 교환하고 조작하기 위한 웹 기술
        - AJAX *주요 기술*

        | 주요기술 | 설명 |
        |-----|----|
        | XMLHttpRequest | - 웹브라우저와 웹 서버간에 메서드가 데이터를 전송하는 객체 폼의 API <br> - 비동기 통신을 담당하는 자바스크립트 객체 |
        | JavaScript | - 객체 기반의 스크립트 프로그래밍 언어 <br> - 웹브라우저 내에서 주로 사용하며 다른 응용 프로그램의 내장 객체에도 접근할 수 있는 기능 O|
        | XML | HTML의 단점을 보완한 인터넷 언어로 특수한 목적을 갖는 마크업 언어 |
        | DOM | XML 문서를 **트리구조의 형태로 접근할수 있게 해주는** API | 
        | XSLT | - XML문서를 다른 XML문서로 변환하는 데 사용하는 XML 기반 언어 <br> - 탐색을 위해 XPath를 사용 |
        | HTML | 인터텟 웹 문서를 표현하는 표준화된 마크업 언어 |
        | CSS | 스타일 시트 |
    4. REST (Representational State Transfer) 
        - REST *기본 형태* : Resource(자원), Method(처리) , Message
        - REST *특징* 
            | 특징 | 설명 |
            |-----|----|
            | 클라이언트/서버 구조 | 클라이언트와 서버는 독립적으로 구현되어야하고 서로간의 의존성은 축소해야함 |
            | 무 상태성 | API서버는 들어오는 요청만 처리, 서버에 불필요한 정보 관리 X → 구현이 단순|
            | 일관된 인터페이스 | HTTP 표준만 따르면 특정 언어 기술에 종속되지 X, 모든 플랫폼에 사용 O |
            | 캐시 처리 가능 | HTTP가 가진 캐싱 기능 적용 O |
            | 자체 표현 구조 | API메세지만 보고도 API를 이해할 수 있는 구조를 가짐|
### * 인터페이스 *보안 기능 적용*
- 인터페이스 ***보안 취약점***
    1. 데이터 통신 시 데이터 **탈취 위협** : 스니핑을 통해 데이터 전송 내역을 감청해 데이터를 탈취하는 위협
        > [스니핑?] 공격 대상에게 직접 공격하지 않도 데이터만 몰래 들여다보는 수동적 공격기법
    2. 데이터 통신 시 데이터 **위변조 위협** : 데이터에 대한 삽입, 삭제, 변조 공격을 통한 시스템 위협
- 인터페이스 ***보안 구현 방안***
    1. 시큐어 코딩 가이드 적용 : (입보시 에코캡아)
        | 적용 대상 | 보안 약점 | 대응 방안 |
        |-----|-----|----|
        | 입력데이터 검증 및 표현 | 프로그램 입력값에 대한 검증누락, 부적절한 검증, 잘못된 형식 지정 | 사용자,프로그램 입력 데이터에 대한 유효성 검증 체계 수립 및 실패시 처리 기능 설계&구현|
        | 보안 기능 | 보안기능의 부적절한 구현 | 인증&접근 통제, 권한관리, 비밀번호등의 정책이 적절하게 반영되도록 설계 및 구현 |
        | 시간 및 상태| 하나 이상의 프로세스가 동작하는 환경에서 시간 및 상태의 부적절한 관리 | 공유자원의 접근 직렬화, 병렬 실행 가능한 프레임워크 사용|
        | 에러 처리 | 에러 미처리, 불충분한 처리등으로 에러 메세지에 중요 정보가 포함 | 정보 유출 등 보안 약점 발생하지 않도록 설계 및 구현 |
        | 코드 오류 | 개발자가 범할 수 있는 코딩오류로 인해 유발 | 코딩 규칙 도출 후 검증 가능한 스크립트 구성돠 경고 순위의 최상향 조정 후 경고 메세지 코드 제거|
        | 캡슐화 | 인가되지 않은 사용자에게 데이터 노출 | 디버거 코드 제거와 필수정보 외의 클래스내 private 접근자 지정 |
        | API 오용 | 의도되지 않은 방법으로 API 사용 or 보안에 취약한 API 사용 | 개발 언어별 취약 API 확보 및 취약 API 검출 프로그램 사용 |
    2. 데이터베이스 보안 적용 
        - 데이터 베이스 ***암호화 알고리즘***
        
            | 구분 | 설명 |
            |-----|----|
            | **대칭 키** 암호화 알고리즘 | 암복호화에 **같은 암호 키** 사용 <br> ``` ex) ARIA 128/192/256, SEED ``` |
            | **비대칭 키** 암호화 알고리즘 | **공개키** 오픈, **비밀키**는 소유자만 알 수 있음 <br> ``` ex) RSA, ECC, ECDSA ```|
            | **해시** 암호화 알고리즘 | 해시값으로 원래 입력값을 찾아 낼 수 없는 **일방향성**의 특성을 가진 알고리즘 <br> ``` ex) SHA-256/384/512, HAS-160 ```|
        - 데이터 베이스 ***암호화 기법*** : (애플티하)
            | 구분 | 설명 |
            |-----|---|
            | **API** 방식 | 애플리케이션 레벨에서 암호모듈(API)을 적용하는 애플리케이션 수정 방식 |
            | **Plug-in** 방식 | 암복호화 모듈이 DB 서버에 설치된 방식 |
            | **TDE** 방식 | DB서버의 DBMS 커널이 자체적으로 암복호화 기능을 수행하는 방식 (내장되어 있는 암호화 기능) |
            | **Hybrid** 방식 |  API + Piug-in |
        - 중요 인터페이스 테이터의 ***암호화 전송***
            | 보안 기술 | 설명 |
            |-----|----|
            | IPSec <br> (IP Security) | - IP 계층(3계층)에서 무결성과 인증을 보장하는 인증 헤더와 기밀성을 보장하는 암호화를 이용해 양 종단 간 구간에 보안 서비스를 제공하는 터널링 프로토콜 <br> - 동작모드는 전송 , 터널모드가 있음 |
            | SSL/TLS | - 전송(4)계층과 응용(7)계층 사이에서 클라이언트와 서버간의 웹 데이터 암호화, 무결성을 보장하는 보안 프로토콜 <br> - 인증모드는 익명, 서버인증, 클라이언트-서버인증 모드가 있음 <br> - IPSec과 달리 **클라이언트와 서버간에 상호인증, 암호방식에 대해 협상을 거침** |
            | S-HTTP <br> (Secure Hypertext Transfer protocol) | - S-HTTP는 클라이언트와 서버간에 전송되는 **모든 메세지를 각각 암호화해 전송** |

## 5-3. 인터페이스 구현 검증
### * 인터페이스 ***구현 검증 도구***
- 종류 : (엑스피 엔셀웨)
    1. xUnit : JAVA, C++, .Net 등 **다양한 언어를 지원**하는 단위테스트 프레임워크
    2. STAF : 서비스 호출, 컴포넌트 재사용 등 **다양한 환경을 지원**하는 테스트 프레임워크 
    3. FitNesse : 웹 기반 **테스트 케이스 설계,실행,결과 확인 등을 지원**하는 테스트 프레임워크
    4. NATF : FitNesse (협업기능) + STAF(재사용,확장성)을 통합합 **NHN(Naver)의 테스트 자동화 프레임워크** 
    5. Selenium : **다양한 브라우저 및 개발언어를 지원**하는 웹 어플리케이션 테스트 프레임 워크 
    6. watir : **Ruby 가번** 웹 어플리케이션 테스트 프레임워크
### * 인터페이스 ***감시 도구***
    
1. 스카우터 (SCOUTER) : 애플리케이션에 대한 모니터링 및 **DB Agent를 통해** 오픈 소스 DB 모니터링 기능, 인터페이스 감시 기능을 제공
2. 제니퍼 (Jennifer) : 애플리케이션의 개발부터 테스트, 오픈, 운영, 안정화까지 **전 생애주기 단계 동안** 성늘을 모니터링하고 분석해주는 **ARM 소프트웨어**
    > [ARM; Application Performance Management] 어플리케이션 모니터링 툴