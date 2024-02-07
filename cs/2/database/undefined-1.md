# 고청천

## 정규화

### 정규화를 하는 이유

정규화를 통해 우리는 테이블 간 중복된 데이터를 방지하고, 이를 통해 무결성을 유지할 수있습니다.&#x20;

### 정규화 1, 2, 3 형

`제 1 정규화` 는 테이블의 컬럼이 원자 값을 갖게 하는 것입니다.&#x20;

<figure><img src="../../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

즉, (영화 음악) 으로 되어 있는 것을 분리하는 것입니다.

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

`제 2 정규화` 는 완전 함수 종속을 만족하도록 테이블을 분해하는 것입니다.

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>



`제 3 정규화` 는 이행적 종속을 없애는 것입니다. A -> B, B -> C 가 성립될 때 A -> C 가 성립되는 것입니다.

<figure><img src="../../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

### 복합키의 장단점



## DB 종류

### MySQL 사용 이유

가격과 안정성 그리고 인지도를 기반으로 선택하였습니다. 커뮤니티 에디션의 경우 무료로 사용할 수 있고, 안정성을 기반으로 높은 인지도를 달성하고 있는 RDB 여서 선택하였습니다.

<figure><img src="../../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

### 사용 버전

8.x 버전을 사용한 이유는 지원 종료와 관련 되있습니다

[https://aws.amazon.com/ko/about-aws/whats-new/2023/11/amazon-rds-mysql-new-minor-version-5-7-44/](https://aws.amazon.com/ko/about-aws/whats-new/2023/11/amazon-rds-mysql-new-minor-version-5-7-44/)

### 버전 별 특징

### NOSQL 이 대중화 된 이유

### NOSQL 이란?

NoSQL 은 비관계형 데이터 베이스를 말합니다. 장점으로는 구조가 자유롭고, 수평적 확장이 용이하다는 점이 있습니다.&#x20;

NOSQL 장점

*   구조가 자유롭다

    > 관계형 데이터베이스의 전형적인 테이블 구조 대신, NoSQL 데이터베이스는 JSON 문서와 같은 하나의 데이터 구조 안에 데이터를 보관합니다. 이러한 비관계형 데이터베이스 설계는 스키마가 필요하지 않으므로 일반적으로 비정형인 대규모 데이터 세트를 관리할 수 있는 `신속한 확장성` 을 제공합니다.
*   DB의 수평적 확장이 용이하다

    > SQL 데이터베이스는 수직적으로만 확장할 수 있고 수평적으로는 확장할 수 없기 때문에 하드웨어를 통해서만 메모리를 추가할 수 있습니다. 그 결과, 수직적 확장이 궁극적으로 회사의 데이터 저장 및 가져오기 능력을 제한하게 됩니다.
    >
    > 이에 비해, NoSQL 데이터베이스는 비관계형이므로 테이블을 연결할 필요가 없습니다.내장된 샤딩 및 고가용성 기능이 `수평적 확장을 용이` 하게 합니다.

## SQL

### DML

데이터 조작어로 데이터 조회하거나 변형하는 명령어입니다.

Selcect, Insert, Update, delete 가 있습니다

### DDL

데이터 정의어로 데이터 구조를 정의하는 데 사용 되는 명령어입니다.

Create, Drop 이 있다

### DCL

데이터 제어어로 데이터 베이스에 접근하고 객체들을 사용하도록 권한을 주고 회수하는 명령어이다

GRANT, REVOKE 명령어가 있습니다.
