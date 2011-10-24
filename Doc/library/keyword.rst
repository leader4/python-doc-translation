:mod:`keyword` --- Python关键字测试
==============================================

.. module:: keyword
   :synopsis: Test whether a string is a keyword in Python.


**Source code:** :source:`Lib/keyword.py`

**源代码:** Lib/keyword.py



This module allows a Python program to determine if a string is a keyword.

这个模块可以让一个Python程序去判定一个字符串是否是保留关键字.

.. function:: iskeyword(s)

   Return true if *s* is a Python keyword.

   返回True 如果*s*是一个Python关键字


.. data:: kwlist

   Sequence containing all the keywords defined for the interpreter.  If any
   keywords are defined to only be active when particular :mod:`__future__`
   statements are in effect, these will be included as well.

   列表包含了解释器定义的所有关键字.若在``__future__``语句生效时任何被定义
   的关键字也会被包含进去.

