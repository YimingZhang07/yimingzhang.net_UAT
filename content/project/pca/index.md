---
title: PCA Tutorial
summary: A derivation from eigenvectors of covariance with Python codes
tags:
  - Python
  - Statistics
date: '2022-08-02'

external_link: ''

url_code: ''
url_pdf: ''
url_slides: ''
url_video: ''
---

{{<toc>}}

Reference document: [PDF](./pca_tutorial.pdf)

## Eigenvector decomposition

Consider a matrix $\mathbf{X}$ with $m$ features and $n$ observations, which means a dimension of $m\times n$. **Suppose the row means are subtracted**, then its covariance matrix is defined as:
$$
\mathbf{S}_{\mathbf{X}} \equiv \frac{1}{n-1} \mathbf{X X}^{T}
$$
Our goal is to manipulate this covariance matrix to $\mathbf{S_Y}$ so that the redundancy among features are reduced. More specifically, we can do a linear transformation of $\mathbf{X}$ by changing the basis:

>   **Goal:** Find some orthonormal matrix $\mathbf{P}$ where $\mathbf{Y}=\mathbf{P} \mathbf{X}$ such that $\mathbf{S}_{\mathbf{Y}} \equiv \frac{1}{n-1} \mathbf{Y} \mathbf{Y}^{T}$ is diagonalized. The rows of $\mathbf{P}$ are the principal components of $\mathbf{X}$.

Through simple derivation, we find the following: (here $\mathbf{P}$ is still pending to find)

{{<math>}}
$$
\begin{aligned}
\mathbf{S}_{\mathbf{Y}} &=\frac{1}{n-1} \mathbf{Y} \mathbf{Y}^{T} \\
&=\frac{1}{n-1}(\mathbf{P} \mathbf{X})(\mathbf{P} \mathbf{X})^{T} \\
&=\frac{1}{n-1} \mathbf{P} \mathbf{X} \mathbf{X}^{T} \mathbf{P}^{T} \\
&=\frac{1}{n-1} \mathbf{P}\left(\mathbf{X X}^{T}\right) \mathbf{P}^{T} \\
\mathbf{S}_{\mathbf{Y}} &=\frac{1}{n-1} \mathbf{P} \mathbf{A} \mathbf{P}^{T}
\end{aligned}
$$
{{</math>}}

The last step above defined $\mathbf{XX^T}=\mathbf{A}$. And of course, $\mathbf{A}$ is a symmetric matrix, thus it has a eigenvector decomposition, where $\mathbf{E}$ is the orthogonal matrix of eigenvectors, and $\mathbf{D}$ is a diagonal matrix with eigenvalues on each diagonal entries, so we have $\mathbf{A} = \mathbf{E D E}^{T}$.

Now, **select the matrix $\mathbf{P}$ to be a matrix where each row is an eigenvector of $\mathbf{XX^T}$**. Then $\mathbf{A}=\mathbf{P}^{T} \mathbf{D} \mathbf{P}$. Put this decomposition into the above derivation, we have:

{{<math>}}
$$
\begin{aligned}
\mathbf{S}_{\mathbf{Y}} &=\frac{1}{n-1} \mathbf{P} \mathbf{A} \mathbf{P}^{T} \\
&=\frac{1}{n-1} \mathbf{P}\left(\mathbf{P}^{T} \mathbf{D} \mathbf{P}\right) \mathbf{P}^{T} \\
&=\frac{1}{n-1}\left(\mathbf{P} \mathbf{P}^{T}\right) \mathbf{D}\left(\mathbf{P} \mathbf{P}^{T}\right) \\
&=\frac{1}{n-1}\left(\mathbf{P} \mathbf{P}^{-1}\right) \mathbf{D}\left(\mathbf{P} \mathbf{P}^{-1}\right) \\
\mathbf{S}_{\mathbf{Y}} &=\frac{1}{n-1} \mathbf{D}
\end{aligned}
$$
{{</math>}}

## Summary

-   The principle components are just the rows of $\mathbf{P}$, the matrix that transforms $\mathbf{X}$ to $\mathbf{Y}$. Also, it is the eigenvectors of the $\mathbf{XX^T}$.
-    The $i-th$ diagonal value of $\mathbf{S_Y}$ is the variance of $\mathbf{X}$ along $p_i$.

## Coding Experiment

{{<iframe "https://nbviewer.org/github/YimingZhang07/ml_coding_library/blob/main/pca.ipynb">}}
