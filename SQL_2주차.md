1번

SELECT seller_id, COUNT(DISTINCT order_id) AS orders  
FROM olist_order_items_dataset  
GROUP BY seller_id  
HAVING COUNT(DISTINCT order_id) >= 100;  


>> distinct의 활용이 중요하다 (중복하여 count되는 것 방지)  
>> 어떤 원리인지 더 학습 필요

2번

select *  
from tips  
WHERE size%2 = 1;  

>> 쉽게 풀 수 있었다  
>> 1번과 비교해서 언제 having 절을 쓰고 언제 where절을 쓰는지가 중요  

3번  

WITH daily_tips AS (  
  SELECT day, ROUND(SUM(tip), 2) AS tip_daily  
  FROM tips  
  GROUP BY day   
)  
SELECT day, tip_daily  
FROM daily_tips  
ORDER BY tip_daily DESC  
LIMIT 1;  
  
>>> 쉽게 풀지 못했다  
>>> with절의 사용방법 익히기  



![ㅇ](./images/ii.png)

