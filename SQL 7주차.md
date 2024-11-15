
1번
```
SELECT ANIMAL_TYPE, IFNULL(NAME, "No name") as NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```
IFNULL(컬럼명, "NULL일경우 대체값")

2번
```
SELECT ANIMAL_TYPE, 
CASE 
    WHEN NAME IS NULL THEN "No name"
    ELSE NAME
    END AS NAME,
SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```
CASE WHEN END로 바꿔서 푸는 것도 문제를 풀기 위해 가지는 사고방식은 비슷한 것 같다

3번

```
SELECT
ANIMAL_ID,NAME,
CASE WHEN SEX_UPON_INTAKE LIKE '%Neutered%' OR SEX_UPON_INTAKE LIKE '%Spayed%' THEN "O"
ELSE "X"
END AS '중성화'
FROM ANIMAL_INS
ORDER BY ANIMAL_ID ASC
```
예전에 풀었던 문제긴 했다
특정 문자열을 포함하는지에 대한 코드를 짜고 싶을 때 LIKE를 활용하면 된다.

4번

```
SELECT animal_id, name,
       if (sex_upon_intake like '%Neutered%' or '%Spayed%', 'O' , 'X') as '중성화'
from animal_ins
order by 1
```
틀린 이유:  
코드를 돌렸을 때 실행은 되지만 결과가 달랐다.

그 이유는 
OR 전후의 '%Neutered%' or '%Spayed%'에 있다.  
일반적인 사람의 문법처럼 생각한다면  like '%Neutered%' or '%Spayed%'라고 쓰면 될 것 같지만  
프로그래밍 언어에서는
 sex_upon_intake like '%Neutered%' or sex_upon_intake like'%Spayed%'  
 라고 써 주어야 된다. (or 연산자의 작동 방식을 생각하면 쉽다.)


