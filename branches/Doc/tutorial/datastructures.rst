.. _tut-structures:

************************
Data Structures 数据结构
************************

This chapter describes some things you've learned about already in more detail,
and adds some new things as well.

本章更详细地描述一些你已经学习过的东西, 而且同样增加了一些新的东西.

.. _tut-morelists:

More on Lists 深入列表
======================

The list data type has some more methods.  Here are all of the methods of list
objects:

列表数据类型还有一些方法. 这里把它所有的方法都列了出来:


.. method:: list.append(x)
   :noindex:

   Add an item to the end of the list; equivalent to ``a[len(a):] = [x]``.
   
   在列表的尾部添加一个项; 等价于 ``a[len(a):] = [x]``.


.. method:: list.extend(L)
   :noindex:

   Extend the list by appending all the items in the given list; equivalent to
   ``a[len(a):] = L``.
   
   在该列表后添加上给出列表所有项; 等价于 ``a[len(a):] = L``.


.. method:: list.insert(i, x)
   :noindex:

   Insert an item at a given position.  The first argument is the index of the
   element before which to insert, so ``a.insert(0, x)`` inserts at the front of
   the list, and ``a.insert(len(a), x)`` is equivalent to ``a.append(x)``.
   
   在一个给出的位置插入一项. 第一个参数就是准备在它之前插入的元素的索引, 因此
   ``a.insert(0, x)`` 会在列表的头部插入, 而 ``a.insert(len(a), x)``
   则等价于 ``a.append(x)``.


.. method:: list.remove(x)
   :noindex:

   Remove the first item from the list whose value is *x*. It is an error if there
   is no such item.
   
   移除列表中第一个值为 *x* 的项. 但没有符合要求的项时, 会产生一个错误.


.. method:: list.pop([i])
   :noindex:

   Remove the item at the given position in the list, and return it.  If no index
   is specified, ``a.pop()`` removes and returns the last item in the list.  (The
   square brackets around the *i* in the method signature denote that the parameter
   is optional, not that you should type square brackets at that position.  You
   will see this notation frequently in the Python Library Reference.)
   
   删除列表给定位置的项, 并返回它. 如果没有指定索引, ``a.pop`` 移除并返回列表的最后一项.
   (函数原型中方括号中的 *i* 意味着它是一个可选参数, 而不是你应当在那里键入一个方括号.
   你将会在 Python 库参考中常常见到那种表示法.)


.. method:: list.index(x)
   :noindex:

   Return the index in the list of the first item whose value is *x*. It is an
   error if there is no such item.
   
   返回列表中第一个值为 *x* 的项. 如果没有符合要求的项, 则产生一个错误.


.. method:: list.count(x)
   :noindex:

   Return the number of times *x* appears in the list.
   
   返回列表中 *x* 出现的次数.


.. method:: list.sort()
   :noindex:

   Sort the items of the list, in place.
   
   原地地为这个列表排序.


.. method:: list.reverse()
   :noindex:

   Reverse the elements of the list, in place.
   
   原地地将列表中的元素翻转.

An example that uses most of the list methods::

   >>> a = [66.25, 333, 333, 1, 1234.5]
   >>> print(a.count(333), a.count(66.25), a.count('x'))
   2 1 0
   >>> a.insert(2, -1)
   >>> a.append(333)
   >>> a
   [66.25, 333, -1, 333, 1, 1234.5, 333]
   >>> a.index(333)
   1
   >>> a.remove(333)
   >>> a
   [66.25, -1, 333, 1, 1234.5, 333]
   >>> a.reverse()
   >>> a
   [333, 1234.5, 1, 333, -1, 66.25]
   >>> a.sort()
   >>> a
   [-1, 1, 66.25, 333, 333, 1234.5]

一个使用大多列表方法的例子::

   >>> a = [66.25, 333, 333, 1, 1234.5]
   >>> print(a.count(333), a.count(66.25), a.count('x'))
   2 1 0
   >>> a.insert(2, -1)
   >>> a.append(333)
   >>> a
   [66.25, 333, -1, 333, 1, 1234.5, 333]
   >>> a.index(333)
   1
   >>> a.remove(333)
   >>> a
   [66.25, -1, 333, 1, 1234.5, 333]
   >>> a.reverse()
   >>> a
   [333, 1234.5, 1, 333, -1, 66.25]
   >>> a.sort()
   >>> a
   [-1, 1, 66.25, 333, 333, 1234.5]


.. _tut-lists-as-stacks:

Using Lists as Stacks 把列表当成栈使用
--------------------------------------

.. sectionauthor:: Ka-Ping Yee <ping@lfw.org>


The list methods make it very easy to use a list as a stack, where the last
element added is the first element retrieved ("last-in, first-out").  To add an
item to the top of the stack, use :meth:`append`.  To retrieve an item from the
top of the stack, use :meth:`pop` without an explicit index.  For example::

   >>> stack = [3, 4, 5]
   >>> stack.append(6)
   >>> stack.append(7)
   >>> stack
   [3, 4, 5, 6, 7]
   >>> stack.pop()
   7
   >>> stack
   [3, 4, 5, 6]
   >>> stack.pop()
   6
   >>> stack.pop()
   5
   >>> stack
   [3, 4]

由于列表的方法, 使得把列表当成栈使用十分简单. 栈的特性是最后添加的元素就是第一个取出的元素
("后进先出"). 要在栈顶添加一个项, 就使用 :meth:`append`. 要从栈顶取回一个项, 
就不带显式索引地使用 :meth:`pop`. 例如::

   >>> stack = [3, 4, 5]
   >>> stack.append(6)
   >>> stack.append(7)
   >>> stack
   [3, 4, 5, 6, 7]
   >>> stack.pop()
   7
   >>> stack
   [3, 4, 5, 6]
   >>> stack.pop()
   6
   >>> stack.pop()
   5
   >>> stack
   [3, 4]


.. _tut-lists-as-queues:

Using Lists as Queues 把列表当队列使用
--------------------------------------

.. sectionauthor:: Ka-Ping Yee <ping@lfw.org>

It is also possible to use a list as a queue, where the first element added is
the first element retrieved ("first-in, first-out"); however, lists are not
efficient for this purpose.  While appends and pops from the end of list are
fast, doing inserts or pops from the beginning of a list is slow (because all
of the other elements have to be shifted by one).

也可以把列表当成队列使用, 队列的特性是第一个添加的元素就是第一个取回的元素
("先进先出"); 然而, 在这里列表是低效的. 从列表的最后添加和弹出是很快的,
而在列表的开头插入或弹出是慢的 (因为所有其它元素得移动一个位置).

To implement a queue, use :class:`collections.deque` which was designed to
have fast appends and pops from both ends.  For example::

   >>> from collections import deque
   >>> queue = deque(["Eric", "John", "Michael"])
   >>> queue.append("Terry")           # Terry arrives
   >>> queue.append("Graham")          # Graham arrives
   >>> queue.popleft()                 # The first to arrive now leaves
   'Eric'
   >>> queue.popleft()                 # The second to arrive now leaves
   'John'
   >>> queue                           # Remaining queue in order of arrival
   deque(['Michael', 'Terry', 'Graham'])

要实现一个队列, 使用 :class:`collection.deque`, 它设计得在两端添加和弹出都很快.
例如::

   >>> from collections import deque
   >>> queue = deque(["Eric", "John", "Michael"])
   >>> queue.append("Terry")           # Terry 进入
   >>> queue.append("Graham")          # Graham 进入
   >>> queue.popleft()                 # 第一个进入的现在离开
   'Eric'
   >>> queue.popleft()                 # 第二个进入的现在离开
   'John'
   >>> queue                           # 剩余的队列, 它按照进入的顺序排列
   deque(['Michael', 'Terry', 'Graham'])


.. _tut-listcomps:

List Comprehensions 列表解析
----------------------------

List comprehensions provide a concise way to create lists from sequences.
Common applications are to make lists where each element is the result of
some operations applied to each member of the sequence, or to create a
subsequence of those elements that satisfy a certain condition.

列表解析提供了一种从序列中创建列表的简明的方法. 
通常的程序会对一个序列的每个元素做些操作以生成列表, 
或者生成序列中满足某个条件的元素的子序列.

A list comprehension consists of brackets containing an expression followed
by a :keyword:`for` clause, then zero or more :keyword:`for` or :keyword:`if`
clauses.  The result will be a list resulting from evaluating the expression in
the context of the :keyword:`for` and :keyword:`if` clauses which follow it.  If
the expression would evaluate to a tuple, it must be parenthesized.

列表解析的结构是, 在一个括号里, 首先是一个表达式, 随后是一个 :keyword:`for` 子句,
然后是零个或更多的 :keyword:`for` 或 :keyword:`if` 子句.
结果将是一个列表, 该列表通过计算随后的 :keyword:`for` 和 :keyword:`if` 子句来获得.
如果要使表达是计算为一个元组, 那么它必须时用圆括号.

Here we take a list of numbers and return a list of three times each number::

   >>> vec = [2, 4, 6]
   >>> [3*x for x in vec]
   [6, 12, 18]

在这里, 我们从一个数字列表返回一个新的列表, 新列表的每一项是原来的三倍::

   >>> vec = [2, 4, 6]
   >>> [3*x for x in vec]
   [6, 12, 18]

Now we get a little fancier::

   >>> [[x, x**2] for x in vec]
   [[2, 4], [4, 16], [6, 36]]

现在是一个有点奇特的::

   >>> [[x, x**2] for x in vec]
   [[2, 4], [4, 16], [6, 36]]

Here we apply a method call to each item in a sequence::

   >>> freshfruit = ['  banana', '  loganberry ', 'passion fruit  ']
   >>> [weapon.strip() for weapon in freshfruit]
   ['banana', 'loganberry', 'passion fruit']

在这里, 我们对序列中的每一项调用了一个方法::

   >>> freshfruit = ['  banana', '  loganberry ', 'passion fruit  ']
   >>> [weapon.strip() for weapon in freshfruit]
   ['banana', 'loganberry', 'passion fruit']

Using the :keyword:`if` clause we can filter the stream::

   >>> [3*x for x in vec if x > 3]
   [12, 18]
   >>> [3*x for x in vec if x < 2]
   []

使用 :keyword:`if` 子句可以过滤::

   >>> [3*x for x in vec if x > 3]
   [12, 18]
   >>> [3*x for x in vec if x < 2]
   []

Tuples can often be created without their parentheses, but not here::

   >>> [x, x**2 for x in vec]  # error - parens required for tuples
     File "<stdin>", line 1, in ?
       [x, x**2 for x in vec]
                  ^
   SyntaxError: invalid syntax
   >>> [(x, x**2) for x in vec]
   [(2, 4), (4, 16), (6, 36)]

元组经常能不使用圆括号创建, 但在这里不行::

   >>> [x, x**2 for x in vec]  # error - parens required for tuples
     File "<stdin>", line 1, in ?
       [x, x**2 for x in vec]
                  ^
   SyntaxError: invalid syntax
   >>> [(x, x**2) for x in vec]
   [(2, 4), (4, 16), (6, 36)]

Here are some nested for loops and other fancy behavior::

   >>> vec1 = [2, 4, 6]
   >>> vec2 = [4, 3, -9]
   >>> [x*y for x in vec1 for y in vec2]
   [8, 6, -18, 16, 12, -36, 24, 18, -54]
   >>> [x+y for x in vec1 for y in vec2]
   [6, 5, -7, 8, 7, -5, 10, 9, -3]
   >>> [vec1[i]*vec2[i] for i in range(len(vec1))]
   [8, 12, -54]

这里是一些嵌套的循环和其它的奇特的行为::

   >>> vec1 = [2, 4, 6]
   >>> vec2 = [4, 3, -9]
   >>> [x*y for x in vec1 for y in vec2]
   [8, 6, -18, 16, 12, -36, 24, 18, -54]
   >>> [x+y for x in vec1 for y in vec2]
   [6, 5, -7, 8, 7, -5, 10, 9, -3]
   >>> [vec1[i]*vec2[i] for i in range(len(vec1))]
   [8, 12, -54]

List comprehensions can be applied to complex expressions and nested functions::

   >>> [str(round(355/113, i)) for i in range(1, 6)]
   ['3.1', '3.14', '3.142', '3.1416', '3.14159']

列表解析可用于复杂的表达式和嵌套的函数::

   >>> [str(round(355/113, i)) for i in range(1, 6)]
   ['3.1', '3.14', '3.142', '3.1416', '3.14159']


Nested List Comprehensions 嵌套列表解析
---------------------------------------

If you've got the stomach for it, list comprehensions can be nested. They are a
powerful tool but -- like all powerful tools -- they need to be used carefully,
if at all.

如果你可以忍受的话, 其实列表解析是可以被嵌套的. 它们是个强大的工具, 但 --
就像所有强大的工具一样 -- 需要被小心地使用, 

Consider the following example of a 3x3 matrix held as a list containing three
lists, one list per row::

    >>> mat = [
    ...        [1, 2, 3],
    ...        [4, 5, 6],
    ...        [7, 8, 9],
    ...       ]

考虑下面的例子, 有一个 3x3 的矩阵, 存储为一个包含三个列表的列表, 每一行一个列表::

    >>> mat = [
    ...        [1, 2, 3],
    ...        [4, 5, 6],
    ...        [7, 8, 9],
    ...       ]

Now, if you wanted to swap rows and columns, you could use a list
comprehension::

    >>> print([[row[i] for row in mat] for i in [0, 1, 2]])
    [[1, 4, 7], [2, 5, 8], [3, 6, 9]]

现在, 如果你想交换行和列, 可以使用列表解析::

    >>> print([[row[i] for row in mat] for i in [0, 1, 2]])
    [[1, 4, 7], [2, 5, 8], [3, 6, 9]]

Special care has to be taken for the *nested* list comprehension:

    To avoid apprehension when nesting list comprehensions, read from right to
    left.

使用*嵌套*列表解析时特别需要注意:

    从右至左地阅读嵌套列表解析能更好理解.

A more verbose version of this snippet shows the flow explicitly::

    for i in [0, 1, 2]:
        for row in mat:
            print(row[i], end="")
        print()

一个该代码片段的冗长版本, 它显式地告诉了流程::

    for i in [0, 1, 2]:
        for row in mat:
            print(row[i], end="")
        print()

In real world, you should prefer built-in functions to complex flow statements.
The :func:`zip` function would do a great job for this use case::

    >>> list(zip(*mat))
    [(1, 4, 7), (2, 5, 8), (3, 6, 9)]

在现实中, 你应当选择内建函数来处理复杂流程语句. 在这里例子里, :func:`zip` 函数非常好用.

    >>> list(zip(*mat))
    [(1, 4, 7), (2, 5, 8), (3, 6, 9)]

See :ref:`tut-unpacking-arguments` for details on the asterisk in this line.

参见 :ref:`tut-unpacking-arguments` 了解本行中星号的详细内容.

.. _tut-del:

The :keyword:`del` statement :keyword:`del` 语句
================================================

There is a way to remove an item from a list given its index instead of its
value: the :keyword:`del` statement.  This differs from the :meth:`pop` method
which returns a value.  The :keyword:`del` statement can also be used to remove
slices from a list or clear the entire list (which we did earlier by assignment
of an empty list to the slice).  For example::

   >>> a = [-1, 1, 66.25, 333, 333, 1234.5]
   >>> del a[0]
   >>> a
   [1, 66.25, 333, 333, 1234.5]
   >>> del a[2:4]
   >>> a
   [1, 66.25, 1234.5]
   >>> del a[:]
   >>> a
   []

这有一种通过给出索引, 而不是值, 删除列表中项的方法: 使用 :keyword:`del` 语句.
它与返回一个值的 :meth:`pop` 方法不同. :keyword:`del` 语句也可以移除列表中的切片,
或者清除整个列表 (之前我们通过给切片赋值为空列表来完成这点). 例如::

   >>> a = [-1, 1, 66.25, 333, 333, 1234.5]
   >>> del a[0]
   >>> a
   [1, 66.25, 333, 333, 1234.5]
   >>> del a[2:4]
   >>> a
   [1, 66.25, 1234.5]
   >>> del a[:]
   >>> a
   []

:keyword:`del` can also be used to delete entire variables::

   >>> del a

:keyword:`del` 也可以用于删除整个变量::

   >>> del a

Referencing the name ``a`` hereafter is an error (at least until another value
is assigned to it).  We'll find other uses for :keyword:`del` later.

在这之后引用 ``a`` 的话会产生一个错误 (至少到给它赋另一个值之前). 后面我们将找到
:keyword:`del` 的其它用法.


.. _tut-tuples:

Tuples and Sequences 元组和序列
===============================

We saw that lists and strings have many common properties, such as indexing and
slicing operations.  They are two examples of *sequence* data types (see
:ref:`typesseq`).  Since Python is an evolving language, other sequence data
types may be added.  There is also another standard sequence data type: the
*tuple*.

我们看到了列表和字符串有很多通用的属性, 例如索引和切片操作.
它们是*序列*数据类型的两个例子 (参看 :ref:`typesseq`).
因为 Python 是一门进化中的语言, 其它的序列类型有可能会被加入.
这还有另一种标准序列数据类型: *元组*.

A tuple consists of a number of values separated by commas, for instance::

   >>> t = 12345, 54321, 'hello!'
   >>> t[0]
   12345
   >>> t
   (12345, 54321, 'hello!')
   >>> # Tuples may be nested:
   ... u = t, (1, 2, 3, 4, 5)
   >>> u
   ((12345, 54321, 'hello!'), (1, 2, 3, 4, 5))

元组由逗号分开的值组成, 例如::

   >>> t = 12345, 54321, 'hello!'
   >>> t[0]
   12345
   >>> t
   (12345, 54321, 'hello!')
   >>> # Tuples may be nested:
   ... u = t, (1, 2, 3, 4, 5)
   >>> u
   ((12345, 54321, 'hello!'), (1, 2, 3, 4, 5))

As you see, on output tuples are always enclosed in parentheses, so that nested
tuples are interpreted correctly; they may be input with or without surrounding
parentheses, although often parentheses are necessary anyway (if the tuple is
part of a larger expression).

如你所看到的, 输出的元组都使用圆括号包围, 那样, 嵌套的元组就可以被正确地解释;
它们在输入时可以加上或不加上周围的圆括号, 尽管经常圆括号是必要的 
(如果元组是一个更大的表达式的一部分).

Tuples have many uses.  For example: (x, y) coordinate pairs, employee records
from a database, etc.  Tuples, like strings, are immutable: it is not possible
to assign to the individual items of a tuple (you can simulate much of the same
effect with slicing and concatenation, though).  It is also possible to create
tuples which contain mutable objects, such as lists.

元组有许多用途. 例如: (x, y) 坐标对, 数据库里的员工记录等. 语句, 与字符串一样,
是不可变的: 无法对元组个别的项赋值 (但你可以使用切片和连接来模拟这个操作).
元组中可以包含可变的对象, 如列表.

A special problem is the construction of tuples containing 0 or 1 items: the
syntax has some extra quirks to accommodate these.  Empty tuples are constructed
by an empty pair of parentheses; a tuple with one item is constructed by
following a value with a comma (it is not sufficient to enclose a single value
in parentheses). Ugly, but effective.  For example::

   >>> empty = ()
   >>> singleton = 'hello',    # <-- note trailing comma
   >>> len(empty)
   0
   >>> len(singleton)
   1
   >>> singleton
   ('hello',)

当元组由 0 或 1 个项构造的时候有一个特殊的问题: 语法为了适应它们而有一些额外的规则.
空元组由一对空的圆括号构造; 一个项的元组由一个值后面跟着一个逗号构造
(把一个值放入一对圆括号里并不足以构造一个元组). 丑陋但有效. 例如::

   >>> empty = ()
   >>> singleton = 'hello',    # <-- 注意后面的逗号
   >>> len(empty)
   0
   >>> len(singleton)
   1
   >>> singleton
   ('hello',)

The statement ``t = 12345, 54321, 'hello!'`` is an example of *tuple packing*:
the values ``12345``, ``54321`` and ``'hello!'`` are packed together in a tuple.
The reverse operation is also possible::

   >>> x, y, z = t

语句 ``t = 12345, 54321, 'hello!'`` 是*元组打包*的一个例子:
值 ``12345``, ``54321`` 和 ``'hello!'`` 被打包进一个元组.
反过来, 这个操作也是可行的::

   >>> x, y, z = t

This is called, appropriately enough, *sequence unpacking* and works for any
sequence on the right-hand side.  Sequence unpacking requires that there are as
many variables on the left side of the equals sign as there are elements in the
sequence.  Note that multiple assignment is really just a combination of tuple
packing and sequence unpacking.

这个操作被合适地称为*序列解包*, 它对右边的序列进行操作. 
序列解包需要等号左边的值的个数与右边序列元素的个数相等.
注意, 多重赋值其实是联合使用了元组打包和序列解包.

.. XXX Add a bit on the difference between tuples and lists.


.. _tut-sets:

Sets 集合
=========

Python also includes a data type for *sets*.  A set is an unordered collection
with no duplicate elements.  Basic uses include membership testing and
eliminating duplicate entries.  Set objects also support mathematical operations
like union, intersection, difference, and symmetric difference.

Python 也有一个 *集合* 数据类型. 集合是一个没有重复元素的没有顺序的容器.
基本用途包括成员关系测试和消除重复条目. 集合对象也支持数学操作, 如联合,
相交, 不同, 和对称不同.

Curly braces or the :func:`set` function can be used to create sets.  Note: To
create an empty set you have to use ``set()``, not ``{}``; the latter creates an
empty dictionary, a data structure that we discuss in the next section.

花括号或函数 :func:`set` 可用于创建集合. 注意: 创建一个空集合只能使用
``set()``, 而不能使用 ``{}``; 后者是创建一个空字典, 字典我们会在下一节里讨论.

Here is a brief demonstration::

   >>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
   >>> print(basket)                      # show that duplicates have been removed
   {'orange', 'banana', 'pear', 'apple'}
   >>> 'orange' in basket                 # fast membership testing
   True
   >>> 'crabgrass' in basket
   False

   >>> # Demonstrate set operations on unique letters from two words
   ...
   >>> a = set('abracadabra')
   >>> b = set('alacazam')
   >>> a                                  # unique letters in a
   {'a', 'r', 'b', 'c', 'd'}
   >>> a - b                              # letters in a but not in b
   {'r', 'd', 'b'}
   >>> a | b                              # letters in either a or b
   {'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
   >>> a & b                              # letters in both a and b
   {'a', 'c'}
   >>> a ^ b                              # letters in a or b but not both
   {'r', 'd', 'b', 'm', 'z', 'l'}

这是一个简明的示范:

   >>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
   >>> print(basket)                      # 重复的被移除了
   {'orange', 'banana', 'pear', 'apple'}
   >>> 'orange' in basket                 # 快速成员关系测试
   True
   >>> 'crabgrass' in basket
   False

   >>> # 在两个单词的不重复的字母里演示集合操作
   ...
   >>> a = set('abracadabra')
   >>> b = set('alacazam')
   >>> a                                  # a 中的不重复字母
   {'a', 'r', 'b', 'c', 'd'}
   >>> a - b                              # a 中有而 b 中没有的字母
   {'r', 'd', 'b'}
   >>> a | b                              # 在 a 中或在 b 中的字母
   {'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
   >>> a & b                              # a 和 b 中都有的字母
   {'a', 'c'}
   >>> a ^ b                              # a 或 b 中只有一个有的字母
   {'r', 'd', 'b', 'm', 'z', 'l'}

Like :ref:`for lists <tut-listcomps>`, there is a set comprehension syntax::

   >>> a = {x for x in 'abracadabra' if x not in 'abc'}
   >>> a
   {'r', 'd'}

就像 :ref:`for lists <tut-listcomps>`, 也有一个集合解析的语法::

   >>> a = {x for x in 'abracadabra' if x not in 'abc'}
   >>> a
   {'r', 'd'}



.. _tut-dictionaries:

Dictionaries 字典
=================

Another useful data type built into Python is the *dictionary* (see
:ref:`typesmapping`). Dictionaries are sometimes found in other languages as
"associative memories" or "associative arrays".  Unlike sequences, which are
indexed by a range of numbers, dictionaries are indexed by *keys*, which can be
any immutable type; strings and numbers can always be keys.  Tuples can be used
as keys if they contain only strings, numbers, or tuples; if a tuple contains
any mutable object either directly or indirectly, it cannot be used as a key.
You can't use lists as keys, since lists can be modified in place using index
assignments, slice assignments, or methods like :meth:`append` and
:meth:`extend`.

Python 中另一个有用的内建数据类型为 *字典* (参看 :ref:`typesmapping`).
其它语言中字典一般被叫做 "关联存储" 或 "关联数组". 与使用某个范围作为索引的序列不一样,
字典通过*键*索引, 键可以是任意不可变类型; 字符串和数字总是可以作为键.
当元组只包含字符串, 数字, 或元组时也可以作为键; 当元组直接或间接地包含可变对象时,
它就不能用作一个键. 不能使用列表作为键, 因为列表可以通过索引, 切片, 或如
:meth:`append` 和 :meth:`extend` 方法原地赋值而被改变.

It is best to think of a dictionary as an unordered set of *key: value* pairs,
with the requirement that the keys are unique (within one dictionary). A pair of
braces creates an empty dictionary: ``{}``. Placing a comma-separated list of
key:value pairs within the braces adds initial key:value pairs to the
dictionary; this is also the way dictionaries are written on output.

最好把字典看成是一个没有顺序的*键: 值*对的集合, 键必须是唯一的 (在一个字典里).
一对花括号创建一个空字典: ``{}``. 
在括号中间放置的用逗号分隔的键:值对列表就是字典的初始键:值对.
这也是字典打印的方式.

The main operations on a dictionary are storing a value with some key and
extracting the value given the key.  It is also possible to delete a key:value
pair with ``del``. If you store using a key that is already in use, the old
value associated with that key is forgotten.  It is an error to extract a value
using a non-existent key.

字典最主要的操作是通过某键存储一个值, 和从给定的键里提取它的值.
使用 ``del`` 可以删除一个键:值对. 如果你使用一个已被使用的键进行存储操作, 
该键的旧值就没有了. 使用一个不存在的键提取它的值会产生一个错误.

Performing ``list(d.keys())`` on a dictionary returns a list of all the keys
used in the dictionary, in arbitrary order (if you want it sorted, just use
``sorted(d.keys())`` instead). [#]_  To check whether a single key is in the
dictionary, use the :keyword:`in` keyword.

在一个字典上执行 ``list(d.keys())`` 返回该字典中所使用键的列表, 以任意的顺序
(如果你想让它排序, 使用 ``sorted(d.keys())``). [#]_ 要检查某一个键是否在字典里,
使用 :keyword:`in` 关键字.

Here is a small example using a dictionary::

   >>> tel = {'jack': 4098, 'sape': 4139}
   >>> tel['guido'] = 4127
   >>> tel
   {'sape': 4139, 'guido': 4127, 'jack': 4098}
   >>> tel['jack']
   4098
   >>> del tel['sape']
   >>> tel['irv'] = 4127
   >>> tel
   {'guido': 4127, 'irv': 4127, 'jack': 4098}
   >>> list(tel.keys())
   ['irv', 'guido', 'jack']
   >>> sorted(tel.keys())
   ['guido', 'irv', 'jack']
   >>> 'guido' in tel
   True
   >>> 'jack' not in tel
   False

这是一个使用字典的小例子::

   >>> tel = {'jack': 4098, 'sape': 4139}
   >>> tel['guido'] = 4127
   >>> tel
   {'sape': 4139, 'guido': 4127, 'jack': 4098}
   >>> tel['jack']
   4098
   >>> del tel['sape']
   >>> tel['irv'] = 4127
   >>> tel
   {'guido': 4127, 'irv': 4127, 'jack': 4098}
   >>> list(tel.keys())
   ['irv', 'guido', 'jack']
   >>> sorted(tel.keys())
   ['guido', 'irv', 'jack']
   >>> 'guido' in tel
   True
   >>> 'jack' not in tel
   False

The :func:`dict` constructor builds dictionaries directly from sequences of
key-value pairs::

   >>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
   {'sape': 4139, 'jack': 4098, 'guido': 4127}

构造器 :func:`dict` 从键-值对序列里直接生成字典::

   >>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
   {'sape': 4139, 'jack': 4098, 'guido': 4127}

In addition, dict comprehensions can be used to create dictionaries from
arbitrary key and value expressions::

   >>> {x: x**2 for x in (2, 4, 6)}
   {2: 4, 4: 16, 6: 36}

额外的, 字典解析可以用来从任意键和值的表达式中创建字典::

   >>> {x: x**2 for x in (2, 4, 6)}
   {2: 4, 4: 16, 6: 36}

When the keys are simple strings, it is sometimes easier to specify pairs using
keyword arguments::

   >>> dict(sape=4139, guido=4127, jack=4098)
   {'sape': 4139, 'jack': 4098, 'guido': 4127}

如键为字符串, 使用关键字参数指定键-值对有时更为简单::

   >>> dict(sape=4139, guido=4127, jack=4098)
   {'sape': 4139, 'jack': 4098, 'guido': 4127}


.. _tut-loopidioms:

Looping Techniques 循环技巧
===========================

When looping through dictionaries, the key and corresponding value can be
retrieved at the same time using the :meth:`items` method. ::

   >>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
   >>> for k, v in knights.items():
   ...     print(k, v)
   ...
   gallahad the pure
   robin the brave

当对字典循环时, 可以使用 :meth:`items` 方法同时取回键和对应的值. ::

   >>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
   >>> for k, v in knights.items():
   ...     print(k, v)
   ...
   gallahad the pure
   robin the brave

When looping through a sequence, the position index and corresponding value can
be retrieved at the same time using the :func:`enumerate` function. ::

   >>> for i, v in enumerate(['tic', 'tac', 'toe']):
   ...     print(i, v)
   ...
   0 tic
   1 tac
   2 toe

对一个序列循环时, 可以使用 :func:`enumerate` 函数来同时取回位置索引和相应的值.

   >>> for i, v in enumerate(['tic', 'tac', 'toe']):
   ...     print(i, v)
   ...
   0 tic
   1 tac
   2 toe

To loop over two or more sequences at the same time, the entries can be paired
with the :func:`zip` function. ::

   >>> questions = ['name', 'quest', 'favorite color']
   >>> answers = ['lancelot', 'the holy grail', 'blue']
   >>> for q, a in zip(questions, answers):
   ...     print('What is your {0}?  It is {1}.'.format(q, a))
   ...
   What is your name?  It is lancelot.
   What is your quest?  It is the holy grail.
   What is your favorite color?  It is blue.

同时对两个或更多的序列进行循环, 可以使用 :func:`zip` 函数来把整个最成对. ::

   >>> questions = ['name', 'quest', 'favorite color']
   >>> answers = ['lancelot', 'the holy grail', 'blue']
   >>> for q, a in zip(questions, answers):
   ...     print('What is your {0}?  It is {1}.'.format(q, a))
   ...
   What is your name?  It is lancelot.
   What is your quest?  It is the holy grail.
   What is your favorite color?  It is blue.

To loop over a sequence in reverse, first specify the sequence in a forward
direction and then call the :func:`reversed` function. ::

   >>> for i in reversed(range(1, 10, 2)):
   ...     print(i)
   ...
   9
   7
   5
   3
   1

反向地对一个序列循环, 首先指定一个正序的序列, 然后调用 :func:`reversed` 函数. ::

   >>> for i in reversed(range(1, 10, 2)):
   ...     print(i)
   ...
   9
   7
   5
   3
   1

To loop over a sequence in sorted order, use the :func:`sorted` function which
returns a new sorted list while leaving the source unaltered. ::

   >>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
   >>> for f in sorted(set(basket)):
   ...     print(f)
   ...
   apple
   banana
   orange
   pear

有序地对一个序列进行循环, 使用 :func:`sorted` 函数, 
它能返回一个新的排序后的列表而保持源数据不变. ::

   >>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
   >>> for f in sorted(set(basket)):
   ...     print(f)
   ...
   apple
   banana
   orange
   pear


.. _tut-conditions:

More on Conditions 深入条件
===========================

The conditions used in ``while`` and ``if`` statements can contain any
operators, not just comparisons.

在 ``while`` 和 ``if`` 语句中使用的条件可以包含任何操作符, 而不仅仅是比较.

The comparison operators ``in`` and ``not in`` check whether a value occurs
(does not occur) in a sequence.  The operators ``is`` and ``is not`` compare
whether two objects are really the same object; this only matters for mutable
objects like lists.  All comparison operators have the same priority, which is
lower than that of all numerical operators.

比较操作符 ``in`` 和 ``not in`` 检查一个值在 (不在) 一个序列里. 操作符 ``is``
和 ``is not`` 比较两个对象是否为同一对象; 这个只对可变对象, 如列表, 要紧.
所有比较操作符有相同的优先级, 比所有数字操作符的优先级都低.

Comparisons can be chained.  For example, ``a < b == c`` tests whether ``a`` is
less than ``b`` and moreover ``b`` equals ``c``.

操作符可以连起来使用. 例如, ``a < b == c`` 测试 ``a`` 小于 ``b`` 
而且 ``b`` 与 ``c`` 相等.

Comparisons may be combined using the Boolean operators ``and`` and ``or``, and
the outcome of a comparison (or of any other Boolean expression) may be negated
with ``not``.  These have lower priorities than comparison operators; between
them, ``not`` has the highest priority and ``or`` the lowest, so that ``A and
not B or C`` is equivalent to ``(A and (not B)) or C``. As always, parentheses
can be used to express the desired composition.

比较式可以使用布尔操作符 ``and`` 和 ``or`` 连接, 可以使用 ``not`` 把比较式
(或其它的布尔表达式) 的结果变为相反. 它们比比较操作符的优先级更低; 在它们之间,
``not`` 优先级最高, 而 ``or`` 的优先级最低, 因此 ``A and not B or C``
等价与 ``(A and (not B)) or C``. 同样, 可以使用圆括号来表达想要的结果.

The Boolean operators ``and`` and ``or`` are so-called *short-circuit*
operators: their arguments are evaluated from left to right, and evaluation
stops as soon as the outcome is determined.  For example, if ``A`` and ``C`` are
true but ``B`` is false, ``A and B and C`` does not evaluate the expression
``C``.  When used as a general value and not as a Boolean, the return value of a
short-circuit operator is the last evaluated argument.

布尔操作符 ``and`` 和 ``or`` 被称为*短路*操作符: 它从左至右计算参数,
并且当结果确定时计算就立即停止. 例如, 如果 ``A`` 和 ``C`` 为真, 而 ``B`` 为假时,
``A and B and C`` 不会计算表达式 ``C``. 
当把短路操作符的返回值作为一个常规值而不是布尔值时, 它的值就是最后计算的参数值.

It is possible to assign the result of a comparison or other Boolean expression
to a variable.  For example, ::

   >>> string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
   >>> non_null = string1 or string2 or string3
   >>> non_null
   'Trondheim'

可以把比较式或其它布尔表达式的值赋给一个变量. 例如, ::

   >>> string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
   >>> non_null = string1 or string2 or string3
   >>> non_null
   'Trondheim'

Note that in Python, unlike C, assignment cannot occur inside expressions. C
programmers may grumble about this, but it avoids a common class of problems
encountered in C programs: typing ``=`` in an expression when ``==`` was
intended.

注意, 在 Python 中, 不像 C, 赋值不可以发生在表达式内部. C 程序员可能对此抱怨,
但是这避免里 C 程序中的一类常见错误: 在有意使用 ``==`` 的表达式里键入了 ``=``.


.. _tut-comparing:

Comparing Sequences and Other Types 比较序列和其它类型
======================================================

Sequence objects may be compared to other objects with the same sequence type.
The comparison uses *lexicographical* ordering: first the first two items are
compared, and if they differ this determines the outcome of the comparison; if
they are equal, the next two items are compared, and so on, until either
sequence is exhausted. If two items to be compared are themselves sequences of
the same type, the lexicographical comparison is carried out recursively.  If
all items of two sequences compare equal, the sequences are considered equal.
If one sequence is an initial sub-sequence of the other, the shorter sequence is
the smaller (lesser) one.  Lexicographical ordering for strings uses the Unicode
codepoint number to order individual characters.  Some examples of comparisons
between sequences of the same type::

   (1, 2, 3)              < (1, 2, 4)
   [1, 2, 3]              < [1, 2, 4]
   'ABC' < 'C' < 'Pascal' < 'Python'
   (1, 2, 3, 4)           < (1, 2, 4)
   (1, 2)                 < (1, 2, -1)
   (1, 2, 3)             == (1.0, 2.0, 3.0)
   (1, 2, ('aa', 'ab'))   < (1, 2, ('abc', 'a'), 4)

序列对象可以与同一类型的其它对象比较. 该比较使用*字典编纂*顺序:
首先比较头两项, 如果它们不同, 它们的比较就决定整个比较的结果;
如果它们相同, 就比较下两项, 就这样直到其中有序列被比较完了.
如果要被比较的两项本身就是相同类型的序列, 那么就递归进行比较.
如果两个序列所有的项都相等, 那么, 它们就相等. 如果一个是另一个的初始子序列,
那么短的就是较小的. 字符串的字典编纂顺序有单个字符的 Unicode 字码来决定.
以下是比较相同类型序列的例子:

   (1, 2, 3)              < (1, 2, 4)
   [1, 2, 3]              < [1, 2, 4]
   'ABC' < 'C' < 'Pascal' < 'Python'
   (1, 2, 3, 4)           < (1, 2, 4)
   (1, 2)                 < (1, 2, -1)
   (1, 2, 3)             == (1.0, 2.0, 3.0)
   (1, 2, ('aa', 'ab'))   < (1, 2, ('abc', 'a'), 4)

Note that comparing objects of different types with ``<`` or ``>`` is legal
provided that the objects have appropriate comparison methods.  For example,
mixed numeric types are compared according to their numeric value, so 0 equals
0.0, etc.  Otherwise, rather than providing an arbitrary ordering, the
interpreter will raise a :exc:`TypeError` exception.

注意, 使用 ``<`` 或 ``>`` 比较两个不同类型的对象有时候是合法的,
条件是它们要有合适的比较方法. 例如, 不同的数字类型可以按照它们的数字大小来比较,
因此 0 等于 0.0, 等等. 否则, 解释器不会提供一个任意的顺序, 而会抛出一个 :exc:`TypeError`
异常.


.. rubric:: Footnotes

.. [#] Calling ``d.keys()`` will return a :dfn:`dictionary view` object.  It
       supports operations like membership test and iteration, but its contents
       are not independent of the original dictionary -- it is only a *view*.

.. [#] 调用 ``d.keys()`` 将返回一个 :dfn:`dictionary view` 对象. 
       它支持类似成员关系测试以及迭代操作, 但是它的内容不是独立于原始字典的 -- 它只是一个*视图*.
