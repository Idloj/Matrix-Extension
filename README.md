
# NetLogo matrix extension

This package contains the NetLogo matrix extension, which provides support for a new datatype (mathematical "matrix" objects) and associated operations (like matrix multiplication) in NetLogo.

## Building

Run `sbt package`.

If compilation succeeds, `matrix.jar` and `matrix.zip` will be created.

## Using

The matrix extension adds a new matrix data structure to NetLogo.
A matrix is a mutable 2-dimensional array containing only numbers.

### When to Use

Although matrices store numbers, much like a list of lists, or an
array of arrays, the primary reason to use the matrix data type is to
take advantage of special mathematical operations associated with
matrices. For instance, matrix multiplication is a convenient way to
perform geometric transformations, and the repeated application of
matrix multiplication can also be used to simulate other dynamic
processes (for instance, processes on graph/network structures).

If you'd like to know more about matrices and how they can be
used, you might consider a course on linear algebra, or search the
web for tutorials. The matrix extension also allows you to solve
linear algebraic equations (specified in a matrix format), and even
to identify trends in your data and perform linear (ordinary least
squares) regressions on data sets with multiple explanatory
variables.

### How to Use

The matrix extension comes preinstalled.

To use the matrix extension in your model, add a line to the top of your Code tab:

```NetLogo
extensions [matrix]
```

If your model already uses other extensions, then it already has an
`extensions` line in it, so just add `matrix` to the list.

### Example

```NetLogo
let m matrix:from-row-list [[1 2 3] [4 5 6]]
print m
=> {{matrix:  [ [ 1 2 3 ][ 4 5 6 ] ]}}
print matrix:pretty-print-text m
=>
[[ 1  2  3 ]
 [ 4  5  6 ]]

print matrix:dimensions m
=> [2 3]
;;(NOTE: row & column indexing starts at 0, not 1)
print matrix:get m 1 2 ;; what number is in row 1, column 2?
=> 6
matrix:set m 1 2 10 ;; change the 6 to a 10
print m
=> {{matrix:  [ [ 1 2 3 ][ 4 5 10 ] ]}}

let m2 matrix:make-identity 3
print m2
=> {{matrix:  [ [ 1 0 0 ][ 0 1 0 ][ 0 0 1 ] ]}}
print matrix:times m m2 ;; multiplying by the identity changes nothing
=> {{matrix:  [ [ 1 2 3 ][ 4 5 10 ] ]}}

;; make a new matrix with the middle 1 changed to -1
let m3 (matrix:set-and-report m2 1 1 -1)
print m3
=> {{matrix:  [ [ 1 0 0 ][ 0 -1 0 ][ 0 0 1 ] ]}}
print matrix:times m m3
=> {{matrix:  [ [ 1 -2 3 ][ 4 -5 10 ] ]}}

print matrix:to-row-list (matrix:plus m2 m3)
=> [[2 0 0] [0 0 0] [0 0 2]]
```

## Primitives

### Matrix creation and conversion to/from lists

[`matrix:make-constant`](#matrixmake-constant)
[`matrix:make-identity`](#matrixmake-identity)
[`matrix:from-row-list`](#matrixfrom-row-list)
[`matrix:from-column-list`](#matrixfrom-column-list)
[`matrix:to-row-list`](#matrixto-row-list)
[`matrix:to-column-list`](#matrixto-column-list)
[`matrix:copy`](#matrixcopy)
[`matrix:pretty-print-text`](#matrixpretty-print-text)

### Advanced features

[`matrix:solve`](#matrixsolve)
[`matrix:forecast-linear-growth`](#matrixforecast-linear-growth)
[`matrix:forecast-compound-growth`](#matrixforecast-compound-growth)
[`matrix:forecast-continuous-growth`](#matrixforecast-continuous-growth)
[`matrix:regress`](#matrixregress)

### Matrix data retrieval and manipulation

[`matrix:get`](#matrixget)
[`matrix:get-row`](#matrixget-row)
[`matrix:get-column`](#matrixget-column)
[`matrix:set`](#matrixset)
[`matrix:set-row`](#matrixset-row)
[`matrix:set-column`](#matrixset-column)
[`matrix:swap-rows`](#matrixswap-rows)
[`matrix:swap-columns`](#matrixswap-columns)
[`matrix:set-and-report`](#matrixset-and-report)
[`matrix:dimensions`](#matrixdimensions)
[`matrix:submatrix`](#matrixsubmatrix)
[`matrix:map`](#matrixmap)

### Math operations

[`matrix:times-scalar`](#matrixtimes-scalar)
[`matrix:times`](#matrixtimes)
[`matrix:*`](#matrix*)
[`matrix:times-element-wise`](#matrixtimes-element-wise)
[`matrix:plus-scalar`](#matrixplus-scalar)
[`matrix:plus`](#matrixplus)
[`matrix:+`](#matrix+)
[`matrix:minus`](#matrixminus)
[`matrix:-`](#matrix-)
[`matrix:inverse`](#matrixinverse)
[`matrix:transpose`](#matrixtranspose)
[`matrix:real-eigenvalues`](#matrixreal-eigenvalues)
[`matrix:imaginary-eigenvalues`](#matriximaginary-eigenvalues)
[`matrix:eigenvectors`](#matrixeigenvectors)
[`matrix:det`](#matrixdet)
[`matrix:rank`](#matrixrank)
[`matrix:trace`](#matrixtrace)




### `matrix:make-constant`

Reports a new n-rows by n-cols matrix object, with all entries in the matrix containing the same value (number).


### `matrix:make-identity`


Reports a new square matrix object (with dimensions n-size x
n-size), consisting of the identity matrix (1s along the main
diagonal, 0s elsewhere).


### `matrix:from-row-list`


Reports a new matrix object, created from a NetLogo list, where
each item in that list is another list (corresponding to each of
the rows of the matrix.)

```NetLogo
print matrix:from-row-list [[1 2] [3 4]]
=> {{matrix:  [ [ 1 2 ][ 3 4 ] ]}}
;; Corresponds to this matrix:
;; 1 2
;; 3 4
```


### `matrix:from-column-list`


Reports a new matrix object, created from a NetLogo list containing
each of the *columns* of the matrix.


### `matrix:to-row-list`

Reports a list of lists, containing each row of the matrix.


### `matrix:to-column-list`

Reports a list of lists, containing each column of the matrix.


### `matrix:copy`


Reports a new matrix that is an exact copy of the given matrix.
This primitive is important because the matrix type is mutable
(changeable). Here's a code example:

```NetLogo
let m1 matrix:from-column-list [[1 4 7][2 5 8][3 6 9]] ; a 3x3 matrix
print m1
=> {{matrix:  [ [ 1 2 3 ][ 4 5 6 ][ 7 8 9 ] ]}}
let m2 m1 ;; m2 refers to the same matrix object as m1
let m3 matrix:copy m1 ;; m3 is a new copy containing m1's data
matrix:set m1 0 0 100 ;; now m1 is changed

print m1
=> {{matrix:  [ [ 100 2 3 ][ 4 5 6 ][ 7 8 9 ] ]}}

print m2
=> {{matrix:  [ [ 100 2 3 ][ 4 5 6 ][ 7 8 9 ] ]}}
;;Notice that m2 was also changed, when m1 was changed!

print m3
=> {{matrix:  [ [ 1 2 3 ][ 4 5 6 ][ 7 8 9 ] ]}}
```



### `matrix:pretty-print-text`

Reports a string that is a textual representation of the matrix, in a format that is reasonably human-readable when displayed.


### `matrix:get`

Reports the (numeric) value at location *row-i* (second argument), *col-j* (third argument), in the given matrix given in the first argument


### `matrix:get-row`

Reports a simple (not nested) NetLogo list containing the elements of *row-i* (second argument) of the matrix supplied in the first argument.


### `matrix:get-column`

Reports a simple (not nested) NetLogo list containing the elements of *col-j* of the matrix supplied in the first argument.


### `matrix:set`

Changes the given *matrix* by setting the value at location *row-i*, *col-j* to *new-value*


### `matrix:set-row`


Changes the given matrix *matrix* by replacing the row at
*row-i* with the contents of the simple (not nested) NetLogo
list *simple-list*. The *simple-list* must have a length
equal to the number of columns in the matrix, i.e., the matrix row
length.



### `matrix:set-column`


Changes the given matrix *matrix* by replacing the column at
*col-j* with the contents of the simple (not nested) NetLogo
list *simple-list*. The *simple-list* must have a length
equal to the number of rows in the matrix, i.e., the matrix
column length length.



### `matrix:swap-rows`

Changes the given matrix *matrix* by swapping the rows at *row1* and *row2* with each other.


### `matrix:swap-columns`

Changes the given matrix *matrix* by swapping the columns at *col1* and *col2* with each other.


### `matrix:set-and-report`


Reports a new matrix, which is a copy of the given matrix except
that the value at *row-i*,*col-j* has been changed to
*new-value*. A NetLogo statement such as
 `set mat matrix:set-and-report mat 2 3 10` will result in `mat` pointing
to this new matrix, a copy of the old version of mat with the
element at row 2, column 3 being set to 10. The old version of `mat`
will be "lost".



### `matrix:dimensions`

Reports a 2-element list ([num-rows,num-cols]), containing the number of rows and number of columns in the given *matrix*


### `matrix:submatrix`


Reports a new matrix object, consisting of a rectangular subsection of the given matrix.
The rectangular region is from row *r1* up to (but not including) row *r2*,
and from column *c1* up to (but not including) column *c2*.

 Here is an example:

```NetLogo
let m matrix:from-row-list [[1 2 3][4 5 6][7 8 9]]
print matrix:submatrix m 0 1 2 3 ; matrix, row-start, col-start, row-end, col-end
                                 ; rows from 0 (inclusive) to 2 (exclusive),
                                 ; columns from 1 (inclusive) to 3 (exclusive)
=> {{matrix:  [ [ 2 3 ][ 5 6 ] ]}}
```



### `matrix:map`


Reports a new matrix which results from applying <i>reporter</i>
(an anonymous reporter or the name of a reporter)
to each of the elements of the given matrix. For example,

```NetLogo
matrix:map sqrt matrix
```

would take the square root of each
element of *matrix*. If more than one matrix argument is provided,
the reporter is given the elements of each matrix as arguments. Thus,

```NetLogo
(matrix:map + matrix1 matrix2)
```

would add *matrix1* and *matrix2*.

This reporter is meant to be the same as `map`, but for matrices
instead of lists.



### `matrix:times-scalar`


As of NetLogo 5.1, `matrix:times` can multiply matrices by scalars
making this function obsolete. Use `matrix:times` instead.

Reports a new matrix, which is the result of multiplying every
entry in the original *matrix* by the given scaling *factor*.


### `matrix:times`


Reports a matrix, which is the result of multiplying the given matrices and scalars
(using standard matrix multiplication -- make sure your matrix dimensions match up.)
Without parentheses, it takes two arguments. With parentheses it takes
two or more. The arguments may either be numbers or matrices, but at
least one must be a matrix.



### `matrix:*`


Reports a matrix, which is the result of multiplying the given matrices
and/or scalars (using standard matrix multiplication -- make sure your matrix
dimensions match up.) This is exactly the same as `matrix:times m1 m2`

Takes precedence over `matrix:+` and `matrix:-`, same as normal multiplication.



### `matrix:times-element-wise`


Reports a matrix, which is the result of multiplying the given matrices
together, element-wise. All elements are multiplied by scalar arguments
as well.
Note that all matrix arguments must have the same dimensions.
Without parentheses, it takes two arguments. With parentheses it takes
two or more. The arguments may either be numbers or matrices, but at
least one must be a matrix.



### `matrix:plus-scalar`


As of NetLogo 5.1, `matrix:plus` can add matrices and scalars
making this function obsolete. Use `matrix:plus` instead.

Reports a matrix, which is the result of adding the constant
*number* to each element of the given *matrix*.



### `matrix:plus`


Reports a matrix, which is the result of adding the given matrices
and scalars. Scalars are added to each element.
Without parentheses, it takes two arguments. With parentheses it takes
two or more. The arguments may either be numbers or matrices, but at
least one must be a matrix.



### `matrix:+`


Reports a matrix, which is the result of adding the given matrices
and/or scalars. This is exactly the same as `matrix:plus m1 m2`

Takes precedence after `matrix:*`, same as normal addition.



### `matrix:minus`


Reports a matrix, which is the result of subtracting all arguments
besides *m1* from *m1*. Scalar arguments are treated as
matrices of the same size as the matrix arguments with every element
equal to that scalar.
Without parentheses, it takes two arguments. With parentheses it takes
two or more. The arguments may either be numbers or matrices, but at
least one must be a matrix.



### `matrix:-`


Reports a matrix, which is the result of subtracting the given matrices
and/or scalars. This is exactly the same as <pre>matrix:minus m1 m2</pre>

Takes precedence after `matrix:*`, same as normal subtraction.



### `matrix:inverse`

Reports the inverse of the given *matrix*, or results in an error if the matrix is not invertible.


### `matrix:transpose`

Reports the transpose of the given *matrix*.


### `matrix:real-eigenvalues`

Reports a list containing the real eigenvalues of the given *matrix*.


### `matrix:imaginary-eigenvalues`

Reports a list containing the imaginary eigenvalues of the given *matrix*.


### `matrix:eigenvectors`

Reports a matrix that contains the eigenvectors of the given *matrix*. (Each eigenvector as a column of the resulting matrix.)


### `matrix:det`

Reports a the determinant of the *matrix*.


### `matrix:rank`

Reports the effective numerical rank of the *matrix*,obtained from SVD (Singular Value Decomposition).


### `matrix:trace`

Reports the trace of the *matrix*, which is simply the sum of the main diagonal elements.


### `matrix:solve`


Reports the solution to a linear system of equations, specified by
the *A* and *C* matrices. In general, solving a set of
linear equations is akin to matrix division. That is, the goal is
to find a matrix B such that *A* * B = *C*. (For simple
linear systems, *C* and B can both be 1-dimensional matrices
-- i.e. vectors). If A is not a square matrix, then a "least squares" solution is returned.

```NetLogo
;; To solve the set of equations x + 3y = 10 and 7x - 4y = 20
;; We make our A matrix [[1 3][7 -4]], and our C matrix [[10][20]]
let A matrix:from-row-list [[1 3][7 -4]]
let C matrix:from-row-list [[10][20]]
print matrix:solve A C
=> {{matrix:  [ [ 4 ][ 2.0000000000000004 ] ]}}
;; NOTE: as you can see, the results may be only approximate
;; (In this case, the true solution should be x=4 and y=2.)
```


### `matrix:forecast-linear-growth`



Reports a four-element list of the form:

<tt>[ forecast constant slope R<sup>2</sup> ]</tt>

The *forecast* is the predicted next value that would
follow in the sequence given by the *data-list* input, based
on a linear trend-line. Normally *data-list* will contain
observations on some variable, Y, from time t = 0 to time t = (n-1)
where n is the number of observations. The *forecast* is the
predicted value of Y at t = n. The *constant* and *slope*
are the parameters of the trend-line

```
Y = *constant* + *slope* * t.
```

The *R<sup>2</sup>* value measures the goodness of fit of the
trend-line to the data, with an R<sup>2</sup> = 1 being a perfect
fit and an R<sup>2</sup> of 0 indicating no discernible trend.
Linear growth assumes that the variable Y grows by a constant
absolute amount each period.

```NetLogo
;; a linear extrapolation of the next item in the list.
print matrix:forecast-linear-growth [20 25 28 32 35 39]
=> [42.733333333333334 20.619047619047638 3.6857142857142824 0.9953743395474031]
;; These results tell us:
;; * the next predicted value is roughly 42.7333
;; * the linear trend line is given by Y = 20.6190 + 3.6857 * t
;; * Y grows by approximately 3.6857 units each period
;; * the R^2 value is roughly 0.9954 (a good fit)
```


### `matrix:forecast-compound-growth`


Reports a four-element list of the form:

<tt>[ forecast constant growth-proportion R<sup>2</sup> ]</tt>

Whereas [matrix:forecast-linear-growth](#matrix:forecast-linear-growth)
assumes growth by a constant absolute amount each period, [matrix:forecast-compound-growth](#matrix:forecast-compound-growth)
assumes that Y grows by a constant **proportion** each period.
The *constant* and *growth-proportion* are the parameters
of the trend-line

<tt>Y = *constant* * *growth-proportion*<sup>t</sup>.</tt>

Note that the growth proportion is typically interpreted as
*growth-proportion* = *(1.0 + growth-rate)*. Therefore,
if [matrix:forecast-compound-growth](#matrix:forecast-compound-growth)
returns a *growth-proportion* of 1.10, that implies that Y
grows by (1.10 - 1.0) = 10% each period. Note that if growth is
negative, [matrix:forecast-compound-growth](#matrix:forecast-compound-growth)
will return a *growth-proportion* of less than one. E.g., a
*growth-proportion* of 0.90 implies a growth rate of -10%.

**NOTE:** The compound growth forecast is achieved by taking the
ln of Y. (See [matrix:regress](#matrix:regress), below.)
Because it is impossible to take the natural log of zero or a
negative number, [matrix:forecast-compound-growth](#matrix:forecast-compound-growth)
will result in an error if it finds a zero or negative number in
*data-list*.

```NetLogo
;; a compound growth extrapolation of the next item in the list.
print matrix:forecast-compound-growth [20 25 28 32 35 39]
=> [45.60964465307147 21.15254147944863 1.136621034423892 0.9760867518334806]
;; These results tell us:
;; * the next predicted value is approximately 45.610
;; * the compound growth trend line is given by Y = 21.1525 * 1.1366 ^ t
;; * Y grows by approximately 13.66% each period
;; * the R^2 value is roughly 0.9761 (a good fit)
```


### `matrix:forecast-continuous-growth`


Reports a four-element list of the form:

<tt>[ *forecast* *constant* &nbsp;*growth-rate* &nbsp;*R<sup>2</sup>* ]</tt>.
 Whereas [matrix:forecast-compound-growth](#matrix:forecast-compound-growth)
assumes discrete time with Y growing by a given proportion each
finite period of time (e.g., a month or a year), [matrix:forecast-continuous-growth](#matrix:forecast-continuous-growth)
assumes that Y is compounded **continuously** (e.g., each second
or fraction of a second). The *constant* and
*growth-rate* are the parameters of the trend-line

<tt>Y = *constant* * e<sup>(growth-rate * t)</sup></tt>

[matrix:forecast-continuous-growth](#matrix:forecast-continuous-growth)
is the "calculus" analog of [matrix:forecast-compound-growth](#matrix:forecast-compound-growth).
The two will normally yield similar (but not identical) results, as
shown in the example below. *growth-rate* may, of course, be
negative.
>
*NOTE:* The continuous growth forecast is achieved by taking
the ln of Y. (See [matrix:regress](#matrix:regress),
below.) Because it is impossible to take the natural log of zero or
a negative number, [matrix:forecast-continuous-growth](#matrix:forecast-continuous-growth)
will result in an error if it finds a zero or negative number in
*data-list*.

```NetLogo
;; a continuous growth extrapolation of the next item in the list.
print matrix:forecast-continuous-growth [20 25 28 32 35 39]
=> [45.60964465307146 21.15254147944863 0.12805985615332668 0.9760867518334806]
;; These results tell us:
;; * the next predicted value is approximately 45.610
;; * the compound growth trend line is given by Y = 21.1525 * e ^ (0.1281 * t)
;; * Y grows by approximately 12.81% each period if compounding takes place continuously
;; * the R^2 value is roughly 0.9761 (a good fit)
```


### `matrix:regress`


All three of the forecast primitives above are just special cases
of performing an OLS (ordinary-least-squares) linear regression --
the matrix:regress primitive provides a flexible/general-purpose
approach. The input is a matrix *data-matrix*, with the first
column being the observations on the dependent variable and each
subsequent column being the observations on the (1 or more)
independent variables. Thus each row consists of an observation of
the dependent variable followed by the corresponding observations
for each independent variable.

The output is a Logo nested list composed of two elements. The
first element is a list containing the regression constant followed
by the coefficients on each of the independent variables. The
second element is a 3-element list containing the R<sup>2</sup>
statistic, the total sum of squares, and the residual sum of
squares. The following code example shows how the [matrix:regress](#matrix:regress) primitive can be used to
perform the same function as the code examples shown in the
matrix:forecast-*-growth primitives above. (However, keep in mind
that the [matrix:regress](#matrix:regress) primitive is
more powerful than this, and can have many more independent
variables in the regression, as indicated in the fourth example
below.)

```NetLogo
;; this is equivalent to what the matrix:forecast-linear-growth does
let data-list [20 25 28 32 35 39]
let indep-var (n-values length data-list [ x -> x ]) ; 0,1,2...,5
let lin-output matrix:regress matrix:from-column-list (list data-list indep-var)
let lincnst item 0 (item 0 lin-output)
let linslpe item 1 (item 0 lin-output)
let linR2   item 0 (item 1 lin-output)
;;Note the "6" here is because we want to forecast the value at time t=6.
print (list (lincnst + linslpe * 6) (lincnst) (linslpe) (linR2))

;; this is equivalent to what the matrix:forecast-compound-growth does
let com-log-data-list  (map ln [20 25 28 32 35 39])
let com-indep-var2 (n-values length com-log-data-list [ x -> x ]) ; 0,1,2...,5
let com-output matrix:regress matrix:from-column-list (list com-log-data-list com-indep-var2)
let comcnst exp item 0 (item 0 com-output)
let comprop exp item 1 (item 0 com-output)
let comR2       item 0 (item 1 com-output)
;;Note the "6" here is because we want to forecast the value at time t=6.
print (list (comcnst * comprop ^ 6) (comcnst) (comprop) (comR2))

;; this is equivalent to what the matrix:forecast-continuous-growth does
let con-log-data-list  (map ln [20 25 28 32 35 39])
let con-indep-var2 (n-values length con-log-data-list [ x -> x ]) ; 0,1,2...,5
let con-output matrix:regress matrix:from-column-list (list con-log-data-list con-indep-var2)
let concnst exp item 0 (item 0 con-output)
let conrate     item 1 (item 0 con-output)
let conR2       item 0 (item 1 con-output)
print (list (concnst * exp (conrate * 6)) (concnst) (conrate) (conR2))

;; example of a regression with two independent variables:
;; Pretend we have a dataset, and we want to know how well happiness
;; is correlated to snack-food consumption and accomplishing goals.
let happiness [2 4 5 8 10]
let snack-food-consumed [3 4 3 7 8]
let goals-accomplished [2 3 5 8 9]
print matrix:regress matrix:from-column-list (list happiness snack-food-consumed goals-accomplished)
=> [[-0.14606741573033788 0.3033707865168543 0.8202247191011234] [0.9801718440185063 40.8 0.8089887640449439]]
;; linear regression: happiness = -0.146 + 0.303*snack-food-consumed + 0.820*goals-accomplished
;; (Since the 0.820 coefficient is higher than the 0.303 coefficient, it appears that each goal
;; accomplished yields more happiness than does each snack consumed, although both are positively
;; correlated with happiness.)
;; Also, we see that R^2 = 0.98, so the two factors together provide a good fit.
```


## Credits

The matrix extension was originally written by Forrest Stonedahl, with significant contributions from Charles Staelin (in particular, the forecast, regression and map primitives).

The matrix extension uses (and is distributed with) the Jama matrix library (Jama-1.0.3.jar, http://math.nist.gov/javanumerics/jama/), which has been released into the public domain.

## Terms of Use

[![CC0](http://i.creativecommons.org/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)

The NetLogo matrix extension is in the public domain.  To the extent possible under law, Uri Wilensky has waived all copyright and related or neighboring rights.
