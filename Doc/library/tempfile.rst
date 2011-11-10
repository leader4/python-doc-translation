:mod:`tempfile` --- 生成临时文件和目录
============================================================

.. sectionauthor:: Zack Weinberg <zack@codesourcery.com>


.. module:: tempfile
   :synopsis: Generate temporary files and directories.


.. index::
   pair: temporary; file name
   pair: temporary; file

**Source code:** :source:`Lib/tempfile.py`

--------------

This module generates temporary files and directories.  It works on all
supported platforms.  It provides three new functions,
:func:`NamedTemporaryFile`, :func:`mkstemp`, and :func:`mkdtemp`, which should
eliminate all remaining need to use the insecure :func:`mktemp` function.
Temporary file names created by this module no longer contain the process ID;
instead a string of six random characters is used.

Also, all the user-callable functions now take additional arguments which
allow direct control over the location and name of temporary files.  It is
no longer necessary to use the global *tempdir* and *template* variables.
To maintain backward compatibility, the argument order is somewhat odd; it
is recommended to use keyword arguments for clarity.

The module defines the following user-callable items:

.. function:: TemporaryFile(mode='w+b', buffering=None, encoding=None, newline=None, suffix='', prefix='tmp', dir=None)

   Return a :term:`file-like object` that can be used as a temporary storage area.
   The file is created using :func:`mkstemp`. It will be destroyed as soon
   as it is closed (including an implicit close when the object is garbage
   collected).  Under Unix, the directory entry for the file is removed
   immediately after the file is created.  Other platforms do not support
   this; your code should not rely on a temporary file created using this
   function having or not having a visible name in the file system.

   The *mode* parameter defaults to ``'w+b'`` so that the file created can
   be read and written without being closed.  Binary mode is used so that it
   behaves consistently on all platforms without regard for the data that is
   stored.  *buffering*, *encoding* and *newline* are interpreted as for
   :func:`open`.

   The *dir*, *prefix* and *suffix* parameters are passed to :func:`mkstemp`.

   The returned object is a true file object on POSIX platforms.  On other
   platforms, it is a file-like object whose :attr:`!file` attribute is the
   underlying true file object. This file-like object can be used in a
   :keyword:`with` statement, just like a normal file.


.. function:: NamedTemporaryFile(mode='w+b', buffering=None, encoding=None, newline=None, suffix='', prefix='tmp', dir=None, delete=True)

   This function operates exactly as :func:`TemporaryFile` does, except that
   the file is guaranteed to have a visible name in the file system (on
   Unix, the directory entry is not unlinked).  That name can be retrieved
   from the :attr:`name` member of the file object.  Whether the name can be
   used to open the file a second time, while the named temporary file is
   still open, varies across platforms (it can be so used on Unix; it cannot
   on Windows NT or later).  If *delete* is true (the default), the file is
   deleted as soon as it is closed.
   The returned object is always a file-like object whose :attr:`!file`
   attribute is the underlying true file object. This file-like object can
   be used in a :keyword:`with` statement, just like a normal file.


.. function:: SpooledTemporaryFile(max_size=0, mode='w+b', buffering=None, encoding=None, newline=None, suffix='', prefix='tmp', dir=None)

   This function operates exactly as :func:`TemporaryFile` does, except that
   data is spooled in memory until the file size exceeds *max_size*, or
   until the file's :func:`fileno` method is called, at which point the
   contents are written to disk and operation proceeds as with
   :func:`TemporaryFile`.

   The resulting file has one additional method, :func:`rollover`, which
   causes the file to roll over to an on-disk file regardless of its size.

   The returned object is a file-like object whose :attr:`_file` attribute
   is either a :class:`StringIO` object or a true file object, depending on
   whether :func:`rollover` has been called. This file-like object can be
   used in a :keyword:`with` statement, just like a normal file.


.. function:: TemporaryDirectory(suffix='', prefix='tmp', dir=None)

   This function creates a temporary directory using :func:`mkdtemp`
   (the supplied arguments are passed directly to the underlying function).
   The resulting object can be used as a context manager (see
   :ref:`context-managers`).  On completion of the context (or destruction
   of the temporary directory object), the newly created temporary directory
   and all its contents are removed from the filesystem.

   The directory name can be retrieved from the :attr:`name` member
   of the returned object.

   The directory can be explicitly cleaned up by calling the
   :func:`cleanup` method.

   .. versionadded:: 3.2


.. function:: mkstemp(suffix='', prefix='tmp', dir=None, text=False)

   Creates a temporary file in the most secure manner possible.  There are
   no race conditions in the file's creation, assuming that the platform
   properly implements the :const:`os.O_EXCL` flag for :func:`os.open`.  The
   file is readable and writable only by the creating user ID.  If the
   platform uses permission bits to indicate whether a file is executable,
   the file is executable by no one.  The file descriptor is not inherited
   by child processes.

   Unlike :func:`TemporaryFile`, the user of :func:`mkstemp` is responsible
   for deleting the temporary file when done with it.

   If *suffix* is specified, the file name will end with that suffix,
   otherwise there will be no suffix.  :func:`mkstemp` does not put a dot
   between the file name and the suffix; if you need one, put it at the
   beginning of *suffix*.

   If *prefix* is specified, the file name will begin with that prefix;
   otherwise, a default prefix is used.

   If *dir* is specified, the file will be created in that directory;
   otherwise, a default directory is used.  The default directory is chosen
   from a platform-dependent list, but the user of the application can
   control the directory location by setting the *TMPDIR*, *TEMP* or *TMP*
   environment variables.  There is thus no guarantee that the generated
   filename will have any nice properties, such as not requiring quoting
   when passed to external commands via ``os.popen()``.

   If *text* is specified, it indicates whether to open the file in binary
   mode (the default) or text mode.  On some platforms, this makes no
   difference.

   :func:`mkstemp` returns a tuple containing an OS-level handle to an open
   file (as would be returned by :func:`os.open`) and the absolute pathname
   of that file, in that order.


.. function:: mkdtemp(suffix='', prefix='tmp', dir=None)

   Creates a temporary directory in the most secure manner possible. There
   are no race conditions in the directory's creation.  The directory is
   readable, writable, and searchable only by the creating user ID.

   The user of :func:`mkdtemp` is responsible for deleting the temporary
   directory and its contents when done with it.

   The *prefix*, *suffix*, and *dir* arguments are the same as for
   :func:`mkstemp`.

   :func:`mkdtemp` returns the absolute pathname of the new directory.


.. function:: mktemp(suffix='', prefix='tmp', dir=None)

   .. deprecated:: 2.3
      Use :func:`mkstemp` instead.

   Return an absolute pathname of a file that did not exist at the time the
   call is made.  The *prefix*, *suffix*, and *dir* arguments are the same
   as for :func:`mkstemp`.

   .. warning::

      Use of this function may introduce a security hole in your program.  By
      the time you get around to doing anything with the file name it returns,
      someone else may have beaten you to the punch.  :func:`mktemp` usage can
      be replaced easily with :func:`NamedTemporaryFile`, passing it the
      ``delete=False`` parameter::

         >>> f = NamedTemporaryFile(delete=False)
         >>> f
         <open file '<fdopen>', mode 'w+b' at 0x384698>
         >>> f.name
         '/var/folders/5q/5qTPn6xq2RaWqk+1Ytw3-U+++TI/-Tmp-/tmpG7V1Y0'
         >>> f.write("Hello World!\n")
         >>> f.close()
         >>> os.unlink(f.name)
         >>> os.path.exists(f.name)
         False

The module uses two global variables that tell it how to construct a
temporary name.  They are initialized at the first call to any of the
functions above.  The caller may change them, but this is discouraged; use
the appropriate function arguments, instead.


.. data:: tempdir

   When set to a value other than ``None``, this variable defines the
   default value for the *dir* argument to all the functions defined in this
   module.

   If ``tempdir`` is unset or ``None`` at any call to any of the above
   functions, Python searches a standard list of directories and sets
   *tempdir* to the first one which the calling user can create files in.
   The list is:

   #. The directory named by the :envvar:`TMPDIR` environment variable.

   #. The directory named by the :envvar:`TEMP` environment variable.

   #. The directory named by the :envvar:`TMP` environment variable.

   #. A platform-specific location:

      * On Windows, the directories :file:`C:\\TEMP`, :file:`C:\\TMP`,
        :file:`\\TEMP`, and :file:`\\TMP`, in that order.

      * On all other platforms, the directories :file:`/tmp`, :file:`/var/tmp`, and
        :file:`/usr/tmp`, in that order.

   #. As a last resort, the current working directory.


.. function:: gettempdir()

   Return the directory currently selected to create temporary files in. If
   :data:`tempdir` is not ``None``, this simply returns its contents; otherwise,
   the search described above is performed, and the result returned.


.. function:: gettempprefix()

   Return the filename prefix used to create temporary files.  This does not
   contain the directory component.


Examples
--------

Here are some examples of typical usage of the :mod:`tempfile` module::

    >>> import tempfile

    # create a temporary file and write some data to it
    >>> fp = tempfile.TemporaryFile()
    >>> fp.write('Hello world!')
    # read data from file
    >>> fp.seek(0)
    >>> fp.read()
    'Hello world!'
    # close the file, it will be removed
    >>> fp.close()

    # create a temporary file using a context manager
    >>> with tempfile.TemporaryFile() as fp:
    ...     fp.write('Hello world!')
    ...     fp.seek(0)
    ...     fp.read()
    'Hello world!'
    >>>
    # file is now closed and removed

    # create a temporary directory using the context manager
    >>> with tempfile.TemporaryDirectory() as tmpdirname:
    ...     print 'created temporary directory', tmpdirname
    >>>
    # directory and contents have been removed
    
#################
应用程序经常要保存一些临时的信息, 这些信息不是特别重要, 没有必要写在配置文件里, 但又不能没有, 这时候就可以把这些信息写到临时文件里. 其实很多程序在运行的时候, 都会产生一大堆临时文件, 有些用于保存日志, 有些用于保存一些临时数据, 还有一些保存一些无关紧要的设置. 在windows操作系统中, 临时文件一般被保存在这个文件夹下: C:/Documents and Settings/User/Local Settings/Temp. 其实我们最常用的IE浏览器在浏览网页的时候, 会产生大量的临时文件, 这些临时文件一般是我们浏览过的网页的本地副本. Python提供了一个tempfile模块, 用来对临时数据进行操作. 查阅Python手册, 里面介绍了如下常用的方法: 
tempfile.mkstemp([suffix=''[, prefix='tmp'[, dir=None[, text=False]]]])

    mkstemp方法用于创建一个临时文件. 该方法仅仅用于创建临时文件, 调用tempfile.mkstemp函数后, 返回包含两个元素的元组, 第一个元素指示操作该临时文件的安全级别, 第二个元素指示该临时文件的路径. 参数suffix和prefix分别表示临时文件名称的后缀和前缀; dir指定了临时文件所在的目录, 如果没有指定目录, 将根据系统环境变量TMPDIR, TEMP或者TMP的设置来保存临时文件; 参数text指定了是否以文本的形式来操作文件, 默认为False, 表示以二进制的形式来操作文件. 
tempfile.mkdtemp([suffix=''[, prefix='tmp'[, dir=None]]])

    该函数用于创建一个临时文件夹. 参数的意思与tempfile.mkdtemp一样. 它返回临时文件夹的绝对路径. 
tempfile.mktemp([suffix=''[, prefix='tmp'[, dir=None]]])

    mktemp用于返回一个临时文件的路径, 但并不创建该临时文件. 
tempfile.tempdir

    该属性用于指定创建的临时文件 (夹) 所在的默认文件夹. 如果没有设置该属性或者将其设为None, Python将返回以下环境变量TMPDIR, TEMP, TEMP指定的目录, 如果没有定义这些环境变量, 临时文件将被创建在当前工作目录. 
tempfile.gettempdir()

    gettempdir()则用于返回保存临时文件的文件夹路径. 
tempfile.TemporaryFile([mode='w+b'[, bufsize=-1[, suffix=''[, prefix='tmp'[, dir=None]]]]])

    该函数返回一个 类文件 对象(file-like)用于临时数据保存 (实际上对应磁盘上的一个临时文件) . 当文件对象被close或者被del的时候, 临时文件将从磁盘上删除. mode、bufsize参数的单方与open()函数一样; suffix和prefix指定了临时文件名的后缀和前缀; dir用于设置临时文件默认的保存路径. 返回的类文件对象有一个file属性, 它指向真正操作的底层的file对象. 
tempfile.NamedTemporaryFile([mode='w+b'[, bufsize=-1[, suffix=''[, prefix='tmp'[, dir=None[, delete=True]]]]]])

    tempfile.NamedTemporaryFile函数的行为与tempfile.TemporaryFile类似, 只不过它多了一个delete参数, 用于指定类文件对象close或者被del之后, 是否也一同删除磁盘上的临时文件 (当delete = True的时候, 行为与TemporaryFile一样) . 
tempfile.SpooledTemporaryFile([max_size=0[, mode='w+b'[, bufsize=-1[, suffix=''[, prefix='tmp'[, dir=None]]]]]])

    tempfile.SpooledTemporaryFile函数的行为与tempfile.TemporaryFile类似. 不同的是向类文件对象写数据的时候, 数据长度只有到达参数max_size指定大小时, 或者调用类文件对象的fileno()方法, 数据才会真正写入到磁盘的临时文件中. 


