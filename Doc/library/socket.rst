:mod:`socket` --- 低层级网络接口
================================================

.. module:: socket
   :synopsis: Low-level networking interface.


This module provides access to the BSD *socket* interface. It is available on
all modern Unix systems, Windows, MacOS, OS/2, and probably additional
platforms.

.. note::

   Some behavior may be platform dependent, since calls are made to the operating
   system socket APIs.

.. index:: object: socket

The Python interface is a straightforward transliteration of the Unix system
call and library interface for sockets to Python's object-oriented style: the
:func:`socket` function returns a :dfn:`socket object` whose methods implement
the various socket system calls.  Parameter types are somewhat higher-level than
in the C interface: as with :meth:`read` and :meth:`write` operations on Python
files, buffer allocation on receive operations is automatic, and buffer length
is implicit on send operations.

socket 的英文意思是 "孔、插座" 
,在这里它是一种进程的通信机制. 可以把 socket 比作
电话插座,区号是通话双方的网络地址;一方电话机发出信号和另一方接受信号,相当
于向 socket 发送数据和从 socket 接收数据;通话结束后挂起电话机,相当于关闭 socket,
撤销链接. 
Python 中提供了丰富的网络编程模块,适当地调用这些模块可以大大提高编程效率,而
且能够保证程序的健壮性. 同时你会惊奇发现,Python 作为脚本语言,对部分网络协议
的实现相当快. 这里介绍的 socket 模块,是 Python 网络编程最基础的一个模块. 当然在
进行网络编程之前你必须清楚网络基本体系结构和基本原理,如果你做过类似的 C 语言
网络编程,你会发现 Python 网络编程的流程与之非常相似. 

网络服务端的建立过程分为 6 个阶段,创建 Socket 对象、绑定端口、监听连接、接受请
求、数据收发、关闭端口. 
分别对应函式
socket.socket() 、 socket.bind() 、 socket.listen() 、 socket.accept() 、
socket.sendall()\socket.recv()、socket.close(). 

网络服务端的建立过程分为四个阶段,创建 Socket 对象、连接服务器、数据收发、关闭Python端口. 分别对应函式 
socket.socket()、
socket.connect()、
socket.sendall()\socket.recv()、
socket.close()


这里采用的是可靠连接 TCP 协议,与之相对应的是不可靠的无连接的 UDP 协议. 创建
socket 对象的协议类型,由 socket.socket()的第二个参数决定. socket.SOCK_STREAM 代
表 TCP 协议,
socket.SOCK_DGRAM 代表 UDP 协议. 
当然采用不同协议的 socket 对象的
操作函式有些差异,这里列举的两个例子都是采用 TCP 协议的. 
例子中选取的端口为 5678,系统中的端口分为两类:0~1023 为周知端口,一般供特定
网络服务使用,1024~65535 为动态端口,供一般应用程序使用. 



.. seealso::

   Module :mod:`socketserver`
      Classes that simplify writing network servers.

   Module :mod:`ssl`
      A TLS/SSL wrapper for socket objects.


Socket families
---------------

Depending on the system and the build options, various socket families
are supported by this module.

Socket addresses are represented as follows:

- A single string is used for the :const:`AF_UNIX` address family.

- A pair ``(host, port)`` is used for the :const:`AF_INET` address family,
  where *host* is a string representing either a hostname in Internet domain
  notation like ``'daring.cwi.nl'`` or an IPv4 address like ``'100.50.200.5'``,
  and *port* is an integral port number.

- For :const:`AF_INET6` address family, a four-tuple ``(host, port, flowinfo,
  scopeid)`` is used, where *flowinfo* and *scopeid* represent the ``sin6_flowinfo``
  and ``sin6_scope_id`` members in :const:`struct sockaddr_in6` in C.  For
  :mod:`socket` module methods, *flowinfo* and *scopeid* can be omitted just for
  backward compatibility.  Note, however, omission of *scopeid* can cause problems
  in manipulating scoped IPv6 addresses.

- :const:`AF_NETLINK` sockets are represented as pairs ``(pid, groups)``.

- Linux-only support for TIPC is available using the :const:`AF_TIPC`
  address family.  TIPC is an open, non-IP based networked protocol designed
  for use in clustered computer environments.  Addresses are represented by a
  tuple, and the fields depend on the address type. The general tuple form is
  ``(addr_type, v1, v2, v3 [, scope])``, where:

  - *addr_type* is one of TIPC_ADDR_NAMESEQ, TIPC_ADDR_NAME, or
    TIPC_ADDR_ID.
  - *scope* is one of TIPC_ZONE_SCOPE, TIPC_CLUSTER_SCOPE, and
    TIPC_NODE_SCOPE.
  - If *addr_type* is TIPC_ADDR_NAME, then *v1* is the server type, *v2* is
    the port identifier, and *v3* should be 0.

    If *addr_type* is TIPC_ADDR_NAMESEQ, then *v1* is the server type, *v2*
    is the lower port number, and *v3* is the upper port number.

    If *addr_type* is TIPC_ADDR_ID, then *v1* is the node, *v2* is the
    reference, and *v3* should be set to 0.

    If *addr_type* is TIPC_ADDR_ID, then *v1* is the node, *v2* is the
    reference, and *v3* should be set to 0.

- Certain other address families (:const:`AF_BLUETOOTH`, :const:`AF_PACKET`)
  support specific representations.

  .. XXX document them!

For IPv4 addresses, two special forms are accepted instead of a host address:
the empty string represents :const:`INADDR_ANY`, and the string
``'<broadcast>'`` represents :const:`INADDR_BROADCAST`.  This behavior is not
compatible with IPv6, therefore, you may want to avoid these if you intend
to support IPv6 with your Python programs.

If you use a hostname in the *host* portion of IPv4/v6 socket address, the
program may show a nondeterministic behavior, as Python uses the first address
returned from the DNS resolution.  The socket address will be resolved
differently into an actual IPv4/v6 address, depending on the results from DNS
resolution and/or the host configuration.  For deterministic behavior use a
numeric address in *host* portion.

All errors raise exceptions.  The normal exceptions for invalid argument types
and out-of-memory conditions can be raised; errors related to socket or address
semantics raise :exc:`socket.error` or one of its subclasses.

Non-blocking mode is supported through :meth:`~socket.setblocking`.  A
generalization of this based on timeouts is supported through
:meth:`~socket.settimeout`.


Module contents
---------------

The module :mod:`socket` exports the following constants and functions:

Python 的 socket 模块定义了四种可能出现的异常. 
1. socket.error:与一般 I/O 操作和数据通信问题有关的异常. 
2. socket.gaierror:与域名解析有关的异常. 
3. socket.herror:与其他地址解析错误有关的异常. 
4. socket.timeout:socket 调用 sockettimeout()函式后,处理超时有关的异常. 



.. exception:: error

   .. index:: module: errno

   This exception is raised for socket-related errors. The accompanying value is
   either a string telling what went wrong or a pair ``(errno, string)``
   representing an error returned by a system call, similar to the value
   accompanying :exc:`os.error`. See the module :mod:`errno`, which contains names
   for the error codes defined by the underlying operating system.


.. exception:: herror

   This exception is raised for address-related errors, i.e. for functions that use
   *h_errno* in the C API, including :func:`gethostbyname_ex` and
   :func:`gethostbyaddr`.

   The accompanying value is a pair ``(h_errno, string)`` representing an error
   returned by a library call. *string* represents the description of *h_errno*, as
   returned by the :c:func:`hstrerror` C function.


.. exception:: gaierror

   This exception is raised for address-related errors, for :func:`getaddrinfo` and
   :func:`getnameinfo`. The accompanying value is a pair ``(error, string)``
   representing an error returned by a library call. *string* represents the
   description of *error*, as returned by the :c:func:`gai_strerror` C function. The
   *error* value will match one of the :const:`EAI_\*` constants defined in this
   module.


.. exception:: timeout

   This exception is raised when a timeout occurs on a socket which has had
   timeouts enabled via a prior call to :meth:`~socket.settimeout`.  The
   accompanying value is a string whose value is currently always "timed out".


.. data:: AF_UNIX
          AF_INET
          AF_INET6

   These constants represent the address (and protocol) families, used for the
   first argument to :func:`socket`.  If the :const:`AF_UNIX` constant is not
   defined then this protocol is unsupported.  More constants may be available
   depending on the system.


.. data:: SOCK_STREAM
          SOCK_DGRAM
          SOCK_RAW
          SOCK_RDM
          SOCK_SEQPACKET

   These constants represent the socket types, used for the second argument to
   :func:`socket`.  More constants may be available depending on the system.
   (Only :const:`SOCK_STREAM` and :const:`SOCK_DGRAM` appear to be generally
   useful.)

.. data:: SOCK_CLOEXEC
          SOCK_NONBLOCK

   These two constants, if defined, can be combined with the socket types and
   allow you to set some flags atomically (thus avoiding possible race
   conditions and the need for separate calls).

   .. seealso::

      `Secure File Descriptor Handling <http://udrepper.livejournal.com/20407.html>`_
      for a more thorough explanation.

   Availability: Linux >= 2.6.27.

   .. versionadded:: 3.2

.. data:: SO_*
          SOMAXCONN
          MSG_*
          SOL_*
          IPPROTO_*
          IPPORT_*
          INADDR_*
          IP_*
          IPV6_*
          EAI_*
          AI_*
          NI_*
          TCP_*

   Many constants of these forms, documented in the Unix documentation on sockets
   and/or the IP protocol, are also defined in the socket module. They are
   generally used in arguments to the :meth:`setsockopt` and :meth:`getsockopt`
   methods of socket objects.  In most cases, only those symbols that are defined
   in the Unix header files are defined; for a few symbols, default values are
   provided.

.. data:: SIO_*
          RCVALL_*

   Constants for Windows' WSAIoctl(). The constants are used as arguments to the
   :meth:`ioctl` method of socket objects.


.. data:: TIPC_*

   TIPC related constants, matching the ones exported by the C socket API. See
   the TIPC documentation for more information.


.. data:: has_ipv6

   This constant contains a boolean value which indicates if IPv6 is supported on
   this platform.


.. function:: create_connection(address[, timeout[, source_address]])

   Convenience function.  Connect to *address* (a 2-tuple ``(host, port)``),
   and return the socket object.  Passing the optional *timeout* parameter will
   set the timeout on the socket instance before attempting to connect.  If no
   *timeout* is supplied, the global default timeout setting returned by
   :func:`getdefaulttimeout` is used.

   If supplied, *source_address* must be a 2-tuple ``(host, port)`` for the
   socket to bind to as its source address before connecting.  If host or port
   are '' or 0 respectively the OS default behavior will be used.

   .. versionchanged:: 3.2
      *source_address* was added.

   .. versionchanged:: 3.2
      support for the :keyword:`with` statement was added.


.. function:: getaddrinfo(host, port, family=0, type=0, proto=0, flags=0)

   Translate the *host*/*port* argument into a sequence of 5-tuples that contain
   all the necessary arguments for creating a socket connected to that service.
   *host* is a domain name, a string representation of an IPv4/v6 address
   or ``None``. *port* is a string service name such as ``'http'``, a numeric
   port number or ``None``.  By passing ``None`` as the value of *host*
   and *port*, you can pass ``NULL`` to the underlying C API.

   The *family*, *type* and *proto* arguments can be optionally specified
   in order to narrow the list of addresses returned.  Passing zero as a
   value for each of these arguments selects the full range of results.
   The *flags* argument can be one or several of the ``AI_*`` constants,
   and will influence how results are computed and returned.
   For example, :const:`AI_NUMERICHOST` will disable domain name resolution
   and will raise an error if *host* is a domain name.

   The function returns a list of 5-tuples with the following structure:

   ``(family, type, proto, canonname, sockaddr)``

   In these tuples, *family*, *type*, *proto* are all integers and are
   meant to be passed to the :func:`socket` function.  *canonname* will be
   a string representing the canonical name of the *host* if
   :const:`AI_CANONNAME` is part of the *flags* argument; else *canonname*
   will be empty.  *sockaddr* is a tuple describing a socket address, whose
   format depends on the returned *family* (a ``(address, port)`` 2-tuple for
   :const:`AF_INET`, a ``(address, port, flow info, scope id)`` 4-tuple for
   :const:`AF_INET6`), and is meant to be passed to the :meth:`socket.connect`
   method.

   The following example fetches address information for a hypothetical TCP
   connection to ``www.python.org`` on port 80 (results may differ on your
   system if IPv6 isn't enabled)::

      >>> socket.getaddrinfo("www.python.org", 80, proto=socket.SOL_TCP)
      [(2, 1, 6, '', ('82.94.164.162', 80)),
       (10, 1, 6, '', ('2001:888:2000:d::a2', 80, 0, 0))]

   .. versionchanged:: 3.2
      parameters can now be passed as single keyword arguments.

.. function:: getfqdn([name])

   Return a fully qualified domain name for *name*. If *name* is omitted or empty,
   it is interpreted as the local host.  To find the fully qualified name, the
   hostname returned by :func:`gethostbyaddr` is checked, followed by aliases for the
   host, if available.  The first name which includes a period is selected.  In
   case no fully qualified domain name is available, the hostname as returned by
   :func:`gethostname` is returned.


.. function:: gethostbyname(hostname)

   Translate a host name to IPv4 address format.  The IPv4 address is returned as a
   string, such as  ``'100.50.200.5'``.  If the host name is an IPv4 address itself
   it is returned unchanged.  See :func:`gethostbyname_ex` for a more complete
   interface. :func:`gethostbyname` does not support IPv6 name resolution, and
   :func:`getaddrinfo` should be used instead for IPv4/v6 dual stack support.


.. function:: gethostbyname_ex(hostname)

   Translate a host name to IPv4 address format, extended interface. Return a
   triple ``(hostname, aliaslist, ipaddrlist)`` where *hostname* is the primary
   host name responding to the given *ip_address*, *aliaslist* is a (possibly
   empty) list of alternative host names for the same address, and *ipaddrlist* is
   a list of IPv4 addresses for the same interface on the same host (often but not
   always a single address). :func:`gethostbyname_ex` does not support IPv6 name
   resolution, and :func:`getaddrinfo` should be used instead for IPv4/v6 dual
   stack support.


.. function:: gethostname()

   Return a string containing the hostname of the machine where  the Python
   interpreter is currently executing.

   If you want to know the current machine's IP address, you may want to use
   ``gethostbyname(gethostname())``. This operation assumes that there is a
   valid address-to-host mapping for the host, and the assumption does not
   always hold.

   Note: :func:`gethostname` doesn't always return the fully qualified domain
   name; use ``getfqdn()`` (see above).


.. function:: gethostbyaddr(ip_address)

   Return a triple ``(hostname, aliaslist, ipaddrlist)`` where *hostname* is the
   primary host name responding to the given *ip_address*, *aliaslist* is a
   (possibly empty) list of alternative host names for the same address, and
   *ipaddrlist* is a list of IPv4/v6 addresses for the same interface on the same
   host (most likely containing only a single address). To find the fully qualified
   domain name, use the function :func:`getfqdn`. :func:`gethostbyaddr` supports
   both IPv4 and IPv6.


.. function:: getnameinfo(sockaddr, flags)

   Translate a socket address *sockaddr* into a 2-tuple ``(host, port)``. Depending
   on the settings of *flags*, the result can contain a fully-qualified domain name
   or numeric address representation in *host*.  Similarly, *port* can contain a
   string port name or a numeric port number.


.. function:: getprotobyname(protocolname)

   Translate an Internet protocol name (for example, ``'icmp'``) to a constant
   suitable for passing as the (optional) third argument to the :func:`socket`
   function.  This is usually only needed for sockets opened in "raw" mode
   (:const:`SOCK_RAW`); for the normal socket modes, the correct protocol is chosen
   automatically if the protocol is omitted or zero.


.. function:: getservbyname(servicename[, protocolname])

   Translate an Internet service name and protocol name to a port number for that
   service.  The optional protocol name, if given, should be ``'tcp'`` or
   ``'udp'``, otherwise any protocol will match.


.. function:: getservbyport(port[, protocolname])

   Translate an Internet port number and protocol name to a service name for that
   service.  The optional protocol name, if given, should be ``'tcp'`` or
   ``'udp'``, otherwise any protocol will match.


.. function:: socket([family[, type[, proto]]])

   Create a new socket using the given address family, socket type and protocol
   number.  The address family should be :const:`AF_INET` (the default),
   :const:`AF_INET6` or :const:`AF_UNIX`.  The socket type should be
   :const:`SOCK_STREAM` (the default), :const:`SOCK_DGRAM` or perhaps one of the
   other ``SOCK_`` constants.  The protocol number is usually zero and may be
   omitted in that case.


.. function:: socketpair([family[, type[, proto]]])

   Build a pair of connected socket objects using the given address family, socket
   type, and protocol number.  Address family, socket type, and protocol number are
   as for the :func:`socket` function above. The default family is :const:`AF_UNIX`
   if defined on the platform; otherwise, the default is :const:`AF_INET`.
   Availability: Unix.

   .. versionchanged:: 3.2
      The returned socket objects now support the whole socket API, rather
      than a subset.


.. function:: fromfd(fd, family, type[, proto])

   Duplicate the file descriptor *fd* (an integer as returned by a file object's
   :meth:`fileno` method) and build a socket object from the result.  Address
   family, socket type and protocol number are as for the :func:`socket` function
   above. The file descriptor should refer to a socket, but this is not checked ---
   subsequent operations on the object may fail if the file descriptor is invalid.
   This function is rarely needed, but can be used to get or set socket options on
   a socket passed to a program as standard input or output (such as a server
   started by the Unix inet daemon).  The socket is assumed to be in blocking mode.


.. function:: ntohl(x)

   Convert 32-bit positive integers from network to host byte order.  On machines
   where the host byte order is the same as network byte order, this is a no-op;
   otherwise, it performs a 4-byte swap operation.


.. function:: ntohs(x)

   Convert 16-bit positive integers from network to host byte order.  On machines
   where the host byte order is the same as network byte order, this is a no-op;
   otherwise, it performs a 2-byte swap operation.


.. function:: htonl(x)

   Convert 32-bit positive integers from host to network byte order.  On machines
   where the host byte order is the same as network byte order, this is a no-op;
   otherwise, it performs a 4-byte swap operation.


.. function:: htons(x)

   Convert 16-bit positive integers from host to network byte order.  On machines
   where the host byte order is the same as network byte order, this is a no-op;
   otherwise, it performs a 2-byte swap operation.


.. function:: inet_aton(ip_string)

   Convert an IPv4 address from dotted-quad string format (for example,
   '123.45.67.89') to 32-bit packed binary format, as a bytes object four characters in
   length.  This is useful when conversing with a program that uses the standard C
   library and needs objects of type :c:type:`struct in_addr`, which is the C type
   for the 32-bit packed binary this function returns.

   :func:`inet_aton` also accepts strings with less than three dots; see the
   Unix manual page :manpage:`inet(3)` for details.

   If the IPv4 address string passed to this function is invalid,
   :exc:`socket.error` will be raised. Note that exactly what is valid depends on
   the underlying C implementation of :c:func:`inet_aton`.

   :func:`inet_aton` does not support IPv6, and :func:`inet_pton` should be used
   instead for IPv4/v6 dual stack support.


.. function:: inet_ntoa(packed_ip)

   Convert a 32-bit packed IPv4 address (a bytes object four characters in
   length) to its standard dotted-quad string representation (for example,
   '123.45.67.89').  This is useful when conversing with a program that uses the
   standard C library and needs objects of type :c:type:`struct in_addr`, which
   is the C type for the 32-bit packed binary data this function takes as an
   argument.

   If the byte sequence passed to this function is not exactly 4 bytes in
   length, :exc:`socket.error` will be raised. :func:`inet_ntoa` does not
   support IPv6, and :func:`inet_ntop` should be used instead for IPv4/v6 dual
   stack support.


.. function:: inet_pton(address_family, ip_string)

   Convert an IP address from its family-specific string format to a packed,
   binary format. :func:`inet_pton` is useful when a library or network protocol
   calls for an object of type :c:type:`struct in_addr` (similar to
   :func:`inet_aton`) or :c:type:`struct in6_addr`.

   Supported values for *address_family* are currently :const:`AF_INET` and
   :const:`AF_INET6`. If the IP address string *ip_string* is invalid,
   :exc:`socket.error` will be raised. Note that exactly what is valid depends on
   both the value of *address_family* and the underlying implementation of
   :c:func:`inet_pton`.

   Availability: Unix (maybe not all platforms).


.. function:: inet_ntop(address_family, packed_ip)

   Convert a packed IP address (a bytes object of some number of characters) to its
   standard, family-specific string representation (for example, ``'7.10.0.5'`` or
   ``'5aef:2b::8'``). :func:`inet_ntop` is useful when a library or network protocol
   returns an object of type :c:type:`struct in_addr` (similar to :func:`inet_ntoa`)
   or :c:type:`struct in6_addr`.

   Supported values for *address_family* are currently :const:`AF_INET` and
   :const:`AF_INET6`. If the string *packed_ip* is not the correct length for the
   specified address family, :exc:`ValueError` will be raised.  A
   :exc:`socket.error` is raised for errors from the call to :func:`inet_ntop`.

   Availability: Unix (maybe not all platforms).


.. function:: getdefaulttimeout()

   Return the default timeout in floating seconds for new socket objects. A value
   of ``None`` indicates that new socket objects have no timeout. When the socket
   module is first imported, the default is ``None``.


.. function:: setdefaulttimeout(timeout)

   Set the default timeout in floating seconds for new socket objects.  When
   the socket module is first imported, the default is ``None``.  See
   :meth:`~socket.settimeout` for possible values and their respective
   meanings.


.. data:: SocketType

   This is a Python type object that represents the socket object type. It is the
   same as ``type(socket(...))``.


.. _socket-objects:

Socket Objects
--------------

Socket objects have the following methods.  Except for :meth:`makefile` these
correspond to Unix system calls applicable to sockets.


.. method:: socket.accept()

   Accept a connection. The socket must be bound to an address and listening for
   connections. The return value is a pair ``(conn, address)`` where *conn* is a
   *new* socket object usable to send and receive data on the connection, and
   *address* is the address bound to the socket on the other end of the connection.


.. method:: socket.bind(address)

   Bind the socket to *address*.  The socket must not already be bound. (The format
   of *address* depends on the address family --- see above.)


.. method:: socket.close()

   Close the socket.  All future operations on the socket object will fail. The
   remote end will receive no more data (after queued data is flushed). Sockets are
   automatically closed when they are garbage-collected.

   .. note::
      :meth:`close()` releases the resource associated with a connection but
      does not necessarily close the connection immediately.  If you want
      to close the connection in a timely fashion, call :meth:`shutdown()`
      before :meth:`close()`.


.. method:: socket.connect(address)

   Connect to a remote socket at *address*. (The format of *address* depends on the
   address family --- see above.)


.. method:: socket.connect_ex(address)

   Like ``connect(address)``, but return an error indicator instead of raising an
   exception for errors returned by the C-level :c:func:`connect` call (other
   problems, such as "host not found," can still raise exceptions).  The error
   indicator is ``0`` if the operation succeeded, otherwise the value of the
   :c:data:`errno` variable.  This is useful to support, for example, asynchronous
   connects.


.. method:: socket.detach()

   Put the socket object into closed state without actually closing the
   underlying file descriptor.  The file descriptor is returned, and can
   be reused for other purposes.

   .. versionadded:: 3.2


.. method:: socket.fileno()

   Return the socket's file descriptor (a small integer).  This is useful with
   :func:`select.select`.

   Under Windows the small integer returned by this method cannot be used where a
   file descriptor can be used (such as :func:`os.fdopen`).  Unix does not have
   this limitation.


.. method:: socket.getpeername()

   Return the remote address to which the socket is connected.  This is useful to
   find out the port number of a remote IPv4/v6 socket, for instance. (The format
   of the address returned depends on the address family --- see above.)  On some
   systems this function is not supported.


.. method:: socket.getsockname()

   Return the socket's own address.  This is useful to find out the port number of
   an IPv4/v6 socket, for instance. (The format of the address returned depends on
   the address family --- see above.)


.. method:: socket.getsockopt(level, optname[, buflen])

   Return the value of the given socket option (see the Unix man page
   :manpage:`getsockopt(2)`).  The needed symbolic constants (:const:`SO_\*` etc.)
   are defined in this module.  If *buflen* is absent, an integer option is assumed
   and its integer value is returned by the function.  If *buflen* is present, it
   specifies the maximum length of the buffer used to receive the option in, and
   this buffer is returned as a bytes object.  It is up to the caller to decode the
   contents of the buffer (see the optional built-in module :mod:`struct` for a way
   to decode C structures encoded as byte strings).


.. method:: socket.gettimeout()

   Return the timeout in floating seconds associated with socket operations,
   or ``None`` if no timeout is set.  This reflects the last call to
   :meth:`setblocking` or :meth:`settimeout`.


.. method:: socket.ioctl(control, option)

   :platform: Windows

   The :meth:`ioctl` method is a limited interface to the WSAIoctl system
   interface.  Please refer to the `Win32 documentation
   <http://msdn.microsoft.com/en-us/library/ms741621%28VS.85%29.aspx>`_ for more
   information.

   On other platforms, the generic :func:`fcntl.fcntl` and :func:`fcntl.ioctl`
   functions may be used; they accept a socket object as their first argument.

.. method:: socket.listen(backlog)

   Listen for connections made to the socket.  The *backlog* argument specifies the
   maximum number of queued connections and should be at least 1; the maximum value
   is system-dependent (usually 5).


.. method:: socket.makefile(mode='r', buffering=None, *, encoding=None, \
                            errors=None, newline=None)

   .. index:: single: I/O control; buffering

   Return a :term:`file object` associated with the socket.  The exact returned
   type depends on the arguments given to :meth:`makefile`.  These arguments are
   interpreted the same way as by the built-in :func:`open` function.

   Closing the file object won't close the socket unless there are no remaining
   references to the socket.  The socket must be in blocking mode; it can have
   a timeout, but the file object's internal buffer may end up in a inconsistent
   state if a timeout occurs.

   .. note::

      On Windows, the file-like object created by :meth:`makefile` cannot be
      used where a file object with a file descriptor is expected, such as the
      stream arguments of :meth:`subprocess.Popen`.


.. method:: socket.recv(bufsize[, flags])

   Receive data from the socket.  The return value is a bytes object representing the
   data received.  The maximum amount of data to be received at once is specified
   by *bufsize*.  See the Unix manual page :manpage:`recv(2)` for the meaning of
   the optional argument *flags*; it defaults to zero.

   .. note::

      For best match with hardware and network realities, the value of  *bufsize*
      should be a relatively small power of 2, for example, 4096.


.. method:: socket.recvfrom(bufsize[, flags])

   Receive data from the socket.  The return value is a pair ``(bytes, address)``
   where *bytes* is a bytes object representing the data received and *address* is the
   address of the socket sending the data.  See the Unix manual page
   :manpage:`recv(2)` for the meaning of the optional argument *flags*; it defaults
   to zero. (The format of *address* depends on the address family --- see above.)


.. method:: socket.recvfrom_into(buffer[, nbytes[, flags]])

   Receive data from the socket, writing it into *buffer* instead of creating a
   new bytestring.  The return value is a pair ``(nbytes, address)`` where *nbytes* is
   the number of bytes received and *address* is the address of the socket sending
   the data.  See the Unix manual page :manpage:`recv(2)` for the meaning of the
   optional argument *flags*; it defaults to zero.  (The format of *address*
   depends on the address family --- see above.)


.. method:: socket.recv_into(buffer[, nbytes[, flags]])

   Receive up to *nbytes* bytes from the socket, storing the data into a buffer
   rather than creating a new bytestring.  If *nbytes* is not specified (or 0),
   receive up to the size available in the given buffer.  Returns the number of
   bytes received.  See the Unix manual page :manpage:`recv(2)` for the meaning
   of the optional argument *flags*; it defaults to zero.


.. method:: socket.send(bytes[, flags])

   Send data to the socket.  The socket must be connected to a remote socket.  The
   optional *flags* argument has the same meaning as for :meth:`recv` above.
   Returns the number of bytes sent. Applications are responsible for checking that
   all data has been sent; if only some of the data was transmitted, the
   application needs to attempt delivery of the remaining data.


.. method:: socket.sendall(bytes[, flags])

   Send data to the socket.  The socket must be connected to a remote socket.  The
   optional *flags* argument has the same meaning as for :meth:`recv` above.
   Unlike :meth:`send`, this method continues to send data from *bytes* until
   either all data has been sent or an error occurs.  ``None`` is returned on
   success.  On error, an exception is raised, and there is no way to determine how
   much data, if any, was successfully sent.


.. method:: socket.sendto(bytes[, flags], address)

   Send data to the socket.  The socket should not be connected to a remote socket,
   since the destination socket is specified by *address*.  The optional *flags*
   argument has the same meaning as for :meth:`recv` above.  Return the number of
   bytes sent. (The format of *address* depends on the address family --- see
   above.)


.. method:: socket.setblocking(flag)

   Set blocking or non-blocking mode of the socket: if *flag* is false, the
   socket is set to non-blocking, else to blocking mode.

   This method is a shorthand for certain :meth:`~socket.settimeout` calls:

   * ``sock.setblocking(True)`` is equivalent to ``sock.settimeout(None)``

   * ``sock.setblocking(False)`` is equivalent to ``sock.settimeout(0.0)``


.. method:: socket.settimeout(value)

   Set a timeout on blocking socket operations.  The *value* argument can be a
   nonnegative floating point number expressing seconds, or ``None``.
   If a non-zero value is given, subsequent socket operations will raise a
   :exc:`timeout` exception if the timeout period *value* has elapsed before
   the operation has completed.  If zero is given, the socket is put in
   non-blocking mode. If ``None`` is given, the socket is put in blocking mode.

   For further information, please consult the :ref:`notes on socket timeouts <socket-timeouts>`.


.. method:: socket.setsockopt(level, optname, value)

   .. index:: module: struct

   Set the value of the given socket option (see the Unix manual page
   :manpage:`setsockopt(2)`).  The needed symbolic constants are defined in the
   :mod:`socket` module (:const:`SO_\*` etc.).  The value can be an integer or a
   bytes object representing a buffer.  In the latter case it is up to the caller to
   ensure that the bytestring contains the proper bits (see the optional built-in
   module :mod:`struct` for a way to encode C structures as bytestrings).


.. method:: socket.shutdown(how)

   Shut down one or both halves of the connection.  If *how* is :const:`SHUT_RD`,
   further receives are disallowed.  If *how* is :const:`SHUT_WR`, further sends
   are disallowed.  If *how* is :const:`SHUT_RDWR`, further sends and receives are
   disallowed.  Depending on the platform, shutting down one half of the connection
   can also close the opposite half (e.g. on Mac OS X, ``shutdown(SHUT_WR)`` does
   not allow further reads on the other end of the connection).

Note that there are no methods :meth:`read` or :meth:`write`; use
:meth:`~socket.recv` and :meth:`~socket.send` without *flags* argument instead.

Socket objects also have these (read-only) attributes that correspond to the
values given to the :class:`socket` constructor.


.. attribute:: socket.family

   The socket family.


.. attribute:: socket.type

   The socket type.


.. attribute:: socket.proto

   The socket protocol.



.. _socket-timeouts:

Notes on socket timeouts
------------------------

A socket object can be in one of three modes: blocking, non-blocking, or
timeout.  Sockets are by default always created in blocking mode, but this
can be changed by calling :func:`setdefaulttimeout`.

* In *blocking mode*, operations block until complete or the system returns
  an error (such as connection timed out).

* In *non-blocking mode*, operations fail (with an error that is unfortunately
  system-dependent) if they cannot be completed immediately: functions from the
  :mod:`select` can be used to know when and whether a socket is available for
  reading or writing.

* In *timeout mode*, operations fail if they cannot be completed within the
  timeout specified for the socket (they raise a :exc:`timeout` exception)
  or if the system returns an error.

.. note::
   At the operating system level, sockets in *timeout mode* are internally set
   in non-blocking mode.  Also, the blocking and timeout modes are shared between
   file descriptors and socket objects that refer to the same network endpoint.
   This implementation detail can have visible consequences if e.g. you decide
   to use the :meth:`~socket.fileno()` of a socket.

Timeouts and the ``connect`` method
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :meth:`~socket.connect` operation is also subject to the timeout
setting, and in general it is recommended to call :meth:`~socket.settimeout`
before calling :meth:`~socket.connect` or pass a timeout parameter to
:meth:`create_connection`.  However, the system network stack may also
return a connection timeout error of its own regardless of any Python socket
timeout setting.

Timeouts and the ``accept`` method
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If :func:`getdefaulttimeout` is not :const:`None`, sockets returned by
the :meth:`~socket.accept` method inherit that timeout.  Otherwise, the
behaviour depends on settings of the listening socket:

* if the listening socket is in *blocking mode* or in *timeout mode*,
  the socket returned by :meth:`~socket.accept` is in *blocking mode*;

* if the listening socket is in *non-blocking mode*, whether the socket
  returned by :meth:`~socket.accept` is in blocking or non-blocking mode
  is operating system-dependent.  If you want to ensure cross-platform
  behaviour, it is recommended you manually override this setting.


.. _socket-example:

Example
-------

Here are four minimal example programs using the TCP/IP protocol: a server that
echoes all data that it receives back (servicing only one client), and a client
using it.  Note that a server must perform the sequence :func:`socket`,
:meth:`~socket.bind`, :meth:`~socket.listen`, :meth:`~socket.accept` (possibly
repeating the :meth:`~socket.accept` to service more than one client), while a
client only needs the sequence :func:`socket`, :meth:`~socket.connect`.  Also
note that the server does not :meth:`~socket.send`/:meth:`~socket.recv` on the
socket it is listening on but on the new socket returned by
:meth:`~socket.accept`.

The first two examples support IPv4 only. ::

   # Echo server program
   import socket

   HOST = ''                 # Symbolic name meaning all available interfaces
   PORT = 50007              # Arbitrary non-privileged port
   s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
   s.bind((HOST, PORT))
   s.listen(1)
   conn, addr = s.accept()
   print('Connected by', addr)
   while True:
       data = conn.recv(1024)
       if not data: break
       conn.send(data)
   conn.close()

::

   # Echo client program
   import socket

   HOST = 'daring.cwi.nl'    # The remote host
   PORT = 50007              # The same port as used by the server
   s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
   s.connect((HOST, PORT))
   s.send(b'Hello, world')
   data = s.recv(1024)
   s.close()
   print('Received', repr(data))

The next two examples are identical to the above two, but support both IPv4 and
IPv6. The server side will listen to the first address family available (it
should listen to both instead). On most of IPv6-ready systems, IPv6 will take
precedence and the server may not accept IPv4 traffic. The client side will try
to connect to the all addresses returned as a result of the name resolution, and
sends traffic to the first one connected successfully. ::

   # Echo server program
   import socket
   import sys

   HOST = None               # Symbolic name meaning all available interfaces
   PORT = 50007              # Arbitrary non-privileged port
   s = None
   for res in socket.getaddrinfo(HOST, PORT, socket.AF_UNSPEC,
                                 socket.SOCK_STREAM, 0, socket.AI_PASSIVE):
       af, socktype, proto, canonname, sa = res
       try:
           s = socket.socket(af, socktype, proto)
       except socket.error as msg:
           s = None
           continue
       try:
           s.bind(sa)
           s.listen(1)
       except socket.error as msg:
           s.close()
           s = None
           continue
       break
   if s is None:
       print('could not open socket')
       sys.exit(1)
   conn, addr = s.accept()
   print('Connected by', addr)
   while True:
       data = conn.recv(1024)
       if not data: break
       conn.send(data)
   conn.close()

::

   # Echo client program
   import socket
   import sys

   HOST = 'daring.cwi.nl'    # The remote host
   PORT = 50007              # The same port as used by the server
   s = None
   for res in socket.getaddrinfo(HOST, PORT, socket.AF_UNSPEC, socket.SOCK_STREAM):
       af, socktype, proto, canonname, sa = res
       try:
           s = socket.socket(af, socktype, proto)
       except socket.error as msg:
           s = None
           continue
       try:
           s.connect(sa)
       except socket.error as msg:
           s.close()
           s = None
           continue
       break
   if s is None:
       print('could not open socket')
       sys.exit(1)
   s.send(b'Hello, world')
   data = s.recv(1024)
   s.close()
   print('Received', repr(data))


The last example shows how to write a very simple network sniffer with raw
sockets on Windows. The example requires administrator privileges to modify
the interface::

   import socket

   # the public network interface
   HOST = socket.gethostbyname(socket.gethostname())

   # create a raw socket and bind it to the public interface
   s = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket.IPPROTO_IP)
   s.bind((HOST, 0))

   # Include IP headers
   s.setsockopt(socket.IPPROTO_IP, socket.IP_HDRINCL, 1)

   # receive all packages
   s.ioctl(socket.SIO_RCVALL, socket.RCVALL_ON)

   # receive a package
   print(s.recvfrom(65565))

   # disabled promiscuous mode
   s.ioctl(socket.SIO_RCVALL, socket.RCVALL_OFF)


.. seealso::

   For an introduction to socket programming (in C), see the following papers:

   - *An Introductory 4.3BSD Interprocess Communication Tutorial*, by Stuart Sechrest

   - *An Advanced 4.3BSD Interprocess Communication Tutorial*, by Samuel J.  Leffler et
     al,

   both in the UNIX Programmer's Manual, Supplementary Documents 1 (sections
   PS1:7 and PS1:8).  The platform-specific reference material for the various
   socket-related system calls are also a valuable source of information on the
   details of socket semantics.  For Unix, refer to the manual pages; for Windows,
   see the WinSock (or Winsock 2) specification.  For IPv6-ready APIs, readers may
   want to refer to :rfc:`3493` titled Basic Socket Interface Extensions for IPv6.


