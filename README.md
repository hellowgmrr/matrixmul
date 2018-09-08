# What is *matrixmul*?

I'm creating a portable C matrix multiplication library as a precursor for a portable neural network library that can fit on microcontrollers for embedded devices, hence portable. That means that this library will adhere to standard C as much as possible, neglecting to use system calls that wouldn't facilitate portability across the myriad of microcontrollers, being careful with its underlying assumptions.

As this software is being made for microcontrollers, with matrices so small and possibly few, performance speed-wise is inconsequential. This library instead focuses on being simple to use and integrate into a project that involves microcontrollers.

# Documentation

# Definitions

## `typedef struct MatrixStruct Matrix`

A struct or type for implementing matrices.

#### Struct members

* `int rows` - The number of rows the matrix has.
* `int columns` - The number of columns the matrix has.
* `float elements[MAX_ELEMENTS]` - An array of floating point numbers that are the matrix's elements.

## `MAX_ELEMENTS`

As matrices have to be statically declared, `MAX_ELEMENTS` defines the total number of elements a matrix can have. The default value is `1000`.

# Functions

## `int multiply(Matrix *matrixA, Matrix *matrixB, Matrix *matrixC)`

Multiplies a `Matrix` *matrixA* by a `Matrix` *matrixB* and stores the result in the `Matrix` *matrixC*. Internally uses `clearMatrix`.

Returns `-1` if the number of columns of *matrixA* is not equal to the number of rows of *MatrixB* and returns `0` otherwise.

## `void clearMatrix(Matrix *)`

Sets all the elements of the given matrix to zero. Useful for reusing matrices.

## `void setRowsColumns(int rows, int columns, Matrix *)`

Sets a `Matrix`'s `rows` and `columns` fields. Takes a pointer to a `Matrix`.

## `void setElement(int row, int column, float element, Matrix *)`

Used for setting a `Matrix`'s elements.

#### Parameters

* `int row` - The row of where to set the new element.
* `int column` - The column of where to set the new element.
* `float element` - The element to place.
* `Matrix *` - The matrix to modify.

## `float getElement(int row, int column, Matrix *)`

Retrieves a matrix's element given a row and column.

#### Parameters

* `int row` - The row to retrieve the element from.
* `int column` - The column to retrieve the element from.
* `Matrix *` - The matrix.

## `int add(Matrix *, Matrix *, Matrix *)`
## `int subtract(Matrix *, Matrix *, Matrix *)`

Artifacts from the very start of development of this library, adds and subtracts matrices. The first two arguments are pointers to the `Matrix`'s to be operated on and the third argument is the result `Matrix` to store the result in. The result `Matrix` should be cleared with `clearMatrix` first.

Returns -1 if the rows or columns are not equal, 0 otherwise.

# Testing

`test.c` is used to facilitate testing of the library. This will help with evaluating the performance of the mechanisms of the library. The following is a list of conditions that `test.c` should create.

### Creating `Matrix` constructs using static memory allocation

The statement `static Matrix A, B, C;` should create three `Matrix` structures in the program's data segment prior to program initialization.

### Setting a matrix's number of rows and columns

``` C
setRowsColumns(2, 2, &A);
setRowsColumns(2, 2, &B);
```

The above statements should set the `A` and `B` matrices' numbers of rows and columns to both be two, thereby creating two two-by-two matrix constructs.

### Setting a matrix's elements

``` C
setElement(1, 1, 2, &A);
setElement(1, 2, 1, &A);
setElement(2, 1, 0, &A);
setElement(2, 2, 3, &A);

setElement(1, 1, 1, &B);
setElement(1, 2, 0, &B);
setElement(2, 1, 1, &B);
setElement(2, 2, 2, &B);
```

The above statements should set the elements in `A` to be [2 1 / 0 3] and the elements in `B` to be [1 0 / 1 2].

### Multiplying matrices

The statement `multiply(&A, &B, &C);` should multiply the aforementioned `A` and `B` matrices and store the result in the `C` matrix. `C` should now be a two-by-two matrix with the elements [3 2 / 3 6].