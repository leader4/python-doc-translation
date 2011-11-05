:mod: ``binascii`` --- 二进制和ASCII之间转换
====================================================

.. module:: binascii
   :synopsis: Tools for converting between binary and various ASCII-encoded binary
              representations.


.. index::
   module: uu
   module: base64
   module: binhex

The :mod:`binascii` module contains a number of methods to convert between
binary and various ASCII-encoded binary representations. Normally, you will not
use these functions directly but use wrapper modules like :mod:`uu`,
:mod:`base64`, or :mod:`binhex` instead. The :mod:`binascii` module contains
low-level functions written in C for greater speed that are used by the
higher-level modules.

binascii 模块包含了许多方法来转换字节和各种ASCII编码的二进制表示。
通常，你不会直接使用这些功能而是使用``uu ``,``Base64的 ``或``binhex`` 代替。该
binascii模块包含用C语言编写的低级函数用于作为更快的速度由更高级别的模块使用。


.. note::

   Encoding and decoding functions do not accept Unicode strings.  Only bytestring
   and bytearray objects can be processed.

   编码和解码功能不接受Unicode字符串。只有bytestring
   ByteArray对象可以处理。

The :mod:`binascii` module defines the following functions:


.. function:: a2b_uu(string)

   Convert a single line of uuencoded data back to binary and return the binary
   data. Lines normally contain 45 (binary) bytes, except for the last line. Line
   data may be followed by whitespace.

   一个单行UUENCODED数据转换成二进制文件，并返回二进制
   数据。线通常含有45（二进制）字节，除了最后一行。线
   数据可能后跟空白。


.. function:: b2a_uu(data)

   Convert binary data to a line of ASCII characters, the return value is the
   converted line, including a newline char. The length of *data* should be at most
   45.

   ASCII字符转换成二进制的数据，返回值是
   转换线，包括一个换行符的字符。 *数据*长度应在最
   45。


.. function:: a2b_base64(string)

   Convert a block of base64 data back to binary and return the binary data. More
   than one line may be passed at a time.

   转换的Base64编码的二进制数据块，并返回返回二进制
   data。超过一行可能是通过一次。


.. function:: b2a_base64(data)

   Convert binary data to a line of ASCII characters in base64 coding. The return
   value is the converted line, including a newline char. The length of *data*
   should be at most 57 to adhere to the base64 standard.

   以base64编码转换ASCII字符的二进制数据。返回
   值是转换线，包括一个换行符的字符。 *数据的长度*
   最多57应坚持的base64标准。


.. function:: a2b_qp(string, header=False)

   Convert a block of quoted-printable data back to binary and return the binary
   data. More than one line may be passed at a time. If the optional argument
   *header* is present and true, underscores will be decoded as spaces.

   使用quoted - printable数据块转换成二进制文件，并返回二进制
   数据。多个行可能是一次通过。如果可选的参数
   *头*，是当前和真实，强调将作为空间解码。

   .. versionchanged:: 3.2
      Accept only bytestring or bytearray objects as input.


.. function:: b2a_qp(data, quotetabs=False, istext=True, header=False)

   Convert binary data to a line(s) of ASCII characters in quoted-printable
   encoding.  The return value is the converted line(s). If the optional argument
   *quotetabs* is present and true, all tabs and spaces will be encoded.   If the
   optional argument *istext* is present and true, newlines are not encoded but
   trailing whitespace will be encoded. If the optional argument *header* is
   present and true, spaces will be encoded as underscores per RFC1522. If the
   optional argument *header* is present and false, newline characters will be
   encoded as well; otherwise linefeed conversion might corrupt the binary data
   stream.

   二进制数据转换成一个ASCII字符的行（S）在使用quoted - printable
   编码。返回值是转换线（S）。如果可选的参数
   * quotetabs目前的和真实的，所有的空格和制表符将被编码。如果
   可选参数* ISTEXT*是目前的和真实的，换行不编码，但
   结尾的空白将被编码。如果可选参数*头*
   目前真实，空格会被编码为每RFC1522强调。如果
   可选参数*头*是当前和假，换行符将
   编码;否则换行的转换可能会损坏的二进制数据
   流。...


.. function:: a2b_hqx(string)

   Convert binhex4 formatted ASCII data to binary, without doing RLE-decompression.
   The string should contain a complete number of binary bytes, or (in case of the
   last portion of the binhex4 data) have the remaining bits zero.

   binhex4格式的ASCII数据转换为二进制，而不做RLE减压。
   该字符串应该包含一个完整的二进制字节数，或（在本案
   最后一部分的binhex4数据），其余位为零。




.. function:: rledecode_hqx(data)

   Perform RLE-decompression on the data, as per the binhex4 standard. The
   algorithm uses ``0x90`` after a byte as a repeat indicator, followed by a count.
   A count of ``0`` specifies a byte value of ``0x90``. The routine returns the
   decompressed data, unless data input data ends in an orphaned repeat indicator,
   in which case the :exc:`Incomplete` exception is raised.

   执行上的数据，按binhex4标准，RLE减压。 “
   算法使用“0x90”后重复计数的指标，一个字节。
   一个“0”count指定的“0x90字节的值”。例程返回
   解压后的数据，除非数据输入数据在一个孤立的重复指标的结束，
   在这种情况下：商务顾客：'不完整的`引发异常。

   .. versionchanged:: 3.2
      Accept only bytestring or bytearray objects as input.


.. function:: rlecode_hqx(data)

   Perform binhex4 style RLE-compression on *data* and return the result.


.. function:: b2a_hqx(data)

   Perform hexbin4 binary-to-ASCII translation and return the resulting string. The
   argument should already be RLE-coded, and have a length divisible by 3 (except
   possibly the last fragment).

   执行hexbin4二进制到ASCII码翻译，并返回结果字符串。 “
   参数应该已经RLE编码，并已被3整除的长度（除
   可能是最后一个片段）。


.. function:: crc_hqx(data, crc)

   Compute the binhex4 crc value of *data*, starting with an initial *crc* and
   returning the result.

   计算binhex4 CRC值*数据*，启动和初始* CRC*返回结果。


.. function:: crc32(data[, crc])

   Compute CRC-32, the 32-bit checksum of data, starting with an initial crc.  This
   is consistent with the ZIP file checksum.  Since the algorithm is designed for
   use as a checksum algorithm, it is not suitable for use as a general hash
   algorithm.  Use as follows::

   的32位校验和数据，计算的CRC - 32，从最初的CRC。这
   与ZIP文件的校验和相一致。由于算法的设计
   使用一个校验和算法，它是不适合作为一般的哈希
   算法。使用如下::

      print(binascii.crc32(b"hello world"))
      # Or, in two pieces:
      crc = binascii.crc32(b"hello")
      crc = binascii.crc32(b" world", crc) & 0xffffffff
      print('crc32 = {:#010x}'.format(crc))

.. note::
   To generate the same numeric value across all Python versions and
   platforms use crc32(data) & 0xffffffff.  If you are only using
   the checksum in packed binary format this is not necessary as the
   return value is the correct 32bit binary representation
   regardless of sign.

   要生成所有Python版本相同的数值，
   平台使用CRC32（数据）为0xffffffff。如果你只使用
   在包装的二进制格式的校验，这是没有必要
   返回值是正确的32位二进制表示
   不管标志。


.. function:: b2a_hex(data)
              hexlify(data)

   Return the hexadecimal representation of the binary *data*.  Every byte of
   *data* is converted into the corresponding 2-digit hex representation.  The
   resulting string is therefore twice as long as the length of *data*.

   返回十六进制表示的二进制数据**每一个字节
   *数据转换成相应的2位数的十六进制表示。 “
   因此，得到的字符串*数据的长度的两倍长*.


.. function:: a2b_hex(hexstr)
              unhexlify(hexstr)

   Return the binary data represented by the hexadecimal string *hexstr*.  This
   function is the inverse of :func:`b2a_hex`. *hexstr* must contain an even number
   of hexadecimal digits (which can be upper or lower case), otherwise a
   :exc:`TypeError` is raised.

   返回的十六进制字符串表示的二进制数据* hexstr*这
   函数是逆：FUNC：`b2a_hex`。 * hexstr*必须包含偶数
   十六进制数字（可以是大写或小写），否则
   ：商务顾客：`TypeError异常`引发。

   .. versionchanged:: 3.2
      Accept only bytestring or bytearray objects as input.


.. exception:: Error

   Exception raised on errors. These are usually programming errors.


.. exception:: Incomplete

   Exception raised on incomplete data. These are usually not programming errors,
   but may be handled by reading a little more data and trying again.

   不完整的数据异常。这些通常是不编程错误，
   但可能是处理读多一点的数据，并再次尝试。


.. seealso::

   Module :mod:`base64`
      Support for base64 encoding used in MIME email messages.

   Module :mod:`binhex`
      Support for the binhex format used on the Macintosh.

   Module :mod:`uu`
      Support for UU encoding used on Unix.

   Module :mod:`quopri`
      Support for quoted-printable encoding used in MIME email messages.
