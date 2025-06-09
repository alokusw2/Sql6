# Sql6

1 Problem 1 : Game Play Analysis II	(https://leetcode.com/problems/game-play-analysis-ii/)

Write a solution to report the device that is first logged in for each player.

Return the result table in any order.

------------------------------------------------------------------------------------------------------------------
SELECT DISTINCT
  player_id,
  FIRST_VALUE(device_id) OVER (
    PARTITION BY player_id
    ORDER BY event_date
  ) AS device_id
FROM Activity;



------------------------------------------------------------------------------------------------------------------


2 Problem 2 : Game Play Analysis III		(https://leetcode.com/problems/game-play-analysis-iii/)

Write a solution to report for each player and date, how many games played so far by the player. That is, the total number of games played by the player until that date. Check the example for clarity.

Return the result table in any order.
------------------------------------------------------------------------------------------------------------------

SELECT
  player_id,
  event_date,
  SUM(games_played) OVER (PARTITION BY player_id ORDER BY event_date) AS games_played_so_far
FROM Activity;




------------------------------------------------------------------------------------------------------------------

3 Problem 3 : Shortest Distance in a Plane		(https://leetcode.com/problems/shortest-distance-in-a-plane/)

The distance between two points p1(x1, y1) and p2(x2, y2) is sqrt((x2 - x1)2 + (y2 - y1)2).

Write a solution to report the shortest distance between any two points from the Point2D table. Round the distance to two decimal points.
------------------------------------------------------------------------------------------------------------------

SELECT ROUND(
         SQRT(MIN(POW(p1.x-p2.x,2) + POW(p1.y-p2.y,2))),
         2
       ) AS shortest
FROM Point2D p1
JOIN Point2D p2
  ON p1.x<>p2.x OR p1.y<>p2.y;




------------------------------------------------------------------------------------------------------------------

4 Problem 4 : Combine Two Tables	(https://leetcode.com/problems/combine-two-tables/)

Write a solution to report the first name, last name, city, and state of each person in the Person table. If the address of a personId is not present in the Address table, report null instead.

Return the result table in any order.


------------------------------------------------------------------------------------------------------------------


SELECT p.firstName, p.lastName, a.city, a.state
FROM Person p
LEFT JOIN Address a USING(personId);



------------------------------------------------------------------------------------------------------------------

5 Problem 5 : Customers with Strictly Increasing Purchases		( https://leetcode.com/problems/customers-with-strictly-increasing-purchases/description/)

Write a solution to report the IDs of the customers with the total purchases strictly increasing yearly.

The total purchases of a customer in one year is the sum of the prices of their orders in that year. If for some year the customer did not make any order, we consider the total purchases 0.
The first year to consider for each customer is the year of their first order.
The last year to consider for each customer is the year of their last order.
Return the result table in any order.
------------------------------------------------------------------------------------------------------------------

WITH Y AS (
  SELECT customer_id, YEAR(order_date) yr, SUM(price) tot
  FROM Orders
  GROUP BY customer_id, yr
)
SELECT customer_id
FROM Y y1
LEFT JOIN Y y2 
  ON y1.customer_id = y2.customer_id AND y2.yr = y1.yr + 1
GROUP BY customer_id
HAVING EVERY(y2.tot > y1.tot);




------------------------------------------------------------------------------------------------------------------
