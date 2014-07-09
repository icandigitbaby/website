[TOC]

# Type Definition #

<br/>
## Data attributes ##

The *MAPIStoreObject* type has two data attributes: a *talloc* context and a *MAPIStore* context, where the latter will hang off the former.

        typedef struct {
          PyObject_HEAD
          TALLOC_CTX                    *mem_ctx;
          struct mapistore_context      *mstore_ctx;
        } MAPIStoreObject;

- - -

> *Regarding the names: The [Python/C API Reference Manual][apiref] strongly discourages the use of the *Py* and *_Py* prefixes. It is necessary to discuss the names so they are both clear and consistent.*

[apiref]: https://docs.python.org/3/c-api/index.html

- - -

<br/>
## `New` Method ##

This method is responsible of creating objects of the type with default initial values. 

        static PyObject *MAPIStore_new(PyTypeObject *type, PyObject *args, PyObject *kwds)

It basically creates a `self` wrapper and initialises its *talloc* and *MAPIStore* contexts by calling their respective initialisation functions. The wrapper is then returned as a `PyObject` pointer. Two arguments can be found at `*args`: `syspath` and the optional argument `path`.
The `mapistore_init` function takes a *talloc* context, a *Samba* *LoadParm* context and a path as arguments. The `syspath` argument is used to initialise the samba LDB, wich will allow the default *LoadParm* parameters to be loaded. If the `path` argument is not provided, a `NULL` pointer can be passed to the `mapistore_init` function.

- - -

> - *Why is `PyObject_New` used as opposed to `(MAPIStoreObject *)type->tp_alloc(type, 0)`? The second appears in the Python reference guide. Same with `PyObject_Del` vs `Py_TYPE(self)->tp_free((PyObject*)self)`.*
- *I'm quite a bit lost with the samba initialisation functions, just copied it hoping they wouldn't cause conflicts. Which is not a good practice.*

- - -

<br/>
## Destructor ##

The deallocation method frees/releases the two Python by calling their respective destructors. Then, it calls `PyObject_Del` to free the object's memory.

        static void MAPIStore_dealloc(PyObject *_self)

## Initialisation Function ##

The `initmapistore` function initialises the type and adds it to the module dictionary, allowing us to create MAPIStore instances by calling its class:

        >>> import mapistore
        >>> mapi = mapistore.MAPIStore()

- - -

> *Review the object creation once the bindings are built*

- - -
<br/>

# Next: Next Chapter #

Proceed to the [Next Chapter](next.html)
