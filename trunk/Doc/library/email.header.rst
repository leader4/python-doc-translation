:mod:`email`: 国际化处理
---------------------------------------

.. module:: email.header
   :synopsis: Representing non-ASCII headers

.. 模块:: email.header
    :简介:主要描述非ASCII格式的header

:rfc:`2822` is the base standard that describes the format of email messages.
It derives from the older :rfc:`822` standard which came into widespread use at
a time when most email was composed of ASCII characters only.  :rfc:`2822` is a
specification written assuming email contains only 7-bit ASCII characters.

:rfc:`2822`描述了电子邮件格式的基本标准. 
它源于上一个版本:rfc:`822`的标准,该版本来源于当时大量使用的仅由ASCII字符组合的电子邮件. 
:rfc:`2822`是一个假设只包含7位ASCII字符电子邮件的规范. 

Of course, as email has been deployed worldwide, it has become
internationalized, such that language specific character sets can now be used in
email messages.  The base standard still requires email messages to be
transferred using only 7-bit ASCII characters, so a slew of RFCs have been
written describing how to encode email containing non-ASCII characters into
:rfc:`2822`\ -compliant format. These RFCs include :rfc:`2045`, :rfc:`2046`,
:rfc:`2047`, and :rfc:`2231`. The :mod:`email` package supports these standards
in its :mod:`email.header` and :mod:`email.charset` modules.

当然,由于电子邮件已风菲全球并成为国际化,因此该语言规范字符集能够应用到电子邮件中. 
基本标准仍然需要使用只有7位的ASCII字符传输电子邮件,因此RFCs已描述了如何对含有非ASCII字符的电子邮件编码为:rfc:`2822`\ -compliant格式. 
这些RFCs包含:rfc:`2045`,:rfc:`2046`,:rfc:`2047`和:rfc:`2231`. :mod:`email`包中的:mod:`email.header`和:mod:`email.charset`模块支持这些标准. 

If you want to include non-ASCII characters in your email headers, say in the
:mailheader:`Subject` or :mailheader:`To` fields, you should use the
:class:`Header` class and assign the field in the :class:`~email.message.Message`
object to an instance of :class:`Header` instead of using a string for the header
value.  Import the :class:`Header` class from the :mod:`email.header` module.
For example:

如果要在电子邮件标题中包含非ASCII字符,那么应该在:mailheader:`Subject`或:mailheader:`To`中使用:class:`Header`类,
并且将:class:`~email.message.Message`对象赋值给:class:`Header`实例,而不是使用字符串. 
从:mod:`email.header`模块导入 :class:`Header`类. 
例如::

   >>> from email.message import Message
   >>> from email.header import Header
   >>> msg = Message()
   >>> h = Header('p\xf6stal', 'iso-8859-1')
   >>> msg['Subject'] = h
   >>> print(msg.as_string())
   Subject: =?iso-8859-1?q?p=F6stal?=



Notice here how we wanted the :mailheader:`Subject` field to contain a non-ASCII
character?  We did this by creating a :class:`Header` instance and passing in
the character set that the byte string was encoded in.  When the subsequent
:class:`~email.message.Message` instance was flattened, the :mailheader:`Subject`
field was properly :rfc:`2047` encoded.  MIME-aware mail readers would show this
header using the embedded ISO-8859-1 character.

注意,如何在:mailheader:`Subject`中包含非ASCII字符?
通过创建一个:class:`Header`实例并传递已编码的字节串的字符集,当:class:`~email.message.Message`实例扁平化时,:mailheader:`Subject`
正好被:rfc:`2047`编码. MIME-aware电子邮件readers将会显示已嵌入式的 ISO-8859-1字符的头. 

Here is the :class:`Header` class description:

下面是:class:`Header`类的描述: 

.. class:: Header(s=None, charset=None, maxlinelen=None, header_name=None, continuation_ws=' ', errors='strict')

   Create a MIME-compliant header that can contain strings in different character
   sets.

   创建一个可以包含在不同的字符的字符串集的MIME-compliant头部. 

   Optional *s* is the initial header value.  If ``None`` (the default), the
   initial header value is not set.  You can later append to the header with
   :meth:`append` method calls.  *s* may be an instance of :class:`bytes` or
   :class:`str`, but see the :meth:`append` documentation for semantics.
   
   选项*s*是初始化的头. ``None``即未设置 (默认值) . 可以在后面使用 :meth:`append`方法添加. 
   *s*可能是:class:`bytes`的实例或:class:`str`,具体请参考:meth:`append`文档. 

   Optional *charset* serves two purposes: it has the same meaning as the *charset*
   argument to the :meth:`append` method.  It also sets the default character set
   for all subsequent :meth:`append` calls that omit the *charset* argument.  If
   *charset* is not provided in the constructor (the default), the ``us-ascii``
   character set is used both as *s*'s initial charset and as the default for
   subsequent :meth:`append` calls.
   
   选项*charset*有两个目的: 它具有相同的含义*charset*参数:meth:`append`方法. 
   它还设置了默认的字符集所有后续: :meth:`append`省略的*charset*参数的调用. 
   *charset*中没有提供构造函数 (默认) ,``us-ascii``
   *s*的初始字符集作为默认字符集是用来后续:meth:`append`调用. 

   The maximum line length can be specified explicitly via *maxlinelen*.  For
   splitting the first line to a shorter value (to account for the field header
   which isn't included in *s*, e.g. :mailheader:`Subject`) pass in the name of the
   field in *header_name*.  The default *maxlinelen* is 76, and the default value
   for *header_name* is ``None``, meaning it is not taken into account for the
   first line of a long, split header.
   
   最大行的长度通过变量*maxlinelen*明确指出. 拆分第一行为较短的值
    (占字段的header不包括在*s*中,例如: :mailheader:`Subject`) 在*header_name*中传递字段名称. 
   *maxlinelen*默认为76,*header_name*默认值是``None``,意味着不考虑第一个很长的拆分header的行. 


   Optional *continuation_ws* must be :rfc:`2822`\ -compliant folding
   whitespace, and is usually either a space or a hard tab character.  This
   character will be prepended to continuation lines.  *continuation_ws*
   defaults to a single space character.
   
   选项*continuation_ws*必须是:rfc:`2822`\ -compliant的折叠空白,通常是空格或硬制表符. 
   这字符将被置于续行. *continuation_ws*默认为一个空格字符. 

   Optional *errors* is passed straight through to the :meth:`append` method.
   
   选项*errors*通过:meth:`append`方法添加. 

   .. method:: append(s, charset=None, errors='strict')

      Append the string *s* to the MIME header.
      
      追加字符串*s*到MIME header. 

      Optional *charset*, if given, should be a :class:`~email.charset.Charset`
      instance (see :mod:`email.charset`) or the name of a character set, which
      will be converted to a :class:`~email.charset.Charset` instance.  A value
      of ``None`` (the default) means that the *charset* given in the constructor
      is used.

      选项*charset*,如果给定的,应该是一个:class:`~email.charset..Charset`
      实例 (请参考:mod:`email.charset`) 或字符集的名称,这将被转换为一个:class:`~email.charset.Charset`实例. 
       ``None``值 (默认) 意味着*charset*已在构造函数中被使用. 

      *s* may be an instance of :class:`bytes` or :class:`str`.  If it is an
      instance of :class:`bytes`, then *charset* is the encoding of that byte
      string, and a :exc:`UnicodeError` will be raised if the string cannot be
      decoded with that character set.

      *s*可能是一个:class:`bytes`或:class:`str`的实例. 如果是一个:class:`bytes`实例,那么*charset*是该字节的编码字符串,
      如果字符串不能解码该字符集,就会抛出:exc:`UnicodeError`异常. 

      If *s* is an instance of :class:`str`, then *charset* is a hint specifying
      the character set of the characters in the string.

      如果*s*是:class:`str`实例,然后*charset*是一个提示指定字符串中的字符的字符集. 

      In either case, when producing an :rfc:`2822`\ -compliant header using
      :rfc:`2047` rules, the string will be encoded using the output codec of
      the charset.  If the string cannot be encoded using the output codec, a
      UnicodeError will be raised.

      在这两种情况下,当使用:rfc:`2047`规则命名:rfc:`2822`\ -compliant头部,该字符串将被编码使用输出编解码器字符集. 
      如果该字符串不能使用output codec进行编码,就会抛出UnicodeError异常. 

      Optional *errors* is passed as the errors argument to the decode call
      if *s* is a byte string.

      如果*s*是字节字符串,那么会传递错误参数选项*errors*到解码调用. 


   .. method:: encode(splitchars=';, \\t', maxlinelen=None, linesep='\\n')

      Encode a message header into an RFC-compliant format, possibly wrapping
      long lines and encapsulating non-ASCII parts in base64 or quoted-printable
      encodings.  Optional *splitchars* is a string containing characters to
      split long ASCII lines on, in rough support of :rfc:`2822`'s *highest
      level syntactic breaks*.  This doesn't affect :rfc:`2047` encoded lines.

      编码成一个RFC兼容的格式的消息头,可能在base64或quoted-printable编码中,封装最大行数和非ASCII部分. 
      选项*splitchars*是一个字符串,其中包含的字符拆分成很长的ASCII行,在粗糙的支持最高:rfc:`2822`的*highestlevel syntactic breaks*. 
      这并不影响:rfc:`2047`编码行. 

      *maxlinelen*, if given, overrides the instance's value for the maximum
      line length.

      *maxlinelen*,如果给定的,覆盖实例的最大值行的长度. 

      *linesep* specifies the characters used to separate the lines of the
      folded header.  It defaults to the most useful value for Python
      application code (``\n``), but ``\r\n`` can be specified in order
      to produce headers with RFC-compliant line separators.

      *linesep*指定用于分隔的行的字符折叠头. 它默认为Python的最有用的价值应用程序代码 (``\n``) ,但``\r\n``可以按顺序指定
      生产与RFC兼容的行分隔符头. 

      .. versionchanged:: 3.2
         Added the *linesep* argument.

      .. 3.2版本改变
         新增 *linesep*参数. 


   The :class:`Header` class also provides a number of methods to support
   standard operators and built-in functions.

   :class:`Header`类还提供了一些方法,以支持标准的操作符和内置函数. 

   .. method:: __str__()

      Returns an approximation of the :class:`Header` as a string, using an
      unlimited line length.  All pieces are converted to unicode using the
      specified encoding and joined together appropriately.  Any pieces with a
      charset of `unknown-8bit` are decoded as `ASCII` using the `replace`
      error handler.

      :class:`Header`作为一个字符串返回一个近似,使用无限制的行长度. 
      销售所有货件都转换为Unicode的使用. 指定的编码,并适当结合在一起. 
      任何与字符集解码的ASCII`使用`取代`unknown-8bit`错误处理程序. 

      .. versionchanged:: 3.2
         Added handling for the `unknown-8bit` charset.

      .. 3.2版本改变
         补充`unknown-8bit`字符集的处理. 


   .. method:: __eq__(other)

      This method allows you to compare two :class:`Header` instances for
      equality.

      该方法用于比较两个:class:`Header`的实例是否相等. 

   .. method:: __ne__(other)

      This method allows you to compare two :class:`Header` instances for
      inequality.

      该方法用于比较两个:class:`Header`实例是否不相等. 

The :mod:`email.header` module also provides the following convenient functions.

:mod:`email.header`模块还提供了以下方便的功能. 


.. function:: decode_header(header)

   Decode a message header value without converting the character set. The header
   value is in *header*.

   对一个无须转换字符集的在*header*中的消息头进行解码. 

   This function returns a list of ``(decoded_string, charset)`` pairs containing
   each of the decoded parts of the header.  *charset* is ``None`` for non-encoded
   parts of the header, otherwise a lower case string containing the name of the
   character set specified in the encoded string.

   这个函数返回一组含有每个解码部分头的``(decoded_string, charset)``对.  *charset*是``None``非编码
   头的部分,否则小写的字符串,其中包含的名称字符集编码的字符串中指定. 

   Here's an example::

      >>> from email.header import decode_header
      >>> decode_header('=?iso-8859-1?q?p=F6stal?=')
      [('p\xf6stal', 'iso-8859-1')]


.. function:: make_header(decoded_seq, maxlinelen=None, header_name=None, continuation_ws=' ')

   Create a :class:`Header` instance from a sequence of pairs as returned by
   :func:`decode_header`.

   :func:`decode_header`从序列对中返回一个新建的:class:`Header`实例. 

   :func:`decode_header` takes a header value string and returns a sequence of
   pairs of the format ``(decoded_string, charset)`` where *charset* is the name of
   the character set.

   :func:`decode_header`需要一个头值的字符串并返回一个``(decoded_string, charset)``序列对,其中*charset*是字符集名称. 

   This function takes one of those sequence of pairs and returns a
   :class:`Header` instance.  Optional *maxlinelen*, *header_name*, and
   *continuation_ws* are as in the :class:`Header` constructor.

   此功能需要那些序列对,并返回一个:class:`Header`实例. 选项*maxlinelen*,*header_name*
   *continuation_ws*与:class:`Header`构造函数一致. 


