---
title:  "Difference between apply and map (and applymap) in Pandas"
date:   2016-03-22
categories: [data]
tags: [data, python]
---
Shown in one simple example:  
1. Create dataframe:  
![createdf](/images/blogs/applymap/createdf.png)  
2. map:  
Iterates over each element of a series 
![map](/images/blogs/applymap/map.png)  
3. apply:
applies a function along any axis of a dataframe.  
axis = 0: vertical  
axis = 1: horizontal  
![apply](/images/blogs/applymap/apply.png)  
4. applymap:  
apply a function to each element of a dataframe  
![applymap](/images/blogs/applymap/applymap.png)  