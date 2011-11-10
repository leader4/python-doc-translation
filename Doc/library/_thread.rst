:mod:`_thread` --- 低级多线程API
==========================================

.. module:: _thread
   :synopsis: Low-level threading API.


.. index::
   single: light-weight processes
   single: processes, light-weight
   single: binary semaphores
   single: semaphores, binary

This module provides low-level primitives for working with multiple threads
(also called :dfn:`light-weight processes` or :dfn:`tasks`) --- multiple threads of
control sharing their global data space.  For synchronization, simple locks
(also called :dfn:`mutexes` or :dfn:`binary semaphores`) are provided.
The :mod:`threading` module provides an easier to use and higher-level
threading API built on top of this module.

Python 是支持多线程的,并且是 native 的线程,主要是通过 thread 和 threading 这两个模
块来实现的. thread 是比较底层的模块,threading 是 thread 的包装,可以更加方便地被使
用. 这里需要提一下的是 Python 对线程的支持还不够完善,不能利用多 CPU,但是下个
版本的 Python 中已经考虑改进这点. 
我们不建议使用 thread 模块,
是由于以下几点原因:
首先,更高级别的 threading 模块更为先进,对线程的支持更为完善,而且使用 thread 模
块里的属性有可能会与 threading 冲突. 
其次,
低级别 thread 模块的同步原语很少
(实际上只有一个) 而 threading 模块则有很多. 
,
还有一个不要使用 thread 的原因,就是它对你的进程什么时候应该结束完全没有控制,
当主线程结束时,所有的线程都会被强制结束掉,没有警告,也不会有正常的清除工作. 


.. index::
   single: pthreads
   pair: threads; POSIX

The module is optional.  It is supported on Windows, Linux, SGI IRIX, Solaris
2.x, as well as on systems that have a POSIX thread (a.k.a. "pthread")
implementation.  For systems lacking the :mod:`_thread` module, the
:mod:`_dummy_thread` module is available. It duplicates this module's interface
and can be used as a drop-in replacement.

It defines the following constants and functions:


.. exception:: error

   Raised on thread-specific errors.


.. data:: LockType

   This is the type of lock objects.


.. function:: start_new_thread(function, args[, kwargs])

   Start a new thread and return its identifier.  The thread executes the function
   *function* with the argument list *args* (which must be a tuple).  The optional
   *kwargs* argument specifies a dictionary of keyword arguments. When the function
   returns, the thread silently exits.  When the function terminates with an
   unhandled exception, a stack trace is printed and then the thread exits (but
   other threads continue to run).
   
   函数将创建一个新的线程,并返回该线程的标识符 (标识符为整数) . 参数 function 表示线程创建之后,
   立即执行的函数,参数 args 是该函数的参数,它是一个元组类型; 第二个参数 kwargs 是可选的,它为函数提供了命名参数字典. 
   函数执行完毕之后,线程将自动退出. 如果函数在执行过程中遇到未处理的异常,该线程将退出,但不会影响其他线程的执行. 


.. function:: interrupt_main()

   Raise a :exc:`KeyboardInterrupt` exception in the main thread.  A subthread can
   use this function to interrupt the main thread.
   
   在主线程中触发 KeyboardInterrupt 异常. 子线程可以使用该方法来中断主线程. 


.. function:: exit()

   Raise the :exc:`SystemExit` exception.  When not caught, this will cause the
   thread to exit silently.
   
   结束当前线程. 调用该函数会触发 SystemExit 异常,如果没有处理该异常,线程将结束. 

..
   function:: exit_prog(status)

      Exit all threads and report the value of the integer argument
      *status* as the exit status of the entire program.
      **Caveat:** code in pending :keyword:`finally` clauses, in this thread
      or in other threads, is not executed.


.. function:: allocate_lock()

   Return a new lock object.  Methods of locks are described below.  The lock is
   initially unlocked.


.. function:: get_ident()

   Return the 'thread identifier' of the current thread.  This is a nonzero
   integer.  Its value has no direct meaning; it is intended as a magic cookie to
   be used e.g. to index a dictionary of thread-specific data.  Thread identifiers
   may be recycled when a thread exits and another thread is created.
   
   返回当前线程的标识符,标识符是一个非零整数. 


.. function:: stack_size([size])

   Return the thread stack size used when creating new threads.  The optional
   *size* argument specifies the stack size to be used for subsequently created
   threads, and must be 0 (use platform or configured default) or a positive
   integer value of at least 32,768 (32kB). If changing the thread stack size is
   unsupported, a :exc:`ThreadError` is raised.  If the specified stack size is
   invalid, a :exc:`ValueError` is raised and the stack size is unmodified.  32kB
   is currently the minimum supported stack size value to guarantee sufficient
   stack space for the interpreter itself.  Note that some platforms may have
   particular restrictions on values for the stack size, such as requiring a
   minimum stack size > 32kB or requiring allocation in multiples of the system
   memory page size - platform documentation should be referred to for more
   information (4kB pages are common; using multiples of 4096 for the stack size is
   the suggested approach in the absence of more specific information).
   Availability: Windows, systems with POSIX threads.


.. data:: TIMEOUT_MAX

   The maximum value allowed for the *timeout* parameter of
   :meth:`Lock.acquire`. Specifying a timeout greater than this value will
   raise an :exc:`OverflowError`.

   .. versionadded:: 3.2


Lock objects have the following methods:


.. method:: lock.acquire(waitflag=1, timeout=-1)

   Without any optional argument, this method acquires the lock unconditionally, if
   necessary waiting until it is released by another thread (only one thread at a
   time can acquire a lock --- that's their reason for existence).

   If the integer *waitflag* argument is present, the action depends on its
   value: if it is zero, the lock is only acquired if it can be acquired
   immediately without waiting, while if it is nonzero, the lock is acquired
   unconditionally as above.

   If the floating-point *timeout* argument is present and positive, it
   specifies the maximum wait time in seconds before returning.  A negative
   *timeout* argument specifies an unbounded wait.  You cannot specify
   a *timeout* if *waitflag* is zero.
   
   　获取琐. 函数返回一个布尔值,如果获取成功,返回 True ,否则返回 False . 参数 waitflag 的默认值是一个非零整数,
   表示如果琐已经被其他线程占用,那么当前线程将一直等待,只到其他线程释放,然后获取访琐. 如果将参数 waitflag 置为 0 ,
   那么当前线程会尝试获取琐,不管琐是否被其他线程占用,当前线程都不会等待. 


   The return value is ``True`` if the lock is acquired successfully,
   ``False`` if not.

   .. versionchanged:: 3.2
      The *timeout* parameter is new.

   .. versionchanged:: 3.2
      Lock acquires can now be interrupted by signals on POSIX.


.. method:: lock.release()

   Releases the lock.  The lock must have been acquired earlier, but not
   necessarily by the same thread.
   
   释放所占用的琐. 


.. method:: lock.locked()

   Return the status of the lock: ``True`` if it has been acquired by some thread,
   ``False`` if not.
   
   判断琐是否被占用. 

In addition to these methods, lock objects can also be used via the
:keyword:`with` statement, e.g.::

   import _thread

   a_lock = _thread.allocate_lock()

   with a_lock:
       print("a_lock is locked while this executes")

**Caveats:**

  .. index:: module: signal

* Threads interact strangely with interrupts: the :exc:`KeyboardInterrupt`
  exception will be received by an arbitrary thread.  (When the :mod:`signal`
  module is available, interrupts always go to the main thread.)

* Calling :func:`sys.exit` or raising the :exc:`SystemExit` exception is
  equivalent to calling :func:`_thread.exit`.

* Not all built-in functions that may block waiting for I/O allow other threads
  to run.  (The most popular ones (:func:`time.sleep`, :meth:`file.read`,
  :func:`select.select`) work as expected.)

* It is not possible to interrupt the :meth:`acquire` method on a lock --- the
  :exc:`KeyboardInterrupt` exception will happen after the lock has been acquired.

* When the main thread exits, it is system defined whether the other threads
  survive.  On most systems, they are killed without executing
  :keyword:`try` ... :keyword:`finally` clauses or executing object
  destructors.

* When the main thread exits, it does not do any of its usual cleanup (except
  that :keyword:`try` ... :keyword:`finally` clauses are honored), and the
  standard I/O files are not flushed.


