:mod:`distutils` --- 建立和安装Python模块 
===========================================================

.. module:: distutils
   :synopsis: Support for building and installing Python modules into an
              existing Python installation.
.. sectionauthor:: Fred L. Drake, Jr. <fdrake@acm.org>


The :mod:`distutils` package provides support for building and installing
additional modules into a Python installation.  The new modules may be either
100%-pure Python, or may be extension modules written in C, or may be
collections of Python packages which include modules coded in both Python and C.

:mod:``distutils`` 包为Python 安装额外的模块的建立和安装提供支持。新的模块可能
是纯Python,或者可能是C语言编写的扩展模块，也有可能是其中包括编码模块的Python包的集合。


这个包将在两个单独的章节讨论：


.. seealso::


   :ref:`distutils-index`
      The manual for developers and packagers of Python modules.  This describes
      how to prepare :mod:`distutils`\ -based packages so that they may be
      easily installed into an existing Python installation.

      开发和Python模块的打包手册。这介绍了如何准备"distutils"的基础包，使
      他们可以很容易的安装现有的Python安装。


   :ref:`install-index`
      An "administrators" manual which includes information on installing
      modules into an existing Python installation.  You do not need to be a
      Python programmer to read this manual.

      "管理员"手册，其中包括的信息是安装到现有的Python安装模块。你不需要
       是一个Python程序员就可以阅读本手册。

