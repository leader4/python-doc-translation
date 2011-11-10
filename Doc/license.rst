.. highlightlang:: none

.. _history-and-license:

********************************
软件历史与许可 
********************************


软件历史
===============================

Python的创始人为吉多·范罗苏姆 (Guido van Rossum) . 在1989年圣诞节期间的阿姆斯特丹, 吉多为了打发圣诞节的无趣, 决心开发一个新的脚本解释程序, 作为ABC语言的一种继承. 之所以选中Python作为程序的名字, 是因为他是一个蒙提·派森的飞行马戏团的爱好者. ABC是由吉多参加设计的一种教学语言. 就吉多本人看来, ABC这种语言非常优美和强大, 是专门为非专业程序员设计的. 但是ABC语言并没有成功, 究其原因, 吉多认为是非开放造成的. 吉多决心在Python中避免这一错误, 并取得了非常好的效果, 完美结合了如C、C++和Java等其他语言. [1]

就这样, Python在吉多手中诞生了. 实际上, 第一个实现是在Mac机上. 可以说, Python是从ABC发展起来, 主要受到了Modula-3 (另一种相当优美且强大的语言, 为小型团体所设计的) 的影响. 并且结合了Unix shell和C的习惯. 

目前吉多·范罗苏姆仍然是Python的主要开发者, 决定整个Python语言的发展方向, Python社区经常称呼他是仁慈的独裁者. 

Python 2.0于2000年10月16日发布, 主要是实现了完整的垃圾回收, 并且支持Unicode. 同时, 整个开发过程更加透明, 社区对开发进程的影响逐渐扩大. Python 3.0于2008年12月3日发布, 此版不完全兼容之前的Python代码. 不过, 很多新特性后来也被移植到旧的Python 2.6/2.7版本. 

Python的设计哲学是 "优雅" 、 "明确" 、 "简单" . 因此, Perl语言中 "总有多种方法来做同一件事" 的理念在Python开发者中通常是难以忍受的. Python开发者的哲学是 "用一种方法, 最好是只有一种方法来做一件事" . 在设计Python语言时, 如果面临多种选择, Python开发者一般会拒绝花哨的语法, 而选择明确的没有或者很少有歧义的语法. 由于这种设计观念的差异, Python源代码通常认为比Perl具备更好的可读性, 并且能够支撑大规模的软件开发. 这些准则被称为Python格言. 在Python解释器内运行import this可以获得完整的列表. 

Python开发人员尽量避开不成熟或者不重要的优化. 一些针对非重要部位的加快运行速度的补丁通常不会被合并到Python内. 所以很多人认为Python很慢. 不过, 根据二八定律, 大多数程序对速度要求不高. 在某些对运行速度要求很高的情况, Python程序员倾向于使用JIT技术, 或者用使用C/C++语言改写这部分程序. 目前可用的JIT技术是Psyco. 

Python是完全面向对象的语言. 函数、模块、数字、字符串都是对象. 并且完全支持继承、重载、派生、多继承, 有益于增强代码的复用性. Python支持重载运算符, 因此Python也支持泛型设计. 相对于Lisp这种传统的函数式编程语言, Python对函数式编程只提供了有限的支持. 有两个标准库(functools, itertools)提供了Haskell和Standard ML中久经考验的函数式编程工具. 

虽然Python可能被粗略地分类为 "脚本语言"  (script language) , 但实际上一些大规模软件开发计划例如Zope、Mnet及BitTorrent, Google也广泛地使用它. Python的支持者较喜欢称它为一种高阶动态编程语言, 原因是 "脚本语言" 泛指仅作简单编程任务的语言, 如shell script、JavaScript等只能处理简单任务的编程语言, 并不能与Python相提并论. 

Python本身被设计为可扩展的. 并非所有的特性和功能都集成到语言核心. Python提供了丰富的API和工具, 以便程序员能够轻松地使用C语言、C++、Cython来编写扩展模块. Python解释器本身也可以被集成到其它需要脚本语言的程序内. 因此, 很多人还把Python作为一种 "胶水语言"  (glue language) 使用. 使用Python将其他语言编写的程序进行集成和封装. 在Google内部的很多项目使用C++编写性能要求极高的部分, 然后用Python调用相应的模块. [来源请求]很多游戏, 如 EVE Online 使用Python来处理游戏中繁多的逻辑. 

Python经常被用于Web开发. 比如, 通过mod_wsgi模块, Apache可以运行用Python编写的Web程序. Python定义了WSGI标准应用接口来协调Http服务器与基于Python的Web程序之间的通信. 一些Web框架, 如Django,TurboGears,web2py,Zope,flask等, 可以让程序员轻松地开发和管理复杂的Web程序. 

在很多操作系统里, Python是标准的系统组件. 大多数Linux发行版以及NetBSD、OpenBSD和Mac OS X都集成了Python, 可以在终端下直接运行Python. 有一些Linux发行版的安装器使用Python语言编写, 比如Ubuntu的Ubiquity安装器,Red Hat Linux和Fedora的Anaconda安装器. Gentoo Linux使用Python来编写它的Portage包管理系统. Python标准库包含了多个调用操作系统功能的库. 通过pywin32这个第三方软件包, Python能够访问Windows的COM服务及其它Windows API. 使用IronPython, Python程序能够直接调用.Net Framework. 一般说来, Python编写的系统管理脚本在可读性、性能、代码重用度、扩展性几方面都优于普通的shell脚本. 

NumPy,SciPy,Matplotlib可以让Python程序员编写科学计算程序. PyQt、PySide、wxPython、PyGTK是Python快速开发桌面应用程序的利器. 

Python对于各种网络协议的支持很完善, 因此经常被用于编写服务器软件、网络爬虫. 第三方库Twisted支持异步网络编程和多数标准的网络协议(包含客户端和服务器), 并且提供了多种工具, 被广泛用于编写高性能的服务器软件. 

很多游戏使用C++编写图形显示等高性能模块, 而使用Python或者Lua编写游戏的逻辑、服务器. 相较于Python, Lua的功能更简单、体积更小; 而Python则支持更多的特性和数据类型. 

YouTube、Google、Yahoo!、NASA都在内部大量地使用Python. OLPC的操作系统Sugar项目的大多数软件都是使用Python编写. 

+----------------+--------------+------------+------------+-----------------+
| Release        | Derived from | Year       | Owner      | GPL compatible? |
+================+==============+============+============+=================+
| 0.9.0 thru 1.2 | n/a          | 1991-1995  | CWI        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 1.3 thru 1.5.2 | 1.2          | 1995-1999  | CNRI       | yes             |
+----------------+--------------+------------+------------+-----------------+
| 1.6            | 1.5.2        | 2000       | CNRI       | no              |
+----------------+--------------+------------+------------+-----------------+
| 2.0            | 1.6          | 2000       | BeOpen.com | no              |
+----------------+--------------+------------+------------+-----------------+
| 1.6.1          | 1.6          | 2001       | CNRI       | no              |
+----------------+--------------+------------+------------+-----------------+
| 2.1            | 2.0+1.6.1    | 2001       | PSF        | no              |
+----------------+--------------+------------+------------+-----------------+
| 2.0.1          | 2.0+1.6.1    | 2001       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.1.1          | 2.1+2.0.1    | 2001       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.2            | 2.1.1        | 2001       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.1.2          | 2.1.1        | 2002       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.1.3          | 2.1.2        | 2002       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.2.1          | 2.2          | 2002       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.2.2          | 2.2.1        | 2002       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.2.3          | 2.2.2        | 2002-2003  | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.3            | 2.2.2        | 2002-2003  | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.3.1          | 2.3          | 2002-2003  | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.3.2          | 2.3.1        | 2003       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.3.3          | 2.3.2        | 2003       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.3.4          | 2.3.3        | 2004       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.3.5          | 2.3.4        | 2005       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.4            | 2.3          | 2004       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.4.1          | 2.4          | 2005       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.4.2          | 2.4.1        | 2005       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.4.3          | 2.4.2        | 2006       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.4.4          | 2.4.3        | 2006       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.5            | 2.4          | 2006       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.5.1          | 2.5          | 2007       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.6            | 2.5          | 2008       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.6.1          | 2.6          | 2008       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.6.2          | 2.6.1        | 2009       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.6.3          | 2.6.2        | 2009       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 2.6.4          | 2.6.3        | 2009       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 3.0            | 2.6          | 2008       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 3.0.1          | 3.0          | 2009       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 3.1            | 3.0.1        | 2009       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 3.1.1          | 3.1          | 2009       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 3.1.2          | 3.1          | 2010       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+
| 3.2            | 3.1          | 2011       | PSF        | yes             |
+----------------+--------------+------------+------------+-----------------+

.. note::

   GPL-compatible doesn't mean that we're distributing Python under the GPL.  All
   Python licenses, unlike the GPL, let you distribute a modified version without
   making your changes open source. The GPL-compatible licenses make it possible to
   combine Python with other software that is released under the GPL; the others
   don't.

Thanks to the many outside volunteers who have worked under Guido's direction to
make these releases possible.


Terms and conditions for accessing or otherwise using Python
============================================================


.. centered:: PSF LICENSE AGREEMENT FOR PYTHON |release|

#. This LICENSE AGREEMENT is between the Python Software Foundation ("PSF"), and
   the Individual or Organization ("Licensee") accessing and otherwise using Python
   |release| software in source or binary form and its associated documentation.

#. Subject to the terms and conditions of this License Agreement, PSF hereby
   grants Licensee a nonexclusive, royalty-free, world-wide license to reproduce,
   analyze, test, perform and/or display publicly, prepare derivative works,
   distribute, and otherwise use Python |release| alone or in any derivative
   version, provided, however, that PSF's License Agreement and PSF's notice of
   copyright, i.e., "Copyright © 2001-2011 Python Software Foundation; All Rights
   Reserved" are retained in Python |release| alone or in any derivative version
   prepared by Licensee.

#. In the event Licensee prepares a derivative work that is based on or
   incorporates Python |release| or any part thereof, and wants to make the
   derivative work available to others as provided herein, then Licensee hereby
   agrees to include in any such work a brief summary of the changes made to Python
   |release|.

#. PSF is making Python |release| available to Licensee on an "AS IS" basis.
   PSF MAKES NO REPRESENTATIONS OR WARRANTIES, EXPRESS OR IMPLIED.  BY WAY OF
   EXAMPLE, BUT NOT LIMITATION, PSF MAKES NO AND DISCLAIMS ANY REPRESENTATION OR
   WARRANTY OF MERCHANTABILITY OR FITNESS FOR ANY PARTICULAR PURPOSE OR THAT THE
   USE OF PYTHON |release| WILL NOT INFRINGE ANY THIRD PARTY RIGHTS.

#. PSF SHALL NOT BE LIABLE TO LICENSEE OR ANY OTHER USERS OF PYTHON |release|
   FOR ANY INCIDENTAL, SPECIAL, OR CONSEQUENTIAL DAMAGES OR LOSS AS A RESULT OF
   MODIFYING, DISTRIBUTING, OR OTHERWISE USING PYTHON |release|, OR ANY DERIVATIVE
   THEREOF, EVEN IF ADVISED OF THE POSSIBILITY THEREOF.

#. This License Agreement will automatically terminate upon a material breach of
   its terms and conditions.

#. Nothing in this License Agreement shall be deemed to create any relationship
   of agency, partnership, or joint venture between PSF and Licensee.  This License
   Agreement does not grant permission to use PSF trademarks or trade name in a
   trademark sense to endorse or promote products or services of Licensee, or any
   third party.

#. By copying, installing or otherwise using Python |release|, Licensee agrees
   to be bound by the terms and conditions of this License Agreement.


.. centered:: BEOPEN.COM LICENSE AGREEMENT FOR PYTHON 2.0


.. centered:: BEOPEN PYTHON OPEN SOURCE LICENSE AGREEMENT VERSION 1

#. This LICENSE AGREEMENT is between BeOpen.com ("BeOpen"), having an office at
   160 Saratoga Avenue, Santa Clara, CA 95051, and the Individual or Organization
   ("Licensee") accessing and otherwise using this software in source or binary
   form and its associated documentation ("the Software").

#. Subject to the terms and conditions of this BeOpen Python License Agreement,
   BeOpen hereby grants Licensee a non-exclusive, royalty-free, world-wide license
   to reproduce, analyze, test, perform and/or display publicly, prepare derivative
   works, distribute, and otherwise use the Software alone or in any derivative
   version, provided, however, that the BeOpen Python License is retained in the
   Software, alone or in any derivative version prepared by Licensee.

#. BeOpen is making the Software available to Licensee on an "AS IS" basis.
   BEOPEN MAKES NO REPRESENTATIONS OR WARRANTIES, EXPRESS OR IMPLIED.  BY WAY OF
   EXAMPLE, BUT NOT LIMITATION, BEOPEN MAKES NO AND DISCLAIMS ANY REPRESENTATION OR
   WARRANTY OF MERCHANTABILITY OR FITNESS FOR ANY PARTICULAR PURPOSE OR THAT THE
   USE OF THE SOFTWARE WILL NOT INFRINGE ANY THIRD PARTY RIGHTS.

#. BEOPEN SHALL NOT BE LIABLE TO LICENSEE OR ANY OTHER USERS OF THE SOFTWARE FOR
   ANY INCIDENTAL, SPECIAL, OR CONSEQUENTIAL DAMAGES OR LOSS AS A RESULT OF USING,
   MODIFYING OR DISTRIBUTING THE SOFTWARE, OR ANY DERIVATIVE THEREOF, EVEN IF
   ADVISED OF THE POSSIBILITY THEREOF.

#. This License Agreement will automatically terminate upon a material breach of
   its terms and conditions.

#. This License Agreement shall be governed by and interpreted in all respects
   by the law of the State of California, excluding conflict of law provisions.
   Nothing in this License Agreement shall be deemed to create any relationship of
   agency, partnership, or joint venture between BeOpen and Licensee.  This License
   Agreement does not grant permission to use BeOpen trademarks or trade names in a
   trademark sense to endorse or promote products or services of Licensee, or any
   third party.  As an exception, the "BeOpen Python" logos available at
   http://www.pythonlabs.com/logos.html may be used according to the permissions
   granted on that web page.

#. By copying, installing or otherwise using the software, Licensee agrees to be
   bound by the terms and conditions of this License Agreement.


.. centered:: CNRI LICENSE AGREEMENT FOR PYTHON 1.6.1

#. This LICENSE AGREEMENT is between the Corporation for National Research
   Initiatives, having an office at 1895 Preston White Drive, Reston, VA 20191
   ("CNRI"), and the Individual or Organization ("Licensee") accessing and
   otherwise using Python 1.6.1 software in source or binary form and its
   associated documentation.

#. Subject to the terms and conditions of this License Agreement, CNRI hereby
   grants Licensee a nonexclusive, royalty-free, world-wide license to reproduce,
   analyze, test, perform and/or display publicly, prepare derivative works,
   distribute, and otherwise use Python 1.6.1 alone or in any derivative version,
   provided, however, that CNRI's License Agreement and CNRI's notice of copyright,
   i.e., "Copyright © 1995-2001 Corporation for National Research Initiatives; All
   Rights Reserved" are retained in Python 1.6.1 alone or in any derivative version
   prepared by Licensee.  Alternately, in lieu of CNRI's License Agreement,
   Licensee may substitute the following text (omitting the quotes): "Python 1.6.1
   is made available subject to the terms and conditions in CNRI's License
   Agreement.  This Agreement together with Python 1.6.1 may be located on the
   Internet using the following unique, persistent identifier (known as a handle):
   1895.22/1013.  This Agreement may also be obtained from a proxy server on the
   Internet using the following URL: http://hdl.handle.net/1895.22/1013."

#. In the event Licensee prepares a derivative work that is based on or
   incorporates Python 1.6.1 or any part thereof, and wants to make the derivative
   work available to others as provided herein, then Licensee hereby agrees to
   include in any such work a brief summary of the changes made to Python 1.6.1.

#. CNRI is making Python 1.6.1 available to Licensee on an "AS IS" basis.  CNRI
   MAKES NO REPRESENTATIONS OR WARRANTIES, EXPRESS OR IMPLIED.  BY WAY OF EXAMPLE,
   BUT NOT LIMITATION, CNRI MAKES NO AND DISCLAIMS ANY REPRESENTATION OR WARRANTY
   OF MERCHANTABILITY OR FITNESS FOR ANY PARTICULAR PURPOSE OR THAT THE USE OF
   PYTHON 1.6.1 WILL NOT INFRINGE ANY THIRD PARTY RIGHTS.

#. CNRI SHALL NOT BE LIABLE TO LICENSEE OR ANY OTHER USERS OF PYTHON 1.6.1 FOR
   ANY INCIDENTAL, SPECIAL, OR CONSEQUENTIAL DAMAGES OR LOSS AS A RESULT OF
   MODIFYING, DISTRIBUTING, OR OTHERWISE USING PYTHON 1.6.1, OR ANY DERIVATIVE
   THEREOF, EVEN IF ADVISED OF THE POSSIBILITY THEREOF.

#. This License Agreement will automatically terminate upon a material breach of
   its terms and conditions.

#. This License Agreement shall be governed by the federal intellectual property
   law of the United States, including without limitation the federal copyright
   law, and, to the extent such U.S. federal law does not apply, by the law of the
   Commonwealth of Virginia, excluding Virginia's conflict of law provisions.
   Notwithstanding the foregoing, with regard to derivative works based on Python
   1.6.1 that incorporate non-separable material that was previously distributed
   under the GNU General Public License (GPL), the law of the Commonwealth of
   Virginia shall govern this License Agreement only as to issues arising under or
   with respect to Paragraphs 4, 5, and 7 of this License Agreement.  Nothing in
   this License Agreement shall be deemed to create any relationship of agency,
   partnership, or joint venture between CNRI and Licensee.  This License Agreement
   does not grant permission to use CNRI trademarks or trade name in a trademark
   sense to endorse or promote products or services of Licensee, or any third
   party.

#. By clicking on the "ACCEPT" button where indicated, or by copying, installing
   or otherwise using Python 1.6.1, Licensee agrees to be bound by the terms and
   conditions of this License Agreement.


.. centered:: ACCEPT


.. centered:: CWI LICENSE AGREEMENT FOR PYTHON 0.9.0 THROUGH 1.2

Copyright © 1991 - 1995, Stichting Mathematisch Centrum Amsterdam, The
Netherlands.  All rights reserved.

Permission to use, copy, modify, and distribute this software and its
documentation for any purpose and without fee is hereby granted, provided that
the above copyright notice appear in all copies and that both that copyright
notice and this permission notice appear in supporting documentation, and that
the name of Stichting Mathematisch Centrum or CWI not be used in advertising or
publicity pertaining to distribution of the software without specific, written
prior permission.

STICHTING MATHEMATISCH CENTRUM DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS
SOFTWARE, INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS, IN NO
EVENT SHALL STICHTING MATHEMATISCH CENTRUM BE LIABLE FOR ANY SPECIAL, INDIRECT
OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE,
DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS
ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS
SOFTWARE.


Licenses and Acknowledgements for Incorporated Software
=======================================================

This section is an incomplete, but growing list of licenses and acknowledgements
for third-party software incorporated in the Python distribution.


Mersenne Twister
----------------

The :mod:`_random` module includes code based on a download from
http://www.math.keio.ac.jp/ matumoto/MT2002/emt19937ar.html. The following are
the verbatim comments from the original code::

   A C-program for MT19937, with initialization improved 2002/1/26.
   Coded by Takuji Nishimura and Makoto Matsumoto.

   Before using, initialize the state by using init_genrand(seed)
   or init_by_array(init_key, key_length).

   Copyright (C) 1997 - 2002, Makoto Matsumoto and Takuji Nishimura,
   All rights reserved.

   Redistribution and use in source and binary forms, with or without
   modification, are permitted provided that the following conditions
   are met:

    1. Redistributions of source code must retain the above copyright
       notice, this list of conditions and the following disclaimer.

    2. Redistributions in binary form must reproduce the above copyright
       notice, this list of conditions and the following disclaimer in the
       documentation and/or other materials provided with the distribution.

    3. The names of its contributors may not be used to endorse or promote
       products derived from this software without specific prior written
       permission.

   THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
   "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
   LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
   A PARTICULAR PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
   CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
   EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
   PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
   PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
   LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
   NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
   SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


   Any feedback is very welcome.
   http://www.math.keio.ac.jp/matumoto/emt.html
   email: matumoto@math.keio.ac.jp


Sockets
-------

The :mod:`socket` module uses the functions, :func:`getaddrinfo`, and
:func:`getnameinfo`, which are coded in separate source files from the WIDE
Project, http://www.wide.ad.jp/. ::

   Copyright (C) 1995, 1996, 1997, and 1998 WIDE Project.
   All rights reserved.

   Redistribution and use in source and binary forms, with or without
   modification, are permitted provided that the following conditions
   are met:
   1. Redistributions of source code must retain the above copyright
      notice, this list of conditions and the following disclaimer.
   2. Redistributions in binary form must reproduce the above copyright
      notice, this list of conditions and the following disclaimer in the
      documentation and/or other materials provided with the distribution.
   3. Neither the name of the project nor the names of its contributors
      may be used to endorse or promote products derived from this software
      without specific prior written permission.

   THIS SOFTWARE IS PROVIDED BY THE PROJECT AND CONTRIBUTORS ``AS IS'' AND
   GAI_ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
   IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
   ARE DISCLAIMED.  IN NO EVENT SHALL THE PROJECT OR CONTRIBUTORS BE LIABLE
   FOR GAI_ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
   DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
   OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
   HOWEVER CAUSED AND ON GAI_ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
   LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN GAI_ANY WAY
   OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF
   SUCH DAMAGE.


Floating point exception control
--------------------------------

The source for the :mod:`fpectl` module includes the following notice::

     ---------------------------------------------------------------------
    /                       Copyright (c) 1996.                           \
   |          The Regents of the University of California.                 |
   |                        All rights reserved.                           |
   |                                                                       |
   |   Permission to use, copy, modify, and distribute this software for   |
   |   any purpose without fee is hereby granted, provided that this en-   |
   |   tire notice is included in all copies of any software which is or   |
   |   includes  a  copy  or  modification  of  this software and in all   |
   |   copies of the supporting documentation for such software.           |
   |                                                                       |
   |   This  work was produced at the University of California, Lawrence   |
   |   Livermore National Laboratory under  contract  no.  W-7405-ENG-48   |
   |   between  the  U.S.  Department  of  Energy and The Regents of the   |
   |   University of California for the operation of UC LLNL.              |
   |                                                                       |
   |                              DISCLAIMER                               |
   |                                                                       |
   |   This  software was prepared as an account of work sponsored by an   |
   |   agency of the United States Government. Neither the United States   |
   |   Government  nor the University of California nor any of their em-   |
   |   ployees, makes any warranty, express or implied, or  assumes  any   |
   |   liability  or  responsibility  for the accuracy, completeness, or   |
   |   usefulness of any information,  apparatus,  product,  or  process   |
   |   disclosed,   or  represents  that  its  use  would  not  infringe   |
   |   privately-owned rights. Reference herein to any specific  commer-   |
   |   cial  products,  process,  or  service  by trade name, trademark,   |
   |   manufacturer, or otherwise, does not  necessarily  constitute  or   |
   |   imply  its endorsement, recommendation, or favoring by the United   |
   |   States Government or the University of California. The views  and   |
   |   opinions  of authors expressed herein do not necessarily state or   |
   |   reflect those of the United States Government or  the  University   |
   |   of  California,  and shall not be used for advertising or product   |
    \  endorsement purposes.                                              /
     ---------------------------------------------------------------------


Asynchronous socket services
----------------------------

The :mod:`asynchat` and :mod:`asyncore` modules contain the following notice::

   Copyright 1996 by Sam Rushing

                           All Rights Reserved

   Permission to use, copy, modify, and distribute this software and
   its documentation for any purpose and without fee is hereby
   granted, provided that the above copyright notice appear in all
   copies and that both that copyright notice and this permission
   notice appear in supporting documentation, and that the name of Sam
   Rushing not be used in advertising or publicity pertaining to
   distribution of the software without specific, written prior
   permission.

   SAM RUSHING DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE,
   INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS, IN
   NO EVENT SHALL SAM RUSHING BE LIABLE FOR ANY SPECIAL, INDIRECT OR
   CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS
   OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT,
   NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN
   CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.


Cookie management
-----------------

The :mod:`http.cookies` module contains the following notice::

   Copyright 2000 by Timothy O'Malley <timo@alum.mit.edu>

                  All Rights Reserved

   Permission to use, copy, modify, and distribute this software
   and its documentation for any purpose and without fee is hereby
   granted, provided that the above copyright notice appear in all
   copies and that both that copyright notice and this permission
   notice appear in supporting documentation, and that the name of
   Timothy O'Malley  not be used in advertising or publicity
   pertaining to distribution of the software without specific, written
   prior permission.

   Timothy O'Malley DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS
   SOFTWARE, INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY
   AND FITNESS, IN NO EVENT SHALL Timothy O'Malley BE LIABLE FOR
   ANY SPECIAL, INDIRECT OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
   WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS,
   WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS
   ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR
   PERFORMANCE OF THIS SOFTWARE.


Profiling
---------

The :mod:`profile` and :mod:`pstats` modules contain the following notice::

   Copyright 1994, by InfoSeek Corporation, all rights reserved.
   Written by James Roskind

   Permission to use, copy, modify, and distribute this Python software
   and its associated documentation for any purpose (subject to the
   restriction in the following sentence) without fee is hereby granted,
   provided that the above copyright notice appears in all copies, and
   that both that copyright notice and this permission notice appear in
   supporting documentation, and that the name of InfoSeek not be used in
   advertising or publicity pertaining to distribution of the software
   without specific, written prior permission.  This permission is
   explicitly restricted to the copying and modification of the software
   to remain in Python, compiled Python, or other languages (such as C)
   wherein the modified or derived code is exclusively imported into a
   Python module.

   INFOSEEK CORPORATION DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS
   SOFTWARE, INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND
   FITNESS. IN NO EVENT SHALL INFOSEEK CORPORATION BE LIABLE FOR ANY
   SPECIAL, INDIRECT OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER
   RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF
   CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN
   CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.


Execution tracing
-----------------

The :mod:`trace` module contains the following notice::

   portions copyright 2001, Autonomous Zones Industries, Inc., all rights...
   err...  reserved and offered to the public under the terms of the
   Python 2.2 license.
   Author: Zooko O'Whielacronx
   http://zooko.com/
   mailto:zooko@zooko.com

   Copyright 2000, Mojam Media, Inc., all rights reserved.
   Author: Skip Montanaro

   Copyright 1999, Bioreason, Inc., all rights reserved.
   Author: Andrew Dalke

   Copyright 1995-1997, Automatrix, Inc., all rights reserved.
   Author: Skip Montanaro

   Copyright 1991-1995, Stichting Mathematisch Centrum, all rights reserved.


   Permission to use, copy, modify, and distribute this Python software and
   its associated documentation for any purpose without fee is hereby
   granted, provided that the above copyright notice appears in all copies,
   and that both that copyright notice and this permission notice appear in
   supporting documentation, and that the name of neither Automatrix,
   Bioreason or Mojam Media be used in advertising or publicity pertaining to
   distribution of the software without specific, written prior permission.


UUencode and UUdecode functions
-------------------------------

The :mod:`uu` module contains the following notice::

   Copyright 1994 by Lance Ellinghouse
   Cathedral City, California Republic, United States of America.
                          All Rights Reserved
   Permission to use, copy, modify, and distribute this software and its
   documentation for any purpose and without fee is hereby granted,
   provided that the above copyright notice appear in all copies and that
   both that copyright notice and this permission notice appear in
   supporting documentation, and that the name of Lance Ellinghouse
   not be used in advertising or publicity pertaining to distribution
   of the software without specific, written prior permission.
   LANCE ELLINGHOUSE DISCLAIMS ALL WARRANTIES WITH REGARD TO
   THIS SOFTWARE, INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND
   FITNESS, IN NO EVENT SHALL LANCE ELLINGHOUSE CENTRUM BE LIABLE
   FOR ANY SPECIAL, INDIRECT OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
   WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
   ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT
   OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

   Modified by Jack Jansen, CWI, July 1995:
   - Use binascii module to do the actual line-by-line conversion
     between ascii and binary. This results in a 1000-fold speedup. The C
     version is still 5 times faster, though.
   - Arguments more compliant with Python standard


XML Remote Procedure Calls
--------------------------

The :mod:`xmlrpc.client` module contains the following notice::

       The XML-RPC client interface is

   Copyright (c) 1999-2002 by Secret Labs AB
   Copyright (c) 1999-2002 by Fredrik Lundh

   By obtaining, using, and/or copying this software and/or its
   associated documentation, you agree that you have read, understood,
   and will comply with the following terms and conditions:

   Permission to use, copy, modify, and distribute this software and
   its associated documentation for any purpose and without fee is
   hereby granted, provided that the above copyright notice appears in
   all copies, and that both that copyright notice and this permission
   notice appear in supporting documentation, and that the name of
   Secret Labs AB or the author not be used in advertising or publicity
   pertaining to distribution of the software without specific, written
   prior permission.

   SECRET LABS AB AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD
   TO THIS SOFTWARE, INCLUDING ALL IMPLIED WARRANTIES OF MERCHANT-
   ABILITY AND FITNESS.  IN NO EVENT SHALL SECRET LABS AB OR THE AUTHOR
   BE LIABLE FOR ANY SPECIAL, INDIRECT OR CONSEQUENTIAL DAMAGES OR ANY
   DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS,
   WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS
   ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE
   OF THIS SOFTWARE.


test_epoll
----------

The :mod:`test_epoll` contains the following notice::

  Copyright (c) 2001-2006 Twisted Matrix Laboratories.

  Permission is hereby granted, free of charge, to any person obtaining
  a copy of this software and associated documentation files (the
  "Software"), to deal in the Software without restriction, including
  without limitation the rights to use, copy, modify, merge, publish,
  distribute, sublicense, and/or sell copies of the Software, and to
  permit persons to whom the Software is furnished to do so, subject to
  the following conditions:

  The above copyright notice and this permission notice shall be
  included in all copies or substantial portions of the Software.

  THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
  EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
  MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
  NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
  LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
  OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
  WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Select kqueue
-------------

The :mod:`select` and contains the following notice for the kqueue interface::

  Copyright (c) 2000 Doug White, 2006 James Knight, 2007 Christian Heimes
  All rights reserved.

  Redistribution and use in source and binary forms, with or without
  modification, are permitted provided that the following conditions
  are met:
  1. Redistributions of source code must retain the above copyright
     notice, this list of conditions and the following disclaimer.
  2. Redistributions in binary form must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS SOFTWARE IS PROVIDED BY THE AUTHOR AND CONTRIBUTORS ``AS IS'' AND
  ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
  IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
  ARE DISCLAIMED.  IN NO EVENT SHALL THE AUTHOR OR CONTRIBUTORS BE LIABLE
  FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
  DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
  OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
  HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
  LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY
  OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF
  SUCH DAMAGE.


strtod and dtoa
---------------

The file :file:`Python/dtoa.c`, which supplies C functions dtoa and
strtod for conversion of C doubles to and from strings, is derived
from the file of the same name by David M. Gay, currently available
from http://www.netlib.org/fp/.  The original file, as retrieved on
March 16, 2009, contains the following copyright and licensing
notice::

   /****************************************************************
    *
    * The author of this software is David M. Gay.
    *
    * Copyright (c) 1991, 2000, 2001 by Lucent Technologies.
    *
    * Permission to use, copy, modify, and distribute this software for any
    * purpose without fee is hereby granted, provided that this entire notice
    * is included in all copies of any software which is or includes a copy
    * or modification of this software and in all copies of the supporting
    * documentation for such software.
    *
    * THIS SOFTWARE IS BEING PROVIDED "AS IS", WITHOUT ANY EXPRESS OR IMPLIED
    * WARRANTY.  IN PARTICULAR, NEITHER THE AUTHOR NOR LUCENT MAKES ANY
    * REPRESENTATION OR WARRANTY OF ANY KIND CONCERNING THE MERCHANTABILITY
    * OF THIS SOFTWARE OR ITS FITNESS FOR ANY PARTICULAR PURPOSE.
    *
    ***************************************************************/


OpenSSL
-------

The modules :mod:`hashlib`, :mod:`posix`, :mod:`ssl`, :mod:`crypt` use
the OpenSSL library for added performance if made available by the
operating system. Additionally, the Windows installers for Python
include a copy of the OpenSSL libraries, so we include a copy of the
OpenSSL license here::


  LICENSE ISSUES
  ==============

  The OpenSSL toolkit stays under a dual license, i.e. both the conditions of
  the OpenSSL License and the original SSLeay license apply to the toolkit.
  See below for the actual license texts. Actually both licenses are BSD-style
  Open Source licenses. In case of any license issues related to OpenSSL
  please contact openssl-core@openssl.org.

  OpenSSL License
  ---------------

    /* ====================================================================
     * Copyright (c) 1998-2008 The OpenSSL Project.  All rights reserved.
     *
     * Redistribution and use in source and binary forms, with or without
     * modification, are permitted provided that the following conditions
     * are met:
     *
     * 1. Redistributions of source code must retain the above copyright
     *    notice, this list of conditions and the following disclaimer.
     *
     * 2. Redistributions in binary form must reproduce the above copyright
     *    notice, this list of conditions and the following disclaimer in
     *    the documentation and/or other materials provided with the
     *    distribution.
     *
     * 3. All advertising materials mentioning features or use of this
     *    software must display the following acknowledgment:
     *    "This product includes software developed by the OpenSSL Project
     *    for use in the OpenSSL Toolkit. (http://www.openssl.org/)"
     *
     * 4. The names "OpenSSL Toolkit" and "OpenSSL Project" must not be used to
     *    endorse or promote products derived from this software without
     *    prior written permission. For written permission, please contact
     *    openssl-core@openssl.org.
     *
     * 5. Products derived from this software may not be called "OpenSSL"
     *    nor may "OpenSSL" appear in their names without prior written
     *    permission of the OpenSSL Project.
     *
     * 6. Redistributions of any form whatsoever must retain the following
     *    acknowledgment:
     *    "This product includes software developed by the OpenSSL Project
     *    for use in the OpenSSL Toolkit (http://www.openssl.org/)"
     *
     * THIS SOFTWARE IS PROVIDED BY THE OpenSSL PROJECT ``AS IS'' AND ANY
     * EXPRESSED OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
     * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
     * PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE OpenSSL PROJECT OR
     * ITS CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
     * SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT
     * NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
     * LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
     * HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT,
     * STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
     * ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED
     * OF THE POSSIBILITY OF SUCH DAMAGE.
     * ====================================================================
     *
     * This product includes cryptographic software written by Eric Young
     * (eay@cryptsoft.com).  This product includes software written by Tim
     * Hudson (tjh@cryptsoft.com).
     *
     */

 Original SSLeay License
 -----------------------

    /* Copyright (C) 1995-1998 Eric Young (eay@cryptsoft.com)
     * All rights reserved.
     *
     * This package is an SSL implementation written
     * by Eric Young (eay@cryptsoft.com).
     * The implementation was written so as to conform with Netscapes SSL.
     *
     * This library is free for commercial and non-commercial use as long as
     * the following conditions are aheared to.  The following conditions
     * apply to all code found in this distribution, be it the RC4, RSA,
     * lhash, DES, etc., code; not just the SSL code.  The SSL documentation
     * included with this distribution is covered by the same copyright terms
     * except that the holder is Tim Hudson (tjh@cryptsoft.com).
     *
     * Copyright remains Eric Young's, and as such any Copyright notices in
     * the code are not to be removed.
     * If this package is used in a product, Eric Young should be given attribution
     * as the author of the parts of the library used.
     * This can be in the form of a textual message at program startup or
     * in documentation (online or textual) provided with the package.
     *
     * Redistribution and use in source and binary forms, with or without
     * modification, are permitted provided that the following conditions
     * are met:
     * 1. Redistributions of source code must retain the copyright
     *    notice, this list of conditions and the following disclaimer.
     * 2. Redistributions in binary form must reproduce the above copyright
     *    notice, this list of conditions and the following disclaimer in the
     *    documentation and/or other materials provided with the distribution.
     * 3. All advertising materials mentioning features or use of this software
     *    must display the following acknowledgement:
     *    "This product includes cryptographic software written by
     *     Eric Young (eay@cryptsoft.com)"
     *    The word 'cryptographic' can be left out if the rouines from the library
     *    being used are not cryptographic related :-).
     * 4. If you include any Windows specific code (or a derivative thereof) from
     *    the apps directory (application code) you must include an acknowledgement:
     *    "This product includes software written by Tim Hudson (tjh@cryptsoft.com)"
     *
     * THIS SOFTWARE IS PROVIDED BY ERIC YOUNG ``AS IS'' AND
     * ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
     * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
     * ARE DISCLAIMED.  IN NO EVENT SHALL THE AUTHOR OR CONTRIBUTORS BE LIABLE
     * FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
     * DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
     * OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
     * HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
     * LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY
     * OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF
     * SUCH DAMAGE.
     *
     * The licence and distribution terms for any publically available version or
     * derivative of this code cannot be changed.  i.e. this code cannot simply be
     * copied and put under another distribution licence
     * [including the GNU Public Licence.]
     */


expat
-----

The :mod:`pyexpat` extension is built using an included copy of the expat
sources unless the build is configured :option:`--with-system-expat`::

  Copyright (c) 1998, 1999, 2000 Thai Open Source Software Center Ltd
                                 and Clark Cooper

  Permission is hereby granted, free of charge, to any person obtaining
  a copy of this software and associated documentation files (the
  "Software"), to deal in the Software without restriction, including
  without limitation the rights to use, copy, modify, merge, publish,
  distribute, sublicense, and/or sell copies of the Software, and to
  permit persons to whom the Software is furnished to do so, subject to
  the following conditions:

  The above copyright notice and this permission notice shall be included
  in all copies or substantial portions of the Software.

  THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
  EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
  MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
  IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
  CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
  TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
  SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


libffi
------

The :mod:`_ctypes` extension is built using an included copy of the libffi
sources unless the build is configured :option:`--with-system-libffi`::

   Copyright (c) 1996-2008  Red Hat, Inc and others.

   Permission is hereby granted, free of charge, to any person obtaining
   a copy of this software and associated documentation files (the
   ``Software''), to deal in the Software without restriction, including
   without limitation the rights to use, copy, modify, merge, publish,
   distribute, sublicense, and/or sell copies of the Software, and to
   permit persons to whom the Software is furnished to do so, subject to
   the following conditions:

   The above copyright notice and this permission notice shall be included
   in all copies or substantial portions of the Software.

   THE SOFTWARE IS PROVIDED ``AS IS'', WITHOUT WARRANTY OF ANY KIND,
   EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
   MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
   NONINFRINGEMENT.  IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
   HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
   WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
   OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
   DEALINGS IN THE SOFTWARE.


zlib
----

The :mod:`zlib` extension is built using an included copy of the zlib
sources unless the zlib version found on the system is too old to be
used for the build::

  Copyright (C) 1995-2011 Jean-loup Gailly and Mark Adler

  This software is provided 'as-is', without any express or implied
  warranty.  In no event will the authors be held liable for any damages
  arising from the use of this software.

  Permission is granted to anyone to use this software for any purpose,
  including commercial applications, and to alter it and redistribute it
  freely, subject to the following restrictions:

  1. The origin of this software must not be misrepresented; you must not
     claim that you wrote the original software. If you use this software
     in a product, an acknowledgment in the product documentation would be
     appreciated but is not required.

  2. Altered source versions must be plainly marked as such, and must not be
     misrepresented as being the original software.

  3. This notice may not be removed or altered from any source distribution.

  Jean-loup Gailly        Mark Adler
  jloup@gzip.org          madler@alumni.caltech.edu


