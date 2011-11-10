
:mod:`netrc` --- netrc 文件处理
======================================

.. module:: netrc
   :synopsis: Loading of .netrc files.
.. moduleauthor:: Eric S. Raymond <esr@snark.thyrsus.com>
.. sectionauthor:: Eric S. Raymond <esr@snark.thyrsus.com>

**Source code:** :source:`Lib/netrc.py`
**源代码:** Lib/netrc.py
--------------

The :class:`netrc` class parses and encapsulates the netrc file format used by
the Unix :program:`ftp` program and other FTP clients.

``netrc`` 类分析和封装netrc文件格式, 用于Unix **ftp** 编程和其它的 FTP客户端.


.. class:: netrc([file])

   A :class:`netrc` instance or subclass instance encapsulates data from  a netrc
   file.  The initialization argument, if present, specifies the file to parse.  If
   no argument is given, the file :file:`.netrc` in the user's home directory will
   be read.  Parse errors will raise :exc:`NetrcParseError` with diagnostic
   information including the file name, line number, and terminating token.
   
    一个``netrc``实例或子类的实例封装一个netrc文件的数据. 如果提供初始化参数, 初始化
   参数为一个文件用于分析. 如果没有提供参数, 在用户home目录下面的``.netrc``将被读取.  
   分析错误将抛出``NetrcParseError``, 它提供了包含名字, 行数, 中断标志的诊断信息. 


.. exception:: NetrcParseError

   Exception raised by the :class:`netrc` class when syntactical errors are
   encountered in source text.  Instances of this exception provide three
   interesting attributes:  :attr:`msg` is a textual explanation of the error,
   :attr:`filename` is the name of the source file, and :attr:`lineno` gives the
   line number on which the error was found.
   
    当源文本中出现语法错误, ``netrc`` class抛出异常. 这个异常的实例提供3个有趣的属性:  
   ``msg``是错误的一个文字性描述, ``filename``是源文件的文件名,  ``lineno``给出
   发现错误的行号


.. _netrc-objects:

netrc Objects
-------------

A :class:`netrc` instance has the following methods:
一个``netrc``有以下方法:


.. method:: netrc.authenticators(host)

   Return a 3-tuple ``(login, account, password)`` of authenticators for *host*.
   If the netrc file did not contain an entry for the given host, return the tuple
   associated with the 'default' entry.  If neither matching host nor default entry
   is available, return ``None``.
   
   返回一个*host*身份认证的3元素元组``(login, account, password)``. 如果netrc
   文件没有包含给定主机的项, 返回关联默认项的元组. 如果既没有匹配的主机, 也没有可用的默认项,
   返回``None``.
   


.. method:: netrc.__repr__()

   Dump the class data as a string in the format of a netrc file. (This discards
   comments and may reorder the entries.)
   
    把类数据以一个字符串形式存贮到一个netrc格式的文件. 
    (存贮的数据可能重新排序)
   

Instances of :class:`netrc` have public instance variables:

``netrc``实例的公共实例变量:


.. attribute:: netrc.hosts

   Dictionary mapping host names to ``(login, account, password)`` tuples.  The
   'default' entry, if any, is represented as a pseudo-host by that name.
   
    一个主机名到``(login, account, password)``元组映射的字典. 如果有默认值, 
   表示提供一个伪主机到名字的隐射字典. 


.. attribute:: netrc.macros

   Dictionary mapping macro names to string lists.
   
    隐射宏名字到字符串列表的字典. 
    

.. note::

   Passwords are limited to a subset of the ASCII character set.  All ASCII
   punctuation is allowed in passwords, however, note that whitespace and
   non-printable characters are not allowed in passwords.  This is a limitation
   of the way the .netrc file is parsed and may be removed in the future.
   
   注意: 密码是ASCII字符集中的一个有限的子集. 所有的ASCII标点都被允许出现在密码中. 但是, 
注意空白字符和非打印字符不允许出现在密码中. 这是一个对 .netrc 文件分析的限制, 可能在
未来被移除. 


