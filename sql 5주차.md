1번
```
SELECT *
FROM (SELECT FOOD_TYPE, REST_ID, REST_NAME, MAX(FAVORITES) AS FAVORITES
FROM REST_INFO
GROUP BY FOOD_TYPE
ORDER BY FOOD_TYPE DESC
```
코드 자체가 안 돌아간다
1차적으로 돌아갈 수 있는 코드로 만들려면
```
SELECT *
FROM (SELECT FOOD_TYPE, REST_ID, REST_NAME, MAX(FAVORITES) AS FAVORITES
FROM REST_INFO
GROUP BY FOOD_TYPE)as A
ORDER BY FOOD_TYPE DESC
```
이렇게 해야 했다.
이렇게 하더라도 정답으로 작동하지 않는데, 그 이유는 
REST_ID, REST_NAME과 같은 다른 컬럼들을 포함한 상태에서 GROUP BY FOOD_TYPE만 사용하고 있어서,
원하는 대로 최대 favorite을 가진 레코드를 제대로 반환하지 못한다.
정답 코드는 서브쿼리를 활용하여 정확하게 매칭한다.

2번
```
WITH RankedRest AS (
    SELECT 
        FOOD_TYPE, REST_ID, REST_NAME, FAVORITES,
        ROW_NUMBER() OVER (PARTITION BY FOOD_TYPE ORDER BY FAVORITES DESC, REST_ID) AS rnk
    FROM REST_INFO
)
SELECT 
    FOOD_TYPE, REST_ID, REST_NAME, FAVORITES
FROM RankedRest
WHERE rnk = 1
ORDER BY FOOD_TYPE DESC;
```

해설: 
ROW_NUMBER() OVER (PARTITION BY FOOD_TYPE ORDER BY FAVORITES DESC, REST_ID) AS rnk
이 부분을 통해 food_type별로 나누고, 좋아요가 많은 순서대로 정렬하고, 그에 각각 rnk를 붙인다
그 다음 랭크가 1인 행들을 출력하여 동일한 결과를 만들 수 있다.

장점:
1. 중첩된 서브쿼리나 여러 번 테이블을 조회할 필요 없이 한번에 계산해둔 후 필터링하는 방식이라, 데이터가 커진다면 처리 속도가 유의미하게 빨라질 것이다.,
2. 덧붙여서 코드 또한 보기 직관적인 것 같다.

```
SELECT 
    EMP_NO, 
    EMP_NAME, 
    SAL,
    RANK() OVER (ORDER BY SAL DESC) AS rnk
FROM 
    HR_EMPLOYEES;
```

rank와 dense_rank, row_number 사이에는 동일 점수 처리에 대한 차이가 있다.
rank는 같은 숫자를 공동 등수로 인식하고 그 밑을 내린다 (2등이 2명이면 그 아래는 4등이 됨)
dense_rank는 같은 숫자를 공동 등수로 인식하지만 그 밑을 내리지 않는다 (2등이 2명이어도 그 아래는 3등)
row_number는 말 그대로 행 번호만 부여하기에, 공동 등수를 인식하지 않는다 (점수가 같아도 등수가 다를 수 있음)


