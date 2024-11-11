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
