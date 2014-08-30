RObject
=====================================

**RObject** can be thought of as the base class of all API classes in Rcpp.
(Technically speaking this is even not true, but it's best to think in this way
to understand the role of **RObject**. See next paragraph for explanation.)
It is not intended to be used directly, but rather it provides the basic
structure of other API classes, and defines some member functions that are
available to all derived classes.

Precisely speaking, class **RObject** has no meaningful definition by its own, but
actually inherits from other four super-classes:

- **PreserveStorage**: This class provides the data field of the underlying R structure
  (of type **SEXP**) that actually describes this object in R. It also defines basic
  member functions to get/set that field.
- **SlotProxyPolicy**: Defines member functions to manipulate slots of S4 objects.
- **AttributeProxyPolicy**: Defines member functions to work with attributes of objects.
- **RObjectMethods**: Basic functions that apply to all API classes.

In some sense, the whole functionality of **RObject** is divided into four parts
that are represented by the classes above. In fact, that API classes in Rcpp are
derived from **RObject** is just an illusion -- They actually inherit from these
four basic classes instead. But since **RObject** is almost equivalent to the combination
of them, it is no harm to regard **RObject** as the parent of other API classes.

Public Member Functions
-------------------------

Constructors
~~~~~~~~~~~~~~

.. cpp:function:: RObject()

   Default constructor. Resulting object is a simple wrapper of
   ``R_NilValue`` (``NULL`` in R).

.. cpp:function:: RObject(const RObject& other)

   Copy constructor. Resulting object will share the SEXP data with *other*.

``template <typename Proxy>``

.. cpp:function:: RObject(const GenericProxy<Proxy>& proxy)

   Create object from a proxy, such as attribute, slot, field, etc.

Defined in **RObject**
~~~~~~~~~~~~~~~~~~~~~~~

``template <typename T>``

.. cpp:function:: RObject& operator=(const T& other)

   Assignment operator. The left-hand-side object will share the SEXP data
   of *other*.

Inherited from **PreserveStorage**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. warning::
   
   Member functions ending with two underscores (``__``) are mostly intended for internal use
   (even though they are public), and should be avoided in user programs.

.. cpp:function:: void set__(SEXP x)
   
   Replace the SEXP data field of this object by another one.

.. cpp:function:: SEXP get__() const

   Return the SEXP data field of this object.

.. cpp:function:: SEXP invalidate__()

   Return the SEXP data field of this object, and invalidate this object
   by setting it to be ``R_NilValue``.

``template <typename T>``

.. cpp:function:: T& copy__(const T& other)

   Copy the SEXP data field from another object.

.. cpp:function:: bool inherits(const char* clazz)

   Test whether this object inherits from a given class. Equivalent to the
   R function ``inherits()``.

.. cpp:function:: operator SEXP() const

   Conversion operator to SEXP.

Inherited from **SlotProxyPolicy**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note::

   The object needs to be S4 in order to call the member functions in
   this section.

.. cpp:function:: SlotProxy slot(const std::string& name)

   Extract the object in slot specified by *name*. This can appear in
   the left hand side of assignment.

.. cpp:function:: const_SlotProxy slot(const std::string& name) const

   Extract the object in slot specified by *name*. Read-only.

.. cpp:function:: bool hasSlot(const std::string& name) const

   Whether this object has a slot given by *name*.

Inherited from **AttributeProxyPolicy**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. cpp:function:: AttributeProxy attr(const std::string& name)

   Extract the object asscociated with attribute *name*. This can appear in
   the left hand side of assignment.

.. cpp:function:: const_AttributeProxy attr(const std::string& name) const

   Extract the object asscociated with attribute *name*. Read-only.

.. cpp:function:: std::vector<std::string> attributeNames() const
   
   Return the attribute names of this object.

.. cpp:function:: bool hasAttribute(const std::string& name) const

   Whether this object has an attribute whose name is specified by *name*.

Inherited from **RObjectMethods**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. cpp:function:: bool isNULL() const
   
   Whether this object is ``NULL``.

.. cpp:function:: int sexp_type() const

   Return the internal SEXP type of this object. Possible values are:

   .. code-block:: cpp
   
      typedef enum {
          NILSXP      = 0,    /* nil = NULL */
          SYMSXP      = 1,    /* symbols */
          LISTSXP     = 2,    /* lists of dotted pairs */
          CLOSXP      = 3,    /* closures */
          ENVSXP      = 4,    /* environments */
          PROMSXP     = 5,    /* promises: [un]evaluated closure arguments */
          LANGSXP     = 6,    /* language constructs (special lists) */
          SPECIALSXP  = 7,    /* special forms */
          BUILTINSXP  = 8,    /* builtin non-special forms */
          CHARSXP     = 9,    /* "scalar" string type (internal only)*/
          LGLSXP      = 10,   /* logical vectors */
          INTSXP      = 13,   /* integer vectors */
          REALSXP     = 14,   /* real variables */
          CPLXSXP     = 15,   /* complex variables */
          STRSXP      = 16,   /* string vectors */
          DOTSXP      = 17,   /* dot-dot-dot object */
          ANYSXP      = 18,   /* make "any" args work */
          VECSXP      = 19,   /* generic vectors */
          EXPRSXP     = 20,   /* expressions vectors */
          BCODESXP    = 21,   /* byte code */
          EXTPTRSXP   = 22,   /* external pointer */
          WEAKREFSXP  = 23,   /* weak reference */
          RAWSXP      = 24,   /* raw bytes */
          S4SXP       = 25,   /* S4 non-vector */
          NEWSXP      = 30,   /* fresh node creaed in new page */
          FREESXP     = 31,   /* node released by GC */
          FUNSXP      = 99    /* Closure or Builtin */
      } SEXPTYPE;

.. cpp:function:: bool isObject() const
   
   Whether this object has a "class" attribute.

.. cpp:function:: bool isS4() const
   
   Whether this is an S4 object in R.



