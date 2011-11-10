:mod:`__future__` --- future语句定义
==================================================

.. module:: __future__
   :synopsis: Future statement definitions

**Source code:** :source:`Lib/__future__.py`

--------------

:mod:`__future__` is a real module, and serves three purposes:

它是一个真正的模块,有三方面目的:

* To avoid confusing existing tools that analyze import statements and expect to
  find the modules they're importing.

  为了避免混淆已存在的的分析导入语句的工具,并
  找到那些正要被导入的模块. 

* To ensure that :ref:`future statements <future>` run under releases prior to
  2.1 at least yield runtime exceptions (the import of :mod:`__future__` will
  fail, because there was no module of that name prior to 2.1).

  为了确保未来的语句**根据版本2.1之前运行
  至少在运行时产生异常 (``的``__future__进口将
  失败,因为没有这个名称预先到2.1) 模块. 

* To document when incompatible changes were introduced, and when they will be
  --- or were --- made mandatory.  This is a form of executable documentation, and
  can be inspected programmatically via importing :mod:`__future__` and examining
  its contents.

   当文件不兼容的更改进行了介绍,当他们
  会------或者是强制性的. 这是一种形式
  可执行文件,并且可以通过编程方式检查
  ````__future__进口和审查其内容. 

Each statement in :file:`__future__.py` is of the form::

   FeatureName = _Feature(OptionalRelease, MandatoryRelease,
                          CompilerFlag)


where, normally, *OptionalRelease* is less than *MandatoryRelease*, and both are
5-tuples of the same form as ``sys.version_info``::

其中,正常情况下,OptionalRelease*** MandatoryRelease*比少,
又都是5作为```` sys.version_info元组相同的形式::

   (PY_MAJOR_VERSION, # the 2 in 2.1.0a3; an int
    PY_MINOR_VERSION, # the 1; an int
    PY_MICRO_VERSION, # the 0; an int
    PY_RELEASE_LEVEL, # "alpha", "beta", "candidate" or "final"; string
    PY_RELEASE_SERIAL # the 3; an int
   )

*OptionalRelease* records the first release in which the feature was accepted.


* OptionalRelease*记录的第一个版本中,该功能接受. 

In the case of a *MandatoryRelease* that has not yet occurred,
*MandatoryRelease* predicts the release in which the feature will become part of
the language.

在一个* MandatoryRelease*,尚未发生的情况下,
* MandatoryRelease*预测,释放该功能将成为语言的一部分. 

Else *MandatoryRelease* records when the feature became part of the language; in
releases at or after that, modules no longer need a future statement to use the
feature in question, but may continue to use such imports.

否则* MandatoryRelease*记录,当该功能成为部分
语言,在时或之后的版本中,模块不再需要
未来的声明中使用问题的功能,但可继续使用这种进口. 

*MandatoryRelease* may also be ``None``, meaning that a planned feature got
dropped.

* MandatoryRelease*也可以````没有,这意味着计划功能有下降.

Instances of class :class:`_Feature` have two corresponding methods,
:meth:`getOptionalRelease` and :meth:`getMandatoryRelease`.

*CompilerFlag* is the (bitfield) flag that should be passed in the fourth
argument to the built-in function :func:`compile` to enable the feature in
dynamically compiled code.  This flag is stored in the :attr:`compiler_flag`
attribute on :class:`_Feature` instances.


* CompilerFlag*是 (位域) 标志应在传递
第四个参数的内置函数``编译 () ``使
功能在动态编译的代码. 这个标志是储存在```` compiler_flag属性的````_Feature实例. 

No feature description will ever be deleted from :mod:`__future__`. Since its
introduction in Python 2.1 the following features have found their way into the
language using this mechanism:


特征描述将永远不被删除````__future__. 由于
在Python 2.1中引入了以下功能已经找到了方法将使用这个机制的语言: 

+------------------+-------------+--------------+---------------------------------------------+
| feature          | optional in | mandatory in | effect                                      |
+==================+=============+==============+=============================================+
| nested_scopes    | 2.1.0b1     | 2.2          | :pep:`227`:                                 |
|                  |             |              | *Statically Nested Scopes*                  |
+------------------+-------------+--------------+---------------------------------------------+
| generators       | 2.2.0a1     | 2.3          | :pep:`255`:                                 |
|                  |             |              | *Simple Generators*                         |
+------------------+-------------+--------------+---------------------------------------------+
| division         | 2.2.0a2     | 3.0          | :pep:`238`:                                 |
|                  |             |              | *Changing the Division Operator*            |
+------------------+-------------+--------------+---------------------------------------------+
| absolute_import  | 2.5.0a1     | 2.7          | :pep:`328`:                                 |
|                  |             |              | *Imports: Multi-Line and Absolute/Relative* |
+------------------+-------------+--------------+---------------------------------------------+
| with_statement   | 2.5.0a1     | 2.6          | :pep:`343`:                                 |
|                  |             |              | *The "with" Statement*                      |
+------------------+-------------+--------------+---------------------------------------------+
| print_function   | 2.6.0a2     | 3.0          | :pep:`3105`:                                |
|                  |             |              | *Make print a function*                     |
+------------------+-------------+--------------+---------------------------------------------+
| unicode_literals | 2.6.0a2     | 3.0          | :pep:`3112`:                                |
|                  |             |              | *Bytes literals in Python 3000*             |
+------------------+-------------+--------------+---------------------------------------------+


.. seealso::

   :ref:`future`
      How the compiler treats future imports.

