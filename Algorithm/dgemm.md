---
title: dgemm
description: 
published: true
date: 2022-03-27T00:08:32.337Z
tags: 
editor: markdown
dateCreated: 2022-03-26T16:14:08.563Z
---

## DGemm
An problem to resolve widely used in Convolution, HPL, HPCG.

https://zhuanlan.zhihu.com/p/464740681

## SPMV
Numerical linear algebra is a basic calculation module in scientific computing. The calculation of solving linear equations, linear least squares problems, eigenvalues ​​and singular values ​​is the most computationally intensive part of scientific computing. With the advent of numerical programming, it is very effective to use complex subroutine libraries to solve such problems. When we write program code that involves linear algebra operations, we usually decompose calculations into basic subroutines such as point multiplication or matrix vector multiplication. As a result, structured programming came into being, specifying basic building blocks and using unique mnemonic names to identify these operations, and in order to improve the efficiency of the algorithmic use of these algebra programs, the names and parameter lists of some of the basic operations were uniformly planned.

From 1973 to 1977, the first "level" Basic Linear Algebra Subprograms (BLAS) identified some kernel operations, mainly Fortran specifications and implementations of subroutines for scalar and vector operations [1]. With the advent of vectors, hierarchical storage, and parallel shared memory machines, 1984-1988 successively improved the second "level" BLAS of matrix vector operations and the third "level" BLAS [2,3] of operations between matrices. Specification. The three "levels" of BLAS are not only the division of its development process, but also a measure of the complexity of the algorithm [4]. In order to further develop the BLAS library, a BLAS technical forum meeting was started at the University of Tennessee symposium in 1995 to discuss the overall functions of BLAS, sparse BLAS, dense BLAS with distributed memory, extended BLAS calculation accuracy, and mixed BLAS calculation accuracy, interval BLAS and extensions to existing BLAS.

With the continuous development of the BLAS benchmark library, it has been able to be applied to many hardware platforms and serves programs related to numerical calculation in various industries. Among them, GeneralMatrix-matrixMultiplication (GEMM) is scientific computing (high-performance computing, machine learning). For basic operations in engineering and data applications, people have been aiming for different computing platforms, trying to find corresponding optimization methods to make their calculations faster.

### Problem description
For decades, General Matrix-Matrix Multiplication (GEMM) has been the standard benchmark for computing performance. GEMM is the most commonly used type of computing model in high-performance computing. Whether in the HPC field, such as FFT, convolution, correlation, filtering, etc., or in the field of DeepLearning, such as convolution layers, fully connected layers, etc., its core algorithms can be directly or indirectly converted into matrix multiplication operations. The GEMM calculation formula is as follows:

\\[
C \leftarrow \alpha \ op(A)\ op(B) + \beta \ C
\\]

Among them, \\(op(X)\\) represents matrix X, or transposed \\(X^{T}\\) of matrix X, or conjugate transposed \\(X^{H}\\) of matrix X, α and β are scalars, matrix A, m rows and k columns, matrix B, k rows and n columns, Matrix C, m rows and n columns.

There are two possible combinations of different numerical types and precisions in mixed precision:

1) All scalar parameters and output parameters (scalar or array) are double precision, and at least one array is single precision. Then the type of combination is as follows: (S = Singlereal, D = Doublereal, C = Singlecomplex, Z = Doublecomplex)



| \\(\alpha\\) | A    | B    | \\(\beta\\) | C    |
| -------- | ---- | ---- | ------- | ---- |
| D        | S    | S    | D       | D    |
| D        | S    | D    | D       | D    |
| D        | D    | S    | D       | D    |
| Z        | C    | C    | Z       | Z    |
| Z        | C    | Z    | Z       | Z    |
| Z        | Z    | C    | Z       | Z    |



2) The precision of all floating-point parameters must be all single precision or all double precision. All scalar parameters and output parameters (scalars or arrays) are complex numbers (unless due to mathematical calculations, all scalar parameters must be real numbers, such as the sum in HERK). The types of combination are as follows:



| \\(\alpha\\) | A    | B    | \\(\beta\\) | C    |
| -------- | ---- | ---- | ------- | ---- |
| C        | S    | S    | C       | C    |
| C        | S    | C    | C       | C    |
| C        | C    | S    | C       | C    |
| Z        | D    | D    | Z       | Z    |
| Z        | D    | Z    | Z       | Z    |
| Z        | Z    | D    | Z       | Z    |

BLAS implementations are usually optimized for calculation speed for specific machines, so using them can bring significant performance advantages. This competition focuses on the calculation performance of the single-precision real number matrix (SGEMM) on domestic advanced computing platforms. Players can refer to the rocBLAS function library [5, 6] for understanding of the relevant content. The API functions implemented by BatchSgemm in the rocblas function library For: rocblas_sgemm_strided_batched.

As the amount of data continues to increase, the size of matrices that need to be calculated in numerical calculations increases. People have proposed to use multiple batches to accelerate matrix multiplication calculations, because multi-batch matrix multiplication can better utilize the computing resources of hardware calculation accelerators. The sub-matrix in each batch in the calculation has a stride address offset and has the same size. Calculated as follows:

\\[
C\left[i *\right. stride \left._{c}\right] \leftarrow \alpha * o p\left(A\left[i *\right.\right. stride \left.\left._{a}\right]\right) * op \left(B\left[i *\right.\right. stride \left.\left._{b}\right]\right)+\beta * C\left[i *\right. stride \left._{c}\right] i \in\left[0\right. ,\text{batch count} - 1]
\\]

In order to further improve the calculation efficiency of the matrix, batch and strided calculation strategies are introduced on the basis of the original matrix multiplication. In order to make full use of the performance advantages of the GPU-like heterogeneous accelerator used in the cluster in this calculation method, the function implementation needs to be further optimized.

### Test Methods

The example function to implement strided batched matrix-matrix operations is as follows. In order to facilitate the better optimization of the players, the function and its parameters are explained as follows:

```
sgemm strided batched(  sgemm operation trans a,
                        sgemm operation trans b,
                        intm,
                        intn,
                        int k,
                        const float* alpha
                        const float A,
                        int lda,
                        int stride a,
                        const float* B
                        int ldb,
                        int stride b,
                        const float* beta,
                        float C,
                        int ldc,
                        int stride C 
                        int batch count)
typedef enum sgemm operation_ {
        operation none = 0,
        operation transpose = 1,
        operation conjugate transpose = 2
} sgemm operation;
```


Input parameters:

Parameter trans_a: sgemm_operation type. Details the format of op (A) used in matrix multiplication

If trans_a = operation_none, then op (A) = A;

If trans_a = operation_transpose, then op (A) = A ';

If trans_a = operation_conjugate_transpose, then op (A) = conjg (A ').

Parameter trans_b: sgemm_operation type. The definition is the same as trans_a;

Parameter m: the number of rows of matrix A, m> 0;

Parameter n: the number of columns in matrix B, n> 0;

Parameter k: the number of columns of matrix A and the number of rows of matrix B, K> 0;

Parameter alpha: is a single-precision real number, representing the scalar coefficient of matrix A;

Parameter A: indicates that the matrix stored in the form of a pointer on the GPU is a single-precision real number matrix A;

Parameter Ida: refers to the size of the first dimension of matrix A when it is actually stored, that is, if the matrix is stored first by row, then Ida K; if it is stored first by column, Ida ≯ M


Parameter stride_a: represents the span from the A matrix to the next matrix;

Parameter B: indicates that the matrix stored in the form of a pointer on the GPU is a single-precision real number matrix B;

Parameter Idb: refers to the size of the first dimension of matrix B in actual storage, and the meaning of details is the same as lda; stride_b represents the span from the start of matrix B to the next matrix;

Parameter beta: a single-precision real number representing the scalar coefficient of matrix C. If beta = 0, you do not need to define matrix C;

Parameter C: indicates that the matrix C stores the elements in the form of pointers as single-precision real numbers;

Parameter Idc: refers to the size of the first dimension of the matrix C in actual storage, with the same details as lda;

Parameter stride_c: represents the span from the start of the C matrix to the next matrix;

Parameter batch_count: indicates the number of sgemm operations in the same batch.

  Output parameters

Parameter C: C matrix covering the input.

  Claim:

1. The player performs matrix multiplication performance optimization based on the given interface function. In a given test code, the API interface function cannot be changed. The non-fixed parameter players involved can be tuned by themselves according to the matrix size involved in the calculation;

2. In the test sample section, the code gives a pseudo-random number generated dense matrix as test data to verify the performance of the algorithm. The main test case sizes are the following three:

|  | M    | N    | K | Batch    |
| -------- | ---- | ---- | ------- | ---- |
| 1        | 64    | 64    | 32       | 30    |
| 2        | 128    | 128    | 64       | 20    |
| 3        | 128    | 512    | 256       | 10    |

3. The competitor needs to submit the function to implement part of the code, the compilation method of the function library, and the generated dynamic link library, test samples, test process, and the encrypted sequence of the running results.

Note: The contestants use the test script provided by the contest group as the basis to improve the function implementation part. To avoid affecting the contestants' performance, please compile and execute the time-encrypted serial code generated by calculating the corresponding matrix to the designated location on the web page.

### SGEMM Kernel Optimization on VEGA

https://github.com/victoryang00/SGEMM_on_VEGA

# Reference
1. C. L. Lawson, R. J. Hanson, D. Kincaid, and F. T. Krogh. Basic Linear Algebra Subprograms for FORTRAN usage. ACM Trans.Math.Software,5:308-323,1979.
2. J. J. Dongarra, J. Du Croz, I. S. Duff, and D. Hammarling. A set of Level 3 Basic Linear Algebra Subprograms. ACM Trans. Math.Software,16:1-28,1990.
3. J. J. Dongarra, J. Du Croz, D. Hammarling, R. J. Hanson. An extended set of FORTRAN Basic Linear Algebra Subprograms. ACM Trans. Math. Software, 14:1-32, 399, 1988.
4. https://en.wikipedia.org/wiki/Basic_Linear_Algebra_Subprograms 
5. https://i-techx.github.io/iTechX/courses?course_code=CS121