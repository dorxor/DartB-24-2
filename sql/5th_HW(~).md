# 5주차(데이터 구조, CONCAT)

## 🏁 문제풀이 - [JOIN] 성분으로 구분한 아이스크림 총 주문량
🌱 상반기 동안 각 아이스크림 성분 타입과 성분 타입에 대한 아이스크림의 총주문량을 총주문량이 작은 순서대로 조회하는 SQL 문을 작성해주세요. 
이때 총주문량을 나타내는 컬럼명은 `TOTAL_ORDER`로 지정해주세요.

```
# 정답 코드
SELECT INGREDIENT_TYPE, SUM(TOTAL_ORDER) AS TOTAL_ORDER
  FROM FIRST_HALF F
  JOIN ICECREAM_INFO I
    ON F.FLAVOR = I.FLAVOR
 GROUP BY INGREDIENT_TYPE
 ORDER BY TOTAL_ORDER;

# 지난 학기에 풀었던 정답 코드
코드를 비교해보면, 좀 더 깔끔해진 걸 볼 수 있음(뿌듯😊)
SELECT B.INGREDIENT_TYPE, SUM(A.TOTAL_ORDER) AS TOTAL_ORDER
  FROM FIRST_HALF AS A
  JOIN ICECREAM_INFO AS B
    ON A.FLAVOR = B.FLAVOR
 GROUP BY B.INGREDIENT_TYPE
 ORDER BY SUM(A.TOTAL_ORDER) ASC;
 ```

![alt text](image.png)


## 6-2. WITH 구문🎬
SQL 쿼리를 작성하다 생기는 일 => 반복이 많아짐 => 점점 복잡해짐 (가독성 하락)\
만약 아래 쿼리가 다른 곳에서도 필요하면 복사 붙여넣기해야 함

```
# 기존
SELECT COL, COL2
  FROM (
    SELECT COL1, COL2, COL3 ...
      FROM TABLE
  )

# WITH 문 사용
WITH TEMP_TABLE AS (
  SELECT COL1, COL2, COL3 ...
    FROM TABLE
), TABLE_NAME AS (
  SELECT COL
    FROM TABLE
) 

SELECT COL1, COL2
  FROM TEMP_TABLE

# SELECT 구문에 이름을 정해주는 것과 유사. 쿼리 내에서 반복적으로 사용할 수 있음. 여러 테이블 가능!
```


## 🏁 문제풀이 - [MAX] 즐겨찾기가 가장 많은 식당 정보 출력하기
🌱 `REST_INFO` 테이블에서 음식종류별로 즐겨찾기수가 가장 많은 식당의 음식 종류, ID, 식당 이름, 즐겨찾기수를 조회하는 SQL문을 작성해주세요.
이때 결과는 음식 종류를 기준으로 내림차순 정렬해주세요.

```
# 정답 코드
  WITH M AS (
SELECT FOOD_TYPE, REST_ID, REST_NAME, MAX(FAVORITES) AS FAVORITES
  FROM REST_INFO
 GROUP BY FOOD_TYPE)
 
SELECT R.FOOD_TYPE, R.REST_ID, R.REST_NAME, R.FAVORITES
  FROM REST_INFO R
  JOIN M ON R.FOOD_TYPE = M.FOOD_TYPE AND R.FAVORITES = M.FAVORITES 
  # 음식 종류, 즐겨찾기수 기준으로 찾아야 정답이 되나.. 결과는 다 똑같은데..
 ORDER BY R.FOOD_TYPE DESC;


# 정답인 것 같지만 아닌 코드
  WITH M AS (
SELECT FOOD_TYPE, REST_ID, REST_NAME, MAX(FAVORITES)
  FROM REST_INFO
 GROUP BY FOOD_TYPE)
 
SELECT R.FOOD_TYPE, R.REST_ID, R.REST_NAME, R.FAVORITES
  FROM REST_INFO R
  JOIN M ON R.FOOD_TYPE = M.FOOD_TYPE AND R.REST_ID = M.REST_ID
 ORDER BY R.FOOD_TYPE DESC;
---
SELECT DISTINCT(FOOD_TYPE) AS FOOD_TYPE, REST_ID, REST_NAME, MAX(FAVORITES) AS FAVORITES
  FROM REST_INFO
 GROUP BY FOOD_TYPE
 ORDER BY 1 DESC;
 ```
 ![alt text](image-2.png)


## 🏁 문제풀이 - [WINDOW] 조건에 맞는 사원 정보 조회하기
🌱 이 서비스에서는 공간을 둘 이상 등록한 사람을 "헤비 유저"라고 부릅니다. 헤비 유저가 등록한 공간의 정보를 아이디 순으로 조회하는 SQL문을 작성해주세요.

```
# 정답 코드
WITH A AS (SELECT HOST_ID
  FROM PLACES
 GROUP BY HOST_ID
 HAVING COUNT(ID) >=2)

SELECT ID, NAME, P.HOST_ID
  FROM PLACES P
 INNER JOIN A ON A.HOST_ID = P.HOST_ID
 ORDER BY 1;
# 동영상 보고 WITH문 사용!

# 지난 학기에 풀었던 정답 코드
SELECT P.ID, NAME, P.HOST_ID
  FROM PLACES P
  JOIN (SELECT HOST_ID
  FROM PLACES
 GROUP BY HOST_ID
 HAVING COUNT(ID) >= 2) AS A 
    ON P.HOST_ID = A.HOST_ID
 ORDER BY ID;
 ```
 ![alt text](image-1.png)


## 🧽Clear!🫧