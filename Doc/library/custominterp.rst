.. _custominterp:

**************************
自定义Python解释器
**************************

The modules described in this chapter allow writing interfaces similar to
Python's interactive interpreter.  If you want a Python interpreter that
supports some special feature in addition to the Python language, you should
look at the :mod:`code` module.  (The :mod:`codeop` module is lower-level, used
to support compiling a possibly-incomplete chunk of Python code.)

在本章中所描述的模块允许你写出类似的接口用来与Python解释器交互. 如果你想要一个Python解释器
支持一些特殊的功能,除了Python语言,你应该看: :mod:'code'.   (: mod: 'codeop' 模块是较低级别的,使用支持编译Python代码可能不完整的块) . 

The full list of modules described in this chapter is:


.. toctree::

   code.rst
   codeop.rst

