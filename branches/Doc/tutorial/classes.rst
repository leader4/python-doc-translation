.. _tut-classes:

***********
Classes 类
***********

Compared with other programming languages, Python's class mechanism adds classes
with a minimum of new syntax and semantics.  It is a mixture of the class
mechanisms found in C++ and Modula-3.  Python classes provide all the standard
features of Object Oriented Programming: the class inheritance mechanism allows
multiple base classes, a derived class can override any methods of its base
class or classes, and a method can call the method of a base class with the same
name.  Objects can contain arbitrary amounts and kinds of data.  As is true for
modules, classes partake of the dynamic nature of Python: they are created at
runtime, and can be modified further after creation.

同别的编程语言相比, Python 的类机制中增加了少量新的语法和语义. 它是 C++
的类机制和 Modula-3 的类机制的混合体. Python 类提供了面向对象编程的所有基本特征:
允许多继承的类继承机制, 派生类可以重写它父类的任何方法,
一个方法可以调用父类中重名的方法. 对象可以包含任意数量和类型的数据成员.
作为模块, 类也拥有 Python 的动态特征: 他们可以被动态创建,
并且可以在创建之后被修改.

In C++ terminology, normally class members (including the data members) are
*public* (except see below :ref:`tut-private`), and all member functions are
*virtual*.  As in Modula-3, there are no shorthands for referencing the object's
members from its methods: the method function is declared with an explicit first
argument representing the object, which is provided implicitly by the call.  As
in Smalltalk, classes themselves are objects.  This provides semantics for
importing and renaming.  Unlike C++ and Modula-3, built-in types can be used as
base classes for extension by the user.  Also, like in C++, most built-in
operators with special syntax (arithmetic operators, subscripting etc.) can be
redefined for class instances.

从C++术语上讲, Python 类的成员 (包括数据成员) 通常都是 *public* 的 (例外见下 :ref:`tut-private`), 
并且所有的成员函数都是 *virtual* 的. 和 Modula-3 中一样, Python
中没有关联对象成员和方法的隐式表达: 所有方法函数在声明时显式地将第一个参数表示为对象,
这个参数的值在方法被调用时隐式赋值. 同 Smalltalk 类似, Python 类本身就是对象.
这就提供了导入和重命名的语义. 与 C++ 和 Modula-3 不同的是, Python
的内置类型可以被当做基类来让使用者扩展. 另外, 像 C++ 一样,
大多数有特殊语法的内置操作符(算数运算符, 下标操作符等等) 在类的实例中都可以重定义.

(Lacking universally accepted terminology to talk about classes, I will make
occasional use of Smalltalk and C++ terms.  I would use Modula-3 terms, since
its object-oriented semantics are closer to those of Python than C++, but I
expect that few readers have heard of it.)

(由于在谈论类的时候缺乏公认的术语, 我会偶尔使用 Smalltalk 和 C++ 的术语.
我更愿意用 Modula-3的术语, 因为它面向对象的语义比C++更贴近Python,
但是我估计没有读者听过这个说法.)
.. _tut-object:

A Word About Names and Objects 关于名称和对象的讨论
====================================================

Objects have individuality, and multiple names (in multiple scopes) can be bound
to the same object.  This is known as aliasing in other languages.  This is
usually not appreciated on a first glance at Python, and can be safely ignored
when dealing with immutable basic types (numbers, strings, tuples).  However,
aliasing has a possibly surprising effect on the semantics of Python code
involving mutable objects such as lists, dictionaries, and most other types.
This is usually used to the benefit of the program, since aliases behave like
pointers in some respects.  For example, passing an object is cheap since only a
pointer is passed by the implementation; and if a function modifies an object
passed as an argument, the caller will see the change --- this eliminates the
need for two different argument passing mechanisms as in Pascal.

对象都具有个别性, 多个名称(在多个作用域中) 可以被绑定到同一个对象上.
这就是其他语言中所谓的别名. 通常第一次接触 Python 可能不会意识到这一点,
而且在处理不变的基本类型(数值, 字符串, 元组)时这一点可能会被安全的忽略.
但是, 在涉及到可变对象如 lists, dictionaries, 以及大多数其他类型时,
别名可能会在 Python 代码的语义上起到惊人的效果. 别名通常对编程有益处,
因为别名在某些方面表现得像指针. 比如, 由于在实现的时候传递的是指针,
所以传递一个对象的开销很小;  又比如将对象作为参数传递给一个函数来对它进行修改,
调用者将会看到对象的变化 --- 这就消除了像Pascal 语言中的两个不同参数之间的传递机制的必要.

.. _tut-scopes:

Python Scopes and Namespaces Python 的作用域和命名空间
========================================================

Before introducing classes, I first have to tell you something about Python's
scope rules.  Class definitions play some neat tricks with namespaces, and you
need to know how scopes and namespaces work to fully understand what's going on.
Incidentally, knowledge about this subject is useful for any advanced Python
programmer.

在介绍类之前, 我必须先告诉你一些关于 Python 作用域规则的事.
类定义用命名空间玩了一些巧妙的把戏, 而你为了完全理解发生了什么就必须知道命名空间和作用域是怎么工作的.
顺便说一下, 这一主题的知识对任何高级 Python 程序员都是有用的.

Let's begin with some definitions. 让我们从定义开始.

A *namespace* is a mapping from names to objects.  Most namespaces are currently
implemented as Python dictionaries, but that's normally not noticeable in any
way (except for performance), and it may change in the future.  Examples of
namespaces are: the set of built-in names (containing functions such as :func:`abs`, and
built-in exception names); the global names in a module; and the local names in
a function invocation.  In a sense the set of attributes of an object also form
a namespace.  The important thing to know about namespaces is that there is
absolutely no relation between names in different namespaces; for instance, two
different modules may both define a function ``maximize`` without confusion ---
users of the modules must prefix it with the module name.

*命名空间* 是从名称到对象的映射. 大多数命名空间现在的实现就如同 Python 的字典,
但通常这一点并不明显(除了在性能上), 而且它有可能在将来发生改变.

By the way, I use the word *attribute* for any name following a dot --- for
example, in the expression ``z.real``, ``real`` is an attribute of the object
``z``.  Strictly speaking, references to names in modules are attribute
references: in the expression ``modname.funcname``, ``modname`` is a module
object and ``funcname`` is an attribute of it.  In this case there happens to be
a straightforward mapping between the module's attributes and the global names
defined in the module: they share the same namespace!  [#]_

顺便说一下, 我用了 *属性* 这个词来称呼任何点后面跟的名称 --- 比如,
在表达式``z.real``中, ``real``就是对象``z``的属性. 更直接的说,
对模块中名称的引用就是属性引用: 在表达式 ``modname.funcname`` 中, ``modname``
是模块对象而 ``funcname`` 是它的一个属性.
在这种情况下模块的属性和它里面所定义的全局名称之间就刚好有一个直接的映射关系:
他们共享同一个命名空间!  [#]_

Attributes may be read-only or writable.  In the latter case, assignment to
attributes is possible.  Module attributes are writable: you can write
``modname.the_answer = 42``.  Writable attributes may also be deleted with the
:keyword:`del` statement.  For example, ``del modname.the_answer`` will remove
the attribute :attr:`the_answer` from the object named by ``modname``.

属性可以是只读的或可写的. 在后一种情况下, 给属性赋值才是可能的. 模块属性是可写的:
你可以写 ``modname.the_answer = 42``. 可以利用:keyword:`del` 语句来删除可写属性.
例如, ``del modname.the_answer`` 将从名为 ``modname`` 的模块中移除属性 :attr:`the_answer`.

Namespaces are created at different moments and have different lifetimes.  The
namespace containing the built-in names is created when the Python interpreter
starts up, and is never deleted.  The global namespace for a module is created
when the module definition is read in; normally, module namespaces also last
until the interpreter quits.  The statements executed by the top-level
invocation of the interpreter, either read from a script file or interactively,
are considered part of a module called :mod:`__main__`, so they have their own
global namespace.  (The built-in names actually also live in a module; this is
called :mod:`builtins`.)

命名空间们是在不同时刻创建的,并且有着不同的生命期. 包含内置名称的命名空间是在 Python
解释器启动时创建的, 而且它永远不被删除. 一个模块的全局命名空间在模块的定义被读取的时候创建;
通常情况下, 模块的命名空间一直持续到解释器退出时. 被最高级别的解释器调用的语句,
不论是从脚本还是从交互读取的, 都被认为是一个名叫 :mod:`__main__` 的模块的一部分,
所以它们有自己的全局命名空间. (内置名称实际上也存在于一个模块中; 这个模块叫 :mod:`builtins`.)

The local namespace for a function is created when the function is called, and
deleted when the function returns or raises an exception that is not handled
within the function.  (Actually, forgetting would be a better way to describe
what actually happens.)  Of course, recursive invocations each have their own
local namespace.

A *scope* is a textual region of a Python program where a namespace is directly
accessible.  "Directly accessible" here means that an unqualified reference to a
name attempts to find the name in the namespace.

Although scopes are determined statically, they are used dynamically. At any
time during execution, there are at least three nested scopes whose namespaces
are directly accessible:

* the innermost scope, which is searched first, contains the local names
* the scopes of any enclosing functions, which are searched starting with the
  nearest enclosing scope, contains non-local, but also non-global names
* the next-to-last scope contains the current module's global names
* the outermost scope (searched last) is the namespace containing built-in names

If a name is declared global, then all references and assignments go directly to
the middle scope containing the module's global names.  To rebind variables
found outside of the innermost scope, the :keyword:`nonlocal` statement can be
used; if not declared nonlocal, those variable are read-only (an attempt to
write to such a variable will simply create a *new* local variable in the
innermost scope, leaving the identically named outer variable unchanged).

Usually, the local scope references the local names of the (textually) current
function.  Outside functions, the local scope references the same namespace as
the global scope: the module's namespace. Class definitions place yet another
namespace in the local scope.

It is important to realize that scopes are determined textually: the global
scope of a function defined in a module is that module's namespace, no matter
from where or by what alias the function is called.  On the other hand, the
actual search for names is done dynamically, at run time --- however, the
language definition is evolving towards static name resolution, at "compile"
time, so don't rely on dynamic name resolution!  (In fact, local variables are
already determined statically.)

A special quirk of Python is that -- if no :keyword:`global` statement is in
effect -- assignments to names always go into the innermost scope.  Assignments
do not copy data --- they just bind names to objects.  The same is true for
deletions: the statement ``del x`` removes the binding of ``x`` from the
namespace referenced by the local scope.  In fact, all operations that introduce
new names use the local scope: in particular, :keyword:`import` statements and
function definitions bind the module or function name in the local scope.

The :keyword:`global` statement can be used to indicate that particular
variables live in the global scope and should be rebound there; the
:keyword:`nonlocal` statement indicates that particular variables live in
an enclosing scope and should be rebound there.

.. _tut-scopeexample:

Scopes and Namespaces Example
-----------------------------

This is an example demonstrating how to reference the different scopes and
namespaces, and how :keyword:`global` and :keyword:`nonlocal` affect variable
binding::

   def scope_test():
       def do_local():
           spam = "local spam"
       def do_nonlocal():
           nonlocal spam
           spam = "nonlocal spam"
       def do_global():
           global spam
           spam = "global spam"

       spam = "test spam"
       do_local()
       print("After local assignment:", spam)
       do_nonlocal()
       print("After nonlocal assignment:", spam)
       do_global()
       print("After global assignment:", spam)

   scope_test()
   print("In global scope:", spam)

The output of the example code is::

   After local assignment: test spam
   After nonlocal assignment: nonlocal spam
   After global assignment: nonlocal spam
   In global scope: global spam

Note how the *local* assignment (which is default) didn't change *scope_test*\'s
binding of *spam*.  The :keyword:`nonlocal` assignment changed *scope_test*\'s
binding of *spam*, and the :keyword:`global` assignment changed the module-level
binding.

You can also see that there was no previous binding for *spam* before the
:keyword:`global` assignment.


.. _tut-firstclasses:

A First Look at Classes
=======================

Classes introduce a little bit of new syntax, three new object types, and some
new semantics.

类引入了一些新的语法, 三种新的对象类型, 和一些新的语义.


.. _tut-classdefinition:

Class Definition Syntax
-----------------------

The simplest form of class definition looks like this:

最简单的类的定义形式看起来像这样:

::

   class ClassName:
       <statement-1>
       .
       .
       .
       <statement-N>

Class definitions, like function definitions (:keyword:`def` statements) must be
executed before they have any effect.  (You could conceivably place a class
definition in a branch of an :keyword:`if` statement, or inside a function.)

类的定义, 和函数定义 (:keyword:`def` 语句) 一样必须在使用它们前执行.
(你可以将一个类定义放置于 :keyword:`if` 语句的分支中, 或一个函数中.)

In practice, the statements inside a class definition will usually be function
definitions, but other statements are allowed, and sometimes useful --- we'll
come back to this later.  The function definitions inside a class normally have
a peculiar form of argument list, dictated by the calling conventions for
methods --- again, this is explained later.

事实上, 类定义内部的语句一般是函数的定义, 但其他的语句也是允许的,
而且还很有用 --- 我们在后面将会继续讨论该问题. 
类内的函数定义一般有一个特殊形式的参数列表, 习惯上称之为方法 --- 同样, 
也将在后面解释.

When a class definition is entered, a new namespace is created, and used as the
local scope --- thus, all assignments to local variables go into this new
namespace.  In particular, function definitions bind the name of the new
function here.

当进入一个类定义, 新的命名空间就被创建了, 这一般作为局部的作用域 --- 
因此, 所有的局部变量都在这个新的作用域中. 特别是, 函数定义会绑定.

When a class definition is left normally (via the end), a *class object* is
created.  This is basically a wrapper around the contents of the namespace
created by the class definition; we'll learn more about class objects in the
next section.  The original local scope (the one in effect just before the class
definition was entered) is reinstated, and the class object is bound here to the
class name given in the class definition header (:class:`ClassName` in the
example).

当离开一个类定义后, 一个 *class object* 就被创建. 
通过类的定义, 就将这个命名空间包装了起来; 我们将在后面学到更多关于类对象的知识.
原来的局部作用域 (在进入一个类定义前的作用域) 将会复位,
而类对象就会在这里绑定, 并且命名为类定义时的名字 (在此例中是 :class:`ClassName`).


.. _tut-classobjects:

Class Objects
-------------

Class objects support two kinds of operations: attribute references and
instantiation.

类对象支持两种操作: 属性引用和实例化.

*Attribute references* use the standard syntax used for all attribute references
in Python: ``obj.name``.  Valid attribute names are all the names that were in
the class's namespace when the class object was created.  So, if the class
definition looked like this:

*属性引用* 使用的语法和 Python 中所有的属性引用一样.
合法的属性名是那些在类的命名空间中定义的名字.
所以一个类定义如果是这样:

::

   class MyClass:
       """A simple example class"""
       i = 12345
       def f(self):
           return 'hello world'

then ``MyClass.i`` and ``MyClass.f`` are valid attribute references, returning
an integer and a function object, respectively. Class attributes can also be
assigned to, so you can change the value of ``MyClass.i`` by assignment.
:attr:`__doc__` is also a valid attribute, returning the docstring belonging to
the class: ``"A simple example class"``.

那么, ``MyClass.i`` 和 ``MyClass.f`` 就是合法的属性引用,
分别返回一个整数和一个函数对象. 类属性也可以被指定, 所以你可以给 ``MyClass.i``
赋值以改变其数值. :attr:`__doc__` 也是一个合法的属性, 
返回属于这个类的 docstring : ``"A simple example class"``.

Class *instantiation* uses function notation.  Just pretend that the class
object is a parameterless function that returns a new instance of the class.
For example (assuming the above class):

类的 *实例化* 使用函数的形式. 只要当作一个无参的函数然后返回一个类的实例就可以了.
比如 (假设有前面的类了):

::

   x = MyClass()

creates a new *instance* of the class and assigns this object to the local
variable ``x``.

创建了一个新的实例, 并且将其指定给局部变量 ``x``.

The instantiation operation ("calling" a class object) creates an empty object.
Many classes like to create objects with instances customized to a specific
initial state. Therefore a class may define a special method named
:meth:`__init__`, like this:

实例化的操作 ("调用" 一个类对象) 创建了空的对象.
在创建实例时, 很多类可能都需要有特定的初始状态.
所以一个类可以定义一个特殊的方法, 称为 :meth:`__init__`, 像这样:

::

   def __init__(self):
       self.data = []

When a class defines an :meth:`__init__` method, class instantiation
automatically invokes :meth:`__init__` for the newly-created class instance.  So
in this example, a new, initialized instance can be obtained by:

当一个类定义了 :meth:`__init__` 方法, 类在实例化时会自动调用 :meth:`__init__`
方法, 用于创建新的类实例. 所以在这个例子中, 一个新的初始化过的实例被创建:

::

   x = MyClass()

Of course, the :meth:`__init__` method may have arguments for greater
flexibility.  In that case, arguments given to the class instantiation operator
are passed on to :meth:`__init__`.  For example, 

当然, 为了更大的灵活性, 方法 :meth:`__init__` 可以有更多的参数.
在这种情况下, 给类的参数会传给 :meth:`__init__`. 例如,

::

   >>> class Complex:
   ...     def __init__(self, realpart, imagpart):
   ...         self.r = realpart
   ...         self.i = imagpart
   ...
   >>> x = Complex(3.0, -4.5)
   >>> x.r, x.i
   (3.0, -4.5)


.. _tut-instanceobjects:

Instance Objects
----------------

Now what can we do with instance objects?  The only operations understood by
instance objects are attribute references.  There are two kinds of valid
attribute names, data attributes and methods.

那么我们现在可以对实例对象做什么? 实例对象唯一能理解的操作就是属性引用.
有两种合法的属性, 数据属性和方法.

*data attributes* correspond to "instance variables" in Smalltalk, and to "data
members" in C++.  Data attributes need not be declared; like local variables,
they spring into existence when they are first assigned to.  For example, if
``x`` is the instance of :class:`MyClass` created above, the following piece of
code will print the value ``16``, without leaving a trace:

*data attribute* 在 Smalltalk 中相应于 "instance variable",
在 C++ 中相应于 "data member". 数据属性不需要声明; 像局部变量,
当它们第一次指定时就会被引入. 比如, 如果 ``x`` 是前面创建的 :class:`MyClass`
的实例, 那么下面的例子就会打印出 ``16``, 而不会有问题:

::

   x.counter = 1
   while x.counter < 10:
       x.counter = x.counter * 2
   print(x.counter)
   del x.counter

The other kind of instance attribute reference is a *method*. A method is a
function that "belongs to" an object.  (In Python, the term method is not unique
to class instances: other object types can have methods as well.  For example,
list objects have methods called append, insert, remove, sort, and so on.
However, in the following discussion, we'll use the term method exclusively to
mean methods of class instance objects, unless explicitly stated otherwise.)

实例属性引用的另一种是方法. 一个方法就是 "属于" 一个对象的函数.
(在 Python 中, 方法的概念并不是类实例所特有: 其他对象类型也可以有方法.
例如, 列表对象有 append, insert, remove, sort, 及等等的方法.
但是, 在下面的讨论中, 我们指的就是类实例对象的方法, 除非特别指出.)

.. index:: object: method

Valid method names of an instance object depend on its class.  By definition,
all attributes of a class that are function  objects define corresponding
methods of its instances.  So in our example, ``x.f`` is a valid method
reference, since ``MyClass.f`` is a function, but ``x.i`` is not, since
``MyClass.i`` is not.  But ``x.f`` is not the same thing as ``MyClass.f`` --- it
is a *method object*, not a function object.

合法的方法名依赖于实例的类. 在定义中, 类的属性如果是那些定义的函数对象,
而这也就是实例的方法. 所以在我们的例子中, ``x.f`` 是一个合法的方法引用,
因为 ``MyClass.f`` 是一个函数, 但是 ``x.i`` 就不是, 因为 ``MyClass.i`` 就不是.
但是 ``x.f`` 和 ``MyClass.f`` 并不一样 --- 它是一个 *method object*,
而不是 *function object*.


.. _tut-methodobjects:

Method Objects
--------------

Usually, a method is called right after it is bound:

通常, 一个方法在其绑定后就可以调用了:

::

   x.f()

In the :class:`MyClass` example, this will return the string ``'hello world'``.
However, it is not necessary to call a method right away: ``x.f`` is a method
object, and can be stored away and called at a later time.  For example:

在 :class:`MyClass` 这个例子中, 这将会返回字符串 ``'hello world'``.
但是, 像这样的调用并不是必须的: ``x.f`` 是一个方法对象,
它可以被保存起来以供下次调用. 例如:

::

   xf = x.f
   while True:
       print(xf())

will continue to print ``hello world`` until the end of time.

将会齿数的打印 ``'hello world'``.

What exactly happens when a method is called?  You may have noticed that
``x.f()`` was called without an argument above, even though the function
definition for :meth:`f` specified an argument.  What happened to the argument?
Surely Python raises an exception when a function that requires an argument is
called without any --- even if the argument isn't actually used...

那么在方法调用是发生了什么? 你可能注意到 ``x.f()`` 调用时并没有参数,
尽管 :meth:`f` 定义时是有一个参数的. 那么这个参数怎么了?
当然, Python 在一个参数缺少时调用一个函数是会发生异常的 ---
就算这个参数没有真正用到...


Actually, you may have guessed the answer: the special thing about methods is
that the object is passed as the first argument of the function.  In our
example, the call ``x.f()`` is exactly equivalent to ``MyClass.f(x)``.  In
general, calling a method with a list of *n* arguments is equivalent to calling
the corresponding function with an argument list that is created by inserting
the method's object before the first argument.

事实上, 你会猜想到: 关于方法, 特殊的东西就是, 对象作为参数传递给了函数的第一个参数.
在我们的例子中, ``x.f()`` 是严格等价于 ``MyClass.f(x)``. 在多数情况下,
调用一个方法 (有个 *n* 个参数), 和调用相应的函数 (也有那 n 个参数, 
但是再额外加入一个使用该方法的对象), 是等价的.

If you still don't understand how methods work, a look at the implementation can
perhaps clarify matters.  When an instance attribute is referenced that isn't a
data attribute, its class is searched.  If the name denotes a valid class
attribute that is a function object, a method object is created by packing
(pointers to) the instance object and the function object just found together in
an abstract object: this is the method object.  When the method object is called
with an argument list, a new argument list is constructed from the instance
object and the argument list, and the function object is called with this new
argument list.

如果你仍然不知道方法如何工作, 那么看看实现或许会解决这些问题.
当一个实例属性被引用时, 但是不是数据属性, 那么它的类将被搜索.
如果该名字代表一个合法的类属性并且是一个函数对象, 一个方法对象就会被创建, 
通过包装 (指向) 实例对象, 
而函数对象仍然只是在抽象的对象中: 这就是方法对象.
当方法对象用一个参数列表调用, 新的参数列表会从实例对象中重新构建,
然后函数对象则调用新的参数列表.



.. _tut-remarks:

Random Remarks
==============

.. These should perhaps be placed more carefully...

Data attributes override method attributes with the same name; to avoid
accidental name conflicts, which may cause hard-to-find bugs in large programs,
it is wise to use some kind of convention that minimizes the chance of
conflicts.  Possible conventions include capitalizing method names, prefixing
data attribute names with a small unique string (perhaps just an underscore), or
using verbs for methods and nouns for data attributes.

数据属性覆写了同名的方法属性; 为了避免这个偶然的名字冲突,
在大型的程序中这会导致很难寻找的 bug, 使用某些命名约定是非常明智的,
这样可以最小的避免冲突. 可能的约定包括大写方法名称, 
在数据类型前增加特殊的前缀 (或者就是一个下划线),
或对于方法使用动词, 而数据成员则使用名词.

Data attributes may be referenced by methods as well as by ordinary users
("clients") of an object.  In other words, classes are not usable to implement
pure abstract data types.  In fact, nothing in Python makes it possible to
enforce data hiding --- it is all based upon convention.  (On the other hand,
the Python implementation, written in C, can completely hide implementation
details and control access to an object if necessary; this can be used by
extensions to Python written in C.)

数据属性可以被该类的方法或者普通的用户 ("客户") 引用.
换句话说, 类是不能实现完全的抽象数据类型.
事实上, 在 Python 中没有任何东西是强制隐藏的 --- 这完全是基于约定.
(在另一方面, Python 是用 C 实现的, 这样就可以实现细节的隐藏和控制访问;
这可以通过编写 Python 的扩展实现.)

Clients should use data attributes with care --- clients may mess up invariants
maintained by the methods by stamping on their data attributes.  Note that
clients may add data attributes of their own to an instance object without
affecting the validity of the methods, as long as name conflicts are avoided ---
again, a naming convention can save a lot of headaches here.

客户需要小心地使用数据属性 --- 客户会弄乱被方法控制的不变量, 
通过使用它们自己的方法属性. 注意用户可以增加它们自己的数据到实例对象上,
而没有检查有没有影响方法的有效性, 只要避免名字冲突 --
在说一次, 命名约定可以避免很多这样令人头疼的问题.

There is no shorthand for referencing data attributes (or other methods!) from
within methods.  I find that this actually increases the readability of methods:
there is no chance of confusing local variables and instance variables when
glancing through a method.

在引用数据属性 (或其他方法 !) 并没有快速的方法.
我发现这的确增加了方法的可读性: 这样就不会被局部变量和实例中的变量所困惑,
特别是在随便看看一个方法时.


Often, the first argument of a method is called ``self``.  This is nothing more
than a convention: the name ``self`` has absolutely no special meaning to
Python.  Note, however, that by not following the convention your code may be
less readable to other Python programmers, and it is also conceivable that a
*class browser* program might be written that relies upon such a convention.

通常, 方法的第一个参数称为 ``self``. 这更多的只是约定:
``self`` 对于 Python 来说没有任何意义. 但注意, 如果不遵循这个约定,
对于其他的程序员来说就比较难以理解了, 一个 *class browser* 程序可能会依赖此约定.

Any function object that is a class attribute defines a method for instances of
that class.  It is not necessary that the function definition is textually
enclosed in the class definition: assigning a function object to a local
variable in the class is also ok.  For example:

作为类属性的任何函数对象, 定义了一个方法用于那个类的实例.
函数是否在一个类体中其实并不重要: 指定一个函数对象给类中的局部变量也是可以的.
例如:

::

   # Function defined outside the class
   def f1(self, x, y):
       return min(x, x+y)

   class C:
       f = f1
       def g(self):
           return 'hello world'
       h = g

Now ``f``, ``g`` and ``h`` are all attributes of class :class:`C` that refer to
function objects, and consequently they are all methods of instances of
:class:`C` --- ``h`` being exactly equivalent to ``g``.  Note that this practice
usually only serves to confuse the reader of a program.

现在 ``f``, ``g`` 和 ``h`` 都是类 :class:`C` 的属性, 并且指向函数对象,
而且都是类 :class:`C` 实例的方法 --- ``h`` 和 ``g`` 是等价的.
注意这个只会是读者感到困惑.

Methods may call other methods by using method attributes of the ``self``
argument:

方法可以通过使用 ``self`` 参数调用其他的方法:

::

   class Bag:
       def __init__(self):
           self.data = []
       def add(self, x):
           self.data.append(x)
       def addtwice(self, x):
           self.add(x)
           self.add(x)

Methods may reference global names in the same way as ordinary functions.  The
global scope associated with a method is the module containing the class
definition.  (The class itself is never used as a global scope.)  While one
rarely encounters a good reason for using global data in a method, there are
many legitimate uses of the global scope: for one thing, functions and modules
imported into the global scope can be used by methods, as well as functions and
classes defined in it.  Usually, the class containing the method is itself
defined in this global scope, and in the next section we'll find some good
reasons why a method would want to reference its own class.

方法可以引用全局变量, 就像普通函数中那样. 与这个方法相关的全局作用域,
是包含那个类定义的模块. (类本身永远不会作为全局作用域使用.)
如果的确需要在方法中使用全局数据, 那么需要合法的使用:
首先一件事, 被导入全局作用域的函数和模块可以被方法使用,
就如定义在里面的函数和类一样. 通常来说, 定义在全局作用域中, 包含方法的类是它自己本身,
并且在后面我们会知道为何方法应该引用自己的类.

Each value is an object, and therefore has a *class* (also called its *type*).
It is stored as ``object.__class__``.

每个值都是一个对象, 所以对于 *class* (或称为它的 *type*) 也是这样.
它存于 ``object.__class__``.


.. _tut-inheritance:

Inheritance
===========

Of course, a language feature would not be worthy of the name "class" without
supporting inheritance.  The syntax for a derived class definition looks like
this:

当然, 一个有 "class" 的语言如果没有继承就没有多大的价值了.
派生类的定义如下:

::

   class DerivedClassName(BaseClassName):
       <statement-1>
       .
       .
       .
       <statement-N>

The name :class:`BaseClassName` must be defined in a scope containing the
derived class definition.  In place of a base class name, other arbitrary
expressions are also allowed.  This can be useful, for example, when the base
class is defined in another module:

:class:`BaseClassName` 的定义对于派生类而言必须是可见的.
在基类的地方, 任意的表达式都是允许的. 这就会非常有用, 
比如基类定义在另一个模块:

::

   class DerivedClassName(modname.BaseClassName):

Execution of a derived class definition proceeds the same as for a base class.
When the class object is constructed, the base class is remembered.  This is
used for resolving attribute references: if a requested attribute is not found
in the class, the search proceeds to look in the base class.  This rule is
applied recursively if the base class itself is derived from some other class.

派生类就可以像基类一样使用. 当一个类被构建, 那么它就会记下基类.
这是用于解决属性引用的问题:  当一个属性在这个类中没有被找到,
那么就会去基类中寻找. 然后搜索就会递归, 因为如果基类本身也是从其他的派生.

There's nothing special about instantiation of derived classes:
``DerivedClassName()`` creates a new instance of the class.  Method references
are resolved as follows: the corresponding class attribute is searched,
descending down the chain of base classes if necessary, and the method reference
is valid if this yields a function object.

实例化一个派生类没有什么特别: ``DerivedClassName()`` 会创建这个类的新实例.
方法的引用如下: 相应的类的属性会被搜寻, 如果需要回去搜寻基类,
如果返回一个函数对象, 那么这个引用就是合法的.

Derived classes may override methods of their base classes.  Because methods
have no special privileges when calling other methods of the same object, a
method of a base class that calls another method defined in the same base class
may end up calling a method of a derived class that overrides it.  (For C++
programmers: all methods in Python are effectively ``virtual``.)

派生类会覆写基类的方法. 因为当调用同样的对象的其他方法时方法并没有什么特别的,
基类的方法会因为先调用派生类的方法而被覆写.
(对于 C++ 程序员: 所有的方法在 Python 中都是 ``vitual`` 的.)

An overriding method in a derived class may in fact want to extend rather than
simply replace the base class method of the same name. There is a simple way to
call the base class method directly: just call ``BaseClassName.methodname(self,
arguments)``.  This is occasionally useful to clients as well.  (Note that this
only works if the base class is accessible as ``BaseClassName`` in the global
scope.)

一个在派生类中覆写的方法可能需要基类的方法.
最简单的方式就是直接调用基类的方法: 调用 ``BaseClassName.methodname(self, arguments)``.
这对于可续来说也是很方便的. (这仅在 ``BaseClassName`` 可访问时才有效.)

Python has two built-in functions that work with inheritance:

Python 有两个内置函数用于继承:

* Use :func:`isinstance` to check an instance's type: ``isinstance(obj, int)``
  will be ``True`` only if ``obj.__class__`` is :class:`int` or some class
  derived from :class:`int`.

  使用 :func:`isinstance` 检查实例的类型: ``isinstance(obj, int)``
  只有在 ``obj.__class__`` 是 :class:`int` 或其派生类时才为 ``True``.

* Use :func:`issubclass` to check class inheritance: ``issubclass(bool, int)``
  is ``True`` since :class:`bool` is a subclass of :class:`int`.  However,
  ``issubclass(float, int)`` is ``False`` since :class:`float` is not a
  subclass of :class:`int`.

  使用 :func:`issubclass` 用于检查类的继承关系: ``issubclass(bool, int)``
  会返回 ``True``, 因为 :class:`bool` 是 :class:`int` 的派生类.
  但是, ``issubclass(float, int)`` 会是 ``False`` 因为 :class:`float`
  并不是 :class:`int` 的派生类.



.. _tut-multiple:

Multiple Inheritance
--------------------

Python supports a form of multiple inheritance as well.  A class definition with
multiple base classes looks like this:

Python 支持多重继承. 一个多重继承的类定义看起来像这样:

::

   class DerivedClassName(Base1, Base2, Base3):
       <statement-1>
       .
       .
       .
       <statement-N>

For most purposes, in the simplest cases, you can think of the search for
attributes inherited from a parent class as depth-first, left-to-right, not
searching twice in the same class where there is an overlap in the hierarchy.
Thus, if an attribute is not found in :class:`DerivedClassName`, it is searched
for in :class:`Base1`, then (recursively) in the base classes of :class:`Base1`,
and if it was not found there, it was searched for in :class:`Base2`, and so on.

对于大多数目的, 在最简单的情况下, 你可以将属性搜寻的方式是,
从下至上, 从左到右, 在继承体系中, 同样的类只会被搜寻一次.
如果一个属性在 :class:`DerivedClassName` 中没有被找到,
它就会搜寻 `Base1`, 然后 (递归地) 搜寻 :class:`Base1` 的基类,
然后如果还是没有找到, 那么就会搜索 :class:`Base2`, 等等.

In fact, it is slightly more complex than that; the method resolution order
changes dynamically to support cooperative calls to :func:`super`.  This
approach is known in some other multiple-inheritance languages as
call-next-method and is more powerful than the super call found in
single-inheritance languages.

事实上, 这更加的复杂; 方法的搜寻顺序会根据调用 :func:`super` 而变化.
这个方法在某些其他多重继承的语言中以 call-next-method 被熟知, 
而且比单继承的语言中要有用.

Dynamic ordering is necessary because all cases of multiple inheritance exhibit
one or more diamond relationships (where at least one of the parent classes
can be accessed through multiple paths from the bottommost class).  For example,
all classes inherit from :class:`object`, so any case of multiple inheritance
provides more than one path to reach :class:`object`.  To keep the base classes
from being accessed more than once, the dynamic algorithm linearizes the search
order in a way that preserves the left-to-right ordering specified in each
class, that calls each parent only once, and that is monotonic (meaning that a
class can be subclassed without affecting the precedence order of its parents).
Taken together, these properties make it possible to design reliable and
extensible classes with multiple inheritance.  For more detail, see
http://www.python.org/download/releases/2.3/mro/.

动态的顺序是很有必要的, 因为在那些处于菱形继承体系中 (这里至少有个父类被多次派生).
比如, 所有的类都从 :class:`object` 派生, 所以到达 :class:`object` 的路径不止一条.
为了防止基类被多次访问, 动态的算法线性化了搜寻的路径, 先从左至右搜索指定的类,
然后这样就可以让每个父类只搜寻一次, 并且单一 (这就意味一个类可以被派生,
但是不会影响其父类的搜寻路径. 使用了这些, 就使得以多重继承设计的类更可靠和可扩展.
具体参考http://www.python.org/download/releases/2.3/mro/.


.. _tut-private:

Private Variables
=================

"Private" instance variables that cannot be accessed except from inside an
object don't exist in Python.  However, there is a convention that is followed
by most Python code: a name prefixed with an underscore (e.g. ``_spam``) should
be treated as a non-public part of the API (whether it is a function, a method
or a data member).  It should be considered an implementation detail and subject
to change without notice.

在 Python 之中, 并不存在那种无法访问的 "私有" 变量.
但是, 在多数的 Python 代码中有个约定: 以一个下划线带头的名字 (如 ``_spam``)
应该作为非公共的 API (不管是函数, 方法或者数据成员).
这应该作为具体的实现, 而且变化它也无须提醒.

Since there is a valid use-case for class-private members (namely to avoid name
clashes of names with names defined by subclasses), there is limited support for
such a mechanism, called :dfn:`name mangling`.  Any identifier of the form
``__spam`` (at least two leading underscores, at most one trailing underscore)
is textually replaced with ``_classname__spam``, where ``classname`` is the
current class name with leading underscore(s) stripped.  This mangling is done
without regard to the syntactic position of the identifier, as long as it
occurs within the definition of a class.

因为有一个合法的情况用于使用私有的成员 (名义上是说在派生类中避免名字的冲突),
因此就有这样的一种机制称为 :dfn:`name mangling`. 任何如 ``__spam`` 形式的标识符,
(在开头至少有两个下划线) 将被替换为 ``_classname__spam``, 此处的 ``classname``
就是当前的类. 这样的处理无须关注标识符的句法上的位置,
尽管它是在一个类的定义中.

Note that the mangling rules are designed mostly to avoid accidents; it still is
possible to access or modify a variable that is considered private.  This can
even be useful in special circumstances, such as in the debugger.

注意, 这样的规则只是用于防止冲突; 它仍然可以访问或修改, 尽管认为这是一个私有变量.
在某些特殊情况下, 如测试等, 是有用的.

Notice that code passed to ``exec()`` or ``eval()`` does not consider the
classname of the invoking class to be the current class; this is similar to the
effect of the ``global`` statement, the effect of which is likewise restricted
to code that is byte-compiled together.  The same restriction applies to
``getattr()``, ``setattr()`` and ``delattr()``, as well as when referencing
``__dict__`` directly.

注意, 传递给 ``exec()`` 或 ``eval()`` 的代码并不会考虑被调用类的类名是当前的类;
这个和 ``global`` 语句的效果一样, 字节编译的代码也有同样的限制.
而对于 ``getattr()``, ``setattr()`` 和 ``delattr()`` 也有这种限制,
直接访问 ``__dict__`` 也是有这样的问题.



.. _tut-odds:

Odds and Ends
=============

Sometimes it is useful to have a data type similar to the Pascal "record" or C
"struct", bundling together a few named data items.  An empty class definition
will do nicely:

有些时候, 有类似于 Pascal 的 "record" 或 C 的 "struct" 这样的数据类型非常有用,
绑定一些命名的数据. 一个空的类定义就将很好:

::

   class Employee:
       pass

   john = Employee() # Create an empty employee record

   # Fill the fields of the record
   john.name = 'John Doe'
   john.dept = 'computer lab'
   john.salary = 1000

A piece of Python code that expects a particular abstract data type can often be
passed a class that emulates the methods of that data type instead.  For
instance, if you have a function that formats some data from a file object, you
can define a class with methods :meth:`read` and :meth:`readline` that get the
data from a string buffer instead, and pass it as an argument.

一段 Python 代码中如果希望一个抽象的数据类型, 那么可以通过传递一个类给那个方法,
就好像有了那个数据类型一样. 
(译者注: 我难以理解此话应该如何翻译. 但我的想法, 这应该就如多态一样, 如:

::

    def handle(dt):
        dt.i += 1

在此处, 我们不需要知道 dt 具体是什么类型, 但是只要知道它有一个属性叫 i 就可以了.
这正是 Python 动态绑定的强大之处. 如果大家看到, 请给出好的意见.
)
例如, 如果你有一个函数用于格式化某些从文件对象中读取的数据,
你可以定义一个类, 然后有方法 :meth:`read` 和 :meth:`readline`
用于读取数据, 然后将这个类作为一个参数传递给那个函数.

.. (Unfortunately, this technique has its limitations: a class can't define
   operations that are accessed by special syntax such as sequence subscripting
   or arithmetic operators, and assigning such a "pseudo-file" to sys.stdin will
   not cause the interpreter to read further input from it.)

Instance method objects have attributes, too: ``m.__self__`` is the instance
object with the method :meth:`m`, and ``m.__func__`` is the function object
corresponding to the method.

实例方法对象也有属性: ``m.__self__`` 就是一个方法 :meth:`m` 的实例对象,
而 ``m.__func__`` 是相应于该方法的函数对象.


.. _tut-exceptionclasses:

Exceptions Are Classes Too
==========================

User-defined exceptions are identified by classes as well.  Using this mechanism
it is possible to create extensible hierarchies of exceptions.

用户定义的异常其实也是类. 使用这个机制, 就可以创建可扩展的异常继承体系.

There are two new valid (semantic) forms for the :keyword:`raise` statement:

有两种合法的形式用于 :keyword:`raise` 语句:

::

   raise Class

   raise Instance

In the first form, ``Class`` must be an instance of :class:`type` or of a
class derived from it.  The first form is a shorthand for:

在第一种形式下, ``Class`` 必须是 :class:`type` 的实例或者其派生.
第一种形式可以简化为这样这样:

::

   raise Class()

A class in an :keyword:`except` clause is compatible with an exception if it is
the same class or a base class thereof (but not the other way around --- an
except clause listing a derived class is not compatible with a base class).  For
example, the following code will print B, C, D in that order:

一个在 :keyword:`except` 中的类, 可以与一个异常相容, 如果该异常是同样的类,
或是它的基类 (但是并不是另一种 -- 一个 except 语句列出的派生类与其基类并不相容).
如下面的代码, 以那种顺序打印出 B, C, D:

::

   class B(Exception):
       pass
   class C(B):
       pass
   class D(C):
       pass

   for c in [B, C, D]:
       try:
           raise c()
       except D:
           print("D")
       except C:
           print("C")
       except B:
           print("B")

Note that if the except clauses were reversed (with ``except B`` first), it
would have printed B, B, B --- the first matching except clause is triggered.

但是注意, 如果 except 语句是反着的 (先用 ``except B``),
那么打印的结果将是 B, B, B -- 第一个总是匹配.

When an error message is printed for an unhandled exception, the exception's
class name is printed, then a colon and a space, and finally the instance
converted to a string using the built-in function :func:`str`.

当因为一个未处理的异常发生时, 错误信息将被打印, 异常的类名将被打印,
然后是一个冒号和空格, 最后是使用 :func:`str` 转换后的实例.


.. _tut-iterators:

Iterators
=========

By now you have probably noticed that most container objects can be looped over
using a :keyword:`for` statement:

到目前为止, 你可能注意到, 大多数的容器对象都可以使用 :keyword:`for` 来迭代:

::

   for element in [1, 2, 3]:
       print(element)
   for element in (1, 2, 3):
       print(element)
   for key in {'one':1, 'two':2}:
       print(key)
   for char in "123":
       print(char)
   for line in open("myfile.txt"):
       print(line)

This style of access is clear, concise, and convenient.  The use of iterators
pervades and unifies Python.  Behind the scenes, the :keyword:`for` statement
calls :func:`iter` on the container object.  The function returns an iterator
object that defines the method :meth:`__next__` which accesses elements in the
container one at a time.  When there are no more elements, :meth:`__next__`
raises a :exc:`StopIteration` exception which tells the :keyword:`for` loop to
terminate.  You can call the :meth:`__next__` method using the :func:`next`
built-in function; this example shows how it all works:

这种形式简洁, 明了并且方便. 迭代器的使用遍布于 Python 之中.
在这个外表之下, :keyword:`for` 语句对容器对象调用了 :func:`iter`.
这个函数返回一个迭代器对象, 它定义了 :meth:`__next__` 方法, 
用以在每次访问时得到一个元素. 当没有任何元素时, :meth:`__next__`
将产生 :exc:`StopIteration` 异常, 它告诉 :keyword:`for` 停止迭代.
你可以使用内置函数 :func:`next` 来调用 :meth:`__next__` 方法;
这个例子展示了它如何工作:

::

   >>> s = 'abc'
   >>> it = iter(s)
   >>> it
   <iterator object at 0x00A1DB50>
   >>> next(it)
   'a'
   >>> next(it)
   'b'
   >>> next(it)
   'c'
   >>> next(it)

   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
       next(it)
   StopIteration

Having seen the mechanics behind the iterator protocol, it is easy to add
iterator behavior to your classes.  Define an :meth:`__iter__` method which
returns an object with a :meth:`__next__` method.  If the class defines
:meth:`__next__`, then :meth:`__iter__` can just return ``self``:

在看到迭代器的机制之后, 就可以很简单的将迭代行为增加到你的类中.
定义一个 :meth:`__iter__` 方法用以返回一个具有 :meth:`__next__` 的对象.
如果这个类定义了 :meth:`__next__` , 那么 :meth:`__iter__` 仅需要返回 ``self``:

::

   class Reverse:
       "Iterator for looping over a sequence backwards"
       def __init__(self, data):
           self.data = data
           self.index = len(data)
       def __iter__(self):
           return self
       def __next__(self):
           if self.index == 0:
               raise StopIteration
           self.index = self.index - 1
           return self.data[self.index]

   >>> rev = Reverse('spam')
   >>> iter(rev)
   <__main__.Reverse object at 0x00A1DB50>
   >>> for char in rev:
   ...     print(char)
   ...
   m
   a
   p
   s


.. _tut-generators:

Generators
==========

:term:`Generator`\s are a simple and powerful tool for creating iterators.  They
are written like regular functions but use the :keyword:`yield` statement
whenever they want to return data.  Each time :func:`next` is called on it, the
generator resumes where it left-off (it remembers all the data values and which
statement was last executed).  An example shows that generators can be trivially
easy to create:

:term:`Generator` (生成器) 是一个用于创建迭代器简单而且强大的工具.
它们和普通的函数很像, 但是当它们需要返回值时, 则使用 :keyword:`yield` 语句.
每次 :func:`next` 被调用时, 生成器会从它上次离开的地方继续执行 (
它会记住所有的数据值和最后一次执行的语句). 一个例子用以展示如何创建生成器:

::

   def reverse(data):
       for index in range(len(data)-1, -1, -1):
           yield data[index]

   >>> for char in reverse('golf'):
   ...     print(char)
   ...
   f
   l
   o
   g

Anything that can be done with generators can also be done with class based
iterators as described in the previous section.  What makes generators so
compact is that the :meth:`__iter__` and :meth:`__next__` methods are created
automatically.

任何可用生成器实现的东西都能用基于迭代器的类实现, 这个在前面有所描述.
让生成器看起来很紧密的原因是它自动创建了 :meth:`__iter` 和 :meth:`__next__`.

Another key feature is that the local variables and execution state are
automatically saved between calls.  This made the function easier to write and
much more clear than an approach using instance variables like ``self.index``
and ``self.data``.

另一个关键的特性在于, 局部变量和执行状态都被自动保存下来.
这就使函数更容易编写并且更加清晰, 相对于使用实例的变量, 如 ``self.index``
和 ``self.data``.

In addition to automatic method creation and saving program state, when
generators terminate, they automatically raise :exc:`StopIteration`. In
combination, these features make it easy to create iterators with no more effort
than writing a regular function.

除了自动创建方法和保存程序状态, 当生成器终止时, 它们会自动产生 :exc:`StopIteration`
异常. 在这些结合起来后, 这就使得能够很简单的创建迭代器, 除了仅需要编写一个函数.


.. _tut-genexps:

Generator Expressions
=====================

Some simple generators can be coded succinctly as expressions using a syntax
similar to list comprehensions but with parentheses instead of brackets.  These
expressions are designed for situations where the generator is used right away
by an enclosing function.  Generator expressions are more compact but less
versatile than full generator definitions and tend to be more memory friendly
than equivalent list comprehensions.

有些简单的生成器可以简洁的写出来, 而且和列表推导很类似, 仅仅是将方括号换成了圆括号.
这些表达式设计用于在一个函数中正好可以用生成器的情况. 生成器表达式更加紧密,
但是功能相对来说也少点, 并且与同样的列表推导式来说更节约内存.

Examples 例子::

   >>> sum(i*i for i in range(10))                 # sum of squares
   285

   >>> xvec = [10, 20, 30]
   >>> yvec = [7, 5, 3]
   >>> sum(x*y for x,y in zip(xvec, yvec))         # dot product
   260

   >>> from math import pi, sin
   >>> sine_table = {x: sin(x*pi/180) for x in range(0, 91)}

   >>> unique_words = set(word  for line in page  for word in line.split())

   >>> valedictorian = max((student.gpa, student.name) for student in graduates)

   >>> data = 'golf'
   >>> list(data[i] for i in range(len(data)-1, -1, -1))
   ['f', 'l', 'o', 'g']



.. rubric:: Footnotes

.. [#] Except for one thing.  Module objects have a secret read-only attribute called
   :attr:`__dict__` which returns the dictionary used to implement the module's
   namespace; the name :attr:`__dict__` is an attribute but not a global name.
   Obviously, using this violates the abstraction of namespace implementation, and
   should be restricted to things like post-mortem debuggers.

