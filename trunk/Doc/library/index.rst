.. _library-index:

###############################
  Python标准库
###############################

:Release: |version|
:Date: |today|

虽然Python的语言参考指南描述了Python语言的确切语法和语义，但这个库参考手册介绍了大量的Python的标准库。它还介绍了一些可选组件，通常包含在Python发行版本中。

Python拥有一个强大的标准库,提供了众多下面列出的内容表示的功能。该库包含内置在模块（用C语言编写），提供系统功能，如文件的访问的I/O的Python程序员，
以及用Python写的解决许多问题的模块，提供标准化的解决方案，否则将无法访问日常编程。这些模块是明确的，旨在鼓励和加强Python程序的可移植性，并完全抽象成平台无关的统一API。Python语言的核心只包含数字、字符串、列表、字典、文件等常见类型和函数，而由Python标准库提供了系统管理、网络通信、文本处理、数据库接口、图形系统、XML处理等额外的功能。Python标准库命名接口清晰、文档良好，很容易学习和使用。

Python社区提供了大量的第三方模块，使用方式与标准库类似。它们的功能无所不包，覆盖科学计算、Web开发、数据库接口、图形系统多个领域，并且大多成熟而稳定。第三方模块可以使用Python或者C语言编写。SWIG,SIP常用于将C语言编写的程序库转化为Python模块。Boost C++ Libraries 包含了一组库，Boost.Python，使得以 Python 或 C++ 编写的编程能互相调用。借助于拥有基于标准库的大量工具、能够使用低级语言如C和可以作为其他库接口的C++，Python已成为一种强大的应用于其他语言与工具之间的胶水语言。


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
