1번
```
SELECT INGREDIENT_TYPE, SUM(TOTAL_ORDER) AS TOTAL_ORDER
FROM ICECREAM_INFO I JOIN FIRST_HALF F
ON I.F.FLAVOR = F.FLAVOR
GROUP BY INGREDIENT_TYPE
```
무난하게 풀 줄 알았는데 시험기간동안 안 봤다고 기초 문법 형태도 가물가물했다.

2번
```
SELECT FOOD_TYPE, REST_ID, REST_NAME, FAVORITES FROM REST_INFO
WHERE FAVORITES IN (SELECT MAX(FAVORITES) FROM REST_INFO GROUP BY FOOD_TYPE)
GROUP BY FOOD_TYPE
ORDER BY FOOD_TYPE DESC
```
IN 다음에 오는 SELECT 절에서 괄호의 위치가 꽤 많이 헷갈렸다.
IN 안에 새로운 SELECT절을 넣는다고 생각하고 짜면 쉬워졌다.

3번

오답 코드
'''
SELECT ID, NAME, HOST_ID FROM PLACES
WHERE COUNT(HOST_ID) > 2
ORDER BY ID
```
> where절에 window함수를 직접 사용할 수 없다!
> groupby절에 having으로 활용해보자

오답 2
'''
SELECT ID, NAME, HOST_ID FROM PLACES
GROUP BY HOST_ID
HAVING COUNT(HOST_ID) >= 2
ORDER BY ID
'''
> 그룹바이에 헤빙절을 사용하면 조건에 맞는 하나의 행만 반환된다!
> 간접적으로 해보자

정답
```
SELECT ID, NAME, HOST_ID FROM PLACES
WHERE HOST_ID IN (SELECT HOST_ID FROM PLACES
                 GROUP BY HOST_ID
                 HAVING COUNT(HOST_ID) >= 2)
ORDER BY ID;
```
HOST ID가 카운트 2 이상인 HOST_ID 테이블에 속하는지 확인하는 방식으로 하면 구할 수 있다.
> SQL적 사고가 필요하다!

