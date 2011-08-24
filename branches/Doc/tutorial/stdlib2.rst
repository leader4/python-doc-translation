.. _tut-brieftourtwo:

*************************************************************************
Brief Tour of the Standard Library -- Part II 标准库的简明介绍（第二部分）
*************************************************************************

This second tour covers more advanced modules that support professional
programming needs.  These modules rarely occur in small scripts.

第二部分涵盖了许多关于专业编程需求的高级模块. 这些模块很少出现在小的程序当中. 

.. _tut-output-formatting:

Output Formatting 格式化输出 
============================

The :mod:`reprlib` module provides a version of :func:`repr` customized for
abbreviated displays of large or deeply nested containers::

   >>> import reprlib
   >>> reprlib.repr(set('supercalifragilisticexpialidocious'))
   "set(['a', 'c', 'd', 'e', 'f', 'g', ...])"

:mod:`reprlib` 模块提供了一个缩略显示内容很多或者深度很广的嵌套容器的自定义版本 :mode:`repr` ::

   >>> import reprlib
   >>> reprlib.repr(set('supercalifragilisticexpialidocious'))
   "set(['a', 'c', 'd', 'e', 'f', 'g', ...])"

The :mod:`pprint` module offers more sophisticated control over printing both
built-in and user defined objects in a way that is readable by the interpreter.
When the result is longer than one line, the "pretty printer" adds line breaks
and indentation to more clearly reveal data structure::

   >>> import pprint
   >>> t = [[[['black', 'cyan'], 'white', ['green', 'red']], [['magenta',
   ...     'yellow'], 'blue']]]
   ...
   >>> pprint.pprint(t, width=30)
   [[[['black', 'cyan'],
      'white',
      ['green', 'red']],
     [['magenta', 'yellow'],
      'blue']]]

:mod:`pprint` 模块通过一种能哆让解释器读懂的方法, 来对内置的和用户自定义的一些对象的输出进行
更复杂的操作. 当返回的结果大于一行时, "pretty printer" 功能模块会加上断行符和适当的缩进, 以
使数据的结构更加清晰明朗::

   >>> import pprint
   >>> t = [[[['black', 'cyan'], 'white', ['green', 'red']], [['magenta',
   ...     'yellow'], 'blue']]]
   ...
   >>> pprint.pprint(t, width=30)
   [[[['black', 'cyan'],
      'white',
      ['green', 'red']],
     [['magenta', 'yellow'],
      'blue']]]

The :mod:`textwrap` module formats paragraphs of text to fit a given screen
width::

   >>> import textwrap
   >>> doc = """The wrap() method is just like fill() except that it returns
   ... a list of strings instead of one big string with newlines to separate
   ... the wrapped lines."""
   ...
   >>> print(textwrap.fill(doc, width=40))
   The wrap() method is just like fill()
   except that it returns a list of strings
   instead of one big string with newlines
   to separate the wrapped lines.

:mod:`textwrap` 模块会根据屏幕的宽度而适当地去调整文本段落::

   >>> import textwrap
   >>> doc = """The wrap() method is just like fill() except that it returns
   ... a list of strings instead of one big string with newlines to separate
   ... the wrapped lines."""
   ...
   >>> print(textwrap.fill(doc, width=40))
   The wrap() method is just like fill()
   except that it returns a list of strings
   instead of one big string with newlines
   to separate the wrapped lines.

The :mod:`locale` module accesses a database of culture specific data formats.
The grouping attribute of locale's format function provides a direct way of
formatting numbers with group separators::

   >>> import locale
   >>> locale.setlocale(locale.LC_ALL, 'English_United States.1252')
   'English_United States.1252'
   >>> conv = locale.localeconv()          # get a mapping of conventions
   >>> x = 1234567.8
   >>> locale.format("%d", x, grouping=True)
   '1,234,567'
   >>> locale.format_string("%s%.*f", (conv['currency_symbol'],
   ...                      conv['frac_digits'], x), grouping=True)
   '$1,234,567.80'

:mod:`locale` 模块访问一个包含因特定语言环境而异的数据格式的数据库. 
locale模块的格式化函数的分组属性, 可以用组别分离器, 直接地去格式化数字::

   >>> import locale
   >>> locale.setlocale(locale.LC_ALL, 'English_United States.1252')
   'English_United States.1252'
   >>> conv = locale.localeconv()          # 得到一个常规框架的映射
   >>> x = 1234567.8
   >>> locale.format("%d", x, grouping=True)
   '1,234,567'
   >>> locale.format_string("%s%.*f", (conv['currency_symbol'],
   ...                      conv['frac_digits'], x), grouping=True)
   '$1,234,567.80'

.. _tut-templating:

Templating 模板化
=================

The :mod:`string` module includes a versatile :class:`Template` class with a
simplified syntax suitable for editing by end-users.  This allows users to
customize their applications without having to alter the application.

:mod:`string` 模块包括一个多元化的 :class:`Template` 类, 为用户提供简化了的语法格式, 
使其可以方便的编辑. 这样可以使用户自定义自己的程序而不用去修改程序本身. 

The format uses placeholder names formed by ``$`` with valid Python identifiers
(alphanumeric characters and underscores).  Surrounding the placeholder with
braces allows it to be followed by more alphanumeric letters with no intervening
spaces.  Writing ``$$`` creates a single escaped ``$``::

   >>> from string import Template
   >>> t = Template('${village}folk send $$10 to $cause.')
   >>> t.substitute(village='Nottingham', cause='the ditch fund')
   'Nottinghamfolk send $10 to the ditch fund.'

这种格式使用由 ``$`` 和合法的Python标识（包括文字、数字和下划线）组成的占位符名称. 将这些占位符
包含在一对花括号里时允许周围存在更多的字母或者数字而不用理会是否有空格. 必要时使用 ``$$`` 去表示
单独的 ``$`` ::

   >>> from string import Template
   >>> t = Template('${village}folk send $$10 to $cause.')
   >>> t.substitute(village='Nottingham', cause='the ditch fund')
   'Nottinghamfolk send $10 to the ditch fund.'

The :meth:`substitute` method raises a :exc:`KeyError` when a placeholder is not
supplied in a dictionary or a keyword argument. For mail-merge style
applications, user supplied data may be incomplete and the
:meth:`safe_substitute` method may be more appropriate --- it will leave
placeholders unchanged if data is missing::

   >>> t = Template('Return the $item to $owner.')
   >>> d = dict(item='unladen swallow')
   >>> t.substitute(d)
   Traceback (most recent call last):
     . . .
   KeyError: 'owner'
   >>> t.safe_substitute(d)
   'Return the unladen swallow to $owner.'

当一个字典或者关键字参数没有给占位符提供相应的值时,  :meth:`substitute` 方法会抛出一个
:exc:`KeyError` 异常. 对于像mail-merge风格的应用程序, 用户可能会提供不完整的数据, 此时, 
:meth:`safe_substitute` 方法可能会更适合——当数据缺失的时候, 它不会改变占位符::

   >>> t = Template('Return the $item to $owner.')
   >>> d = dict(item='unladen swallow')
   >>> t.substitute(d)
   Traceback (most recent call last):
     . . .
   KeyError: 'owner'
   >>> t.safe_substitute(d)
   'Return the unladen swallow to $owner.'

Template subclasses can specify a custom delimiter.  For example, a batch
renaming utility for a photo browser may elect to use percent signs for
placeholders such as the current date, image sequence number, or file format::

   >>> import time, os.path
   >>> photofiles = ['img_1074.jpg', 'img_1076.jpg', 'img_1077.jpg']
   >>> class BatchRename(Template):
   ...     delimiter = '%'
   >>> fmt = input('Enter rename style (%d-date %n-seqnum %f-format):  ')
   Enter rename style (%d-date %n-seqnum %f-format):  Ashley_%n%f

   >>> t = BatchRename(fmt)
   >>> date = time.strftime('%d%b%y')
   >>> for i, filename in enumerate(photofiles):
   ...     base, ext = os.path.splitext(filename)
   ...     newname = t.substitute(d=date, n=i, f=ext)
   ...     print('{0} --> {1}'.format(filename, newname))

   img_1074.jpg --> Ashley_0.jpg
   img_1076.jpg --> Ashley_1.jpg
   img_1077.jpg --> Ashley_2.jpg

Template类的子类可以指定一个自己的分隔符. 例如, 现在有一大批文件的重命名工作, 针对的是一个照
片浏览器, 它可能会选择使用百分符号将当前时间、图片的序列号或者文件格式分隔出来作为占位符::

   >>> import time, os.path
   >>> photofiles = ['img_1074.jpg', 'img_1076.jpg', 'img_1077.jpg']
   >>> class BatchRename(Template):
   ...     delimiter = '%'
   >>> fmt = input('Enter rename style (%d-date %n-seqnum %f-format):  ')
   Enter rename style (%d-date %n-seqnum %f-format):  Ashley_%n%f

   >>> t = BatchRename(fmt)
   >>> date = time.strftime('%d%b%y')
   >>> for i, filename in enumerate(photofiles):
   ...     base, ext = os.path.splitext(filename)
   ...     newname = t.substitute(d=date, n=i, f=ext)
   ...     print('{0} --> {1}'.format(filename, newname))

   img_1074.jpg --> Ashley_0.jpg
   img_1076.jpg --> Ashley_1.jpg
   img_1077.jpg --> Ashley_2.jpg

Another application for templating is separating program logic from the details
of multiple output formats.  This makes it possible to substitute custom
templates for XML files, plain text reports, and HTML web reports.

另一个用于模板化的应用程序是将项目的逻辑按多种输出格式的细节分离开来. 这使得从传统的模板形式转化
为XML文件、纯文本形式和html网页成为了可能. 

.. _tut-binary-formats:

Working with Binary Data Record Layouts 用二进制数拓纪录布局的处理
==================================================================

The :mod:`struct` module provides :func:`pack` and :func:`unpack` functions for
working with variable length binary record formats.  The following example shows
how to loop through header information in a ZIP file without using the
:mod:`zipfile` module.  Pack codes ``"H"`` and ``"I"`` represent two and four
byte unsigned numbers respectively.  The ``"<"`` indicates that they are
standard size and in little-endian byte order::

   import struct
   data = open('myfile.zip', 'rb').read()
   
   start = 0
   for i in range(3):                      # show the first 3 file headers
       start += 14
       fields = struct.unpack('<IIIHH', data[start:start+16])
       crc32, comp_size, uncomp_size, filenamesize, extra_size = fields

       start += 16
       filename = data[start:start+filenamesize]
       start += filenamesize
       extra = data[start:start+extra_size]
       print(filename, hex(crc32), comp_size, uncomp_size)

       start += extra_size + comp_size     # skip to the next header

:mod:`struct` 模块一些函数, 如 :fun:`pack` 和 :fun:`unpack` 函数去处理长度可变的二进制
记录格式. 下面这个例子演示了如何在不使用 :mod:`zipfile` 模块的情况下去循环得到一个ZIP文件的
标题信息. 包代码 ``H`` 和 ``I`` 分别表示两个和四个字节的无符号数字. 而 ``<`` 则表示它们是标准
大小并以字节大小的顺序排列在后面::

   import struct

   data = open('myfile.zip', 'rb').read()
   start = 0
   for i in range(3):                      # 显示最开始的3个标题
       start += 14
       fields = struct.unpack('<IIIHH', data[start:start+16])
       crc32, comp_size, uncomp_size, filenamesize, extra_size = fields

       start += 16
       filename = data[start:start+filenamesize]
       start += filenamesize
       extra = data[start:start+extra_size]
       print(filename, hex(crc32), comp_size, uncomp_size)

       start += extra_size + comp_size     # 跳到另一个标题

.. _tut-multi-threading:

Multi-threading 多线程
======================

Threading is a technique for decoupling tasks which are not sequentially
dependent.  Threads can be used to improve the responsiveness of applications
that accept user input while other tasks run in the background.  A related use
case is running I/O in parallel with computations in another thread.

线程是一种使没有顺序关系的任务并发执行的技术. 线程可以用来改进应用程序的响应方式使这些程序可
在接受用户输入的同时在后台执行另一些操作. 一个与此相关的例子是运行输入输出程序的同时在另一个
线程序中执行计算操作. 

The following code shows how the high level :mod:`threading` module can run
tasks in background while the main program continues to run::

   import threading, zipfile

   class AsyncZip(threading.Thread):
       def __init__(self, infile, outfile):
           threading.Thread.__init__(self)
           self.infile = infile
           self.outfile = outfile
       def run(self):
           f = zipfile.ZipFile(self.outfile, 'w', zipfile.ZIP_DEFLATED)
           f.write(self.infile)
           f.close()
           print('Finished background zip of:', self.infile)

   background = AsyncZip('mydata.txt', 'myarchive.zip')
   background.start()
   print('The main program continues to run in foreground.')

   background.join()    # Wait for the background task to finish
   print('Main program waited until background was done.')

下面的代码显示了高级模块 :mod:`threading` 如何实现主程序执行的同时在后台执行另一个相应的程
序::

   import threading, zipfile

   class AsyncZip(threading.Thread):
       def __init__(self, infile, outfile):
           threading.Thread.__init__(self)
           self.infile = infile
           self.outfile = outfile
       def run(self):
           f = zipfile.ZipFile(self.outfile, 'w', zipfile.ZIP_DEFLATED)
           f.write(self.infile)
           f.close()
           print('Finished background zip of:', self.infile)

   background = AsyncZip('mydata.txt', 'myarchive.zip')
   background.start()
   print('The main program continues to run in foreground.')

   background.join()    # 等待后台任务结束
   print('Main program waited until background was done.')

The principal challenge of multi-threaded applications is coordinating threads
that share data or other resources.  To that end, the threading module provides
a number of synchronization primitives including locks, events, condition
variables, and semaphores.

多线层应用程序的最大挑战就是协调行线程之间数据或者其它资源的共享. 为此, 线程模块提供了许多同步
原始函数, 包括锁定、条件变量和信号. 

While those tools are powerful, minor design errors can result in problems that
are difficult to reproduce.  So, the preferred approach to task coordination is
to concentrate all access to a resource in a single thread and then use the
:mod:`queue` module to feed that thread with requests from other threads.
Applications using :class:`Queue` objects for inter-thread communication and
coordination are easier to design, more readable, and more reliable.

虽然有这些强大的工具, 但设计上的一个小错误仍然可以导至难以恢复的问题. 因此, 对于协调各线程我们
更倾向于把的有的访问集中在一个单独的线程上, 这个线程使用 :mod:`queue` 模块把其它线程的请求全
部集中起来. 应用程序使用 :class:`Queue` 的对象来进行跨线程的交流和协调可以使得设计变得更简单, 
而且更易阅读, 更可靠. 

.. _tut-logging:

Logging 日志
============

The :mod:`logging` module offers a full featured and flexible logging system.
At its simplest, log messages are sent to a file or to ``sys.stderr``::

   import logging
   logging.debug('Debugging information')
   logging.info('Informational message')
   logging.warning('Warning:config file %s not found', 'server.conf')
   logging.error('Error occurred')
   logging.critical('Critical error -- shutting down')
:mod:`logging` 模块提供一整套富有特色且灵活的日志系统. 用最简单的方式说, 就是把日志消息传送
给一个文件或者 ``sys.stderr`::

   import logging
   logging.debug('Debugging information')
   logging.info('Informational message')
   logging.warning('Warning:config file %s not found', 'server.conf')
   logging.error('Error occurred')
   logging.critical('Critical error -- shutting down')

This produces the following output::

   WARNING:root:Warning:config file server.conf not found
   ERROR:root:Error occurred
   CRITICAL:root:Critical error -- shutting down

上述会产生以下的输出::

   WARNING:root:Warning:config file server.conf not found
   ERROR:root:Error occurred
   CRITICAL:root:Critical error -- shutting down

By default, informational and debugging messages are suppressed and the output
is sent to standard error.  Other output options include routing messages
through email, datagrams, sockets, or to an HTTP Server.  New filters can select
different routing based on message priority: :const:`DEBUG`, :const:`INFO`,
:const:`WARNING`, :const:`ERROR`, and :const:`CRITICAL`.

默认的情况下, 信息和调试消息会被捕捉, 并发送给标准错误流. 其它的一些输出选项包括经由邮件、数据报
套接字或者发送给一个HTTP服务器的路由消息. 新的地滤器可以选择不同的基于消息优先级的的路由, 而消息
的优先级有: :const:`DEBUG`, :const:`INFO`, :const:`WARING`, :const:`ERROR`, 和
:const:`CRIFICAL`.

The logging system can be configured directly from Python or can be loaded from
a user editable configuration file for customized logging without altering the
application.

日志系统可以被Python语言配置, 或者被一个用户的可编辑的配置文件加载, 以此去自定义日志而不用去修
程序本身. 

.. _tut-weak-references:

Weak References 弱引用
======================

Python does automatic memory management (reference counting for most objects and
:term:`garbage collection` to eliminate cycles).  The memory is freed shortly
after the last reference to it has been eliminated.

Python语言自动管理内存（大多数对象都有一个对它的引用而 :term:`garbage collection` 对它们进
行回收）. 当最后一个引用结束之后内存即刻被回收. 

This approach works fine for most applications but occasionally there is a need
to track objects only as long as they are being used by something else.
Unfortunately, just tracking them creates a reference that makes them permanent.
The :mod:`weakref` module provides tools for tracking objects without creating a
reference.  When the object is no longer needed, it is automatically removed
from a weakref table and a callback is triggered for weakref objects.  Typical
applications include caching objects that are expensive to create::

   >>> import weakref, gc
   >>> class A:
   ...     def __init__(self, value):
   ...             self.value = value
   ...     def __repr__(self):
   ...             return str(self.value)
   ...
   >>> a = A(10)                   # create a reference
   >>> d = weakref.WeakValueDictionary()
   >>> d['primary'] = a            # does not create a reference
   >>> d['primary']                # fetch the object if it is still alive
   10
   >>> del a                       # remove the one reference
   >>> gc.collect()                # run garbage collection right away
   0
   >>> d['primary']                # entry was automatically removed
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
       d['primary']                # entry was automatically removed
     File "C:/python31/lib/weakref.py", line 46, in __getitem__
       o = self.data[key]()
   KeyError: 'primary'
  
这种机制对大多数应用程序来说都是有效的, 但也有偶然情况, 只有当它们被其它东西引用的时候才需要去
跟踪对象. 不幸的是, 仅仅是跟踪它们也需要创建一个引用.  :mod:`weakref` 模块提供一些工具可以
达到同样的效果而不用去创建一个引用. 当不再需要这个对象的时候, 它会自动地从一个weakref表中移除
然后鉵发对weadref对象的回调. 通常应用程序都会对那些创建时花费较多时间的对象提供一个缓存::

   >>> import weakref, gc
   >>> class A:
   ...     def __init__(self, value):
   ...             self.value = value
   ...     def __repr__(self):
   ...             return str(self.value)
   ...
   >>> a = A(10)                   # 创建一个引用
   >>> d = weakref.WeakValueDictionary()
   >>> d['primary'] = a            # 没有创建一个引用
   >>> d['primary']                # 如果存在的话获取这个对旬
   10
   >>> del a                       # 移除这个引和
   >>> gc.collect()                # 直接调用回收机制
   0
   >>> d['primary']                # 调用的入口已经自动被移除了
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
       d['primary']                # 调用的入口已经自动被移除了
     File "C:/python31/lib/weakref.py", line 46, in __getitem__
       o = self.data[key]()
   KeyError: 'primary'

.. _tut-list-tools:

Tools for Working with Lists 处理列表的工具
===========================================

Many data structure needs can be met with the built-in list type. However,
sometimes there is a need for alternative implementations with different
performance trade-offs.

许多数据的结构都需要用到内置的列表类型. 但有时候需要在可选择地不同呈现方式中进行权衡. 

The :mod:`array` module provides an :class:`array()` object that is like a list
that stores only homogeneous data and stores it more compactly.  The following
example shows an array of numbers stored as two byte unsigned binary numbers
(typecode ``"H"``) rather than the usual 16 bytes per entry for regular lists of
Python int objects::

   >>> from array import array
   >>> a = array('H', [4000, 10, 700, 22222])
   >>> sum(a)
   26932
   >>> a[1:3]
   array('H', [10, 700])

:mod:`array` 模块提供了一个 :class:`array()` 对象, 这个对象像一个列表一样存储同一类型的数据, 
而且更简洁. 下面这个例子演示了将一组数字以两个字节的无符号整数（类形码 ``H`` )形式存储为一个数组
而不是通常的Python的list对象的16字节的形式::

   >>> from array import array
   >>> a = array('H', [4000, 10, 700, 22222])
   >>> sum(a)
   26932
   >>> a[1:3]
   array('H', [10, 700])

The :mod:`collections` module provides a :class:`deque()` object that is like a
list with faster appends and pops from the left side but slower lookups in the
middle. These objects are well suited for implementing queues and breadth first
tree searches::

   >>> from collections import deque
   >>> d = deque(["task1", "task2", "task3"])
   >>> d.append("task4")
   >>> print("Handling", d.popleft())
   Handling task1

   unsearched = deque([starting_node])
   def breadth_first_search(unsearched):
       node = unsearched.popleft()
       for m in gen_moves(node):
           if is_goal(m):
               return m
           unsearched.append(m)

:mod:`cllections` 模块提供了一个 :class:`deque()` 对象, 它可以像一个列表一样在左边进行
快速的apend和pop操作, 但在内部查寻时相对较慢. 这些对象可以方便地成为一个队列和地行广度优先树
搜索::

   >>> from collections import deque
   >>> d = deque(["task1", "task2", "task3"])
   >>> d.append("task4")
   >>> print("Handling", d.popleft())
   Handling task1

   unsearched = deque([starting_node])
   def breadth_first_search(unsearched):
       node = unsearched.popleft()
       for m in gen_moves(node):
           if is_goal(m):
               return m
           unsearched.append(m)

In addition to alternative list implementations, the library also offers other
tools such as the :mod:`bisect` module with functions for manipulating sorted
lists::

   >>> import bisect
   >>> scores = [(100, 'perl'), (200, 'tcl'), (400, 'lua'), (500, 'python')]
   >>> bisect.insort(scores, (300, 'ruby'))
   >>> scores
   [(100, 'perl'), (200, 'tcl'), (300, 'ruby'), (400, 'lua'), (500, 'python')]

此外, 标准库里也提供了一些其它的工具, 如 :mod:`bisect` 模块, 它有一些对列表进行排序的函数::

   >>> import bisect
   >>> scores = [(100, 'perl'), (200, 'tcl'), (400, 'lua'), (500, 'python')]
   >>> bisect.insort(scores, (300, 'ruby'))
   >>> scores
   [(100, 'perl'), (200, 'tcl'), (300, 'ruby'), (400, 'lua'), (500, 'python')]

The :mod:`heapq` module provides functions for implementing heaps based on
regular lists.  The lowest valued entry is always kept at position zero.  This
is useful for applications which repeatedly access the smallest element but do
not want to run a full list sort::

   >>> from heapq import heapify, heappop, heappush
   >>> data = [1, 3, 5, 7, 9, 2, 4, 6, 8, 0]
   >>> heapify(data)                      # rearrange the list into heap order
   >>> heappush(data, -5)                 # add a new entry
   >>> [heappop(data) for i in range(3)]  # fetch the three smallest entries
   [-5, 0, 1]

:mod:`heapq` 模块提供了一些函数通过常规列表去实现堆. 最低层的入口通常都在零处. 这对于重复访
问一些很小的元素但又不想对整个列表进行排序的应用程序来说十分有效::

   >>> from heapq import heapify, heappop, heappush
   >>> data = [1, 3, 5, 7, 9, 2, 4, 6, 8, 0]
   >>> heapify(data)                      # 将列表重新整理成堆
   >>> heappush(data, -5)                 # 增加一个新入口
   >>> [heappop(data) for i in range(3)]  # 取出这三个最小的入口
   [-5, 0, 1]

.. _tut-decimal-fp:

Decimal Floating Point Arithmetic 十进制浮点数的运算
====================================================

The :mod:`decimal` module offers a :class:`Decimal` datatype for decimal
floating point arithmetic.  Compared to the built-in :class:`float`
implementation of binary floating point, the class is especially helpful for

:mod:`decimal` 模块提供了一个 :class:`Decimal` 针对十进制浮点小数运算的数据类型. 与内
置的数据类型 :class:`float` （针对二进制浮点小数） 相比而言, 它对以下几种情况更为有效

* financial applications and other uses which require exact decimal
  representation,
  
* 金融方面的应用程序和其它需要准确显示小数的地方

* control over precision,

* 需要精确控制, 

* control over rounding to meet legal or regulatory requirements,
  
* 需要四舍五入以满足法制或者监管要求, 
    
* tracking of significant decimal places, or

* 需要跟踪有意义的小数部分, 即精度, 或者, 

* applications where the user expects the results to match calculations done by
  hand.
  
* 一些用户希望结果符合自己的计算要求的应用程序. 

For example, calculating a 5% tax on a 70 cent phone charge gives different
results in decimal floating point and binary floating point. The difference
becomes significant if the results are rounded to the nearest cent::

   >>> from decimal import *
   >>> round(Decimal('0.70') * Decimal('1.05'), 2)
   Decimal('0.74')
   >>> round(.70 * 1.05, 2)
   0.73

例如, 计算七毛钱话费的5％的锐收, 用十进制浮点小数和二进制浮点小数, 得到的结果会不同. 如果结果以分
的精确度来舍入的话, 这种差异就会变得很重要::

   >>> from decimal import *
   >>> round(Decimal('0.70') * Decimal('1.05'), 2)
   Decimal('0.74')
   >>> round(.70 * 1.05, 2)
   0.73

The :class:`Decimal` result keeps a trailing zero, automatically inferring four
place significance from multiplicands with two place significance.  Decimal
reproduces mathematics as done by hand and avoids issues that can arise when
binary floating point cannot exactly represent decimal quantities.

:class:`Decimal` 的结果会在末尾追加0, 自动从有两位有效数字的乘数相乘中判断应有四位有效数字. 
Decimal复制了手工运算的精度, 避免了二进制浮点小数不能准确表示十进数精度而产生的问题. 

Exact representation enables the :class:`Decimal` class to perform modulo
calculations and equality tests that are unsuitable for binary floating point::

   >>> Decimal('1.00') % Decimal('.10')
   Decimal('0.00')
   >>> 1.00 % 0.10
   0.09999999999999995

   >>> sum([Decimal('0.1')]*10) == Decimal('1.0')
   True
   >>> sum([0.1]*10) == 1.0
   False

精确的显示使得 :class:`Decimal` 可以进行模运算和判断值的等同性, 而这些是二进制浮点数不适合的::

   >>> Decimal('1.00') % Decimal('.10')
   Decimal('0.00')
   >>> 1.00 % 0.10
   0.09999999999999995

   >>> sum([Decimal('0.1')]*10) == Decimal('1.0')
   True
   >>> sum([0.1]*10) == 1.0
   False

The :mod:`decimal` module provides arithmetic with as much precision as needed::

   >>> getcontext().prec = 36
   >>> Decimal(1) / Decimal(7)
   Decimal('0.142857142857142857142857142857142857')

:mod:`decimal` 模块可以实现各种需求的精度运算::

   >>> Decimal('1.00') % Decimal('.10')
   Decimal('0.00')
   >>> 1.00 % 0.10
   0.09999999999999995

   >>> sum([Decimal('0.1')]*10) == Decimal('1.0')
   True
   >>> sum([0.1]*10) == 1.0
   False

