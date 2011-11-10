:mod:`array` --- 高效的数值型数组
===================================================

.. module:: array

   :synopsis: Space efficient arrays of uniformly typed numeric values.


.. index:: single: arrays

本模块定义了一种对象类型,而此种对象类型可紧凑地代表一组基本值类型数组:字符类型,整
数类型浮点类型.数组是一种序列类型并且其行为非常类似于列表,只是数组里存储的对象类型
受限.类型由对象生成时的类型编码'type code'指定.而类型编码是一个单字符.类型编
码定义如下:

+-----------+----------------+-------------------+-----------------------+
| Type code | C Type         | Python Type       | Minimum size in bytes |
+===========+================+===================+=======================+
| ``'b'``   | signed char    | int               | 1                     |
+-----------+----------------+-------------------+-----------------------+
| ``'B'``   | unsigned char  | int               | 1                     |
+-----------+----------------+-------------------+-----------------------+
| ``'u'``   | Py_UNICODE     | Unicode character | 2 (see note)          |
+-----------+----------------+-------------------+-----------------------+
| ``'h'``   | signed short   | int               | 2                     |
+-----------+----------------+-------------------+-----------------------+
| ``'H'``   | unsigned short | int               | 2                     |
+-----------+----------------+-------------------+-----------------------+
| ``'i'``   | signed int     | int               | 2                     |
+-----------+----------------+-------------------+-----------------------+
| ``'I'``   | unsigned int   | int               | 2                     |
+-----------+----------------+-------------------+-----------------------+
| ``'l'``   | signed long    | int               | 4                     |
+-----------+----------------+-------------------+-----------------------+
| ``'L'``   | unsigned long  | int               | 4                     |
+-----------+----------------+-------------------+-----------------------+
| ``'f'``   | float          | float             | 4                     |
+-----------+----------------+-------------------+-----------------------+
| ``'d'``   | double         | float             | 8                     |
+-----------+----------------+-------------------+-----------------------+


.. note::

   The ``'u'`` typecode corresponds to Python's unicode character.  On narrow
   Unicode builds this is 2-bytes, on wide builds this is 4-bytes.

   注:'u'类型编码代表Python的unicode字符.在狭义构建版本中,这是2个字节而在广义构建
版本中,它有4个字节.

The actual representation of values is determined by the machine architecture
(strictly speaking, by the C implementation).  The actual size can be accessed
through the :attr:`itemsize` attribute.
      
------------------------------------------------------------------------------------------------------------------------------------------------------

值的实际代表由机器体系决定(严格说来,由C的实施决定).实际大小由'itemsize'属性获取.


本模块定义了如下类型:

.. class:: array(typecode[, initializer])

   A new array whose items are restricted by *typecode*, and initialized
   from the optional *initializer* value, which must be a list, object
   supporting the buffer interface, or iterable over elements of the
   appropriate type.
         
------------------------------------------------------------------------------------------------------------------------------------------------------

    一个新的数组(其成员项由typecode指定),并由可选项initializer值初始化,而其必须为一个
    列表,或支持缓冲接口的对象,或可遍历的适当类型的元素.

   If given a list or string, the initializer is passed to the new array's
   :meth:`fromlist`, :meth:`frombytes`, or :meth:`fromunicode` method (see below)
   to add initial items to the array.  Otherwise, the iterable initializer is
   passed to the :meth:`extend` method.
         
------------------------------------------------------------------------------------------------------------------------------------------------------

   如果给定一个列表或字串,初始化子(initializer)则被传到新数组的'fromlist()','frombytes()'或
   'fromunicode()'方法(见下)以向数组添加初始化项.否则, 可遍历初始化子将被传到extend()方法


.. data:: typecodes

   A string with all available type codes.
            
------------------------------------------------------------------------------------------------------------------------------------------------------

   一个允许所有可能类型代码的串

Array objects support the ordinary sequence operations of indexing, slicing,
concatenation, and multiplication.  When using slice assignment, the assigned
value must be an array object with the same type code; in all other cases,
:exc:`TypeError` is raised. Array objects also implement the buffer interface,
and may be used wherever buffer objects are supported.
         
------------------------------------------------------------------------------------------------------------------------------------------------------

数组对象支持普通序列操作如索引,取子串,联接及复制.当使用取子串操作时,指定值必须是与串
对象相同的类型代码,否则,产生'TypeError'错误.数组对象也实现了缓冲接口,也可在支持缓冲
对象的场景下使用.

The following data items and methods are also supported:
         
------------------------------------------------------------------------------------------------------------------------------------------------------

下列数据项及方法也被支持:

.. attribute:: array.typecode

   The typecode character used to create the array.
            
------------------------------------------------------------------------------------------------------------------------------------------------------

类型代码typecode字符用于创建数组

.. attribute:: array.itemsize

   The length in bytes of one array item in the internal representation.
            
------------------------------------------------------------------------------------------------------------------------------------------------------

在内部表示中,代表一个数组项元素的长度(以字节计)

.. method:: array.append(x)

   Append a new item with value *x* to the end of the array.
            
------------------------------------------------------------------------------------------------------------------------------------------------------

   追加一个新的其数值为'x'的数据项到数组尾部

.. method:: array.buffer_info()

   Return a tuple ``(address, length)`` giving the current memory address and the
   length in elements of the buffer used to hold array's contents.  The size of the
   memory buffer in bytes can be computed as ``array.buffer_info()[1] *
   array.itemsize``.  This is occasionally useful when working with low-level (and
   inherently unsafe) I/O interfaces that require memory addresses, such as certain
   :c:func:`ioctl` operations.  The returned numbers are valid as long as the array
   exists and no length-changing operations are applied to it.
            
------------------------------------------------------------------------------------------------------------------------------------------------------
   
   给定当前内存地址及内存缓冲区中元素个数,返回数组(地址,长度),通常用于保持数组内容.
   内存缓冲区的大小以字节计算,由公式array.buffer_info()[1]*array.itemsize获得.
   偶尔用于底层I/O接口操作如ioctl(),以获取内存地址.返回的数值只要数组存在且无改变长度的
   数组操作就有效.

   .. note::

      When using array objects from code written in C or C++ (the only way to
      effectively make use of this information), it makes more sense to use the buffer
      interface supported by array objects.  This method is maintained for backward
      compatibility and should be avoided in new code.  The buffer interface is
      documented in :ref:`bufferobjects`.
               
------------------------------------------------------------------------------------------------------------------------------------------------------
      
    注:当使用由C或C++编写的代码产生的数组对象时(唯一高效使用这类信息途径),使用数组对象
    支持的缓冲接口更有意义.这一方法保持了后向兼容性,且应在新代码中避免使用.缓冲接口详见
    Buffer Protocol


.. method:: array.byteswap()

   "Byteswap" all items of the array.  This is only supported for values which are
   1, 2, 4, or 8 bytes in size; for other types of values, :exc:`RuntimeError` is
   raised.  It is useful when reading data from a file written on a machine with a
   different byte order.
            
------------------------------------------------------------------------------------------------------------------------------------------------------

   
    字节交换所有数组内数据项.本方法只支持元素大小为1,2,4,8字节的数值项.对于其它类型值,
    将产生'RuntimeError'错误.本方法在从一个有着不同字节顺序的机器读取文件数据时有用.

.. method:: array.count(x)

   Return the number of occurrences of *x* in the array.
            
------------------------------------------------------------------------------------------------------------------------------------------------------

   返回数组中'x'元素出现的次数

.. method:: array.extend(iterable)

   Append items from *iterable* to the end of the array.  If *iterable* is another
   array, it must have *exactly* the same type code; if not, :exc:`TypeError` will
   be raised.  If *iterable* is not an array, it must be iterable and its elements
   must be the right type to be appended to the array.
            
------------------------------------------------------------------------------------------------------------------------------------------------------


从'iterable'中附加元素项至数组末尾.若'iterable'是另一个数组,其必须为与母数组同
一类型代码.若非如此,将产生'TypeError'.若'iterable'不是一个数组,则其必须可遍历且
其元素必须与将被附加到的数组同类型.

.. method:: array.frombytes(s)

   Appends items from the string, interpreting the string as an array of machine
   values (as if it had been read from a file using the :meth:`fromfile` method).
         
------------------------------------------------------------------------------------------------------------------------------------------------------

   从串中附加数据项,将串当作一个机器码值的数组(就如同fromfile()方法从文件中读取数据)

   .. versionadded:: 3.2
      :meth:`fromstring` is renamed to :meth:`frombytes` for clarity.
New in version 3.2: ``fromstring()`` is renamed to ``frombytes()``
   for clarity.
         
------------------------------------------------------------------------------------------------------------------------------------------------------

   在新的版本3.2中,为了清晰起见fromstring()方法已更名为frombytes()方法

.. method:: array.fromfile(f, n)


   Read *n* items (as machine values) from the :term:`file object` *f* and append
   them to the end of the array.  If less than *n* items are available,
   :exc:`EOFError` is raised, but the items that were available are still
   inserted into the array. *f* must be a real built-in file object; something
   else with a :meth:`read` method won't do.
            
------------------------------------------------------------------------------------------------------------------------------------------------------

   从文件对象f中读取n项(作为机器码),并附加到数组尾部.若可读取数少于n,则'EOFError'抛出,
   但可用的项数据还是被附加到数组. f必须是一个确实存在的内建文件对象; 另外的东西用read()方法有时不工作.

.. method:: array.fromlist(list)

   Append items from the list.  This is equivalent to ``for x in list:
   a.append(x)`` except that if there is a type error, the array is unchanged.
            
------------------------------------------------------------------------------------------------------------------------------------------------------

    从列表中附加数据项.本方法等同于 'for x in list: a.append(x)'语句.除非有一个类型
    错误,此时本数组未改变

.. method:: array.fromstring()


   Deprecated alias for :meth:`frombytes`.
            
------------------------------------------------------------------------------------------------------------------------------------------------------

   'frombytes()'方法的相对的别名


.. method:: array.fromunicode(s)

   Extends this array with data from the given unicode string.  The array must
   be a type ``'u'`` array; otherwise a :exc:`ValueError` is raised.  Use
   ``array.frombytes(unicodestring.encode(enc))`` to append Unicode data to an
   array of some other type.
            
------------------------------------------------------------------------------------------------------------------------------------------------------

    用给定unicode  串扩充数组. 数组必须是'u'类型的数组,否则,抛出'ValueError'错误.
    使用'array.frombytes(unicodestring.encode(enc))'方法来将Unicode数据附加至其它
    类型的数组上.


.. method:: array.index(x)

   Return the smallest *i* such that *i* is the index of the first occurrence of
   *x* in the array.
             
------------------------------------------------------------------------------------------------------------------------------------------------------

    返回数组中第一次出现 x 值的下标(最小下标值)

.. method:: array.insert(i, x)

   Insert a new item with value *x* in the array before position *i*. Negative
   values are treated as being relative to the end of the array.
             
------------------------------------------------------------------------------------------------------------------------------------------------------

    在i 位置前,将x 值插入数组.负值i将被认为是数组尾部.

.. method:: array.pop([i])

   Removes the item with the index *i* from the array and returns it. The optional
   argument defaults to ``-1``, so that by default the last item is removed and
   returned.
            
------------------------------------------------------------------------------------------------------------------------------------------------------

    从数组i位置移除数据项并返回它.可选参数默认为'-1',故默认返回数组最后一项并删除之.

.. method:: array.remove(x)

   Remove the first occurrence of *x* from the array.
             
------------------------------------------------------------------------------------------------------------------------------------------------------

    删除数组中的 x 值的第一次出现

.. method:: array.reverse()

   Reverse the order of the items in the array.
             
------------------------------------------------------------------------------------------------------------------------------------------------------

    逆序数组数据项

.. method:: array.tobytes()

   Convert the array to an array of machine values and return the bytes
   representation (the same sequence of bytes that would be written to a file by
   the :meth:`tofile` method.)
             
------------------------------------------------------------------------------------------------------------------------------------------------------

    将一个数组转换为机器值的数组并返回字节代码串(同样的字节串将被tofile()方法写入文件中).


   .. versionadded:: 3.2
      :meth:`tostring` is renamed to :meth:`tobytes` for clarity.
    
    版本3.2新功能:为清晰起见tostring()方法已被重命名为tobytes()方法.

.. method:: array.tofile(f)

   Write all items (as machine values) to the :term:`file object` *f*.
         
------------------------------------------------------------------------------------------------------------------------------------------------------

   将所有数据项(作为机器值)写入文件对象f

.. method:: array.tolist()

   Convert the array to an ordinary list with the same items.
             
------------------------------------------------------------------------------------------------------------------------------------------------------

    转换数组为一个数据项相同的普通列表

.. method:: array.tostring()

   Deprecated alias for :meth:`tobytes`.
            
------------------------------------------------------------------------------------------------------------------------------------------------------

   'tobytes()'方法相对的别名

.. method:: array.tounicode()

   Convert the array to a unicode string.  The array must be a type ``'u'`` array;
   otherwise a :exc:`ValueError` is raised. Use ``array.tobytes().decode(enc)`` to
   obtain a unicode string from an array of some other type.
            
------------------------------------------------------------------------------------------------------------------------------------------------------

    将一个串转换成一个unicode串.这个串必须是'u'类型的数组;否则抛出'ValueError'异常.
    使用'array.tobytes().decode(enc)'方法来从其它类型的数组中获取unicode串


When an array object is printed or converted to a string, it is represented as
``array(typecode, initializer)``.  The *initializer* is omitted if the array is
empty, otherwise it is a string if the *typecode* is ``'u'``, otherwise it is a
list of numbers.  The string is guaranteed to be able to be converted back to an
array with the same type and value using :func:`eval`, so long as the
:func:`array` function has been imported using ``from array import array``.
         
------------------------------------------------------------------------------------------------------------------------------------------------------

当一个数组对象被打印出来或被转换为字符串时,它又以'array(typecode,initializer)'的
形式表示. 这个initializer在数组为空时被省略, 其它情况下,若'typecode'为'u'则为字
符串,其它情况下为一个数值型的列表. 只要'array()'函数通过使用从数组引入数组被引入,
并使用'eval()',字符串当然可以被转换回同样类型同样数值的数组

Examples::

   array('l')
   array('u', 'hello \u2641')
   array('l', [1, 2, 3, 4, 5])
   array('d', [1.0, 2.0, 3.14])


.. seealso::

   Module :mod:`struct`
      Packing and unpacking of heterogeneous binary data.
               
------------------------------------------------------------------------------------------------------------------------------------------------------

    封包和解包不同结构的二进制数据


   Module :mod:`xdrlib`
      Packing and unpacking of External Data Representation (XDR) data as used in some
      remote procedure call systems.
               
------------------------------------------------------------------------------------------------------------------------------------------------------

    封包和解包外部数据表示(XDR)将之作用于某些远程系统过程调用


   `The Numerical Python Manual <http://numpy.sourceforge.net/numdoc/HTML/numdoc.htm>`_
      The Numeric Python extension (NumPy) defines another array type; see
      http://numpy.sourceforge.net/ for further information about Numerical Python.
      (A PDF version of the NumPy manual is available at
      http://numpy.sourceforge.net/numdoc/numdoc.pdf).
               
------------------------------------------------------------------------------------------------------------------------------------------------------

    数字化的Python扩展定义了其它一些数组类型;参见http://numpy.sourceforge.net/以获
    取更多关于数字化Python的信息.PDF版本的numpy手册可在http://numpy.sourceforge.net/numdoc/numdoc.pdf获取.

