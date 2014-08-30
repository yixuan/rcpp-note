Vector (Typedefs and Constructors)
=====================================

The **Vector** class in Rcpp is a template class that represent various types of
vectors (integer, numeric, character, list, etc.) in R. It's also the foundation of
**Matrix** and higher dimensional array structures. The **Vector** class accepts one
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

   Wrap a given vector. A type conversion will be conducted if types don't match.

``template <typename Proxy>``

.. cpp:function:: Vector(const GenericProxy<Proxy>& proxy)

   Create vector from a proxy, such as attribute, slot, field, etc.

.. cpp:function:: Vector(const no_init& obj)

   Create a vector without initializating the values. An example:
   
   .. code-block:: cpp
      
      SEXP noinit()
      {
          return Rcpp::NumericVector(Rcpp::no_init(10));
      }

.. cpp:function:: Vector(const int& size, const stored_type& u)

   Create a vector of length *size*, and fill it with value *u*.

.. cpp:function:: Vector(const std::string& st)

   ???

.. cpp:function:: Vector(const char* st)

   ???

.. cpp:function:: Vector(const int& size, Func gen)

   - *size*: length of the vector.
   - *gen*: a function that takes no argument and returns a number of the 
     same type of the vector, with the signature
     
     stored_type **gen**\()
   
   Create a vector of length *size*, and use function *gen* to fill the elements.
   An example:

   .. code-block:: cpp
   
      double f() { return 2.0; }
      SEXP vec2()
      {
          // a vector of length 10 filled with 2.0
          return Rcpp::NumericVector(10, f);
      }

.. cpp:function:: Vector(const int& size)

   Create a vector of length *size*, and fill it with zeros (of the proper type).

.. cpp:function:: Vector(const Dimension& dims)

   Create a vector with the given dimension, and fill it with zeros. The **Dimension**
   class is defined in ``<Rcpp/Dimension.h>``. An example:
   
   .. code-block:: cpp
      
      SEXP array3d()
      {
          Rcpp::Dimension dim(2, 3, 4);
          // a 2x3x4 array
          return Rcpp::NumericVector(dim);
      }

``template <typename U>``

.. cpp:function:: Vector(const Dimension& dims, const U& u)

   Create a vector with the given dimension, and fill it with value *u*. Type will
   be converted if necessary.

``template <bool NA, typename VEC>``

.. cpp:function:: Vector(const VectorBase<RTYPE, NA, VEC>& other)

   Create a vector from another object that is also derived from the **VectorBase** class.
   Typically *other* is an Rcpp sugar expression, in which case the expression is evaluated
   and copied to the created vector. If *other* is actually a vector of the same element type,
   this function is equivalent to a copy constructor.

``template <typename U>``

.. cpp:function:: Vector(const int& size, const U& u)

   Create a vector of length *size*, and fill it with value *u*. Type will
   be converted if necessary.

``template <bool NA, typename T>``

.. cpp:function:: Vector(const sugar::SingleLogicalResult<NA, T>& obj)

   Create a vector of length 1 from an object of class **SingleLogicalResult**, usually the result
   returned by ``Rcpp::all()`` or ``Rcpp::any()``.

``template <typename U1>``

.. cpp:function:: Vector(const int& siz, Func gen, const U1& u1)

   - *gen* is a function with the signature
     
     stored_type **gen**\(U1)

   Create a vector of length *siz*, and fill it with the function call ``gen(u1)``.
   An example:
   
   .. code-block:: cpp
      
      SEXP my_rexp()
      {
          Rcpp::RNGScope scp;
          // 10 exponential random numbers of mean 1
          return Rcpp::NumericVector(10, R::rexp, 1.0);
      }

``template <typename U1, typename U2>``

.. cpp:function:: Vector(const int& siz, Func gen, const U1& u1, const U2& u2)

   - *gen* is a function with the signature
     
     stored_type **gen**\(U1, U2)

   Create a vector of length *siz*, and fill it with the function call ``gen(u1, u2)``.
   An example:
   
   .. code-block:: cpp
      
      SEXP my_rnorm()
      {
          Rcpp::RNGScope scp;
          // 10 normal random numbers of mean 1 and sd 0.5
          return Rcpp::NumericVector(10, R::rnorm, 1.0, 0.5);
      }

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
      
      SEXP copy_vec()
      {
          double src[] = {1.0, 2.0, 3.0, 4.0, 5.0};
          return Rcpp::NumericVector(src, src + 5);
      }

``template <typename InputIterator>``

.. cpp:function:: Vector(InputIterator first, InputIterator last, int n)

   Create a vector of length *n*, and copy the data between iterators *first* and *last*
   to the created vector. *n* should be greater than or equal to the distance betwen
   *first* and *last*. An example:
   
   .. code-block:: cpp
      
      SEXP copy_vec2()
      {
          double src[] = {1.0, 2.0, 3.0, 4.0, 5.0};
          // last five values are uninitialized
          return Rcpp::NumericVector(src, src + 5, 10);
      }

``template <typename InputIterator, typename Func>``

.. cpp:function:: Vector(InputIterator first, InputIterator last, Func func)

   - *func* is a unary function that takes one argument of the type pointed by
     **InputIterator**, and returns a number convertible to the type of the vector.
   
   Apply function *func* to each element in the range [*first*, *last*),
   and use the resulting values to create the vector. An example:
   
   .. code-block:: cpp
      
      double dsqrt(double x) { return sqrt(x); }
      SEXP sqrt_init()
      {
          double src[] = {1.0, 2.0, 3.0, 4.0, 5.0};
          return Rcpp::NumericVector(src, src + 5, dsqrt);
      }

``template <typename InputIterator, typename Func>``

.. cpp:function:: Vector(InputIterator first, InputIterator last, Func func, int n)

   - *func* is a unary function that takes one argument of the type pointed by
     **InputIterator**, and returns a number convertible to the type of the vector.
   
   Create a vector of length *n*, and fill the first few elements using the rule
   as in the previous constructor. *n* should be greater than or equal to the
   distance between *first* and *last*.



