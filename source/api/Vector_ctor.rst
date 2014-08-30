Vector (Constructors)
=====================================

Intro

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

   ???

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
          return NumericVector(10, f);
      }

.. cpp:function:: Vector(const int& size)

   Create a vector of length *size*, and fill it with zeros (of the proper type).

.. cpp:function:: Vector(const Dimension& dims)

   Create a vector with the given dimension, and fill it with zeros. The **Dimension**
   class is defined in ``<Rcpp/Dimension.h>``.

``template <typename U>``

.. cpp:function:: Vector(const Dimension& dims, const U& u)

   Create a vector with the given dimension, and fill it with value *u*. Type will
   be converted if necessary.

``template <bool NA, typename VEC>``

.. cpp:function:: Vector(const VectorBase<RTYPE, NA, VEC>& other)

   ???

``template <typename U>``

.. cpp:function:: Vector(const int& size, const U& u)

   Create a vector of length *size*, and fill it with value *u*. Type will
   be converted if necessary.

``template <bool NA, typename T>``

.. cpp:function:: Vector(const sugar::SingleLogicalResult<NA, T>& obj)

   Create a vector from a sugar expression. ???

``template <typename U1>``

.. cpp:function:: Vector(const int& siz, Func gen, const U1& u1)

   - *gen* is a function with the signature
     
     stored_type **gen**\(U1)

   Create a vector of length *siz*, and fill it with the function call ``gen(u1)``.

``template <typename U1, typename U2>``

.. cpp:function:: Vector(const int& siz, Func gen, const U1& u1, const U2& u2)

   - *gen* is a function with the signature
     
     stored_type **gen**\(U1, U2)

   Create a vector of length *siz*, and fill it with the function call ``gen(u1, u2)``.

``template <typename U1, typename U2, typename U3>``

.. cpp:function:: Vector(const int& siz, Func gen, const U1& u1, const U2& u2, const U3& u3)

   - *gen* is a function with the signature
     
     stored_type **gen**\(U1, U2, U3)

   Create a vector of length *siz*, and fill it with the function call ``gen(u1, u2, u3)``.

``template <typename InputIterator>``

.. cpp:function:: Vector(InputIterator first, InputIterator last)

   Copy the data between iterators *first* and *last* to the created vector.

``template <typename InputIterator>``

.. cpp:function:: Vector(InputIterator first, InputIterator last, int n)

   Create a vector of length *n*, and copy the data between iterators *first* and *last*
   to the created vector. *n* should be greater than or equal to the distance betwen
   *first* and *last*.

``template <typename InputIterator, typename Func>``

.. cpp:function:: Vector(InputIterator first, InputIterator last, Func func)

   - *func* is a unary function that takes one argument of the type pointed by
     **InputIterator**, and returns a number convertible to the type of the vector.
   
   Apply function *func* to each element in the range [*first*, *last*),
   and use the resulting values to create the vector.

``template <typename InputIterator, typename Func>``

.. cpp:function:: Vector(InputIterator first, InputIterator last, Func func, int n)

   - *func* is a unary function that takes one argument of the type pointed by
     **InputIterator**, and returns a number convertible to the type of the vector.
   
   Create a vector of length *n*, and fill the first few elements using the rule
   as in the previous constructor. *n* should be greater than or equal to the
   distance between *first* and *last*.



