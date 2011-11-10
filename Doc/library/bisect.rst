:mod:`bisect` --- 数组平分规则
===========================================

.. module:: bisect
   :synopsis: Array bisection algorithms for binary searching.
.. sectionauthor:: Fred L. Drake, Jr. <fdrake@acm.org>
.. sectionauthor:: Raymond Hettinger <python at rcn.com>
.. example based on the PyModules FAQ entry by Aaron Watters <arw@pythonpros.com>

**Source code:** :source:`Lib/bisect.py`

**源代码 :  ** Lib/bisect.py

--------------

This module provides support for maintaining a list in sorted order without
having to sort the list after each insertion.  For long lists of items with
expensive comparison operations, this can be an improvement over the more common
approach.  The module is called :mod:`bisect` because it uses a basic bisection
algorithm to do its work.  The source code may be most useful as a working
example of the algorithm (the boundary conditions are already right!).

此模块提供支持维护排序顺序列表, 而不必在每次插入后进行排序列表. 对于长列表项目
频繁的比较操作, 这是一个明显超越常规方式的提高. 该模块被称为"bisect", 以为它使
用的基本平分算法(basic bisection algorithm)做其工作. 源代码可作为最有用的算
法工作示例(边界条件已经准备好了! ). 


The following functions are provided:

提供以下函数: 


.. function:: bisect_left(a, x, lo=0, hi=len(a))

   Locate the insertion point for *x* in *a* to maintain sorted order.
   The parameters *lo* and *hi* may be used to specify a subset of the list
   which should be considered; by default the entire list is used.  If *x* is
   already present in *a*, the insertion point will be before (to the left of)
   any existing entries.  The return value is suitable for use as the first
   parameter to ``list.insert()`` assuming that *a* is already sorted.


   bisect.bisect_left(a, x, lo=0, hi=len(a))

   在*a*中找到插入点*x*, 并保持排列顺序. 参数*lo*和*hi*用来指定范围内的列表子
   集, 以整个列表的默认排序. 如果*x*就是现在的*a*, 插入点就在整个列表的前面(整
   个列表的最左侧). 假设*a*已经被排序, 返回的值是作为``list.insert()``的第一
   个参数是可以使用的. 



   The returned insertion point *i* partitions the array *a* into two halves so
   that ``all(val < x for val in a[lo:i])`` for the left side and
   ``all(val >= x for val in a[i:hi])`` for the right side.

   返回的插入点*i*将数组*a*分割为两部分, ``all(var < x for val in a[lo:i])``
   作为左侧部分, ``all(val >= x for val in a[i:hi]`` 作为右侧部分. 



.. function:: bisect_right(a, x, lo=0, hi=len(a))
              bisect(a, x, lo=0, hi=len(a))

   Similar to :func:`bisect_left`, but returns an insertion point which comes
   after (to the right of) any existing entries of *x* in *a*.


bisect.bisect_right(a, x, lo=0, hi=len(a))
bisect.bisect(a, x, lo=0, hi=len(a))

   与``bisect_left()``, 但是返回是插入点为*x*在*a*中的整个右侧部分


   The returned insertion point *i* partitions the array *a* into two halves so
   that ``all(val <= x for val in a[lo:i])`` for the left side and
   ``all(val > x for val in a[i:hi])`` for the right side.

   返回的插入点*i*将数组*a*分割为两部分, ``all(var <=x for val in a[lo:i])``
   作为左侧部分, ``all(val > x for val in a[i:hi]`` 作为右侧部分. 



.. function:: insort_left(a, x, lo=0, hi=len(a))

   Insert *x* in *a* in sorted order.  This is equivalent to
   ``a.insert(bisect.bisect_left(a, x, lo, hi), x)`` assuming that *a* is
   already sorted.  Keep in mind that the O(log n) search is dominated by
   the slow O(n) insertion step.

.. function:: insort_right(a, x, lo=0, hi=len(a))
              insort(a, x, lo=0, hi=len(a))

   将*x*以规则顺序插入*a*中. 假设*a*已经被排序, 这是与``a.insert(bisect.bi
   sect_left(a, x, lo, hi), x)``等价的. 请注意, O(log n)的查找是受控于O(n)
   的插入步骤. 

   Similar to :func:`insort_left`, but inserting *x* in *a* after any existing
   entries of *x*.

   与``insort_left()``类似, 但是在*a*中插入的*x*在整个*a*之后. (原文为
   *x*之后)


.. see also::

   `SortedCollection recipe
   <http://code.activestate.com/recipes/577197-sortedcollection/>`_ that uses
   bisect to build a full-featured collection class with straight-forward search
   methods and support for a key-function.  The keys are precomputed to save
   unnecessary calls to the key function during searches.

   相关项目:

   SortedCollection的秘诀是用bisect建立一个全功能的搜集类, 该类包括直接的搜
   索方法和对一个关键函数的支持. 其中关键在于在搜索过程中, 可以节省不必要的对关
   键函数的调用. 


搜索排序的列表
--------------


The above :func:`bisect` functions are useful for finding insertion points but
can be tricky or awkward to use for common searching tasks. The following five
functions show how to transform them into the standard lookups for sorted
lists::

上述的``bisect()``函数对于查找插入点会非常有用, 但对于普通的查找任务, 该函数
变得要么棘手要么笨拙. 下列5个函数展示了怎样将他们转化到对排序列表的标准查找. 

    def index(a, x):
        'Locate the leftmost value exactly equal to x'
        i = bisect_left(a, x)
        if i != len(a) and a[i] == x:
            return i
        raise ValueError

    def find_lt(a, x):
        'Find rightmost value less than x'
        i = bisect_left(a, x)
        if i:
            return a[i-1]
        raise ValueError

    def find_le(a, x):
        'Find rightmost value less than or equal to x'
        i = bisect_right(a, x)
        if i:
            return a[i-1]
        raise ValueError

    def find_gt(a, x):
        'Find leftmost value greater than x'
        i = bisect_right(a, x)
        if i != len(a):
            return a[i]
        raise ValueError

    def find_ge(a, x):
        'Find leftmost item greater than or equal to x'
        i = bisect_left(a, x)
        if i != len(a):
            return a[i]
        raise ValueError


其他例子
--------------

.. _bisect-example:

The :func:`bisect` function can be useful for numeric table lookups. This
example uses :func:`bisect` to look up a letter grade for an exam score (say)
based on a set of ordered numeric breakpoints: 90 and up is an 'A', 80 to 89 is
a 'B', and so on::

   ``bisect()``函数对于数字类型的表格查找非常有用. 这个例子使用``bisect()``函数
来查找一个类似于examscore(say)函数的字母等级, 该函数基于一个数字类型的断点: 90
或更高为'A', 80到89之间为'B', 等等: 

   >>> def grade(score, breakpoints=[60, 70, 80, 90], grades='FDCBA'):
   ...     i = bisect(breakpoints, score)
   ...     return grades[i]
   ...
   >>> [grade(score) for score in [33, 99, 77, 70, 89, 90, 100]]
   ['F', 'A', 'C', 'C', 'B', 'A', 'A']

Unlike the :func:`sorted` function, it does not make sense for the :func:`bisect`
functions to have *key* or *reversed* arguments because that would lead to an
inefficient design (successive calls to bisect functions would not "remember"
all of the previous key lookups).

不同于``sorted()``函数, 它不会使 ``bisect()``函数有*key*还是*reversed*的
争论变得毫无意义, 因为那会导致一个效率不高的设计(成功的对bisect函数的调用不会
" 记住 "所有之前的键查找). 

Instead, it is better to search a list of precomputed keys to find the index
of the record in question::

相反, 它是利用列表中预先计算的键值去查找问题中的记录索引: 


    >>> data = [('red', 5), ('blue', 1), ('yellow', 8), ('black', 0)]
    >>> data.sort(key=lambda r: r[1])
    >>> keys = [r[1] for r in data]         # precomputed list of keys
    >>> data[bisect_left(keys, 0)]
    ('black', 0)
    >>> data[bisect_left(keys, 1)]
    ('blue', 1)
    >>> data[bisect_left(keys, 5)]
    ('red', 5)
    >>> data[bisect_left(keys, 8)]
    ('yellow', 8)


