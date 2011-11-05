
.. _simple:

*****************
 语句
*****************

.. index:: pair: simple; statement

Simple statements are comprised within a single logical line. Several simple
statements may occur on a single line separated by semicolons.  The syntax for
simple statements is:

简单语句可以在一个逻辑行中表达。某些简单语句可以用分号分隔而占据一行。简单语句的语法如下：

.. productionlist::
   simple_stmt: `expression_stmt`
              : | `assert_stmt`
              : | `assignment_stmt`
              : | `augmented_assignment_stmt`
              : | `pass_stmt`
              : | `del_stmt`
              : | `return_stmt`
              : | `yield_stmt`
              : | `raise_stmt`
              : | `break_stmt`
              : | `continue_stmt`
              : | `import_stmt`
              : | `global_stmt`
              : | `nonlocal_stmt`


.. _exprstmts:

异常语句
=====================

.. index::
   pair: expression; statement
   pair: expression; list
.. index:: pair: expression; list

Expression statements are used (mostly interactively) to compute and write a
value, or (usually) to call a procedure (a function that returns no meaningful
result; in Python, procedures return the value ``None``).  Other uses of
expression statements are allowed and occasionally useful.  The syntax for an
expression statement is:

表达式语句用于计算和写一个值(多用在交互方式下)，或调用一个过程（一个返回没有意义的结果的函数，在Python中, 过程返回值 ``None`` ）。也有其它表达式语句的使用方法，但并不常用。表达式语句的语法如下:

.. productionlist::
   expression_stmt: `expression_list`

An expression statement evaluates the expression list (which may be a single
expression).

表达式语句要对该表达式列表（可能只是一个表达式）求值。

.. index::
   builtin: repr
   object: None
   pair: string; conversion
   single: output
   pair: standard; output
   pair: writing; values
   pair: procedure; call

In interactive mode, if the value is not ``None``, it is converted to a string
using the built-in :func:`repr` function and the resulting string is written to
standard output on a line by itself (except if the result is ``None``, so that
procedure calls do not cause any output.)

在交互方式下，如果其值不为 ``None`` ，就使用内置函数 :func:`repr` 将其转换为字符串，把结果写到标准输出的单独一行上（如果结果为 ``None`` ，这个过程调用不会生成任何输出）。

.. _assignment:

赋值语句
=====================

.. index::
   pair: assignment; statement
   pair: binding; name
   pair: rebinding; name
   object: mutable
   pair: attribute; assignment

Assignment statements are used to (re)bind names to values and to modify
attributes or items of mutable objects:

赋值语句把名字（重新）绑定到值，并且修改可变对象的属性或者可变对象项：

.. productionlist::
   assignment_stmt: (`target_list` "=")+ (`expression_list` | `yield_expression`)
   target_list: `target` ("," `target`)* [","]
   target: `identifier`
         : | "(" `target_list` ")"
         : | "[" `target_list` "]"
         : | `attributeref`
         : | `subscription`
         : | `slicing`
         : | "*" `target`

(See section :ref:`primaries` for the syntax definitions for the last three
symbols.)

（最后三项符号的语法定义参看 :ref:`primaries` 节）

An assignment statement evaluates the expression list (remember that this can be
a single expression or a comma-separated list, the latter yielding a tuple) and
assigns the single resulting object to each of the target lists, from left to
right.

赋值语句对expression_list求值（记住可以是单个表达式或者一个逗号分隔的序列，后者产生一个元组），然后从左到右地将结果对象逐个赋给target list中的每个对象。

.. index::
   single: target
   pair: target; list

Assignment is defined recursively depending on the form of the target (list).
When a target is part of a mutable object (an attribute reference, subscription
or slicing), the mutable object must ultimately perform the assignment and
decide about its validity, and may raise an exception if the assignment is
unacceptable.  The rules observed by various types and the exceptions raised are
given with the definition of the object types (see section :ref:`types`).

根据target (list)的形式，赋值被递归地定义。当target是某可变对象的一部分（属性引用、下标、片断）时，最终一定是该可变对象执行该赋值操作并确定有效性。如果该赋值不可接受，可以抛出异常。不同类型以及抛出异常所遵循的规则在该对象类型的定义中给出（见 :ref:`types` 节）。

.. index:: triple: target; list; assignment

Assignment of an object to a target list, optionally enclosed in parentheses or
square brackets, is recursively defined as follows.

将一个对象赋值给一个target list（可能以方括号或者大括号括住），其递归定义如下。

* If the target list is a single target: The object is assigned to that target.

  如果target list是单个目标，该对象就直接赋予该目标。

* If the target list is a comma-separated list of targets: The object must be an
  iterable with the same number of items as there are targets in the target list,
  and the items are assigned, from left to right, to the corresponding targets.
  (This rule is relaxed as of Python 1.5; in earlier versions, the object had to
  be a tuple.  Since strings are sequences, an assignment like ``a, b = "xy"`` is
  now legal as long as the string has the right length.)

  如果target list是一组用逗号分隔的目标，该对象必须是可迭代的，并且其子项个数与target list中的目标个数相同。这个对象的子项从左到右地逐个赋予对应target。（这个规则从Python 1.5开始放宽了，在早期版本中，对象必须是一个元组。既然字符串是有序类型对象，像”a,b = ”xy””这样的赋值现在也是合法的了，只要该字符串有正确的长度。）

  * If the target list contains one target prefixed with an asterisk, called a
    "starred" target: The object must be a sequence with at least as many items
    as there are targets in the target list, minus one.  The first items of the
    sequence are assigned, from left to right, to the targets before the starred
    target.  The final items of the sequence are assigned to the targets after
    the starred target.  A list of the remaining items in the sequence is then
    assigned to the starred target (the list can be empty).

    target list内包括一个以星号（ ``*`` ）为前缀的target，称之为“星号目标(starred target)”。这时，对象必须是一个有序类型，其元素数至少要与target list中的目标数减一相同。对象中前面的项直接赋予给target list里的“星号目标”之前的target。对象列表中最后的元素赋予“星号对象”之后的target。对象中的其余元素作为一个列表赋给“星号对象”（这个列表可能为空）。

  * Else: The object must be a sequence with the same number of items as there
    are targets in the target list, and the items are assigned, from left to
    right, to the corresponding targets.

    否则：对象必须是一个和target list有一样的元素数的有序类型，并将其值从左至右依次赋予给对应目标。

Assignment of an object to a single target is recursively defined as follows.

将一个object赋给单个target的递归定义如下：

* If the target is an identifier (name):
  
  如果target是一个标识符（名字）：

  * If the name does not occur in a :keyword:`global` or :keyword:`nonlocal`
    statement in the current code block: the name is bound to the object in the
    current local namespace.

    如果名字并没有在当前代码块中的  :keyword:`global` 或者 :keyword:`nonlocal` 语句中出现，这些名字就绑定在当前的局部名字空间中。

  * Otherwise: the name is bound to the object in the global namespace or the
    outer namespace determined by :keyword:`nonlocal`, respectively.

  .. index:: single: destructor

  The name is rebound if it was already bound.  This may cause the reference
  count for the object previously bound to the name to reach zero, causing the
  object to be deallocated and its destructor (if it has one) to be called.

 如果名字已经绑定过就会进行重新绑定。这可能导致名字之前绑定的object引用计数变成零，导致它被释放和调用析构器（如果有）。

* If the target is a target list enclosed in parentheses or in square brackets:
  The object must be an iterable with the same number of items as there are
  targets in the target list, and its items are assigned, from left to right,
  to the corresponding targets.

  如果target是一个由圆括号或方括号括住的target list。object必须是可迭代的，而且与target list有相同数量的项。这个object中的项会从左到右赋予给target。

  .. index:: pair: attribute; assignment

* If the target is an attribute reference: The primary expression in the
  reference is evaluated.  It should yield an object with assignable attributes;
  if this is not the case, :exc:`TypeError` is raised.  That object is then
  asked to assign the assigned object to the given attribute; if it cannot
  perform the assignment, it raises an exception (usually but not necessarily
  :exc:`AttributeError`).

  如果target是一个属性引用：引用中的基元表达式先被求值，这应该产生一个可以进行属性赋值的对象，否则就会导致 :exc:`TypeError` 异常。然后会要求这个对象将object赋予其相应的属性。如果它无法完成赋值，就会产生一个异常（通常是，但不一定总是 :exc:`AttributeError` ）。

  .. _attr-target-note:

  Note: If the object is a class instance and the attribute reference occurs on
  both sides of the assignment operator, the RHS expression, ``a.x`` can access
  either an instance attribute or (if no instance attribute exists) a class
  attribute.  The LHS target ``a.x`` is always set as an instance attribute,
  creating it if necessary.  Thus, the two occurrences of ``a.x`` do not
  necessarily refer to the same attribute: if the RHS expression refers to a
  class attribute, the LHS creates a new instance attribute as the target of the
  assignment:

  注意：如果对象是一个类实例，并且属性引用同时出现在赋值操作符和RHS表达式两侧。 ``a.x`` 要么能够访问实例属性，或者（如果没有不存在对应实例属性）类属性。LHS目标 ``a.x`` 总是会设置实例属性，如果没有就创建一个这样的属性。这样， ``a.x`` 的出现就一定不会引用相同属性了。如果RHS表达式引用了一个类属性，LHS就会创建一个新的实例属性人为赋值的目标::

     class Cls:
         x = 3             # class variable
     inst = Cls()
     inst.x = inst.x + 1   # writes inst.x as 4 leaving Cls.x as 3

  This description does not necessarily apply to descriptor attributes, such as
  properties created with :func:`property`.

  这里的描述对于描述符属性来说并不是一定要满足的，例如用 :func:`property` 创建的特性（property）。

  .. index::
     pair: subscription; assignment
     object: mutable

* If the target is a subscription: The primary expression in the reference is
  evaluated.  It should yield either a mutable sequence object (such as a list)
  or a mapping object (such as a dictionary).  Next, the subscript expression is
  evaluated.

  如果target是一个下标。引用中的基元表达式先被求值，这应该要么产生一个可变有序对象（如列表）或者一个映射类型的对象（如字典）。然后，对下标表达式求值。

  .. index::
     object: sequence
     object: list

  If the primary is a mutable sequence object (such as a list), the subscript
  must yield an integer.  If it is negative, the sequence's length is added to
  it.  The resulting value must be a nonnegative integer less than the
  sequence's length, and the sequence is asked to assign the assigned object to
  its item with that index.  If the index is out of range, :exc:`IndexError` is
  raised (assignment to a subscripted sequence cannot add new items to a list).

  如果基元是一个可变有序类型对象（如列表），下标必须产生一个整数。如果为负数，就加上有序类型的长度作为结果。最终结果应该是小于有序类型对象长度的非负整数，然后，会要求有序类型将object赋予以这个数为索引的元素上。如果索引超出范围，就会导致 :exc:`IndexError` 异常（这里隐含着，通过给下标表达式赋值不能把元素添加到列表中）。

  .. index::
     object: mapping
     object: dictionary

  If the primary is a mapping object (such as a dictionary), the subscript must
  have a type compatible with the mapping's key type, and the mapping is then
  asked to create a key/datum pair which maps the subscript to the assigned
  object.  This can either replace an existing key/value pair with the same key
  value, or insert a new key/value pair (if no key with the same value existed).

  如果基元是一个映射对象（如字典），下标的类型必须与映射的键类型兼容，并且要求映射对象创建一个新“键值对”，它从这个下标映射到要被赋值的对象。这要么会替换掉有相同键值的“键值对”，要么插入一个新“键值对”（如果之前不存在没有这个键）。

  For user-defined objects, the :meth:`__setitem__` method is called with
  appropriate arguments.

  对于用户定义对象，会使用相应参数调用方法  :meth:`__setitem__` 。

  .. index:: pair: slicing; assignment

* If the target is a slicing: The primary expression in the reference is
  evaluated.  It should yield a mutable sequence object (such as a list).  The
  assigned object should be a sequence object of the same type.  Next, the lower
  and upper bound expressions are evaluated, insofar they are present; defaults
  are zero and the sequence's length.  The bounds should evaluate to integers.
  If either bound is negative, the sequence's length is added to it.  The
  resulting bounds are clipped to lie between zero and the sequence's length,
  inclusive.  Finally, the sequence object is asked to replace the slice with
  the items of the assigned sequence.  The length of the slice may be different
  from the length of the assigned sequence, thus changing the length of the
  target sequence, if the object allows it.

  如果target是一个片断。引用中的基元表达式先被求值，这应该产生一个可变有序对象（如列表）。要赋值的对象应该是相同类型的有序类型对象。之后，会对下界和上界表达式求值（如果有的话），默认为0和有序类型对象的长度L。上下界对象应该计算为一个整数，如果其中任何一个为负数，就把长度L加上去，结果值最终应该处于0到长度L之间（即0到L－1）。最后，会要求有序类型对象把片断所指出的子序列替换掉。片断长度可能与赋值序列不同，这会改变target有序类型对象的长度（如果允许的话）。
  
.. impl-detail::

   In the current implementation, the syntax for targets is taken to be the same
   as for expressions, and invalid syntax is rejected during the code generation
   phase, causing less detailed error messages.
 
   在目前的实现中，target的语法要求与表达式相同，而无效语法会在代码生成阶段拒绝，导致不太详细的错误信息。

WARNING: Although the definition of assignment implies that overlaps between the
left-hand side and the right-hand side are 'safe' (for example ``a, b = b, a``
swaps two variables), overlaps *within* the collection of assigned-to variables
are not safe!  For instance, the following program prints ``[0, 2]``:

警告：虽然赋值定义隐含左右两端重叠是“安全”的（例如，使用 ``a, b = b, a`` 交换两个变量），但在被赋值变量的集合（collection）内重叠是不安全的，例如，以下程序就会打印 ``[0, 2]`` ::

   x = [0, 1]
   i = 0
   i, x[i] = 1, 2
   print(x)


.. seealso::

   :pep:`3132` - Extended Iterable Unpacking
      The specification for the ``*target`` feature.


.. _augassign:

增强赋值语句
-------------------------------

.. index::
   pair: augmented; assignment
   single: statement; assignment, augmented

Augmented assignment is the combination, in a single statement, of a binary
operation and an assignment statement:

增量赋值就是在单条语句内合并了一个二元运算和一个赋值语句。

.. productionlist::
   augmented_assignment_stmt: `augtarget` `augop` (`expression_list` | `yield_expression`)
   augtarget: `identifier` | `attributeref` | `subscription` | `slicing`
   augop: "+=" | "-=" | "*=" | "/=" | "//=" | "%=" | "**="
        : | ">>=" | "<<=" | "&=" | "^=" | "|="

(See section :ref:`primaries` for the syntax definitions for the last three
symbols.)

（最后三项符号的语法定义见 :ref:`primaries` ）

An augmented assignment evaluates the target (which, unlike normal assignment
statements, cannot be an unpacking) and the expression list, performs the binary
operation specific to the type of assignment on the two operands, and assigns
the result to the original target.  The target is only evaluated once.

增量赋值语句对target(和一般的赋值语句不同，它不能是展开的对象(unpacking))和表达式列表求值，执行取决于两个操作数间赋值方式的二元运算，并将结果赋值给原先的target。target仅被求值一次。

An augmented assignment expression like ``x += 1`` can be rewritten as ``x = x +
1`` to achieve a similar, but not exactly equal effect. In the augmented
version, ``x`` is only evaluated once. Also, when possible, the actual operation
is performed *in-place*, meaning that rather than creating a new object and
assigning that to the target, the old object is modified instead.

增量赋值语句，比如 ``x+= 1`` , 可以重写为 ``x = x + 1`` ，效果是类似的，但并不完全一样。在增量版本中， ``x`` 仅求值一次。而且只要可能实际操作就会 *就地进行* ，意思是不再创建一个新对象然后将其赋值给目标，而是直接修改已有对象。

With the exception of assigning to tuples and multiple targets in a single
statement, the assignment done by augmented assignment statements is handled the
same way as normal assignments. Similarly, with the exception of the possible
*in-place* behavior, the binary operation performed by augmented assignment is
the same as the normal binary operations.

除了在一条语句中赋值给元组和多个对象的情况，增量赋值语句所完成的赋值用与普通赋值同样的方式处理。类似地，除了可能的 *就地方式* ，由增量赋值执行的二元运算和普通二元运算也是一样的。

For targets which are attribute references, the same :ref:`caveat about class
and instance attributes <attr-target-note>` applies as for regular assignments.

对于target是属性引用的情况，使用与常规赋值相同的规则， :ref:`参见类和实例属性中的特别提示<attr-target-note>`  。

.. _assert:

:keyword:`assert` 语句
===============================

.. index::
   statement: assert
   pair: debugging; assertions

Assert statements are a convenient way to insert debugging assertions into a
program:

断言语句是在程序中插入调试断言的常用方法：

.. productionlist::
   assert_stmt: "assert" `expression` ["," `expression`]

The simple form, ``assert expression``, is equivalent to :

简单形式的, ``assert expression`` ，等价于::

   if __debug__:
      if not expression: raise AssertionError

The extended form, ``assert expression1, expression2``, is equivalent to :

扩展形式的， ``assert expression1, expression2`` 等价于::

   if __debug__:
      if not expression1: raise AssertionError(expression2)

.. index::
   single: __debug__
   exception: AssertionError

These equivalences assume that :const:`__debug__` and :exc:`AssertionError` refer to
the built-in variables with those names.  In the current implementation, the
built-in variable :const:`__debug__` is ``True`` under normal circumstances,
``False`` when optimization is requested (command line option -O).  The current
code generator emits no code for an assert statement when optimization is
requested at compile time.  Note that it is unnecessary to include the source
code for the expression that failed in the error message; it will be displayed
as part of the stack trace.

这些等价式假定了 :const:`__debug_` 和 :exc:`AssertionError` 引用了相应的内置变量。当前实现里，内置变量 :const:`__debug__` 一般为 ``True`` ；在要求优化的情况（命令行选项 :option:`-O` ）为 ``False`` 。当前的代码生成器在优化编译时不会为任何断言语句生成代码。注意，在错误信息包括源代码的作法是多余的，因为它们会作为回溯对象的一部分显示。

Assignments to :const:`__debug__` are illegal.  The value for the built-in variable
is determined when the interpreter starts.

给 :const:`__debug__` 赋值是非法的，内置变量的值都是由解释器在启动时确定的。

.. _pass:

:keyword:`pass` 语句
=============================

.. index::
   statement: pass
   pair: null; operation
           pair: null; operation

.. productionlist::
   pass_stmt: "pass"

:keyword:`pass` is a null operation --- when it is executed, nothing happens.
It is useful as a placeholder when a statement is required syntactically, but no
code needs to be executed, for example:

:keyword:`pass` 是一个空操作——当执行它时，什么也不做。它主要作为一个占位符，即当语法上要求有一个语句，但什么也不需要执行时，例如::

   def f(arg): pass    # a function that does nothing (yet)

   class C: pass       # a class with no methods (yet)


.. _del:

:keyword:`del` 语句
============================

.. index::
   statement: del
   pair: deletion; target
   triple: deletion; target; list

.. productionlist::
   del_stmt: "del" `target_list`

Deletion is recursively defined very similar to the way assignment is defined.
Rather that spelling it out in full details, here are some hints.

删除采用了与赋值相似的递归定义方法，这里不再介绍所有细节，下面是一些提示。

Deletion of a target list recursively deletes each target, from left to right.

target list的递归删除操作会从左到右地删除每个对象。

.. index::
   statement: global
   pair: unbinding; name

Deletion of a name removes the binding of that name from the local or global
namespace, depending on whether the name occurs in a :keyword:`global` statement
in the same code block.  If the name is unbound, a :exc:`NameError` exception
will be raised.
删除名字就是在局部名字空间或全局名字空间删除掉该名字的绑定，从哪个名字空间删除取决于名字是否出现在相同代码块的 :keyword:`globals` 语句中。如果名字没有绑定，就会抛出异常 :exc:`NameError` 。


.. index:: pair: attribute; deletion

Deletion of attribute references, subscriptions and slicings is passed to the
primary object involved; deletion of a slicing is in general equivalent to
assignment of an empty slice of the right type (but even this is determined by
the sliced object).

.. versionchanged:: 3.2

   Previously it was illegal to delete a name from the local namespace if it
   occurs as a free variable in a nested block.

在局部名字空间中删除一个名字是合法的，但如果这个名字是在嵌套块中作为自由变量出现的，就是非法的了。


.. _return:

:keyword:`return` 语句
===============================

.. index::
   statement: return
   pair: function; definition
   pair: class; definition

.. productionlist::
   return_stmt: "return" [`expression_list`]

:keyword:`return` may only occur syntactically nested in a function definition,
not within a nested class definition.

:keyword:`return` 在语法上仅可以出现在函数定义中，不能出现在类定义中。

If an expression list is present, it is evaluated, else ``None`` is substituted.

如果给出了表达式表，就计算其值, 否则就代以 ``None`` 。

:keyword:`return` leaves the current function call with the expression list (or
``None``) as return value.

:keyword:`return` 的作用是离开当前函数调用, 并以表达式表的值（或 ``None`` ）为返回值。

.. index:: keyword: finally

When :keyword:`return` passes control out of a :keyword:`try` statement with a
:keyword:`finally` clause, that :keyword:`finally` clause is executed before
really leaving the function.

当使用 :keyword:`return` 离开具有 :keyword:`finally` 子句的 :keyword:`try` 语句的控制流时， :keyword:`ﬁnally` 子句中的语句会在函数真正退出之前执行。

In a generator function, the :keyword:`return` statement is not allowed to
include an :token:`expression_list`.  In that context, a bare :keyword:`return`
indicates that the generator is done and will cause :exc:`StopIteration` to be
raised.

在生成器函数中。 :keyword:`return` 语句不允许包括 :token:`expression_list` 。在该种情况下, 空 :keyword:`return` 语句指出生成器结束，并引发一个 :exc:`StopIteration` 异常。

.. _yield:

:keyword:`yield` 语句
==============================

.. index::
   statement: yield
   single: generator; function
   single: generator; iterator
   single: function; generator
   exception: StopIteration

.. productionlist::
   yield_stmt: `yield_expression`

The :keyword:`yield` statement is only used when defining a generator function,
and is only used in the body of the generator function. Using a :keyword:`yield`
statement in a function definition is sufficient to cause that definition to
create a generator function instead of a normal function.

:keyword:`yield` 语句只在定义生成器函数时使用，也只能用于生成器函数体中。在一个函数定义中使用 :keyword:`yield` 语句足以导致该定义产生一个生成器函数而不是普通函数。

When a generator function is called, it returns an iterator known as a generator
iterator, or more commonly, a generator.  The body of the generator function is
executed by calling the :func:`next` function on the generator repeatedly until
it raises an exception.

当生成器函数被调用的时候，它返回一个迭代器，称为生成器迭代器，或者更常用的称谓“生成器”。通过反复调用生成器的 :meth:`next` 方法可以运行生成器的函数体，直到抛出一个异常。

When a :keyword:`yield` statement is executed, the state of the generator is
frozen and the value of :token:`expression_list` is returned to :meth:`next`'s
caller.  By "frozen" we mean that all local state is retained, including the
current bindings of local variables, the instruction pointer, and the internal
evaluation stack: enough information is saved so that the next time :func:`next`
is invoked, the function can proceed exactly as if the :keyword:`yield`
statement were just another external call.

当执行一个 :keyword:`yield` 语句时，对应生成器的状态就被冻结起来，而 :token:`expression_list`: 的值则被返回给 :meth:`next` 方法的调用者。所谓“冻结”我们指的是所有局部状态都被保持，包括局部变量的当前绑定、指令指针、内部的求值堆栈——这里保留了足够多的信息使得当下次激活 :meth:`next` 的时候，函数执行起来就好像 :keyword:`yield` 语句不过是另外一个外部调用。

The :keyword:`yield` statement is allowed in the :keyword:`try` clause of a
:keyword:`try` ...  :keyword:`finally` construct.  If the generator is not
resumed before it is finalized (by reaching a zero reference count or by being
garbage collected), the generator-iterator's :meth:`close` method will be
called, allowing any pending :keyword:`finally` clauses to execute.

:keyword:`yield` 语句允许出现于  :keyword:`try` ...  :keyword:`ﬁnally` 结构的  :keyword:`try` 子句当中。如果生成器在它结束（到达零引用计数，或者被垃圾收集）之前还没有执行完，就会调用生成器迭代器的方法 :meth:`close` ，以允许任何可能适合的 :keyword:`finally` 语句的执行。

.. seealso::

   :pep:`0255` - Simple Generators
      The proposal for adding generators and the :keyword:`yield` statement to Python.

      为Python添加生成器和 :keyword:`yield` 语句的提案。

   :pep:`0342` - Coroutines via Enhanced Generators
      The proposal that, among other generator enhancements, proposed allowing
      :keyword:`yield` to appear inside a :keyword:`try` ... :keyword:`finally` block.

      与其他产生器改进的提案一起，这建议在 :keyword:`try` ... :keyword:`finally` 块中允许 :keyword:`yield` 。

.. _raise:

:keyword:`raise` 语句
==============================

.. index::
   statement: raise
   single: exception
   pair: raising; exception
   single: __traceback__ (exception attribute)

.. productionlist::
   raise_stmt: "raise" [`expression` ["from" `expression`]]

If no expressions are present, :keyword:`raise` re-raises the last exception
that was active in the current scope.  If no exception is active in the current
scope, a :exc:`TypeError` exception is raised indicating that this is an error
(if running under IDLE, a :exc:`queue.Empty` exception is raised instead).

如果不给出表达式( :token:`expression` )， :keyword:`raise` 重新引发当前作用域内最后一个引发的异常。如果当前作用域内没有活动的异常，就为引发一个 :exc:`TypeError` 异常，指明出现一个错误（如果是在IDLE下运行的，会以 :exc:`queue.Empty` 异常取代）。

Otherwise, :keyword:`raise` evaluates the first expression as the exception
object.  It must be either a subclass or an instance of :class:`BaseException`.
If it is a class, the exception instance will be obtained when needed by
instantiating the class with no arguments.

否则， :keyword:`raise` 对第一个表达式求值作为异常对象，它要么是 :class:`BaseException` 的子类，或者实例。如果它是一个类，会使用它获取（无参数）一个异常实例。

The :dfn:`type` of the exception is the exception instance's class, the
:dfn:`value` is the instance itself.

异常类型（ :dfn:`type` ）指异常实例的类，值（ :dfn:`value` ）指实例本身。

.. index:: object: traceback

A traceback object is normally created automatically when an exception is raised
and attached to it as the :attr:`__traceback__` attribute, which is writable.
You can create an exception and set your own traceback in one step using the
:meth:`with_traceback` exception method (which returns the same exception
instance, with its traceback set to its argument), like so:

当引发一个异常时，回溯对象一般会自动创建并与异常的可写 :attr:`__traceback__` 属性关联起来。你可以创建一个异常对象，并用异常方法 :meth:`with_traceback` 关联自己的回溯对象（它会返回相同的异常对象，但已经关联好了回溯对象）::

   raise Exception("foo occurred").with_traceback(tracebackobj)

.. index:: pair: exception; chaining
           __cause__ (exception attribute)
           __context__ (exception attribute)

The ``from`` clause is used for exception chaining: if given, the second
*expression* must be another exception class or instance, which will then be
attached to the raised exception as the :attr:`__cause__` attribute (which is
writable).  If the raised exception is not handled, both exceptions will be
printed:

``from`` 子句可以用于实现异常链：如果使用，第二个 *expression* 必须是另一个异常类或者实例，它会写入被引发的异常的可写属性 :attr:`__cause__` 。如果这个异常没有得到处理，这两个异常都会被报告::

   >>> try:
   ...     print(1 / 0)
   ... except Exception as exc:
   ...     raise RuntimeError("Something bad happened") from exc
   ...
   Traceback (most recent call last):
     File "<stdin>", line 2, in <module>
   ZeroDivisionError: int division or modulo by zero

   The above exception was the direct cause of the following exception:

   Traceback (most recent call last):
     File "<stdin>", line 4, in <module>
   RuntimeError: Something bad happened

A similar mechanism works implicitly if an exception is raised inside an
exception handler: the previous exception is then attached as the new
exception's :attr:`__context__` attribute:

另一个类似的机制是在处理异常时引发另一个异常，这样前一个异常都被关联入新异常的 :attr:`__context__` 属性中 ::

   >>> try:
   ...     print(1 / 0)
   ... except:
   ...     raise RuntimeError("Something bad happened")
   ...
   Traceback (most recent call last):
     File "<stdin>", line 2, in <module>
   ZeroDivisionError: int division or modulo by zero

   During handling of the above exception, another exception occurred:

   Traceback (most recent call last):
     File "<stdin>", line 4, in <module>
   RuntimeError: Something bad happened

Additional information on exceptions can be found in section :ref:`exceptions`,
and information about handling exceptions is in section :ref:`try`.

关于异常的额外信息可以参考 :ref:`exceptions` ，关于如何处理异常的信息可以参考 :ref:`try` 。

.. _break:

:keyword:`break` 语句
==============================

.. index::
   statement: break
   statement: for
   statement: while
   pair: loop; statement

.. productionlist::
   break_stmt: "break"

:keyword:`break` may only occur syntactically nested in a :keyword:`for` or
:keyword:`while` loop, but not nested in a function or class definition within
that loop.

:keyword:`break` 在语法上只能出现在 :keyword:`for` 或 :keyword:`while` 循环中, 但不能出现在循环中的函数定义或类定义中。

.. index:: keyword: else
           pair: loop control; target

It terminates the nearest enclosing loop, skipping the optional :keyword:`else`
clause if the loop has one.

它中断最内层的循环，跳过其可选的 :keyword:`else` 语句（如果有）。

If a :keyword:`for` loop is terminated by :keyword:`break`, the loop control
target keeps its current value.

如果 :keyword:`for` 循环被 :keyword:`break` 中断，它的循环控制目标对象还保持当前值。

.. index:: keyword: finally

When :keyword:`break` passes control out of a :keyword:`try` statement with a
:keyword:`finally` clause, that :keyword:`finally` clause is executed before
really leaving the loop.

当使用 :keyword:`break` 离开具有 :keyword:`finally` 子句的 :keyword:`try` 语句的控制流时， :keyword:`finally` 子句中的语句会在循环真正退出之前执行。

.. _continue:

 :keyword:`continue` 语句
=================================

.. index::
   statement: continue
   statement: for
   statement: while
   pair: loop; statement
   keyword: finally

.. productionlist::
   continue_stmt: "continue"

:keyword:`continue` may only occur syntactically nested in a :keyword:`for` or
:keyword:`while` loop, but not nested in a function or class definition or
:keyword:`finally` clause within that loop.  It continues with the next
cycle of the nearest enclosing loop.

:keyword:`continue` 在语法上只能出现在 :keyword:`for` 或 :keyword:`while` 循环中, 但不能出现在循环中的函数定义，类定义或者 :keyword:`finally` 子句中。这个语句继续执行最内层的循环的下一次周期。

When :keyword:`continue` passes control out of a :keyword:`try` statement with a
:keyword:`finally` clause, that :keyword:`finally` clause is executed before
really starting the next loop cycle.

当使用 :keyword:`continue` 离开具有 :keyword:`finally` 子句的 :keyword:`try` 语句的控制流时， :keyword:`finally` 子句中的语句会在循环进入下一个周期之前执行。

.. _import:
.. _from:

:keyword:`import` 语句
===============================

.. index::
   statement: import
   single: module; importing
   pair: name; binding
   keyword: from

.. productionlist::
   import_stmt: "import" `module` ["as" `name`] ( "," `module` ["as" `name`] )*
              : | "from" `relative_module` "import" `identifier` ["as" `name`]
              : ( "," `identifier` ["as" `name`] )*
              : | "from" `relative_module` "import" "(" `identifier` ["as" `name`]
              : ( "," `identifier` ["as" `name`] )* [","] ")"
              : | "from" `module` "import" "*"
   module: (`identifier` ".")* `identifier`
   relative_module: "."* `module` | "."+
   name: `identifier`

Import statements are executed in two steps: (1) find a module, and initialize
it if necessary; (2) define a name or names in the local namespace (of the scope
where the :keyword:`import` statement occurs). The statement comes in two
forms differing on whether it uses the :keyword:`from` keyword. The first form
(without :keyword:`from`) repeats these steps for each identifier in the list.
The form with :keyword:`from` performs step (1) once, and then performs step
(2) repeatedly. For a reference implementation of step (1), see the
:mod:`importlib` module.

:keyword:`import` 语句分两步执行：(1) 找到模块，如果需要则进行初始化；(2) 在( :keyword:`import` 语句所发生的范围内)局部名字空间中定义一个或者多个名字。 :keyword:`import` 语句按是否带 :keyword:`from` 关键字，分为两种形式：第一种形式（不带 :keyword:`from` ）对列表中的每个标枳符重复这些步骤。带 :keyword:`from` 的形式只执行步骤(1)一次，然后重复地执行步骤(2)。关于步骤(1)的参考实现，可以参考 :mod:`importlib` 模块。

.. index::
    single: package

To understand how step (1) occurs, one must first understand how Python handles
hierarchical naming of modules. To help organize modules and provide a
hierarchy in naming, Python has a concept of packages. A package can contain
other packages and modules while modules cannot contain other modules or
packages. From a file system perspective, packages are directories and modules
are files. The original `specification for packages
<http://www.python.org/doc/essays/packages.html>`_ is still available to read,
although minor details have changed since the writing of that document.

要理解第一步是如何进行的，先得明白Python是如何处理模块间的名字层次的，为了更好的组织模块和提供名字空间，Python引入了包的概念。一个包可以包括其他包和模块，但模块不能包括其他模块和包。从文件系统的角度上看，如果包是目录的话，模块就是文件了。最初的 `包规范 <http://www.python.org/doc/essays/packages.html>`_ 仍然值得参考，虽然某些细节已经不同了。

.. index::
    single: sys.modules

Once the name of the module is known (unless otherwise specified, the term
"module" will refer to both packages and modules), searching
for the module or package can begin. The first place checked is
:data:`sys.modules`, the cache of all modules that have been imported
previously. If the module is found there then it is used in step (2) of import
unless ``None`` is found in :data:`sys.modules`, in which case
:exc:`ImportError` is raised.

一旦模块的名字已知了（如果没有特别指定，术语“模块”可以指包或者模块），对模块或者包的搜索就可以开始了。检查的第一个位置是 :data:`sys.modules` ，即之前导入的所有模块的缓存。如果模块在缓存中找到了，就执行导入过程的第二步，但是如果 :data:`sys.modules`　找到的是 :keyword:`None` ，就会抛出异常 :exc:`ImportError`  。

.. index::
    single: sys.meta_path
    single: finder
    pair: finder; find_module
    single: __path__

If the module is not found in the cache, then :data:`sys.meta_path` is searched
(the specification for :data:`sys.meta_path` can be found in :pep:`302`).
The object is a list of :term:`finder` objects which are queried in order as to
whether they know how to load the module by calling their :meth:`find_module`
method with the name of the module. If the module happens to be contained
within a package (as denoted by the existence of a dot in the name), then a
second argument to :meth:`find_module` is given as the value of the
:attr:`__path__` attribute from the parent package (everything up to the last
dot in the name of the module being imported). If a finder can find the module
it returns a :term:`loader` (discussed later) or returns ``None``.

如果在缓冲中没有找到模块，然后就会搜索 :data:`sys.meta_path` （ :data:`sys.meta_path` 的规范可以在 :pep:`302` 中找到），它是一个被顺序查询 :term:`finder` 对象列表，通过传递模块名调用它们的 :meth:`find_module` 方法以决定是否装载该模块。如果模块是某个包的一部分（即名字中包含句号），那么需要指定“父包”（即模块名最后一个句号之前的部分）的 :attr:`__path__` 属性值作为 :meth:`find_module` 方法的第二个参数。如果一个finder找到了可用模块，它就会返回一个 :term:`loader` 或者返回 `None` 。

.. index::
    single: sys.path_hooks
    single: sys.path_importer_cache
    single: sys.path

If none of the finders on :data:`sys.meta_path` are able to find the module
then some implicitly defined finders are queried. Implementations of Python
vary in what implicit meta path finders are defined. The one they all do
define, though, is one that handles :data:`sys.path_hooks`,
:data:`sys.path_importer_cache`, and :data:`sys.path`.

如果没有finder能够在 :data:`sys.meta_path` 找到模块，就会查询一些隐式定义的finder。在隐式finder细节上，各种Python实现各不相同，但它们都会处理 :data:`sys.path_hooks` 、 :data:`sys.path_importer_cache` 和 :data:`sys.path` 。

The implicit finder searches for the requested module in the "paths" specified
in one of two places ("paths" do not have to be file system paths). If the
module being imported is supposed to be contained within a package then the
second argument passed to :meth:`find_module`, :attr:`__path__` on the parent
package, is used as the source of paths. If the module is not contained in a
package then :data:`sys.path` is used as the source of paths.

隐式finder在两种指定“路径”上搜索指定模块（“路径”并不一定是文件系统路径）。如果模块作为包的一部分导入，那么 :meth:`find_module` 方法的第二个参数，即父包的 :attr:`__path__` 属性就作为路径源。如果模块不包括在任何包内，就使用 :data:`sys.path` 作为路径源。

Once the source of paths is chosen it is iterated over to find a finder that
can handle that path. The dict at :data:`sys.path_importer_cache` caches
finders for paths and is checked for a finder. If the path does not have a
finder cached then :data:`sys.path_hooks` is searched by calling each object in
the list with a single argument of the path, returning a finder or raises
:exc:`ImportError`. If a finder is returned then it is cached in
:data:`sys.path_importer_cache` and then used for that path entry. If no finder
can be found but the path exists then a value of ``None`` is
stored in :data:`sys.path_importer_cache` to signify that an implicit,
file-based finder that handles modules stored as individual files should be
used for that path. If the path does not exist then a finder which always
returns ``None`` is placed in the cache for the path.

一旦选择了某路径源，就会迭代寻找一个可以处理该路径的finder。字典 :data:`sys.path_importer_cache` 为路径源缓冲了finder，因此先搜索这个字典。如果路径没有缓冲对应的finder，那么就搜索 :data:`sys.path_hooks` 使用路径作为唯一的参数调用其中的每个对象，以返回一个finder或者引起异常 :exc:`ImportError` 。如果找到一个finder，它就会被缓冲在 :data:`sys.path_importer_cache` ，然后用于相应的路径上。如果没有找到对应的finder，但这个路径是存在的，就会在 :data:`sys.path_importer_cache` 对应位置上存上 `None` ，以标明这里使用隐式finder，即能够处理单独文件作为模块的finder。如果路径不存在，那么应该把一个总是返回 :keyword:`None` 的finder存储在缓冲里。

.. index::
    single: loader
    pair: loader; load_module
    exception: ImportError

If no finder can find the module then :exc:`ImportError` is raised. Otherwise
some finder returned a loader whose :meth:`load_module` method is called with
the name of the module to load (see :pep:`302` for the original definition of
loaders). A loader has several responsibilities to perform on a module it
loads. First, if the module already exists in :data:`sys.modules` (a
possibility if the loader is called outside of the import machinery) then it
is to use that module for initialization and not a new module. But if the
module does not exist in :data:`sys.modules` then it is to be added to that
dict before initialization begins. If an error occurs during loading of the
module and it was added to :data:`sys.modules` it is to be removed from the
dict. If an error occurs but the module was already in :data:`sys.modules` it
is left in the dict.

如果没有任何finder能够找到一个指定模块，就是应该引发 :exc:`ImportError` 异常。否则，某些finder就应该返回一个loader，使用模块名作为参数调用loader的 :meth:`load_module` 方法加载模块（关于loader的最初定义参见 :pep:`302` ）。loader在加载模块时需要完成一系列任务。首先，如果模块已经在 :data:`sys.modules` 字典中已经存在了（一种可能是在导入机制之外使用过了loader），它就直接使用这个模块进行初始化而不创建新模块了。如果 :data:`sys.modules` 内不存在该模块，它就会在初始化新模块之前把它加入到其中。如果在加载模块时发生错误，就会把刚刚加入 :data:`sys.modules` 中的模块删除掉。如果这个模块在加载之前就已经存在于这个字典中，即使发生错误也不会删除它。

.. index::
    single: __name__
    single: __file__
    single: __path__
    single: __package__
    single: __loader__

The loader must set several attributes on the module. :data:`__name__` is to be
set to the name of the module. :data:`__file__` is to be the "path" to the file
unless the module is built-in (and thus listed in
:data:`sys.builtin_module_names`) in which case the attribute is not set.
If what is being imported is a package then :data:`__path__` is to be set to a
list of paths to be searched when looking for modules and packages contained
within the package being imported. :data:`__package__` is optional but should
be set to the name of package that contains the module or package (the empty
string is used for module not contained in a package). :data:`__loader__` is
also optional but should be set to the loader object that is loading the
module.

loader必须在模块上设置一些属性 :data:`__name__` 设为模块的名字。如果不是内置模块（列出于 :data:`sys.builtin_module_names` 中）， :data:`__file__` 会被设置为文件名，否则就不设置这个属性。如果导入的是一个包，那么 :data:`__path__` 应该设置为搜索这个模块和上层包的搜索路径列表。 :data:`__package__` 是可选的，应该被设置为包括这个模块或包的包名（如果没有上层包就为空串）。 :data:`__loader__` 也是可选的，应该设置为加载这个模块的loader。

.. index::
    exception: ImportError

If an error occurs during loading then the loader raises :exc:`ImportError` if
some other exception is not already being propagated. Otherwise the loader
returns the module that was loaded and initialized.

如果加载过程中发生错误，并且没有传播的其他异常，loader就会引发 :exc:`ImportError` 异常。否则，loader就会返回一个加载好和完成初始化的模块。

When step (1) finishes without raising an exception, step (2) can begin.

在第一步完成之后，第二步就可以开始了。

The first form of :keyword:`import` statement binds the module name in the local
namespace to the module object, and then goes on to import the next identifier,
if any.  If the module name is followed by :keyword:`as`, the name following
:keyword:`as` is used as the local name for the module.

:keyword:`import` 语句的第一种形式（无 :keyword:`from` ）将模块名在局部名字空间中绑定到模块对象上，然后继续导入下一个标识符（如果有）。如果模块名是跟在 :keyword:`as` 之后的，这个名字就在局部名字空间表示这个模块。

.. index::
   pair: name; binding
   exception: ImportError

The :keyword:`from` form does not bind the module name: it goes through the list
of identifiers, looks each one of them up in the module found in step (1), and
binds the name in the local namespace to the object thus found.  As with the
first form of :keyword:`import`, an alternate local name can be supplied by
specifying ":keyword:`as` localname".  If a name is not found,
:exc:`ImportError` is raised.  If the list of identifiers is replaced by a star
(``'*'``), all public names defined in the module are bound in the local
namespace of the :keyword:`import` statement.

:keyword:`from` 形式并不绑定模块名：它会遍历标识符列表，直到遇到在第一步中找到的模块，将其在局部名字空间中的名字绑定。与第一种形式的 :keyword:`import` 语句相同，也可以使用 ":keyword:`as` localname"  提供另一个名字。如果名字没有找到，就会引发 :exc:`ImportError` 异常。如果标识符列表由一个星号( ``*`` )代替，模块中的所有公开名字都会绑定在 :keyword:`import` 语句的局部名字空间中。

.. index:: single: __all__ (optional module attribute)

The *public names* defined by a module are determined by checking the module's
namespace for a variable named ``__all__``; if defined, it must be a sequence of
strings which are names defined or imported by that module.  The names given in
``__all__`` are all considered public and are required to exist.  If ``__all__``
is not defined, the set of public names includes all names found in the module's
namespace which do not begin with an underscore character (``'_'``).
``__all__`` should contain the entire public API. It is intended to avoid
accidentally exporting items that are not part of the API (such as library
modules which were imported and used within the module).

对模块的 *公开名字* 的判定，是通过检查模块名字空间中的一个名为 ``__all__`` 变量。如果定义了该变量，它必须是一个字符串序列，保存了由该模块定义或导入的名字。 ``__all__`` 中的名字都被认为是公开的，并且要求是存在的。如果没有定义 ``__all__`` ，公开名字集合就包括在模块中找到的不以下划线（ ``_`` ）开头的所有名字。 ``__all__`` 应该包括完整的公开API，它就是为了避免导出不是API的符号项（例如模块内导入和使用的库模块）。

The :keyword:`from` form with ``*`` may only occur in a module scope.  The wild
card form of import --- ``import *`` --- is only allowed at the module level.
Attempting to use it in class or function definitions will raise a
:exc:`SyntaxError`.

使用 ``*`` 形式的 :keyword:`from` 语句只能出现在模块作用域内。类似地通配形式的导入 —— ``import *`` —— 也只允许在模块作用域内使用。当类或者函数定义中使用它们时会导致发生异常 :exc:`SyntaxError` 。

.. index::
    single: relative; import

When specifying what module to import you do not have to specify the absolute
name of the module. When a module or package is contained within another
package it is possible to make a relative import within the same top package
without having to mention the package name. By using leading dots in the
specified module or package after :keyword:`from` you can specify how high to
traverse up the current package hierarchy without specifying exact names. One
leading dot means the current package where the module making the import
exists. Two dots means up one package level. Three dots is up two levels, etc.
So if you execute ``from . import mod`` from a module in the ``pkg`` package
then you will end up importing ``pkg.mod``. If you execute ``from ..subpkg2
import mod`` from within ``pkg.subpkg1`` you will import ``pkg.subpkg2.mod``.
The specification for relative imports is contained within :pep:`328`.

在指定你要导入的模块时，你并不一定要指定模块的绝对名字。当一个模块或包是在其他包之内时，在同一个顶层包下，我们有可能使用不需指明包名的相对加载。通过在 :keyword:`from` 之后的模块或者包之前指定几个句号，就可以在没有精确名字的情况下，指定在当前包层次中向上搜索多少层。一个句号代表包括导入操作所在模块的当前包。两个句号代表上一个包层次，三个句号代表再上一个包层次，依次类推。所以，如果你在 ``pkg`` 包中执行 ``from . import mod`` ，你就会导入 ``pkg.mod`` 。如果你从包 ``pkg.subpkg1`` 执行 ``from ..subpkg2 import mod`` ，你就会导入 ``pkg.subpkg2.mod`` 。相对导入的规范参见 :pep:`328` 。

:func:`importlib.import_module` is provided to support applications that
determine which modules need to be loaded dynamically.


.. _future:

预览语句（Future statements）
-----------------------------------------

.. index:: pair: future; statement

A :dfn:`future statement` is a directive to the compiler that a particular
module should be compiled using syntax or semantics that will be available in a
specified future release of Python.  The future statement is intended to ease
migration to future versions of Python that introduce incompatible changes to
the language.  It allows use of the new features on a per-module basis before
the release in which the feature becomes standard.

:dfn:`预览语句` 是针对编译器的一个功能指示，通知其要编译某些模块，以打开某些会在以后版本的Python中正式出现的语法或语义。预览语句的目标在于，简化与以后版本的Python的迁移工作，这种新版本在语言层面上引入了与老版本并不兼容的特征。这也允许我们在某些功能成为标准功能之前时，在模块层次上使用它们。

.. productionlist:: *
   future_statement: "from" "__future__" "import" feature ["as" name]
                   : ("," feature ["as" name])*
                   : | "from" "__future__" "import" "(" feature ["as" name]
                   : ("," feature ["as" name])* [","] ")"
   feature: identifier
   name: identifier

A future statement must appear near the top of the module.  The only lines that
can appear before a future statement are:

* the module docstring (if any),
* comments,
* blank lines, and
* other future statements.

.. XXX change this if future is cleaned out

The features recognized by Python 3.0 are ``absolute_import``, ``division``,
``generators``, ``unicode_literals``, ``print_function``, ``nested_scopes`` and
``with_statement``.  They are all redundant because they are always enabled, and
only kept for backwards compatibility.

Python 3.0 接受的功能（feature）有 ``absolute_import`` 、 ``division`` 、 ``generators`` 、 ``unicode_literals`` 、 ``print_function`` 、 ``nested_scopes`` 和 ``with_statement`` 。它们都是冗余的，因为这些功能总是打开的，这些开关只有保持兼容性。

A future statement is recognized and treated specially at compile time: Changes
to the semantics of core constructs are often implemented by generating
different code.  It may even be the case that a new feature introduces new
incompatible syntax (such as a new reserved word), in which case the compiler
may need to parse the module differently.  Such decisions cannot be pushed off
until runtime.

预览语句是在编译时识别和处理的，因为对核心构造语义的改变通常要生成不同的代码，甚至于它可能引入与现有版本语法并不兼容的新功能（例如引入新的保留字），这时编译器需要对这个模块进行不同的解析。此种操作不可能推迟到运行时。

For any given release, the compiler knows which feature names have been defined,
and raises a compile-time error if a future statement contains a feature not
known to it.

对于一个给定版本，编译器都知道一个预览功能的名字集合。如果预览语句包括了一个它所不知道的功能名字，就会导致一个编译错误。

The direct runtime semantics are the same as for any import statement: there is
a standard module :mod:`__future__`, described later, and it will be imported in
the usual way at the time the future statement is executed.

这个功能的直接运行时语义与任何导入语句相同：我们即将要介绍的标准模块 :mod:`__future__` ，它可以在执行预览语句的地方以正常方式导入。

The interesting runtime semantics depend on the specific feature enabled by the
future statement.

可用的运行时语义取决于预览语句打开了哪些特定功能。

Note that there is nothing special about the statement:

注意以下语句没有任何特殊之处::

   import __future__ [as name]

That is not a future statement; it's an ordinary import statement with no
special semantics or syntax restrictions.

这并不是一条预览语句。这是一条普通导入语句没有任何特殊语义或者语法限制。

Code compiled by calls to the built-in functions :func:`exec` and :func:`compile`
that occur in a module :mod:`M` containing a future statement will, by default,
use the new syntax or semantics associated with the future statement.  This can
be controlled by optional arguments to :func:`compile` --- see the documentation
of that function for details.

在包括预览语句的模块 :mod:`M` 内调用内置函数 :func:`exec` 和 :func:`compile` 编译出的代码，默认情况下都会使用相应预览语句中开启的新语法和语义。这个功能可以用函数 :func:`compile` 的一个可选参数控制。

A future statement typed at an interactive interpreter prompt will take effect
for the rest of the interpreter session.  If an interpreter is started with the
:option:`-i` option, is passed a script name to execute, and the script includes
a future statement, it will be in effect in the interactive session started
after the script is executed.

在交互式解释器中键入的预览语句会影响其余整个解释器会话过程。如果解释器是带 :option:`-i` 选项启动的，而它传递了一个要执行的脚本名。如果这个脚本包括有预览语句，它也会影响这个脚本执行完之后的交互式解释会话。

.. seealso::

   :pep:`236` - Back to the __future__
      The original proposal for the __future__ mechanism.

      __future__机制的原始提案。        

.. _global:

 :keyword:`global` 语句
===============================

.. index::
   statement: global
   triple: global; name; binding

.. productionlist::
   global_stmt: "global" `identifier` ("," `identifier`)*

The :keyword:`global` statement is a declaration which holds for the entire
current code block.  It means that the listed identifiers are to be interpreted
as globals.  It would be impossible to assign to a global variable without
:keyword:`global`, although free variables may refer to globals without being
declared global.

:keyword:`global` 语句是对整个代码块都有作用的一个声明。它指出其所列的标识符要解释为全局的。不用 :keyword:`global` 声明，也可能赋值给全局变量，这时自由变量就引用了一个没有声明的全局变量。

Names listed in a :keyword:`global` statement must not be used in the same code
block textually preceding that :keyword:`global` statement.

在 :keyword:`global` 中出现的名字不能在 :keyword:`global` 之前的代码中使用。

Names listed in a :keyword:`global` statement must not be defined as formal
parameters or in a :keyword:`for` loop control target, :keyword:`class`
definition, function definition, or :keyword:`import` statement.

在 :keyword:`global` 中出现的名字不能作为形参，不能作为 :keyword:`for` 的循环控制目标对象，也不能在 :keyword:`class` 定义、函数定义、 :keyword:`import` 语句中出现。

.. impl-detail::

   The current implementation does not enforce the latter two restrictions, but
   programs should not abuse this freedom, as future implementations may enforce
   them or silently change the meaning of the program.

   当前实现并不强制执行后两种限制，但程序不能滥用这种自由，以后的实现可能会对其强行限制，或者默默地改变程序行为。

.. index::
   builtin: exec
   builtin: eval
   builtin: compile

**Programmer's note:** the :keyword:`global` is a directive to the parser.  It
applies only to code parsed at the same time as the :keyword:`global` statement.
In particular, a :keyword:`global` statement contained in a string or code
object supplied to the built-in :func:`exec` function does not affect the code
block *containing* the function call, and code contained in such a string is
unaffected by :keyword:`global` statements in the code containing the function
call.  The same applies to the :func:`eval` and :func:`compile` functions.

**程序员注意:** :keyword:`global` 是一个解析器的指示字。它只对与 :keyword:`global` 一起解析的代码有效。特别地， :keyword:`exec` 语句中的 :keyword:`global` 语句不会对包括有该 :keyword:`exec` 语句的代码块产生影响，反之亦然，包括有 :keyword:`global` 语句的函数也不会其所包括的 :keyword:`exec` 语句中的代码产生影响。相同的机制也应用于 :func:`eval` 和 :func:`compile` 函数。

.. _nonlocal:

 :keyword:`nonlocal` 语句
=================================

.. index:: statement: nonlocal

.. productionlist::
   nonlocal_stmt: "nonlocal" `identifier` ("," `identifier`)*

.. XXX add when implemented
                : ["=" (`target_list` "=")+ expression_list]
                : | "nonlocal" identifier augop expression_list

The :keyword:`nonlocal` statement causes the listed identifiers to refer to
previously bound variables in the nearest enclosing scope.  This is important
because the default behavior for binding is to search the local namespace
first.  The statement allows encapsulated code to rebind variables outside of
the local scope besides the global (module) scope.

:keyword:`nonlocal` 语句指出它之后所列标识符绑定的变量位于最近的上层封闭作用域中。这个功能之所以重要，因为绑定的默认行为先从局部名字空间搜索。这个语句允许封装的代码也可以将变量在全局作用域之外的其他局部作用域中绑定。

.. XXX not implemented
   The :keyword:`nonlocal` statement may prepend an assignment or augmented
   assignment, but not an expression.

Names listed in a :keyword:`nonlocal` statement, unlike to those listed in a
:keyword:`global` statement, must refer to pre-existing bindings in an
enclosing scope (the scope in which a new binding should be created cannot
be determined unambiguously).

在 :keyword:`nonlocal` 语句中列出的名字，不像 :keyword:`global` 语句的标识符列表，它们必须是上层作用域中已经存在的绑定（做不到无歧义地判断会创建新绑定的作用域）。

Names listed in a :keyword:`nonlocal` statement must not collide with
pre-existing bindings in the local scope.

:keyword:`nonlocal` 语句中的名字列表不能与局部作用域中的名字发生冲突。

.. seealso::

   :pep:`3104` - Access to Names in Outer Scopes
      The specification for the :keyword:`nonlocal` statement.

      :keyword:`nonlocal` 语句的规范。

.. rubric:: Footnotes

.. [#] It may occur within an :keyword:`except` or :keyword:`else` clause.  The
   restriction on occurring in the :keyword:`try` clause is implementor's
   laziness and will eventually be lifted.
