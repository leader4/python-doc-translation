:mod:`filecmp` --- 文件和目录比较
=================================================

.. module:: filecmp
   :synopsis: Compare files efficiently.
.. sectionauthor:: Moshe Zadka <moshez@zadka.site.co.il>

**Source code:** :source:`Lib/filecmp.py`

--------------

The :mod:`filecmp` module defines functions to compare files and directories,
with various optional time/correctness trade-offs. For comparing files,
see also the :mod:`difflib` module.

The :mod:`filecmp` module defines the following functions:


.. function:: cmp(f1, f2, shallow=True)

   Compare the files named *f1* and *f2*, returning ``True`` if they seem equal,
   ``False`` otherwise.

   Unless *shallow* is given and is false, files with identical :func:`os.stat`
   signatures are taken to be equal.

   Files that were compared using this function will not be compared again unless
   their :func:`os.stat` signature changes.

   Note that no external programs are called from this function, giving it
   portability and efficiency.


.. function:: cmpfiles(dir1, dir2, common, shallow=True)

   Compare the files in the two directories *dir1* and *dir2* whose names are
   given by *common*.

   Returns three lists of file names: *match*, *mismatch*,
   *errors*.  *match* contains the list of files that match, *mismatch* contains
   the names of those that don't, and *errors* lists the names of files which
   could not be compared.  Files are listed in *errors* if they don't exist in
   one of the directories, the user lacks permission to read them or if the
   comparison could not be done for some other reason.

   The *shallow* parameter has the same meaning and default value as for
   :func:`filecmp.cmp`.

   For example, ``cmpfiles('a', 'b', ['c', 'd/e'])`` will compare ``a/c`` with
   ``b/c`` and ``a/d/e`` with ``b/d/e``.  ``'c'`` and ``'d/e'`` will each be in
   one of the three returned lists.


Example::

   >>> import filecmp
   >>> filecmp.cmp('undoc.rst', 'undoc.rst')
   True
   >>> filecmp.cmp('undoc.rst', 'index.rst')
   False


.. _dircmp-objects:

The :class:`dircmp` class
-------------------------

:class:`dircmp` instances are built using this constructor:


.. class:: dircmp(a, b, ignore=None, hide=None)

   Construct a new directory comparison object, to compare the directories *a* and
   *b*. *ignore* is a list of names to ignore, and defaults to ``['RCS', 'CVS',
   'tags']``. *hide* is a list of names to hide, and defaults to ``[os.curdir,
   os.pardir]``.

   The :class:`dircmp` class provides the following methods:


   .. method:: report()

      Print (to ``sys.stdout``) a comparison between *a* and *b*.


   .. method:: report_partial_closure()

      Print a comparison between *a* and *b* and common immediate
      subdirectories.


   .. method:: report_full_closure()

      Print a comparison between *a* and *b* and common subdirectories
      (recursively).

   The :class:`dircmp` offers a number of interesting attributes that may be
   used to get various bits of information about the directory trees being
   compared.

   Note that via :meth:`__getattr__` hooks, all attributes are computed lazily,
   so there is no speed penalty if only those attributes which are lightweight
   to compute are used.


   .. attribute:: left_list

      Files and subdirectories in *a*, filtered by *hide* and *ignore*.


   .. attribute:: right_list

      Files and subdirectories in *b*, filtered by *hide* and *ignore*.


   .. attribute:: common

      Files and subdirectories in both *a* and *b*.


   .. attribute:: left_only

      Files and subdirectories only in *a*.


   .. attribute:: right_only

      Files and subdirectories only in *b*.


   .. attribute:: common_dirs

      Subdirectories in both *a* and *b*.


   .. attribute:: common_files

      Files in both *a* and *b*


   .. attribute:: common_funny

      Names in both *a* and *b*, such that the type differs between the
      directories, or names for which :func:`os.stat` reports an error.


   .. attribute:: same_files

      Files which are identical in both *a* and *b*.


   .. attribute:: diff_files

      Files which are in both *a* and *b*, whose contents differ.


   .. attribute:: funny_files

      Files which are in both *a* and *b*, but could not be compared.


   .. attribute:: subdirs

      A dictionary mapping names in :attr:`common_dirs` to :class:`dircmp`
      objects.
  filecmp模块用于比较文件及文件夹的内容, 它是一个轻量级的工具, 使用非常简单. python标准库还提供了difflib模块用于比较文件的内容. 关于difflib模块, 且听下回分解. 

    filecmp定义了两个函数, 用于方便地比较文件与文件夹: 

filecmp.cmp(f1, f2[, shallow]): 

    比较两个文件的内容是否匹配. 参数f1, f2指定要比较的文件的路径. 可选参数shallow指定比较文件时是否需要考虑文件本身的属性 (通过os.stat函数可以获得文件属性) . 如果文件内容匹配, 函数返回True, 否则返回False. 

filecmp.cmpfiles(dir1, dir2, common[, shallow]): 

    比较两个文件夹内指定文件是否相等. 参数dir1, dir2指定要比较的文件夹, 参数common指定要比较的文件名列表. 函数返回包含3个list元素的元组, 分别表示匹配、不匹配以及错误的文件列表. 错误的文件指的是不存在的文件, 或文件被琐定不可读, 或没权限读文件, 或者由于其他原因访问不了该文件. 

    filecmp模块中定义了一个dircmp类, 用于比较文件夹, 通过该类比较两个文件夹, 可以获取一些详细的比较结果 (如只在A文件夹存在的文件列表) , 并支持子文件夹的递归比较. 

dircmp提供了三个方法用于报告比较的结果: 

    report(): 只比较指定文件夹中的内容 (文件与文件夹) 
    report_partial_closure(): 比较文件夹及第一级子文件夹的内容
    report_full_closure(): 递归比较所有的文件夹的内容

dircmp还提供了下面这些属性用于获取比较的详细结果: 

    left_list: 左边文件夹中的文件与文件夹列表; 
    right_list: 右边文件夹中的文件与文件夹列表; 
    common: 两边文件夹中都存在的文件或文件夹; 
    left_only: 只在左边文件夹中存在的文件或文件夹; 
    right_only: 只在右边文件夹中存在的文件或文件夹; 
    common_dirs: 两边文件夹都存在的子文件夹; 
    common_files: 两边文件夹都存在的子文件; 
    common_funny: 两边文件夹都存在的子文件夹; 
    same_files: 匹配的文件; 
    diff_files: 不匹配的文件; 
    funny_files: 两边文件夹中都存在, 但无法比较的文件; 
    subdirs: 我没看明白这个属性的意思, python手册中的解释如下: A dictionary mapping names in common_dirs to dircmp objects


