Matrix
=====================================

A **Matrix** in Rcpp is a **Vector** with dimension attributes.
It supports all operations defined in **Vector**, and additionally,
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

.. cpp:function:: Matrix()

   Create a matrix with zero row and zero column.

.. cpp:function:: Matrix(const Matrix& other)

   Copy constructor. Resulting object will share the SEXP data with *other*.

.. cpp:function:: Matrix(SEXP x)

   Wrap a given Matrix. A type conversion will be conducted if types don't match.

.. cpp:function:: Matrix(const Dimension& dims)

   Create a Matrix with the given dimension, and fill it with zeros. The **Dimension**
   class is defined in ``<Rcpp/Dimension.h>``. An example:
   
   .. code-block:: cpp
      
      SEXP gen_matrix()
      {
          Rcpp::Dimension dim(6, 7);
          // a 6x7 matrix
          return Rcpp::NumericMatrix(dim);
      }

.. cpp:function:: Matrix(const int& nrows_, const int& ncols)

   Create a Matrix with *nrows_* rows and *ncols* columns, and fill it with zeros.

::

   template <typename Iterator>

.. cpp:function:: Matrix(const int& nrows_, const int& ncols, Iterator start)

   Create a Matrix with *nrows_* rows and *ncols* columns, and fill it with data starting from *start*.
 
.. cpp:function:: Matrix(const int& n)

   Create a diagonal Matrix with *n* rows and *n* columns, and fill it with zeroes.
 
::

   template <bool NA, typename MAT>

.. cpp:function:: Matrix(const MatrixBase<RTYPE, NA, MAT>& other)

   Create a matrix from another object that is also derived from the **MatrixBase** class.
   Typically *other* is an Rcpp sugar expression, such as the example below:
   
   .. code-block:: cpp
      
      SEXP matrix_from_sugar()
      {
          using namespace Rcpp;
          NumericMatrix x(5);
          IntegerMatrix y = col(x); // rhs is a sugar expression
          return y;
      }

.. cpp:function:: Matrix(const SubMatrix<RTYPE>& sub)

   Create a matrix from a submatrix.

Defined in **Matrix**
~~~~~~~~~~~~~~~~~~~~~~

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

.. cpp:function:: Column col(int i)

   Return the *i*-th column (0-based).

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
   for the second argument. For example:
   
   .. code-block:: cpp
   
      SEXP get_row()
      {
          using namespace Rcpp;
          NumericMatrix x(5);
          x.fill_diag(1);
          NumericMatrix::Row r = x(0, _); // extract the first row
          NumericVector y = r; // copy to a vector
          r[0] = 0; // change the first element to 0
          return x; // x is also changed
      }

.. cpp:function:: Column operator()(internal::NamedPlaceHolder, int i)

   Get the *i*-th column of the matrix. The symbol ``_`` can be used as a placeholder
   for the first argument.

.. cpp:function:: Column operator()(internal::NamedPlaceHolder, int i) const

   Get the *i*-th column of the matrix. Read-only.

.. cpp:function:: Sub operator()(const Range& row_range, const Range& col_range)

   Get the submatrix specified by the row range and column range. An example:
   
   .. code-block:: cpp
   
      SEXP matrix_sub()
      {
          using namespace Rcpp;
          NumericMatrix x(6, 7);
          NumericMatrix y(2, 3);
          x(0, 0) = x(0, 1) = x(1, 1) = x(1, 2) = 1;
          // get the 2x3 matrix in the topleft corner
          NumericMatrix::Sub sub = x(Range(0, 1), Range(0, 2));
          y = sub; // assign y to sub
          return y;
      }

.. cpp:function:: Sub operator()(internal::NamedPlaceHolder, const Range& col_range)

   Select several columns of this matrix.

.. cpp:function:: Sub operator()(const Range& row_range, internal::NamedPlaceHolder)

   Select several rows of this matrix.

Static Public Member Functions
-------------------------------

::

   template <typename U>

.. cpp:function:: static Matrix diag(int size, const U& diag_value)

   Create a *size* by *size* diagonal matrix and fill the diagonal
   elements with *diag_value*. 




