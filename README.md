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



## 프로시져 

저장 프로시저 또는 stored procedure는 일련의 쿼리를 

마치 하나의 함수처럼 실행하기 위한 쿼리의 집합니다.

이를 활용하면 쿼리전에 제약조건을 걸거나, 

쿼리 후에 함수를 실행하거나 하는 등의 행위가 가능하다.



## 반정규화를 사용해서 성능을 잡자!

정규화가 심해지면 조인이 발생하면서 성능이 저하

릴레이션, 속성 역정규화 (릴레이션 = 테이블)

단 모든 정규화가 끝나고 고려하기 

데이터의 정합성에는 영향주지 않기 

# 주의 사항 

##### SELECT문으로 조회하는 것도 서버를 쓰기 때문에 돈이다.

##### 트래픽 과부하가 걸리게 된다. 


# 쿼리 설계 및 Tip

## Group By는 확실히 

## WHERE 1=1을 쓰자

- `동적 쿼리를 사용할 때 조건문을 줄이기 위해서이다.`

- SELECT * FROM user;

- SELECT * FROM user WHERE 1=1;

- 둘의 결과는 동일하다.

- 하지만 동적 쿼리를 사용해 조건절에 다른 조건을 줘야 하는 경우는 후자가 더 좋다.


### &lt, &gt 

&lt 부등호(<)
&gt 부등호(>)

### 코드값 비교시에는 

ncode = '001' 보다는 ncode <> '001' 을 쓰자! 


### union과 select 여러번

비교

### order by의 속도를 줄이자

자체적 정렬코드가 빠르다 
편리함보단 효율성

### ESCAPE 옵션 
jig_igigi@naver.com 찾고 싶으면

WHERE EMAIL LIKE '_ _ _ _%'; (x)

WHERE EMAIL LIKE '_ _ _#_%' ESCAPE '#'(o) 


### UPDATE 문을 잘쓰자!

update 문으로 쿼리를 줄일 수 있다.
효율적으로 쓰자!

###  WHEREin 

조건에서 어떤거 포함이다.
Where A = '1 ' AND ~~~ (x)
A in ('1', '2')

### LEFT OUTER JOIN

SELECT 
* 
FROM 테이블1 A
LEFT OUTER JOIN 테이블2 B
ON A.no = B. no

### Cross Join

크로스 조인은 두개의 테이블에서 CATESIAN PRODUCT 연산의 결과를 출력

CATESIAN PRODUCT란 행과 행의 나열하는 결과를 보여주는 것

A 테이블      B 테이블
   1             4
   2             5
   3             6 
   
   크로스 조인시 
   
   14 
   15 
   16 
   24
   25
   26
   34 
   35 
   36


## DECODE 

sql 일종의 if - else if - else 문

## 2번째 글짜를 검색조건 

name LIKE '_T%'

## 문자열 함수

UPPER : 대문자
LOWER : 소문자
INITCAP : 처음만 대문자

## STUFF

해당 범위의 문자를 대체할때 좋다.




### interect

공통으로 나오는 하나의 부분을 검색하기위해서 

SELECT igno FROM DEPT
interect
SELECT igno FROM EMP

### 쿼리는 길다고 나쁜게 아니다

### NOT IN, IN 을 잘쓰자 

A.idx NOT IN ('007','008','009','010')

### WHERE절에 CASE문 
CASE WHEN  A.pidx IN ('002','003') THEN '사람'  
				     ELSE '동물'
				END group,

### 백업하는법 
CREATE TO 로 변경테이블을 기존테이블 _ 날짜로 생성

INSERT INTO 변경테이블 SELECT * FROM 기존테이블

## ROUND, TRUNC

조건이 양수이면 소숫점, 음수이면 정수자리 
반올림
버림

## MongoDB

몽고디비의 키값은 존재하는가? X

아이디(_id)는 기본키이며 ObectId라고 한다. ObjectId는 고유한 값으로 자동으로 생성되며 크기는 12바이트이다.

자체적인 일련번호를 달 수 도 있다.

# 쿼리연습 

### WHERE 서브쿼리 

##### SMITH 사원과 같은 부서(deptno)에 근무하는 사원의 empno, ename, deptno를 조회

```
SELECT empno, ename, deptno FROM EMP 
WHERE deptno IN (SELECT deptno
                          FROM EMP 
                          WHERE ename = 'SMITH');
```


##### JONES 사원보다 급여(sal)가 많은 사원의 empno, ename, sal을 조회하는 
```
SELECT empno, ename, sal FROM EMP 
WHERE sal < (SELECT sal
                          FROM EMP 
                          WHERE ename = 'JONES');
```

##### 	emp 테이블에서 empno, ename, hiredate, hiredate의 요일을 조회

```
SELECT empno,ename, hiredate TO_CHAR((TO_DATE(hiredate)),'day')
FROM emp
```


#####  emp 테이블에서 2월에 입사(hiredate)한 사원의 ename, hiredate를 조회.
```
SELECT ename,hiredate FROM emp
    WHERE TO_CHAR(hiredate, ‘MM’) = 02
```

##### 부분 업데이트 

```
UPDATE St_HMedia
SET ChargeQtyYN ='Y', ModifyUser ='jig', ModifyDate =GetDate()
WHERE HReaderNo in (Select HReaderNo From St_ChargeY)
```

