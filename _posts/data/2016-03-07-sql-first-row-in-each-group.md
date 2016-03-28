---
title:  "SQL - First row in each Group"
date:   2016-03-07
categories: [data]
tags: [data,sql]
---
Say we have a nice teacher rating website for Hogwarts students (www.castledoor.hw) and users like Harry and Hermione like to come and review their professors.  I have got a **reviews** table that looks like:  
<!-- this part is for inserting a table, which is not convenient in markdown-->
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg .tg-yw4l{vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-031e">PK_id</th>
    <th class="tg-031e">users</th>
    <th class="tg-031e">n_reviews</th>
  </tr>
  <tr>
    <td class="tg-031e">1</td>
    <td class="tg-031e">Harry</td>
    <td class="tg-031e">5</td>
  </tr>
  <tr>
    <td class="tg-031e">2</td>
    <td class="tg-031e">Hermione</td>
    <td class="tg-031e">8</td>
  </tr>
  <tr>
    <td class="tg-031e">3</td>
    <td class="tg-031e">Hermione</td>
    <td class="tg-031e">12</td>
  </tr>
  <tr>
    <td class="tg-yw4l">4</td>
    <td class="tg-yw4l">Harry</td>
    <td class="tg-yw4l">3</td>
  </tr>
  <tr>
    <td class="tg-yw4l">5</td>
    <td class="tg-yw4l">Hermione</td>
    <td class="tg-yw4l">6</td>
  </tr>
</table>


**So the question is:**  
How to get the highest number of reviews by each student?  
Sample output would be:  
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
</style>
<table class="tg">
  <tr>
    <th class="tg-031e">PK_id</th>
    <th class="tg-031e">users</th>
    <th class="tg-031e">n_reviews</th>
  </tr>
  <tr>
    <td class="tg-031e">1</td>
    <td class="tg-031e">Harry</td>
    <td class="tg-031e">5</td>
  </tr>
  <tr>
    <td class="tg-031e">3</td>
    <td class="tg-031e">Hermione</td>
    <td class="tg-031e">12</td>
  </tr>
</table>

**And the question really is:**   
How to select first row of each set of rows grouped with a _GROUP BY_?  
**Answer:**

```sql
WITH review_order AS
(
SELECT
	PK_id, users, n_reviews,
	ROW_NUMBER() OVER (PARTITION BY users ORDER BY n_reviews DESC) AS rn
FROM
	reviews
)
SELECT
	PK_id, users, n_reviews
FROM
	review_order
WHERE
	rn = 1
``` 

* This only works in MS SQL and Oracle, not in MySQL.
