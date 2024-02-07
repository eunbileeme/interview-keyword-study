# 이은비

> ※ 기준은MySQL입니다.

## :floppy\_disk: 조인

### 조인이란?

> 데이터베이스에서 두 개 이상의 테이블을 연결하여 하나의 열로 표현한 개념으로써, 테이블 또는 결과 셋으로 나타납니다.

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

1. INNER JOIN

> 교집합, 동일한 값이 있는 행만 반환합니다.

```
SELECT *
FROM POST post
(INNER) JOIN POST_IMAGES post_images ON post.post_id = post_images.post_id;
```

2. OUTER JOIN

* LEFT OUTER JOIN

> &#x20;(조건에 따른) 첫  번째 테이블의 모든 행과 그에 따른 두 번째 테이블의 행의 값을 반환합니다.

```
SELECT *
FROM POST post
(INNER) JOIN POST_IMAGES post_images ON post.post_id = post_images.post_id;
LEFT JOIN POST_LIKE post_like ON post.post_id = post_like.post_id AND post_like.user_id = #{userId}
```

* RIGHT OUTER JOIN

> (조건에 따른) 두 번째 테이블의 모든 행과 그에 따른 첫 번째 테이블의 행 값을 반환합니다.

```
SELECT *
FROM POST post
(INNER) JOIN POST_IMAGES post_images ON post.post_id = post_images.post_id;
LEFT JOIN POST_LIKE post_like ON post.post_id = post_like.post_id AND post_like.user_id = #{userId}
RIGHT JOIN FOLLOW follow ON post.user_id = follow.to_user_id AND follow.from_user_id = #{userId}
```

> [https://inpa.tistory.com/entry/MYSQL-%F0%9F%93%9A-JOIN-%EC%A1%B0%EC%9D%B8-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EA%B8%B0%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC](https://inpa.tistory.com/entry/MYSQL-%F0%9F%93%9A-JOIN-%EC%A1%B0%EC%9D%B8-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EA%B8%B0%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC)

### 크로스 조인은 왜 성능 상 좋지 않을까?

> 크로스 조인은 하나의 입력 테이블에 m개의 행이 있고, 다른 하나의 입력 테이블에 n개의 행이 있을 경우 Cartesian Product(카티션 곱)에 의해 m \* n의 행이 만들어집니다. 따라서 상당히 많은 양의 데이터가 만들어질 수 있으므로 사용에 주의가 필요합니다.

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

> [https://toe10.tistory.com/111](https://toe10.tistory.com/111)

### ON vs WHERE

> ON 절은 JOIN(조인)을 하기 전 필터링을 하는 반면, WHEERE 절은 JOIN을 수행한 후 필터링을 진행합니다.

* ON 절은 조인을 수행할 경우, 조인 조건을 명시하기 위해 사용하는 예약어입니다.
  * 반면, WHERE절은 일반적으로 데이터를 조회할 경우, 필터링을 위해 명시하는 조건 예약어입니다.

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

> [https://codingdog.tistory.com/entry/mysql-on%EC%A0%88-vs-where%EC%A0%88-%EC%96%B8%EC%A0%9C-%EC%96%B4%EB%96%BB%EA%B2%8C-%ED%95%84%ED%84%B0%EB%A7%81-%EB%90%98%EB%8A%94%EA%B0%80](https://codingdog.tistory.com/entry/mysql-on%EC%A0%88-vs-where%EC%A0%88-%EC%96%B8%EC%A0%9C-%EC%96%B4%EB%96%BB%EA%B2%8C-%ED%95%84%ED%84%B0%EB%A7%81-%EB%90%98%EB%8A%94%EA%B0%80)

### 서브 쿼리는 성능 저하가 왜 발생할까?

> 서브쿼리는 메인쿼리에 의해 호출되며, 그 결과는 메인쿼리에서 일시적으로 사용이 됩니다. 즉, 데이터베이스에 별도의 테이블이나 뷰로서 영구적인 데이터로 저장되지 않습니다. 따라서 서브쿼리를 실행할 때마다 실행 비용이 발생하며 내용이 복잡할 수록 비용은 증가하게 됩니다.

* 또한, 데이터 I/O 비용이 발생합니다.
  * 즉, 연산 결과의 크기에 따라 데이터 저장 비용이 달라질 수 있습니다.
* 서브쿼리로 만들어지는 데이터는 테이블과 다르게 데이터를 설명하는 메타 정보를 가지고 있지 않습니다.
  * 옵티마이저(SQL 실행 계획을 수립 및 관리하는 시스템)가 쿼리를 해석하기 위해 필요한 정보를 서브쿼리에서 읽어낼 수 없습니다.



* 서브 쿼리란 쿼리 안에 존재하는 쿼리로서, 소괄호 안에 쿼리를 작성하는 방식으로 표현합니다.
  * 서브 쿼리의 활용 예시는 다음과 같습니다.

1. 테이블 서브쿼리

> 테이블이나 뷰의 이름, 테이블을 반환하는 저장 프로시저나 함수 이름을 사용할 수 있는 곳에 활용

* ex. 조인할 때, FROM 절에 사용하는 서브쿼리

2. 단일 컬럼 테이블 서브쿼리

> 테이블 서브쿼리나 값의 목록을 IN 조건으로 비교하는 곳에 활용

* ex. WHERE 절에서 IN (혹은 NOT IN) 구문을 활용한 서브쿼리

3. 스칼라 서브쿼리

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

> [https://medium.com/@connect2yh/%EC%84%9C%EB%B8%8C%EC%BF%BC%EB%A6%AC-%EC%8D%A8-%EB%A7%90%EC%95%84-6301d250e98a](https://medium.com/@connect2yh/%EC%84%9C%EB%B8%8C%EC%BF%BC%EB%A6%AC-%EC%8D%A8-%EB%A7%90%EC%95%84-6301d250e98a)

### (JPA) Fetch Join이 SQL 수준(SQL에서 어떤 식으로 쿼리가 나가는지)에서 어떻게 이뤄질까?

> 처음 조회부터 조인시켜 데이터를 가져올 수 있게 하기 위해, 연관된 엔티티나 컬렉션을 한 번의 SQL을 통해 함께 조회하는 방향으로 이뤄집니다.

* Fetch Join(페치 조인)은 주로 `N+1` 로 인해 **처음 조회부터 조언을 시켜 데이터를 가져올 수 있게 하기 위해** 등장하였습니다.
  * `N+1` 문제란 연관 관계가 설정된 엔티티를 조회할 경우, 조회된 데이터 갯수만큼 연관 관계의 조회 쿼리가 추가로 발생하여 데이터를 읽어오는 현상입니다.
* 또한, 종류가 아니며 JPQL(Java Persistence Query Language)에서 성능 최적화를 위해 사용하는 쿼리입니다.

```
SELECT m.*, t.* FROM Member m INNER JOIN Team t ON m.team_id=t.id
```

> [https://woo-chang.tistory.com/38](https://woo-chang.tistory.com/38)

> [https://programmer93.tistory.com/83](https://programmer93.tistory.com/83)

> [https://woo-chang.tistory.com/38](https://woo-chang.tistory.com/38)

## 파일 시스템

### 데이터베이스보다 파일 시스템에서 데이터를 읽어올 경우, 왜 느릴까?

> 데이터베이스를 이용하여 데이터를 관리할 경우, 다음과 같은 일련의 이유들로 더 빠르게 데이터를 가져올 수 있습니다.

1. 데이터베이스의 업데이트 최적화

* 데이터베이스는 트랜잭션 업데이트를 제공하므로 시스템이나 애플리케이션 충돌에 관계없이, 완전한 트랜잭션의 원자적 적용이 가능합니다.
  * 대형 BLOB(Binary Large Object,  큰 이진 객체)를 완전히 덮어쓰는 대신, 일부만 수정하여 업데이트를 최적화할 수 있습니다.

2. 데이터베이스의 동기화
3. (많은 데이터를 저장할 경우) 데이터베이스의 인덱스의 존재 유무
4. 64KB보다 작은 이미지와 인라인 BLOB를 지원하는 스토리지 엔진

* 64kb보다 작은 이미지 파일을 데이터베이스에서 다룰 경우, 데이터베이스의 저장 엔진이 별도의 위치에 저장하고 참조하는 대신, BLOB 데이터가 데이터베이스 행 내에 바로 저장할 수 있습니다.
  * 추가로, InnoDB는 TEXT 또는 BLOB 데이터의 경우 최대 767 byte만 행 내에 직접 저장하고 별도의 위치에 저장하는 반면, MyISAM은 이러한 제한 없이 BLOB 데이터를 행 내에 직접 저장할 수 있습니다.
* 따라서 데이터를 행 내에 직접 저장할 경우, 참조 지역성이 높아져 데이터 접근 시간이 감소합니다.

> [https://medium.com/@ahmedm.saeedelgohary/performance-comparison-of-relational-databases-and-filesystems-for-write-intensive-web-applications-368d1973e205](https://medium.com/@ahmedm.saeedelgohary/performance-comparison-of-relational-databases-and-filesystems-for-write-intensive-web-applications-368d1973e205)

> [https://www.linkedin.com/pulse/which-better-saving-files-database-file-system-abuthahir-sulaiman/](https://www.linkedin.com/pulse/which-better-saving-files-database-file-system-abuthahir-sulaiman/)

> [https://medium.com/@vaibhav0109/should-i-use-db-to-store-file-410ee22268c7](https://medium.com/@vaibhav0109/should-i-use-db-to-store-file-410ee22268c7)

## DBMS

### 데이터 독립성

### 논리적 데이터 독립성

> 하위 단계의 데이터베이스의 논리적 구조(ex. 스키마, 테이블 구조)가 변경되더라도, 이러한 변경이 상위 단계의  응용 프로그램의 비즈니스 로직에 영향을 미치지 않는 속성을 말합니다.&#x20;

* DBA(데이터베이스 관리자)가 특정 테이블에 새로운 열을 추가하거나 데이터 형식을 변경해도, 응용 프로그램은 이러한 변경에 영향받지 않고 기존의 방식대로 데이터를 처리할 수 있어야 합니다.

### 물리적 데이터 독립성

> 하위 단계의 데이터 구조(ex. 디스크에 저장되는 방식)가변경되더라도 상위 단계(ex. WAS와 같은 응용 프로그램)에 영향을 미치지 않는 속성을 일컫습니다.

* DBA(데이터베이스 관리자)가 성능 최적화를 위해 데이터를 다른 형식의 디스크에 옮기거나 인덱스를 변경해도, 사용자의 쿼리나 응용 프로그램 코드는 변경할 필요가 없습니다.

> [https://needjarvis.tistory.com/287](https://needjarvis.tistory.com/287)

### INSERT 문 DB() 동작 과정

> MySQL - InnoDB를 기준으로 다음과 같습니다.

1. 클라이언트 INSERT문 실행

* 클라이언트(ex. 어플리케이션, MySQL 콘솔)가 INSERT문을 실행하면, 해당 요청이 MySQL 서버로 전송됩니다.

2. SQL 파싱 및 최적화

* MySQL 서버는 SQL문을 파싱하여 문법적으로 올바른지 확인합니다.
  * 이후, 서버는 INSERT문을 실행하기 위해 가장 효율적인 방법을 결정하기 위한 최적화 과정을 거칩니다.

3. 트랜잭션 시작

* 트랜잭션 기반의 저장 엔진인 InnoDB에서 INSERT문이 실행될 때, MySQL은 새로운 트랜잭션을 시작하거나 기존 트랜잭션 내에서 실행됩니다.

4. 데이터 쓰기

* INSER문에 의해 지정된 데이터는 데이터베이스 테이블의 적절한 행에 쓰여집니다.
  * InnoDB는 해당 데이터를 먼저 버퍼 풀에 작성한 후, 실제 디스크에 저장하는 `지연 쓰기` 방식을 통해 진행합니다.

5. 인덱스 갱신

* 삽입된 데이터가 테이블의 인덱스에 영향을 줄 경우, 관련 인덱스가 업데이트 됩니다.

6. 커밋 처리

* 트랜잭션을 커밋하면, 모든 변경 사항이 영구적으로 데이터베이스에 저장됩니다.
  * InnoDB의 경우, 커밋 시점에서 변경 사항이 디스크에 기록되며 트랜잭션 로그에도 해당 정보가 기록됩니다.

7. 레디큐 작업

* 데이터가 안전하게 저장되면, MySQL 서버는 클라이언트에게 작업 완료를 알리고 결과(ex. 행의 수)를 반환합니다.
