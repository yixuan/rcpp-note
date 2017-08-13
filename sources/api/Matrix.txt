Matrix
=====================================

.. cpp:class:: Matrix

A :cpp:class:`Matrix` in Rcpp is a :cpp:class:`Vector` with dimension attributes.
It supports all operations defined in :cpp:class:`Vector`, and additionally,
it provides new subsetters to retrieve a row, a column or a submatrix.

Type Definitions
------------------

.. cpp:type:: ComplexMatrix
   
   ::
     
     typedef Matrix<CPLXSXP> ComplexMatrix;

.. cpp:type:: IntegerMatrix
   
   ::
     
     typedef Matrix<INTSXP> IntegerMatrix;
     
.. cpp:type:: LogicalMatrix

   ::

      typedef Matrix<LGLSXP> LogicalMatrix;

.. cpp:type:: NumericMatrix

   ::

      typedef Matrix<REALSXP> NumericMatrix;

.. cpp:type:: RawMatrix

   ::

      typedef Matrix<RAWSXP> RawMatrix;

.. cpp:type:: CharacterMatrix

   ::

      typedef Matrix<STRSXP> CharacterMatrix;

.. cpp:type:: StringMatrix

   ::

      typedef Matrix<STRSXP> StringMatrix;

.. cpp:type:: GenericMatrix

   ::

      typedef Matrix<VECSXP> GenericMatrix;

.. cpp:type:: ListMatrix

   ::

      typedef Matrix<VECSXP> ListMatrix;

.. cpp:type:: ExpressionMatrix

   ::

      typedef Matrix<EXPRSXP> ExpressionMatrix;


Public Member Functions
-------------------------

Constructors
~~~~~~~~~~~~~~

.. cpp:function:: Matrix::Matrix()

   Create a matrix with zero row and zero column.

.. cpp:function:: Matrix::Matrix(const Matrix& other)

   Copy constructor. Resulting object will share the SEXP data with *other*.

.. cpp:function:: Matrix::Matrix(SEXP x)

   Wrap a given Matrix. A type conversion will be conducted if types don't match.

.. cpp:function:: Matrix::Matrix(const Dimension& dims)

   Create a Matrix with the given dimension, and fill it with zeros. The **Dimension**
   class is defined in ``<Rcpp/Dimension.h>``. An example:
   
   .. code-block:: r
      
      library(Rcpp)
      evalCpp("NumericMatrix(Dimension(2, 3))")
      ##      [,1] [,2] [,3]
      ## [1,]    0    0    0
      ## [2,]    0    0    0

.. cpp:function:: Matrix::Matrix(const int& nrows_, const int& ncols)

   Create a Matrix with *nrows_* rows and *ncols* columns, and fill it with zeros.

   .. code-block:: r
      
      library(Rcpp)
      evalCpp("NumericMatrix(2, 3)")
      ##      [,1] [,2] [,3]
      ## [1,]    0    0    0
      ## [2,]    0    0    0

::

   template <typename Iterator>

.. cpp:function:: Matrix::Matrix(const int& nrows_, const int& ncols, Iterator start)

   Create a Matrix with *nrows_* rows and *ncols* columns, and fill it with data starting from *start*.

   .. code-block:: cpp

      #include <Rcpp.h>
      using namespace Rcpp;
      
      // [[Rcpp::export]]
      RObject ex_Matrix_data()
      {
          double src[] = {1, 2, 3, 4, 5, 6};
          return NumericMatrix(2, 2, src);
      }
      
      /*** R
      
      ex_Matrix_data()
      ##      [,1] [,2]
      ## [1,]    1    3
      ## [2,]    2    4

      */
 
.. cpp:function:: Matrix::Matrix(const int& n)

   Create a diagonal Matrix with *n* rows and *n* columns, and fill it with zeroes.
   
   .. code-block:: r
      
      library(Rcpp)
      evalCpp("NumericMatrix(2)")
      ##      [,1] [,2]
      ## [1,]    0    0
      ## [2,]    0    0
 
::

   template <bool NA, typename MAT>

.. cpp:function:: Matrix::Matrix(const MatrixBase<RTYPE, NA, MAT>& other)

   Create a matrix from another object that is also derived from the **MatrixBase** class.
   Typically *other* is an Rcpp sugar expression, such as the example below:

   .. code-block:: cpp

      #include <Rcpp.h>
      using namespace Rcpp;
      
      // [[Rcpp::export]]
      RObject ex_Matrix_fromsugar()
      {
          NumericMatrix x(3);
          IntegerMatrix y = col(x); // rhs is a sugar expression
          return y;
      }
      
      /*** R
      
      ex_Matrix_fromsugar()
      ##      [,1] [,2] [,3]
      ## [1,]    1    2    3
      ## [2,]    1    2    3
      ## [3,]    1    2    3

      */

.. cpp:function:: Matrix::Matrix(const SubMatrix<RTYPE>& sub)

   Create a matrix from a submatrix.

   .. code-block:: cpp

      #include <Rcpp.h>
      using namespace Rcpp;
      
      // [[Rcpp::export]]
      RObject ex_Matrix_fromsub(NumericMatrix x, int n)
      {
          NumericMatrix::Sub sub = x(Range(0, n - 1), Range(0, n - 1));
          NumericMatrix res(sub);
          return res;
      }
      
      /*** R
      
      x = matrix(as.numeric(1:9), 3, 3)
      ex_Matrix_fromsub(x, 2);
      ##      [,1] [,2]
      ## [1,]    1    4
      ## [2,]    2    5

      */

Defined in :cpp:class:`Matrix`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. cpp:function:: int ncol() const

   Return the number of columns.

.. cpp:function:: int nrow() const

   Return the number of rows.

.. cpp:function:: int cols() const

   Alias of ``ncol()``.

.. cpp:function:: int rows() const

   Alias of ``nrow()``.

.. cpp:function:: Row row(int i)

   Return the *i*-th row (0-based).

.. cpp:function:: Column column(int i)

   Return the *i*-th column (0-based).
   
   .. code-block:: cpp

      #include <Rcpp.h>
      using namespace Rcpp;
      
      // [[Rcpp::export]]
      RObject ex_Matrix_rowcol(NumericMatrix x, int ir, int ic)
      {
          NumericMatrix::Row    r = x.row(ir - 1);
          NumericMatrix::Column c = x.column(ic - 1);
          // Row and Column act as vectors
          r[0] = c[0] = 0;
          // They can also be assigned to vectors
          NumericVector rvec = r;
          NumericVector cvec = c;
          return List::create(Named("row") = rvec, Named("col") = cvec);
      }
      
      /*** R
      
      x = matrix(as.numeric(1:9), 3, 3)
      ex_Matrix_rowcol(x, 2, 2);
      ## $row
      ## [1] 0 5 8
      ## 
      ## $col
      ## [1] 0 5 6
      x
      ##      [,1] [,2] [,3]
      ## [1,]    1    0    7
      ## [2,]    0    5    8
      ## [3,]    3    6    9

      */

.. cpp:function:: iterator begin()

   Return an iterator pointing to the first element of the matrix.

.. cpp:function:: iterator end()

   Return an iterator pointing to the *past-the-end* element of the matrix.

.. cpp:function:: const_iterator begin() const

   Return an iterator pointing to the first element of the matrix.
   Read-only.

.. cpp:function:: const_iterator end() const

   Return an iterator pointing to the *past-the-end* element of the matrix.
   Read-only.

::
   
   template <typename U>

.. cpp:function:: void fill_diag(const U& u)

   Fill the diagonal elements of this matrix with *u*.

   .. code-block:: cpp

      #include <Rcpp.h>
      using namespace Rcpp;
      
      // [[Rcpp::export]]
      RObject ex_Matrix_filldiag(NumericMatrix x)
      {
          x.fill_diag(0.0);
          return x;
      }
      
      /*** R
      
      x = matrix(as.numeric(1:9), 3, 3)
      ex_Matrix_filldiag(x);
      x
      ##      [,1] [,2] [,3]
      ## [1,]    0    4    7
      ## [2,]    2    0    8
      ## [3,]    3    6    0
      
      ## This leads to unexpected results
      x2 = matrix(as.numeric(1:8), 2, 4)
      ex_Matrix_filldiag(x2);
      ##      [,1] [,2] [,3] [,4]
      ## [1,]    0    3    5    7
      ## [2,]    2    4    0    8

      */

.. cpp:function:: Proxy operator[](int i)

   Get the reference of the *i*-th element (0-based) of the matrix **without**
   bound check.

.. cpp:function:: const_Proxy operator[](int i) const

   Get the *i*-th element of the vector **without** bound check. Read-only.

.. cpp:function:: Proxy operator()(const size_t& i, const size_t& j)

   Get the reference of the element in *i*-th row and *j*-th column (both 0-based)
   with bound check.

.. cpp:function:: const_Proxy operator()(const size_t& i, const size_t& j) const

   Get the element in *i*-th row and *j*-th column (both 0-based) with bound check.
   Read-only.

.. cpp:function:: Row operator()(int i, internal::NamedPlaceHolder)

   Get the *i*-th row of the matrix (0-based). The symbol ``_`` can be used as a placeholder
   for the second argument.

.. cpp:function:: Column operator()(internal::NamedPlaceHolder, int i)

   Get the *i*-th column of the matrix. The symbol ``_`` can be used as a placeholder
   for the first argument.

.. cpp:function:: Column operator()(internal::NamedPlaceHolder, int i) const

   Get the *i*-th column of the matrix. Read-only.

.. cpp:function:: Sub operator()(const Range& row_range, const Range& col_range)

   Get the submatrix specified by the row range and column range.

.. cpp:function:: Sub operator()(internal::NamedPlaceHolder, const Range& col_range)

   Select several columns of this matrix.

.. cpp:function:: Sub operator()(const Range& row_range, internal::NamedPlaceHolder)

   Select several rows of this matrix.

Examples of matrix subsetters:

.. code-block:: cpp

   #include <Rcpp.h>
   using namespace Rcpp;
   
   // [[Rcpp::export]]
   RObject ex_Matrix_subsetter()
   {
       double src[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12};
       NumericMatrix mat(3, 4, src);
       Rcout << "Matrix:\n";
       Rf_PrintValue(mat);
       
       // Single index subsetter
       Rcout << "\nFirst element: " << mat[0] << "\n";
       Rcout << "\nLast element: " << mat[mat.length() - 1] << "\n";
       
       // double indices subsetter
       Rcout << "\n(1, 3) element: " << mat(0, 2) << "\n";
       
       // Get the first row
       NumericVector r = mat(0, _);
       Rcout << "\nFirst row:\n";
       Rf_PrintValue(r);
       
       // Get the first two rows
       NumericMatrix rs = mat(Range(0, 1), _);
       Rcout << "\nFirst two rows:\n";
       Rf_PrintValue(rs);
       
       // Get the last column
       NumericVector c = mat(_, mat.ncol() - 1);
       Rcout << "\nLast column:\n";
       Rf_PrintValue(c);
       
       // Get columns 1 to 3
       NumericMatrix cs = mat(_, Range(0, 2));
       Rcout << "\nColumns 1 to 3:\n";
       Rf_PrintValue(cs);

       // Submatrix from (2, 2) to (3, 4)
       NumericMatrix sub = mat(Range(1, 2), Range(1, 3));
       Rcout << "\n(2, 2) to (3, 4) block:\n";
       Rf_PrintValue(sub);
       
       return R_NilValue;
   }
   
   /*** R
   
   invisible(ex_Matrix_subsetter())
   ## Matrix:
   ##      [,1] [,2] [,3] [,4]
   ## [1,]    1    4    7   10
   ## [2,]    2    5    8   11
   ## [3,]    3    6    9   12
   ## 
   ## First element: 1
   ## 
   ## Last element: 12
   ## 
   ## (1, 3) element: 7
   ## 
   ## First row:
   ## [1]  1  4  7 10
   ## 
   ## First two rows:
   ##      [,1] [,2] [,3] [,4]
   ## [1,]    1    4    7   10
   ## [2,]    2    5    8   11
   ## 
   ## Last column:
   ## [1] 10 11 12
   ## 
   ## Columns 1 to 3:
   ##      [,1] [,2] [,3]
   ## [1,]    1    4    7
   ## [2,]    2    5    8
   ## [3,]    3    6    9
   ## 
   ## (2, 2) to (3, 4) block:
   ##      [,1] [,2] [,3]
   ## [1,]    5    8   11
   ## [2,]    6    9   12
   
   */

Static Public Member Functions
-------------------------------

::

   template <typename U>

.. cpp:function:: static Matrix diag(int size, const U& diag_value)

   Create a *size* by *size* diagonal matrix and fill the diagonal
   elements with *diag_value*.
   
   .. code-block:: r

      library(Rcpp)
      evalCpp("NumericMatrix::diag(3, 1.2)")
      ##      [,1] [,2] [,3]
      ## [1,]  1.2  0.0  0.0
      ## [2,]  0.0  1.2  0.0
      ## [3,]  0.0  0.0  1.2


