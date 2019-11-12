# DB-Note
DB의 효율성을 위한 방법, 쿼리 설계 및 Tip 정리  



# 효율적인 퀄리를 사용할땐!!!


## 색인기능, INDEX란

- 전체 테이블을 뒤져서 찾는 FULL-TABLE SCAN (별로)

- 그래서 `인덱스를 이용한 색인을 쓰자!`

- 특정 순서로 정렬된 테이블 내용에 대한 포인터를 포함하는 데이터 구조

- 페이지의 행과 유사하다.

- 쿼리의 WHERE 절, JOIN 절 또는 ORDER BY 절에 사용 된 열에 인덱스가 있으면 쿼리 성능 향상 


-인덱스는 열에 따라 테이블의 행을 정렬하여 읽기 쿼리 속도를 높이는 방법

-테이블의 모든 행을 검사하는 대신 서버는 인덱스에 대해 2 진 검색을 수행



### 장점 

행 수가 많을수록 성능 업업!! 
 

### 단점

쓰기 속도와 데이터베이스 크기

색인을 저장하면 공간이 필요합니다

INSERT, UPDATE => 색인 업데이트

그래도 `장점이 더 크다!!`



# 쿼리 설계 및 Tip


## WHERE 1=1을 쓰자

- `동적 쿼리를 사용할 때 조건문을 줄이기 위해서이다.`

- SELECT * FROM user;

- SELECT * FROM user WHERE 1=1;

- 둘의 결과는 동일하다.

- 하지만 동적 쿼리를 사용해 조건절에 다른 조건을 줘야 하는 경우는 후자가 더 좋다.


### &lt, &gt 

&lt 부등호(<)
&gt 부등호(>)
