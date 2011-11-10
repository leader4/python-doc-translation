:mod:`errno` --- 标准的errno系统符号
==============================================

.. module:: errno
   :synopsis: Standard errno system symbols.


This module makes available standard ``errno`` system symbols. The value of each
symbol is the corresponding integer value. The names and descriptions are
borrowed from :file:`linux/include/errno.h`, which should be pretty
all-inclusive.

此模块提供 "标准" 的errno系统符号.   "
每个符号的值是对应的整数值. 的名称和
描述都借鉴了`` Linux / include / errno.h中的的 ",这应该
很包容一切. 


.. data:: errorcode

   Dictionary providing a mapping from the errno value to the string name in the
   underlying system.  For instance, ``errno.errorcode[errno.EPERM]`` maps to
   ``'EPERM'``.

    词典提供的errno值映射到字符串
   在基础系统的名称. 例如,
    "errno.errorcode [errno.EPERM] `` ``'EPERM'" 的地图. 

To translate a numeric error code to an error message, use :func:`os.strerror`.

要转换一个数字错误代码的错误消息,请使用
`` os.strerror ()``.

Of the following list, symbols that are not used on the current platform are not
defined by the module.  The specific list of defined symbols is available as
``errno.errorcode.keys()``.  Symbols available can include:

下面的列表,符号,不使用对当前
平台是没有定义的模块. 具体定义列表
符号是`` errno.errorcode.keys ()``.符号可
可以包括: 


.. data:: EPERM

   Operation not permitted

   不允许操作


.. data:: ENOENT

   No such file or directory

    没有这样的文件或目录


.. data:: ESRCH

   No such process

   没有这样的进程


.. data:: EINTR

   Interrupted system call

   中断的系统调用


.. data:: EIO

   I/O error

    I / O错误


.. data:: ENXIO

   No such device or address

   没有这样的设备或地址


.. data:: E2BIG

   Arg list too long

   参数列表太长


.. data:: ENOEXEC

   Exec format error

   EXEC格式错误


.. data:: EBADF

   Bad file number

   错误的文件数


.. data:: ECHILD

   No child processes

   没有子进程


.. data:: EAGAIN

   Try again

   再试一次


.. data:: ENOMEM

   Out of memory

   内存不足


.. data:: EACCES

   Permission denied

   权限被拒绝


.. data:: EFAULT

   Bad address

   错误的地址


.. data:: ENOTBLK

   Block device required

    块设备要求

.. data:: EBUSY

   Device or resource busy

   设备或资源忙


.. data:: EEXIST

   File exists

   文件已存在


.. data:: EXDEV

   Cross-device link

   跨设备链路


.. data:: ENODEV

   No such device

   没有这样的设备


.. data:: ENOTDIR

   Not a directory

   不是目录


.. data:: EISDIR

   Is a directory

   是一个目录


.. data:: EINVAL

   Invalid argument

   无效的参数


.. data:: ENFILE

   File table overflow

   文件表溢出


.. data:: EMFILE

   Too many open files

    打开的文件太多


.. data:: ENOTTY

   Not a typewriter

   不是一台打字机


.. data:: ETXTBSY

   Text file busy

   文本文件忙


.. data:: EFBIG

   File too large

   文件过大


.. data:: ENOSPC

   No space left on device

   设备上没有空间


.. data:: ESPIPE

   Illegal seek

    非法寻求


.. data:: EROFS

   Read-only file system

   只读文件系统


.. data:: EMLINK

   Too many links

   环节过多


.. data:: EPIPE

   Broken pipe

   管道破裂


.. data:: EDOM

   Math argument out of domain of func

   数学函数域的参数进行


.. data:: ERANGE

   Math result not representable

   数学结果不表示


.. data:: EDEADLK

   Resource deadlock would occur

   资源死锁会发生


.. data:: ENAMETOOLONG

   File name too long

   文件名太长


.. data:: ENOLCK

   No record locks available

   没有可用的记录锁


.. data:: ENOSYS

   Function not implemented

   未实现的功能


.. data:: ENOTEMPTY

   Directory not empty

   目录不是空的


.. data:: ELOOP

   Too many symbolic links encountered

   遇到太多的符号链接


.. data:: EWOULDBLOCK

   Operation would block

   操作会阻止


.. data:: ENOMSG

   No message of desired type

   无任何所需类型的消息


.. data:: EIDRM

   Identifier removed

    标识符被删除


.. data:: ECHRNG

   Channel number out of range

   超出范围的通道数量


.. data:: EL2NSYNC

   Level 2 not synchronized

   2级不同步


.. data:: EL3HLT

   Level 3 halted

    停止3级


.. data:: EL3RST

   Level 3 reset

   等级3复位


.. data:: ELNRNG

   Link number out of range

   超出范围的链接数量


.. data:: EUNATCH

   Protocol driver not attached

   不附加议定书的驱动程序


.. data:: ENOCSI

   No CSI structure available

   没有CSI结构可用


.. data:: EL2HLT

   Level 2 halted

   停止2级


.. data:: EBADE

   Invalid exchange

   无效的交换


.. data:: EBADR

   Invalid request descriptor

   无效的请求描述


.. data:: EXFULL

   Exchange full

   全部被交换


.. data:: ENOANO

   No anode

    没有阳极


.. data:: EBADRQC

   Invalid request code

   无效的请求代码


.. data:: EBADSLT

   Invalid slot

   无效的插槽


.. data:: EDEADLOCK

   File locking deadlock error

   文件锁定死锁错误


.. data:: EBFONT

   Bad font file format

   错误的字体文件格式


.. data:: ENOSTR

   Device not a stream

   设备未流


.. data:: ENODATA

   No data available

   无可用数据


.. data:: ETIME

   Timer expired

   计时器过期


.. data:: ENOSR

   Out of streams resources

   流资源流失


.. data:: ENONET

   Machine is not on the network

    机器不是在网络上


.. data:: ENOPKG

   Package not installed

   安装软件包暂时不可用


.. data:: EREMOTE

   Object is remote

   对象是远程


.. data:: ENOLINK

   Link has been severed

   链接已被切断


.. data:: EADV

   Advertise error

   广告的错误


.. data:: ESRMNT

   Srmount error

     Srmount错误


.. data:: ECOMM

   Communication error on send

   发送时通信错误


.. data:: EPROTO

   Protocol error

    协议错误


.. data:: EMULTIHOP

   Multihop attempted

   多跳企图


.. data:: EDOTDOT

   RFS specific error

   RFS的具体错误


.. data:: EBADMSG

   Not a data message

   不是一个数据信息


.. data:: EOVERFLOW

   Value too large for defined data type

   定义的数据类型的值太大


.. data:: ENOTUNIQ

   Name not unique on network

   名称在网络上的并非唯一


.. data:: EBADFD

   File descriptor in bad state

   在状态不好的文件描述符


.. data:: EREMCHG

   Remote address changed

   远程地址已变更


.. data:: ELIBACC

   Can not access a needed shared library

   无法访问所需的共享库


.. data:: ELIBBAD

   Accessing a corrupted shared library

   访问损坏的共享库


.. data:: ELIBSCN

   .lib section in a.out corrupted

   a.out 中的部分.lib中断


.. data:: ELIBMAX

   Attempting to link in too many shared libraries

   试图链接太多的共享库


.. data:: ELIBEXEC

   Cannot exec a shared library directly

   无法直接一个共享的库EXEC


.. data:: EILSEQ

   Illegal byte sequence

   非法字节序列


.. data:: ERESTART

   Interrupted system call should be restarted

    应该重新启动中断的系统调用


.. data:: ESTRPIPE

   Streams pipe error

   流管道错误


.. data:: EUSERS

   Too many users

   用户过多


.. data:: ENOTSOCK

   Socket operation on non-socket

   非套接字上的套接字操作


.. data:: EDESTADDRREQ

   Destination address required

   需要目标地址


.. data:: EMSGSIZE

   Message too long

   消息太长


.. data:: EPROTOTYPE

   Protocol wrong type for socket

   协议套接字错误类型


.. data:: ENOPROTOOPT

   Protocol not available

   协议没有用


.. data:: EPROTONOSUPPORT

   Protocol not supported

   协议不支持


.. data:: ESOCKTNOSUPPORT

   Socket type not supported

   套接字类型不支持


.. data:: EOPNOTSUPP

   Operation not supported on transport endpoint

   不支持的操作上传输端点


.. data:: EPFNOSUPPORT

   Protocol family not supported

   不支持的协议族


.. data:: EAFNOSUPPORT

   Address family not supported by protocol

   按协议不支持的地址族


.. data:: EADDRINUSE

   Address already in use

   地址已在使用


.. data:: EADDRNOTAVAIL

   Cannot assign requested address

   无法分配请求的地址


.. data:: ENETDOWN

   Network is down

   网络已关闭


.. data:: ENETUNREACH

   Network is unreachable

   网络不可达


.. data:: ENETRESET

   Network dropped connection because of reset

   网络连接的下降,因为复位


.. data:: ECONNABORTED

   Software caused connection abort

   软体造成连线中止


.. data:: ECONNRESET

   Connection reset by peer

   连接复位


.. data:: ENOBUFS

   No buffer space available

   没有可用的缓冲空间


.. data:: EISCONN

   Transport endpoint is already connected

   已连接的传输端点


.. data:: ENOTCONN

   Transport endpoint is not connected

   不连接的传输端点


.. data:: ESHUTDOWN

   Cannot send after transport endpoint shutdown

   传输端点关闭后无法发送


.. data:: ETOOMANYREFS

   Too many references: cannot splice

   引用过多: 无法接合


.. data:: ETIMEDOUT

   Connection timed out

   连接超时


.. data:: ECONNREFUSED

   Connection refused

   连接被拒绝


.. data:: EHOSTDOWN

   Host is down

   主机已关闭


.. data:: EHOSTUNREACH

   No route to host

   没有路由到主机


.. data:: EALREADY

   Operation already in progress

   操作已在进行中


.. data:: EINPROGRESS

   Operation now in progress

   操作现在正在进行中


.. data:: ESTALE

   Stale NFS file handle

    陈旧的NFS文件句柄


.. data:: EUCLEAN

   Structure needs cleaning

   结构需要清理


.. data:: ENOTNAM

   Not a XENIX named type file

   不是一个XENIX的命名类型的文件


.. data:: ENAVAIL

   No XENIX semaphores available

   没有XENIX的信号可用


.. data:: EISNAM

   Is a named type file

    是一个命名的类型的文件


.. data:: EREMOTEIO

   Remote I/O error

   远程I / O错误


.. data:: EDQUOT

   Quota exceeded

   超过配额


