Environment
=====================================

Public Member Functions
-------------------------

Constructors
~~~~~~~~~~~~~~

.. cpp:function:: Environment()

   Default constructor. Resulting object is the global environment
   ``R_GlobalEnv`` (``.GlobalEnv`` in R).

.. cpp:function:: Environment(SEXP x)

   Wrap a given environment.

.. cpp:function:: Environment(const std::string& name)

   Get the environment asscociated with the given name, such as ".GlobalEnv"
   and "package:stats". Equivalent to ``as.environment()`` in R.

.. cpp:function:: Environment(int pos)

   Get the environment in the given position (1-based) in search path.
   Equivalent to ``as.environment()`` in R.

Defined in **Environment**
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. cpp:function:: SEXP ls(bool all) const
   
   - *all*: whether to list object whose name begins with a dot (``.``).

   List objects in the environment, equivalent to ``ls()`` in R.

