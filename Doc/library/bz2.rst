:mod:`bz2` --- 兼容:program:`bzip2`的压缩方式
===========================================================

.. module:: bz2
   :synopsis: Interface to compression and decompression routines
              compatible with bzip2.
.. moduleauthor:: Gustavo Niemeyer <niemeyer@conectiva.com>
.. sectionauthor:: Gustavo Niemeyer <niemeyer@conectiva.com>


This module provides a comprehensive interface for the bz2 compression library.
It implements a complete file interface, one-shot (de)compression functions, and
types for sequential (de)compression.

这个模块提供了一个全面的接口bz2压缩库. 它实现了一个完整的文件接口, 一次性
 (DE) 的压缩功能和顺序 (DE) 压缩的类型. 


For other archive formats, see the :mod:`gzip`, :mod:`zipfile`, and
:mod:`tarfile` modules.

对于其他存档格式, 请参阅 "GZIP" , ``zipfile``, 和``tarfile``模块. 


Here is a summary of the features offered by the bz2 module:

这里是一个BZ2模块所提供的功能摘要: 


* :class:`BZ2File` class implements a complete file interface, including
  :meth:`~BZ2File.readline`, :meth:`~BZ2File.readlines`,
  :meth:`~BZ2File.writelines`, :meth:`~BZ2File.seek`, etc;
  
  *`` BZ2File``类实现了一个完整的文件接口, 包括
 ``readline()``, ``readlines()``, ``writelines()``, ``seek()``,等;
 


* :class:`BZ2File` class implements emulated :meth:`~BZ2File.seek` support;

* `` BZ2File "类实现仿真 ``seek()``的支持;
 

* :class:`BZ2File` class implements universal newline support;

*`` BZ2File "类实现通用换行符的支持;


* :class:`BZ2File` class offers an optimized line iteration using a readahead
  algorithm;
  
*`` BZ2File``类为使用预读算法的迭代提供了一个优化的线路;
   

* Sequential (de)compression supported by :class:`BZ2Compressor` and
  :class:`BZ2Decompressor` classes;

*Sequential (de)压缩支持  "BZ2Compressor``和`` BZ2Decompressor" 类;


* One-shot (de)compression supported by :func:`compress` and :func:`decompress`

 *One-shot (de) "压缩支持``compress()``和  ``decompress()``功能;
 
 
  functions;

* Thread safety uses individual locking mechanism.

*线程安全使用单独的锁定机制. 




 (De) 压缩文件
------------------------

Handling of compressed files is offered by the :class:`BZ2File` class.

压缩文件的处理是透过``BZ2File``


.. class:: BZ2File(filename, mode='r', buffering=0, compresslevel=9)

   Open a bz2 file. Mode can be either ``'r'`` or ``'w'``, for reading (default)
   or writing. When opened for writing, the file will be created if it doesn't
   exist, and truncated otherwise. If *buffering* is given, ``0`` means
   unbuffered, and larger numbers specify the buffer size; the default is
   ``0``. If *compresslevel* is given, it must be a number between ``1`` and
   ``9``; the default is ``9``. Add a ``'U'`` to mode to open the file for input
   with universal newline support. Any line ending in the input file will be
   seen as a ``'\n'`` in Python.  Also, a file so opened gains the attribute
   :attr:`newlines`; the value for this attribute is one of ``None`` (no newline
   read yet), ``'\r'``, ``'\n'``, ``'\r\n'`` or a tuple containing all the
   newline types seen. Universal newlines are available only when
   reading. Instances support iteration in the same way as normal :class:`file`
   instances.
   
    打开一个BZ2文件. 模式可以是``'r'`` 或 ``'w'``, (默认)阅读或写. 
   当文件打开写时, 文件将被创建, 如果该文件不存在将被截断. 
   如果是*buffering*, ``0`` 代表无缓冲. 较大的数字指定缓冲区大小, 
   默认为 "0" . 如果是* compresslevel*,它必须是一个`` 1 "到" 9 "之间的数
   默认为"9";添加一个``'U'``模式给开放的具有普遍性的输入文件新行的支持. 
   输入文件中的任何行结束, 将在Python中被视为一个 ``'\n'``.
   同样,打开一个属性``newlines``;这个属性的值是``None`` (无换行符可读) , 
   ``'\r'``, ``'\n'``, ``'\r\n'`` 或者一个元组包含所有可见的换行符类型, 
   只有当阅读普遍换行, 实例支持迭代一样正常``file``实例. 



   :class:`BZ2File` supports the :keyword:`with` statement.

    "BZ2File``支持 ``with``声明. 
 
 
   .. versionchanged:: 3.1
      Support for the :keyword:`with` statement was added.

   改变在3.1版:支持``with``声明. 
   
   
   .. method:: close()

      Close the file. Sets data attribute :attr:`closed` to true. A closed file
      cannot be used for further I/O operations. :meth:`close` may be called
      more than once without error.

      关闭该文件.  设置数据属性的``closed``为true. 
      一个已经关闭的文件不能被用于进一步操作I / O. 
      ``close()``可以被调用多次而不发生错误. 


   .. method:: read([size])

      Read at most *size* uncompressed bytes, returned as a byte string. If the
      *size* argument is negative or omitted, read until EOF is reached.

 大多数*size*在阅读未压缩字节, 返回一个字节字符串.
      如果*size* 参数为负或省略, 读到EOF就达到了. 
      

   .. method:: readline([size])

      Return the next line from the file, as a byte string, retaining newline.
      A non-negative *size* argument limits the maximum number of bytes to
      return (an incomplete line may be returned then). Return an empty byte
      string at EOF.
      
       从文件中的下一行返回, 作为一个字节的字符串, 保留换行符. 
      一个非负的*size* 参数限制返回最大字节数.  (然后可能会返回
      一个不完整的行) 在EOF返回一个空字节的字符串. 


   .. method:: readlines([size])

      Return a list of lines read. The optional *size* argument, if given, is an
      approximate bound on the total number of bytes in the lines returned.

      读取行返回一个列表.可选的*size*参数, 
      如果给定的是一个近似约束中的字节总数线路的返回. 
      

   .. method:: seek(offset[, whence])

      Move to new file position. Argument *offset* is a byte count. Optional
      argument *whence* defaults to ``os.SEEK_SET`` or ``0`` (offset from start
      of file; offset should be ``>= 0``); other values are ``os.SEEK_CUR`` or
      ``1`` (move relative to current position; offset can be positive or
      negative), and ``os.SEEK_END`` or ``2`` (move relative to end of file;
      offset is usually negative, although many platforms allow seeking beyond
      the end of a file).
      
       移动到新的文件位置. 参数*offset*是一个字节计数. 
      可选参数*whence*默认为``os.SEEK_SET``或 "0" ;其他值是 "os.SEEK_CUR
      " 或 "1"  (相对于当前移动位置;偏移可以是正或负) , 和 "os.SEEK_END" 
      或 "2"  (相对移动的结束文件;偏移量通常是负的, 尽管很多平台允许寻求
      的文件超出末尾) . 
      

      Note that seeking of bz2 files is emulated, and depending on the
      parameters the operation may be extremely slow.
      
      请注意, 寻求的bz2文件是模拟的, 并取决于参数的操作可能会非常缓慢. 


   .. method:: tell()

      Return the current file position, an integer.
      
      返回当前的文件位置, 一个整数. 


   .. method:: write(data)

      Write the byte string *data* to file. Note that due to buffering,
      :meth:`close` may be needed before the file on disk reflects the data
      written.

  给文件写入字节串*data* . 请注意由于缓冲, ``close() "可能需要, 
      在磁盘上反映文件写入的数据. 
      
      
   .. method:: writelines(sequence_of_byte_strings)

      Write the sequence of byte strings to the file. Note that newlines are not
      added. The sequence can be any iterable object producing byte strings.
      This is equivalent to calling write() for each byte string.

      写入文件的字节串序列. 注意不添加换行. 该序列可以是任何迭代对象
      生产字节的字符. 这相当于每个字节的字符串调用write(). 

顺序压缩
--------------------------

Sequential compression and decompression is done using the classes
:class:`BZ2Compressor` and :class:`BZ2Decompressor`.

使用 "BZ2Compressor``和"BZ2Decompressor" 类来完成连续压缩和解压. 


.. class:: BZ2Compressor(compresslevel=9)

   Create a new compressor object. This object may be used to compress data
   sequentially. If you want to compress data in one shot, use the
   :func:`compress` function instead. The *compresslevel* parameter, if given,
   must be a number between ``1`` and ``9``; the default is ``9``.

 创建一个新的压缩对象.  这个对象可以用来压缩数据的顺序. 
   如果你想一次性压缩数据使用 ``compress()``函数来代替. 
   如果给出* compresslevel*参数, 必须有一个数是在1和9之间, 默认为 "9" . 



   .. method:: compress(data)

      Provide more data to the compressor object. It will return chunks of
      compressed data whenever possible. When you've finished providing data to
      compress, call the :meth:`flush` method to finish the compression process,
      and return what is left in internal buffers.

 为压缩对象提供更多的数据.  它将尽可能返回压缩数据块. 
      当你为压缩提供完数据, 调用``flush()`` 方法去完成压缩过程, 
      并返回还剩下什么留在内部缓冲区. 
      
      
      
   .. method:: flush()

      Finish the compression process and return what is left in internal
      buffers. You must not use the compressor object after calling this method.

      完成这个压缩的过程,还剩下内部缓冲. 您不能调用此方法后使用压缩对象. 



.. class:: BZ2Decompressor()

   Create a new decompressor object. This object may be used to decompress data
   sequentially. If you want to decompress data in one shot, use the
   :func:`decompress` function instead.
    
   创建一个新的解压缩对象.    这个对象可以用来解压缩数据的顺序. 
   如果你想一次性解压缩数据, 使用``decompress()``函数来代替. 


   .. method:: decompress(data)

      Provide more data to the decompressor object. It will return chunks of
      decompressed data whenever possible. If you try to decompress data after
      the end of stream is found, :exc:`EOFError` will be raised. If any data
      was found after the end of stream, it'll be ignored and saved in
      :attr:`unused_data` attribute.
      
      为解压缩对象提供更多的数据. 如果你试着在数据流结束后解压数据
      将会发现``EOFError``得到提升. 如果发现任何数据流结束后, 
      ``unused_data`` 属性将被忽略并保存. 



单次压缩
------------------------

One-shot compression and decompression is provided through the :func:`compress`
and :func:`decompress` functions.

单次压缩和解压是通过``compress()`` 和``decompress()``方法. 


.. function:: compress(data, compresslevel=9)

   Compress *data* in one shot. If you want to compress data sequentially, use
   an instance of :class:`BZ2Compressor` instead. The *compresslevel* parameter,
   if given, must be a number between ``1`` and ``9``; the default is ``9``.
   
   一次促成*data*压缩. 如果你想按顺序压缩, 使用实例``BZ2Compressor``
   代替.  如果给定* compresslevel*参数, 必须是1~9之间的数字, 默认为 "9" . 
   


.. function:: decompress(data)

   Decompress *data* in one shot. If you want to decompress data sequentially,
   use an instance of :class:`BZ2Decompressor` instead.
   一次促成*data*解压缩. 如果你想按顺序解压缩, 用实例``BZ2Decompressor``
   代替. 

