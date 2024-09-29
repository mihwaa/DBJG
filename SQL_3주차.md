데이터 타입
1. 숫자
2. 문자
3. 시간, 날짜
4. 부울

보이는 것과 저장된 것에 차이가 있는 경우 데이터 타입을 변경해야 한다!
SELECT
  DATA(1 AS STRING)
OR int64
오류나는경우 데이터 앞에 safe_DATA > 변환 실패시 NULL 반환

새로운 집계함수

1. GROUP BY ALL이 새로 생겼다!

쿼리 작성 흐름
1. 지표 고민 -> 어떤 문제를 해결해야 하는가?
2. 지표 구체화 -> 어떤 값을 뽑아야 할까?
3. 지표 탐색 -> 유사한 문제.. 있는 거 활용!


쿼리 작성 템플릿
- 쿼리 작성 목표 및 지표
- 쿼리 계산 방법
- 데이터의 기간
- 사용할 테이블
- join key
- 데이터 특징



1번 문제

```
SELECT USER_ID, NICKNAME, SUM(PRICE) as TOTAL_SALES 
FROM USED_GOODS_USER JOIN USED_GOODS_BOARD
ON USER_ID = WRITER_ID
WHERE STATUS = "DONE"
GROUP BY USER_ID
HAVING TOTAL_SALES >= 700000
ORDER BY TOTAL_SALES
```

이거 왜 그룹바이 USER_ID가 들어가는지 계속 이해를 못했다. 
select에 sum같은 집계함수를 활용할 때에는 무얼 기준으로 계산하는지를 정해주지 않으면 전체 집계를 구한다는 걸 알게 되었다.


2번 문제
```
SELECT ITEM_ID, ITEM_NAME, RARITY
FROM ITEM_INFO
WHERE ITEM_ID NOT IN (SELECT PARENT_ITEM_ID 
                     FROM ITEM_TREE
                     WHERE PARENT_ITEM_ID IS NOT NULL)
ORDER BY ITEM_ID DESC
```
IS NOT NULL이 들어가야 하는 이유가 비직관적이다.
NOT IN의 연산경우 파라미터 중 NULL값이 포함되면 정상적 결과를 추출하지 못한다고 한다 (OR이 아닌 AND연산과 유사)
예시로

```
1 NOT IN (0, null)
=> 1 != 0 (true)
=> 1 != null (unknown)

* true AND unknown = unknown
```
1은 0이나 NULL이 아니므로 TRUE를 추출해야 하지만,
실제로는 UNKNOWN이 반환되어 추출되지 않게 된다.



3번 문제
```
SELECT ID, EMAIL, FIRST_NAME, LAST_NAME
FROM DEVELOPERS
WHERE SKILL_CODE & (SELECT CODE FROM SKILLCODES WHERE NAME = "Python")
OR SKILL_CODE & (SELECT CODE FROM SKILLCODES WHERE NAME = "C#")
ORDER BY ID;
```

스터디에서 했던 건데 다시 해서 성공했다.
