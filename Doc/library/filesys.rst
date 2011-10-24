.. _filesys:

*************************
文件和文件夹访问 
*************************

The modules described in this chapter deal with disk files and directories.  For
example, there are modules for reading the properties of files, manipulating
paths in a portable way, and creating temporary files.  The full list of modules
in this chapter is:

本章描述的这个模块用来处理磁盘文件和文件夹.
举例来说, 有的模块可以读取文件的属性, 合适的方法操纵文件路径, 创建临时文件.
这章中介绍的完整模块列表如下:



.. toctree::

   os.path.rst
   fileinput.rst
   stat.rst
   filecmp.rst
   tempfile.rst
   glob.rst
   fnmatch.rst
   linecache.rst
   shutil.rst
   macpath.rst


.. seealso::

   Module :mod:`os`

      Operating system interfaces, including functions to work with files at a
      lower level than Python :term:`file objects <file object>`.
       操作系统接口, 包括比Python  *file objects* 工作在更底层的函数.

   Module :mod:`io`


      Python's built-in I/O library, including both abstract classes and
      some concrete classes such as file I/O.

   Python的内置 I/O 库, 包括抽象类和一些具体的类例如文件 I/O

   Built-in function :func:`open`


      The standard way to open files for reading and writing with Python.

       使用Python打开文件进行读取和写入操作的标准方法.




