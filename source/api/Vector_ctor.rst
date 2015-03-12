Vector (Typedefs and Constructors)
=====================================

The **Vector** class in Rcpp is a template class that represent various types of
vectors (integer, numeric, character, list, etc.) in R. It's also the foundation of
:doc:`Matrix` and higher dimensional array structures. The **Vector** class accepts one
major template parameter, ``RTYPE``, to indicate the type of its elements.

Type Definitions
------------------

.. cpp:type:: ComplexVector
   
   ::
     
     typedef Vector<CPLXSXP> ComplexVector;

.. cpp:type:: IntegerVector
   
   ::
     
     typedef Vector<INTSXP> IntegerVector;
     
.. cpp:type:: LogicalVector

   ::

      typedef Vector<LGLSXP> LogicalVector;

.. cpp:type:: NumericVector

   ::

      typedef Vector<REALSXP> NumericVector;

.. cpp:type:: DoubleVector

   ::

      typedef Vector<REALSXP> DoubleVector;

.. cpp:type:: RawVector

   ::

      typedef Vector<RAWSXP> RawVector;

.. cpp:type:: CharacterVector

   ::

      typedef Vector<STRSXP> CharacterVector;

.. cpp:type:: StringVector

   ::

      typedef Vector<STRSXP> StringVector;

.. cpp:type:: GenericVector

   ::

      typedef Vector<VECSXP> GenericVector;

.. cpp:type:: List

   ::

      typedef Vector<VECSXP> List;

.. cpp:type:: ExpressionVector

   ::

      typedef Vector<EXPRSXP> ExpressionVector;


Public Member Functions
-------------------------

Constructors
~~~~~~~~~~~~~~

.. cpp:function:: Vector()

   Default constructor. This creates a vector of the appropriate type and zero length.

.. cpp:function:: Vector(const Vector& other)

   Copy constructor. Resulting object will share the SEXP data with *other*.

.. cpp:function:: Vector(SEXP x)

   Wrap a given vector. A type conversion will be conducted if types do not match.

``template <typename Proxy>``

.. cpp:function:: Vector(const GenericProxy<Proxy>& proxy)

   Create vector from a proxy, such as attribute, slot, field, etc.

.. cpp:function:: Vector(const no_init& obj)

   Create a vector without initializating the values. An example:
   
   .. code-block:: r
      
      library(Rcpp)
      evalCpp("NumericVector(Rcpp::no_init(10))")
      ## [1] 3.458460e-323 6.946245e-310 4.940656e-324 6.946245e-310 9.881313e-324
      ## [6] 6.946245e-310 6.946245e-310  0.000000e+00 6.946245e-310  0.000000e+00

.. cpp:function:: Vector(const int& size, const stored_type& u)

   Create a vector of length *size*, and fill it with value *u*.

   .. code-block:: r
      
      library(Rcpp)
      evalCpp("NumericVector(10, 2.0)")
      ## [1] 2 2 2 2 2 2 2 2 2 2

``template <typename U>``

.. cpp:function:: Vector(const int& size, const U& u)

   Create a vector of length *size*, and fill it with value *u*. Type will
   be converted if necessary.

   .. code-block:: r
      
      library(Rcpp)
      evalCpp("IntegerVector(10, 2.1)")  ## type conversion
      ## [1] 2 2 2 2 2 2 2 2 2 2

.. cpp:function:: Vector(const std::string& st)

   Create a vector from a given string, as the example below shows:
   
   .. code-block:: cpp
   
      #include <Rcpp.h>
      using namespace Rcpp;

      // [[Rcpp::export]]
      RObject ex_Vector_string()
      {
          CharacterVector x(std::string("Hello"));
          return x; // a length-one character vector
      }

      /*** R
      ex_Vector_string()
      ## [1] "Hello"
      */
   
   .. warning::
   
      This constructor as well as the next one, are intended to work
      on **CharacterVector**. Vectors of other types might encounter
      errors with these constructors.

.. cpp:function:: Vector(const char* st)

   Ditto.

.. cpp:function:: Vector(const int& size)

   Create a vector of length *size*, and fill it with zeros (of the proper type).

   .. code-block:: r
      
      library(Rcpp)
      evalCpp("NumericVector(10)")
      ## [1] 0 0 0 0 0 0 0 0 0 0

.. cpp:function:: Vector(const Dimension& dims)

   Create a vector with the given dimension, and fill it with zeros. The **Dimension**
   class is defined in ``<Rcpp/Dimension.h>``.
   
   .. code-block:: cpp
   
      #include <Rcpp.h>
      using namespace Rcpp;

      // [[Rcpp::export]]
      RObject ex1_Vector_dims()
      {
          Dimension dim(2, 3, 4);
          // a 2x3x4 array
          return NumericVector(dim);
      }

      /*** R
      ex1_Vector_dims()

      ## , , 1
      ## 
      ##      [,1] [,2] [,3]
      ## [1,]    0    0    0
      ## [2,]    0    0    0
      ## 
      ## , , 2
      ## 
      ##      [,1] [,2] [,3]
      ## [1,]    0    0    0
      ## [2,]    0    0    0
      ## 
      ## , , 3
      ## 
      ##      [,1] [,2] [,3]
      ## [1,]    0    0    0
      ## [2,]    0    0    0
      ## 
      ## , , 4
      ## 
      ##      [,1] [,2] [,3]
      ## [1,]    0    0    0
      ## [2,]    0    0    0

      */

``template <typename U>``

.. cpp:function:: Vector(const Dimension& dims, const U& u)

   Create a vector with the given dimension, and fill it with value *u*. Type will
   be converted if necessary.

   .. code-block:: cpp
   
      #include <Rcpp.h>
      using namespace Rcpp;

      // [[Rcpp::export]]
      RObject ex2_Vector_dims()
      {
          Dimension dim(2, 2, 2);
          // a 2x2x2 array filled with 1
          return NumericVector(dim, 1.0);
      }

      /*** R
      ex2_Vector_dims()

      ## , , 1
      ## 
      ##      [,1] [,2]
      ## [1,]    1    1
      ## [2,]    1    1
      ## 
      ## , , 2
      ## 
      ##      [,1] [,2]
      ## [1,]    1    1
      ## [2,]    1    1

      */

``template <bool NA, typename VEC>``

.. cpp:function:: Vector(const VectorBase<RTYPE, NA, VEC>& other)

   Create a vector from another object that is also derived from the **VectorBase** class.
   Typically *other* is an Rcpp sugar expression, in which case the expression is evaluated
   and copied to the created vector. If *other* is actually a vector of the same element type,
   this function is equivalent to a copy constructor.

``template <bool NA, typename T>``

.. cpp:function:: Vector(const sugar::SingleLogicalResult<NA, T>& obj)

   Create a vector of length 1 from an object of class **SingleLogicalResult**, usually the result
   returned by ``Rcpp::all()`` or ``Rcpp::any()``.

   .. code-block:: cpp
   
      #include <Rcpp.h>
      using namespace Rcpp;

      // [[Rcpp::export]]
      RObject ex_Vector_slr()
      {
          IntegerVector v(5, 2); // length 5, filled with 2
          return LogicalVector(all(v > 1));
      }

      /*** R
      ex_Vector_slr() ## TRUE
      */

.. cpp:function:: Vector(const int& size, Func gen)

   - *size*: length of the vector.
   - *gen*: a function that takes no argument and returns a number of the 
     same type of the vector, with the signature
     
     stored_type **gen**\()
   
   Create a vector of length *size*, and use function *gen* to fill the elements.

   .. code-block:: cpp
   
      #include <Rcpp.h>
      using namespace Rcpp;

      double f() { return 2.0; }
      // [[Rcpp::export]]
      RObject ex0_Vector_gen()
      {
          // a vector of length 10 filled with 2.0
          return NumericVector(10, f);
      }

      /*** R
      ex0_Vector_gen()
      ## [1] 2 2 2 2 2 2 2 2 2 2
      */

``template <typename U1>``

.. cpp:function:: Vector(const int& siz, Func gen, const U1& u1)

   - *gen* is a function with the signature
     
     stored_type **gen**\(U1)

   Create a vector of length *siz*, and fill it with the function call ``gen(u1)``.

   .. code-block:: cpp
   
      #include <Rcpp.h>
      using namespace Rcpp;

      // [[Rcpp::export]]
      RObject ex1_Vector_gen()
      {
          Rcpp::RNGScope scp;
          // 5 exponential random numbers of mean 1
          return NumericVector(5, R::rexp, 1.0);
      }

      /*** R
      set.seed(123)
      ex1_Vector_gen()
      ## [1] 0.84345726 0.57661027 1.32905487 0.03157736 0.05621098
      */

``template <typename U1, typename U2>``

.. cpp:function:: Vector(const int& siz, Func gen, const U1& u1, const U2& u2)

   - *gen* is a function with the signature
     
     stored_type **gen**\(U1, U2)

   Create a vector of length *siz*, and fill it with the function call ``gen(u1, u2)``.

   .. code-block:: cpp
   
      #include <Rcpp.h>
      using namespace Rcpp;

      // [[Rcpp::export]]
      RObject ex2_Vector_gen()
      {
          Rcpp::RNGScope scp;
          // 5 exponential random numbers of mean 1 and sd 0.5
          return NumericVector(5, R::rnorm, 1.0, 0.5);
      }

      /*** R
      set.seed(123)
      ex2_Vector_gen()
      ## [1] 0.7197622 0.8849113 1.7793542 1.0352542 1.0646439
      */

``template <typename U1, typename U2, typename U3>``

.. cpp:function:: Vector(const int& siz, Func gen, const U1& u1, const U2& u2, const U3& u3)

   - *gen* is a function with the signature
     
     stored_type **gen**\(U1, U2, U3)

   Create a vector of length *siz*, and fill it with the function call ``gen(u1, u2, u3)``.

``template <typename InputIterator>``

.. cpp:function:: Vector(InputIterator first, InputIterator last)

   Copy the data between iterators *first* and *last* to the created vector.
   An example:

   .. code-block:: cpp
   
      #include <Rcpp.h>
      using namespace Rcpp;

      // [[Rcpp::export]]
      RObject ex1_Vector_input()
      {
          double src[] = {1.0, 2.0, 3.0, 4.0, 5.0};
          return NumericVector(src, src + 5);
      }

      /*** R
      ex1_Vector_input()
      ## [1] 1 2 3 4 5
      */

``template <typename InputIterator>``

.. cpp:function:: Vector(InputIterator first, InputIterator last, int n)

   Create a vector of length *n*, and copy the data between iterators *first* and *last*
   to the created vector. *n* should be greater than or equal to the distance betwen
   *first* and *last*.

   .. code-block:: cpp
   
      #include <Rcpp.h>
      using namespace Rcpp;

      // [[Rcpp::export]]
      RObject ex2_Vector_input()
      {
          double src[] = {1.0, 2.0, 3.0, 4.0, 5.0};
          // last three values are uninitialized
          return NumericVector(src, src + 5, 8);
      }

      /*** R
      ex2_Vector_input()
      ## [1]  1.000000e+00  2.000000e+00  3.000000e+00  4.000000e+00  5.000000e+00
      ## [6] 2.121996e-314 6.365987e-314           NaN
      */

.. _ctor-trans:

``template <typename InputIterator, typename Func>``

.. cpp:function:: Vector(InputIterator first, InputIterator last, Func func)

   - *func* is a unary function that takes one argument of the type pointed by
     **InputIterator**, and returns a number convertible to the type of the vector.
   
   Apply function *func* to each element in the range [*first*, *last*),
   and use the resulting values to create the vector.

   .. code-block:: cpp
   
      #include <Rcpp.h>
      using namespace Rcpp;

      // [[Rcpp::export]]
      RObject ex3_Vector_input()
      {
          double src[] = {-1.0, 2.0, -3.0, -4.0, 5.0};
          return NumericVector(src, src + 5, fabs);
      }

      /*** R
      ex3_Vector_input()
      ## [1] 1 2 3 4 5
      */

``template <typename InputIterator, typename Func>``

.. cpp:function:: Vector(InputIterator first, InputIterator last, Func func, int n)

   - *func* is a unary function that takes one argument of the type pointed by
     **InputIterator**, and returns a number convertible to the type of the vector.
   
   Create a vector of length *n*, and fill the first few elements using the rule
   as in the previous constructor. *n* should be greater than or equal to the
   distance between *first* and *last*.



