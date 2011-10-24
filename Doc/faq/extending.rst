=======================
 扩展/嵌入常见问题
=======================

.. contents::

.. highlight:: c


.. XXX need review for Python 3.


*可以用C创建函数吗？
-----------------------------------

Yes, you can create built-in modules containing functions, variables, exceptions
and even new types in C.  This is explained in the document
:ref:`extending-index`.

Most intermediate or advanced Python books will also cover this topic.

是的，你可以用C创建内置模块包含的函数、变量、异常甚至是新的类型。
甚至在C例外和新类型这在*Extending and Embedding the Python Interpreter*文档中有解释。
*扩展和嵌入Python解释*.

大多数中级或高级Python书籍也将涵盖这一主题。


*可以用C++创建函数吗？
-------------------------------------

Yes, using the C compatibility features found in C++.  Place ``extern "C" {
... }`` around the Python include files and put ``extern "C"`` before each
function that is going to be called by the Python interpreter.  Global or static
C++ objects with constructors are probably not a good idea.

是的，使用C兼容性功能创建C++函数.在Python包含文件附近放置``extern "C" { ... }``，在每个会被Python解释器调用的函数前放置``extern "C"``。
的“C”{...周围的Python} `` ``包含文件，把为extern“C”``
在每次函数将被调用由Python
解释器使用全局或使用构造函数的静态C++对象可能不是一个好主意。
可能不是一个好主意。


.. _c-wrapper-software:



*编写C程序比较困难，是否有其他方法?
----------------------------------------------


有许多方法代替编写自己的C扩展，这取决于你想要做的事。

如果你需要更快的速度，Psyco从Python字节码生成X86汇编代码。
bytecode 字节码您可以使用Psyco编译代码中大部分时间要求严格的函数，只要举手之劳就可以获得非常显著的改善，只要你的机器使用的是x86兼容的处理器。
在你的代码功能，并取得非常显着改善
举手之劳，只要你的机器上运行的
x86兼容处理器。

Cython机器相关的Pyrex是Python稍微修改过的编译器，它能生成相应的C代码。
修改Python的形式并生成相应的C代码。Cython
Cython 和 Pyrex使在对Python的C API没有了解的情况下写一个扩展成为可能。
学习Python的C API。

如果你需要连接到一些C或C++库，这些地区没有的Python
目前存在的扩展，你可以试着用图书馆的资料
同类型和功能作为SWIG的工具等。SIP协议，CXX来升压或
编织也是封装C + +库的替代品。


*如何从C任意执行Python语句？
-----------------------------------------------------

The highest-level function to do this is :c:func:`PyRun_SimpleString` which takes
a single string argument to be executed in the context of the module
``__main__`` and returns 0 for success and -1 when an exception occurred
(including ``SyntaxError``).  If you want more control, use
:c:func:`PyRun_String`; see the source for :c:func:`PyRun_SimpleString` in
``Python/pythonrun.c``.

最高级别的功能，这样做是`` PyRun_SimpleString（）``
它接受一个字符串参数在执行上下文
`` `` __main__模块，并成功返回0和-1时
发生异常（包括语法，`` ``）。如果你想了解更多
控制，使用`` PyRun_String ()``;看到源
`` PyRun_SimpleString（）`` ``中的Python / pythonrun.c ``。




*如何从C计算任意Python表达式？
---------------------------------------------------------

Call the function :c:func:`PyRun_String` from the previous question with the
start symbol :c:data:`Py_eval_input`; it parses an expression, evaluates it and
returns its value.

调用函数`` PyRun_String（）``从以前的问题与
开始符号`` `` Py_eval_input，它解析表达式，计算
它并返回其值。




*怎样从一个Python对象提取C值吗？
-----------------------------------------------

That depends on the object's type.  If it's a tuple, :c:func:`PyTuple_Size`
returns its length and :c:func:`PyTuple_GetItem` returns the item at a specified
index.  Lists have similar functions, :c:func:`PyListSize` and
:c:func:`PyList_GetItem`.

For strings, :c:func:`PyString_Size` returns its length and
:c:func:`PyString_AsString` a pointer to its value.  Note that Python strings may
contain null bytes so C's :c:func:`strlen` should not be used.

To test the type of an object, first make sure it isn't *NULL*, and then use
:c:func:`PyString_Check`, :c:func:`PyTuple_Check`, :c:func:`PyList_Check`, etc.

There is also a high-level API to Python objects which is provided by the
so-called 'abstract' interface -- read ``Include/abstract.h`` for further
details.  It allows interfacing with any kind of Python sequence using calls
like :c:func:`PySequence_Length`, :c:func:`PySequence_GetItem`, etc.)  as well as
many other useful protocols.

这取决于对象的类型。如果它是一个元组，
`` PyTuple_Size（）``返回它的长度和`` PyTuple_GetItem（）``
返回位于指定索引的项目。列表有类似的功能，
`` PyListSize（）``和`` PyList_GetItem ()``.

对于字符串，`` PyString_Size（）``返回它的长度和
`` PyString_AsString（）``到其值的指针。请注意，Python的
所以C字符串可能包含的`` strlen的（）``不应该使用空字节。

要测试一个对象的类型，首先要确保它不是*空*，和
然后使用`` `` PyTuple_Check ()``, PyString_Check ()``,
`` PyList_Check ()``,等

还有一个高层次的API，以Python对象所提供有关
所谓的'抽象'界面 - 读``包含/ abstract.h ``为
进一步的细节。它允许任何接口类型的Python
序列使用电话一样`` PySequence_Length ()``,
`` PySequence_GetItem ()``,等）以及其它许多有用的
规范。




*怎样使用Py_BuildValue（）创建一个任意长度元组？
-------------------------------------------------------------------

You can't.  Use ``t = PyTuple_New(n)`` instead, and fill it with objects using
``PyTuple_SetItem(t, i, o)`` -- note that this "eats" a reference count of
``o``, so you have to :c:func:`Py_INCREF` it.  Lists have similar functions
``PyList_New(n)`` and ``PyList_SetItem(l, i, o)``.  Note that you *must* set all
the tuple items to some value before you pass the tuple to Python code --
``PyTuple_New(n)`` initializes them to NULL, which isn't a valid Python value.




*怎样调用C对象的方法？
----------------------------------------

The :c:func:`PyObject_CallMethod` function can be used to call an arbitrary
method of an object.  The parameters are the object, the name of the method to
call, a format string like that used with :c:func:`Py_BuildValue`, and the
argument values::

   PyObject *
   PyObject_CallMethod(PyObject *object, char *method_name,
                       char *arg_format, ...);

This works for any object that has methods -- whether built-in or user-defined.
You are responsible for eventually :c:func:`Py_DECREF`\ 'ing the return value.

To call, e.g., a file object's "seek" method with arguments 10, 0 (assuming the
file object pointer is "f")::

   res = PyObject_CallMethod(f, "seek", "(ii)", 10, 0);
   if (res == NULL) {
           ... an exception occurred ...
   }
   else {
           Py_DECREF(res);
   }

Note that since :c:func:`PyObject_CallObject` *always* wants a tuple for the
argument list, to call a function without arguments, pass "()" for the format,
and to call a function with one argument, surround the argument in parentheses,
e.g. "(i)".




*怎样获取PyErr_Print（）的输出（或任何打印到stout/stderr的输出）
----------------------------------------------------------------------------------------

In Python code, define an object that supports the ``write()`` method.  Assign
this object to :data:`sys.stdout` and :data:`sys.stderr`.  Call print_error, or
just allow the standard traceback mechanism to work. Then, the output will go
wherever your ``write()`` method sends it.

The easiest way to do this is to use the StringIO class in the standard library.

Sample code and use for catching stdout:

   >>> class StdoutCatcher:
   ...     def __init__(self):
   ...         self.data = ''
   ...     def write(self, stuff):
   ...         self.data = self.data + stuff
   ...
   >>> import sys
   >>> sys.stdout = StdoutCatcher()
   >>> print('foo')
   >>> print('hello world!')
   >>> sys.stderr.write(sys.stdout.data)
   foo
   hello world!




*怎样从C中访问Python写的一个模块？
--------------------------------------------------

You can get a pointer to the module object as follows::

   module = PyImport_ImportModule("<modulename>");

If the module hasn't been imported yet (i.e. it is not yet present in
:data:`sys.modules`), this initializes the module; otherwise it simply returns
the value of ``sys.modules["<modulename>"]``.  Note that it doesn't enter the
module into any namespace -- it only ensures it has been initialized and is
stored in :data:`sys.modules`.

You can then access the module's attributes (i.e. any name defined in the
module) as follows::

   attr = PyObject_GetAttrString(module, "<attrname>");

Calling :c:func:`PyObject_SetAttrString` to assign to variables in the module
also works.




怎样在Python使用C++对象接口？
----------------------------------------------

Depending on your requirements, there are many approaches.  To do this manually,
begin by reading :ref:`the "Extending and Embedding" document
<extending-index>`.  Realize that for the Python run-time system, there isn't a
whole lot of difference between C and C++ -- so the strategy of building a new
Python type around a C structure (pointer) type will also work for C++ objects.

For C++ libraries, see :ref:`c-wrapper-software`.




*我使用Setup文件添加了一个模块却出错了，为什么？
--------------------------------------------------------------

Setup must end in a newline, if there is no newline there, the build process
fails.  (Fixing this requires some ugly shell script hackery, and this bug is so
minor that it doesn't seem worth the effort.)




*怎样调试扩展模块？
----------------------------

When using GDB with dynamically loaded extensions, you can't set a breakpoint in
your extension until your extension is loaded.

In your ``.gdbinit`` file (or interactively), add the command::

   br _PyImport_LoadDynamicModule

Then, when you run GDB::

   $ gdb /local/bin/python
   gdb) run myscript.py
   gdb) continue # repeat until your extension is loaded
   gdb) finish   # so that your extension is loaded
   gdb) br myfunction.c:50
   gdb) continue


*我想在的Linux系统上构建一个Python模块，但有些文件丢失了，问什么？
文件丢失。为什么？
--------------------------------------------------------------------------------------

Most packaged versions of Python don't include the
:file:`/usr/lib/python2.{x}/config/` directory, which contains various files
required for compiling Python extensions.

For Red Hat, install the python-devel RPM to get the necessary files.

For Debian, run ``apt-get install python-dev``.




*”SystemError：_PyImport_FixupExtension：module yourmodule not loaded"是什么意思？
-------------------------------------------------------------------------------------

This means that you have created an extension module named "yourmodule", but
your module init function does not initialize with that name.

Every module init function will have a line similar to::

   module = Py_InitModule("yourmodule", yourmodule_functions);

If the string passed to this function is not the same name as your extension
module, the :exc:`SystemError` exception will be raised.




*怎样分辨“ncomplete input”，“invalid input”？
------------------------------------------------------

Sometimes you want to emulate the Python interactive interpreter's behavior,
where it gives you a continuation prompt when the input is incomplete (e.g. you
typed the start of an "if" statement or you didn't close your parentheses or
triple string quotes), but it gives you a syntax error message immediately when
the input is invalid.

In Python you can use the :mod:`codeop` module, which approximates the parser's
behavior sufficiently.  IDLE uses this, for example.

The easiest way to do it in C is to call :c:func:`PyRun_InteractiveLoop` (perhaps
in a separate thread) and let the Python interpreter handle the input for
you. You can also set the :c:func:`PyOS_ReadlineFunctionPointer` to point at your
custom input function. See ``Modules/readline.c`` and ``Parser/myreadline.c``
for more hints.

However sometimes you have to run the embedded Python interpreter in the same
thread as your rest application and you can't allow the
:c:func:`PyRun_InteractiveLoop` to stop while waiting for user input.  The one
solution then is to call :c:func:`PyParser_ParseString` and test for ``e.error``
equal to ``E_EOF``, which means the input is incomplete).  Here's a sample code
fragment, untested, inspired by code from Alex Farber::

   #include <Python.h>
   #include <node.h>
   #include <errcode.h>
   #include <grammar.h>
   #include <parsetok.h>
   #include <compile.h>

   int testcomplete(char *code)
     /* code should end in \n */
     /* return -1 for error, 0 for incomplete, 1 for complete */
   {
     node *n;
     perrdetail e;

     n = PyParser_ParseString(code, &_PyParser_Grammar,
                              Py_file_input, &e);
     if (n == NULL) {
       if (e.error == E_EOF)
         return 0;
       return -1;
     }

     PyNode_Free(n);
     return 1;
   }

Another solution is trying to compile the received string with
:c:func:`Py_CompileString`. If it compiles without errors, try to execute the
returned code object by calling :c:func:`PyEval_EvalCode`. Otherwise save the
input for later. If the compilation fails, find out if it's an error or just
more input is required - by extracting the message string from the exception
tuple and comparing it to the string "unexpected EOF while parsing".  Here is a
complete example using the GNU readline library (you may want to ignore
**SIGINT** while calling readline())::

   #include <stdio.h>
   #include <readline.h>

   #include <Python.h>
   #include <object.h>
   #include <compile.h>
   #include <eval.h>

   int main (int argc, char* argv[])
   {
     int i, j, done = 0;                          /* lengths of line, code */
     char ps1[] = ">>> ";
     char ps2[] = "... ";
     char *prompt = ps1;
     char *msg, *line, *code = NULL;
     PyObject *src, *glb, *loc;
     PyObject *exc, *val, *trb, *obj, *dum;

     Py_Initialize ();
     loc = PyDict_New ();
     glb = PyDict_New ();
     PyDict_SetItemString (glb, "__builtins__", PyEval_GetBuiltins ());

     while (!done)
     {
       line = readline (prompt);

       if (NULL == line)                          /* CTRL-D pressed */
       {
         done = 1;
       }
       else
       {
         i = strlen (line);

         if (i > 0)
           add_history (line);                    /* save non-empty lines */

         if (NULL == code)                        /* nothing in code yet */
           j = 0;
         else
           j = strlen (code);

         code = realloc (code, i + j + 2);
         if (NULL == code)                        /* out of memory */
           exit (1);

         if (0 == j)                              /* code was empty, so */
           code[0] = '\0';                        /* keep strncat happy */

         strncat (code, line, i);                 /* append line to code */
         code[i + j] = '\n';                      /* append '\n' to code */
         code[i + j + 1] = '\0';

         src = Py_CompileString (code, "<stdin>", Py_single_input);

         if (NULL != src)                         /* compiled just fine - */
         {
           if (ps1  == prompt ||                  /* ">>> " or */
               '\n' == code[i + j - 1])           /* "... " and double '\n' */
           {                                               /* so execute it */
             dum = PyEval_EvalCode (src, glb, loc);
             Py_XDECREF (dum);
             Py_XDECREF (src);
             free (code);
             code = NULL;
             if (PyErr_Occurred ())
               PyErr_Print ();
             prompt = ps1;
           }
         }                                        /* syntax error or E_EOF? */
         else if (PyErr_ExceptionMatches (PyExc_SyntaxError))
         {
           PyErr_Fetch (&exc, &val, &trb);        /* clears exception! */

           if (PyArg_ParseTuple (val, "sO", &msg, &obj) &&
               !strcmp (msg, "unexpected EOF while parsing")) /* E_EOF */
           {
             Py_XDECREF (exc);
             Py_XDECREF (val);
             Py_XDECREF (trb);
             prompt = ps2;
           }
           else                                   /* some other syntax error */
           {
             PyErr_Restore (exc, val, trb);
             PyErr_Print ();
             free (code);
             code = NULL;
             prompt = ps1;
           }
         }
         else                                     /* some non-syntax error */
         {
           PyErr_Print ();
           free (code);
           code = NULL;
           prompt = ps1;
         }

         free (line);
       }
     }

     Py_XDECREF(glb);
     Py_XDECREF(loc);
     Py_Finalize();
     exit(0);
   }




*怎样找到未定义的g++符号__builtin_new或__pure_virtual？
--------------------------------------------------------------------

To dynamically load g++ extension modules, you must recompile Python, relink it
using g++ (change LINKCC in the Python Modules Makefile), and link your
extension module using g++ (e.g., ``g++ -shared -o mymodule.so mymodule.o``).


*怎样创建一个对象类，其一部方法用C实现，另一部分用Python实现，如通过继承？
----------------------------------------------------------------------------------------------------------------

In Python 2.2, you can inherit from built-in classes such as :class:`int`,
:class:`list`, :class:`dict`, etc.

The Boost Python Library (BPL, http://www.boost.org/libs/python/doc/index.html)
provides a way of doing this from C++ (i.e. you can inherit from an extension
class written in C++ using the BPL).

*当导入模块X，为什么提示“undefined symbol: PyUnicodeUCS2*”？
-------------------------------------------------------------------------

You are using a version of Python that uses a 4-byte representation for Unicode
characters, but some C extension module you are importing was compiled using a
Python that uses a 2-byte representation for Unicode characters (the default).

If instead the name of the undefined symbol starts with ``PyUnicodeUCS4``, the
problem is the reverse: Python was built using 2-byte Unicode characters, and
the extension module was compiled using a Python with 4-byte Unicode characters.

This can easily occur when using pre-built extension packages.  RedHat Linux
7.x, in particular, provided a "python2" binary that is compiled with 4-byte
Unicode.  This only causes the link failure if the extension uses any of the
``PyUnicode_*()`` functions.  It is also a problem if an extension uses any of
the Unicode-related format specifiers for :c:func:`Py_BuildValue` (or similar) or
parameter specifications for :c:func:`PyArg_ParseTuple`.

You can check the size of the Unicode character a Python interpreter is using by
checking the value of sys.maxunicode:

   >>> import sys
   >>> if sys.maxunicode > 65535:
   ...     print('UCS4 build')
   ... else:
   ...     print('UCS2 build')

The only way to solve this problem is to use extension modules compiled with a
Python binary built using the same size for Unicode characters.
