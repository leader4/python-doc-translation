.. _tut-brieftour:

***************************************************
Brief Tour of the Standard Library 标准库的简明介绍
***************************************************


.. _tut-os-interface:

Operating System Interface 与操作系统的接口
===========================================

The :mod:`os` module provides dozens of functions for interacting with the
operating system::

   >>> import os
   >>> os.getcwd()      # Return the current working directory
   'C:\\Python31'
   >>> os.chdir('/server/accesslogs')   # Change current working directory
   >>> os.system('mkdir today')   # Run the command mkdir in the system shell
   0

:mod:`os` 模块提供了许多与操作系统交互的接口::

   >>> import os
   >>> os.getcwd()      # 返回当前工作目录
   'C:\\Python31' 
   >>> os.chdir('/server/accesslogs')   # 改变当前工作目录 
   >>> os.system('mkdir today')   # 在系统的shell中运行mkdir命令
   0


Be sure to use the ``import os`` style instead of ``from os import *``.  This
will keep :func:`os.open` from shadowing the built-in :func:`open` function which
operates much differently.

记住要使用 ``import os`` 这种风格而不是 ``from os import *`` 。 这样会避免使得 :fun:`os.open`
覆盖了功能截然不同的 :func:`os.open` 。

.. index:: builtin: help

The built-in :func:`dir` and :func:`help` functions are useful as interactive
aids for working with large modules like :mod:`os`::

   >>> import os
   >>> dir(os)
   <returns a list of all module functions>
   >>> help(os)
   <returns an extensive manual page created from the module's docstrings>

内置函数 :func:`dir` 和 :func:`help` 对于处理像 :mod:`os`:: 这样的大型模块来说是一种十分有效的
工具::

   >>> import os
   >>> dir(os)
   <returns a list of all module functions>
   >>> help(os)
   <returns an extensive manual page created from the module's docstrings>

For daily file and directory management tasks, the :mod:`shutil` module provides
a higher level interface that is easier to use::

   >>> import shutil
   >>> shutil.copyfile('data.db', 'archive.db')
   >>> shutil.move('/build/executables', 'installdir')

对于日常文件和目录的管理， :mod:`shutil` 模块提供了更便捷、更高层次的接口::

>>> import shutil
   >>> shutil.copyfile('data.db', 'archive.db')
   >>> shutil.move('/build/executables', 'installdir')

.. _tut-file-wildcards:

File Wildcards 文件的通配符
=========================

The :mod:`glob` module provides a function for making file lists from directory
wildcard searches::

   >>> import glob
   >>> glob.glob('*.py')
   ['primes.py', 'random.py', 'quote.py']

:mod:`glob` 模块提供了这样一个函数，这个函数使我们能以通配符的方式搜索某个目录下的特定文件，
并列出它们::

   >>> import glob
   >>> glob.glob('*.py')
   ['primes.py', 'random.py', 'quote.py']

.. _tut-command-line-arguments:

Command Line Arguments 命令行参数
================================

Common utility scripts often need to process command line arguments. These
arguments are stored in the :mod:`sys` module's *argv* attribute as a list.  For
instance the following output results from running ``python demo.py one two
three`` at the command line::

   >>> import sys
   >>> print(sys.argv)
   ['demo.py', 'one', 'two', 'three']

一些实用的脚本通常需要处理命令行参数。这些参数被 :mod:`sys` 模块的 *argv* 
属性以列表的方式存储起来。下例中，命令行中运行 ``python demo.py one two three`` ，其结果便能
说明这一点::

   >>> import sys
   >>> print(sys.argv)
   ['demo.py', 'one', 'two', 'three']

The :mod:`getopt` module processes *sys.argv* using the conventions of the Unix
:func:`getopt` function.  More powerful and flexible command line processing is
provided by the :mod:`argparse` module.

:mod:`getopt` 模块使用Unix通常用的 :func:`getopt` 函数去处理 *sys.argv* 。 
:mod:`argparse` 提供了更强大且更灵活的处理方法。

.. _tut-stderr:

Error Output Redirection and Program Termination 错误的重定向输出和程序的终止
========================================================================

The :mod:`sys` module also has attributes for *stdin*, *stdout*, and *stderr*.
The latter is useful for emitting warnings and error messages to make them
visible even when *stdout* has been redirected::

   >>> sys.stderr.write('Warning, log file not found starting a new one\n')
   Warning, log file not found starting a new one

:mod:`sys` 模块还包括了 *stdin*, *stdout*, *stderr* 属性。而最后一个属性 *stderr* 可以
有效地使警告和出错信息以可见的方式传输出来，即使是 *stdout* 被重定向了::

   >>> sys.stderr.write('Warning, log file not found starting a new one\n')
   Warning, log file not found starting a new one

The most direct way to terminate a script is to use ``sys.exit()``.

最直接地结束整个程序的方法是调用 ``sys.exit()`` 。


.. _tut-string-pattern-matching:

String Pattern Matching 字符串模式的区配
=====================================

The :mod:`re` module provides regular expression tools for advanced string
processing. For complex matching and manipulation, regular expressions offer
succinct, optimized solutions::

   >>> import re
   >>> re.findall(r'\bf[a-z]*', 'which foot or hand fell fastest')
   ['foot', 'fell', 'fastest']
   >>> re.sub(r'(\b[a-z]+) \1', r'\1', 'cat in the the hat')
   'cat in the hat'

:mod:`re` 模块提供了一些常规语句工具，以便对字符串做进一步的处理。对于复杂的区配
和操作，这些语句提供了简洁的、优化了的解决方法::

   >>> import re
   >>> re.findall(r'\bf[a-z]*', 'which foot or hand fell fastest')
   ['foot', 'fell', 'fastest']
   >>> re.sub(r'(\b[a-z]+) \1', r'\1', 'cat in the the hat')
   'cat in the hat'

When only simple capabilities are needed, string methods are preferred because
they are easier to read and debug::

   >>> 'tea for too'.replace('too', 'two')
   'tea for two'

而当只需要一些简单功能的时候，我们更倾向于字符串方法，因为它们更容易阅读和调试::

   >>> 'tea for too'.replace('too', 'two')
   'tea for two'

.. _tut-mathematics:

Mathematics 数学处理
===================

The :mod:`math` module gives access to the underlying C library functions for
floating point math::

   >>> import math
   >>> math.cos(math.pi / 4)
   0.70710678118654757
   >>> math.log(1024, 2)
   10.0

:mod:`math` 模块使我们可以访问底层的C语言库里关于小数的一些函数::

   >>> import math
   >>> math.cos(math.pi / 4)
   0.70710678118654757
   >>> math.log(1024, 2)
   10.0

The :mod:`random` module provides tools for making random selections::

   >>> import random
   >>> random.choice(['apple', 'pear', 'banana'])
   'apple'
   >>> random.sample(range(100), 10)   # sampling without replacement
   [30, 83, 16, 4, 8, 81, 41, 50, 18, 33]
   >>> random.random()    # random float
   0.17970987693706186
   >>> random.randrange(6)    # random integer chosen from range(6)
   4

:mod:`random` 模块提供了产生随机数的工具::

   >>> import random
   >>> random.choice(['apple', 'pear', 'banana'])
   'apple'
   >>> random.sample(range(100), 10)   # 生成无需更换的随机抽样样本
   [30, 83, 16, 4, 8, 81, 41, 50, 18, 33]
   >>> random.random()    # 生成随机小数
   0.17970987693706186
   >>> random.randrange(6)    # 以range(6)里的数为基准生成随机整数
   4

The SciPy project <http://scipy.org> has many other modules for numerical
computations.

Scipy工程 <http://scipy.org> 里有许多关于数值计算的模块。

.. _tut-internet-access:

Internet Access 访问互联网
========================

There are a number of modules for accessing the internet and processing internet
protocols. Two of the simplest are :mod:`urllib.request` for retrieving data
from urls and :mod:`smtplib` for sending mail::

   >>> from urllib.request import urlopen
   >>> for line in urlopen('http://tycho.usno.navy.mil/cgi-bin/timer.pl'):
   ...     line = line.decode('utf-8')  # Decoding the binary data to text.
   ...     if 'EST' in line or 'EDT' in line:  # look for Eastern Time
   ...         print(line)

   <BR>Nov. 25, 09:43:32 PM EST

   >>> import smtplib
   >>> server = smtplib.SMTP('localhost')
   >>> server.sendmail('soothsayer@example.org', 'jcaesar@example.org',
   ... """To: jcaesar@example.org
   ... From: soothsayer@example.org
   ...
   ... Beware the Ides of March.
   ... """)
   >>> server.quit()

python里包含了许多访问互联网和处理互联网协议的模块。其中最简单的两个分别是，从网址中检索数据的

:mod:`urllib.request` 模块，和发送邮件的 :mod:`smtplib` 模块::

   >>> from urllib.request import urlopen
   >>> for line in urlopen('http://tycho.usno.navy.mil/cgi-bin/timer.pl'):
   ...     line = line.decode('utf-8')  # 将二进制文件解码成普通字符
   ...     if 'EST' in line or 'EDT' in line:  # 查找西方国家的时间
   ...         print(line)

   <BR>Nov. 25, 09:43:32 PM EST

   >>> import smtplib
   >>> server = smtplib.SMTP('localhost')
   >>> server.sendmail('soothsayer@example.org', 'jcaesar@example.org',
   ... """To: jcaesar@example.org
   ... From: soothsayer@example.org
   ...
   ... Beware the Ides of March.
   ... """)
   >>> server.quit()

(Note that the second example needs a mailserver running on localhost.)

(注意：第二个例子需要本地有一个邮件服务器。)

.. _tut-dates-and-times:

Dates and Times 日期和时间
========================

The :mod:`datetime` module supplies classes for manipulating dates and times in
both simple and complex ways. While date and time arithmetic is supported, the
focus of the implementation is on efficient member extraction for output
formatting and manipulation.  The module also supports objects that are timezone
aware. ::

   >>> # dates are easily constructed and formatted
   >>> from datetime import date
   >>> now = date.today()
   >>> now
   datetime.date(2003, 12, 2)
   >>> now.strftime("%m-%d-%y. %d %b %Y is a %A on the %d day of %B.")
   '12-02-03. 02 Dec 2003 is a Tuesday on the 02 day of December.'

   >>> # dates support calendar arithmetic
   >>> birthday = date(1964, 7, 31)
   >>> age = now - birthday
   >>> age.days
   14368

:mod:`datetime` 模块提供了操作日期和时间的类，包括了简单和复杂两种方式。当我们知道了时间和日期的
算法后，工作的重心便放在了如何有效地格式化输出和操作之上了。该模块也提供了区分时区的对象。::

   >>> # dates are easily constructed and formatted
   >>> from datetime import date
   >>> now = date.today()
   >>> now
   datetime.date(2003, 12, 2)
   >>> now.strftime("%m-%d-%y. %d %b %Y is a %A on the %d day of %B.")
   '12-02-03. 02 Dec 2003 is a Tuesday on the 02 day of December.'

   >>> # dates support calendar arithmetic
   >>> birthday = date(1964, 7, 31)
   >>> age = now - birthday
   >>> age.days
   14368

.. _tut-data-compression:

Data Compression 数据的压缩
==========================

Common data archiving and compression formats are directly supported by modules
including: :mod:`zlib`, :mod:`gzip`, :mod:`bz2`, :mod:`zipfile` and
:mod:`tarfile`. ::

   >>> import zlib
   >>> s = b'witch which has which witches wrist watch'
   >>> len(s)
   41
   >>> t = zlib.compress(s)
   >>> len(t)
   37
   >>> zlib.decompress(t)
   b'witch which has which witches wrist watch'
   >>> zlib.crc32(s)
   226805979

有些模块可以支持常规的数据压缩和解压，这些模块块包括： :mod:`zlib`, :mod:`gzip`, 
:mod:`zipfile` 和 :mod:`tarfile`.::

   >>> import zlib
   >>> s = b'witch which has which witches wrist watch'
   >>> len(s)
   41
   >>> t = zlib.compress(s)
   >>> len(t)
   37
   >>> zlib.decompress(t)
   b'witch which has which witches wrist watch'
   >>> zlib.crc32(s)
   226805979

.. _tut-performance-measurement:

Performance Measurement 性能测试
===============================

Some Python users develop a deep interest in knowing the relative performance of
different approaches to the same problem. Python provides a measurement tool
that answers those questions immediately.

一些Python的使用者对相同问题的不同解决方法的相对性能优劣有着极大的兴趣。而Python也提供了一些测试
工具，使得这些问题的答案一目了然。

For example, it may be tempting to use the tuple packing and unpacking feature
instead of the traditional approach to swapping arguments. The :mod:`timeit`
module quickly demonstrates a modest performance advantage::

   >>> from timeit import Timer
   >>> Timer('t=a; a=b; b=t', 'a=1; b=2').timeit()
   0.57535828626024577
   >>> Timer('a,b = b,a', 'a=1; b=2').timeit()
   0.54962537085770791

例如，我们会使用tuple的打包和解包的特性而不是传统的方法去接收参数。 :mod:`timeit` 模块可以很快地
显示出性能上的优势，即使这些优势很微小::

   >>> from timeit import Timer
   >>> Timer('t=a; a=b; b=t', 'a=1; b=2').timeit()
   0.57535828626024577
   >>> Timer('a,b = b,a', 'a=1; b=2').timeit()
   0.54962537085770791

In contrast to :mod:`timeit`'s fine level of granularity, the :mod:`profile` and
:mod:`pstats` modules provide tools for identifying time critical sections in
larger blocks of code.

和 :mod:`timeit` 良好的精确性不同的是， :mod:`profile` 模块和 :mod:`pstats` 模块提供了一些
以大块代码的方式辨别临界时间的工具。

.. _tut-quality-control:

Quality Control 质量控制
=======================

One approach for developing high quality software is to write tests for each
function as it is developed and to run those tests frequently during the
development process.

一种开发高质量软件的方法是，针对每一个功能，在假使它们已经开发完成的状态下，编写一些测试程序，而且在
开发的过程中，不断地去运行这些测试程序。

The :mod:`doctest` module provides a tool for scanning a module and validating
tests embedded in a program's docstrings.  Test construction is as simple as
cutting-and-pasting a typical call along with its results into the docstring.
This improves the documentation by providing the user with an example and it
allows the doctest module to make sure the code remains true to the
documentation::

   def average(values):
       """Computes the arithmetic mean of a list of numbers.

       >>> print(average([20, 30, 70]))
       40.0
       """
       return sum(values) / len(values)

   import doctest
   doctest.testmod()   # automatically validate the embedded tests

:mod:`doctest` 模块提供了工具去浏览一个模块并通过嵌入在文档中的测试程序进行有效性测试。
测试的构成简单到只需将这个模块的调用过程和结果进行剪切和粘贴操作，保存到文档当中。通过在文档中
给用户呈现一个例子，从而提高了文档的可读性。同时，它还确保了代码是忠实于文档的::

   def average(values):
       """Computes the arithmetic mean of a list of numbers.

       >>> print(average([20, 30, 70]))
       40.0
       """
       return sum(values) / len(values)

   import doctest
   doctest.testmod()   # 自动地通过嵌入的测试程序进行有效性检测

The :mod:`unittest` module is not as effortless as the :mod:`doctest` module,
but it allows a more comprehensive set of tests to be maintained in a separate
file::

   import unittest

   class TestStatisticalFunctions(unittest.TestCase):

       def test_average(self):
           self.assertEqual(average([20, 30, 70]), 40.0)
           self.assertEqual(round(average([1, 5, 7]), 1), 4.3)
           self.assertRaises(ZeroDivisionError, average, [])
           self.assertRaises(TypeError, average, 20, 30, 70)

   unittest.main() # Calling from the command line invokes all tests

:mod:`unittest` 模块并没有 :mod:`doctest` 这么轻松简单，但它在一个独立维护的文件中，提供了
更综合的测试集::

   import unittest

   class TestStatisticalFunctions(unittest.TestCase):

       def test_average(self):
           self.assertEqual(average([20, 30, 70]), 40.0)
           self.assertEqual(round(average([1, 5, 7]), 1), 4.3)
           self.assertRaises(ZeroDivisionError, average, [])
           self.assertRaises(TypeError, average, 20, 30, 70)

   unittest.main() # 通过命令行调用所有的测试程序

.. _tut-batteries-included:

Batteries Included 充电区
========================

Python has a "batteries included" philosophy.  This is best seen through the
sophisticated and robust capabilities of its larger packages. For example:

Python有一个原理的充电区。这是你了解python原理和它的各种包的强大功能的最佳方式。例如：

* The :mod:`xmlrpc.client` and :mod:`xmlrpc.server` modules make implementing
  remote procedure calls into an almost trivial task.  Despite the modules
  names, no direct knowledge or handling of XML is needed.

* :mod:`xmlrpc.client` 模块和 :mod:`xmlrpc.server` 模块使得远距离程序的调用变得简单
  便捷。你不用去管任何模块的名字，也不必掌握XML的知识。

* The :mod:`email` package is a library for managing email messages, including
  MIME and other RFC 2822-based message documents. Unlike :mod:`smtplib` and
  :mod:`poplib` which actually send and receive messages, the email package has
  a complete toolset for building or decoding complex message structures
  (including attachments) and for implementing internet encoding and header
  protocols.

* :mod:`email` 包是一个处理email消息的库，包括MIME和其它以RFC 2822为基准的消息文档。
  它不像 :mod:`poplib` 模块和 :mod:`smtplib` 模块只发送和接收消息，email包有一个完整的
  工具集去创建或者解码复杂的消息结构（包括附件）和执和互联网编码和包头协议。

* The :mod:`xml.dom` and :mod:`xml.sax` packages provide robust support for
  parsing this popular data interchange format. Likewise, the :mod:`csv` module
  supports direct reads and writes in a common database format. Together, these
  modules and packages greatly simplify data interchange between Python
  applications and other tools.

* :mod:`xml.dom` 包和 :mod:`xml.sax` 包为解析这种流行的数据交换格式提供了强大的支持。
  同样地， :mod:`csv` 模块对读写常规的数据库文件提供了支持。这些包和模块结合在一起，大大
  简化了Python应用程序和其它工具的数据交换方法。

* Internationalization is supported by a number of modules including
  :mod:`gettext`, :mod:`locale`, and the :mod:`codecs` package.

* 一些模块如 :mode:`gettext` , :mod:`locale` 和包 :mod:`codecs`，为Python的国际化，
  提供了支持。


