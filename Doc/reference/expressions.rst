
.. _expressions:

***********
异常
***********

.. index:: expression, BNF

This chapter explains the meaning of the elements of expressions in Python.

本章描述了Python中表达式组成元素的含义. 

**Syntax Notes:** In this and the following chapters, extended BNF notation will
be used to describe syntax, not lexical analysis.  When (one alternative of) a
syntax rule has the form

**语法注意: ** 在本章和之后的章节中, 将使用扩展BNF记法描述语法, 而不是词法分析. 当某语法规则 (之一) 具有如下形式, 

.. productionlist:: *
   name: `othername`

and no semantics are given, the semantics of this form of ``name`` are the same
as for ``othername``.

并且没有给出语义说明的时候, 这种形式的 ``name`` 与 ``othername`` 具有相同的语义. 

.. _conversions:

算法转换
======================

.. index:: pair: arithmetic; conversion

When a description of an arithmetic operator below uses the phrase "the numeric
arguments are converted to a common type," this means that the operator
implementation for built-in types works that way:

在下面的数值运算符描述中的 "数值型参数被转换为通用类型" , 是指这些操作符对内置类型按如下方式工作: 

* If either argument is a complex number, the other is converted to complex;

  如果其中一个参数是复数, 另一个也要转换成复数; 

* otherwise, if either argument is a floating point number, the other is
  converted to floating point;

  否则, 如果其中一个参数是浮点数, 另一个也要转换成浮点数; 

* otherwise, both must be integers and no conversion is necessary.

  否则, 两个一定都是整数, 不需要转换. 

Some additional rules apply for certain operators (e.g., a string left argument
to the '%' operator).  Extensions must define their own conversion behavior.

某些运算符有特殊的规则(例如,  '%' 操作符左边的字符串操作数) . 扩展必须定义自己的转换行为. 

.. _atoms:

原子 (Atoms) 
=================

.. index:: atom

Atoms are the most basic elements of expressions.  The simplest atoms are
identifiers or literals.  Forms enclosed in parentheses, brackets or braces are
also categorized syntactically as atoms.  The syntax for atoms is:

原子是表达式最基本的组成单位. 最简单的原子是标识符或者字面值. 圆括号、方括号和大括号括住的文本在语法上也看成是原子, 原子的语法如下: 

.. productionlist::
   atom: `identifier` | `literal` | `enclosure`
   enclosure: `parenth_form` | `list_display` | `dict_display` | `set_display`
            : | `generator_expression` | `yield_atom`


.. _atom-identifiers:

标识符(Names)
-------------------

.. index:: name, identifier

An identifier occurring as an atom is a name.  See section :ref:`identifiers`
for lexical definition and section :ref:`naming` for documentation of naming and
binding.

作为原子出现的标识符是一个名字. 关于词法定义可参考 :ref:`identifiers` 节, 名字与绑定的文档可以参考 :ref:`naming` 节. 

.. index:: exception: NameError

When the name is bound to an object, evaluation of the atom yields that object.
When a name is not bound, an attempt to evaluate it raises a :exc:`NameError`
exception.

当某名字绑定的是一个对象时, 对该原子的求值 (evaluation) 就会导出 (yield) 那个对象. 当没有绑定名字而试图对其求值 (evaluate) 时, 就会抛出 :exc:`NameError` 异常. 

.. index::
   pair: name; mangling
   pair: private; names

**Private name mangling:** When an identifier that textually occurs in a class
definition begins with two or more underscore characters and does not end in two
or more underscores, it is considered a :dfn:`private name` of that class.
Private names are transformed to a longer form before code is generated for
them.  The transformation inserts the class name in front of the name, with
leading underscores removed, and a single underscore inserted in front of the
class name.  For example, the identifier ``__spam`` occurring in a class named
``Ham`` will be transformed to ``_Ham__spam``.  This transformation is
independent of the syntactical context in which the identifier is used.  If the
transformed name is extremely long (longer than 255 characters), implementation
defined truncation may happen.  If the class name consists only of underscores,
no transformation is done.

**私有名字变换: ** 在类定义中, 以两个或更多下划线开始, 但不以两个或更多下划线结束的标识符, 作为类的私有名字 ( :dfn:`private name` ) . 在生成代码之前, 私有名字会被变换成更长的形式. 这种变换是, 在其前面插入类名 (类名前的下划线将被去掉) , 并在类名前插入一条下划线. 例如, 在类 ``Ham`` 中定义的标识符 ``__spam`` 会被变换成 ``_Ham__spam`` . 这种变换与使用标识符的语法上下文无关. 如果变换后的结果过长 (超过255个字符) , 实现可能会截短名字. 如果类名只由下划线组成, 就不进行这种变换. 

.. _atom-literals:

字面值 (Literals) 
------------------------

.. index:: single: literal

Python supports string and bytes literals and various numeric literals:

Python支持字符串字面值、字节字面值和各种数值字面值: 

.. productionlist::
   literal: `stringliteral` | `bytesliteral`
          : | `integer` | `floatnumber` | `imagnumber`

Evaluation of a literal yields an object of the given type (string, bytes,
integer, floating point number, complex number) with the given value.  The value
may be approximated in the case of floating point and imaginary (complex)
literals.  See section :ref:`literals` for details.

对字面值求值会得到一个给定值的给定类型的对象 (字符串、字节、整数、浮点数和复数) , 如果是浮点数和虚数 (复数) , 那么这个值可能是个近似值, 详见 :ref:`literals` 一节的介绍. 

.. index::
   triple: immutable; data; type
   pair: immutable; object

With the exception of bytes literals, these all correspond to immutable data
types, and hence the object's identity is less important than its value.
Multiple evaluations of literals with the same value (either the same occurrence
in the program text or a different occurrence) may obtain the same object or a
different object with the same value.

除了字节序列的字面值, 所有字面值都属于不可变的数据类型, 因此对象的标识比起它们的值来说显得次要一些. 多次使用相同的字面值 (反复使用相同的程序代码, 或者在不同的地方出现) 获得的可能是相同的对象或具有相同值的不同对象. 

.. _parenthesized:

括号的形式
-------------------

.. index:: single: parenthesized form

A parenthesized form is an optional expression list enclosed in parentheses:

括号表达式是位于一对圆括号之间的表达式列表 (列表也可为空) . 

.. productionlist::
   parenth_form: "(" [`expression_list`] ")"

A parenthesized expression list yields whatever that expression list yields: if
the list contains at least one comma, it yields a tuple; otherwise, it yields
the single expression that makes up the expression list.

括号内表达式列表的结果取决于其内部表达式列表的结果: 如果表达式列表中包括至少一个逗号, 它就生成一个元组; 否则, 就生成一个由表达式列表组成的表达式. 

.. index:: pair: empty; tuple

An empty pair of parentheses yields an empty tuple object.  Since tuples are
immutable, the rules for literals apply (i.e., two occurrences of the empty
tuple may or may not yield the same object).

一对空圆括号会生成一个空的元组对象. 因为元组是不可变的, 因此适用字面值的规则, 即空元组的两次出现可能 (也可能不) 生成相同对象. 

.. index::
   single: comma
   pair: tuple; display

Note that tuples are not formed by the parentheses, but rather by use of the
comma operator.  The exception is the empty tuple, for which parentheses *are*
required --- allowing unparenthesized "nothing" in expressions would cause
ambiguities and allow common typos to pass uncaught.

请注意元组并不是依靠圆括号构成的, 而是使用逗号. 但空元组是个例外, 此时圆括号是必须的 --- 如果表达式中允许有不加圆括号的" 空" 可能会带来歧义, 出现一些易犯的错误. 

.. _comprehensions:

列表/集合/字典的表达
-----------------------------------------

For constructing a list, a set or a dictionary Python provides special syntax
called "displays", each of them in two flavors:

为了构造列表、集合和字典对象, Python提供了一种特殊语法, 称为 "display" , 分为两类: 

* either the container contents are listed explicitly, or

  要么明确地列出容器对象的内容. 

* they are computed via a set of looping and filtering instructions, called a
  :dfn:`comprehension`.
  
  要么通过一个循环和过滤方法的组合构造, 这称为 `comprehension` . 

Common syntax elements for comprehensions are:

comprehension的通用语法是: 

.. productionlist::
   comprehension: `expression` `comp_for`
   comp_for: "for" `target_list` "in" `or_test` [`comp_iter`]
   comp_iter: `comp_for` | `comp_if`
   comp_if: "if" `expression_nocond` [`comp_iter`]

The comprehension consists of a single expression followed by at least one
:keyword:`for` clause and zero or more :keyword:`for` or :keyword:`if` clauses.
In this case, the elements of the new container are those that would be produced
by considering each of the :keyword:`for` or :keyword:`if` clauses a block,
nesting from left to right, and evaluating the expression to produce an element
each time the innermost block is reached.

comprehension由一个表达式, 后跟至少一个 :keyword:`for` 子句, 然后是一个或多个 :keyword:`for` 或 :keyword:`if` 子句组成. 此时, 这个新容器对象的元素是由每个从左到右嵌套的:keyword:`for` 和 :keyword:`if` 子句产生的. 每次执行到最内层代码块时计算前面那个表达式的值. 

Note that the comprehension is executed in a separate scope, so names assigned
to in the target list don't "leak" in the enclosing scope.

注意, comprehension是在分开的作用域内执行的, 因此, 在目标列表内使用的临时名字是不会 "泄漏" 出到上层作用域的. 

.. _lists:

列表形式
-------------

.. index::
   pair: list; display
   pair: list; comprehensions
   pair: empty; list
   object: list

A list display is a possibly empty series of expressions enclosed in square
brackets:

列表用一对方括号包围的表达式序列 (可能为空) 表示: 

.. productionlist::
   list_display: "[" [`expression_list` | `comprehension`] "]"

A list display yields a new list object, the contents being specified by either
a list of expressions or a comprehension.  When a comma-separated list of
expressions is supplied, its elements are evaluated from left to right and
placed into the list object in that order.  When a comprehension is supplied,
the list is constructed from the elements resulting from the comprehension.

使用列表display会生成一个新的列表对象. 它的内容由一个表达式列表或comprehension给出. 使用以逗号分隔的表达式列表时, Python会从左到右对每个元素求值然后按顺序放进列表对象中. 如果是comprehension, 列表由comprehension的计算结果组成. 

.. _set:

集合形式
------------

.. index:: pair: set; display
           object: set

A set display is denoted by curly braces and distinguishable from dictionary
displays by the lack of colons separating keys and values:

集合由一对大括号标识, 与字典的区别在于, 集合不使用字典中键和值之间的冒号. 

.. productionlist::
   set_display: "{" (`expression_list` | `comprehension`) "}"

A set display yields a new mutable set object, the contents being specified by
either a sequence of expressions or a comprehension.  When a comma-separated
list of expressions is supplied, its elements are evaluated from left to right
and added to the set object.  When a comprehension is supplied, the set is
constructed from the elements resulting from the comprehension.

使用集合display会生成一个新的集合对象. 它的内容由一个表达式序列或comprehension给出. 使用以逗号分隔的表达式列表时, Python会从左到右对每个元素求值然后按顺序放进集合对象中. 如果是comprehension, 集合由comprehension的计算结果组成. 

An empty set cannot be constructed with ``{}``; this literal constructs an empty
dictionary.

空集合不能用 ``{}`` 建立; 这个字面值表示的是空字典. 

.. _dict:

字典形式
-------------------

.. index:: pair: dictionary; display
           key, datum, key/datum pair
           object: dictionary

A dictionary display is a possibly empty series of key/datum pairs enclosed in
curly braces:

字典用一对大括号括住的 "键／值对" 序列 (可能为空) 表示. 

.. productionlist::
   dict_display: "{" [`key_datum_list` | `dict_comprehension`] "}"
   key_datum_list: `key_datum` ("," `key_datum`)* [","]
   key_datum: `expression` ":" `expression`
   dict_comprehension: `expression` ":" `expression` `comp_for`

A dictionary display yields a new dictionary object.

使用字典display会生成一个新的字典对象. 

If a comma-separated sequence of key/datum pairs is given, they are evaluated
from left to right to define the entries of the dictionary: each key object is
used as a key into the dictionary to store the corresponding datum.  This means
that you can specify the same key multiple times in the key/datum list, and the
final dictionary's value for that key will be the last one given.

使用以逗号分隔的 "键／值对" 列表时, Python会从左到右地定义字典中的每个元素; 每个键对象作为字典一个键值存储对应的数据. 这意味着你可以在这个 "键／值对" 列表中多次使用相同键, 但只有最后一次使用的值会保存下来. 

A dict comprehension, in contrast to list and set comprehensions, needs two
expressions separated with a colon followed by the usual "for" and "if" clauses.
When the comprehension is run, the resulting key and value elements are inserted
in the new dictionary in the order they are produced.

字典comprehension与列表和集合的不同在于, 它需要用冒号分隔的两个表达式, 之后再尾随着通常的"for"和"if"子句. 当comprehension运行时, 结果 "键值对" 按产生顺序加入到新字典中. 

.. index:: pair: immutable; object
           hashable

Restrictions on the types of the key values are listed earlier in section
:ref:`types`.  (To summarize, the key type should be :term:`hashable`, which excludes
all mutable objects.)  Clashes between duplicate keys are not detected; the last
datum (textually rightmost in the display) stored for a given key value
prevails.

关于键值的类型限制已经在之前的 :ref:`types` 一节中有所介绍 (概要地讲, 键的类型应该是 :term:`hashable` 的, 这排除了所有可变对象) . 无论是哪种方法, 都不会检查相同键导致的冲突, 只有最后一个数据项 (在书写上是最右边的) 才会保留到字典中. 

.. _genexpr:

生成器表达式
---------------------

.. index:: pair: generator; expression
           object: generator

A generator expression is a compact generator notation in parentheses:

Generator表达式是圆括号内的一个紧凑的generator记法. 

.. productionlist::
   generator_expression: "(" `expression` `comp_for` ")"

A generator expression yields a new generator object.  Its syntax is the same as
for comprehensions, except that it is enclosed in parentheses instead of
brackets or curly braces.

Generator表达式会构造一个generator对象. 它的语法与comprehension相同, 除了两端是圆括号, 而不是方括号或者大括号. 

Variables used in the generator expression are evaluated lazily when the
:meth:`__next__` method is called for generator object (in the same fashion as
normal generators).  However, the leftmost :keyword:`for` clause is immediately
evaluated, so that an error produced by it can be seen before any other possible
error in the code that handles the generator expression.  Subsequent
:keyword:`for` clauses cannot be evaluated immediately since they may depend on
the previous :keyword:`for` loop. For example: ``(x*y for x in range(10) for y
in bar(x))``.

Generator表达式中的变量会被推迟到调用generator对象的 :meth:`__next__` 方法时计算, 这与普通generator对象相同. 但是, 最左的 :keyword:`for` 子句会立即得到调用, 所以这个子句中的错误会在任何处理generator表达式的代码中的错误之前发现. 其后的 :keyword:`for` 子句不会被立即计算, 因为他们可能依赖于前面的 :keyword:`for` 循环, 例如:  ``(x*y for x in range(10) for y in bar(x))`` . 

The parentheses can be omitted on calls with only one argument.  See section
:ref:`calls` for the detail.

如果调用只有一个参数, 那么可以省略这个括号, 见 :ref:`calls` . 

.. _yieldexpr:

Yield 表达式
-----------------

.. index::
   keyword: yield
   pair: yield; expression
   pair: generator; function

.. productionlist::
   yield_atom: "(" `yield_expression` ")"
   yield_expression: "yield" [`expression_list`]

The :keyword:`yield` expression is only used when defining a generator function,
and can only be used in the body of a function definition.  Using a
:keyword:`yield` expression in a function definition is sufficient to cause that
definition to create a generator function instead of a normal function.

:keyword:`yield` 表达式只能在定义generator函数时使用, 并且只能用于函数体内. 在函数定义中使用 :keyword:`yield` 表达式会使这个函数成为generator函数, 而不是正常函数. 

When a generator function is called, it returns an iterator known as a
generator.  That generator then controls the execution of a generator function.
The execution starts when one of the generator's methods is called.  At that
time, the execution proceeds to the first :keyword:`yield` expression, where it
is suspended again, returning the value of :token:`expression_list` to
generator's caller.  By suspended we mean that all local state is retained,
including the current bindings of local variables, the instruction pointer, and
the internal evaluation stack.  When the execution is resumed by calling one of
the generator's methods, the function can proceed exactly as if the
:keyword:`yield` expression was just another external call.  The value of the
:keyword:`yield` expression after resuming depends on the method which resumed
the execution.

在调用一个generator函数时, 它会返回一个generator对象作为迭代器. 这个generator对象控制着generator函数的执行. 调用这个generator对象的方法调用时, 函数就会开始执行, 这时, 函数会处理第一个 :keyword:`yield` 表达式, 并在这里暂停执行函数, 还会返回表达式 :token:`expression_list` 的值给generator对象的调用者. 函数暂停执行意味着所有的局部状态都被保存下来了, 包括局部变量的当前绑定、指令指针和内部栈. 在调用某个generator对象的方法时, 函数就会恢复执行, 就好像 :keyword:`yield` 表达式只是一个对外部功能的调用一样. 在恢复执行时,  :keyword:`yield` 表达式的值依赖于恢复执行时调用的什么方法. 

.. index:: single: coroutine

All of this makes generator functions quite similar to coroutines; they yield
multiple times, they have more than one entry point and their execution can be
suspended.  The only difference is that a generator function cannot control
where should the execution continue after it yields; the control is always
transfered to the generator's caller.

这种generator函数的所有特征与coroutines很相近: 他们都多次产生 (yield) 值, 他们有多个入口点并且执行可以暂停. 唯一的差异在于generator函数在产生 (yield) 值之后无法控制在什么地方继续执行, 控制权会转移到generator的调用者上面. 

The :keyword:`yield` statement is allowed in the :keyword:`try` clause of a
:keyword:`try` ...  :keyword:`finally` construct.  If the generator is not
resumed before it is finalized (by reaching a zero reference count or by being
garbage collected), the generator-iterator's :meth:`close` method will be
called, allowing any pending :keyword:`finally` clauses to execute.

:keyword:`yield` 语句可以出现在 :keyword:`try` ... :keyword:`finally` 构造中的 :keyword:`try` 子句中. 如果一个generator对象在终结 (引用计数变为0, 或者被垃圾回收) 之前没有能恢复执行, 就会调用的generator对象 :meth:`close` 方法, 给等待的 :keyword:`finally` 子句执行的机会. 

.. index:: object: generator

The following generator's methods can be used to control the execution of a
generator function:

以下generator的方法用于控制generator函数的执行: 

.. index:: exception: StopIteration


.. method:: generator.__next__()

   Starts the execution of a generator function or resumes it at the last
   executed :keyword:`yield` expression.  When a generator function is resumed
   with a :meth:`__next__` method, the current :keyword:`yield` expression
   always evaluates to :const:`None`.  The execution then continues to the next
   :keyword:`yield` expression, where the generator is suspended again, and the
   value of the :token:`expression_list` is returned to :meth:`next`'s caller.
   If the generator exits without yielding another value, a :exc:`StopIteration`
   exception is raised.

   开始generator函数的执行, 或者从上次执行的 :keyword:`yield` 表达式处恢复执行. 当使用 :meth:`__next__` 方法恢复generator函数的执行时, 当前 :keyword:`yield` 表达式都会被计算成 :const:`None` . 执行然后会继续到下次遇见 :keyword:`yield` 表达式, generator函数会再次被挂起, 表达式 :token:`expression_list` 的值会被返回给 :meth:`next` 的调用者. 如果generator函数没有产生 (yield) 新值就直接退出了, 就会导致抛出异常 :exc:`StopIteration` . 

   This method is normally called implicitly, e.g. by a :keyword:`for` loop, or
   by the built-in :func:`next` function.

   通常不会直接调用这个方法, 而是通过像 :keyword:`for` 循环或内置的 :func:`next` 函数隐式地使用它. 

.. method:: generator.send(value)

   Resumes the execution and "sends" a value into the generator function.  The
   ``value`` argument becomes the result of the current :keyword:`yield`
   expression.  The :meth:`send` method returns the next value yielded by the
   generator, or raises :exc:`StopIteration` if the generator exits without
   yielding another value.  When :meth:`send` is called to start the generator,
   it must be called with :const:`None` as the argument, because there is no
   :keyword:`yield` expression that could receive the value.

   恢复执行, 并给generator函数 "发送" 一个值.  ``value`` 参数的值会成为当前 :keyword:`yield` 表达式的结果,  :meth:`send` 方法会返回generator函数产生的下一个值, 或者它没有产生 (yield) 其它值便退出时就抛出异常 :exc:`StopIteration` . 使用 :meth:`send` 方法启动一个generator函数时, 必须使用 :const:`None` 作为参数, 因为这时没有任何 :keyword:`yield` 表达式可以接收这个值. 

.. method:: generator.throw(type[, value[, traceback]])

   Raises an exception of type ``type`` at the point where generator was paused,
   and returns the next value yielded by the generator function.  If the generator
   exits without yielding another value, a :exc:`StopIteration` exception is
   raised.  If the generator function does not catch the passed-in exception, or
   raises a different exception, then that exception propagates to the caller.

   在generator函数暂停点上抛出一个类型为 ``type`` 的异常, 并返回generator函数产生 (yield) 的下一个值. generator函数没有产生 (yield) 其它值便退出时就抛出异常 :exc:`StopIteration` . 如果generator没有捕获这个传入的异常, 或者抛出了一个不同的异常, 那么这个异常会传播给调用者处理. 

.. index:: exception: GeneratorExit


.. method:: generator.close()

   Raises a :exc:`GeneratorExit` at the point where the generator function was
   paused.  If the generator function then raises :exc:`StopIteration` (by
   exiting normally, or due to already being closed) or :exc:`GeneratorExit` (by
   not catching the exception), close returns to its caller.  If the generator
   yields a value, a :exc:`RuntimeError` is raised.  If the generator raises any
   other exception, it is propagated to the caller.  :meth:`close` does nothing
   if the generator has already exited due to an exception or normal exit.

   在generator函数被暂停点抛出异常 :exc:`GeneratorExit` . 如果generator函数之后抛出了异常 :exc:`StopIteration` (通过正常退出, 或者是已经关闭了), 或者 :exc:`GeneratorExit` (因为没有捕获这个异常), 那么close会返回到调用者. 如果generator函数产生 (yield) 了一个值, 那么就会抛出 :exc:`RuntimeError` 异常. 如果generator函数抛出了任何其他异常, 它都会传播给其调用者. 如果generator函数因为异常或者是正常退出已经关闭了,  :meth:`close` 方法什么也不会做. 

Here is a simple example that demonstrates the behavior of generators and
generator functions:

下面是一个简单的例子演示了generator和generator函数的行为::

   >>> def echo(value=None):
   ...     print("Execution starts when 'next()' is called for the first time.")
   ...     try:
   ...         while True:
   ...             try:
   ...                 value = (yield value)
   ...             except Exception as e:
   ...                 value = e
   ...     finally:
   ...         print("Don't forget to clean up when 'close()' is called.")
   ...
   >>> generator = echo(1)
   >>> print(next(generator))
   Execution starts when 'next()' is called for the first time.
   1
   >>> print(next(generator))
   None
   >>> print(generator.send(2))
   2
   >>> generator.throw(TypeError, "spam")
   TypeError('spam',)
   >>> generator.close()
   Don't forget to clean up when 'close()' is called.


.. seealso::

   :pep:`0255` - Simple Generators
      The proposal for adding generators and the :keyword:`yield` statement to Python.

      为Python增加generator和 :keyword:`yield` 语句的提案. 

   :pep:`0342` - Coroutines via Enhanced Generators
      The proposal to enhance the API and syntax of generators, making them
      usable as simple coroutines.

      改进generator API和语法, 使其像简单coroutine一样可用的提案. 

.. _primaries:

基元(Primaries)
======================

.. index:: single: primary

Primaries represent the most tightly bound operations of the language. Their
syntax is:

基元是指和语言本身中关系最紧密的操作. 它们的语法如下: 

.. productionlist::
   primary: `atom` | `attributeref` | `subscription` | `slicing` | `call`


.. _attribute-references:

属性引用 (Attribute references) 
------------------------------------

.. index:: pair: attribute; reference

An attribute reference is a primary followed by a period and a name:

属性引用由一个基元 (primary) 后跟一个句号和一个名字构成: 

.. productionlist::
   attributeref: `primary` "." `identifier`

.. index::
   exception: AttributeError
   object: module
   object: list

The primary must evaluate to an object of a type that supports attribute
references, which most objects do.  This object is then asked to produce the
attribute whose name is the identifier (which can be customized by overriding
the :meth:`__getattr__` method).  If this attribute is not available, the
exception :exc:`AttributeError` is raised.  Otherwise, the type and value of the
object produced is determined by the object.  Multiple evaluations of the same
attribute reference may yield different objects.

基元必须是一个计算 (evalute) 出来的支持属性引用的类型的实例, 多数情况下指一个对象. 然后, 会要求这个对象生成属性, 其 `identifer` 的名字就是属性名 (这一步可以通过 :meth:`__getattr__` 方法覆盖定制) . 如果该属性无效, 就会抛出异常 :exc:`AttributeError` . 否则, 对象本身就确定了属性的类型和值. 对同一属性的多次求值 (evaluation) 可能会创建不同的对象. 

.. _subscriptions:

下标 (Subscriptions) 
--------------------------------

.. index:: single: subscription

.. index::
   object: sequence
   object: mapping
   object: string
   object: tuple
   object: list
   object: dictionary
   pair: sequence; item

A subscription selects an item of a sequence (string, tuple or list) or mapping
(dictionary) object:

下标会选择一个有序类型对象 (字符串、元组和列表) 或映射 (字典) 对象中的一项: 

.. productionlist::
   subscription: `primary` "[" `expression_list` "]"

The primary must evaluate to an object that supports subscription, e.g. a list
or dictionary.  User-defined objects can support subscription by defining a
:meth:`__getitem__` method.

基元 (primary) 必须是一个计算出来的支持下标的对象, 例如列表或者字典. 用户定义对象可以通过定义 :meth:`__getitem__` 支持下标. 

For built-in objects, there are two types of objects that support subscription:

有两种内置对象可以支持下标: 

If the primary is a mapping, the expression list must evaluate to an object
whose value is one of the keys of the mapping, and the subscription selects the
value in the mapping that corresponds to that key.  (The expression list is a
tuple except if it has exactly one item.)

If the primary is a sequence, the expression (list) must evaluate to an integer
or a slice (as discussed in the following section).

The formal syntax makes no special provision for negative indices in
sequences; however, built-in sequences all provide a :meth:`__getitem__`
method that interprets negative indices by adding the length of the sequence
to the index (so that ``x[-1]`` selects the last item of ``x``).  The
resulting value must be a nonnegative integer less than the number of items in
the sequence, and the subscription selects the item whose index is that value
(counting from zero). Since the support for negative indices and slicing
occurs in the object's :meth:`__getitem__` method, subclasses overriding
this method will need to explicitly add that support.

.. index::
   single: character
   pair: string; item

A string's items are characters.  A character is not a separate data type but a
string of exactly one character.

字符串的元素是字符. 字符不是单独的数据类型, 而是只包括一个字符的字符串. 

.. _slicings:

切片
--------

.. index::
   single: slicing
   single: slice

.. index::
   object: sequence
   object: string
   object: tuple
   object: list

A slicing selects a range of items in a sequence object (e.g., a string, tuple
or list).  Slicings may be used as expressions or as targets in assignment or
:keyword:`del` statements.  The syntax for a slicing:

片断选择某个有序类型对象 (如字符串、元组或者列表) 的若干个元素. 片断可以作为表达式使用, 也可以作为赋值和 ``del`` 语句的目标. 下面是片断的语法: 

.. productionlist::
   slicing: `primary` "[" `slice_list` "]"
   slice_list: `slice_item` ("," `slice_item`)* [","]
   slice_item: `expression` | `proper_slice`
   proper_slice: [`lower_bound`] ":" [`upper_bound`] [ ":" [`stride`] ]
   lower_bound: `expression`
   upper_bound: `expression`
   stride: `expression`

There is ambiguity in the formal syntax here: anything that looks like an
expression list also looks like a slice list, so any subscription can be
interpreted as a slicing.  Rather than further complicating the syntax, this is
disambiguated by defining that in this case the interpretation as a subscription
takes priority over the interpretation as a slicing (this is the case if the
slice list contains no proper slice).

在这里形式语法的说明中有点含糊: 任何看起来像表达式列表的结构也可以看作是片断列表, 所以任何下标都可以解释为片断. 为了避免语法的复杂化, 我们这样避免歧义: 这样的结构我们优先判断为下标, 其次作为表达式列表 (即不包括片断列表没有包括适当片断的时候) . 

.. index::
   single: start (slice object attribute)
   single: stop (slice object attribute)
   single: step (slice object attribute)

The semantics for a slicing are as follows.  The primary must evaluate to a
mapping object, and it is indexed (using the same :meth:`__getitem__` method as
normal subscription) with a key that is constructed from the slice list, as
follows.  If the slice list contains at least one comma, the key is a tuple
containing the conversion of the slice items; otherwise, the conversion of the
lone slice item is the key.  The conversion of a slice item that is an
expression is that expression.  The conversion of a proper slice is a slice
object (see section :ref:`types`) whose :attr:`start`, :attr:`stop` and
:attr:`step` attributes are the values of the expressions given as lower bound,
upper bound and stride, respectively, substituting ``None`` for missing
expressions.

片断的语义如下: primary必须被计算成一个映射对象, 并且它以从slice list中构造出的键作为索引 (与下标的工作方式相同, 即通过方法 :meth:`__getitem__` ) . 如果slice list包括至少一个逗号, 键就是一个从片断项转换的元组, 否则, 唯一的片断项就作为键. 本身就是表达式的片断项的转换结果就是该表达式. 一个适当片断在转换后就是片断对象 (参见 :ref:`types` 一节) , 属性 :attr:`start` 、 :attr:`stop` 和 :attr:`step` 分别是作为下界、上界、步长的表达式的值, 如果缺少对应的表达式, 就用 ``None`` 补齐. 

.. _calls:

调用
-----

.. index:: single: call

.. index:: object: callable

A call calls a callable object (e.g., a function) with a possibly empty series
of arguments:

调用就是以一系列参数 (可能为空) 调用一个可调用对象 (例如函数) :

.. productionlist::
   call: `primary` "(" [`argument_list` [","] | `comprehension`] ")"
   argument_list: `positional_arguments` ["," `keyword_arguments`]
                :   ["," "*" `expression`] ["," `keyword_arguments`]
                :   ["," "**" `expression`]
                : | `keyword_arguments` ["," "*" `expression`]
                :   ["," `keyword_arguments`] ["," "**" `expression`]
                : | "*" `expression` ["," `keyword_arguments`] ["," "**" `expression`]
                : | "**" `expression`
   positional_arguments: `expression` ("," `expression`)*
   keyword_arguments: `keyword_item` ("," `keyword_item`)*
   keyword_item: `identifier` "=" `expression`

A trailing comma may be present after the positional and keyword arguments but
does not affect the semantics.

在位置参数和关键字参数之后可以尾随一个逗号, 但它对语义没有任何影响. 

The primary must evaluate to a callable object (user-defined functions, built-in
functions, methods of built-in objects, class objects, methods of class
instances, and all objects having a :meth:`__call__` method are callable).  All
argument expressions are evaluated before the call is attempted.  Please refer
to section :ref:`function` for the syntax of formal parameter lists.

基元, 必须被计算成一个可调用对象 (用户定义函数、内置函数、内置方法对象、类对象、类实例方法、和所有其他定义了 :meth:`__call__` 方法模拟可调用对象的对象. ) 所有参数表达都在调用执行之前计算, 关于形参表的语法参见 :ref:`function` 一节. 

.. XXX update with kwonly args PEP

If keyword arguments are present, they are first converted to positional
arguments, as follows.  First, a list of unfilled slots is created for the
formal parameters.  If there are N positional arguments, they are placed in the
first N slots.  Next, for each keyword argument, the identifier is used to
determine the corresponding slot (if the identifier is the same as the first
formal parameter name, the first slot is used, and so on).  If the slot is
already filled, a :exc:`TypeError` exception is raised. Otherwise, the value of
the argument is placed in the slot, filling it (even if the expression is
``None``, it fills the slot).  When all arguments have been processed, the slots
that are still unfilled are filled with the corresponding default value from the
function definition.  (Default values are calculated, once, when the function is
defined; thus, a mutable object such as a list or dictionary used as default
value will be shared by all calls that don't specify an argument value for the
corresponding slot; this should usually be avoided.)  If there are any unfilled
slots for which no default value is specified, a :exc:`TypeError` exception is
raised.  Otherwise, the list of filled slots is used as the argument list for
the call.

如果有关键字参数, 它们会先按如下步骤转换为位置参数: 第一步、根据形参表创建一串空闲槽, 如果有N个位置参数, 它们就被放在前N个槽中. 然后, 对于每个关键字参数, 根据它的标识符名字确定其对应的槽 (如果其标识符与第一个形参数名相同, 它就占用第一个槽, 以此类推) . 如果发现某个槽已经被占用, 就是导致 :exc:`TypeError` 异常, 否则将参数的值 (即使为 :const:`None` ) 放进槽中. 当处理完所有关键字参数后, 所有未填充的槽用函数定义中的默认值填充 (默认值是在函数定义时计算出来的, 所以当使用列表和字典这种可变类型对象做默认值时, 它们就会被那些没有为相应槽指定参数的调用所共享, 一般情况要避免这些) . 如果仍有未填充无默认值的槽位, 就会抛出 :exc:`TypeError` 异常. 否则, 所有被填充的槽就当作调用的参数表使用了. 

.. impl-detail::

   An implementation may provide built-in functions whose positional parameters
   do not have names, even if they are 'named' for the purpose of documentation,
   and which therefore cannot be supplied by keyword.  In CPython, this is the
   case for functions implemented in C that use :c:func:`PyArg_ParseTuple` to
   parse their arguments.

   实现提供的内置函数的位置参数可能根本就没有名字, 即使它们在文档中是有名字的. 因此不能用关键字方法指定. 在CPython里, 当使用C语言的 :cfunc:`PyArg_ParseTuple` 解析函数参数时就是这种情况. 

If there are more positional arguments than there are formal parameter slots, a
:exc:`TypeError` exception is raised, unless a formal parameter using the syntax
``*identifier`` is present; in this case, that formal parameter receives a tuple
containing the excess positional arguments (or an empty tuple if there were no
excess positional arguments).

在形式参数没有使用 ``*identifier`` 语法, 并且位置参数多于形参槽数就会导致 :exc:`TypeError` 异常. 在使用该种语法时, 形参会接受一个包括有额外位置参数的元组 (如果没有额外的位置参数, 元组就为空) . 

If any keyword argument does not correspond to a formal parameter name, a
:exc:`TypeError` exception is raised, unless a formal parameter using the syntax
``**identifier`` is present; in this case, that formal parameter receives a
dictionary containing the excess keyword arguments (using the keywords as keys
and the argument values as corresponding values), or a (new) empty dictionary if
there were no excess keyword arguments.

如果有任何一个关键字参数没有对应形参名字, 并且形参列表里没有使用 ``**identifier`` 语法, 就会引发 :exc:`TypeError` 异常. 使用该种语法时. 形参会接受一个包括有额外关键字参数的字典 (关键字是键, 参数值作为对应的值) ; 如果没有额外的关键字参数, 这个 (新) 字典就为空. 

If the syntax ``*expression`` appears in the function call, ``expression`` must
evaluate to a sequence.  Elements from this sequence are treated as if they were
additional positional arguments; if there are positional arguments *x1*,...,
*xN*, and ``expression`` evaluates to a sequence *y1*, ..., *yM*, this is
equivalent to a call with M+N positional arguments *x1*, ..., *xN*, *y1*, ...,
*yM*.

如果在函数调用中使用了 ``*expression`` , 那么 ``expression`` 的计算结果必须是有序类型, 这个有序类型对象的元素按额外的位置参数处理. 如果存在有位置参数 *x1* ,..., *xN* , 并且 ``*exprsseion`` 的计算结果为 *y1* ,..., *yM* , 那么函数就是有M+N个参数了,  *x1* , ..., *xN* ,  *y1* , ...,
 *yM* . 

A consequence of this is that although the ``*expression`` syntax may appear
*after* some keyword arguments, it is processed *before* the keyword arguments
(and the ``**expression`` argument, if any -- see below).  So:

由此可以得到一个推论, 尽管 ``*expression`` 可以出现在关键字参数 *之后* , 但它会在处理关键字参数 *之前* 得到处理.  (如果有的话,  ``**expression`` 也是如此, 见下述) , 所以::

   >>> def f(a, b):
   ...  print(a, b)
   ...
   >>> f(b=1, *(2,))
   2 1
   >>> f(a=1, *(2,))
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   TypeError: f() got multiple values for keyword argument 'a'
   >>> f(1, *(2,))
   1 2

It is unusual for both keyword arguments and the ``*expression`` syntax to be
used in the same call, so in practice this confusion does not arise.

同时使用关键字参数和 ``*expression`` 调用的情况并不常见, 所以在实践中这种混乱很少发生. 

If the syntax ``**expression`` appears in the function call, ``expression`` must
evaluate to a mapping, the contents of which are treated as additional keyword
arguments.  In the case of a keyword appearing in both ``expression`` and as an
explicit keyword argument, a :exc:`TypeError` exception is raised.

如果在函数调用中使用 ``**expression`` , 那么 ``expression`` 的计算结果必须是一个映射类型的对象, 其内容作为附加的关键字参数. 如果一个关键字同时出现在 ``expression`` 中和显式关键字参数中, 就会抛出 :exc:`TypeError` 异常. 

Formal parameters using the syntax ``*identifier`` or ``**identifier`` cannot be
used as positional argument slots or as keyword argument names.

使用 ``*identifier`` 或 ``**identifier`` 语法形式的形参不能作为位置参数槽, 或者作为关键字参数名. 

A call always returns some value, possibly ``None``, unless it raises an
exception.  How this value is computed depends on the type of the callable
object.

如果调用没有抛出异常, 通常会返回一些值, 有可能为 ``None`` . 这个值如何计算依赖于可调用对象的类型. 

If it is---

a user-defined function:
   .. index::
      pair: function; call
      triple: user-defined; function; call
      object: user-defined function
      object: function

   The code block for the function is executed, passing it the argument list.  The
   first thing the code block will do is bind the formal parameters to the
   arguments; this is described in section :ref:`function`.  When the code block
   executes a :keyword:`return` statement, this specifies the return value of the
   function call.

   用户定义函数. 执行此函数的代码块, 并把参数传给它. 这个代码块要做的第一件事就是将形参与实参对应起来, 关于这点参见 :ref:`function` . 当代码块执行到 :keyword:`return` 语句时, 会指定这次函数调用的返回值. 

a built-in function or method:
   .. index::
      pair: function; call
      pair: built-in function; call
      pair: method; call
      pair: built-in method; call
      object: built-in method
      object: built-in function
      object: method
      object: function

   The result is up to the interpreter; see :ref:`built-in-funcs` for the
   descriptions of built-in functions and methods.

   内置函数或者方法. 结果依赖于解释器, 参见 :ref:`built-in-funcs` 的相应介绍. 

a class object:
   .. index::
      object: class
      pair: class object; call

   A new instance of that class is returned.

   类对象. 返回这个类的一个新实例. 
   
a class instance method:
   .. index::
      object: class instance
      object: instance
      pair: class instance; call

   The corresponding user-defined function is called, with an argument list that is
   one longer than the argument list of the call: the instance becomes the first
   argument.

   调用对应的用户定义函数, 比普通的函数调用多一个参数: 该实例成为方法的第一个参数. 

a class instance:
   .. index::
      pair: instance; call
      single: __call__() (object method)

   The class must define a :meth:`__call__` method; the effect is then the same as
   if that method was called.

   类实例. 类实例必须定义方法 :meth:`__call__` , 效果同对该方法的调用. 

.. _power:

幂运算符 (The power operator) 
================================

The power operator binds more tightly than unary operators on its left; it binds
less tightly than unary operators on its right.  The syntax is:

幂运算符比它左边的一元运算符的优先级更高; 但比右边的一元运算符要低. 语法为: 

.. productionlist::
   power: `primary` ["**" `u_expr`]

Thus, in an unparenthesized sequence of power and unary operators, the operators
are evaluated from right to left (this does not constrain the evaluation order
for the operands): ``-1**2`` results in ``-1``.

因此, 在一个没有额外括号的幂运算符和一元运算符序列中, 求值会从右至左进行 (这点对操作数本身的求值顺序没有影响) :  ``-1**2`` 会计算为 ``-1`` . 

The power operator has the same semantics as the built-in :func:`pow` function,
when called with two arguments: it yields its left argument raised to the power
of its right argument.  The numeric arguments are first converted to a common
type, and the result is of that type.

当以两个参数调用内置函数 :func:`pow` 时, 幂运算符与它有相同的语义: 生成左边参数值的右边参数值次方的计算结果. 数值型参数先被转换成通用类型, 结果的类型与参数类型相同. 

For int operands, the result has the same type as the operands unless the second
argument is negative; in that case, all arguments are converted to float and a
float result is delivered. For example, ``10**2`` returns ``100``, but
``10**-2`` returns ``0.01``.

对于整数操作数, 如果第二个参数不是负数, 结果类型与操作数相同. 否则, 所以参数先被转换为浮点数, 并产生一个浮点结果. 例如,  ``10**2`` 返回 ``100`` , 但
 ``10**-2`` 返回 ``0.01`` . 

Raising ``0.0`` to a negative power results in a :exc:`ZeroDivisionError`.
Raising a negative number to a fractional power results in a :class:`complex`
number. (In earlier versions it raised a :exc:`ValueError`.)

计算 ``0.0`` 的负数次幂时会抛出  :exc:`ZeroDivisionError` 异常. 计算负数的分数次幂会生成一个 :class:`complex` 值 (之前版本会抛出 :exc:`ValueError` 异常) . 

.. _unary:

一元运算和位运算
=======================================

.. index::
   triple: unary; arithmetic; operation
   triple: unary; bitwise; operation

All unary arithmetic and bitwise operations have the same priority:

所有一元算术运算符和位运算符有相同的优先级: 

.. productionlist::
   u_expr: `power` | "-" `u_expr` | "+" `u_expr` | "~" `u_expr`

.. index::
   single: negation
   single: minus

The unary ``-`` (minus) operator yields the negation of its numeric argument.

一元运算符 ``-``  (减) 取数值型操作数的负值. 

.. index:: single: plus

The unary ``+`` (plus) operator yields its numeric argument unchanged.

一元运算符 ``+``  (加) 取数值型操作数值本身. 

.. index:: single: inversion


The unary ``~`` (invert) operator yields the bitwise inversion of its integer
argument.  The bitwise inversion of ``x`` is defined as ``-(x+1)``.  It only
applies to integral numbers.

一元运算符 ``~``  (取反) 会对其整数参数求逆(比特级).  ``x`` 的比特级求逆运算定义为 ``-(x+1)`` . 这个运算符只用于整数操作数. 

.. index:: exception: TypeError

In all three cases, if the argument does not have the proper type, a
:exc:`TypeError` exception is raised.

在所有以上三种情况下, 如果参数的类型不合法, 就会引发一个 :exc:`TypeError` 异常. 

.. _binary:

二元算术运算 (Binary arithmetic operations) 
=======================================================

.. index:: triple: binary; arithmetic; operation

The binary arithmetic operations have the conventional priority levels.  Note
that some of these operations also apply to certain non-numeric types.  Apart
from the power operator, there are only two levels, one for multiplicative
operators and one for additive operators:

二元算术运算符的优先级符合我们的正常习惯. 但要注意其中有些运算符也可以应用于非数值型操作数, 除了幂运算符, 它们只分两个优先级, 即乘法类运算和加法类运算. 

.. productionlist::
   m_expr: `u_expr` | `m_expr` "*" `u_expr` | `m_expr` "//" `u_expr` | `m_expr` "/" `u_expr`
         : | `m_expr` "%" `u_expr`
   a_expr: `m_expr` | `a_expr` "+" `m_expr` | `a_expr` "-" `m_expr`

.. index:: single: multiplication

The ``*`` (multiplication) operator yields the product of its arguments.  The
arguments must either both be numbers, or one argument must be an integer and
the other must be a sequence. In the former case, the numbers are converted to a
common type and then multiplied together.  In the latter case, sequence
repetition is performed; a negative repetition factor yields an empty sequence.

``*`` (乘)运算符计算其操作数的乘积. 要么两个参数的类型都是数值型, 要么一个是整数另一个是有序类型. 第一种情况下, 数值参数先被转换成通用类型然后计算乘积. 后一种情况会重复连接有序类型对象. 一个负重复因子会产生一个空有序类型对象. 

.. index::
   exception: ZeroDivisionError
   single: division

The ``/`` (division) and ``//`` (floor division) operators yield the quotient of
their arguments.  The numeric arguments are first converted to a common type.
Integer division yields a float, while floor division of integers results in an
integer; the result is that of mathematical division with the 'floor' function
applied to the result.  Division by zero raises the :exc:`ZeroDivisionError`
exception.

``/``  (除) 和 ``//``  (整除) 运算符生成参数的商. 数值型参数首先被转换成通用类型, 整数除法的计算结果会产生一个浮点类型的结果, 而整除操作则返回整数结果, 即 'floor' 函数做数学计算的结果. 除以零会引发 :exc:`ZeroDivisionError` 异常. 

.. index:: single: modulo

The ``%`` (modulo) operator yields the remainder from the division of the first
argument by the second.  The numeric arguments are first converted to a common
type.  A zero right argument raises the :exc:`ZeroDivisionError` exception.  The
arguments may be floating point numbers, e.g., ``3.14%0.7`` equals ``0.34``
(since ``3.14`` equals ``4*0.7 + 0.34``.)  The modulo operator always yields a
result with the same sign as its second operand (or zero); the absolute value of
the result is strictly smaller than the absolute value of the second operand
[#]_.

``%``  (模) 运算符计算第一个参数除以第二参数得到的余数. 数值型参数首先被转换成通用类型, 右面的参数为零会引发 :exc:`ZeroDivisionError` 异常. 参数可以是浮点数, 例如 ``3.14%0.7`` 等于 ``0.34``  (因为 ``3.14`` 等于 ``4*0.7 + 0.34`` ) . 模运算符的结果一定与第二个参数的符号相同 (或者为0) , 并且结果的绝对值一定小于第二个参数的绝对值. 

The floor division and modulo operators are connected by the following
identity: ``x == (x//y)*y + (x%y)``.  Floor division and modulo are also
connected with the built-in function :func:`divmod`: ``divmod(x, y) == (x//y,
x%y)``. [#]_.

整除和取模运算可以用以下等式联系起来:  ``x == (x//y)*y + (x%y)`` . 整除和模运算也可以用内置函数 :func:`divmod` , 即 ``divmod(x, y) == (x//y,
x%y)`` . 

In addition to performing the modulo operation on numbers, the ``%`` operator is
also overloaded by string objects to perform old-style string formatting (also
known as interpolation).  The syntax for string formatting is described in the
Python Library Reference, section :ref:`old-string-formatting`.

除了执行数字上的模运算,  ``%`` 运算符也被字符串类型重载为旧风格的字符串格式化 (也称为interpolation) . 字符串格式化的语法在Python库参考 (Python Library Reference) 中介绍, 见 :ref:`old-string-formatting` . 

The floor division operator, the modulo operator, and the :func:`divmod`
function are not defined for complex numbers.  Instead, convert to a floating
point number using the :func:`abs` function if appropriate.

整除、模运算符和 :func:`divmod` 函数都不能操作复数. 但可以在需要的时候用 :func:`abs` 函数将它们转换成浮点数. 

.. index:: single: addition

The ``+`` (addition) operator yields the sum of its arguments.  The arguments
must either both be numbers or both sequences of the same type.  In the former
case, the numbers are converted to a common type and then added together.  In
the latter case, the sequences are concatenated.

``+``  (加) 运算符计算参数的和, 参数要么必须都是数值型, 或者都是相同类型的有序类型对象. 对于前一种情况, 它们先被转换成通用类型然后相加. 后一种情况下, 所有有序类型对象都会被连接起来. 

.. index:: single: subtraction

The ``-`` (subtraction) operator yields the difference of its arguments.  The
numeric arguments are first converted to a common type.

``-``  (减) 计算参数的差, 数值型的参数首先被转换成通用类型. 

.. _shifting:

移位操作
===================

.. index:: pair: shifting; operation

The shifting operations have lower priority than the arithmetic operations:

移位运算符的优先级比算术运算符低. 

.. productionlist::
   shift_expr: `a_expr` | `shift_expr` ( "<<" | ">>" ) `a_expr`

These operators accept integers as arguments.  They shift the first argument to
the left or right by the number of bits given by the second argument.

这些运算符接受整数作为参数. 它们将第一个参数向左或向右移动第二个参数指出的位数. 

.. index:: exception: ValueError

A right shift by *n* bits is defined as division by ``pow(2,n)``.  A left shift
by *n* bits is defined as multiplication with ``pow(2,n)``.

右移 *n* 位可以定义为除以 ``pow(2,n)`` . 左移 *n* 位可以定义为乘以 ``pow(2,n)`` . 

.. note::

   In the current implementation, the right-hand operand is required
   to be at most :attr:`sys.maxsize`.  If the right-hand operand is larger than
   :attr:`sys.maxsize` an :exc:`OverflowError` exception is raised.

   在当前实现中, 右操作数最大为  :attr:`sys.maxsize`　. 如果超过了这个限制, 就是抛出异常　 :exc:`OverflowError` . 

.. _bitwise:

二元位操作运算 (Binary bitwise operations) 
===========================================================

.. index:: triple: binary; bitwise; operation

Each of the three bitwise operations has a different priority level:

移位运算符的优先级各不相同: 

.. productionlist::
   and_expr: `shift_expr` | `and_expr` "&" `shift_expr`
   xor_expr: `and_expr` | `xor_expr` "^" `and_expr`
   or_expr: `xor_expr` | `or_expr` "|" `xor_expr`

.. index:: pair: bitwise; and

The ``&`` operator yields the bitwise AND of its arguments, which must be
integers.

``&`` 运算符生成参数的比特级 AND 运算, 参数必须是整数. 

.. index::
   pair: bitwise; xor
   pair: exclusive; or

The ``^`` operator yields the bitwise XOR (exclusive OR) of its arguments, which
must be integers.

``^`` 运算符生成参数的比特级 XOR 运算 (排斥或) , 参数必须是整数. 

.. index::
   pair: bitwise; or
   pair: inclusive; or

The ``|`` operator yields the bitwise (inclusive) OR of its arguments, which
must be integers.

``|`` 运算符生成参数的比特级 OR 运算 (包容或) , 参数必须是整数. 

.. _comparisons:
.. _is:
.. _is not:
.. _in:
.. _not in:

比较
===========

.. index:: single: comparison

.. index:: pair: C; language

Unlike C, all comparison operations in Python have the same priority, which is
lower than that of any arithmetic, shifting or bitwise operation.  Also unlike
C, expressions like ``a < b < c`` have the interpretation that is conventional
in mathematics:

与C语言不同, Python中所有比较运算符具有相同的优先级, 但比所有算术运算符、移位运算符和位运算符都低, 并且, 不像C语言, 表达式 ``a < b < c`` 与其数学含义相同. 

.. productionlist::
   comparison: `or_expr` ( `comp_operator` `or_expr` )*
   comp_operator: "<" | ">" | "==" | ">=" | "<=" | "!="
                : | "is" ["not"] | ["not"] "in"

Comparisons yield boolean values: ``True`` or ``False``.

比较运算符会生成布尔值 ``True`` 和 ``False`` . 

.. index:: pair: chaining; comparisons

Comparisons can be chained arbitrarily, e.g., ``x < y <= z`` is equivalent to
``x < y and y <= z``, except that ``y`` is evaluated only once (but in both
cases ``z`` is not evaluated at all when ``x < y`` is found to be false).

比较操作可以任意连接, 例如,  ``x < y <= z`` 等价于 ``x < y and y <= z`` , 除了 ``y`` 只会求值一次 (但在这两种情况下, 都是只要发现 ``x < y`` 为假,  ``z`` 就不会被求值了) . 

Formally, if *a*, *b*, *c*, ..., *y*, *z* are expressions and *op1*, *op2*, ...,
*opN* are comparison operators, then ``a op1 b op2 c ... y opN z`` is equivalent
to ``a op1 b and b op2 c and ... y opN z``, except that each expression is
evaluated at most once.

形式上讲, 如果 *a* , *b* , *c* , ..., *y* , *z* 为表达式,  *op1* , *op2* , ..., *opN* 为比较运算符, 则 ``a op1 b op2 c ...y opN z`` 等价于 ``a op1 b and b op2 c and ... y opN z`` , 除了每个表达式最多只求值一次. 

Note that ``a op1 b op2 c`` doesn't imply any kind of comparison between *a* and
*c*, so that, e.g., ``x < y > z`` is perfectly legal (though perhaps not
pretty).

注意 ``a op1 b op2 c`` 并没有隐式地规定 ``a`` 和 ``c`` 之间的比较运算种类, 所以 ``x < y > z`` 是完全合法的 (虽然不太美观) . 

The operators ``<``, ``>``, ``==``, ``>=``, ``<=``, and ``!=`` compare the
values of two objects.  The objects need not have the same type. If both are
numbers, they are converted to a common type.  Otherwise, the ``==`` and ``!=``
operators *always* consider objects of different types to be unequal, while the
``<``, ``>``, ``>=`` and ``<=`` operators raise a :exc:`TypeError` when
comparing objects of different types that do not implement these operators for
the given pair of types.  You can control comparison behavior of objects of
non-built-in types by defining rich comparison methods like :meth:`__gt__`,
described in section :ref:`customization`.

运算符 ``<`` 、 ``>`` 、 ``==`` 、 ``>=`` 、 ``<=`` 和 ``!=`` 比较两个对象的值, 它们不需要具有相同的的类型. 如果两者都是数值型的, 它们都先转换成通用类型. 否则,  ``==`` 和 ``!=`` 会把不同类型的值始终看成是不相等的, 而在没有实现不同类型对象间比较的运算时,  ``<`` 、 ``>`` 、 ``>=`` 和 ``<=`` 则会抛出异常 :exc:`TypeError` . 可以通过定义在 :ref:`customization` 一节中定义的厚比较方法 (如 :meth:`__gt__` ) 定制对象的比较行为. 

Comparison of objects of the same type depends on the type:

相同类型间对象的比较行为依赖于类型: 

* Numbers are compared arithmetically.

  数值型按大小比较. 

* The values :const:`float('NaN')` and :const:`Decimal('NaN')` are special.
  The are identical to themselves, ``x is x`` but are not equal to themselves,
  ``x != x``.  Additionally, comparing any value to a not-a-number value
  will return ``False``.  For example, both ``3 < float('NaN')`` and
  ``float('NaN') < 3`` will return ``False``.

  值 :const:`float('NaN')` 和 :const:`Decimal('NaN')` 比较特殊. 它们与自己完全相同, 即 ``x is x`` . 但与自身并不相等, 即 ``x != x`` . 另外, 将任何值与非数字值比较都会返回 ``False`` , 例如,  ``3 < float('NaN')`` 和 ``float('NaN') < 3`` 都会返回 ``False`` . 

* Bytes objects are compared lexicographically using the numeric values of their
  elements.

  字节序列对象通过元素的数字值按字典序比较. 

* Strings are compared lexicographically using the numeric equivalents (the
  result of the built-in function :func:`ord`) of their characters. [#]_ String
  and bytes object can't be compared!

  串按字典序进行数学相等比较 (每个字符的序数用内置函数 :func:`ord` 得到) . 字符串和字符序列不能相互比较! 

* Tuples and lists are compared lexicographically using comparison of
  corresponding elements.  This means that to compare equal, each element must
  compare equal and the two sequences must be of the same type and have the same
  length.

  元组和按列表字典序通过比较对应的项进行比较. 因此 "相等" 意味着两者的每个元素必须是相等的, 两个有序类型必须是相同类型的, 并且长度相同. 

  If not equal, the sequences are ordered the same as their first differing
  elements.  For example, ``[1,2,x] <= [1,2,y]`` has the same value as
  ``x <= y``.  If the corresponding element does not exist, the shorter
  sequence is ordered first (for example, ``[1,2] < [1,2,3]``).

  如果不相等, 有序类型将按第一个不同元素确定顺序. 例如,  ``[1,2,x] <= [1,2,y]`` 与 ``x <= y`` 相等. 如果对应元素不存在, 则短些的有序类型排在前面, 例如,  ``[1,2] < [1,2,3]`` . 

* Mappings (dictionaries) compare equal if and only if they have the same
  ``(key, value)`` pairs. Order comparisons ``('<', '<=', '>=', '>')``
  raise :exc:`TypeError`.

  映射 (字典) 相等, 当且仅当它有相同的 ``(key, value)`` 对. 顺序比较 ``('<', '<=', '>=', '>')`` 会抛出异常 :exc:`TypeError` . 

* Sets and frozensets define comparison operators to mean subset and superset
  tests.  Those relations do not define total orderings (the two sets ``{1,2}``
  and {2,3} are not equal, nor subsets of one another, nor supersets of one
  another).  Accordingly, sets are not appropriate arguments for functions
  which depend on total ordering.  For example, :func:`min`, :func:`max`, and
  :func:`sorted` produce undefined results given a list of sets as inputs.

  集合和冻结集合 (frozenset) 将比较操作符定义成判断是否为真子集和超集测试的操作. 这种关系并没有定义集合间的顺序 (例如,  ``{1,2}`` 与 ``{2,3}`` 并不相等, 同时也相互不为各自的真子集和超集 ) . 因此, 不应该把集合作为参数传递给行为依赖于参数比较结果的函数. 例如函数 :func:`min` 、 :func:`max` 和 :func:`sorted` 在使用集合作为参数时会产生未定义的结果. 

* Most other objects of built-in types compare unequal unless they are the same
  object; the choice whether one object is considered smaller or larger than
  another one is made arbitrarily but consistently within one execution of a
  program.

  大多数其它内置类型对象的比较, 如果对象不同结果就是不等的. 对象间哪个大, 哪个小是不可以预知的, 但相同程序的比较结果是前后一致的. 

Comparison of objects of the differing types depends on whether either
of the types provide explicit support for the comparison.  Most numeric types
can be compared with one another, but comparisons of :class:`float` and
:class:`Decimal` are not supported to avoid the inevitable confusion arising
from representation issues such as ``float('1.1')`` being inexactly represented
and therefore not exactly equal to ``Decimal('1.1')`` which is.  When
cross-type comparison is not supported, the comparison method returns
``NotImplemented``.  This can create the illusion of non-transitivity between
supported cross-type comparisons and unsupported comparisons.  For example,
``Decimal(2) == 2`` and `2 == float(2)`` but ``Decimal(2) != float(2)``.

不同类型对象间的比较行为取决于是否有任何一个类型提供了对这种比较的显式支持. 大多数数值型类型之间可以互相比较, 但不支持 :class:`float` 与 :class:`Decimal` 的比较, 以避免无可规避地表达上的混淆, 例如 ``float('1.1')`` 是不精确的, 因而不会与 ``Decimal('1.1')`` 精确相等. 在不支持交叉类型比较时, 就会返回 ``NotImplemented`` . 注意, 这会造成一种 "支持交叉类型比较" 和 "不支持交叉类型比较" 间的不可传递性的表象, 例如, ``Decimal(2) == 2`` 并且 `2 == float(2)`` 但 ``Decimal(2) != float(2)`` . 

.. _membership-test-details:

The operators :keyword:`in` and :keyword:`not in` test for membership.  ``x in
s`` evaluates to true if *x* is a member of *s*, and false otherwise.  ``x not
in s`` returns the negation of ``x in s``.  All built-in sequences and set types
support this as well as dictionary, for which :keyword:`in` tests whether a the
dictionary has a given key. For container types such as list, tuple, set,
frozenset, dict, or collections.deque, the expression ``x in y`` is equivalent
to ``any(x is e or x == e for e in y)``.

``in`` 运算符和 ``not in`` 运算符用于测试成员资格. 如果 ``x`` 是 ``s`` 的成员, 那么 ``x in s`` 的结果为真, 否则为假.  ``x not in s`` 的结果与上相反. 所有内置有序类型和集合类型、以及字典都支持这种运算, 字典的 :keyword:`in` 会测试左操作是不是它的键. 对于容器类型, 例如列表、元组、集合和冻结集合、字典或者其它collection, 表达式 ``x in y`` 等价于 ``any(x is e or x == e for e in y)`` . 
 
For the string and bytes types, ``x in y`` is true if and only if *x* is a
substring of *y*.  An equivalent test is ``y.find(x) != -1``.  Empty strings are
always considered to be a substring of any other string, so ``"" in "abc"`` will
return ``True``.

对于字符串和字节序列类型,  ``x in y`` 当且仅当 *x* 是 *y* 的子串. 一个等价的测试是 ``y.find(x) != -1`` . 空串被认为是所有字符串的子串, 所以 ``"" in "abc"`` 会返回 ``True`` . 

For user-defined classes which define the :meth:`__contains__` method, ``x in
y`` is true if and only if ``y.__contains__(x)`` is true.

对于定义了 :meth:`__contains__` 方法的用户定义类,  ``x in y`` 为真仅当 ``y.__contains_(x)`` 为真. 

For user-defined classes which do not define :meth:`__contains__` but do define
:meth:`__iter__`, ``x in y`` is true if some value ``z`` with ``x == z`` is
produced while iterating over ``y``.  If an exception is raised during the
iteration, it is as if :keyword:`in` raised that exception.

Lastly, the old-style iteration protocol is tried: if a class defines
:meth:`__getitem__`, ``x in y`` is true if and only if there is a non-negative
integer index *i* such that ``x == y[i]``, and all lower integer indices do not
raise :exc:`IndexError` exception.  (If any other exception is raised, it is as
if :keyword:`in` raised that exception).

如果用户定义类没有定义 :meth:`__contains__` 方法, 但定义了 :meth:`__iter__` 方法, 如果在 ``y`` 上迭代产生的值 ``z`` , 满足 ``x == z`` , 就认为 ``x in y`` 等于真. 如果迭代时引发了任何异常, 就等同于是 :keyword:`in` 引发的一样. 

最后, 尝试使用旧风格的迭代协议, 如果类定义了 :meth:`__getitem__` 方法,  ``x in y`` 为真, 当且仅当存在一个非负的索引 *i* , 使得 ``x == y[i]`` 满足, 并且所有小于该数的索引不能引发 :exc:`IndexError` 异常 (如果引发了任何其它异常, 就等同于是该运算符引发的一样) . 

.. index::
   operator: in
   operator: not in
   pair: membership; test
   object: sequence

The operator :keyword:`not in` is defined to have the inverse true value of
:keyword:`in`.

运算符 :keyword:`not in` 与运算符 :keyword:`in` 有相反的结果. 

.. index::
   operator: is
   operator: is not
   pair: identity; test

The operators :keyword:`is` and :keyword:`is not` test for object identity: ``x
is y`` is true if and only if *x* and *y* are the same object.  ``x is not y``
yields the inverse truth value. [#]_

运算符 :keyword:`is` 和 :keyword:`is not` 测试对象标识:  ``x is y`` 为真, 当且仅当 *x* 和 *y* 是相同的对象.  ``x is not y`` 可以得到相反的结果. 

.. _booleans:
.. _and:
.. _or:
.. _not:

布尔操作
==================

.. index::
   pair: Conditional; expression
   pair: Boolean; operation

.. productionlist::
   or_test: `and_test` | `or_test` "or" `and_test`
   and_test: `not_test` | `and_test` "and" `not_test`
   not_test: `comparison` | "not" `not_test`

In the context of Boolean operations, and also when expressions are used by
control flow statements, the following values are interpreted as false:
``False``, ``None``, numeric zero of all types, and empty strings and containers
(including strings, tuples, lists, dictionaries, sets and frozensets).  All
other values are interpreted as true.  User-defined objects can customize their
truth value by providing a :meth:`__bool__` method.

在布尔运算的上下文里, 以及控制流语句所使用的表达式中, 以下值解释为假:  ``False`` ,  ``None`` , 所有类型的数值零, 空字符串和空容器对象 (包括字符串、元组、列表、字典、集合和冻结集合) . 所有其它值解释为真. 用户定义类型可以通过定义 :meth:`__bool__` 定制其真值类型. 
.. index:: operator: not

The operator :keyword:`not` yields ``True`` if its argument is false, ``False``
otherwise.

如果运算符 :keyword:`not` 的参数为假, 它返回 ``True`` ,否则返回 ``False`` . 

.. index:: operator: and

The expression ``x and y`` first evaluates *x*; if *x* is false, its value is
returned; otherwise, *y* is evaluated and the resulting value is returned.

表达式 ``x and y`` 首先计算 *x* ; 如果 ``x`` 为假, 就返回它的值, 否则就计算 ``y`` 的值并返回其结果. 

.. index:: operator: or

The expression ``x or y`` first evaluates *x*; if *x* is true, its value is
returned; otherwise, *y* is evaluated and the resulting value is returned.

表达式 ``x or y`` 首先计算 *x* ; 如果 ``x`` 为真, 就返回它的值, 否则就计算 ``y`` 的值并返回其结果. 

(Note that neither :keyword:`and` nor :keyword:`or` restrict the value and type
they return to ``False`` and ``True``, but rather return the last evaluated
argument.  This is sometimes useful, e.g., if ``s`` is a string that should be
replaced by a default value if it is empty, the expression ``s or 'foo'`` yields
the desired value.  Because :keyword:`not` has to invent a value anyway, it does
not bother to return a value of the same type as its argument, so e.g., ``not
'foo'`` yields ``False``, not ``''``.)

 (注意 ``and`` 和 ``or`` 都没有限制返回的值和类型必须是 ``False`` 或 ``True`` , 而是最后一个求值的表达式的结果. 在某些情况下这特别有用, 例如, 如果 ``s`` 是一个如果为空就应该被替换成默认值的字符串, 表达式 ``s or ' foo' `` 就会得到希望的结果. 因为 ``not`` 也必须生成一个值, 它的返回值类型不必与其参数的类型相同. 这样, 例如, ``not ' foo' `` 会返回 ``False`` , 而不是 ``''`` . ) 

条件表达式
=======================

.. index::
   pair: conditional; expression
   pair: ternary; operator

.. productionlist::
   conditional_expression: `or_test` ["if" `or_test` "else" `expression`]
   expression: `conditional_expression` | `lambda_form`
   expression_nocond: `or_test` | `lambda_form_nocond`

Conditional expressions (sometimes called a "ternary operator") have the lowest
priority of all Python operations.

条件表达式 (有时称为 "三元操作符" ) 在所有Python运算中的优先级最低. 

The expression ``x if C else y`` first evaluates the condition, *C* (*not* *x*);
if *C* is true, *x* is evaluated and its value is returned; otherwise, *y* is
evaluated and its value is returned.

表达式 ``x if C else y`` 首先计算 *C*  (*不是* *x*), 如果 *C* 为真,  *x* 才被计算并返回它的值; 否则, 计算 *y* 的值并返回之. 

See :pep:`308` for more details about conditional expressions.

关于条件表达式的更多细节, 可以参考 :pep:`308` . 

.. _lambdas:
.. _lambda:

Lambdas
=======

.. index::
   pair: lambda; expression
   pair: lambda; form
   pair: anonymous; function

.. productionlist::
   lambda_form: "lambda" [`parameter_list`]: `expression`
   lambda_form_nocond: "lambda" [`parameter_list`]: `expression_nocond`

Lambda forms (lambda expressions) have the same syntactic position as
expressions.  They are a shorthand to create anonymous functions; the expression
``lambda arguments: expression`` yields a function object.  The unnamed object
behaves like a function object defined with :

Lambda型 (lambda表达式) 在语法上与表达式有相同的位置. 这是一个创建匿名函数的快捷方法, 表达式 ``lambda arguments: expression`` 会生成一个函数对象, 这个无名对象的行为与以下函数行为基本相同::

   def <lambda>(arguments):
       return expression

See section :ref:`function` for the syntax of parameter lists.  Note that
functions created with lambda forms cannot contain statements or annotations.

对于参数表语法, 参见 :ref:`function` . 注意由lambda型创建的函数不能包括语句或者注解 (annotation) . 

.. _exprlists:

表达式列表
================

.. index:: pair: expression; list

.. productionlist::
   expression_list: `expression` ( "," `expression` )* [","]

.. index:: object: tuple

An expression list containing at least one comma yields a tuple.  The length of
the tuple is the number of expressions in the list.  The expressions are
evaluated from left to right.

表达式表是一个包括至少一个逗号的元组, 它的长度是其中表达式的个数, 其中的表达式从左到右按顺序求值. 

.. index:: pair: trailing; comma

The trailing comma is required only to create a single tuple (a.k.a. a
*singleton*); it is optional in all other cases.  A single expression without a
trailing comma doesn't create a tuple, but rather yields the value of that
expression. (To create an empty tuple, use an empty pair of parentheses:
``()``.)

只有在创建单元素元组时 (又称为 *singleton* ) 时才需要最后的逗号, 否则它是可选的. 没有后缀逗号的单独表达式不会创建元组, 但仍会计算该表达式的值 (可以使用一对空括号 ``()`` 创建一个空元组) . 

.. _evalorder:

运算顺序
================

.. index:: pair: evaluation; order

Python evaluates expressions from left to right.  Notice that while evaluating
an assignment, the right-hand side is evaluated before the left-hand side.

Python自左至右的对表达式求值, 但请注意赋值时右侧的求值先于左侧. 

In the following lines, expressions will be evaluated in the arithmetic order of
their suffixes::

   expr1, expr2, expr3, expr4
   (expr1, expr2, expr3, expr4)
   {expr1: expr2, expr3: expr4}
   expr1 + expr2 * (expr3 - expr4)
   expr1(expr2, expr3, *expr4, **expr5)
   expr3, expr4 = expr1, expr2


.. _operator-summary:

总结 (Summary) 
=====================

.. index:: pair: operator; precedence

The following table summarizes the operator precedences in Python, from lowest
precedence (least binding) to highest precedence (most binding).  Operators in
the same box have the same precedence.  Unless the syntax is explicitly given,
operators are binary.  Operators in the same box group left to right (except for
comparisons, including tests, which all have the same precedence and chain from
left to right --- see section :ref:`comparisons` --- and exponentiation, which
groups from right to left).

下表总结了Python中运算符的优先级, 从最低优先级 (最弱的绑定) 到最高优先级(最强的绑定) . 同一格子中的运算符具有相同的优先级. 如果没有特殊的语法规定, 运算符是二元的. 同一格子内的运算符都从左至右结合 (比较运算符和成员资格测试运算符是个例外, 它们有相同的优先级并可以从左到右串接起来 —— 参见 :ref:`comparisons` , 此外, 幂运算符也是从右至左结合的) . 

+-----------------------------------------------+-------------------------------------+
| Operator                                      | Description                         |
+===============================================+=====================================+
| :keyword:`lambda`                             | Lambda expression                   |
+-----------------------------------------------+-------------------------------------+
| :keyword:`if` -- :keyword:`else`              | Conditional expression              |
+-----------------------------------------------+-------------------------------------+
| :keyword:`or`                                 | Boolean OR                          |
+-----------------------------------------------+-------------------------------------+
| :keyword:`and`                                | Boolean AND                         |
+-----------------------------------------------+-------------------------------------+
| :keyword:`not` *x*                            | Boolean NOT                         |
+-----------------------------------------------+-------------------------------------+
| :keyword:`in`, :keyword:`not` :keyword:`in`,  | Comparisons, including membership   |
| :keyword:`is`, :keyword:`is not`, ``<``,      | tests and identity tests,           |
| ``<=``, ``>``, ``>=``, ``!=``, ``==``         |                                     |
+-----------------------------------------------+-------------------------------------+
| ``|``                                         | Bitwise OR                          |
+-----------------------------------------------+-------------------------------------+
| ``^``                                         | Bitwise XOR                         |
+-----------------------------------------------+-------------------------------------+
| ``&``                                         | Bitwise AND                         |
+-----------------------------------------------+-------------------------------------+
| ``<<``, ``>>``                                | Shifts                              |
+-----------------------------------------------+-------------------------------------+
| ``+``, ``-``                                  | Addition and subtraction            |
+-----------------------------------------------+-------------------------------------+
| ``*``, ``/``, ``//``, ``%``                   | Multiplication, division, remainder |
|                                               | [#]_                                |
+-----------------------------------------------+-------------------------------------+
| ``+x``, ``-x``, ``~x``                        | Positive, negative, bitwise NOT     |
+-----------------------------------------------+-------------------------------------+
| ``**``                                        | Exponentiation [#]_                 |
+-----------------------------------------------+-------------------------------------+
| ``x[index]``, ``x[index:index]``,             | Subscription, slicing,              |
| ``x(arguments...)``, ``x.attribute``          | call, attribute reference           |
+-----------------------------------------------+-------------------------------------+
| ``(expressions...)``,                         | Binding or tuple display,           |
| ``[expressions...]``,                         | list display,                       |
| ``{key:datum...}``,                           | dictionary display,                 |
| ``{expressions...}``                          | set display                         |
+-----------------------------------------------+-------------------------------------+


.. rubric:: Footnotes

.. [#] While ``abs(x%y) < abs(y)`` is true mathematically, for floats it may not be
   true numerically due to roundoff.  For example, and assuming a platform on which
   a Python float is an IEEE 754 double-precision number, in order that ``-1e-100 %
   1e100`` have the same sign as ``1e100``, the computed result is ``-1e-100 +
   1e100``, which is numerically exactly equal to ``1e100``.  The function
   :func:`math.fmod` returns a result whose sign matches the sign of the
   first argument instead, and so returns ``-1e-100`` in this case. Which approach
   is more appropriate depends on the application.

   虽然在数学上 ``abs(x%y) < abs(y)`` 一定为真, 但可能因为舍入的原因导致在程序里这个表达式结果不为真. 例如, 假定某个平台使用IEEE 754的双精度浮点数表示Python浮点数, 这样 ``-1e-100 % 1e100`` 与 ``1e100`` 的符号相同, 且计算结果为 ``-1e-100 + 1e100`` , 它在数值上完全等于 ``1e100`` . 而模块 :mod:`math` 中的函数 :func:`fmod` 返回结果的符号与第一个参数的相同, 返回值是 ``-1e-100`` . 哪种方法更合适取决于应用程序. 

.. [#] If x is very close to an exact integer multiple of y, it's possible for
   ``x//y`` to be one larger than ``(x-x%y)//y`` due to rounding.  In such
   cases, Python returns the latter result, in order to preserve that
   ``divmod(x,y)[0] * y + x % y`` be very close to ``x``.

   如果 *x* 非常接近于 *y* 的倍数, 那么因为舍入的原因 ``x//y`` 有可能大于 ``(x-x%y)//y`` . 这时, Python会返回后者作为结果, 以防止 ``divmod(x,y)[0] * y + x % y`` 过于接近 ``x`` . 

.. [#] While comparisons between strings make sense at the byte level, they may
   be counter-intuitive to users.  For example, the strings ``"\u00C7"`` and
   ``"\u0327\u0043"`` compare differently, even though they both represent the
   same unicode character (LATIN CAPITAL LETTER C WITH CEDILLA).  To compare
   strings in a human recognizable way, compare using
   :func:`unicodedata.normalize`.

   虽然字符串的比较就是字节意义上比较, 但它们对于用户来说有可能与直觉冲突. 例如,  ``"\u00C7"`` 与 ``"\u0327\u0043"`` 比较结果是不同, 即使它是其实是相同的unicode字符 (大写拉丁字母C和一个下划线) . 通常意义上的比较, 应该使用  :func:`unicodedata.normalize` . 

.. [#] Due to automatic garbage-collection, free lists, and the dynamic nature of
   descriptors, you may notice seemingly unusual behaviour in certain uses of
   the :keyword:`is` operator, like those involving comparisons between instance
   methods, or constants.  Check their documentation for more info.

.. [#] The ``%`` operator is also used for string formatting; the same
   precedence applies.

.. [#] The power operator ``**`` binds less tightly than an arithmetic or
   bitwise unary operator on its right, that is, ``2**-1`` is ``0.5``.

