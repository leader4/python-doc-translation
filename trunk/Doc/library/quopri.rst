:mod:`quopri` --- 编码解码 MIME quoted-printable 数据 
==============================================================

.. module:: quopri
   :synopsis: Encode and decode files using the MIME quoted-printable encoding.


.. index::
   pair: quoted-printable; encoding
   single: MIME; quoted-printable encoding

**Source code:** :source:`Lib/quopri.py`

--------------

This module performs quoted-printable transport encoding and decoding, as
defined in :rfc:`1521`: "MIME (Multipurpose Internet Mail Extensions) Part One:
Mechanisms for Specifying and Describing the Format of Internet Message Bodies".
The quoted-printable encoding is designed for data where there are relatively
few nonprintable characters; the base64 encoding scheme available via the
:mod:`base64` module is more compact if there are many such characters, as when
sending a graphics file.

此模块执行运输使用quoted - printable编码和解码, 如RFC 1521中定义: ``的MIME (多用途Internet邮件扩展) 
第一部分: 指定和描述Internet消息体的格式的机制 ". . 
使用quoted - printable编码的目的是为有相对较少的非打印字符的数据;
base64编码方案通过的base64模块提供更紧凑, 如果有很多这样的字符发送图形文件时, . 


.. function:: decode(input, output, header=False)

解码 (输入, 输出, [头]) 

   Decode the contents of the *input* file and write the resulting decoded binary
   data to the *output* file. *input* and *output* must be :term:`file objects
   <file object>`.  *input* will be read until ``input.readline()`` returns an
   empty string. If the optional argument *header* is present and true, underscore
   will be decoded as space. This is used to decode "Q"-encoded headers as
   described in :rfc:`1522`: "MIME (Multipurpose Internet Mail Extensions)
   Part Two: Message Header Extensions for Non-ASCII Text".

   解码输入文件中的内容, 写解码产生的二进制数据输出文件. 
   输入和输出必须是文件对象或文件对象接口的对象, 模仿. 将读取输入, 直到input.readline () 
   返回一个空字符串. 如果可选参数头是当前和真实, 强调将作为空间解码. 这是用来解码 "Q''编码的头在RFC 1522中描述的: " MIME (多用途Internet邮件扩展) 第二部分: 非ASCII文本 "消息头扩展. 


.. function:: encode(input, output, quotetabs, header=False)

编码 (输入, 输出, quotetabs) 


   Encode the contents of the *input* file and write the resulting quoted-printable
   data to the *output* file. *input* and *output* must be :term:`file objects
   <file object>`.  *input* will be read until ``input.readline()`` returns an
   empty string. *quotetabs* is a flag which controls whether to encode embedded
   spaces and tabs; when true it encodes such embedded whitespace, and when
   false it leaves them unencoded.  Note that spaces and tabs appearing at the
   end of lines are always encoded, as per :rfc:`1521`.  *header* is a flag
   which controls if spaces are encoded as underscores as per :rfc:`1522`.

   编码输入文件的内容, 并写由此产生的数据到输出文件中使用quoted - printable. 
   输入和输出必须是文件对象或文件对象接口的对象, 模仿. 
   将读取输入, 直到input.readline () 返回一个空字符串. 
    quotetabs是一个标志, 它控制是否编码嵌入空格和制表符, 当它真正的编码等嵌入空白, 
   并离开他们未编码时的虚假. 请注意, 线的末端出现的空格和制表符总是每个RFC 1521编码, . 


.. function:: decodestring(s, header=False)

decodestring ([头]) 

   Like :func:`decode`, except that it accepts a source string and returns the
   corresponding decoded string.

   像DECODE () , 但它接受一个源字符串, 并返回相应的的解码的字符串. 


.. function:: encodestring(s, quotetabs=False, header=False)

   Like :func:`encode`, except that it accepts a source string and returns the
   corresponding encoded string.  *quotetabs* and *header* are optional
   (defaulting to ``False``), and are passed straight through to :func:`encode`.

   一样的encode () , 除非它接受一个源字符串, 并返回相应的编码的字符串.  quotetabs是可选的 (默认为0) , 并通过直通编码 () . 


.. seealso::
另请参见: 

   Module :mod:`base64`
      Encode and decode MIME base64 data

      模块mimify: 
MIME邮件处理一般事业. 
模块BASE64: 
编码和解码的MIME base64数据. 

朗读
显示对应的拉丁字符的拼音字典

