:mod:`glob` --- Unix风格的路径名模式扩展
=====================================================

.. module:: glob
   :synopsis: Unix shell style pathname pattern expansion.


.. index:: single: filenames; pathname expansion

**Source code:** :source:`Lib/glob.py`

--------------

The :mod:`glob` module finds all the pathnames matching a specified pattern
according to the rules used by the Unix shell.  No tilde expansion is done, but
``*``, ``?``, and character ranges expressed with ``[]`` will be correctly
matched.  This is done by using the :func:`os.listdir` and
:func:`fnmatch.fnmatch` functions in concert, and not by actually invoking a
subshell.  (For tilde and shell variable expansion, use
:func:`os.path.expanduser` and :func:`os.path.expandvars`.)


.. function:: glob(pathname)

   Return a possibly-empty list of path names that match *pathname*, which must be
   a string containing a path specification. *pathname* can be either absolute
   (like :file:`/usr/src/Python-1.5/Makefile`) or relative (like
   :file:`../../Tools/\*/\*.gif`), and can contain shell-style wildcards. Broken
   symlinks are included in the results (as in the shell).
   
   返回所有匹配的文件路径列表. 它只有一个参数pathname,定义了文件路径匹配规则,这里可以是绝对路径,也可以是相对路径. 


.. function:: iglob(pathname)

   Return an :term:`iterator` which yields the same values as :func:`glob`
   without actually storing them all simultaneously.
   
   获取一个可编历对象,使用它可以逐个获取匹配的文件路径名. 与glob.glob()的区别是: 
   glob.glob同时获取所有的匹配路径,而glob.iglob一次只获取一个匹配路径. 这有点类似于.NET中操作数据库用到的DataSet与DataReader. 


For example, consider a directory containing only the following files:
:file:`1.gif`, :file:`2.txt`, and :file:`card.gif`.  :func:`glob` will produce
the following results.  Notice how any leading components of the path are
preserved. ::

   >>> import glob
   >>> glob.glob('./[0-9].*')
   ['./1.gif', './2.txt']
   >>> glob.glob('*.gif')
   ['1.gif', 'card.gif']
   >>> glob.glob('?.gif')
   ['1.gif']


.. seealso::

   Module :mod:`fnmatch`
      Shell-style filename (not path) expansion


