
.. _execmodel:

***************
可执行模块
***************

.. index:: single: execution model


.. _naming:

命名和绑定
==================

.. index::
   pair: code; block
   single: namespace
   single: scope

.. index::
   single: name
   pair: binding; name

:dfn:`Names` refer to objects.  Names are introduced by name binding operations.
Each occurrence of a name in the program text refers to the :dfn:`binding` of
that name established in the innermost function block containing the use.

名字 ( :dfn:`Names` ) 是一个对象的引用. 名字是通过名字绑定操作引入的. 在程序文本中名字的每次出现,都是引用了在最内层函数块建立的针对所用名字的绑定 ( :dfn:`binding` ) . 

.. index:: block

A :dfn:`block` is a piece of Python program text that is executed as a unit.
The following are blocks: a module, a function body, and a class definition.
Each command typed interactively is a block.  A script file (a file given as
standard input to the interpreter or specified on the interpreter command line
the first argument) is a code block.  A script command (a command specified on
the interpreter command line with the '**-c**' option) is a code block.  The
string argument passed to the built-in functions :func:`eval` and :func:`exec`
is a code block.

代码块 ( :dfn:`block` ) 是可以作为执行单元的一段Python程序代码. 以下都属于代码块: 模块、函数体和类定义. 每条交互式输入的命令都是一个代码块. 脚本文件 (通过标准输入或者命令行的第一个参数告知解释器) 也是代码块. 脚本命令 (由命令行选项 '**-c**' 指定) 也是代码块. 传递给内置函数 :func:`eval` 和 :func:`exec` 的字符串参数也是代码块. 

.. index:: pair: execution; frame

A code block is executed in an :dfn:`execution frame`.  A frame contains some
administrative information (used for debugging) and determines where and how
execution continues after the code block's execution has completed.

代码块是在执行栈桢 ( :dfn:`execution frame` ) 内执行的. 栈桢包括有某些管理性信息 (用于调试) ,并决定了执行完这段代码块后在什么地方、如何继续. 

.. index:: scope

A :dfn:`scope` defines the visibility of a name within a block.  If a local
variable is defined in a block, its scope includes that block.  If the
definition occurs in a function block, the scope extends to any blocks contained
within the defining one, unless a contained block introduces a different binding
for the name.  The scope of names defined in a class block is limited to the
class block; it does not extend to the code blocks of methods -- this includes
comprehensions and generator expressions since they are implemented using a
function scope.  This means that the following will fail:

作用域( :dfn:`scope` )定义了名字在代码块内的可见性. 如果局部变量是在一个代码块内部定义的,那么这个局部变量的作用域包括这个代码块. 如果定义出现在一个函数块里,那么定义域会扩展到这个函数所包括的任何块,除非内部块对这个名字有另外的绑定. 在类块中定义的名字绑定限于该类块内,它不会扩展到方法的代码块中——包括comprehensions和generator表达式,因为这些也是使用函数作用域实现的. 这意味着以下代码会失败::

   class A:
       a = 42
       b = list(a + i for i in range(10))

.. index:: single: environment

When a name is used in a code block, it is resolved using the nearest enclosing
scope.  The set of all such scopes visible to a code block is called the block's
:dfn:`environment`.

在一个代码块内使用名字时,它会使用最接近的封闭 (enclosing) 作用域进行解析. 一个代码块的可见作用域集合叫做块的 `环境`  ( :dfn:`environment` ) . 

.. index:: pair: free; variable

If a name is bound in a block, it is a local variable of that block, unless
declared as :keyword:`nonlocal`.  If a name is bound at the module level, it is
a global variable.  (The variables of the module code block are local and
global.)  If a variable is used in a code block but not defined there, it is a
:dfn:`free variable`.

如果某个名字绑定在代码块内,并且不是用 :keyword:`nonlocal` 声明的,就称为它是这个块内的局部变量. 如果名字绑定在模块级别上,它就是全局变量 (模块代码块的变量是局部的,也是全局的) . 如果某名字在代码块中有所使用,但并不在该块中定义,就称它为 `自由变量` ( :dfn:`free variable` ) . 

.. index::
   single: NameError (built-in exception)
   single: UnboundLocalError

When a name is not found at all, a :exc:`NameError` exception is raised.  If the
name refers to a local variable that has not been bound, a
:exc:`UnboundLocalError` exception is raised.  :exc:`UnboundLocalError` is a
subclass of :exc:`NameError`.

如果没有找到名字所需的绑定,就抛出异常 :exc:`NameError` . 如果名字引用的局部变量是没有绑定的,就抛出异常 :exc:`UnboundLocalError` , :exc:`UnboundLocalError` 是 :exc:`NameError` 的一个子类. 

.. index:: statement: from

The following constructs bind names: formal parameters to functions,
:keyword:`import` statements, class and function definitions (these bind the
class or function name in the defining block), and targets that are identifiers
if occurring in an assignment, :keyword:`for` loop header, or after
:keyword:`as` in a :keyword:`with` statement or :keyword:`except` clause.
The :keyword:`import` statement
of the form ``from ... import *`` binds all names defined in the imported
module, except those beginning with an underscore.  This form may only be used
at the module level.

以下构造可以绑定名字:  函数的形式参数、 :keyword:`import` 语句、类和函数定义 (在定义块中绑定类或函数名字) 以及赋值语句的目标标识符、 :keyword:`for` 循环头、
出现在 :keyword:`with` 语句或者 :keyword:`except` 子句之后的 :keyword:`as` 语句、以 ``from ... import *`` 形式出现的 :keyword:`import` 语句会绑定导入模块中的所有名字 (以下划线开始的名字除外) ,这种形式仅用于模块级别上. 

A target occurring in a :keyword:`del` statement is also considered bound for
this purpose (though the actual semantics are to unbind the name).  It is
illegal to unbind a name that is referenced by an enclosing scope; the compiler
will report a :exc:`SyntaxError`.

:keyword:`del` 语句的目标也是作为一个名字绑定出现的 (虽然整条语句的功能是解除绑定) . 试图解除在另外封闭作用域引用的名字的绑定是不合法的,编译器会抛出异常 :exc:`SyntaxError` . 

Each assignment or import statement occurs within a block defined by a class or
function definition or at the module level (the top-level code block).

所有的赋值语句、import语句都必须出现在类定义或者函数定义块,或者模块级别上 (顶级代码块上) . 

If a name binding operation occurs anywhere within a code block, all uses of the
name within the block are treated as references to the current block.  This can
lead to errors when a name is used within a block before it is bound.  This rule
is subtle.  Python lacks declarations and allows name binding operations to
occur anywhere within a code block.  The local variables of a code block can be
determined by scanning the entire text of the block for name binding operations.

如果一个代码块内任何地方出现了某名字绑定操作,那么在该代码块内这个名字的使用都会被认为是对当前块内的引用 (即局部变量) . 如果一个名字在绑定它之前使用的话就会导致错误. 这个规则有些微妙. Python缺少声明,并且允许名字绑定操作发生任何地方. 以名字绑定为目的扫描整个代码块,就可以检测出代码块中的局部变量. 

If the :keyword:`global` statement occurs within a block, all uses of the name
specified in the statement refer to the binding of that name in the top-level
namespace.  Names are resolved in the top-level namespace by searching the
global namespace, i.e. the namespace of the module containing the code block,
and the builtins namespace, the namespace of the module :mod:`builtins`.  The
global namespace is searched first.  If the name is not found there, the builtins
namespace is searched.  The global statement must precede all uses of the name.

如果代码块内出现了 :keyword:`global` 语句,那么使用以这条语句指定的名字,都会引用顶层名字空间中的绑定. 在顶层名字空间解析的名字,首先会搜索全局名字空间 (即包含代码块的模块的名字空间) ,如果没有找到,会再搜索内置名字空间 (模块 :mod:`builtins` 的名字空间) . global语句必须在使用相应全局变量之前使用. 

.. XXX document "nonlocal" semantics here

.. index:: pair: restricted; execution

The builtins namespace associated with the execution of a code block is actually
found by looking up the name ``__builtins__`` in its global namespace; this
should be a dictionary or a module (in the latter case the module's dictionary
is used).  By default, when in the :mod:`__main__` module, ``__builtins__`` is
the built-in module :mod:`builtins`; when in any other module,
``__builtins__`` is an alias for the dictionary of the :mod:`builtins` module
itself.  ``__builtins__`` can be set to a user-created dictionary to create a
weak form of restricted execution.

代码块执行中的内置名字空间,实际上是通过查找名字 ``__builtins__`` 做到的. 这应该是一个字典,或者是一个模块 (使用模块的字典) . 默认情况下,在模块 :mod:`__main__` 里, ``__builtins__`` 是内置模块 :mod:`builtins` ; 在其他模块里, ``__builtins__`` 是 :mod:`builtins` 模块字典的一个别名.  ``__builtins__`` 也可以是一个用户创建的字典,以创建一种弱形式的受限执行环境. 

.. impl-detail::

   Users should not touch ``__builtins__``; it is strictly an implementation
   detail.  Users wanting to override values in the builtins namespace should
   :keyword:`import` the :mod:`builtins` module and modify its
   attributes appropriately.

   用户不应该干涉 ``__builtins__`` ,严格地讲,这属于实现的细节. 希望覆盖内置名字空间的用户,应该使用导入 :mod:`builtins` 模块,适当修改它的属性. 

.. index:: module: __main__

The namespace for a module is automatically created the first time a module is
imported.  The main module for a script is always called :mod:`__main__`.

The :keyword:`global` statement has the same scope as a name binding operation
in the same block.  If the nearest enclosing scope for a free variable contains
a global statement, the free variable is treated as a global.

A class definition is an executable statement that may use and define names.
These references follow the normal rules for name resolution.  The namespace of
the class definition becomes the attribute dictionary of the class.  Names
defined at the class scope are not visible in methods.

类定义语句是一条可以使用和定义名字的可执行语句. 这些引用遵循名字解析的正常规则. 类定义的名字空间会成为类的属性字典. 类作用域内定义的名字在方法里是不可见的. 

.. _dynamic-features:

动态交互
---------------------------------

There are several cases where Python statements are illegal when used in
conjunction with nested scopes that contain free variables.

在几种情况下,在包括自由变量的嵌套作用域中使用某些Python语句是不合法的. 

If a variable is referenced in an enclosing scope, it is illegal to delete the
name.  An error will be reported at compile time.

如果在一个封闭作用域中引用了某名字,那么删除该名字就是非法的. 这会导致一个编译错误. 

If the wild card form of import --- ``import *`` --- is used in a function and
the function contains or is a nested block with free variables, the compiler
will raise a :exc:`SyntaxError`.

如果在函数里使用了 ``import *`` 式的 import 语句,并且函数包括或本身就是一个有自由变量的嵌套块,那么编译器会抛出异常 :exc:`SyntaxError` . 

.. XXX from * also invalid with relative imports (at least currently)

The :func:`eval` and :func:`exec` functions do not have access to the full
environment for resolving names.  Names may be resolved in the local and global
namespaces of the caller.  Free variables are not resolved in the nearest
enclosing namespace, but in the global namespace.  [#]_ The :func:`exec` and
:func:`eval` functions have optional arguments to override the global and local
namespace.  If only one namespace is specified, it is used for both.

函数 :func:`eval` 和 :func:`exec` 在解析名字时没有访问全部环境的能力. 名字可以在调用者的局部和变局名字空间内解析. 自由变量不会在最接近的封闭作用域内解析,而发生在全局作用域里. [1] 可以为函数 :func:`exec` 和 :func:`eval` 提供可选参数覆盖全局和局部名字空间. 如果只提供了一个参数,这个参数就会用当作两种名字空间使用. 

.. _exceptions:

异常
==========

.. index:: single: exception

.. index::
   single: raise an exception
   single: handle an exception
   single: exception handler
   single: errors
   single: error handling

Exceptions are a means of breaking out of the normal flow of control of a code
block in order to handle errors or other exceptional conditions.  An exception
is *raised* at the point where the error is detected; it may be *handled* by the
surrounding code block or by any code block that directly or indirectly invoked
the code block where the error occurred.

 "异常" 是一种打破代码块正常执行流的方法,用于处理错误和其他异常条件. 异常会在检查到错误的点上抛出. 它可以在就近的代码块内处理,也可能在任何调用(直接或者间接调用)错误发生块的代码块中处理. 

The Python interpreter raises an exception when it detects a run-time error
(such as division by zero).  A Python program can also explicitly raise an
exception with the :keyword:`raise` statement. Exception handlers are specified
with the :keyword:`try` ... :keyword:`except` statement.  The :keyword:`finally`
clause of such a statement can be used to specify cleanup code which does not
handle the exception, but is executed whether an exception occurred or not in
the preceding code.

Python解释器会在发现运行时错误时 (例如除零) 抛出异常. Python程序也可以使用 :keyword:`raise` 语句抛出异常. 可以使用 :keyword:`try` ... :keyword:`except` 指定异常处理者. 这个语句的 :keyword:`finally` 子句可以指定一段并不专门用于处理异常的代码,不过,这些代码无论异常出现与否都会得到执行. 

.. index:: single: termination model

Python uses the "termination" model of error handling: an exception handler can
find out what happened and continue execution at an outer level, but it cannot
repair the cause of the error and retry the failing operation (except by
re-entering the offending piece of code from the top).

Python采用了错误处理的 "终结" 模型: 异常处理者能够找出发生了什么,并且在外层代码块里继续执行,但是它不能修复错误和重试失败的操作 (除了重新进入 "当事" 代码) . 

.. index:: single: SystemExit (built-in exception)

When an exception is not handled at all, the interpreter terminates execution of
the program, or returns to its interactive main loop.  In either case, it prints
a stack backtrace, except when the exception is :exc:`SystemExit`.

如果异常根本没有得到处理,解释器会结束程序的执行,或者返回到交互主循环中. 无论是哪种情况,都会打印一个栈回溯信息,但发生 :exc:`SystemExit` 异常时除外. 

Exceptions are identified by class instances.  The :keyword:`except` clause is
selected depending on the class of the instance: it must reference the class of
the instance or a base class thereof.  The instance can be received by the
handler and can carry additional information about the exceptional condition.

异常是由一个类实例标识的. 根据异常实例的类选择使用哪条 :keyword:`except` 子句处理异常,这个子句必须引用这个实例的类或者父类. 异常处理者可以接收这个实例,实例也能够携带异常条件的额外信息. 

.. note::

   Exception messages are not part of the Python API.  Their contents may change
   from one version of Python to the next without warning and should not be
   relied on by code which will run under multiple versions of the interpreter.

   异常消息并不作为Python API的一部分. 不同版本的Python可能没有任何警告的前提下修改其内容,因此要在多个版本的解释器上运行的代码不应该依赖于它. 

See also the description of the :keyword:`try` statement in section :ref:`try`
and :keyword:`raise` statement in section :ref:`raise`.

关于异常,也可以参考在 :ref:`try` 一节关于 :keyword:`try` 语句的介绍,以及 :ref:`raise` 一节中 :keyword:`raise` 语句的介绍. 

.. rubric:: Footnotes

.. [#] This limitation occurs because the code that is executed by these operations
       is not available at the time the module is compiled.

        
       这个限制的原因在于编译该模块时,这些操作执行的代码还是无效的. 

