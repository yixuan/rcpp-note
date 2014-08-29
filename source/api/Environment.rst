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

   List objects in the environment, equivalent to ``ls(envir = this, all = all)`` in R.

.. cpp:function:: SEXP get(const std::string& name) const

   Get an object from the environment. If not found, ``R_NilValue`` will be returned.
   
.. cpp:function:: SEXP find(const std::string& name) const

   Get an object from the environment, or from one of its parents.
   If not found, an error will be thrown.

.. cpp:function:: bool exists(const std::string& name) const

   Test whether an object named *name* exists in the environment.

.. cpp:function:: bool assign(const std::string& name, SEXP x) const

   Attemp to assign *x* to *name* in this environment. Return ``true`` if the assignment
   is successful, and throw an error if the binding is locked.

::

  template <typename WRAPPABLE>

.. cpp:function:: bool assign(const std::string& name, const WRAPPABLE x) const

   Wrap and assign. If there is a ``wrap()`` method taking an object of WRAPPABLE type,
   then it is wrapped and the corresponding SEXP data is assigned in the environment.

.. cpp:function:: bool isLocked() const

   Return ``true`` if this environment is locked. Equivalent R function is
   ``environmentIsLocked()``.

.. cpp:function:: bool remove(const std::string& name)

   Remove an object from this environment. Return ``true`` if the removal is successful.
   If the object doesn't exist, an error will be thrown.

.. cpp:function:: void lock(bool bindings = false)

   - *bindings*: whether to lock the bindings of this environment as well.

   Lock this environment. Equivalent R function is ``lockEnvironment()``.

.. cpp:function:: void lockBinding(const std::string& name)

   Lock the given binding in this environment. Equivalent R function is ``lockBinding()``.

.. cpp:function:: void unlockBinding(const std::string& name)

   Unlock the given binding in this environment. Equivalent R function is ``unlockBinding()``.

.. cpp:function:: bool bindingIsLocked(const std::string& name) const

   Return ``true`` if the binding is locked in this environment.
   Equivalent R function is ``bindingIsLocked()``.

.. cpp:function:: bool bindingIsActive(const std::string& name) const

   Return ``true`` if the binding is active in this environment.
   Equivalent R function is ``bindingIsActive()``.

.. cpp:function:: bool is_user_database() const

   Indicates if this is a user defined database.
 

