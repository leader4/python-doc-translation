.. _library-index:

###############################
  The Python Standard Library Python 标准库
###############################

:Release: |version|
:Date: |today|

While :ref:`reference-index` describes the exact syntax and
semantics of the Python language, this library reference manual
describes the standard library that is distributed with Python. It also
describes some of the optional components that are commonly included
in Python distributions.

尽管 :ref:`reference-index` 中说明了 Python 语言明确的语法和语义, 
不过此库参考手册说明了随 Python 一起发布的标准库.
它还说明了一些通常包含在 Python 发布中的可选组件.

Python's standard library is very extensive, offering a wide range of
facilities as indicated by the long table of contents listed below. The
library contains built-in modules (written in C) that provide access to
system functionality such as file I/O that would otherwise be
inaccessible to Python programmers, as well as modules written in Python
that provide standardized solutions for many problems that occur in
everyday programming. Some of these modules are explicitly designed to
encourage and enhance the portability of Python programs by abstracting
away platform-specifics into platform-neutral APIs.

Python 标准库涉及十分广泛, 提供了由后面的长目录列表中表示的极大范围的灵活度.
这个库包含用其他方式难以实现的访问类似文件 I/O 的系统功能的内置模块 (使用 C 编写),
同时包含用 Python 编写的提供在日常编程中遇到的许多问题的标准解决方法的模块.
其中一些模块明确设计用于通过抽象平台相关的技术为平台独立的 API 以鼓励和增强 Python 程序的可移植性.

The Python installers for the Windows platform usually includes
the entire standard library and often also include many additional
components. For Unix-like operating systems Python is normally provided
as a collection of packages, so it may be necessary to use the packaging
tools provided with the operating system to obtain some or all of the
optional components.

Windows 平台的 Python 安装程序通常包含完整的标准库, 也常常包含许多额外的组件.
在类 Unix 操作系统中 Python 一般作为包集合的形式提供, 所以可能需要使用操作系统中的打包工具来获取一些或所有的可选组件.

In addition to the standard library, there is a growing collection of
several thousand components (from individual programs and modules to
packages and entire application development frameworks), available from
the `Python Package Index <http://pypi.python.org/pypi>`_.

除了标准库, 还有一个由几千个组件 (来自于包中的个别程序和模块以及完整的应用程序开发框架) 组成的不断增长的集合, 
可从 `Python Package Index <http://pypi.python.org/pypi>`_ 获得.

.. toctree::
   :maxdepth: 2
   :numbered:

   intro.rst
   functions.rst
   constants.rst
   stdtypes.rst
   exceptions.rst

   strings.rst
   datatypes.rst
   numeric.rst
   functional.rst
   filesys.rst
   persistence.rst
   archiving.rst
   fileformats.rst
   crypto.rst
   allos.rst
   someos.rst
   ipc.rst
   netdata.rst
   markup.rst
   internet.rst
   mm.rst
   i18n.rst
   frameworks.rst
   tk.rst
   development.rst
   debug.rst
   python.rst
   custominterp.rst
   modules.rst
   language.rst
   misc.rst
   windows.rst
   unix.rst
   undoc.rst
