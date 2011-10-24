.. highlightlang:: c


.. _extending-intro:

******************************
用C,C++扩展Python
******************************

It is quite easy to add new built-in modules to Python, if you know how to
program in C.  Such :dfn:`extension modules` can do two things that can't be
done directly in Python: they can implement new built-in object types, and they
can call C library functions and system calls.

为 Python 添加新的内建模块十分简单, 如果你知道如何使用 C 编程的话.
这些 :dfn:`extension modules` 可以做两件不能直接用 Python 做的事:
它们可以实现新的内建对象类型, 并且他们可以调用 C 库函数和系统调用.

To support extensions, the Python API (Application Programmers Interface)
defines a set of functions, macros and variables that provide access to most
aspects of the Python run-time system.  The Python API is incorporated in a C
source file by including the header ``"Python.h"``.

为支持扩展, Python API (应用程序接口) 定义了一系列的函数, 宏和变量来提供 Python
运行时系统的大多数方面的访问. 通过在 C 源文件里包含头文件 ``"Python.h"``,
来包含 Python API.

The compilation of an extension module depends on its intended use as well as on
your system setup; details are given in later chapters.

扩展模块的编译依赖于它的用途以及您系统的设置; 在后面的章节里会给出详细信息.

Do note that if your use case is calling C library functions or system calls,
you should consider using the :mod:`ctypes` module rather than writing custom
C code. Not only does :mod:`ctypes` let you write Python code to interface
with C code, but it is more portable between implementations of Python than
writing and compiling an extension module which typically ties you to CPython.

注意, 如果你使用的情况是调用 C 库函数或者系统调用, 你应当考虑使用 :mod:`ctypes`
模块, 而不是写自定的 C 代码. :mod:`ctypes` 不仅使您可以让 Python 代码使用 C
写的接口, 而且, 相比于编写和编译一个扩展模块从而您与 CPython 联系的方法,
这种实现更具备可移植性.


.. _extending-simpleexample:

简单的例子
================

Let's create an extension module called ``spam`` (the favorite food of Monty
Python fans...) and let's say we want to create a Python interface to the C
library function :c:func:`system`. [#]_ This function takes a null-terminated
character string as argument and returns an integer.  We want this function to
be callable from Python as follows:

让我们来创建一个名为 ``spam`` (Monty Python 粉丝最爱的食物) 的扩展模块,
并且我们想要为 C 库函数 :c:func:`system` 创建一个 Python 接口.
[#]_ 这个函数接受一个以空白字符结尾的字符串作为参数, 并返回一个整数.
我们想要这个函数可以如下地被 Python 调用::

   >>> import spam
   >>> status = spam.system("ls -l")
Begin by creating a file :file:`spammodule.c`.  (Historically, if a module is
called ``spam``, the C file containing its implementation is called
:file:`spammodule.c`; if the module name is very long, like ``spammify``, the
module name can be just :file:`spammify.c`.)

首先创建一个文件, :file:`spammodule.c`. (从历史上看, 如果一个模块名为 ``spam``,
那么包含它的实现的 C 文件就命名为 :file:`spammodule.c`; 如果模块名非常长, 如
``spammify``, 那么可以相应的 C 文件名可以就是 :file:`spammify.c`.)

The first line of our file can be::

   #include <Python.h>

which pulls in the Python API (you can add a comment describing the purpose of
the module and a copyright notice if you like).

我们文件的第一行可以是::

   #include <Python.h>

这样可以包含 Python API （如果喜欢的话, 你可以添加描述该模块目的的注释和版权声明).

.. note::

   Since Python may define some pre-processor definitions which affect the standard
   headers on some systems, you *must* include :file:`Python.h` before any standard
   headers are included.

   因为在一些系统上, Python 可能定义了一些影响标准头文件的预处理器定义, 因此,
   你*必须*在任何包含标准头文件之前包含 :file:`Python.h`.

All user-visible symbols defined by :file:`Python.h` have a prefix of ``Py`` or
``PY``, except those defined in standard header files. For convenience, and
since they are used extensively by the Python interpreter, ``"Python.h"``
includes a few standard header files: ``<stdio.h>``, ``<string.h>``,
``<errno.h>``, and ``<stdlib.h>``.  If the latter header file does not exist on
your system, it declares the functions :c:func:`malloc`, :c:func:`free` and
:c:func:`realloc` directly.

除了在标准头文件里定义的符号, :file:`Python.h` 定义的所有用户可见符号都有一个前缀,
``Py`` 或 ``PY``. 为了方便， 而且因为它们被 Python 解释器广泛地使用, ``"Python.h"``
包含几个标准头文件: ``<stdio.h>``, ``string.h``, ``<errno.h``, 和 ``<stdlib.h>``.
如果后面的头文件在你的系统里不存在的话, 它会直接地声明函数 :c:func:`malloc`,
:c:func:`free` 和 :c:func:`realloc`.

The next thing we add to our module file is the C function that will be called
when the Python expression ``spam.system(string)`` is evaluated (we'll see
shortly how it ends up being called)::

   static PyObject *
   spam_system(PyObject *self, PyObject *args)
   {
       const char *command;
       int sts;

       if (!PyArg_ParseTuple(args, "s", &command))
           return NULL;
       sts = system(command);
       return PyLong_FromLong(sts);
   }

There is a straightforward translation from the argument list in Python (for
example, the single expression ``"ls -l"``) to the arguments passed to the C
function.  The C function always has two arguments, conventionally named *self*
and *args*.

从 Python 中的参数列表 (本例中, 简单的表达式 ``"ls -l"``) 到传递给 C
函数的参数之间有一个明确的转换. C 函数通常有两个参数, 约定命名为 *self* 和 *args*.

The *self* argument points to the module object for module-level functions;
for a method it would point to the object instance.

对于模块级别的函数, 参数 *self* 指向这个模块对象; 而对于方法, 它将指向这个对象实例.

The *args* argument will be a pointer to a Python tuple object containing the
arguments.  Each item of the tuple corresponds to an argument in the call's
argument list.  The arguments are Python objects --- in order to do anything
with them in our C function we have to convert them to C values.  The function
:c:func:`PyArg_ParseTuple` in the Python API checks the argument types and
converts them to C values.  It uses a template string to determine the required
types of the arguments as well as the types of the C variables into which to
store the converted values.  More about this later.

参数 *args* 是一个指针, 它指向一个包含参数的 Python 元组对象.
该元组里的每一项对应于调用的参数表里的一个参数. 这些参数是 Python 对象 ---
为了能在我们的 C 函数里对它们做任何事, 我们必须把它们转化为 C 值.
包含于 Python API 的函数 :c:func:`PyArg_ParseTuple` 检查参数的类型并把它们转化为 C 值.
它使用一个模板字符串来确定参数所需的类型, 以及储存转换后的值的 C 变量的类型.
后面会有更多相关内容.

:c:func:`PyArg_ParseTuple` returns true (nonzero) if all arguments have the right
type and its components have been stored in the variables whose addresses are
passed.  It returns false (zero) if an invalid argument list was passed.  In the
latter case it also raises an appropriate exception so the calling function can
return *NULL* immediately (as we saw in the example).

:c:func:`PyArg_ParseTuple` 在所有参数的类型正确, 并且它的组件被储存在地址被传递的变量里的情况下,
会返回真 (非零值). 如果被传递一个无效的参数表, 它返回假 (0).
后一种情况, 它也抛出一个适当的异常, 因此调用函数可以立即返回 *NULL* (就如我们在这个例子里看到的). 


.. _extending-errors:

Intermezzo: 错误和异常
=================================

An important convention throughout the Python interpreter is the following: when
a function fails, it should set an exception condition and return an error value
(usually a *NULL* pointer).  Exceptions are stored in a static global variable
inside the interpreter; if this variable is *NULL* no exception has occurred.  A
second global variable stores the "associated value" of the exception (the
second argument to :keyword:`raise`).  A third variable contains the stack
traceback in case the error originated in Python code.  These three variables
are the C equivalents of the result in Python of :meth:`sys.exc_info` (see the
section on module :mod:`sys` in the Python Library Reference).  It is important
to know about them to understand how errors are passed around.

有一个重要的约定贯穿整个 Python 解释器: 当一个函数失败时, 它应当设置一个异常条件,
并且返回一个错误值 (通常是一个 *NULL* 指针). 异常存储在解释器内部的一个静态全局变量里;
如果这个值为 *NULL*, 那么没有发生任何异常. 第二个全局变量存储了异常的 "相关值"
(传递给 :keyword:`raise` 的第二个参数). 如果错误的源头在 Python 代码里, 那么第三个变量包含堆栈跟踪.
这三个变量是 Python 中 :meth:`sys.exc_info` (参阅 Python 库参考关于 :mod:`sys` 模块的部分)
的结果等价. 了解这些对于理解错误如何到处传递是重要的.

The Python API defines a number of functions to set various types of exceptions.

Python API 定义了一些函数来设置异常的各种类型.

The most common one is :c:func:`PyErr_SetString`.  Its arguments are an exception
object and a C string.  The exception object is usually a predefined object like
:c:data:`PyExc_ZeroDivisionError`.  The C string indicates the cause of the error
and is converted to a Python string object and stored as the "associated value"
of the exception.

最常见的一个是 :c:func:`PyErr_SetString`.  它的参数是一个异常对象和一个 C 字符串.
异常对象通常是一个预先定义的对象, 如 :c:data:`PyExc_ZeroDivisionError`. C
字符串表明错误的起因, 它被转换为一个 Python 字符串对象, 并存作异常的 "相关值".

Another useful function is :c:func:`PyErr_SetFromErrno`, which only takes an
exception argument and constructs the associated value by inspection of the
global variable :c:data:`errno`.  The most general function is
:c:func:`PyErr_SetObject`, which takes two object arguments, the exception and
its associated value.  You don't need to :c:func:`Py_INCREF` the objects passed
to any of these functions.

另一个有用的函数是 :c:func:`PyErr_SetFromErrno`, 它只需要一个异常参数, 
并且通过全局变量 :c:data:`errno` 构造相关值. 最为一般的函数是 :c:func:`PyErr_SetObject`,
它需要两个参数, 异常以及它的相关值. 你不需要 :c:func:`Py_INCREF` 被传递给这些函数的对象.

You can test non-destructively whether an exception has been set with
:c:func:`PyErr_Occurred`.  This returns the current exception object, or *NULL*
if no exception has occurred.  You normally don't need to call
:c:func:`PyErr_Occurred` to see whether an error occurred in a function call,
since you should be able to tell from the return value.

无论是否已经设置了一个异常, 你都可以使用 :c:func:`PyErr_Occurred` 来无破坏的测试.
这会返回当前的异常对象, 如果没有异常发生就返回 *NULL*, 通常, 你不需要调用
:c:func:`PyErr_Occurred` 来查看是否在一次函数调用中发生了错误,
因为你应该可以从返回值中知道这点.

When a function *f* that calls another function *g* detects that the latter
fails, *f* should itself return an error value (usually *NULL* or ``-1``).  It
should *not* call one of the :c:func:`PyErr_\*` functions --- one has already
been called by *g*. *f*'s caller is then supposed to also return an error
indication to *its* caller, again *without* calling :c:func:`PyErr_\*`, and so on
--- the most detailed cause of the error was already reported by the function
that first detected it.  Once the error reaches the Python interpreter's main
loop, this aborts the currently executing Python code and tries to find an
exception handler specified by the Python programmer.

当函数 *f* 调用函数 *g*, *g* 失败了, *f* 自己应当返回一个错误值 (通常是 *NULL* 或 ``-1``).
不应当调用 :c:func:`PyErr_\*` 中的任何一个 --- 有一个已经被 *g* 调用.
*f* 的调用者被假定也返回一个错误指示给*它的*调用者, *没有*调用 :c:func:`PyErr_\*`,
等等 --- 错误最为详细的起因已经被第一个检测到它的函数报告了. 一旦错误到达了 Python
解释器的主循环, 这会中止当前 Python 代码的执行, 并且尝试寻找由 Python 程序员指定的异常处理器.

(There are situations where a module can actually give a more detailed error
message by calling another :c:func:`PyErr_\*` function, and in such cases it is
fine to do so.  As a general rule, however, this is not necessary, and can cause
information about the cause of the error to be lost: most operations can fail
for a variety of reasons.)

有一些情况，一个模块实际上能通过调用其他PyErr_*函数给出错误的更多细节信息，在这些情形下，调用其他函数是很好的。
然而，按照惯例，上述这种做法并不需要，它可能会导致引起错误原因的信息丢失：绝大部分操作都可能由于各种各样的原因而失败。

To ignore an exception set by a function call that failed, the exception
condition must be cleared explicitly by calling :c:func:`PyErr_Clear`.  The only
time C code should call :c:func:`PyErr_Clear` is if it doesn't want to pass the
error on to the interpreter but wants to handle it completely by itself
(possibly by trying something else, or pretending nothing went wrong).

为了忽略一个失败的函数调用而导致的异常，这种异常情况必须保证通过调用PyErr_Clear函数被清除，C语言只有在它不想将错误传递给解释器而想完全自主的控制它的情况下才需调用Python_Clear。

Every failing :c:func:`malloc` call must be turned into an exception --- the
direct caller of :c:func:`malloc` (or :c:func:`realloc`) must call
:c:func:`PyErr_NoMemory` and return a failure indicator itself.  All the
object-creating functions (for example, :c:func:`PyLong_FromLong`) already do
this, so this note is only relevant to those who call :c:func:`malloc` directly.

每一个内存分配（malloc）调用的失败必须产生一个异常----直接内存分配的调用者必须调用PyErr_NoMemory并返回一个失败提示。所有的对象创建函数（如PyInt_FromLong）已经做到了这一点，所以这条规则是和那些想直接调用内存分配（malloc）有关的。

Also note that, with the important exception of :c:func:`PyArg_ParseTuple` and
friends, functions that return an integer status usually return a positive value
or zero for success and ``-1`` for failure, like Unix system calls.

还需注意的是，通过PyArg_ParseTuple、函数，若返回一个正整数或0则表示成功，而-1表示失败，这就像Unix操作系统中的调用。

Finally, be careful to clean up garbage (by making :c:func:`Py_XDECREF` or
:c:func:`Py_DECREF` calls for objects you have already created) when you return
an error indicator!

最后，当你返回一个错误指示时小心清除垃圾（通过执行对已创建对象的调用函数Py_XDECREF或Py_DECREF）

The choice of which exception to raise is entirely yours.  There are predeclared
C objects corresponding to all built-in Python exceptions, such as
:c:data:`PyExc_ZeroDivisionError`, which you can use directly. Of course, you
should choose exceptions wisely --- don't use :c:data:`PyExc_TypeError` to mean
that a file couldn't be opened (that should probably be :c:data:`PyExc_IOError`).
If something's wrong with the argument list, the :c:func:`PyArg_ParseTuple`
function usually raises :c:data:`PyExc_TypeError`.  If you have an argument whose
value must be in a particular range or must satisfy other conditions,
:c:data:`PyExc_ValueError` is appropriate.

是否去引发异常完全由你的选择，对于Python内建异常，都有与之对应的C语言对象，如PyExc_TypeError表示文件无法打开
(这很可能是PyExc_IOError)。如果参数列表出现了问题，PyArg_ParseTuple函数通常会引发PyExc_TypeError。而若是一个参数的值必须在一定范围内或者必须满足其他条件，便引发PyExc_ValueError。

You can also define a new exception that is unique to your module. For this, you
usually declare a static object variable at the beginning of your file:

你也可以定义一些新的对于你的模块来说是唯一的异常。这样，你通常会在源文件的开头部分声明一个静态对象变量::

   static PyObject *SpamError;

and initialize it in your module's initialization function (:c:func:`PyInit_spam`)
with an exception object (leaving out the error checking for now):

并在你的模块初始化函数（initspam）中用一个异常对象初始化它（暂时不考虑错误检查）::

   PyMODINIT_FUNC
   PyInit_spam(void)
   {
       PyObject *m;

       m = PyModule_Create(&spammodule);
       if (m == NULL)
           return NULL;

       SpamError = PyErr_NewException("spam.error", NULL, NULL);
       Py_INCREF(SpamError);
       PyModule_AddObject(m, "error", SpamError);
       return m;
   }

Note that the Python name for the exception object is :exc:`spam.error`.  The
:c:func:`PyErr_NewException` function may create a class with the base class
being :exc:`Exception` (unless another class is passed in instead of *NULL*),
described in :ref:`bltin-exceptions`.

注意这个异常对象的Python名是spam.error。而PyErr_NewException函数会用基础异常类建立一个类（除非有另外一个类已经换作NULL通过），这是内建异常中所描述的。

Note also that the :c:data:`SpamError` variable retains a reference to the newly
created exception class; this is intentional!  Since the exception could be
removed from the module by external code, an owned reference to the class is
needed to ensure that it will not be discarded, causing :c:data:`SpamError` to
become a dangling pointer. Should it become a dangling pointer, C code which
raises the exception could cause a core dump or other unintended side effects.

还需注意的是，SpamError变量会保留一个对新创建异常类的引用，而这是有深意的。因为异常可以被外部函数从模块中移除掉，而这个类的引用需要用来保证它不会被抛弃（译者注：即失去对该异常类的跟踪），导致其为一个悬指针，引发该异常的C语言代码会引起存储器清0或其他意想不到的副作用(side effects)。
We discuss the use of PyMODINIT_FUNC as a function return type later in this sample.
接下来我们讨论这个例子(sample)中的作为函数返回类型的PyMODINIT_FUNC的用法。

We discuss the use of ``PyMODINIT_FUNC`` as a function return type later in this
sample.

The :exc:`spam.error` exception can be raised in your extension module using a
call to :c:func:`PyErr_SetString` as shown below::

   static PyObject *
   spam_system(PyObject *self, PyObject *args)
   {
       const char *command;
       int sts;

       if (!PyArg_ParseTuple(args, "s", &command))
           return NULL;
       sts = system(command);
       if (sts < 0) {
           PyErr_SetString(SpamError, "System command failed");
           return NULL;
       }
       return PyLong_FromLong(sts);
   }


.. _backtoexample:

回到例子中
===================

Going back to our example function, you should now be able to understand this
statement::

   if (!PyArg_ParseTuple(args, "s", &command))
       return NULL;

It returns *NULL* (the error indicator for functions returning object pointers)
if an error is detected in the argument list, relying on the exception set by
:c:func:`PyArg_ParseTuple`.  Otherwise the string value of the argument has been
copied to the local variable :c:data:`command`.  This is a pointer assignment and
you are not supposed to modify the string to which it points (so in Standard C,
the variable :c:data:`command` should properly be declared as ``const char
*command``).

The next statement is a call to the Unix function :c:func:`system`, passing it
the string we just got from :c:func:`PyArg_ParseTuple`::

   sts = system(command);

Our :func:`spam.system` function must return the value of :c:data:`sts` as a
Python object.  This is done using the function :c:func:`PyLong_FromLong`. ::

   return PyLong_FromLong(sts);

In this case, it will return an integer object.  (Yes, even integers are objects
on the heap in Python!)

If you have a C function that returns no useful argument (a function returning
:c:type:`void`), the corresponding Python function must return ``None``.   You
need this idiom to do so (which is implemented by the :c:macro:`Py_RETURN_NONE`
macro)::

   Py_INCREF(Py_None);
   return Py_None;

:c:data:`Py_None` is the C name for the special Python object ``None``.  It is a
genuine Python object rather than a *NULL* pointer, which means "error" in most
contexts, as we have seen.


.. _methodtable:

模块方法列表和初始化函数
=====================================================

I promised to show how :c:func:`spam_system` is called from Python programs.
First, we need to list its name and address in a "method table"::

   static PyMethodDef SpamMethods[] = {
       ...
       {"system",  spam_system, METH_VARARGS,
        "Execute a shell command."},
       ...
       {NULL, NULL, 0, NULL}        /* Sentinel */
   };

Note the third entry (``METH_VARARGS``).  This is a flag telling the interpreter
the calling convention to be used for the C function.  It should normally always
be ``METH_VARARGS`` or ``METH_VARARGS | METH_KEYWORDS``; a value of ``0`` means
that an obsolete variant of :c:func:`PyArg_ParseTuple` is used.

When using only ``METH_VARARGS``, the function should expect the Python-level
parameters to be passed in as a tuple acceptable for parsing via
:c:func:`PyArg_ParseTuple`; more information on this function is provided below.

The :const:`METH_KEYWORDS` bit may be set in the third field if keyword
arguments should be passed to the function.  In this case, the C function should
accept a third ``PyObject \*`` parameter which will be a dictionary of keywords.
Use :c:func:`PyArg_ParseTupleAndKeywords` to parse the arguments to such a
function.

The method table must be referenced in the module definition structure::

   static struct PyModuleDef spammodule = {
      PyModuleDef_HEAD_INIT,
      "spam",   /* name of module */
      spam_doc, /* module documentation, may be NULL */
      -1,       /* size of per-interpreter state of the module,
                   or -1 if the module keeps state in global variables. */
      SpamMethods
   };

This structure, in turn, must be passed to the interpreter in the module's
initialization function.  The initialization function must be named
:c:func:`PyInit_name`, where *name* is the name of the module, and should be the
only non-\ ``static`` item defined in the module file::

   PyMODINIT_FUNC
   PyInit_spam(void)
   {
       return PyModule_Create(&spammodule);
   }

Note that PyMODINIT_FUNC declares the function as ``PyObject *`` return type,
declares any special linkage declarations required by the platform, and for C++
declares the function as ``extern "C"``.

When the Python program imports module :mod:`spam` for the first time,
:c:func:`PyInit_spam` is called. (See below for comments about embedding Python.)
It calls :c:func:`PyModule_Create`, which returns a module object, and
inserts built-in function objects into the newly created module based upon the
table (an array of :c:type:`PyMethodDef` structures) found in the module definition.
:c:func:`PyModule_Create` returns a pointer to the module object
that it creates.  It may abort with a fatal error for
certain errors, or return *NULL* if the module could not be initialized
satisfactorily. The init function must return the module object to its caller,
so that it then gets inserted into ``sys.modules``.

When embedding Python, the :c:func:`PyInit_spam` function is not called
automatically unless there's an entry in the :c:data:`PyImport_Inittab` table.
To add the module to the initialization table, use :c:func:`PyImport_AppendInittab`,
optionally followed by an import of the module::

   int
   main(int argc, char *argv[])
   {
       /* Add a built-in module, before Py_Initialize */
       PyImport_AppendInittab("spam", PyInit_spam);

       /* Pass argv[0] to the Python interpreter */
       Py_SetProgramName(argv[0]);

       /* Initialize the Python interpreter.  Required. */
       Py_Initialize();

       /* Optionally import the module; alternatively,
          import can be deferred until the embedded script
          imports it. */
       PyImport_ImportModule("spam");

An example may be found in the file :file:`Demo/embed/demo.c` in the Python
source distribution.

.. note::

   Removing entries from ``sys.modules`` or importing compiled modules into
   multiple interpreters within a process (or following a :c:func:`fork` without an
   intervening :c:func:`exec`) can create problems for some extension modules.
   Extension module authors should exercise caution when initializing internal data
   structures.

A more substantial example module is included in the Python source distribution
as :file:`Modules/xxmodule.c`.  This file may be used as a  template or simply
read as an example.


.. _compilation:

编译和链接
=======================

There are two more things to do before you can use your new extension: compiling
and linking it with the Python system.  If you use dynamic loading, the details
may depend on the style of dynamic loading your system uses; see the chapters
about building extension modules (chapter :ref:`building`) and additional
information that pertains only to building on Windows (chapter
:ref:`building-on-windows`) for more information about this.

If you can't use dynamic loading, or if you want to make your module a permanent
part of the Python interpreter, you will have to change the configuration setup
and rebuild the interpreter.  Luckily, this is very simple on Unix: just place
your file (:file:`spammodule.c` for example) in the :file:`Modules/` directory
of an unpacked source distribution, add a line to the file
:file:`Modules/Setup.local` describing your file::

   spam spammodule.o

and rebuild the interpreter by running :program:`make` in the toplevel
directory.  You can also run :program:`make` in the :file:`Modules/`
subdirectory, but then you must first rebuild :file:`Makefile` there by running
':program:`make` Makefile'.  (This is necessary each time you change the
:file:`Setup` file.)

If your module requires additional libraries to link with, these can be listed
on the line in the configuration file as well, for instance::

   spam spammodule.o -lX11


.. _callingpython:

用C来调用Python函数
===============================

So far we have concentrated on making C functions callable from Python.  The
reverse is also useful: calling Python functions from C. This is especially the
case for libraries that support so-called "callback" functions.  If a C
interface makes use of callbacks, the equivalent Python often needs to provide a
callback mechanism to the Python programmer; the implementation will require
calling the Python callback functions from a C callback.  Other uses are also
imaginable.

Fortunately, the Python interpreter is easily called recursively, and there is a
standard interface to call a Python function.  (I won't dwell on how to call the
Python parser with a particular string as input --- if you're interested, have a
look at the implementation of the :option:`-c` command line option in
:file:`Modules/main.c` from the Python source code.)

Calling a Python function is easy.  First, the Python program must somehow pass
you the Python function object.  You should provide a function (or some other
interface) to do this.  When this function is called, save a pointer to the
Python function object (be careful to :c:func:`Py_INCREF` it!) in a global
variable --- or wherever you see fit. For example, the following function might
be part of a module definition::

   static PyObject *my_callback = NULL;

   static PyObject *
   my_set_callback(PyObject *dummy, PyObject *args)
   {
       PyObject *result = NULL;
       PyObject *temp;

       if (PyArg_ParseTuple(args, "O:set_callback", &temp)) {
           if (!PyCallable_Check(temp)) {
               PyErr_SetString(PyExc_TypeError, "parameter must be callable");
               return NULL;
           }
           Py_XINCREF(temp);         /* Add a reference to new callback */
           Py_XDECREF(my_callback);  /* Dispose of previous callback */
           my_callback = temp;       /* Remember new callback */
           /* Boilerplate to return "None" */
           Py_INCREF(Py_None);
           result = Py_None;
       }
       return result;
   }

This function must be registered with the interpreter using the
:const:`METH_VARARGS` flag; this is described in section :ref:`methodtable`.  The
:c:func:`PyArg_ParseTuple` function and its arguments are documented in section
:ref:`parsetuple`.

The macros :c:func:`Py_XINCREF` and :c:func:`Py_XDECREF` increment/decrement the
reference count of an object and are safe in the presence of *NULL* pointers
(but note that *temp* will not be  *NULL* in this context).  More info on them
in section :ref:`refcounts`.

.. index:: single: PyObject_CallObject()

Later, when it is time to call the function, you call the C function
:c:func:`PyObject_CallObject`.  This function has two arguments, both pointers to
arbitrary Python objects: the Python function, and the argument list.  The
argument list must always be a tuple object, whose length is the number of
arguments.  To call the Python function with no arguments, pass in NULL, or
an empty tuple; to call it with one argument, pass a singleton tuple.
:c:func:`Py_BuildValue` returns a tuple when its format string consists of zero
or more format codes between parentheses.  For example::

   int arg;
   PyObject *arglist;
   PyObject *result;
   ...
   arg = 123;
   ...
   /* Time to call the callback */
   arglist = Py_BuildValue("(i)", arg);
   result = PyObject_CallObject(my_callback, arglist);
   Py_DECREF(arglist);

:c:func:`PyObject_CallObject` returns a Python object pointer: this is the return
value of the Python function.  :c:func:`PyObject_CallObject` is
"reference-count-neutral" with respect to its arguments.  In the example a new
tuple was created to serve as the argument list, which is :c:func:`Py_DECREF`\
-ed immediately after the call.

The return value of :c:func:`PyObject_CallObject` is "new": either it is a brand
new object, or it is an existing object whose reference count has been
incremented.  So, unless you want to save it in a global variable, you should
somehow :c:func:`Py_DECREF` the result, even (especially!) if you are not
interested in its value.

Before you do this, however, it is important to check that the return value
isn't *NULL*.  If it is, the Python function terminated by raising an exception.
If the C code that called :c:func:`PyObject_CallObject` is called from Python, it
should now return an error indication to its Python caller, so the interpreter
can print a stack trace, or the calling Python code can handle the exception.
If this is not possible or desirable, the exception should be cleared by calling
:c:func:`PyErr_Clear`.  For example::

   if (result == NULL)
       return NULL; /* Pass error back */
   ...use result...
   Py_DECREF(result);

Depending on the desired interface to the Python callback function, you may also
have to provide an argument list to :c:func:`PyObject_CallObject`.  In some cases
the argument list is also provided by the Python program, through the same
interface that specified the callback function.  It can then be saved and used
in the same manner as the function object.  In other cases, you may have to
construct a new tuple to pass as the argument list.  The simplest way to do this
is to call :c:func:`Py_BuildValue`.  For example, if you want to pass an integral
event code, you might use the following code::

   PyObject *arglist;
   ...
   arglist = Py_BuildValue("(l)", eventcode);
   result = PyObject_CallObject(my_callback, arglist);
   Py_DECREF(arglist);
   if (result == NULL)
       return NULL; /* Pass error back */
   /* Here maybe use the result */
   Py_DECREF(result);

Note the placement of ``Py_DECREF(arglist)`` immediately after the call, before
the error check!  Also note that strictly speaking this code is not complete:
:c:func:`Py_BuildValue` may run out of memory, and this should be checked.

You may also call a function with keyword arguments by using
:c:func:`PyObject_Call`, which supports arguments and keyword arguments.  As in
the above example, we use :c:func:`Py_BuildValue` to construct the dictionary. ::

   PyObject *dict;
   ...
   dict = Py_BuildValue("{s:i}", "name", val);
   result = PyObject_Call(my_callback, NULL, dict);
   Py_DECREF(dict);
   if (result == NULL)
       return NULL; /* Pass error back */
   /* Here maybe use the result */
   Py_DECREF(result);


.. _parsetuple:

从外部函数中提取参数
============================================

.. index:: single: PyArg_ParseTuple()

The :c:func:`PyArg_ParseTuple` function is declared as follows::

   int PyArg_ParseTuple(PyObject *arg, char *format, ...);

The *arg* argument must be a tuple object containing an argument list passed
from Python to a C function.  The *format* argument must be a format string,
whose syntax is explained in :ref:`arg-parsing` in the Python/C API Reference
Manual.  The remaining arguments must be addresses of variables whose type is
determined by the format string.

Note that while :c:func:`PyArg_ParseTuple` checks that the Python arguments have
the required types, it cannot check the validity of the addresses of C variables
passed to the call: if you make mistakes there, your code will probably crash or
at least overwrite random bits in memory.  So be careful!

Note that any Python object references which are provided to the caller are
*borrowed* references; do not decrement their reference count!

Some example calls::

   #define PY_SSIZE_T_CLEAN  /* Make "s#" use Py_ssize_t rather than int. */
   #include <Python.h>

::

   int ok;
   int i, j;
   long k, l;
   const char *s;
   Py_ssize_t size;

   ok = PyArg_ParseTuple(args, ""); /* No arguments */
       /* Python call: f() */

::

   ok = PyArg_ParseTuple(args, "s", &s); /* A string */
       /* Possible Python call: f('whoops!') */

::

   ok = PyArg_ParseTuple(args, "lls", &k, &l, &s); /* Two longs and a string */
       /* Possible Python call: f(1, 2, 'three') */

::

   ok = PyArg_ParseTuple(args, "(ii)s#", &i, &j, &s, &size);
       /* A pair of ints and a string, whose size is also returned */
       /* Possible Python call: f((1, 2), 'three') */

::

   {
       const char *file;
       const char *mode = "r";
       int bufsize = 0;
       ok = PyArg_ParseTuple(args, "s|si", &file, &mode, &bufsize);
       /* A string, and optionally another string and an integer */
       /* Possible Python calls:
          f('spam')
          f('spam', 'w')
          f('spam', 'wb', 100000) */
   }

::

   {
       int left, top, right, bottom, h, v;
       ok = PyArg_ParseTuple(args, "((ii)(ii))(ii)",
                &left, &top, &right, &bottom, &h, &v);
       /* A rectangle and a point */
       /* Possible Python call:
          f(((0, 0), (400, 300)), (10, 10)) */
   }

::

   {
       Py_complex c;
       ok = PyArg_ParseTuple(args, "D:myfunction", &c);
       /* a complex, also providing a function name for errors */
       /* Possible Python call: myfunction(1+2j) */
   }


.. _parsetupleandkeywords:

扩展函数的关键字参数
==========================================

.. index:: single: PyArg_ParseTupleAndKeywords()

The :c:func:`PyArg_ParseTupleAndKeywords` function is declared as follows::

   int PyArg_ParseTupleAndKeywords(PyObject *arg, PyObject *kwdict,
                                   char *format, char *kwlist[], ...);

The *arg* and *format* parameters are identical to those of the
:c:func:`PyArg_ParseTuple` function.  The *kwdict* parameter is the dictionary of
keywords received as the third parameter from the Python runtime.  The *kwlist*
parameter is a *NULL*-terminated list of strings which identify the parameters;
the names are matched with the type information from *format* from left to
right.  On success, :c:func:`PyArg_ParseTupleAndKeywords` returns true, otherwise
it returns false and raises an appropriate exception.

.. note::

   Nested tuples cannot be parsed when using keyword arguments!  Keyword parameters
   passed in which are not present in the *kwlist* will cause :exc:`TypeError` to
   be raised.

.. index:: single: Philbrick, Geoff

Here is an example module which uses keywords, based on an example by Geoff
Philbrick (philbrick@hks.com)::

   #include "Python.h"

   static PyObject *
   keywdarg_parrot(PyObject *self, PyObject *args, PyObject *keywds)
   {
       int voltage;
       char *state = "a stiff";
       char *action = "voom";
       char *type = "Norwegian Blue";

       static char *kwlist[] = {"voltage", "state", "action", "type", NULL};

       if (!PyArg_ParseTupleAndKeywords(args, keywds, "i|sss", kwlist,
                                        &voltage, &state, &action, &type))
           return NULL;

       printf("-- This parrot wouldn't %s if you put %i Volts through it.\n",
              action, voltage);
       printf("-- Lovely plumage, the %s -- It's %s!\n", type, state);

       Py_INCREF(Py_None);

       return Py_None;
   }

   static PyMethodDef keywdarg_methods[] = {
       /* The cast of the function is necessary since PyCFunction values
        * only take two PyObject* parameters, and keywdarg_parrot() takes
        * three.
        */
       {"parrot", (PyCFunction)keywdarg_parrot, METH_VARARGS | METH_KEYWORDS,
        "Print a lovely skit to standard output."},
       {NULL, NULL, 0, NULL}   /* sentinel */
   };

::

   void
   initkeywdarg(void)
   {
     /* Create the module and add the functions */
     Py_InitModule("keywdarg", keywdarg_methods);
   }


.. _buildvalue:

构建任意值
=========================

This function is the counterpart to :c:func:`PyArg_ParseTuple`.  It is declared
as follows::

   PyObject *Py_BuildValue(char *format, ...);

It recognizes a set of format units similar to the ones recognized by
:c:func:`PyArg_ParseTuple`, but the arguments (which are input to the function,
not output) must not be pointers, just values.  It returns a new Python object,
suitable for returning from a C function called from Python.

One difference with :c:func:`PyArg_ParseTuple`: while the latter requires its
first argument to be a tuple (since Python argument lists are always represented
as tuples internally), :c:func:`Py_BuildValue` does not always build a tuple.  It
builds a tuple only if its format string contains two or more format units. If
the format string is empty, it returns ``None``; if it contains exactly one
format unit, it returns whatever object is described by that format unit.  To
force it to return a tuple of size 0 or one, parenthesize the format string.

Examples (to the left the call, to the right the resulting Python value)::

   Py_BuildValue("")                        None
   Py_BuildValue("i", 123)                  123
   Py_BuildValue("iii", 123, 456, 789)      (123, 456, 789)
   Py_BuildValue("s", "hello")              'hello'
   Py_BuildValue("y", "hello")              b'hello'
   Py_BuildValue("ss", "hello", "world")    ('hello', 'world')
   Py_BuildValue("s#", "hello", 4)          'hell'
   Py_BuildValue("y#", "hello", 4)          b'hell'
   Py_BuildValue("()")                      ()
   Py_BuildValue("(i)", 123)                (123,)
   Py_BuildValue("(ii)", 123, 456)          (123, 456)
   Py_BuildValue("(i,i)", 123, 456)         (123, 456)
   Py_BuildValue("[i,i]", 123, 456)         [123, 456]
   Py_BuildValue("{s:i,s:i}",
                 "abc", 123, "def", 456)    {'abc': 123, 'def': 456}
   Py_BuildValue("((ii)(ii)) (ii)",
                 1, 2, 3, 4, 5, 6)          (((1, 2), (3, 4)), (5, 6))


.. _refcounts:

引用计数
================

In languages like C or C++, the programmer is responsible for dynamic allocation
and deallocation of memory on the heap.  In C, this is done using the functions
:c:func:`malloc` and :c:func:`free`.  In C++, the operators ``new`` and
``delete`` are used with essentially the same meaning and we'll restrict
the following discussion to the C case.

Every block of memory allocated with :c:func:`malloc` should eventually be
returned to the pool of available memory by exactly one call to :c:func:`free`.
It is important to call :c:func:`free` at the right time.  If a block's address
is forgotten but :c:func:`free` is not called for it, the memory it occupies
cannot be reused until the program terminates.  This is called a :dfn:`memory
leak`.  On the other hand, if a program calls :c:func:`free` for a block and then
continues to use the block, it creates a conflict with re-use of the block
through another :c:func:`malloc` call.  This is called :dfn:`using freed memory`.
It has the same bad consequences as referencing uninitialized data --- core
dumps, wrong results, mysterious crashes.

Common causes of memory leaks are unusual paths through the code.  For instance,
a function may allocate a block of memory, do some calculation, and then free
the block again.  Now a change in the requirements for the function may add a
test to the calculation that detects an error condition and can return
prematurely from the function.  It's easy to forget to free the allocated memory
block when taking this premature exit, especially when it is added later to the
code.  Such leaks, once introduced, often go undetected for a long time: the
error exit is taken only in a small fraction of all calls, and most modern
machines have plenty of virtual memory, so the leak only becomes apparent in a
long-running process that uses the leaking function frequently.  Therefore, it's
important to prevent leaks from happening by having a coding convention or
strategy that minimizes this kind of errors.

Since Python makes heavy use of :c:func:`malloc` and :c:func:`free`, it needs a
strategy to avoid memory leaks as well as the use of freed memory.  The chosen
method is called :dfn:`reference counting`.  The principle is simple: every
object contains a counter, which is incremented when a reference to the object
is stored somewhere, and which is decremented when a reference to it is deleted.
When the counter reaches zero, the last reference to the object has been deleted
and the object is freed.

An alternative strategy is called :dfn:`automatic garbage collection`.
(Sometimes, reference counting is also referred to as a garbage collection
strategy, hence my use of "automatic" to distinguish the two.)  The big
advantage of automatic garbage collection is that the user doesn't need to call
:c:func:`free` explicitly.  (Another claimed advantage is an improvement in speed
or memory usage --- this is no hard fact however.)  The disadvantage is that for
C, there is no truly portable automatic garbage collector, while reference
counting can be implemented portably (as long as the functions :c:func:`malloc`
and :c:func:`free` are available --- which the C Standard guarantees). Maybe some
day a sufficiently portable automatic garbage collector will be available for C.
Until then, we'll have to live with reference counts.

While Python uses the traditional reference counting implementation, it also
offers a cycle detector that works to detect reference cycles.  This allows
applications to not worry about creating direct or indirect circular references;
these are the weakness of garbage collection implemented using only reference
counting.  Reference cycles consist of objects which contain (possibly indirect)
references to themselves, so that each object in the cycle has a reference count
which is non-zero.  Typical reference counting implementations are not able to
reclaim the memory belonging to any objects in a reference cycle, or referenced
from the objects in the cycle, even though there are no further references to
the cycle itself.

The cycle detector is able to detect garbage cycles and can reclaim them so long
as there are no finalizers implemented in Python (:meth:`__del__` methods).
When there are such finalizers, the detector exposes the cycles through the
:mod:`gc` module (specifically, the
``garbage`` variable in that module).  The :mod:`gc` module also exposes a way
to run the detector (the :func:`collect` function), as well as configuration
interfaces and the ability to disable the detector at runtime.  The cycle
detector is considered an optional component; though it is included by default,
it can be disabled at build time using the :option:`--without-cycle-gc` option
to the :program:`configure` script on Unix platforms (including Mac OS X).  If
the cycle detector is disabled in this way, the :mod:`gc` module will not be
available.


.. _refcountsinpython:

Python中的引用计数
----------------------------

There are two macros, ``Py_INCREF(x)`` and ``Py_DECREF(x)``, which handle the
incrementing and decrementing of the reference count. :c:func:`Py_DECREF` also
frees the object when the count reaches zero. For flexibility, it doesn't call
:c:func:`free` directly --- rather, it makes a call through a function pointer in
the object's :dfn:`type object`.  For this purpose (and others), every object
also contains a pointer to its type object.

The big question now remains: when to use ``Py_INCREF(x)`` and ``Py_DECREF(x)``?
Let's first introduce some terms.  Nobody "owns" an object; however, you can
:dfn:`own a reference` to an object.  An object's reference count is now defined
as the number of owned references to it.  The owner of a reference is
responsible for calling :c:func:`Py_DECREF` when the reference is no longer
needed.  Ownership of a reference can be transferred.  There are three ways to
dispose of an owned reference: pass it on, store it, or call :c:func:`Py_DECREF`.
Forgetting to dispose of an owned reference creates a memory leak.

It is also possible to :dfn:`borrow` [#]_ a reference to an object.  The
borrower of a reference should not call :c:func:`Py_DECREF`.  The borrower must
not hold on to the object longer than the owner from which it was borrowed.
Using a borrowed reference after the owner has disposed of it risks using freed
memory and should be avoided completely. [#]_

The advantage of borrowing over owning a reference is that you don't need to
take care of disposing of the reference on all possible paths through the code
--- in other words, with a borrowed reference you don't run the risk of leaking
when a premature exit is taken.  The disadvantage of borrowing over owning is
that there are some subtle situations where in seemingly correct code a borrowed
reference can be used after the owner from which it was borrowed has in fact
disposed of it.

A borrowed reference can be changed into an owned reference by calling
:c:func:`Py_INCREF`.  This does not affect the status of the owner from which the
reference was borrowed --- it creates a new owned reference, and gives full
owner responsibilities (the new owner must dispose of the reference properly, as
well as the previous owner).


.. _ownershiprules:

所有权规则
---------------

Whenever an object reference is passed into or out of a function, it is part of
the function's interface specification whether ownership is transferred with the
reference or not.

Most functions that return a reference to an object pass on ownership with the
reference.  In particular, all functions whose function it is to create a new
object, such as :c:func:`PyLong_FromLong` and :c:func:`Py_BuildValue`, pass
ownership to the receiver.  Even if the object is not actually new, you still
receive ownership of a new reference to that object.  For instance,
:c:func:`PyLong_FromLong` maintains a cache of popular values and can return a
reference to a cached item.

Many functions that extract objects from other objects also transfer ownership
with the reference, for instance :c:func:`PyObject_GetAttrString`.  The picture
is less clear, here, however, since a few common routines are exceptions:
:c:func:`PyTuple_GetItem`, :c:func:`PyList_GetItem`, :c:func:`PyDict_GetItem`, and
:c:func:`PyDict_GetItemString` all return references that you borrow from the
tuple, list or dictionary.

The function :c:func:`PyImport_AddModule` also returns a borrowed reference, even
though it may actually create the object it returns: this is possible because an
owned reference to the object is stored in ``sys.modules``.

When you pass an object reference into another function, in general, the
function borrows the reference from you --- if it needs to store it, it will use
:c:func:`Py_INCREF` to become an independent owner.  There are exactly two
important exceptions to this rule: :c:func:`PyTuple_SetItem` and
:c:func:`PyList_SetItem`.  These functions take over ownership of the item passed
to them --- even if they fail!  (Note that :c:func:`PyDict_SetItem` and friends
don't take over ownership --- they are "normal.")

When a C function is called from Python, it borrows references to its arguments
from the caller.  The caller owns a reference to the object, so the borrowed
reference's lifetime is guaranteed until the function returns.  Only when such a
borrowed reference must be stored or passed on, it must be turned into an owned
reference by calling :c:func:`Py_INCREF`.

The object reference returned from a C function that is called from Python must
be an owned reference --- ownership is transferred from the function to its
caller.


.. _thinice:

薄冰
--------

There are a few situations where seemingly harmless use of a borrowed reference
can lead to problems.  These all have to do with implicit invocations of the
interpreter, which can cause the owner of a reference to dispose of it.

The first and most important case to know about is using :c:func:`Py_DECREF` on
an unrelated object while borrowing a reference to a list item.  For instance::

   void
   bug(PyObject *list)
   {
       PyObject *item = PyList_GetItem(list, 0);

       PyList_SetItem(list, 1, PyLong_FromLong(0L));
       PyObject_Print(item, stdout, 0); /* BUG! */
   }

This function first borrows a reference to ``list[0]``, then replaces
``list[1]`` with the value ``0``, and finally prints the borrowed reference.
Looks harmless, right?  But it's not!

Let's follow the control flow into :c:func:`PyList_SetItem`.  The list owns
references to all its items, so when item 1 is replaced, it has to dispose of
the original item 1.  Now let's suppose the original item 1 was an instance of a
user-defined class, and let's further suppose that the class defined a
:meth:`__del__` method.  If this class instance has a reference count of 1,
disposing of it will call its :meth:`__del__` method.

Since it is written in Python, the :meth:`__del__` method can execute arbitrary
Python code.  Could it perhaps do something to invalidate the reference to
``item`` in :c:func:`bug`?  You bet!  Assuming that the list passed into
:c:func:`bug` is accessible to the :meth:`__del__` method, it could execute a
statement to the effect of ``del list[0]``, and assuming this was the last
reference to that object, it would free the memory associated with it, thereby
invalidating ``item``.

The solution, once you know the source of the problem, is easy: temporarily
increment the reference count.  The correct version of the function reads::

   void
   no_bug(PyObject *list)
   {
       PyObject *item = PyList_GetItem(list, 0);

       Py_INCREF(item);
       PyList_SetItem(list, 1, PyLong_FromLong(0L));
       PyObject_Print(item, stdout, 0);
       Py_DECREF(item);
   }

This is a true story.  An older version of Python contained variants of this bug
and someone spent a considerable amount of time in a C debugger to figure out
why his :meth:`__del__` methods would fail...

The second case of problems with a borrowed reference is a variant involving
threads.  Normally, multiple threads in the Python interpreter can't get in each
other's way, because there is a global lock protecting Python's entire object
space.  However, it is possible to temporarily release this lock using the macro
:c:macro:`Py_BEGIN_ALLOW_THREADS`, and to re-acquire it using
:c:macro:`Py_END_ALLOW_THREADS`.  This is common around blocking I/O calls, to
let other threads use the processor while waiting for the I/O to complete.
Obviously, the following function has the same problem as the previous one::

   void
   bug(PyObject *list)
   {
       PyObject *item = PyList_GetItem(list, 0);
       Py_BEGIN_ALLOW_THREADS
       ...some blocking I/O call...
       Py_END_ALLOW_THREADS
       PyObject_Print(item, stdout, 0); /* BUG! */
   }


.. _nullpointers:

NULL 指针
-------------

In general, functions that take object references as arguments do not expect you
to pass them *NULL* pointers, and will dump core (or cause later core dumps) if
you do so.  Functions that return object references generally return *NULL* only
to indicate that an exception occurred.  The reason for not testing for *NULL*
arguments is that functions often pass the objects they receive on to other
function --- if each function were to test for *NULL*, there would be a lot of
redundant tests and the code would run more slowly.

It is better to test for *NULL* only at the "source:" when a pointer that may be
*NULL* is received, for example, from :c:func:`malloc` or from a function that
may raise an exception.

The macros :c:func:`Py_INCREF` and :c:func:`Py_DECREF` do not check for *NULL*
pointers --- however, their variants :c:func:`Py_XINCREF` and :c:func:`Py_XDECREF`
do.

The macros for checking for a particular object type (``Pytype_Check()``) don't
check for *NULL* pointers --- again, there is much code that calls several of
these in a row to test an object against various different expected types, and
this would generate redundant tests.  There are no variants with *NULL*
checking.

The C function calling mechanism guarantees that the argument list passed to C
functions (``args`` in the examples) is never *NULL* --- in fact it guarantees
that it is always a tuple. [#]_

It is a severe error to ever let a *NULL* pointer "escape" to the Python user.

.. Frank Stajano:
   A pedagogically buggy example, along the lines of the previous listing, would
   be helpful here -- showing in more concrete terms what sort of actions could
   cause the problem. I can't very well imagine it from the description.


.. _cplusplus:

用C++写扩展
=========================

It is possible to write extension modules in C++.  Some restrictions apply.  If
the main program (the Python interpreter) is compiled and linked by the C
compiler, global or static objects with constructors cannot be used.  This is
not a problem if the main program is linked by the C++ compiler.  Functions that
will be called by the Python interpreter (in particular, module initialization
functions) have to be declared using ``extern "C"``. It is unnecessary to
enclose the Python header files in ``extern "C" {...}`` --- they use this form
already if the symbol ``__cplusplus`` is defined (all recent C++ compilers
define this symbol).


.. _using-capsules:

为扩展模块提供一个C API
=========================================

.. sectionauthor:: Konrad Hinsen <hinsen@cnrs-orleans.fr>


Many extension modules just provide new functions and types to be used from
Python, but sometimes the code in an extension module can be useful for other
extension modules. For example, an extension module could implement a type
"collection" which works like lists without order. Just like the standard Python
list type has a C API which permits extension modules to create and manipulate
lists, this new collection type should have a set of C functions for direct
manipulation from other extension modules.

At first sight this seems easy: just write the functions (without declaring them
``static``, of course), provide an appropriate header file, and document
the C API. And in fact this would work if all extension modules were always
linked statically with the Python interpreter. When modules are used as shared
libraries, however, the symbols defined in one module may not be visible to
another module. The details of visibility depend on the operating system; some
systems use one global namespace for the Python interpreter and all extension
modules (Windows, for example), whereas others require an explicit list of
imported symbols at module link time (AIX is one example), or offer a choice of
different strategies (most Unices). And even if symbols are globally visible,
the module whose functions one wishes to call might not have been loaded yet!

Portability therefore requires not to make any assumptions about symbol
visibility. This means that all symbols in extension modules should be declared
``static``, except for the module's initialization function, in order to
avoid name clashes with other extension modules (as discussed in section
:ref:`methodtable`). And it means that symbols that *should* be accessible from
other extension modules must be exported in a different way.

Python provides a special mechanism to pass C-level information (pointers) from
one extension module to another one: Capsules. A Capsule is a Python data type
which stores a pointer (:c:type:`void \*`).  Capsules can only be created and
accessed via their C API, but they can be passed around like any other Python
object. In particular,  they can be assigned to a name in an extension module's
namespace. Other extension modules can then import this module, retrieve the
value of this name, and then retrieve the pointer from the Capsule.

There are many ways in which Capsules can be used to export the C API of an
extension module. Each function could get its own Capsule, or all C API pointers
could be stored in an array whose address is published in a Capsule. And the
various tasks of storing and retrieving the pointers can be distributed in
different ways between the module providing the code and the client modules.

Whichever method you choose, it's important to name your Capsules properly.
The function :c:func:`PyCapsule_New` takes a name parameter
(:c:type:`const char \*`); you're permitted to pass in a *NULL* name, but
we strongly encourage you to specify a name.  Properly named Capsules provide
a degree of runtime type-safety; there is no feasible way to tell one unnamed
Capsule from another.

In particular, Capsules used to expose C APIs should be given a name following
this convention::

    modulename.attributename

The convenience function :c:func:`PyCapsule_Import` makes it easy to
load a C API provided via a Capsule, but only if the Capsule's name
matches this convention.  This behavior gives C API users a high degree
of certainty that the Capsule they load contains the correct C API.

The following example demonstrates an approach that puts most of the burden on
the writer of the exporting module, which is appropriate for commonly used
library modules. It stores all C API pointers (just one in the example!) in an
array of :c:type:`void` pointers which becomes the value of a Capsule. The header
file corresponding to the module provides a macro that takes care of importing
the module and retrieving its C API pointers; client modules only have to call
this macro before accessing the C API.

The exporting module is a modification of the :mod:`spam` module from section
:ref:`extending-simpleexample`. The function :func:`spam.system` does not call
the C library function :c:func:`system` directly, but a function
:c:func:`PySpam_System`, which would of course do something more complicated in
reality (such as adding "spam" to every command). This function
:c:func:`PySpam_System` is also exported to other extension modules.

The function :c:func:`PySpam_System` is a plain C function, declared
``static`` like everything else::

   static int
   PySpam_System(const char *command)
   {
       return system(command);
   }

The function :c:func:`spam_system` is modified in a trivial way::

   static PyObject *
   spam_system(PyObject *self, PyObject *args)
   {
       const char *command;
       int sts;

       if (!PyArg_ParseTuple(args, "s", &command))
           return NULL;
       sts = PySpam_System(command);
       return PyLong_FromLong(sts);
   }

In the beginning of the module, right after the line ::

   #include "Python.h"

two more lines must be added::

   #define SPAM_MODULE
   #include "spammodule.h"

The ``#define`` is used to tell the header file that it is being included in the
exporting module, not a client module. Finally, the module's initialization
function must take care of initializing the C API pointer array::

   PyMODINIT_FUNC
   PyInit_spam(void)
   {
       PyObject *m;
       static void *PySpam_API[PySpam_API_pointers];
       PyObject *c_api_object;

       m = PyModule_Create(&spammodule);
       if (m == NULL)
           return NULL;

       /* Initialize the C API pointer array */
       PySpam_API[PySpam_System_NUM] = (void *)PySpam_System;

       /* Create a Capsule containing the API pointer array's address */
       c_api_object = PyCapsule_New((void *)PySpam_API, "spam._C_API", NULL);

       if (c_api_object != NULL)
           PyModule_AddObject(m, "_C_API", c_api_object);
       return m;
   }

Note that ``PySpam_API`` is declared ``static``; otherwise the pointer
array would disappear when :func:`PyInit_spam` terminates!

The bulk of the work is in the header file :file:`spammodule.h`, which looks
like this::

   #ifndef Py_SPAMMODULE_H
   #define Py_SPAMMODULE_H
   #ifdef __cplusplus
   extern "C" {
   #endif

   /* Header file for spammodule */

   /* C API functions */
   #define PySpam_System_NUM 0
   #define PySpam_System_RETURN int
   #define PySpam_System_PROTO (const char *command)

   /* Total number of C API pointers */
   #define PySpam_API_pointers 1


   #ifdef SPAM_MODULE
   /* This section is used when compiling spammodule.c */

   static PySpam_System_RETURN PySpam_System PySpam_System_PROTO;

   #else
   /* This section is used in modules that use spammodule's API */

   static void **PySpam_API;

   #define PySpam_System \
    (*(PySpam_System_RETURN (*)PySpam_System_PROTO) PySpam_API[PySpam_System_NUM])

   /* Return -1 on error, 0 on success.
    * PyCapsule_Import will set an exception if there's an error.
    */
   static int
   import_spam(void)
   {
       PySpam_API = (void **)PyCapsule_Import("spam._C_API", 0);
       return (PySpam_API != NULL) ? 0 : -1;
   }

   #endif

   #ifdef __cplusplus
   }
   #endif

   #endif /* !defined(Py_SPAMMODULE_H) */

All that a client module must do in order to have access to the function
:c:func:`PySpam_System` is to call the function (or rather macro)
:c:func:`import_spam` in its initialization function::

   PyMODINIT_FUNC
   PyInit_client(void)
   {
       PyObject *m;

       m = PyModule_Create(&clientmodule);
       if (m == NULL)
           return NULL;
       if (import_spam() < 0)
           return NULL;
       /* additional initialization can happen here */
       return m;
   }

The main disadvantage of this approach is that the file :file:`spammodule.h` is
rather complicated. However, the basic structure is the same for each function
that is exported, so it has to be learned only once.

Finally it should be mentioned that Capsules offer additional functionality,
which is especially useful for memory allocation and deallocation of the pointer
stored in a Capsule. The details are described in the Python/C API Reference
Manual in the section :ref:`capsules` and in the implementation of Capsules (files
:file:`Include/pycapsule.h` and :file:`Objects/pycapsule.c` in the Python source
code distribution).

.. rubric:: Footnotes

.. [#] An interface for this function already exists in the standard module :mod:`os`
   --- it was chosen as a simple and straightforward example.

.. [#] The metaphor of "borrowing" a reference is not completely correct: the owner
   still has a copy of the reference.

.. [#] Checking that the reference count is at least 1 **does not work** --- the
   reference count itself could be in freed memory and may thus be reused for
   another object!

.. [#] These guarantees don't hold when you use the "old" style calling convention ---
   this is still found in much existing code.

