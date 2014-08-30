Function
=====================================

The **Function** class is the wrapper of R functions. An object from **Function** class
can be "called" by the parenthesis operator ``()``, with up to 20 arguments.

Public Member Functions
-------------------------

Constructors
~~~~~~~~~~~~~~

.. cpp:function:: Function(const Function& other)

   Copy constructor. Resulting object will share the SEXP data with *other*.

::

   template <typename Proxy>

.. cpp:function:: Function(const GenericProxy<Proxy>& proxy)

   Create object from a proxy, such as attribute, slot, field, etc.

.. cpp:function:: Function(SEXP x)

   Wrap a given R function.

.. cpp:function:: Function(const std::string& name)

   Look for an R function by its name. The search starts from the global environment,
   and then recursively goes to the parents.

Defined in **Function**
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. cpp:function:: SEXP environment() const

   Return the enclosing environment (the environment where this function is defined)
   of this function.

.. cpp:function:: SEXP body() const

   Return the body of this function.
   
.. cpp:function:: SEXP operator()() const

   Call the function with no argument.

::
   
   template <typename T1>

.. cpp:function:: SEXP operator()(const T1& t1) const

   Call the function with argument *t1*.

::
   
   template <typename T1, typename T2>

.. cpp:function:: SEXP operator()(const T1& t1, const T2& t2) const

   Call the function with arguments *t1* and *t2*.

::
   
   template <...>

.. cpp:function:: SEXP operator()(...) const

   Call the function with up to 20 arguments. The arguments passed to this function
   should be "wrappable".

Inherited from other classes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

See :doc:`RObject`.

