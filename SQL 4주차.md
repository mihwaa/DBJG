1번 정답
```
select ITEM_INFO.ITEM_ID, ITEM_INFO.ITEM_NAME
FROM ITEM_INFO JOIN ITEM_TREE
ON ITEM_INFO.ITEM_ID = ITEM_TREE.ITEM_ID
WHERE PARENT_ITEM_ID IS NULL
```

헷갈리지 않기 위해 별칭을 지정했다가 오히려 더 헷갈려서 해제했다.
무난히 풀 수 있는 문제였다.

2번 정답
```
SELECT ROUTE,
CONCAT(ROUND(SUM(D_BETWEEN_DIST), 2), 'km') AS TOTAL_DISTANCE,
CONCAT(ROUND(AVG(D_BETWEEN_DIST), 3), 'km') AS AVERAGE_DISTANCE FROM SUBWAY_DISTANCE
GROUP BY ROUTE
ORDER BY TOTAL_DISTANCE DESC;
```

concat을 처음으로 활용해 보아서 조금 헤맸지만 사용법을 확실히 알게 되었다.
+ 문제에 오류가 있는 것 같다... TOTAL DISTANCE순으로 하면 정답처리가 안된다
+ 오히려 OREDER BY ROUTE DESC로 바꾸었을 때 정답처리가 되었다.

3번 오답
```
SELECT ID, NAME, HOST_ID FROM PLACES
GROUP BY HOST_ID
HAVING COUNT(HOST_ID) >= 2
ORDER BY ID
```

3번 정답
```
SELECT ID, NAME, HOST_ID FROM PLACES
WHERE HOST_ID IN(
    SELECT HOST_ID FROM PLACES
    GROUP BY HOST_ID
    HAVING COUNT(HOST_ID) >= 2)
ORDER BY ID
```

가장 많이 고민하고 틀린 문제였다.
그룹바이를 그냥 걸고 헤빙을 하면 대표 행 하나씩만 출력이 되어서 모든 행이 출력되지 않는다는 걸 알게 되었다.
서브쿼리를 활용하여 해결하였다.

