Vector (Member Functions)
=====================================

Public Member Functions
-------------------------

Defined  in **Vector**
~~~~~~~~~~~~~~~~~~~~~~~

.. cpp:function:: R_len_t length() const

   Return the length of the vector.

.. cpp:function:: R_len_t size() const

   Return the length of the vector, alias of ``length()``.

.. cpp:function:: size_t offset(const size_t& i, const size_t& j) const

   When this is actually a matrix, give the offset (i.e., the index) of the element
   in row *i* and column *j* (both 0-based).

.. cpp:function:: size_t offset(const size_t& i) const
   
   Give the offset of the *i*-th element with bound check.

.. cpp:function:: size_t offset(const std::string& name) const

   Give the offset of the element whose name is *name* in the vector. An example:
   
   .. code-block:: cpp
   
      SEXP test_offset()
      {
          using namespace Rcpp;
          NumericVector x = NumericVector::create(
                                Named("a") = 1.0,
                                Named("b") = 2.0,
                                3.0
                            );
          return wrap(x.offset("b")); // should be 1
      }

::
   
   template <typename U>

.. cpp:function:: void fill(const U& u)

   Fill the vector with value *u*.

.. cpp:function:: iterator begin()

   Return an iterator pointing to the first element of the vector.

.. cpp:function:: iterator end()

   Return an iterator pointing to the *past-the-end* element of the vector.

.. cpp:function:: const_iterator begin() const

   Return an iterator pointing to the first element of the vector. Read-only.

.. cpp:function:: const_iterator end() const

   Return an iterator pointing to the *past-the-end* element of the vector. Read-only.

.. cpp:function:: Proxy operator[](int i)

   Get the reference of the *i*-th element (0-based) of the vector **without**
   bound check.

.. cpp:function:: const_Proxy operator[](int i) const

   Get the *i*-th element of the vector **without** bound check. Read-only.
   
.. cpp:function:: Proxy operator()(const size_t& i)
   
   Get the reference of the *i*-th element of the vector **with**
   bound check.
   
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


Inherited from other classes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

See :doc:`RObject`.

Static Public Member Functions
-------------------------------

.. cpp:function:: static store_type get_na()

   Return ``NA`` of the same type as elements in the vector.

.. cpp:function:: static bool is_na(stored_type x)

   Test whether *x* is ``NA`` (of the proper type).





