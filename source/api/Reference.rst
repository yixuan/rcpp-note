Reference
=====================================

The **Reference** class is a wrapper of reference class (RC) objects in R.

Public Member Functions
-------------------------

Constructors
~~~~~~~~~~~~~~

.. cpp:function:: Reference()

   Default constructor. Resulting in ``R_NilValue``.

.. cpp:function:: Reference(const Reference& other)

   Copy constructor. Resulting object will share the SEXP data with *other*.

.. cpp:function:: Reference(SEXP x)

   Wrap a given RC object.

.. cpp:function:: Reference(const std::string& klass)

   Create an RC object of the requested class.

Inherited from **FieldProxyPolicy**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. cpp:function:: FieldProxy field(const std::string& name)

   Extract the field with name *name*. This can appear in
   the left hand side of assignment.

.. cpp:function:: const_FieldProxy field(const std::string& name) const

   Extract the field with name *name*. Read-only.

Inherited from other classes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

See :doc:`RObject`.

