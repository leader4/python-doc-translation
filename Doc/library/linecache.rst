:mod:`linecache` --- 检索访问文本行
================================================

.. module:: linecache
   :synopsis: This module provides random access to individual lines from text files.
.. sectionauthor:: Moshe Zadka <moshez@zadka.site.co.il>

**Source code:** :source:`Lib/linecache.py`

--------------

The :mod:`linecache` module allows one to get any line from any file, while
attempting to optimize internally, using a cache, the common case where many
lines are read from a single file.  This is used by the :mod:`traceback` module
to retrieve source lines for inclusion in  the formatted traceback.

``linecache``模块允许从任何文件提取任何行,同时使用缓存尝试内部优化,普通情况
下多行文本是从单一文件中读取的. 它是利用``traceback``模块恢复那些收入在格式化
回滚中的源文本行. 

The :mod:`linecache` module defines the following functions:

``linecache``模块定义了下列函数: 


.. function:: getline(filename, lineno, module_globals=None)

   Get line *lineno* from file named *filename*. This function will never raise an
   exception --- it will return ``''`` on errors (the terminating newline character
   will be included for lines that are found).

   从名字为*filename*的文件中得到文本行的*lineno*. 这个函数不会出现一个例外
   --- 它会返回``''``表示错误(行终止符也被包括在被发现的行文本里). 



   .. index:: triple: module; search; path

   If a file named *filename* is not found, the function will look for it in the
   module search path, ``sys.path``, after first checking for a :pep:`302`
   ``__loader__`` in *module_globals*, in case the module was imported from a
   zipfile or other non-filesystem import source.

   如果名字为*filename*的文件没有被找到,在*module_globals*中第一次检查
   **PEP 302**``__loader__** 之后,这个函数会在模块搜索路径``sys.path``
   中查找,以防模块被从压缩文件中或其他非文件系统导入源导入. 


.. function:: clearcache()

   Clear the cache.  Use this function if you no longer need lines from files
   previously read using :func:`getline`.

   清除缓存. 如果你不再需要之前用``getline()``从文件中读取的文本行,就使用这个
   函数


.. function:: checkcache(filename=None)

   Check the cache for validity.  Use this function if files in the cache  may have
   changed on disk, and you require the updated version.  If *filename* is omitted,
   it will check all the entries in the cache.

    检查缓存的正确性. 如果缓存中的文件在磁盘上被改变时,同时你需要更新的文件,就
   使用此函数. 如果*filename*被省略了,它会检查在缓存中的所有项. 



Example::

   >>> import linecache
   >>> linecache.getline('/etc/passwd', 4)
   'sys:x:3:3:sys:/dev:/bin/sh\n'


