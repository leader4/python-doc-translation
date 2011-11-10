:mod:`binhex` ---编码和解码binhex4文件
=================================================

.. module:: binhex
   :synopsis: Encode and decode files in binhex4 format.


This module encodes and decodes files in binhex4 format, a format allowing
representation of Macintosh files in ASCII. Only the data fork is handled.

这个模块编码和解码binhex4格式的文件. binhex4格式支持ASCII的Macintosh文件. 
仅仅处理数据. 

模块定义了下面的方法:

.. function:: binhex(input, output)

   Convert a binary file with filename *input* to binhex file *output*. The
   *output* parameter can either be a filename or a file-like object (any object
   supporting a :meth:`write` and :meth:`close` method).

   把*input*的2进制文件转化为*output*的binhex文件. *output*参数可以是一个文件名
   或者是一个类似与文件的对象(任意的支持``write()``和``close()``的对象)

.. function:: hexbin(input, output)

   Decode a binhex file *input*. *input* may be a filename or a file-like object
   supporting :meth:`read` and :meth:`close` methods. The resulting file is written
   to a file named *output*, unless the argument is ``None`` in which case the
   output filename is read from the binhex file.

   解码一个binhex的*input*文件. *input* 可能是一个文件名或者是一个支持``read()``
   和``close()``方法的类文件对象.如果参数不是``None``, 结果写入到*output*文件中. 
   否则输入文件名来自binhex文件. 

   The following exception is also defined:

   还定义了以下的异常: 

.. exception:: Error

   Exception raised when something can't be encoded using the binhex format (for
   example, a filename is too long to fit in the filename field), or when input is
   not properly encoded binhex data.

   当有些不能使用binxhex格式进行编码的时候会抛出异常. (举例: 一个文件名太长), 或者输入
   是不能被正确编码binxhex的数据. 

.. seealso::

   Module :mod:`binascii`
      Support module containing ASCII-to-binary and binary-to-ASCII conversions.

      含有从ASCII到2进制和2进制到ASCII转换的支撑模块. 


.. _binhex-notes:

注意
-----

There is an alternative, more powerful interface to the coder and decoder, see
the source for details.

   这是一个非常规的, 强大的编码和解码的接口. 细节参考源文件. 


If you code or decode textfiles on non-Macintosh platforms they will still use
the old Macintosh newline convention (carriage-return as end of line).

如果你在非Macintosh平台上编码或者解码文本文件, 将仍然使用旧的Macintosh的换行符
约定(回车作为一行的结束). 


As of this writing, :func:`hexbin` appears to not work in all cases.

当在写的时候, 只要出现了``hexbin()`` 都不能工作. 



