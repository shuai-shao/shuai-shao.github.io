---
title:  "Singular-value Decomposition"
date:   2018-06-25
categories: [data]
tags: [data]
---
Singular-value Decomposition is widely used in collaborative filtering.  
Imagine you are building a recommendation system for recommending books to users. You have m users and n potential books to recommend. You also have some historical ratings of some users on some books.  
Based on this, you can formulate a utility matrix where each row represents a user and each column represents a book. You can, of course, calculate the correlation between users or books and recommend based on a correlation weighted ratings average, but the problem with that is the matrix is very sparse (you have lots of empty entries), and the process is very computationally expensive (hard to scale).  
An idea to resolve this is to factorize the utility matrix using singular-value decomposition. By using, you can decompose the m*n matrix into three matrices: A = USV^T - m*n = (m*k) * (k*k) * (k*n). The idea is to do dimension reduction by extracting  latent factors from the utility matrix (and hopefully still preserves most variance). Intuitively, this is similar to finding a low dimensional manifold out of a high dimensional space. By doing so, you are able to represent information in a condenser way, and thus mitigate the data sparsity and computationally issue.  
Following up on the above book recommendation example. Intuitively what we are doing here is finding k most 'important' features and project users and books onto the k-dimensional space these k features define. For example, one feature could be the genre of the book, another could be the language of the book. Both U and V are singular matrices. Each row of U represents an eigenvector for a user. Each column of V^T represents an eigenvector for a book. S is a diagonal matrix and each diagonal entry represents an eigenvalue (strength of the latent factor). The predicted rating from user i to book j will simply be a summation of the projection of i on dimension k, projection of j on dimension k, and the strength of dimension k over all k dimensions.  
(Get the picture right, and SVD is easy to understand)
