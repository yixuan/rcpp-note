Vector (Member Functions)
=====================================

Public Member Functions
-------------------------

Defined  in **Vector**
~~~~~~~~~~~~~~~~~~~~~~~

.. cpp:function:: R_len_t length() const

   Return the length of the vector.

   .. code-block:: cpp

      #include <Rcpp.h>
      using namespace Rcpp;
      
      // [[Rcpp::export]]
      int ex_Vector_length(NumericVector x)
      {
          return x.length();
      }
      
      /*** R
      
      ex_Vector_length(rnorm(5))  ## 5
      
      */      

.. cpp:function:: R_len_t size() const

   Return the length of the vector, alias of ``length()``.

.. cpp:function:: size_t offset(const size_t& i, const size_t& j) const

   When this is actually a matrix, give the offset (i.e., the index) of the element
   in row *i* and column *j* (both 0-based).

   .. code-block:: cpp

      #include <Rcpp.h>
      using namespace Rcpp;
      
      // [[Rcpp::export]]
      int ex1_Vector_offset(NumericVector x, int i, int j)
      {
          return x.offset(i - 1, j - 1) + 1;
      }
      
      /*** R
      
      set.seed(123)
      m = matrix(rnorm(24), 4, 6)
      ex1_Vector_offset(m, 2, 4)  ## 14
      m[2, 4]  ## 0.110682
      m[ex1_Vector_offset(m, 2, 4)]  ## 0.110682
      
      */

.. cpp:function:: size_t offset(const size_t& i) const
   
   Give the offset of the *i*-th element with bound check. 0-based.

   .. code-block:: cpp

      #include <Rcpp.h>
      using namespace Rcpp;
      
      // [[Rcpp::export]]
      int ex2_Vector_offset(NumericVector x, int i)
      {
          return x.offset(i);
      }
      
      /*** R
      
      v = c(10, 20, 30)
      ex2_Vector_offset(v, 1)  ## 1
      ex2_Vector_offset(v, 3)  ## index out of bounds error
      
      */

.. cpp:function:: size_t offset(const std::string& name) const

   Give the offset of the element whose name is *name* in the vector. 0-based.
   
   .. code-block:: cpp
   
      #include <Rcpp.h>
      using namespace Rcpp;
      
      // [[Rcpp::export]]
      int ex3_Vector_offset(NumericVector x)
      {
          return x.offset("a");
      }
      
      /*** R
      
      v1 = c(a = 10, b = 20, 30)
      ex3_Vector_offset(v1)  ## 0
      v2 = c(c = 40, d = 50)
      ex3_Vector_offset(v2)  ## index out of bounds error
      
      */

::
   
   template <typename U>

.. cpp:function:: void fill(const U& u)

   Fill the vector with value *u*.

   .. code-block:: cpp

      #include <Rcpp.h>
      using namespace Rcpp;
      
      // [[Rcpp::export]]
      RObject ex_Vector_fill(NumericVector x)
      {
          NumericVector xx = clone(x);
          xx.fill(2.0);
          return xx;
      }
      
      /*** R
      
      v1 = c(2, 1, 3)
      ex_Vector_fill(v1)
      ## [1] 2 2 2
      
      */

.. cpp:function:: iterator begin()

   Return an iterator pointing to the first element of the vector.

   .. code-block:: cpp

      #include <Rcpp.h>
      using namespace Rcpp;
      
      // [[Rcpp::export]]
      int ex_Vector_iterator(NumericVector x)
      {
          // another way to get the length of a vector
          int len = std::distance(x.begin(), x.end());
          // iterators can be casted to pointers
          double *ptr = x.begin();
          *ptr = 5.0;
          return len;
      }
      
      /*** R
      
      v = c(2, 1, 3)
      ex_Vector_iterator(v)  ## 3
      v
      ## [1] 5 1 3
      
      */

.. cpp:function:: iterator end()

   Return an iterator pointing to the *past-the-end* element of the vector.

.. cpp:function:: const_iterator begin() const

   Return an iterator pointing to the first element of the vector. Read-only.

.. cpp:function:: const_iterator end() const

   Return an iterator pointing to the *past-the-end* element of the vector. Read-only.

.. cpp:function:: Proxy operator[](int i)

   Get the reference of the *i*-th element (0-based) of the vector **without**
   bound check.

   .. code-block:: cpp

      #include <Rcpp.h>
      using namespace Rcpp;
      
      // [[Rcpp::export]]
      double ex1_Vector_indexer(NumericVector x)
      {
          x[0] = 5.0;
          return x[2];
      }
      
      /*** R
      
      v = c(2, 1, 5)
      ex1_Vector_indexer(v)  ## 5
      v
      ## [1] 5 1 5
      
      */

.. cpp:function:: const_Proxy operator[](int i) const

   Get the *i*-th element of the vector **without** bound check. Read-only.
   
.. cpp:function:: Proxy operator()(const size_t& i)
   
   Get the reference of the *i*-th element of the vector **with**
   bound check.

   .. code-block:: cpp

      #include <Rcpp.h>
      using namespace Rcpp;
      
      // [[Rcpp::export]]
      double ex2_Vector_indexer(NumericVector x)
      {
          x(0) = 5.0;
          return x(999);
      }
      
      /*** R
      
      v = c(2, 1, 5)
      ex2_Vector_indexer(v)  ## index out of bounds error
      v
      ## [1] 5 1 5
      
      */
   
.. cpp:function:: const_Proxy operator()(const size_t& i) const

   Get the *i*-th element of the vector **with** bound check. Read-only.

.. cpp:function:: Proxy operator()(const size_t& i, const size_t& j)
   
   When this is a matrix, get the reference of the element in row *i* and
   column *j* (both 0-based).
   
.. cpp:function:: const_Proxy operator()(const size_t& i, const size_t& j) const

   When this is a matrix, get the the element in row *i* and column *j*.

.. cpp:function:: NameProxy operator[](const std::string& name)
   
   Get the reference of the element whose name is *name* in the vector.
   
.. cpp:function:: NameProxy operator()(const std::string& name)

   Ditto.

.. cpp:function:: NameProxy operator[](const std::string& name) const
   
   Get the element whose name is *name* in the vector. Read-only.
   
.. cpp:function:: NameProxy operator()(const std::string& name) const

   Ditto.

.. cpp:function:: operator RObject() const
   
   Convert to **RObject** object.

::

   template <int RHS_RTYPE, bool RHS_NA, typename RHS_T>

.. cpp:function:: SubsetProxy<RTYPE, StoragePolicy, RHS_RTYPE, RHS_NA, RHS_T> operator[](const VectorBase<RHS_RTYPE, RHS_NA, RHS_T>& rhs)

   Use sugar expression to subset the vector.

::

   template <int RHS_RTYPE, bool RHS_NA, typename RHS_T>

.. cpp:function:: const SubsetProxy<RTYPE, StoragePolicy, RHS_RTYPE, RHS_NA, RHS_T> operator[](const VectorBase<RHS_RTYPE, RHS_NA, RHS_T>& rhs) const

   Use sugar expression to subset the vector. Read-only.

.. cpp:function:: Vector& sort()

   Sort the vector in place in increasing order, and return the sorted vector.

::

   template <typename InputIterator>

.. cpp:function:: void assign(InputIterator first, InputIterator last)

   Copy and assign the data between *first* and *last* to this vector. An example:
   
   .. code-block:: cpp
   
      SEXP test_assign()
      {
          Rcpp::NumericVector x(10); // originally of length 10
          double src[] = {1, 2, 3};
          x.assign(src, src + 3);
          return x; // now becomes c(1, 2, 3)
      }

::

   template <typename T>

.. cpp:function:: void push_back(const T& object)

   Append a new element *object* to this vector.

::

   template <typename T>

.. cpp:function:: void push_back(const T& object, const std::string& name)

   Append a new element *object* with name *name* to this vector.

::

   template <typename T>

.. cpp:function:: void push_front(const T& object)

   Add a new element *object* to the front of this vector.

::

   template <typename T>

.. cpp:function:: void push_back(const T& object, const std::string& name)

   Add a new element *object* with name *name* to the front of this vector.

An example for the four functions above:

.. code-block:: cpp

   SEXP add_element()
   {
       Rcpp::NumericVector x(0);
       x.push_back(1);
       x.push_back(10, "ten");
       x.push_front(2);
       x.push_front(9, "nine");
       return x; // c(nine = 9, 2, 1, ten = 10)
   }

::

   template <typename T>

.. cpp:function:: iterator insert(iterator position, const T& object)

   Insert new element *object* before the element pointed to by *position*.
   Return the pointer to the newly added element.

::

   template <typename T>

.. cpp:function:: iterator insert(int position, const T& object)

   Insert new element *object* before the *position*-th element (0-based).
   Return the pointer to the newly added element.
   
   .. code-block:: cpp
      
      SEXP vector_insert()
      {
          Rcpp::NumericVector x(1);
          double *iter = x.begin();
          iter = x.insert(iter, 2);
          x.insert(iter, 3);
          x.insert(1, 2.5);
          return x; // c(3, 2.5, 2, 0)
      }

.. cpp:function:: iterator erase(int position)

   Remove the *position*-th element (0-based).
   Return the pointer to the new location of the element that followed the
   erased element.

.. cpp:function:: iterator erase(iterator position)

   Remove the element pointed to by *position*.
   Return the pointer to the new location of the element that followed the
   erased element.

.. cpp:function:: iterator erase(int first, int last)

   Remove the elements in the range [*first*, *last*).
   Return the pointer to the new location of the element that followed the
   last erased element.

.. cpp:function:: iterator erase(iterator first, iterator last)

   Remove the elements in the range [*first*, *last*).
   Return the pointer to the new location of the element that followed the
   last erased element.
   
   .. code-block:: cpp
      
      SEXP vector_erase()
      {
          using namespace Rcpp;
          NumericVector x = NumericVector::create(1, 2, 3, 4, 5, 6, 7);
          double *iter = x.begin();
          x.erase(iter + 1, iter + 3); // remove 2 and 3
          // now x becomes c(1, 4, 5, 6, 7)
          x.erase(3, 5); // remove 6 and 7
          return x; // c(1, 4, 5)
      }

.. cpp:function:: bool containsElementNamed(const char* target) const

   Whether this vector contains an element with the target name.

.. cpp:function:: int findName(const std::string& name) const

   Find the index of the element whose name is *name* in the vector.

.. cpp:function:: SEXP eval() const

   Evaluate the vector in global environment. It may only make sense for **ExpressionVector**.

.. cpp:function:: SEXP eval(SEXP env) const

   Evaluate the vector in the environment given by *env*. It may only make sense for **ExpressionVector**.

Inherited from **NamesProxyPolicy**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. cpp:function:: NamesProxy names()

   Extract the names of this vector. This can appear in
   the left hand side of assignment.

.. cpp:function:: const_NamesProxy names() const

   Extract the names of this vector. Read-only.

Inherited from other classes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

See :doc:`RObject`.

Static Public Member Functions
-------------------------------

.. cpp:function:: static store_type get_na()

   Return ``NA`` of the same type as elements in the vector.

.. cpp:function:: static bool is_na(stored_type x)

   Test whether *x* is ``NA`` (of the proper type).

::

   template <typename InputIterator>

.. cpp:function:: static Vector import(InputIterator first, InputIterator last)

   Create a vector filled with the data between *first* and *last*.

::

   template <typename InputIterator, typename F>

.. cpp:function:: static Vector import_transform(InputIterator first, InputIterator last, F f)

   A direct call to :ref:`Vector(InputIterator first, InputIterator last, Func func) <ctor-trans>`.

.. cpp:function:: static Vector create()

   Create a vector of zero length.

``template <typename T1>``

.. cpp:function:: static Vector create(const T1& t1)

   Create a vector containing element *t1*.

``template <typename T1, typename T2>``

.. cpp:function:: static Vector create(const T1& t1, const T2& t2)

   Create a vector containing elements *t1* and *t2*.

``template <...>``

.. cpp:function:: static Vector create(...)

   Create a vector containing the arguments passed in, up to 20 elements.

