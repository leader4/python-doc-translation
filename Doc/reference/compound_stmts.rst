.. _compound:

*******************
复合语句
*******************

.. index:: pair: compound; statement

Compound statements contain (groups of) other statements; they affect or control
the execution of those other statements in some way.  In general, compound
statements span multiple lines, although in simple incarnations a whole compound
statement may be contained in one line.

复合语句包含有其它语句 (组) . 它们以某种方式影响或控制其它语句的执行. 一般地, 复合语句会跨越多行, 但一个完整的复合语句也可以简化在一行中. 

The :keyword:`if`, :keyword:`while` and :keyword:`for` statements implement
traditional control flow constructs.  :keyword:`try` specifies exception
handlers and/or cleanup code for a group of statements, while the
:keyword:`with` statement allows the execution of initialization and
finalization code around a block of code.  Function and class definitions are
also syntactically compound statements.

:keyword:`if` ,  :keyword:`while` 和 :keyword:`for` 语句实现了传统的控制结构.  :keyword:`try` 语句为一组语句指定了异常处理器和/或清理 (cleanup) 代码.  :keyword:`with` 语句允许为其内的代码提供初始化的清理 (finalization) 代码. 函数定义和类定义在语法上也被看作复合语句. 

.. index::
   single: clause
   single: suite

Compound statements consist of one or more 'clauses.'  A clause consists of a
header and a 'suite.'  The clause headers of a particular compound statement are
all at the same indentation level. Each clause header begins with a uniquely
identifying keyword and ends with a colon.  A suite is a group of statements
controlled by a clause.  A suite can be one or more semicolon-separated simple
statements on the same line as the header, following the header's colon, or it
can be one or more indented statements on subsequent lines.  Only the latter
form of suite can contain nested compound statements; the following is illegal,
mostly because it wouldn't be clear to which :keyword:`if` clause a following
:keyword:`else` clause would belong:

复合语句由一个或多个" 子句"  (clause) 组成. 一个子句由一个头和一个" 语句序列"  (suite) 组成. 一个具体复合语句内的所有子句头都具有相同的缩进层次. 每个子句 "头" 以一个唯一性标识关键字开始并以一个冒号结束.  "语句序列" , 是该子句所控制的一组语句, 一个语句序列可以是与子句头处于同一行的, 在子句头冒号之后以分号分隔的多条简单语句, 或者也可以是在后面连续行中缩进的语句. 只有第二种情况下, 子句序列才允许包括嵌套复合语句. 下面这样是非法的, 这样处理大部分原因是不易判断 :keyword:`else` 子句与前面的 :keyword:`if` 子句的配对关系::

   if test1: if test2: print(x)

Also note that the semicolon binds tighter than the colon in this context, so
that in the following example, either all or none of the :func:`print` calls are
executed:

也要注意在这样的上下文中分号的优先级比冒号高, 所以在下面的例子中, 要么执行全部的 :func:`print` 调用, 要么一个也不执行::

   if x < y < z: print(x); print(y); print(z)

Summarizing:
总结: 

.. productionlist::
   compound_stmt: `if_stmt`
                : | `while_stmt`
                : | `for_stmt`
                : | `try_stmt`
                : | `with_stmt`
                : | `funcdef`
                : | `classdef`
   suite: `stmt_list` NEWLINE | NEWLINE INDENT `statement`+ DEDENT
   statement: `stmt_list` NEWLINE | `compound_stmt`
   stmt_list: `simple_stmt` (";" `simple_stmt`)* [";"]

.. index::
   single: NEWLINE token
   single: DEDENT token
   pair: dangling; else

Note that statements always end in a ``NEWLINE`` possibly followed by a
``DEDENT``.  Also note that optional continuation clauses always begin with a
keyword that cannot start a statement, thus there are no ambiguities (the
'dangling :keyword:`else`' problem is solved in Python by requiring nested
:keyword:`if` statements to be indented).

注意语句结尾的 ``NEWLINE`` 之后可能还有一个 ``DEDENT`` , 注意可选的续行子句都是以不能开始另一个语句的关键字开头的, 因此这里不存在歧义 (" 悬挂 :keyword:`else` 问题"已经因为Python要求缩进嵌套语句而解决掉了) . 

The formatting of the grammar rules in the following sections places each clause
on a separate line for clarity.

为了叙述清楚, 以下章节中每个子句的语法规则格式都会分行列出. 

.. _if:
.. _elif:
.. _else:

The :keyword:`if` 语句
===========================

.. index::
   statement: if
   keyword: elif
   keyword: else
           keyword: elif
           keyword: else

The :keyword:`if` statement is used for conditional execution:

:keyword:`if` 语句用于条件执行:

.. productionlist::
   if_stmt: "if" `expression` ":" `suite`
          : ( "elif" `expression` ":" `suite` )*
          : ["else" ":" `suite`]

It selects exactly one of the suites by evaluating the expressions one by one
until one is found to be true (see section :ref:`booleans` for the definition of
true and false); then that suite is executed (and no other part of the
:keyword:`if` statement is executed or evaluated).  If all expressions are
false, the suite of the :keyword:`else` clause, if present, is executed.

它对表达式逐个求值, 直到其中一个为真时, 准确地选择相应的一个语句序列 (对于真和假的定义参见 :ref:`booleans` 节) , 然后该执行语句序列 ( :keyword:`if` 语句的其它部分不会被执行和计算) . 如果所有表达式都为假, 并且给出了 :keyword:`else` 子句, 那么将执行它包括的语句序列. 

.. _while:

The :keyword:`while` 语句
==============================

.. index::
   statement: while
   keyword: else
   pair: loop; statement
   keyword: else

The :keyword:`while` statement is used for repeated execution as long as an
expression is true:

:keyword:`while` 用于重复执行, 前提是条件表达式为真:

.. productionlist::
   while_stmt: "while" `expression` ":" `suite`
             : ["else" ":" `suite`]

This repeatedly tests the expression and, if it is true, executes the first
suite; if the expression is false (which may be the first time it is tested) the
suite of the :keyword:`else` clause, if present, is executed and the loop
terminates.

:keyword:`while` 会重复地计算表达式的值, 并且如果为真, 就执行第一个语句序列; 如果为假 (可能在第一次比较时) , 就执行else子句 (如果给出) 并退出循环. 

.. index::
   statement: break
   statement: continue

A :keyword:`break` statement executed in the first suite terminates the loop
without executing the :keyword:`else` clause's suite.  A :keyword:`continue`
statement executed in the first suite skips the rest of the suite and goes back
to testing the expression.

在第一个语句序列中执行 :keyword:`break` 语句就可以做到不执行 :keyword:`else` 子句而退出循环. 在第一个语句序列执行 :keyword:`continue` 语句可以跳过该子句的其余部分直接进入下次的表达式测试. 

.. _for:

The :keyword:`for` 语句
============================

.. index::
   statement: for
   keyword: in
   keyword: else
   pair: target; list
   pair: loop; statement
   keyword: in
   keyword: else
   pair: target; list
   object: sequence

The :keyword:`for` statement is used to iterate over the elements of a sequence
(such as a string, tuple or list) or other iterable object:

:keyword:`for` 语句用于迭代有序类型 (像串、元组或列表) 或其它可迭代对象的元素:

.. productionlist::
   for_stmt: "for" `target_list` "in" `expression_list` ":" `suite`
           : ["else" ":" `suite`]

The expression list is evaluated once; it should yield an iterable object.  An
iterator is created for the result of the ``expression_list``.  The suite is
then executed once for each item provided by the iterator, in the order of
ascending indices.  Each item in turn is assigned to the target list using the
standard rules for assignments (see :ref:`assignment`), and then the suite is
executed.  When the items are exhausted (which is immediately when the sequence
is empty or an iterator raises a :exc:`StopIteration` exception), the suite in
the :keyword:`else` clause, if present, is executed, and the loop terminates.

只计算一次 *expression_list* , 它应该生成一个迭代器对象. 然后在迭代器每次提供一个元素时就会执行语句序列 (suite) 一次, 元素按索引升序循环给出. 每个元素使用标准的赋值规则 (见 :ref:`assignment` ) 依次赋给循环的 *target_list* , 然后执行语句序列. 当迭代完毕后(当有序类型对象为空, 或者迭代器抛出异常 :exc:`StopIteration` 时立即结束循环) , 就执行 :keyword:`else` 子句 (如果给出) 中的语句序列, 最后结束循环. 

.. index::
   statement: break
   statement: continue

A :keyword:`break` statement executed in the first suite terminates the loop
without executing the :keyword:`else` clause's suite.  A :keyword:`continue`
statement executed in the first suite skips the rest of the suite and continues
with the next item, or with the :keyword:`else` clause if there was no next
item.

在第一个语句序列中执行 :keyword:`break` 语句可以不执行 :keyword:`else` 子句就退出循环. 在第一个语句序列中执行 :keyword:`continue` 语句可以跳过该子句的其余部分, 直接处理下个元素, 或者如果没有下个元素了, 就进入 :keyword:`else` 子句. 

The suite may assign to the variable(s) in the target list; this does not affect
the next item assigned to it.

语句序列可以对 *target_list* 中的变量赋值, 这不影响 :keyword:`for` 语句赋下一项元素给它. 

.. index::
   builtin: range

Names in the target list are not deleted when the loop is finished, but if the
sequence is empty, it will not have been assigned to at all by the loop.  Hint:
the built-in function :func:`range` returns an iterator of integers suitable to
emulate the effect of Pascal's ``for i := a to b do``; e.g., ``list(range(3))``
returns the list ``[0, 1, 2]``.

在循环结束后, 这个 *target_list* 并不会删除, 但如果有序类型对象为空, 它根本就不会在循环中赋值. 小技巧:内置函数 :func:`range` 返回一个整数列表, 可以用于模拟Pascal语言中的 ``for i := a to b`` 的行为, 例如 ``list(range(3))`` 返回列表 ``[0, 1, 2]`` . 

.. note::

   .. index::
      single: loop; over mutable sequence
      single: mutable sequence; loop over

   There is a subtlety when the sequence is being modified by the loop (this can
   only occur for mutable sequences, i.e. lists).  An internal counter is used
   to keep track of which item is used next, and this is incremented on each
   iteration.  When this counter has reached the length of the sequence the loop
   terminates.  This means that if the suite deletes the current (or a previous)
   item from the sequence, the next item will be skipped (since it gets the
   index of the current item which has already been treated).  Likewise, if the
   suite inserts an item in the sequence before the current item, the current
   item will be treated again the next time through the loop. This can lead to
   nasty bugs that can be avoided by making a temporary copy using a slice of
   the whole sequence, e.g., :

   警告:如果在循环中要修改有序类型对象 (仅对可变类型而言, 即列表) , 这里有一些要注意的地方. 有一个内部计数器用于跟踪下一轮循环使用哪一个元素, 并且每次迭代就增加一次. 当这个计数器到达有序类型对象的长度时该循环就结束了. 这意味着如果语句序列删除了当前元素 (或一个之前的元素) 时, 下一个元素就会被跳过去 (因为当前索引值的元素已经处理过了) . 类似地, 如果在当前元素前插入了一个元素, 则当前元素会在下一轮循环再次得到处理. 这可能会导致难以觉察的错误, 但可以通过使用含有整个有序类型对象的片断而生成的临时拷贝避免这个问题, 例如::

      for x in a[:]:
          if x < 0: a.remove(x)


.. _try:
.. _except:
.. _finally:

The :keyword:`try` 语句
============================

.. index::
   statement: try
   keyword: except
   keyword: finally
.. index:: keyword: except

The :keyword:`try` statement specifies exception handlers and/or cleanup code
for a group of statements:

:keyword:`try` 语句为一组语句指定异常处理器和/或清理代码: 

.. productionlist::
   try_stmt: try1_stmt | try2_stmt
   try1_stmt: "try" ":" `suite`
            : ("except" [`expression` ["as" `target`]] ":" `suite`)+
            : ["else" ":" `suite`]
            : ["finally" ":" `suite`]
   try2_stmt: "try" ":" `suite`
            : "finally" ":" `suite`


The :keyword:`except` clause(s) specify one or more exception handlers. When no
exception occurs in the :keyword:`try` clause, no exception handler is executed.
When an exception occurs in the :keyword:`try` suite, a search for an exception
handler is started.  This search inspects the except clauses in turn until one
is found that matches the exception.  An expression-less except clause, if
present, must be last; it matches any exception.  For an except clause with an
expression, that expression is evaluated, and the clause matches the exception
if the resulting object is "compatible" with the exception.  An object is
compatible with an exception if it is the class or a base class of the exception
object or a tuple containing an item compatible with the exception.

:keyword:`except` 子句指定了一个或多个异常处理器. 当在 :keyword:`try` 子句中没有异常发生时, 异常处理器将不被执行. 当在 :keyword:`try` 子句中有异常发生时, 就会开始搜索异常处理器. 它会按书写顺序搜索每个子句, 直到有一个匹配的处理器找到为止. 如果存在一个没有指定异常的 :keyword:`except` 子句, 它必须放在最后, 它会匹配任何异常. 当一个 :keyword:`except` 子句携带了一个表达式时, 这个表达式会被求值, 如果结果与该异常" 兼容" , 那么该子句就匹配上了这个异常. 对象与异常兼容是指, 对象与这个异常的类或者基类相同, 或者对象是一个元组, 它的某个项包括与该异常兼容的对象. 

If no except clause matches the exception, the search for an exception handler
continues in the surrounding code and on the invocation stack.  [#]_

如果没有 :keyword:`except` 子句匹配异常, 异常处理器的搜索工作将继续在外层代码和调用栈上进行. 

If the evaluation of an expression in the header of an except clause raises an
exception, the original search for a handler is canceled and a search starts for
the new exception in the surrounding code and on the call stack (it is treated
as if the entire :keyword:`try` statement raised the exception).

如果在 :keyword:`except` 子句头部计算表达式时引发了异常, 那么就会中断原异常处理器的搜索工作, 而在外层代码和调用栈上搜索新的异常处理器 (就好像是整个 :keyword:`try` 语句发生了异常一样) . 

When a matching except clause is found, the exception is assigned to the target
specified after the :keyword:`as` keyword in that except clause, if present, and
the except clause's suite is executed.  All except clauses must have an
executable block.  When the end of this block is reached, execution continues
normally after the entire try statement.  (This means that if two nested
handlers exist for the same exception, and the exception occurs in the try
clause of the inner handler, the outer handler will not handle the exception.)

当找到了一个匹配的 :keyword:`except` 子句时, 异常对象就被赋给 :keyword:`except` 子句中关键字 :keyword:`as` 指定的目标对象 (如果给出) , 并且执行其
后的语句序列. 每个 :keyword:`except` 子句必须一个可执行代码块. 当执行到该代码块末尾时, 会跳转到整个 :keyword:`try` 语句之后继续正常执行 (这意味着, 如果有两个嵌套的异常处理器要处理同一个异常的话, 那么如果异常已经在内层处理了, 外层处理器就不会响应这个异常了) . 

When an exception has been assigned using ``as target``, it is cleared at the
end of the except clause.  This is as if :

在使用 ``as target`` 形式将异常赋值时, 它会在 :keyword:`except` 子句结束时自动清除掉::

   except E as N:
       foo

was translated to ::

   except E as N:
       try:
           foo
       finally:
           del N

This means the exception must be assigned to a different name to be able to
refer to it after the except clause.  Exceptions are cleared because with the
traceback attached to them, they form a reference cycle with the stack frame,
keeping all locals in that frame alive until the next garbage collection occurs.

这意味着如果你想在 :keyword:`except` 子句之后访问这个异常, 就必须在处理它时把它赋给另一个变量. 这么设计的原因在于回溯跟踪对象与这个异常关联, 而它们与栈桢会构成了一个引用循环, 从而使栈桢上所有局部变量直到下次垃圾回收时才被回收. 

.. index::
   module: sys
   object: traceback

Before an except clause's suite is executed, details about the exception are
stored in the :mod:`sys` module and can be access via :func:`sys.exc_info`.
:func:`sys.exc_info` returns a 3-tuple consisting of the exception class, the
exception instance and a traceback object (see section :ref:`types`) identifying
the point in the program where the exception occurred.  :func:`sys.exc_info`
values are restored to their previous values (before the call) when returning
from a function that handled an exception.

.. index::
   keyword: else
   statement: return
   statement: break
   statement: continue

The optional :keyword:`else` clause is executed if and when control flows off
the end of the :keyword:`try` clause. [#]_ Exceptions in the :keyword:`else`
clause are not handled by the preceding :keyword:`except` clauses.

当控制从 :keyword:`try` 子句的尾部结束时 (即没有异常发生时) , 就执行可选的 :keyword:`else` 子句. 在 :keyword:`else` 子句中引发的异常不会在前面的 :keyword:`except` 子句里得到处理. 

.. index:: keyword: finally

If :keyword:`finally` is present, it specifies a 'cleanup' handler.  The
:keyword:`try` clause is executed, including any :keyword:`except` and
:keyword:`else` clauses.  If an exception occurs in any of the clauses and is
not handled, the exception is temporarily saved. The :keyword:`finally` clause
is executed.  If there is a saved exception, it is re-raised at the end of the
:keyword:`finally` clause. If the :keyword:`finally` clause raises another
exception or executes a :keyword:`return` or :keyword:`break` statement, the
saved exception is lost.  The exception information is not available to the
program during execution of the :keyword:`finally` clause.

如果给出了 :keyword:`finally` , 它就指定一个"清理"处理器 (cleanup handler) . 这种语法下,  :keyword:`try` 子句会得到执行, 也包括任何 :keyword:`except` 和 :keyword:`else` 子句. 如果在任何子句中发生了异常, 并且这个异常没有得到处理, 该异常就会被临时保存起来. 之后,  :keyword:`finally` 子句就会得以执行. 然后暂存的异常在 :keyword:`finally` 子句末尾被重新引发. 如果执行 :keyword:`finally` 子句时引发了另一个异常或执行了:keyword:`return` 或 :keyword:`break` 语句, 就会抛弃保存的异常. 在执行 :keyword:`finally` 子句时异常信息是无效的. 

.. index::
   statement: return
   statement: break
   statement: continue

When a :keyword:`return`, :keyword:`break` or :keyword:`continue` statement is
executed in the :keyword:`try` suite of a :keyword:`try`...\ :keyword:`finally`
statement, the :keyword:`finally` clause is also executed 'on the way out.' A
:keyword:`continue` statement is illegal in the :keyword:`finally` clause. (The
reason is a problem with the current implementation --- this restriction may be
lifted in the future).

当在 :keyword:`try` ...\ :keyword:`finally` 语句中的 :keyword:`try` 语句序列中执行 :keyword:`return` 、 :keyword:`break` 或 :keyword:`continue`  时,  :keyword:`finally` 子句也会 "在退出的路上" 被执行. 在 :keyword:`finally` 子句中的 :keyword:`continue` 语句是非法的 (这缘于因为当前实现中的一个问题——以后可能会去掉这个限制) . 

Additional information on exceptions can be found in section :ref:`exceptions`,
and information on using the :keyword:`raise` statement to generate exceptions
may be found in section :ref:`raise`.

关于异常的更多信息可以在 :ref:`exceptions` 中找到, 关于如何使用 :keyword:`raise` 语句产生异常的信息, 可以在 :ref:`raise` 中找到. 

.. _with:
.. _as:

The :keyword:`with` 语句
=============================

.. index:: statement: with

The :keyword:`with` statement is used to wrap the execution of a block with
methods defined by a context manager (see section :ref:`context-managers`).
This allows common :keyword:`try`...\ :keyword:`except`...\ :keyword:`finally`
usage patterns to be encapsulated for convenient reuse.

:keyword:`with` 语句用于封装上下文管理器 (见 :ref:`context-managers` ) 定义的方法的代码块的执行. 这允许我们方便地复用常见的 :keyword:`try`...\ :keyword:`except`...\ :keyword:`finally` 使用模式. 

.. productionlist::
   with_stmt: "with" with_item ("," with_item)* ":" `suite`
   with_item: `expression` ["as" `target`]

The execution of the :keyword:`with` statement with one "item" proceeds as follows:

#. The context expression (the expression given in the :token:`with_item`) is
   evaluated to obtain a context manager.

#. The context manager's :meth:`__exit__` is loaded for later use.

   对上下文表达式求值得到一个上下文管理器. 
   
#. The context manager's :meth:`__enter__` method is invoked.

   调用上下文管理器的 :meth:`__enter__` 方法. 

#. If a target was included in the :keyword:`with` statement, the return value
   from :meth:`__enter__` is assigned to it.

   如果 :keyword:`with` 语句包括有 target , 就将 :meth:`__enter__` 的返回值赋给它. 

   .. note::

      The :keyword:`with` statement guarantees that if the :meth:`__enter__`
      method returns without an error, then :meth:`__exit__` will always be
      called. Thus, if an error occurs during the assignment to the target list,
      it will be treated the same as an error occurring within the suite would
      be. See step 6 below.

      :keyword:`with` 语句保证了如果 :meth:`__enter__` 是无错返回的, 就一定会调用 :meth:`__exit__` 方法. 如果在给 target list 赋值时发生错误, 就按在语句序列里发生错误同样对待, 参见下面的步骤６. 

#. The suite is executed.

   执行语句序列. 

#. The context manager's :meth:`__exit__` method is invoked.  If an exception
   caused the suite to be exited, its type, value, and traceback are passed as
   arguments to :meth:`__exit__`. Otherwise, three :const:`None` arguments are
   supplied.

   调用上下文管理器的 :meth:`__exit__` 方法. 如果语句序列导致了一个异常, 那么异常的异常的类型, 值和回溯对象都作为参数传递给 :meth:`__exit__` 方法. 否则, 使用 :const:`None` 作为参数. 

   If the suite was exited due to an exception, and the return value from the
   :meth:`__exit__` method was false, the exception is reraised.  If the return
   value was true, the exception is suppressed, and execution continues with the
   statement following the :keyword:`with` statement.

   如果语句序列因为异常退出, 且 :meth:`__exit__` 方法返回假, 那么异常就会重新抛出. 如果返回值为真, 异常就会被 "吃掉" , 并且执行会在 :keyword:`with` 语句之后继续. 

   If the suite was exited for any reason other than an exception, the return
   value from :meth:`__exit__` is ignored, and execution proceeds at the normal
   location for the kind of exit that was taken.

   如果语句序列不是因为异常的原因退出的, 那么 :meth:`__exit__` 的返回值会被忽略掉, 并且在退出点后继续运行程序. 

With more than one item, the context managers are processed as if multiple
:keyword:`with` statements were nested::

   with A() as a, B() as b:
       suite

is equivalent to ::

   with A() as a:
       with B() as b:
           suite

.. versionchanged:: 3.1
   Support for multiple context expressions.

.. seealso::

   :pep:`0343` - The "with" statement
      The specification, background, and examples for the Python :keyword:`with`
      statement.


.. _function:
.. _def:

函数定义
====================

.. index::
   statement: def
   pair: function; definition
   pair: function; name
   pair: name; binding
   object: user-defined function
   object: function
   pair: function; name
   pair: name; binding

A function definition defines a user-defined function object (see section
:ref:`types`):

"函数定义"定义了一个用户定义函数对象 (见 :ref:`types` ) : 

.. productionlist::
   funcdef: [`decorators`] "def" `funcname` "(" [`parameter_list`] ")" ["->" `expression`] ":" `suite`
   decorators: `decorator`+
   decorator: "@" `dotted_name` ["(" [`argument_list` [","]] ")"] NEWLINE
   dotted_name: `identifier` ("." `identifier`)*
   parameter_list: (`defparameter` ",")*
                 : (  "*" [`parameter`] ("," `defparameter`)*
                 : [, "**" `parameter`]
                 : | "**" `parameter`
                 : | `defparameter` [","] )
   parameter: `identifier` [":" `expression`]
   defparameter: `parameter` ["=" `expression`]
   funcname: `identifier`


A function definition is an executable statement.  Its execution binds the
function name in the current local namespace to a function object (a wrapper
around the executable code for the function).  This function object contains a
reference to the current global namespace as the global namespace to be used
when the function is called.

函数定义是一个可执行语句. 执行它会在当前局部名字空间中将函数名字与函数对象 (一个函数可执行代码的包装对象) 绑定在一起. 这个函数对象包括一个全局名字空间的引用, 以便在调用时使用. 

The function definition does not execute the function body; this gets executed
only when the function is called. [#]_

函数定义不执行函数体, 它们只在调用时执行. 

.. index::
  statement: @

A function definition may be wrapped by one or more :term:`decorator` expressions.
Decorator expressions are evaluated when the function is defined, in the scope
that contains the function definition.  The result must be a callable, which is
invoked with the function object as the only argument. The returned value is
bound to the function name instead of the function object.  Multiple decorators
are applied in nested fashion. For example, the following code :

函数定义前可能有若干个 :term:`decorator` 表达式. Decorator表达式于函数定义时, 且在函数定义所在的作用域里求值. 结果必须是可调用的, 它以函数对象为唯一参数, 然后它的返回值将与函数名绑定, 而不是函数对象本身. 多个Decorator表达式可以嵌套使用, 例如, 以下代码::

   @f1(arg)
   @f2
   def func(): pass

is equivalent to ::

   def func(): pass
   func = f1(arg)(f2(func))

.. index:: triple: default; parameter; value

When one or more parameters have the form *parameter* ``=`` *expression*, the
function is said to have "default parameter values."  For a parameter with a
default value, the corresponding argument may be omitted from a call, in which
case the parameter's default value is substituted.  If a parameter has a default
value, all following parameters up until the "``*``" must also have a default
value --- this is a syntactic restriction that is not expressed by the grammar.

当一个或多个参数以 *parameter* ``=`` *expression* 形式出现时, 我们就说这个函数具有" 默认参数值" . 对于有默认参数值的参数, 可以在调用时省略它们, 此时他们被赋予默认值. 如果某参数具有默认值, 则它之后直到 "``*``" 的所有参数都必须有默认值 —— 这是以上语法说明中没有表达出来的一个限制. 

**Default parameter values are evaluated when the function definition is
executed.** This means that the expression is evaluated once, when the function
is defined, and that that same "pre-computed" value is used for each call.  This
is especially important to understand when a default parameter is a mutable
object, such as a list or a dictionary: if the function modifies the object
(e.g. by appending an item to a list), the default value is in effect modified.
This is generally not what was intended.  A way around this is to use ``None``
as the default, and explicitly test for it in the body of the function, e.g.:

**默认参数值是在执行函数定义时计算的. ** 这意味着这个表达式仅仅求值一次, 时间是函数定义时, 并且所有调用都使用这个" 预计算" 的值. 在理解默认参数值是一个像列表、字典这样的可变对象时, 这需要特别注意: 如果修改了这个对象 (例如给列表追加了一项) , 默认值也随之修改. 这通常是应该避免的. 避免这个麻烦的一个方法就是使用 ``None`` 作默认值, 然后在函数体中作显式的测试, 例如::

   def whats_on_the_telly(penguin=None):
       if penguin is None:
           penguin = []
       penguin.append("property of the zoo")
       return penguin

.. index::
  statement: *
  statement: **

Function call semantics are described in more detail in section :ref:`calls`. A
function call always assigns values to all parameters mentioned in the parameter
list, either from position arguments, from keyword arguments, or from default
values.  If the form "``*identifier``" is present, it is initialized to a tuple
receiving any excess positional parameters, defaulting to the empty tuple.  If
the form "``**identifier``" is present, it is initialized to a new dictionary
receiving any excess keyword arguments, defaulting to a new empty dictionary.
Parameters after "``*``" or "``*identifier``" are keyword-only parameters and
may only be passed used keyword arguments.

函数调用语义的详细说明, 参见 :ref:`calls` 一节. 函数调用通常会给每个参数表中的参数赋一个值, 值的来源要么是位置参数、要么是关键字参数或者是默认值. 如果给出了  "``*identifier``" 语法, 这个标识符就被初始化成一个接受所有额外位置参数的元组, 默认为空元组. 如果使用了 "``**identiﬁer``" 语法, 它就被初始化成一个接受所有额外关键字参数的字典, 默认为一个新的空字典. 在 "``*``" or "``*identifier``" 之后的参数必须是纯关键字参数, 并且只能使用指定关键字的方式传递. 

.. index:: pair: function; annotations

Parameters may have annotations of the form "``: expression``" following the
parameter name.  Any parameter may have an annotation even those of the form
``*identifier`` or ``**identifier``.  Functions may have "return" annotation of
the form "``-> expression``" after the parameter list.  These annotations can be
any valid Python expression and are evaluated when the function definition is
executed.  Annotations may be evaluated in a different order than they appear in
the source code.  The presence of annotations does not change the semantics of a
function.  The annotation values are available as values of a dictionary keyed
by the parameters' names in the :attr:`__annotations__` attribute of the
function object.

可以使用参数名之后 "``: expression``" 语法为参数添加一个注解. 任何参数都可以有注解, 甚至包括 ``*identifier`` 或 ``**identifier`` . 函数也可以有一个 "返回 "注解, 语法是在参数列表之后使用 "``-> expression``" . 这些注解可以是任何合法的Python表达式, 它是在函数定义时求值的, 但它们的求值顺序可能与在源代码中的书写顺序不同. 使用注解不会改变函数的语义, 注解的值可以通过函数对象的属性 :attr:`__annotations__` 访问, 它是一个字典, 键是参数名字. 

.. index:: pair: lambda; form

It is also possible to create anonymous functions (functions not bound to a
name), for immediate use in expressions.  This uses lambda forms, described in
section :ref:`lambda`.  Note that the lambda form is merely a shorthand for a
simplified function definition; a function defined in a ":keyword:`def`"
statement can be passed around or assigned to another name just like a function
defined by a lambda form.  The ":keyword:`def`" form is actually more powerful
since it allows the execution of multiple statements and annotations.

也可以创建匿名函数 (没有名字与之绑定的函数) , 它可以直接在表达式中使用. 这是通过lambda表达式实现的, 详见 :ref:`lambda` . 注意lambda形式只是一个简单函数的简写形式, 以 :keyword:`def` 定义的函数也可以被传递、或者赋予另一个名字, 与以lambda定义的函数一样. 以 :keyword:`def` 定义的函数功能要更强大些, 因为它允许执行多条语句和注解. 

**Programmer's note:** Functions are first-class objects.  A "``def``" form
executed inside a function definition defines a local function that can be
returned or passed around.  Free variables used in the nested function can
access the local variables of the function containing the def.  See section
:ref:`naming` for details.

**程序员注意:** 在函数定义中执行的 :keyword:`def` 可以创建一个局部函数, 可用于返回和传递. 在嵌套函数里, 可以通过自由变量访问包括这个函数定义的函数的局部变量. 详见 :ref:`naming` . 

.. _class:

类定义
=================

.. index::
   object: class
   statement: class
   pair: class; definition
   pair: class; name
   pair: name; binding
   pair: execution; frame
   single: inheritance
   single: docstring

A class definition defines a class object (see section :ref:`types`):

"类定义"定义一个类对象 (参见 :ref:`types` ) :

.. productionlist::
   classdef: [`decorators`] "class" `classname` [`inheritance`] ":" `suite`
   inheritance: "(" [`argument_list` [","] | `comprehension`] ")"
   classname: `identifier`

A class definition is an executable statement.  The inheritance list usually
gives a list of base classes (see :ref:`metaclasses` for more advanced uses), so
each item in the list should evaluate to a class object which allows
subclassing.  Classes without an inheritance list inherit, by default, from the
base class :class:`object`; hence, ::

   class Foo:
       pass

is equivalent to ::

   class Foo(object):
       pass

The class's suite is then executed in a new execution frame (see :ref:`naming`),
using a newly created local namespace and the original global namespace.
(Usually, the suite contains mostly function definitions.)  When the class's
suite finishes execution, its execution frame is discarded but its local
namespace is saved. [#]_ A class object is then created using the inheritance
list for the base classes and the saved local namespace for the attribute
dictionary.  The class name is bound to this class object in the original local
namespace.

类的语句序列在新的栈桢结构 (见 :ref:`naming` ) 内执行, 它会使用一个新建的局部名字空间和现有全局名字空间 (这个语句序列里通常只有函数定义) . 当这个语句序执行结束时, 就会丢弃掉这个栈桢结构, 但其局部名字空间被保存了下来. 之后, 使用继承关系列表作为基类, 使用保存的名字空间作为属性字典, 创建新的类对象. 最后, 这个新类对象的名字, 会在最初的局部名字空间中与该类对象绑定. 

Class creation can be customized heavily using :ref:`metaclasses <metaclasses>`.

Classes can also be decorated: just like when decorating functions, ::

   @f1(arg)
   @f2
   class Foo: pass

is equivalent to ::

   class Foo: pass
   Foo = f1(arg)(f2(Foo))

The evaluation rules for the decorator expressions are the same as for function
decorators.  The result must be a class object, which is then bound to the class
name.

**Programmer's note:** Variables defined in the class definition are class
attributes; they are shared by instances.  Instance attributes can be set in a
method with ``self.name = value``.  Both class and instance attributes are
accessible through the notation "``self.name``", and an instance attribute hides
a class attribute with the same name when accessed in this way.  Class
attributes can be used as defaults for instance attributes, but using mutable
values there can lead to unexpected results.  :ref:`Descriptors <descriptors>`
can be used to create instance variables with different implementation details.

**程序员注意:** 在类定义中定义的变量是类属性, 它们由所有类实例共享. 实例属性可以使用 ``self.name = value`` 设置值. 实例属性和类属性都可以使用这种方式访问, 但实例属性会掩盖掉类属性. 类属性可以用于实例属性的默认值, 但使用可变对象作为默认值可能导致并非预期的效果, 还可以使用　:ref:`Descriptors <descriptors>` 创建具有不同实现的实例属性. 

.. seealso::

   :pep:`3116` - Metaclasses in Python 3
   :pep:`3129` - Class Decorators


.. rubric:: Footnotes

.. [#] The exception is propagated to the invocation stack only if there is no
   :keyword:`finally` clause that negates the exception.

.. [#] Currently, control "flows off the end" except in the case of an exception
   or the execution of a :keyword:`return`, :keyword:`continue`, or
   :keyword:`break` statement.

.. [#] A string literal appearing as the first statement in the function body is
   transformed into the function's ``__doc__`` attribute and therefore the
   function's :term:`docstring`.

.. [#] A string literal appearing as the first statement in the class body is
   transformed into the namespace's ``__doc__`` item and therefore the class's
   :term:`docstring`.

