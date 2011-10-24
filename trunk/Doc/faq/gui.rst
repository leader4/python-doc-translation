:tocdepth: 2

======================================
图形用户界面
======================================

.. contents::

.. XXX need review for Python 3.


常见GUI问题
================================

Python都可以用那些跨平台的GUI工具?
========================================================

Depending on what platform(s) you are aiming at, there are several.  Some
of them haven't been ported to Python 3 yet.  At least `Tkinter`_ and `Qt`_
are known to be Python 3-compatible.

.. XXX check links

Tkinter
-------

Python默认包含有面向对象的Tcl/Tk图形集合的标准封装，称作tkinter。
这绝对是最简单的安装和使用它的方式了。
虽然它涉及了许多Python的二进制程序包。
进一步了解TK，或想看到源码，请查阅Tcl/Tk的主页。Tcl/Tk同时被完全移植到了苹果操作系统，Windows和Unix平台上。

wxWidgets
---------

wxWidgets (http://www.wxwidgets.org) 是一个免费的跨平台的用C++写的图形库，它可在不同平台上创建原生的操作系统图形组件。



它被绑定到Python，Perl，Ruby等多种语言上。

wxPython (http://www.wxpython.org) 是针对
wxwidgets的Python语言封装包。 当然它跟不上官方 wxWidgets的更新速度，
但它总是提供大量的纯Python代码的功能和其他语言封装无法做到的新特性。

wxPython的用户群和开发社区都很活跃。

wxWidgets 和 wxPython都是免费开源的
他们的许可允许用户开发商业化的产品，和免费、共享软件一样毫无限制。


Qt
---


还有一种绑定是针对QT的(可以用PyQt也可以用PySide）。
PyQt 目前比PySide更成熟些，
但是它要求你购买Riverbank Computing 的使用许可，如果你想开发私有软件的话。PySide是完全免费的，它由诺基亚收购的Qt部门（以前是另外一家公司）开发，是PyQt的最佳代替品。

Qt 4.5 升级到了LGPL license; 同时，商业许可证仍由诺基亚提供。

Gtk+
----


PyGtk是由James Henstridge封装的GTK的python程序包。
可查阅 <http://www.pygtk.org>.


FLTK
----

Python bindings for `the FLTK toolkit <http://www.fltk.org>`_, a simple yet
powerful and mature cross-platform windowing system, are available from `the
PyFLTK project <http://pyfltk.sourceforge.net>`_.

FLTK 工具的Python绑定, 一个成熟、简单易用，功能强大的跨平台窗口系统，由PyFLTK项目维护。


FOX
----

A wrapper for `the FOX toolkit <http://www.fox-toolkit.org/>`_ called `FXpy
<http://fxpy.sourceforge.net/>`_ is available.  FOX supports both Unix variants
and Windows.

FOX工具的封装，被称作 FXpy。FOX 支持
Unix variants 和 Windows.



OpenGL
------

For OpenGL bindings, see `PyOpenGL <http://pyopengl.sourceforge.net>`_.

OpenGL的绑定, 请查阅 PyOpenGL.


Python有特定平台的GUI工具吗？
========================================================

`The Mac port <http://python.org/download/mac>`_ by Jack Jansen has a rich and
ever-growing set of modules that support the native Mac toolbox calls.  The port
supports MacOS X's Carbon libraries.

By installing the `PyObjc Objective-C bridge
<http://pyobjc.sourceforge.net>`_, Python programs can use MacOS X's
Cocoa libraries. See the documentation that comes with the Mac port.

:ref:`Pythonwin <windows-faq>` by Mark Hammond includes an interface to the
Microsoft Foundation Classes and a Python programming environment
that's written mostly in Python using the MFC classes.


Tkinter questions
=================

How do I freeze Tkinter applications?
-------------------------------------

Freeze is a tool to create stand-alone applications.  When freezing Tkinter
applications, the applications will not be truly stand-alone, as the application
will still need the Tcl and Tk libraries.

One solution is to ship the application with the Tcl and Tk libraries, and point
to them at run-time using the :envvar:`TCL_LIBRARY` and :envvar:`TK_LIBRARY`
environment variables.

To get truly stand-alone applications, the Tcl scripts that form the library
have to be integrated into the application as well. One tool supporting that is
SAM (stand-alone modules), which is part of the Tix distribution
(http://tix.sourceforge.net/).

Build Tix with SAM enabled, perform the appropriate call to
:c:func:`Tclsam_init`, etc. inside Python's
:file:`Modules/tkappinit.c`, and link with libtclsam and libtksam (you
might include the Tix libraries as well).


Can I have Tk events handled while waiting for I/O?
---------------------------------------------------

Yes, and you don't even need threads!  But you'll have to restructure your I/O
code a bit.  Tk has the equivalent of Xt's :c:func:`XtAddInput()` call, which allows you
to register a callback function which will be called from the Tk mainloop when
I/O is possible on a file descriptor.  Here's what you need::

   from Tkinter import tkinter
   tkinter.createfilehandler(file, mask, callback)

The file may be a Python file or socket object (actually, anything with a
fileno() method), or an integer file descriptor.  The mask is one of the
constants tkinter.READABLE or tkinter.WRITABLE.  The callback is called as
follows::

   callback(file, mask)

You must unregister the callback when you're done, using ::

   tkinter.deletefilehandler(file)

Note: since you don't know *how many bytes* are available for reading, you can't
use the Python file object's read or readline methods, since these will insist
on reading a predefined number of bytes.  For sockets, the :meth:`recv` or
:meth:`recvfrom` methods will work fine; for other files, use
``os.read(file.fileno(), maxbytecount)``.


I can't get key bindings to work in Tkinter: why?
-------------------------------------------------

An often-heard complaint is that event handlers bound to events with the
:meth:`bind` method don't get handled even when the appropriate key is pressed.

The most common cause is that the widget to which the binding applies doesn't
have "keyboard focus".  Check out the Tk documentation for the focus command.
Usually a widget is given the keyboard focus by clicking in it (but not for
labels; see the takefocus option).
