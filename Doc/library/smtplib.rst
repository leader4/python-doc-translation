:mod:`smtplib` --- SMTP 协议客户端
=======================================

.. module:: smtplib
   :synopsis: SMTP protocol client (requires sockets).
.. sectionauthor:: Eric S. Raymond <esr@snark.thyrsus.com>


.. index::
   pair: SMTP; protocol
   single: Simple Mail Transfer Protocol

**Source code:** :source:`Lib/smtplib.py`

--------------

The :mod:`smtplib` module defines an SMTP client session object that can be used
to send mail to any Internet machine with an SMTP or ESMTP listener daemon.  For
details of SMTP and ESMTP operation, consult :rfc:`821` (Simple Mail Transfer
Protocol) and :rfc:`1869` (SMTP Service Extensions).


.. class:: SMTP(host='', port=0, local_hostname=None[, timeout])

   A :class:`SMTP` instance encapsulates an SMTP connection.  It has methods
   that support a full repertoire of SMTP and ESMTP operations. If the optional
   host and port parameters are given, the SMTP :meth:`connect` method is called
   with those parameters during initialization.  An :exc:`SMTPConnectError` is
   raised if the specified host doesn't respond correctly. The optional
   *timeout* parameter specifies a timeout in seconds for blocking operations
   like the connection attempt (if not specified, the global default timeout
   setting will be used).

   For normal use, you should only require the initialization/connect,
   :meth:`sendmail`, and :meth:`quit` methods.  An example is included below.


.. class:: SMTP_SSL(host='', port=0, local_hostname=None, keyfile=None, certfile=None[, timeout])

   A :class:`SMTP_SSL` instance behaves exactly the same as instances of
   :class:`SMTP`. :class:`SMTP_SSL` should be used for situations where SSL is
   required from the beginning of the connection and using :meth:`starttls` is
   not appropriate. If *host* is not specified, the local host is used. If
   *port* is zero, the standard SMTP-over-SSL port (465) is used. *keyfile*
   and *certfile* are also optional, and can contain a PEM formatted private key
   and certificate chain file for the SSL connection. The optional *timeout*
   parameter specifies a timeout in seconds for blocking operations like the
   connection attempt (if not specified, the global default timeout setting
   will be used).


.. class:: LMTP(host='', port=LMTP_PORT, local_hostname=None)

   The LMTP protocol, which is very similar to ESMTP, is heavily based on the
   standard SMTP client. It's common to use Unix sockets for LMTP, so our :meth:`connect`
   method must support that as well as a regular host:port server. To specify a
   Unix socket, you must use an absolute path for *host*, starting with a '/'.

   Authentication is supported, using the regular SMTP mechanism. When using a Unix
   socket, LMTP generally don't support or require any authentication, but your
   mileage might vary.


A nice selection of exceptions is defined as well:


.. exception:: SMTPException

   Base exception class for all exceptions raised by this module.


.. exception:: SMTPServerDisconnected

   This exception is raised when the server unexpectedly disconnects, or when an
   attempt is made to use the :class:`SMTP` instance before connecting it to a
   server.


.. exception:: SMTPResponseException

   Base class for all exceptions that include an SMTP error code. These exceptions
   are generated in some instances when the SMTP server returns an error code.  The
   error code is stored in the :attr:`smtp_code` attribute of the error, and the
   :attr:`smtp_error` attribute is set to the error message.


.. exception:: SMTPSenderRefused

   Sender address refused.  In addition to the attributes set by on all
   :exc:`SMTPResponseException` exceptions, this sets 'sender' to the string that
   the SMTP server refused.


.. exception:: SMTPRecipientsRefused

   All recipient addresses refused.  The errors for each recipient are accessible
   through the attribute :attr:`recipients`, which is a dictionary of exactly the
   same sort as :meth:`SMTP.sendmail` returns.


.. exception:: SMTPDataError

   The SMTP server refused to accept the message data.


.. exception:: SMTPConnectError

   Error occurred during establishment of a connection  with the server.


.. exception:: SMTPHeloError

   The server refused our ``HELO`` message.


.. exception:: SMTPAuthenticationError

   SMTP authentication went wrong.  Most probably the server didn't accept the
   username/password combination provided.


.. seealso::

   :rfc:`821` - Simple Mail Transfer Protocol
      Protocol definition for SMTP.  This document covers the model, operating
      procedure, and protocol details for SMTP.

   :rfc:`1869` - SMTP Service Extensions
      Definition of the ESMTP extensions for SMTP.  This describes a framework for
      extending SMTP with new commands, supporting dynamic discovery of the commands
      provided by the server, and defines a few additional commands.


.. _smtp-objects:

SMTP Objects
------------

An :class:`SMTP` instance has the following methods:


.. method:: SMTP.set_debuglevel(level)

   Set the debug output level.  A true value for *level* results in debug messages
   for connection and for all messages sent to and received from the server.


.. method:: SMTP.connect(host='localhost', port=0)

   Connect to a host on a given port.  The defaults are to connect to the local
   host at the standard SMTP port (25). If the hostname ends with a colon (``':'``)
   followed by a number, that suffix will be stripped off and the number
   interpreted as the port number to use. This method is automatically invoked by
   the constructor if a host is specified during instantiation.


.. method:: SMTP.docmd(cmd, args='')

   Send a command *cmd* to the server.  The optional argument *args* is simply
   concatenated to the command, separated by a space.

   This returns a 2-tuple composed of a numeric response code and the actual
   response line (multiline responses are joined into one long line.)

   In normal operation it should not be necessary to call this method explicitly.
   It is used to implement other methods and may be useful for testing private
   extensions.

   If the connection to the server is lost while waiting for the reply,
   :exc:`SMTPServerDisconnected` will be raised.


.. method:: SMTP.helo(name='')

   Identify yourself to the SMTP server using ``HELO``.  The hostname argument
   defaults to the fully qualified domain name of the local host.
   The message returned by the server is stored as the :attr:`helo_resp` attribute
   of the object.

   In normal operation it should not be necessary to call this method explicitly.
   It will be implicitly called by the :meth:`sendmail` when necessary.


.. method:: SMTP.ehlo(name='')

   Identify yourself to an ESMTP server using ``EHLO``.  The hostname argument
   defaults to the fully qualified domain name of the local host.  Examine the
   response for ESMTP option and store them for use by :meth:`has_extn`.
   Also sets several informational attributes: the message returned by
   the server is stored as the :attr:`ehlo_resp` attribute, :attr:`does_esmtp`
   is set to true or false depending on whether the server supports ESMTP, and
   :attr:`esmtp_features` will be a dictionary containing the names of the
   SMTP service extensions this server supports, and their
   parameters (if any).

   Unless you wish to use :meth:`has_extn` before sending mail, it should not be
   necessary to call this method explicitly.  It will be implicitly called by
   :meth:`sendmail` when necessary.

.. method:: SMTP.ehlo_or_helo_if_needed()

   This method call :meth:`ehlo` and or :meth:`helo` if there has been no
   previous ``EHLO`` or ``HELO`` command this session.  It tries ESMTP ``EHLO``
   first.

   :exc:`SMTPHeloError`
     The server didn't reply properly to the ``HELO`` greeting.

.. method:: SMTP.has_extn(name)

   Return :const:`True` if *name* is in the set of SMTP service extensions returned
   by the server, :const:`False` otherwise. Case is ignored.


.. method:: SMTP.verify(address)

   Check the validity of an address on this server using SMTP ``VRFY``. Returns a
   tuple consisting of code 250 and a full :rfc:`822` address (including human
   name) if the user address is valid. Otherwise returns an SMTP error code of 400
   or greater and an error string.

   .. note::

      Many sites disable SMTP ``VRFY`` in order to foil spammers.


.. method:: SMTP.login(user, password)

   Log in on an SMTP server that requires authentication. The arguments are the
   username and the password to authenticate with. If there has been no previous
   ``EHLO`` or ``HELO`` command this session, this method tries ESMTP ``EHLO``
   first. This method will return normally if the authentication was successful, or
   may raise the following exceptions:

   :exc:`SMTPHeloError`
      The server didn't reply properly to the ``HELO`` greeting.

   :exc:`SMTPAuthenticationError`
      The server didn't accept the username/password combination.

   :exc:`SMTPException`
      No suitable authentication method was found.


.. method:: SMTP.starttls(keyfile=None, certfile=None)

   Put the SMTP connection in TLS (Transport Layer Security) mode.  All SMTP
   commands that follow will be encrypted.  You should then call :meth:`ehlo`
   again.

   If *keyfile* and *certfile* are provided, these are passed to the :mod:`socket`
   module's :func:`ssl` function.

   If there has been no previous ``EHLO`` or ``HELO`` command this session,
   this method tries ESMTP ``EHLO`` first.

   :exc:`SMTPHeloError`
      The server didn't reply properly to the ``HELO`` greeting.

   :exc:`SMTPException`
     The server does not support the STARTTLS extension.

   :exc:`RuntimeError`
     SSL/TLS support is not available to your Python interpreter.


.. method:: SMTP.sendmail(from_addr, to_addrs, msg, mail_options=[], rcpt_options=[])

   Send mail.  The required arguments are an :rfc:`822` from-address string, a list
   of :rfc:`822` to-address strings (a bare string will be treated as a list with 1
   address), and a message string.  The caller may pass a list of ESMTP options
   (such as ``8bitmime``) to be used in ``MAIL FROM`` commands as *mail_options*.
   ESMTP options (such as ``DSN`` commands) that should be used with all ``RCPT``
   commands can be passed as *rcpt_options*.  (If you need to use different ESMTP
   options to different recipients you have to use the low-level methods such as
   :meth:`mail`, :meth:`rcpt` and :meth:`data` to send the message.)

   .. note::

      The *from_addr* and *to_addrs* parameters are used to construct the message
      envelope used by the transport agents.  ``sendmail`` does not modify the
      message headers in any way.

   msg may be a string containing characters in the ASCII range, or a byte
   string.  A string is encoded to bytes using the ascii codec, and lone ``\r``
   and ``\n`` characters are converted to ``\r\n`` characters.  A byte string
   is not modified.

   If there has been no previous ``EHLO`` or ``HELO`` command this session, this
   method tries ESMTP ``EHLO`` first. If the server does ESMTP, message size and
   each of the specified options will be passed to it (if the option is in the
   feature set the server advertises).  If ``EHLO`` fails, ``HELO`` will be tried
   and ESMTP options suppressed.

   This method will return normally if the mail is accepted for at least one
   recipient. Otherwise it will raise an exception.  That is, if this method does
   not raise an exception, then someone should get your mail. If this method does
   not raise an exception, it returns a dictionary, with one entry for each
   recipient that was refused.  Each entry contains a tuple of the SMTP error code
   and the accompanying error message sent by the server.

   This method may raise the following exceptions:

   :exc:`SMTPRecipientsRefused`
      All recipients were refused.  Nobody got the mail.  The :attr:`recipients`
      attribute of the exception object is a dictionary with information about the
      refused recipients (like the one returned when at least one recipient was
      accepted).

   :exc:`SMTPHeloError`
      The server didn't reply properly to the ``HELO`` greeting.

   :exc:`SMTPSenderRefused`
      The server didn't accept the *from_addr*.

   :exc:`SMTPDataError`
      The server replied with an unexpected error code (other than a refusal of a
      recipient).

   Unless otherwise noted, the connection will be open even after an exception is
   raised.

   .. versionchanged:: 3.2 *msg* may be a byte string.


.. method:: SMTP.send_message(msg, from_addr=None, to_addrs=None, mail_options=[], rcpt_options=[])

   This is a convenience method for calling :meth:`sendmail` with the message
   represented by an :class:`email.message.Message` object.  The arguments have
   the same meaning as for :meth:`sendmail`, except that *msg* is a ``Message``
   object.

   If *from_addr* is ``None``, ``send_message`` sets its value to the value of
   the :mailheader:`From` header from *msg*.  If *to_addrs* is ``None``,
   ``send_message`` combines the values (if any) of the :mailheader:`To`,
   :mailheader:`CC`, and :mailheader:`Bcc` fields from *msg*.  Regardless of
   the values of *from_addr* and *to_addrs*, ``send_message`` deletes any  Bcc
   field from *msg*.  It then serializes *msg* using
   :class:`~email.generator.BytesGenerator` with ``\r\n`` as the *linesep*, and
   calls :meth:`sendmail` to transmit the resulting message.

   .. versionadded:: 3.2


.. method:: SMTP.quit()

   Terminate the SMTP session and close the connection.  Return the result of
   the SMTP ``QUIT`` command.


Low-level methods corresponding to the standard SMTP/ESMTP commands ``HELP``,
``RSET``, ``NOOP``, ``MAIL``, ``RCPT``, and ``DATA`` are also supported.
Normally these do not need to be called directly, so they are not documented
here.  For details, consult the module code.


.. _smtp-example:

SMTP Example
------------

This example prompts the user for addresses needed in the message envelope ('To'
and 'From' addresses), and the message to be delivered.  Note that the headers
to be included with the message must be included in the message as entered; this
example doesn't do any processing of the :rfc:`822` headers.  In particular, the
'To' and 'From' addresses must be included in the message headers explicitly. ::

   import smtplib

   def prompt(prompt):
       return input(prompt).strip()

   fromaddr = prompt("From: ")
   toaddrs  = prompt("To: ").split()
   print("Enter message, end with ^D (Unix) or ^Z (Windows):")

   # Add the From: and To: headers at the start!
   msg = ("From: %s\r\nTo: %s\r\n\r\n"
          % (fromaddr, ", ".join(toaddrs)))
   while True:
       try:
           line = input()
       except EOFError:
           break
       if not line:
           break
       msg = msg + line

   print("Message length is", len(msg))

   server = smtplib.SMTP('localhost')
   server.set_debuglevel(1)
   server.sendmail(fromaddr, toaddrs, msg)
   server.quit()

.. note::

   In general, you will want to use the :mod:`email` package's features to
   construct an email message, which you can then send
   via :meth:`~smtplib.SMTP.send_message`; see :ref:`email-examples`.
   
###################
smtplib.SMTP([host[, port[, local_hostname[, timeout]]]])

　　SMTP类构造函数, 表示与SMTP服务器之间的连接, 通过这个连接我们可以向smtp服务器发送指令, 执行相关操作 (如: 登陆、发送邮件) . 该类提供了许多方法, 将在下面介绍. 它的所有参数都是可选的, 其中host参数表示smtp服务器主机名, 上面例子中的smtp主机为"smtp.yeah.net"; port表示smtp服务的端口, 默认是25; 如果在创建SMTP对象的时候提供了这两个参数, 在初始化的时候会自动调用connect方法去连接服务器. 
　　smtplib模块还提供了SMTP_SSL类和LMTP类, 对它们的操作与SMTP基本一致. 
　　smtplib.SMTP提供的方法: 
SMTP.set_debuglevel(level)

　　设置是否为调试模式. 默认为False, 即非调试模式, 表示不输出任何调试信息. 
SMTP.connect([host[, port]])

　　连接到指定的smtp服务器. 参数分别表示smpt主机和端口. 注意: 也可以在host参数中指定端口号 (如: smpt.yeah.net:25) , 这样就没必要给出port参数. 
SMTP.docmd(cmd[, argstring])

　　向smtp服务器发送指令. 可选参数argstring表示指令的参数. 下面的例子完全通过调用docmd方法向服务器发送指令来实现邮件的发送 (在smtp.yeah.net邮件服务器上试验通过. 其他邮件服务器没有试过) : 

    import smtplib, base64, time  
    userName = base64.encodestring('from').strip()  
    password = base64.encodestring('password').strip()  
    smtp = smtplib.SMTP()  
    smtp.connect("smtp.yeah.net:25")  
    print smtp.docmd('helo', 'from')  
    print smtp.docmd('auth login')  
    print smtp.docmd(userName)  
    print smtp.docmd(password)  
    print smtp.docmd('mail from:', '<from@yeah.net>')  
    print smtp.docmd('rcpt to:', '<from@yeah.net>')  
    #data 指令表示邮件内容  
    print smtp.docmd('data')  
    print smtp.docmd('''''from: from@yeah.net 
    to: from@yeah.net 
    subject: subject 
    email body 
    . 
    ''')  
    smtp.quit()  

SMTP.helo([hostname])

　　使用"helo"指令向服务器确认身份. 相当于告诉smtp服务器 "我是谁" . 
SMTP.has_extn(name)

　　判断指定名称在服务器邮件列表中是否存在. 出于安全考虑, smtp服务器往往屏蔽了该指令. 
SMTP.verify(address)

　　判断指定邮件地址是否在服务器中存在. 出于安全考虑, smtp服务器往往屏蔽了该指令. 
SMTP.login(user, password)

　　登陆到smtp服务器. 现在几乎所有的smtp服务器, 都必须在验证用户信息合法之后才允许发送邮件. 
SMTP.sendmail(from_addr, to_addrs, msg[, mail_options, rcpt_options])

　　发送邮件. 这里要注意一下第三个参数, msg是字符串, 表示邮件. 我们知道邮件一般由标题, 发信人, 收件人, 邮件内容, 附件等构成, 发送邮件的时候, 要注意msg的格式. 这个格式就是smtp协议中定义的格式. 在上面的例子中, msg的值为: 

    '''''From: from@yeah.net 
    To: to@21cn.com 
    Subject: test 
     
    just for test'''  

 

　　这个字符串的的意思表示邮件发件人为"from@yeah.net", 收件人为" to@21cn.com", 邮件标题为"test", 邮件内容为"just for test". 细心的你可能会疑问: 如果要发送的邮件内容很复杂, 包含图片、视频、附件等内容, 按照MIME的格式来拼接字符串, 将是一件非常麻烦的事. 不用担心, python已经考虑到了这点, 它为我们提供了email模块, 使用该模块可以轻松的发送带图片、视频、附件等复杂内容的邮件. 在介绍完smtplib模块之后, 我会简单介绍email模块的基本使用. 
SMTP.quit()

　　断开与smtp服务器的连接, 相当于发送"quit"指令. 
email及其相关子模块

　　emial模块用来处理邮件消息, 包括MIME和其他基于RFC 2822 的消息文档. 使用这些模块来定义邮件的内容, 是非常简单的. 下面是一些常用的类: 

class email.mime.multipart. MIMEMultipart: 多个MIME对象的集合. 

class email.mime.audio. MIMEAudio: MIME音频对象. 

class email.mime.image. MIMEImage: MIME二进制文件对象. 

class email.mime.text. MIMEText: MIME文本对象.  

