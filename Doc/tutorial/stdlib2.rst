.. _tut-brieftourtwo:

*********************************************
标准库概要 -- 第二部分
*********************************************

This second tour covers more advanced modules that support professional
programming needs.  These modules rarely occur in small scripts.

第二部分包含了支持专业编程工作所需的更高级的模块, 这些模块很少出现在小脚本中. 


.. _tut-output-formatting:

输出格式
=================

The :mod:`reprlib` module provides a version of :func:`repr` customized for
abbreviated displays of large or deeply nested containers::

repr提供了一个 :func:`repr` 的定制版本, 以显示大型或深度嵌套的容器: 

   >>> import reprlib
   >>> reprlib.repr(set('supercalifragilisticexpialidocious'))
   "set(['a', 'c', 'd', 'e', 'f', 'g', ...])"

The :mod:`pprint` module offers more sophisticated control over printing both
built-in and user defined objects in a way that is readable by the interpreter.
When the result is longer than one line, the "pretty printer" adds line breaks
and indentation to more clearly reveal data structure::

pprint模块给老手提供了一种解释器可读的方式深入控制内置和用户自定义对象的打印. 当输出
超过一行的时候,  "美化打印 (pretty printer) " 添加断行和标识符, 使得数据结构显示的更清晰: 

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

textwrap模块格式化文本段落以适应设定的屏宽: 

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

locale模块按访问预定好的国家信息数据库. locale 的格式化函数属性集提供了一个直接方式以分组标示格式化数字: 

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


.. _tut-templating:

模版
==========

The :mod:`string` module includes a versatile :class:`Template` class with a
simplified syntax suitable for editing by end-users.  This allows users to
customize their applications without having to alter the application.

string 提供了一个灵活多变的模版类 :template, 使用它最终用户可以用简单的进行编辑. 这使用户可以在不进行改变的情况下定制他们的应用程序. 

The format uses placeholder names formed by ``$`` with valid Python identifiers
(alphanumeric characters and underscores).  Surrounding the placeholder with
braces allows it to be followed by more alphanumeric letters with no intervening
spaces.  Writing ``$$`` creates a single escaped ``$``::

格式使用 $ 为开头的 Python 合法标识 (数字、字母和下划线) 作为占位符. 占位符外面的大括号使它可以和其它的字符不加空格混在一起.  $$ 创建一个单独的  "$" :

   >>> from string import Template
   >>> t = Template('${village}folk send $$10 to $cause.')
   >>> t.substitute(village='Nottingham', cause='the ditch fund')
   'Nottinghamfolk send $10 to the ditch fund.'

The :meth:`substitute` method raises a :exc:`KeyError` when a placeholder is not
supplied in a dictionary or a keyword argument. For mail-merge style
applications, user supplied data may be incomplete and the
:meth:`safe_substitute` method may be more appropriate --- it will leave
placeholders unchanged if data is missing::

字典或者关键字参数中缺少某个占位符的时候 substitute() 方法抛出 KeyError 异常. 在邮件-合并风格的应用程序中, 用户提供的数据可能并不完整, 也许用 safe-substitute() 方法更合适——如果数据不完整, 它保留未改动的占位符::

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

模版子类可以指定一个定制分隔符. 例如, 图像浏览器的批量命名工具可能选用百分号作为表示当前日期、图像序列号或文件格式的占位符: 

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

另一个应用是将多样化的输出格式细节从程序逻辑中分离出来. 这使得为 XML 文件, 纯文本报表, HTML web 报表定制替换模版成为可能. 


.. _tut-binary-formats:

使用二进制记录层
=======================================

The :mod:`struct` module provides :func:`pack` and :func:`unpack` functions for
working with variable length binary record formats.  The following example shows
how to loop through header information in a ZIP file without using the
:mod:`zipfile` module.  Pack codes ``"H"`` and ``"I"`` represent two and four
byte unsigned numbers respectively.  The ``"<"`` indicates that they are
standard size and in little-endian byte order::

struct模块提供 :pack 和 unpack函数用于变长二进制记录格式. 以下示例显示了如何通过ZIP文件的头信息 (压缩代码中的 "H" 和 "L" 分别传递二和四字节无符号整数) 权文博::

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


.. _tut-multi-threading:

多线程
===============

Threading is a technique for decoupling tasks which are not sequentially
dependent.  Threads can be used to improve the responsiveness of applications
that accept user input while other tasks run in the background.  A related use
case is running I/O in parallel with computations in another thread.

线程是一个分离无顺序依赖关系任务的技术. 在某些任务运行于后台的时候应用程序会变得迟缓, 线程可以提升其速度. 一个相关的应用是在I/O的同时其它线程可以并行计算. 

The following code shows how the high level :mod:`threading` module can run
tasks in background while the main program continues to run::

下面的代码显示了高级模块 :mod`threading` 如何在主程序运行的同时运行任务::

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

The principal challenge of multi-threaded applications is coordinating threads
that share data or other resources.  To that end, the threading module provides
a number of synchronization primitives including locks, events, condition
variables, and semaphores.

多线程应用程序最重要的挑战是在协调线程共享的数据和其它资源. 最终, 线程模块提供了几个基本的同步方式如锁、事件, 条件变量和信号旗. 

While those tools are powerful, minor design errors can result in problems that
are difficult to reproduce.  So, the preferred approach to task coordination is
to concentrate all access to a resource in a single thread and then use the
:mod:`queue` module to feed that thread with requests from other threads.
Applications using :class:`Queue` objects for inter-thread communication and
coordination are easier to design, more readable, and more reliable.

尽管工具很强大, 但微小的设计错误也可能造成难以挽回的故障. 因此, 更好的方法是将所有的资源访问集中到一个独立的线程中, 然后使用 Queue 模块调度该线程相应其它线程的请求. 应用程序使用 Queue 对象可以让内部线程通信和协调更容易设计, 更可读, 更可靠. 


.. _tut-logging:

日志
=======

The :mod:`logging` module offers a full featured and flexible logging system.
At its simplest, log messages are sent to a file or to ``sys.stderr``::

logging模块提供了完整和灵活的日志系统. 它最简单的用法是记录信息并发送到一个文件或 ``sys.stderr`::

   import logging
   logging.debug('Debugging information')
   logging.info('Informational message')
   logging.warning('Warning:config file %s not found', 'server.conf')
   logging.error('Error occurred')
   logging.critical('Critical error -- shutting down')

This produces the following output::

这里是输出::

   WARNING:root:Warning:config file server.conf not found
   ERROR:root:Error occurred
   CRITICAL:root:Critical error -- shutting down

By default, informational and debugging messages are suppressed and the output
is sent to standard error.  Other output options include routing messages
through email, datagrams, sockets, or to an HTTP Server.  New filters can select
different routing based on message priority: :const:`DEBUG`, :const:`INFO`,
:const:`WARNING`, :const:`ERROR`, and :const:`CRITICAL`.

默认情况下捕获信息和调试消息并将输出发送到标准错误流. 其它可选的路由信息方式通过email, 数据报文, socket或者HTTP Server. 基于消息属性, 新的过滤器可以选择不同的路由: DEBUG, INFO, WARNING, ERROR 和CRITICAL. 

The logging system can be configured directly from Python or can be loaded from
a user editable configuration file for customized logging without altering the
application.

日志系统可以直接在 Python 中定制, 也可以不经过应用程序直接在一个用户可编辑的配置文件中加载. 


.. _tut-weak-references:

弱引用
===============

Python does automatic memory management (reference counting for most objects and
:term:`garbage collection` to eliminate cycles).  The memory is freed shortly
after the last reference to it has been eliminated.

Python 自动进行内存管理 (对大多数的对象进行引用计数和:term:`垃圾回收`以循环利用) 在最后一个引用消失后, 内存会很快释放. 

This approach works fine for most applications but occasionally there is a need
to track objects only as long as they are being used by something else.
Unfortunately, just tracking them creates a reference that makes them permanent.
The :mod:`weakref` module provides tools for tracking objects without creating a
reference.  When the object is no longer needed, it is automatically removed
from a weakref table and a callback is triggered for weakref objects.  Typical
applications include caching objects that are expensive to create::

这个工作方式对大多数应用程序工作良好, 但是偶尔会需要跟踪对象来做一些事. 不幸的是, 仅仅为跟踪它们创建引用也会使其长期存在.  weakref 模块提供了不用创建引用的跟踪对象工具, 一旦对象不再存在, 它自动从弱引用表上删除并触发回调. 典型的应用包括捕获难以构造的对象: 

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


.. _tut-list-tools:

链表工具
============================

Many data structure needs can be met with the built-in list type. However,
sometimes there is a need for alternative implementations with different
performance trade-offs.

很多数据结构可能会用到内置链表类型. 然而, 有时可能需要不同性能代价的实现. 

The :mod:`array` module provides an :class:`array()` object that is like a list
that stores only homogeneous data and stores it more compactly.  The following
example shows an array of numbers stored as two byte unsigned binary numbers
(typecode ``"H"``) rather than the usual 16 bytes per entry for regular lists of
Python int objects::

array模块提供了一个类似链表的array()对象, 它仅仅是存储数据, 更为紧凑. 
以下的示例演示了一个存储双字节无符号整数的数组 (类型编码 `` "H" ``) 而非存储16字节 Python 整数对象的普通正规链表:

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

collections模块提供了类似链表的`deque() 对象, 它从左边添加 (append) 
和弹出 (pop) 更快, 但是在内部查询更慢. 这些对象更适用于队列实现和广度优先的树搜索::

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

除了链表的替代实现, 该库还提供了 bisect 这样的模块以操作存储链表: 

   >>> import bisect
   >>> scores = [(100, 'perl'), (200, 'tcl'), (400, 'lua'), (500, 'python')]
   >>> bisect.insort(scores, (300, 'ruby'))
   >>> scores
   [(100, 'perl'), (200, 'tcl'), (300, 'ruby'), (400, 'lua'), (500, 'python')]

The :mod:`heapq` module provides functions for implementing heaps based on
regular lists.  The lowest valued entry is always kept at position zero.  This
is useful for applications which repeatedly access the smallest element but do
not want to run a full list sort::

heapq提供了基于正规链表的堆实现. 最小的值总是保持在0点. 这在希望循环访问最小元素但是不想执行完整堆排序的时候非常有用::

   >>> from heapq import heapify, heappop, heappush
   >>> data = [1, 3, 5, 7, 9, 2, 4, 6, 8, 0]
   >>> heapify(data)                      # rearrange the list into heap order
   >>> heappush(data, -5)                 # add a new entry
   >>> [heappop(data) for i in range(3)]  # fetch the three smallest entries
   [-5, 0, 1]


.. _tut-decimal-fp:

十进制浮点数算法
=================================

The :mod:`decimal` module offers a :class:`Decimal` datatype for decimal
floating point arithmetic.  Compared to the built-in :class:`float`
implementation of binary floating point, the class is especially helpful for

:mod:`decimal` 模块提供了一个 :class:`Decimal` 针对十进制浮点小数运算的数据类型. 与内
置的数据类型 :class:`float`  (针对二进制浮点小数)  相比而言, 它对以下几种情况更为有效

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

  decimal 模块提供了一个 Decimal 数据类型用于浮点数计算. 相比内置的二进制浮点数实现 `float`, 新类型特别适用于: 

  金融应用和其它需要精确十进制表达的场合, 

  控制精度, 

  控制舍入以适应法律或者规定要求, 

  确保十进制数位精度, 

  或者用户希望用作数学计算的场合. 

For example, calculating a 5% tax on a 70 cent phone charge gives different
results in decimal floating point and binary floating point. The difference
becomes significant if the results are rounded to the nearest cent::

例如, 计算 70 分电话费的 5% 税计算, 十进制浮点数和二进制浮点数计算结果的差别如下. 如果在分值上舍入, 这个差别就很重要了::

   >>> from decimal import *
   >>> round(Decimal('0.70') * Decimal('1.05'), 2)
   Decimal('0.74')
   >>> round(.70 * 1.05, 2)
   0.73

The :class:`Decimal` result keeps a trailing zero, automatically inferring four
place significance from multiplicands with two place significance.  Decimal
reproduces mathematics as done by hand and avoids issues that can arise when
binary floating point cannot exactly represent decimal quantities.

Decimal 的结果总是保有结尾的0, 自动从两位精度延伸到4位. Decimal重现了手工的数学运算, 这就确保了二进制浮点数无法精确保有的数据精度. 

Exact representation enables the :class:`Decimal` class to perform modulo
calculations and equality tests that are unsuitable for binary floating point::

高精度使 Decimal 可以执行二进制浮点数无法进行的模运算和等值测试::

   >>> Decimal('1.00') % Decimal('.10')
   Decimal('0.00')
   >>> 1.00 % 0.10
   0.09999999999999995

   >>> sum([Decimal('0.1')]*10) == Decimal('1.0')
   True
   >>> sum([0.1]*10) == 1.0
   False

The :mod:`decimal` module provides arithmetic with as much precision as needed::

decimal提供了高精度算法::

   >>> getcontext().prec = 36
   >>> Decimal(1) / Decimal(7)
   Decimal('0.142857142857142857142857142857142857')



