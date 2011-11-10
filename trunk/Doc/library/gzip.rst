:mod:`gzip` --- 解压缩
=================================================

.. module:: gzip
   :synopsis: Interfaces for gzip compression and decompression using file objects.

**Source code:** :source:`Lib/gzip.py`

--------------

This module provides a simple interface to compress and decompress files just
like the GNU programs :program:`gzip` and :program:`gunzip` would.

这个模块提供了一个类似 GNU 程序 (gzip,gunzip)的压缩和解压缩的简单接口. 

The data compression is provided by the :mod:`zlib` module.

数据压缩由``zlib``模块提供. 


The :mod:`gzip` module provides the :class:`GzipFile` class. The :class:`GzipFile`
class reads and writes :program:`gzip`\ -format files, automatically compressing
or decompressing the data so that it looks like an ordinary :term:`file object`.

'gzip'模块提供``GzipFile``类. 
``GzipFile``类读写 **gzip**格式的文件,
就像普通的*file object*一样自动压缩和解压缩数据. 


Note that additional file formats which can be decompressed by the
:program:`gzip` and :program:`gunzip` programs, such  as those produced by
:program:`compress` and :program:`pack`, are not supported by this module.

注意: 
那些**gzip** & **gunzip**追加的可解压文件格式是不可以使用这个模块. 
如: **compress**  (精简) & **pack** (包) . 


For other archive formats, see the :mod:`bz2`, :mod:`zipfile`, and
:mod:`tarfile` modules.

对于其他的格式,请参阅 ``bz2``, ``zipfile``, &``tarfile``模块


The module defines the following items:

模块定义了如下规则: 


.. class:: GzipFile(filename=None, mode=None, compresslevel=9, fileobj=None, mtime=None)

   Constructor for the :class:`GzipFile` class, which simulates most of the
   methods of a :term:`file object`, with the exception of the :meth:`truncate`
   method.  At least one of *fileobj* and *filename* must be given a non-trivial
   value.

   ``GzipFile``类的构造器,模拟除了``truncate()``以外的大部分的
   *file object*函数. *fileobj* & *filename*中至少有一个必须给于值. 


   The new class instance is based on *fileobj*, which can be a regular file, a
   :class:`StringIO` object, or any other object which simulates a file.  It
   defaults to ``None``, in which case *filename* is opened to provide a file
   object.

    新的类的实例建立在*fileobj*基础之上. 它可以是一个普通文档,或者是
   一个``StringIO``对象,又或者是其它的模拟文档对象. 
   而在当*filename*被作为一个文件对象打开时,*fileobj*的模认值为``None``. 


   When *fileobj* is not ``None``, the *filename* argument is only used to be
   included in the :program:`gzip` file header, which may includes the original
   filename of the uncompressed file.  It defaults to the filename of *fileobj*, if
   discernible; otherwise, it defaults to the empty string, and in this case the
   original filename is not included in the header.

   当*fileobj*值不为``None``时,
   *filename*参数只能在含有未解压文档原文件名的**gzip**的文件名中被保含. 
   它如果可以识别,那么默认为*fileobj*的文件名; 
   否则它默认为空字符串,在这种情况下,原文件名不包含在标题里. 


   The *mode* argument can be any of ``'r'``, ``'rb'``, ``'a'``, ``'ab'``, ``'w'``,
   or ``'wb'``, depending on whether the file will be read or written.  The default
   is the mode of *fileobj* if discernible; otherwise, the default is ``'rb'``. If
   not given, the 'b' flag will be added to the mode to ensure the file is opened
   in binary mode for cross-platform portability.

   *mode*参数可以是``'r'``, ``'rb'``, ``'a'``, ``'ab'``, ``'w'``, or
   `'wb'``的任意一个,取决于文件是否可以被读取. 如果 *fileobj*可
  被识别,它默认为 *fileobj*的模式; 否则,默认为``'rb'``. 如果
  没有被给出,在模式里会添加`b`以保证文件可以在跨平台时以二进
  制模式打开. 


   The *compresslevel* argument is an integer from ``1`` to ``9`` controlling the
   level of compression; ``1`` is fastest and produces the least compression, and
   ``9`` is slowest and produces the most compression.  The default is ``9``.

   *compresslevel*参数是一个表示压缩等级的整数,由``1``到``9``来分别表示
   ``1``为最快最少的压缩比率,而``9``则是最慢最大的压缩比率. 默认为``9``


   The *mtime* argument is an optional numeric timestamp to be written to
   the stream when compressing.  All :program:`gzip` compressed streams are
   required to contain a timestamp.  If omitted or ``None``, the current
   time is used.  This module ignores the timestamp when decompressing;
   however, some programs, such as :program:`gunzip`\ , make use of it.
   The format of the timestamp is the same as that of the return value of
   ``time.time()`` and of the ``st_mtime`` member of the object returned
   by ``os.stat()``.

   *mtime*参数是可选项,当压缩完成后可将数字时间戳写在数据流里. 
   所有的**gzip**压缩流都被要求包含时间戳. 如果缺少或者为``None``,
   则使用即时时间. 当解压时,这个模块将忽略时间戳; 但是,诸如
   **gunzip**类的程序依然在使用它. 时间戳的格式如``time.time()``的返回值,
   及由``os.stat()``返回的对象中的``st_mtime``成员. 


   Calling a :class:`GzipFile` object's :meth:`close` method does not close
   *fileobj*, since you might wish to append more material after the compressed
   data.  This also allows you to pass a :class:`io.BytesIO` object opened for
   writing as *fileobj*, and retrieve the resulting memory buffer using the
   :class:`io.BytesIO` object's :meth:`~io.BytesIO.getvalue` method.

    调用 ``GzipFile``对象中的``close()``函数无法关闭*fileobj*,
   因为你也许希望在压缩的数据后加入更多的内容. 这可以允许你通过一个
   ``io.BytesIO``项目像*fileobj*一样打开写入,然后使用``io.BytesIO``对象
   中的``getvalue()``函数检索内存缓冲结果. 


   :class:`GzipFile` supports the :class:`io.BufferedIOBase` interface,
   including iteration and the :keyword:`with` statement.  Only the
   :meth:`truncate` method isn't implemented.

   ``GzipFile``支持``io.BufferedIOBase``接口. 包括交互式图标
   和``with``声明. 只有``read1()``和 ``truncate()``不被执行. 


   :class:`GzipFile` also provides the following method:

   ``GzipFile``也提供了如下方法: 


   .. method:: peek([n])

      Read *n* uncompressed bytes without advancing the file position.
      At most one single read on the compressed stream is done to satisfy
      the call.  The number of bytes returned may be more or less than
      requested.

      读取*n*未压缩字节,而不推进文件位置. 
      大多数时候,可以根据要求单独读取压缩流. 
      返回的字节数可能比要求多或少. 


      .. versionadded:: 3.2

   .. versionchanged:: 3.1
      Support for the :keyword:`with` statement was added.

   .. versionchanged:: 3.2
      Support for zero-padded files was added.

   .. versionchanged:: 3.2
      Support for unseekable files was added.


.. function:: open(filename, mode='rb', compresslevel=9)

   This is a shorthand for ``GzipFile(filename,`` ``mode,`` ``compresslevel)``.
   The *filename* argument is required; *mode* defaults to ``'rb'`` and
   *compresslevel* defaults to ``9``.

 这是``GzipFile(filename,`` ``mode,`` ``compresslevel)``的简写. 
   *filename*是必需的,*mode*默认``'rb'``,*compresslevel*默认``9``


.. function:: compress(data, compresslevel=9)

   Compress the *data*, returning a :class:`bytes` object containing
   the compressed data.  *compresslevel* has the same meaning as in
   the :class:`GzipFile` constructor above.

    压缩*data*,返回一个包含被压缩数据的``bytes``对象. 
   *compresslevel* 意义同上面``GzipFile``介绍


   .. versionadded:: 3.2

.. function:: decompress(data)

   Decompress the *data*, returning a :class:`bytes` object containing the
   uncompressed data.

   解压缩*data*,返回一个包含被解压数据的``bytes``对象. 


   .. versionadded:: 3.2


.. _gzip-usage-examples:

Examples of usage
-----------------

Example of how to read a compressed file::

读取压缩文件: 



   import gzip
   with gzip.open('/home/joe/file.txt.gz', 'rb') as f:
       file_content = f.read()

Example of how to create a compressed GZIP file::

如何建立一个GZIP压缩文档: 


   import gzip
   content = b"Lots of content here"
   with gzip.open('/home/joe/file.txt.gz', 'wb') as f:
       f.write(content)

Example of how to GZIP compress an existing file::

如何压缩已有文档


   import gzip
   with open('/home/joe/file.txt', 'rb') as f_in:
       with gzip.open('/home/joe/file.txt.gz', 'wb') as f_out:
           f_out.writelines(f_in)

Example of how to GZIP compress a binary string::

如何压缩二进制字符串
   import gzip
   s_in = b"Lots of content here"
   s_out = gzip.compress(s_in)

.. seealso::

   Module :mod:`zlib`
      The basic data compression module needed to support the :program:`gzip` file
      format.

        模块``zlib``
      基础的数据压缩模块必须要支持**gzip**文件格式. 





