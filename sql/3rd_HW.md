# 3주차(집계, 윈도우함수, 데이터 병합 방법 이해)

## 2-8. 새로운 집계 함수 소개(GROUP BY ALL, 2024-02-26에 나온 함수)
GROUP BY ALL? 열 이름을 명시하지 않아도 추론을 통해 그룹화할 칼럼을 자동으로 선택해서 그룹화해줌\
→ 모든 행을 그룹화하는데 사용되는 기능

## 3-2. SQL 쿼리 작성하는 흐름
1. 지표 고민(+문제 정의) : 어떤 문제를 해결하기 위해 데이터가 필요한가?
2. 지표 구체화 : 추상적이지 않고 구체적인 지표 명시(분자, 분모 표시. 이름을 구체적으로)
ex) A칼럼은 어떤 밸류만 써야한다 등
3. 지표 탐색 : 유사한 문제를 해결한 케이스가 있나 확인 → 존재한다면 해당 쿼리 리뷰, 없다면 구글이나 GPT 사용해봐도..
4. 쿼리 작성 : 데이터가 있는 테이블 찾기
→ 1개 → 바로 활용 / 2개 이상 → 연결 방법 고민(JOIN)
5. 데이터 정합성 확인 : 예상한 결과와 동일한지 확인\
**데이터 정합성**? 데이터의 정확성, 일관성, 그리고 신뢰성을 의미
6. 쿼리 가독성 : 나중을 위해 깔끔하게 쿼리 작성
7. 쿼리 저장 : 쿼리는 재사용되므로 문서로 저장

## 3-3. 쿼리 작성 템플릿과 생산성 도구
✅ 쿼리 작성 템플릿
```
# 쿼리를 작성하는 목표, 확인할 지표 :
# 쿼리 계산 방법 :
# 데이터의 기간 :
# 사용할 테이블 :
# Join KEY :
# 데이터 특징 :
```

## 3-4. 오류를 디버깅하는 방법
오류 메세지 \
→ 현재 작성한 방식으로 하면 답을 얻을 수 없어요(길잡이 역할)\
→ 이 부분에 문제가 되어요(문제 진단)

Syntax Error(문법 오류)\
빨간 밑줄 바로 앞뒤에 문제가 있을 확률이 큼

📍case1) **SELECT list must not be empty at [10:1]**
```
SELECT  # 이 부분이 비어있기에 생기는 오류
  FROM
```
📍case2) **Number of arguments does not match for aggregate function COUNT**
```
SELECT
 COUNT(id, kor_name) # count 괄호 안에는 하나의 인자만 들어감!
  FROM basic.pokemon
```
📍case3) **SELECT list expression references column type1 a which is neither grouped nor aggregated**
SELECT 목록 식은 다음에서 그룹화되거나 집계되지 않은 열을 참조한다.
```
SELECT
 type1,
 count(id) as cnt
  FROM basic.pokemon
 GROUP BY type1 # 추가된 쿼리. 집계 함수를 사용할 때, 집계 함수가 적용되지 않은 다른 열은 반드시 GROUP BY절에 포함되어야 한다.
```
📍case4) **Syntax error : Expected end of input but got keyword SELECT at [8:1]**
```
SELECT
 type1,
count(id) as cnt
  FROM basic.pokemon
 GROUP BY type1 # 하나의 쿼리엔 SELECT가 1개만 있어야 함. 쿼리가 끝나는 부분에 ;을 붙이거나 실행할 부분만 드래그 앤 드랍해서 실행
SELECT *
  FROM basic.trainer
  ```

## 💥📷강의 수강 인증샷📷💥
![image.png]()

## 🏁 문제풀이 - 조건에 맞는 사용자와 총 거래금액 조회하기
Q. USED_GOODS_BOARD와 USED_GOODS_USER 테이블에서 완료된 중고 거래의 총금액이 70만 원 이상인 사람의 회원 ID, 닉네임, 총거래금액을 조회하는 SQL문을 작성해주세요. 결과는 총거래금액을 기준으로 오름차순 정렬해주세요.

```
# 쿼리를 작성하는 목표, 확인할 지표 : 완료된 중고 거래의 총금액이 70만 원 이상인 사람의 1)회원 ID, 2)닉네임, 3)총거래금액을 조회하는 SQL문을 작성
# 쿼리 계산 방법 :
# 데이터의 기간 :
# 사용할 테이블 : USED_GOODS_BOARD(중고 거래 게시판 정보), USED_GOODS_USER(중고 거래 게시판 사용자 정보)
# Join KEY : USED_GOODS_USER.USER_ID & USED_GOODS_BOARD.WRITER_ID
# 데이터 특징 : STREET_ADDRESS2 -> Nullable

# 정답 코드
SELECT USER_ID, 
       NICKNAME, 
       sum(PRICE) as TOTAL_SALES
  FROM USED_GOODS_BOARD B
  JOIN USED_GOODS_USER U
    on U.USER_ID = B.WRITER_ID
 WHERE STATUS = 'DONE'
 GROUP BY USER_ID, NICKNAME
 HAVING TOTAL_SALES >= 700000
 ORDER BY TOTAL_SALES ASC;
 
 # 오답 코드
 SELECT USER_ID, 
       NICKNAME, 
       sum(PRICE) as TOTAL_SALES
  FROM USED_GOODS_BOARD B
  JOIN USED_GOODS_USER U
    on U.USER_ID = B.WRITER_ID
 GROUP BY USER_ID, NICKNAME
 HAVING TOTAL_SALES >= 700000
 ORDER BY TOTAL_SALES ASC;
 
 SELECT USER_ID, 
       NICKNAME, 
       sum(PRICE) as TOTAL_SALES
  FROM USED_GOODS_BOARD B
  JOIN USED_GOODS_USER U
    on U.USER_ID = B.WRITER_ID
 GROUP BY USER_ID # 그룹 바이에 집계함수 씌우지 않은 열 전부 적어주기(강의에서 나왔던 내용😅)
 HAVING TOTAL_SALES >= 700000
 ORDER BY 3 DESC;
```

## 🏁 문제풀이 - 업그레이드 할 수 없는 아이템 구하기
Q. 더 이상 업그레이드할 수 없는 아이템의 아이템 ID(ITEM_ID), 아이템 명(ITEM_NAME), 아이템의 희귀도(RARITY)를 출력하는 SQL 문을 작성해 주세요. 이때 결과는 아이템 ID를 기준으로 내림차순 정렬해 주세요.

```
# 더 이상 업그레이드할 수 없는 아이템의 아이템 ID(ITEM_ID), 아이템 명(ITEM_NAME), 아이템의 희귀도(RARITY)를 출력 
# I.ITEM_ID, ITEM_NAME, RARITY 
# 결과는 아이템 ID를 기준으로 내림차순 정렬해 주세요. ORDER BY ITEM_ID DESC

SELECT I.ITEM_ID, ITEM_NAME, RARITY 
  FROM ITEM_TREE T
  RIGHT JOIN ITEM_INFO I
    ON T.PARENT_ITEM_ID = I.ITEM_ID
    WHERE PARENT_ITEM_ID IS NULL # 문제 상단에 IS NULL 관련 문제라고 적혀있던 것에서 힌트를 얻음
    ORDER BY ITEM_ID DESC
;

# OUTER JOIN이 안되더라,, MYSQL에서 FULL OUTER JOIN이 기본적으로 지원되지 않기 때문
```

## 🏁 문제풀이 - 조건에 맞는 개발자 찾기

Q. DEVELOPERS 테이블에서 Python이나 C# 스킬을 가진 개발자의 정보를 조회하려 합니다. 조건에 맞는 개발자의 ID, 이메일, 이름, 성을 조회하는 SQL 문을 작성해 주세요.

결과는 ID를 기준으로 오름차순 정렬해 주세요.

```
select DISTINCT(id), email, first_name, last_name
  from developers d
  join skillcodes s
    on s.code & d.skill_code=s.code # 비트 연산자(&)를 사용하여 skill_code가 해당 기술 코드와 일치하는지를 검사
 where s.name in ('C#','Python')
 order by id;
```