:mod:`zipfile` ---使用ZIP文档
=========================================

.. module:: zipfile
   :synopsis: Read and write ZIP-format archive files.
.. moduleauthor:: James C. Ahlstrom <jim@interet.com>
.. sectionauthor:: James C. Ahlstrom <jim@interet.com>

**Source code:** :source:`Lib/zipfile.py`

--------------

The ZIP file format is a common archive and compression standard. This module
provides tools to create, read, write, append, and list a ZIP file.  Any
advanced use of this module will require an understanding of the format, as
defined in `PKZIP Application Note
<http://www.pkware.com/documents/casestudies/APPNOTE.TXT>`_.

This module does not currently handle multi-disk ZIP files.
It can handle ZIP files that use the ZIP64 extensions
(that is ZIP files that are more than 4 GByte in size).  It supports
decryption of encrypted files in ZIP archives, but it currently cannot
create an encrypted file.  Decryption is extremely slow as it is
implemented in native Python rather than C.

For other archive formats, see the :mod:`bz2`, :mod:`gzip`, and
:mod:`tarfile` modules.

The module defines the following items:

.. exception:: BadZipFile

   The error raised for bad ZIP files (old name: ``zipfile.error``).

   .. versionadded:: 3.2


.. exception:: BadZipfile

   This is an alias for :exc:`BadZipFile` that exists for compatibility with
   Python versions prior to 3.2.  Usage is deprecated.


.. exception:: LargeZipFile

   The error raised when a ZIP file would require ZIP64 functionality but that has
   not been enabled.


.. class:: ZipFile
   :noindex:

   The class for reading and writing ZIP files.  See section
   :ref:`zipfile-objects` for constructor details.


.. class:: PyZipFile
   :noindex:

   Class for creating ZIP archives containing Python libraries.


.. class:: ZipInfo(filename='NoName', date_time=(1980,1,1,0,0,0))

   Class used to represent information about a member of an archive. Instances
   of this class are returned by the :meth:`getinfo` and :meth:`infolist`
   methods of :class:`ZipFile` objects.  Most users of the :mod:`zipfile` module
   will not need to create these, but only use those created by this
   module. *filename* should be the full name of the archive member, and
   *date_time* should be a tuple containing six fields which describe the time
   of the last modification to the file; the fields are described in section
   :ref:`zipinfo-objects`.


.. function:: is_zipfile(filename)

   Returns ``True`` if *filename* is a valid ZIP file based on its magic number,
   otherwise returns ``False``.  *filename* may be a file or file-like object too.

   .. versionchanged:: 3.1
      Support for file and file-like objects.


.. data:: ZIP_STORED

   The numeric constant for an uncompressed archive member.


.. data:: ZIP_DEFLATED

   The numeric constant for the usual ZIP compression method.  This requires the
   zlib module.  No other compression methods are currently supported.


.. seealso::

   `PKZIP Application Note <http://www.pkware.com/documents/casestudies/APPNOTE.TXT>`_
      Documentation on the ZIP file format by Phil Katz, the creator of the format and
      algorithms used.

   `Info-ZIP Home Page <http://www.info-zip.org/>`_
      Information about the Info-ZIP project's ZIP archive programs and development
      libraries.


.. _zipfile-objects:

ZipFile Objects
---------------


.. class:: ZipFile(file, mode='r', compression=ZIP_STORED, allowZip64=False)

   Open a ZIP file, where *file* can be either a path to a file (a string) or a
   file-like object.  The *mode* parameter should be ``'r'`` to read an existing
   file, ``'w'`` to truncate and write a new file, or ``'a'`` to append to an
   existing file.  If *mode* is ``'a'`` and *file* refers to an existing ZIP
   file, then additional files are added to it.  If *file* does not refer to a
   ZIP file, then a new ZIP archive is appended to the file.  This is meant for
   adding a ZIP archive to another file (such as :file:`python.exe`).  If
   *mode* is ``a`` and the file does not exist at all, it is created.
   *compression* is the ZIP compression method to use when writing the archive,
   and should be :const:`ZIP_STORED` or :const:`ZIP_DEFLATED`; unrecognized
   values will cause :exc:`RuntimeError` to be raised.  If :const:`ZIP_DEFLATED`
   is specified but the :mod:`zlib` module is not available, :exc:`RuntimeError`
   is also raised. The default is :const:`ZIP_STORED`.  If *allowZip64* is
   ``True`` zipfile will create ZIP files that use the ZIP64 extensions when
   the zipfile is larger than 2 GB. If it is  false (the default) :mod:`zipfile`
   will raise an exception when the ZIP file would require ZIP64 extensions.
   ZIP64 extensions are disabled by default because the default :program:`zip`
   and :program:`unzip` commands on Unix (the InfoZIP utilities) don't support
   these extensions.
   
   创建一个ZipFile对象,表示一个zip文件. 参数file表示文件的路径或类文件对象(file-like object); 
   参数mode指示打开zip文件的模式,默认值为'r',表示读已经存在的zip文件,也可以为'w'或'a','w'表示新建一个zip文档或覆盖一个已经存在的zip文档,
   'a'表示将数据附加到一个现存的zip文档中. 参数compression表示在写zip文档时使用的压缩方法,它的值可以是zipfile. ZIP_STORED 或zipfile.
    ZIP_DEFLATED. 如果要操作的zip文件大小超过2G,应该将allowZip64设置为True. 

   If the file is created with mode ``'a'`` or ``'w'`` and then
   :meth:`close`\ d without adding any files to the archive, the appropriate
   ZIP structures for an empty archive will be written to the file.

   ZipFile is also a context manager and therefore supports the
   :keyword:`with` statement.  In the example, *myzip* is closed after the
   :keyword:`with` statement's suite is finished---even if an exception occurs::

      with ZipFile('spam.zip', 'w') as myzip:
          myzip.write('eggs.txt')

   .. versionadded:: 3.2
      Added the ability to use :class:`ZipFile` as a context manager.


.. method:: ZipFile.close()

   Close the archive file.  You must call :meth:`close` before exiting your program
   or essential records will not be written.


.. method:: ZipFile.getinfo(name)

   Return a :class:`ZipInfo` object with information about the archive member
   *name*.  Calling :meth:`getinfo` for a name not currently contained in the
   archive will raise a :exc:`KeyError`.
   
   获取zip文档内指定文件的信息. 返回一个zipfile.ZipInfo对象,它包括文件的详细信息. 将在下面 具体介绍该对象. 


.. method:: ZipFile.infolist()

   Return a list containing a :class:`ZipInfo` object for each member of the
   archive.  The objects are in the same order as their entries in the actual ZIP
   file on disk if an existing archive was opened.
   
   获取zip文档内所有文件的信息,返回一个zipfile.ZipInfo的列表. 


.. method:: ZipFile.namelist()

   Return a list of archive members by name.
   
   获取zip文档内所有文件的名称列表. 


.. method:: ZipFile.open(name, mode='r', pwd=None)

   Extract a member from the archive as a file-like object (ZipExtFile). *name* is
   the name of the file in the archive, or a :class:`ZipInfo` object. The *mode*
   parameter, if included, must be one of the following: ``'r'`` (the  default),
   ``'U'``, or ``'rU'``. Choosing ``'U'`` or  ``'rU'`` will enable universal newline
   support in the read-only object. *pwd* is the password used for encrypted files.
   Calling  :meth:`open` on a closed ZipFile will raise a  :exc:`RuntimeError`.

   .. note::

      The file-like object is read-only and provides the following methods:
      :meth:`!read`, :meth:`!readline`, :meth:`!readlines`, :meth:`!__iter__`,
      :meth:`!__next__`.

   .. note::

      If the ZipFile was created by passing in a file-like object as the  first
      argument to the constructor, then the object returned by :meth:`.open` shares the
      ZipFile's file pointer.  Under these  circumstances, the object returned by
      :meth:`.open` should not  be used after any additional operations are performed
      on the  ZipFile object.  If the ZipFile was created by passing in a string (the
      filename) as the first argument to the constructor, then  :meth:`.open` will
      create a new file object that will be held by the ZipExtFile, allowing it to
      operate independently of the  ZipFile.

   .. note::

      The :meth:`open`, :meth:`read` and :meth:`extract` methods can take a filename
      or a :class:`ZipInfo` object.  You will appreciate this when trying to read a
      ZIP file that contains members with duplicate names.


.. method:: ZipFile.extract(member, path=None, pwd=None)

   Extract a member from the archive to the current working directory; *member*
   must be its full name or a :class:`ZipInfo` object).  Its file information is
   extracted as accurately as possible.  *path* specifies a different directory
   to extract to.  *member* can be a filename or a :class:`ZipInfo` object.
   *pwd* is the password used for encrypted files.
   
   将zip文档内的指定文件解压到当前目录. 参数member指定要解压的文件名称或对应的ZipInfo对象; 
   参数path指定了解析文件保存的文件夹; 参数pwd为解压密码. 


.. method:: ZipFile.extractall(path=None, members=None, pwd=None)

   Extract all members from the archive to the current working directory.  *path*
   specifies a different directory to extract to.  *members* is optional and must
   be a subset of the list returned by :meth:`namelist`.  *pwd* is the password
   used for encrypted files.
   
   解压zip文档中的所有文件到当前目录. 参数members的默认值为zip文档内的所有文件名称列表,也可以自己设置,选择要解压的文件名称. 

   .. warning::

      Never extract archives from untrusted sources without prior inspection.
      It is possible that files are created outside of *path*, e.g. members
      that have absolute filenames starting with ``"/"`` or filenames with two
      dots ``".."``.


.. method:: ZipFile.printdir()

   Print a table of contents for the archive to ``sys.stdout``.
   
   将zip文档内的信息打印到控制台上. 


.. method:: ZipFile.setpassword(pwd)

   Set *pwd* as default password to extract encrypted files.
   
   设置zip文档的密码. 


.. method:: ZipFile.read(name, pwd=None)

   Return the bytes of the file *name* in the archive.  *name* is the name of the
   file in the archive, or a :class:`ZipInfo` object.  The archive must be open for
   read or append. *pwd* is the password used for encrypted  files and, if specified,
   it will override the default password set with :meth:`setpassword`.  Calling
   :meth:`read` on a closed ZipFile  will raise a :exc:`RuntimeError`.
   
   获取zip文档内指定文件的二进制数据. 下面的例子演示了read()的使用,zip文档内包括一个txt.txt的文本文件,使用read()方法读取其二进制数据,然后保存到D:/txt.txt. 


.. method:: ZipFile.testzip()

   Read all the files in the archive and check their CRC's and file headers.
   Return the name of the first bad file, or else return ``None``. Calling
   :meth:`testzip` on a closed ZipFile will raise a :exc:`RuntimeError`.


.. method:: ZipFile.write(filename, arcname=None, compress_type=None)

   Write the file named *filename* to the archive, giving it the archive name
   *arcname* (by default, this will be the same as *filename*, but without a drive
   letter and with leading path separators removed).  If given, *compress_type*
   overrides the value given for the *compression* parameter to the constructor for
   the new entry.  The archive must be open with mode ``'w'`` or ``'a'`` -- calling
   :meth:`write` on a ZipFile created with mode ``'r'`` will raise a
   :exc:`RuntimeError`.  Calling  :meth:`write` on a closed ZipFile will raise a
   :exc:`RuntimeError`.
   
   将指定文件添加到zip文档中. filename为文件路径,arcname为添加到zip文档之后保存的名称, 
   参数compress_type表示压缩方法,它的值可以是zipfile. ZIP_STORED 或zipfile. ZIP_DEFLATED. 

   .. note::

      There is no official file name encoding for ZIP files. If you have unicode file
      names, you must convert them to byte strings in your desired encoding before
      passing them to :meth:`write`. WinZip interprets all file names as encoded in
      CP437, also known as DOS Latin.

   .. note::

      Archive names should be relative to the archive root, that is, they should not
      start with a path separator.

   .. note::

      If ``arcname`` (or ``filename``, if ``arcname`` is  not given) contains a null
      byte, the name of the file in the archive will be truncated at the null byte.


.. method:: ZipFile.writestr(zinfo_or_arcname, bytes[, compress_type])

   Write the string *bytes* to the archive; *zinfo_or_arcname* is either the file
   name it will be given in the archive, or a :class:`ZipInfo` instance.  If it's
   an instance, at least the filename, date, and time must be given.  If it's a
   name, the date and time is set to the current date and time. The archive must be
   opened with mode ``'w'`` or ``'a'`` -- calling  :meth:`writestr` on a ZipFile
   created with mode ``'r'``  will raise a :exc:`RuntimeError`.  Calling
   :meth:`writestr` on a closed ZipFile will raise a :exc:`RuntimeError`.
   
   writestr()支持将二进制数据直接写入到压缩文档. 

   If given, *compress_type* overrides the value given for the *compression*
   parameter to the constructor for the new entry, or in the *zinfo_or_arcname*
   (if that is a :class:`ZipInfo` instance).

   .. note::

      When passing a :class:`ZipInfo` instance as the *zinfo_or_arcname* parameter,
      the compression method used will be that specified in the *compress_type*
      member of the given :class:`ZipInfo` instance.  By default, the
      :class:`ZipInfo` constructor sets this member to :const:`ZIP_STORED`.

   .. versionchanged:: 3.2
      The *compression_type* argument.
      
ZipFile.getinfo(name) 方法返回的是一个ZipInfo对象,表示zip文档中相应文件的信息. 它支持如下属性: 

ZipInfo.filename:  获取文件名称. 
ZipInfo.date_time:  获取文件最后修改时间. 返回一个包含6个元素的元组: (年, 月, 日, 时, 分, 秒)
ZipInfo.compress_type:  压缩类型. 
ZipInfo.comment:  文档说明. 
ZipInfo.extr:  扩展项数据. 
ZipInfo.create_system:  获取创建该zip文档的系统. 
ZipInfo.create_version:  获取 创建zip文档的PKZIP版本. 
ZipInfo.extract_version:  获取 解压zip文档所需的PKZIP版本. 
ZipInfo.reserved:  预留字段,当前实现总是返回0. 
ZipInfo.flag_bits:  zip标志位. 
ZipInfo.volume:  文件头的卷标. 
ZipInfo.internal_attr:  内部属性. 
ZipInfo.external_attr:  外部属性. 
ZipInfo.header_offset:  文件头偏移位. 
ZipInfo.CRC:  未压缩文件的CRC-32. 
ZipInfo.compress_size:  获取压缩后的大小. 
ZipInfo.file_size:  获取未压缩的文件大小. 

The following data attributes are also available:


.. attribute:: ZipFile.debug

   The level of debug output to use.  This may be set from ``0`` (the default, no
   output) to ``3`` (the most output).  Debugging information is written to
   ``sys.stdout``.

.. attribute:: ZipFile.comment

   The comment text associated with the ZIP file.  If assigning a comment to a
   :class:`ZipFile` instance created with mode 'a' or 'w', this should be a
   string no longer than 65535 bytes.  Comments longer than this will be
   truncated in the written archive when :meth:`ZipFile.close` is called.


.. _pyzipfile-objects:

PyZipFile Objects
-----------------

The :class:`PyZipFile` constructor takes the same parameters as the
:class:`ZipFile` constructor, and one additional parameter, *optimize*.

.. class:: PyZipFile(file, mode='r', compression=ZIP_STORED, allowZip64=False, \
                     optimize=-1)

   .. versionadded:: 3.2
      The *optimize* parameter.

   Instances have one method in addition to those of :class:`ZipFile` objects:

   .. method:: PyZipFile.writepy(pathname, basename='')

      Search for files :file:`\*.py` and add the corresponding file to the
      archive.

      If the *optimize* parameter to :class:`PyZipFile` was not given or ``-1``,
      the corresponding file is a :file:`\*.pyo` file if available, else a
      :file:`\*.pyc` file, compiling if necessary.

      If the *optimize* parameter to :class:`PyZipFile` was ``0``, ``1`` or
      ``2``, only files with that optimization level (see :func:`compile`) are
      added to the archive, compiling if necessary.

      If the pathname is a file, the filename must end with :file:`.py`, and
      just the (corresponding :file:`\*.py[co]`) file is added at the top level
      (no path information).  If the pathname is a file that does not end with
      :file:`.py`, a :exc:`RuntimeError` will be raised.  If it is a directory,
      and the directory is not a package directory, then all the files
      :file:`\*.py[co]` are added at the top level.  If the directory is a
      package directory, then all :file:`\*.py[co]` are added under the package
      name as a file path, and if any subdirectories are package directories,
      all of these are added recursively.  *basename* is intended for internal
      use only.  The :meth:`writepy` method makes archives with file names like
      this::

         string.pyc                   # Top level name
         test/__init__.pyc            # Package directory
         test/testall.pyc             # Module test.testall
         test/bogus/__init__.pyc      # Subpackage directory
         test/bogus/myfile.pyc        # Submodule test.bogus.myfile


.. _zipinfo-objects:

ZipInfo Objects
---------------

Instances of the :class:`ZipInfo` class are returned by the :meth:`getinfo` and
:meth:`infolist` methods of :class:`ZipFile` objects.  Each object stores
information about a single member of the ZIP archive.

Instances have the following attributes:


.. attribute:: ZipInfo.filename

   Name of the file in the archive.


.. attribute:: ZipInfo.date_time

   The time and date of the last modification to the archive member.  This is a
   tuple of six values:

   +-------+--------------------------+
   | Index | Value                    |
   +=======+==========================+
   | ``0`` | Year                     |
   +-------+--------------------------+
   | ``1`` | Month (one-based)        |
   +-------+--------------------------+
   | ``2`` | Day of month (one-based) |
   +-------+--------------------------+
   | ``3`` | Hours (zero-based)       |
   +-------+--------------------------+
   | ``4`` | Minutes (zero-based)     |
   +-------+--------------------------+
   | ``5`` | Seconds (zero-based)     |
   +-------+--------------------------+


.. attribute:: ZipInfo.compress_type

   Type of compression for the archive member.


.. attribute:: ZipInfo.comment

   Comment for the individual archive member.


.. attribute:: ZipInfo.extra

   Expansion field data.  The `PKZIP Application Note
   <http://www.pkware.com/documents/casestudies/APPNOTE.TXT>`_ contains
   some comments on the internal structure of the data contained in this string.


.. attribute:: ZipInfo.create_system

   System which created ZIP archive.


.. attribute:: ZipInfo.create_version

   PKZIP version which created ZIP archive.


.. attribute:: ZipInfo.extract_version

   PKZIP version needed to extract archive.


.. attribute:: ZipInfo.reserved

   Must be zero.


.. attribute:: ZipInfo.flag_bits

   ZIP flag bits.


.. attribute:: ZipInfo.volume

   Volume number of file header.


.. attribute:: ZipInfo.internal_attr

   Internal attributes.


.. attribute:: ZipInfo.external_attr

   External file attributes.


.. attribute:: ZipInfo.header_offset

   Byte offset to the file header.


.. attribute:: ZipInfo.CRC

   CRC-32 of the uncompressed file.


.. attribute:: ZipInfo.compress_size

   Size of the compressed data.


.. attribute:: ZipInfo.file_size

   Size of the uncompressed file.
   
   
Python标准模块中,有多个模块用于数据的压缩与解压缩,如zipfile,gzip, bz2等等. 上次介绍了zipfile模块,今天就来讲讲zlib模块. 
zlib.compress(string[, level])
zlib.decompress(string[, wbits[, bufsize]])

　　zlib.compress用于压缩流数据. 参数string指定了要压缩的数据流,参数level指定了压缩的级别,它的取值范围是1到9. 
压缩速度与压缩率成反比,1表示压缩速度最快,而压缩率最低,而9则表示压缩速度最慢但压缩率最高. zlib.decompress用于解压数据. 
参数string指定了需要解压的数据,wbits和bufsize分别用于设置系统缓冲区大小(window buffer )与输出缓冲区大小(output buffer). 

　我们也可以使用Compress/Decompress对象来对数据进行压缩/解压缩. zlib.compressobj([level]) 与zlib.decompress(string[, wbits[, bufsize]]) 
分别创建Compress/Decompress缩对象. 通过对象对数据进行压缩和解压缩的使用方式与上面介绍的zlib.compress,zlib.decompress非常类似. 但两者对数据的压缩还是有区别的,
这主要体现在对大量数据进行操作的情况下. 假如现在要压缩一个非常大的数据文件 (上百M) ,如果使用zlib.compress来压缩的话,必须先一次性将文件里的数据读到内存里,然后将数据进行压缩. 
这样势必会战用太多的内存. 如果使用对象来进行压缩,那么没有必要一次性读取文件的所有数据,可以先读一部分数据到内存里进行压缩,压缩完后写入文件,然后再读其他部分的数据压缩,
如此循环重复,只到压缩完整个文件. 



