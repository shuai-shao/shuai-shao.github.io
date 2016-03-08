---
title:  "SQL - First row in each Group"
date:   2016-03-07
categories: [data]
tags: [data,sql]
---
Say we have a nice teacher rating website for Hogwarts students (www.castledoor.hw) and users like Harry and Hermione like to come and review their professors.  I have got a **reviews** table that looks like:

![sql1](/images/blogs/sql1.jpg)

**So the question is:**  
How to get the highest number of reviews by each student?  
Sample output would be:  

![sql2](/images/blogs/sql2.jpg)

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