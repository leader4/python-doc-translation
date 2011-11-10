:mod:`marshal` --- 内部Python的对象序列化 
=======================================================

.. module:: marshal
   :synopsis: Convert Python objects to streams of bytes and back (with different
              constraints).


This module contains functions that can read and write Python values in a binary
format.  The format is specific to Python, but independent of machine
architecture issues (e.g., you can write a Python value to a file on a PC,
transport the file to a Sun, and read it back there).  Details of the format are
undocumented on purpose; it may change between Python versions (although it
rarely does). [#]_

该模块包含可以以二进制格式中读取和写入Python值的函数. 格式是特定于Python,但
独立于机器架构问题(例如,你可以在一台PC的一个文件中写入一个Python的值,将该
文件传到一台Sun服务器上,在Sun服务器上读出). 格式的细节故意不做文档化; 它在不同
的Python版本中会有所不同(即使很少这样做). [#]_

.. index::
   module: pickle
   module: shelve
   object: code

This is not a general "persistence" module.  For general persistence and
transfer of Python objects through RPC calls, see the modules :mod:`pickle` and
:mod:`shelve`.  The :mod:`marshal` module exists mainly to support reading and
writing the "pseudo-compiled" code for Python modules of :file:`.pyc` files.
Therefore, the Python maintainers reserve the right to modify the marshal format
in backward incompatible ways should the need arise.  If you're serializing and
de-serializing Python objects, use the :mod:`pickle` module instead -- the
performance is comparable, version independence is guaranteed, and pickle
supports a substantially wider range of objects than marshal.

这不是普通意义上"持久"的模块. 对于一般通过RPC调用Python对象的转移和持久性,
参看``pickle``和``shelve``模块. ``marshal``模块存在的主要目的是支持对
``.pyc``文件的Python模块进行读和写"伪编译代码". 因此,Python的维护者预备
权利,以向后不兼容的方式修改marshal格式,是非常有必要的. 如果你序列化和反序列
化Python对象,使用``pickle``模块代替--性能更好,可以保证版本的独立性,并且
pickle支持比marshal更广的对象范围. 



.. warning::

   The :mod:`marshal` module is not intended to be secure against erroneous or
   maliciously constructed data.  Never unmarshal data received from an
   untrusted or unauthenticated source.

   警告: ``marshal``模块对于错误或者恶意构造的数据并不安全. 绝对不要使用marshal
处理从不信任或者没有被验证的源接收的数据. 


Not all Python object types are supported; in general, only objects whose value
is independent from a particular invocation of Python can be written and read by
this module.  The following types are supported: booleans, integers, floating
point numbers, complex numbers, strings, bytes, bytearrays, tuples, lists, sets,
frozensets, dictionaries, and code objects, where it should be understood that
tuples, lists, sets, frozensets and dictionaries are only supported as long as
the values contained therein are themselves supported; and recursive lists, sets
and dictionaries should not be written (they will cause infinite loops).  The
singletons :const:`None`, :const:`Ellipsis` and :exc:`StopIteration` can also be
marshalled and unmarshalled.

并非所有的Python对象都被支持; 一般情况下,只有对象的值是独立于Python的特定调用,
才可以被模块读和写. 以下类型被支持: 布尔类型,整数,浮点数,复数,字符串,字节,
字节组,元组,列表,集合,冻结集合和代码中的对象,它应该被理解为: 元组,列表,
集合,冻结集合和字典只支持他们本身自带的所支持的值; 并且递归列表,集合和字典不
应该被写入(他们会造成无限的循环). 单独得``None``,``Ellipsis``和
``StopIteration``也可以被marshalled和unmarshalled. 



There are functions that read/write files as well as functions operating on
strings.

这里有一些函数可以像函数操作字符串一样地读/写文件. 



The module defines these functions:

模块定义了这些函数: 


.. function:: dump(value, file[, version])

   Write the value on the open file.  The value must be a supported type.  The
   file must be an open file object such as ``sys.stdout`` or returned by
   :func:`open` or :func:`os.popen`.  It must be opened in binary mode (``'wb'``
   or ``'w+b'``).

    在打开的文件中写入值. 该值必须是被支持的类型. 该文件必须是一个像
   ``sys.stdout``或者通过``open()``或者``os.popen()返回的这样的打开的
   文件对象. 它必须以二进制模式(``'wb'``或者``'w+b'``打开). 



   If the value has (or contains an object that has) an unsupported type, a
   :exc:`ValueError` exception is raised --- but garbage data will also be written
   to the file.  The object will not be properly read back by :func:`load`.

   如果值(或者包含的对象)有不被至支持的类型,会出现一个``ValueError``的提示
   --- 但是垃圾数据也会被写入到文件中. 该对象不会被``load()``正确读回. 



   The *version* argument indicates the data format that ``dump`` should use
   (see below).

   *version*参数指明了``dump``应该使用的数据格式. (见下文)


.. function:: load(file)

   Read one value from the open file and return it.  If no valid value is read
   (e.g. because the data has a different Python version's incompatible marshal
   format), raise :exc:`EOFError`, :exc:`ValueError` or :exc:`TypeError`.  The
   file must be an open file object opened in binary mode (``'rb'`` or
   ``'r+b'``).

   从打开的文件中读取一个值并返回它. 如果无效值被读取(; 例如,因为数据在不同的
   Python版本中具有不相容的marshal格式),会提示``EOFError``,``ValueError``
   或者``TypeError``. 该文件必须是以二进制模式(``'rb'``或者``'r+b'``)打开的
   文件对象. 



   .. note::

      If an object containing an unsupported type was marshalled with :func:`dump`,
      :func:`load` will substitute ``None`` for the unmarshallable type.

      注意: 如果一个包含不被支持类型的对象用``dump()``,``load()``被marshalled,
   会用``None``替代不能被marshall的类型. 


.. function:: dumps(value[, version])

   Return the string that would be written to a file by ``dump(value, file)``.  The
   value must be a supported type.  Raise a :exc:`ValueError` exception if value
   has (or contains an object that has) an unsupported type.

    返回的字符串以``dump(value,file)``的形式写入文件. 该值必须是被支持的类型. 
   如果该值(或者)有不被支持的类型


   The *version* argument indicates the data format that ``dumps`` should use
   (see below).

   *version*参数指明了``dump``应该使用的数据格式. (见下文)


.. function:: loads(string)

   Convert the string to a value.  If no valid value is found, raise
   :exc:`EOFError`, :exc:`ValueError` or :exc:`TypeError`.  Extra characters in the
   string are ignored.

    将字符串转换为值. 如果无效的值出现,会提示``EOFError``,``ValueError``
   或者``TypeError``. 在字符串当中的特别字符会被忽视. 


In addition, the following constants are defined:

此外,下列常量的定义如下: 


.. data:: version

   Indicates the format that the module uses. Version 0 is the historical
   format, version 1 shares interned strings and version 2 uses a binary format
   for floating point numbers. The current version is 2.

    指定模块使用的格式. 版本0是历史的格式,版本1分享实验字符串,版本2对浮点数使用
   二进制格式. 现在的版本为2.


.. rubric:: Footnotes

.. [#] The name of this module stems from a bit of terminology used by the designers of
   Modula-3 (amongst others), who use the term "marshalling" for shipping of data
   around in a self-contained form. Strictly speaking, "to marshal" means to
   convert some data from internal to external form (in an RPC buffer for instance)
   and "unmarshalling" for the reverse process.

   [1] 模块的名字起源于Modula-3的设计者使用的位的术语,该设计者使用术语
"marshalling"表示以数据自有的形式分发数据. 严格的讲,"to marshal"意味着将内部
的一些数据转化到外部形式(例如RPC缓冲区),"unmarshalling"则表示逆过程. 





