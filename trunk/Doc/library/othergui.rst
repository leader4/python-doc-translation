.. _other-gui-packages:

其他图形用户界面包
=======================================================



There are an number of extension widget sets to :mod:`tkinter`.

这有一个``tkinter``扩展部件的数量。


.. seealso::

参见：

   `Python megawidgets <http://pmw.sourceforge.net/>`_
      is a toolkit for building high-level compound widgets in Python using the
      :mod:`tkinter` package.  It consists of a set of base classes and a library of
      flexible and extensible megawidgets built on this foundation. These megawidgets
      include notebooks, comboboxes, selection widgets, paned widgets, scrolled
      widgets, dialog windows, etc.  Also, with the Pmw.Blt interface to BLT, the
      busy, graph, stripchart, tabset and vector commands are be available.
      
       是使用``tkinter``包构建高层次的符合型部件的工具箱. 它包含了建立在megawidgets
      基础上的一套灵活的和可扩展的类库。这些megawidgets包括记事本，组合框，选择部件，
      窗口部件, 滚动部件, 对话窗口等。另外, 当对BLT使用Pmw.Blt接口时，busy, graph, 
      stripchart, tabset and vector 命令都可以用。
      

      The initial ideas for Pmw were taken from the Tk ``itcl`` extensions ``[incr
      Tk]`` by Michael McLennan and ``[incr Widgets]`` by Mark Ulferts. Several of the
      megawidgets are direct translations from the itcl to Python. It offers most of
      the range of widgets that ``[incr Widgets]`` does, and is almost as complete as
      Tix, lacking however Tix's fast :class:`HList` widget for drawing trees.
      
      
      Pmw最开始的想法是采用Tk的``itcl``扩展，Michael McLennan的``[incr Tk]``
      和 Mark Ulferts 的``[incr Widgets]``. 直接把这几个megawidgets从itcl移
      植到Python. 它提供的很多部件``[incr Widgets]``已经实现了, 并且大都是Tix
      完成的。但是缺少一个绘制树的快速的Tix的``HList``部件。
      

   `Tkinter3000 Widget Construction Kit (WCK) <http://tkinter.effbot.org/>`_
      is a library that allows you to write new Tkinter widgets in pure Python.  The
      WCK framework gives you full control over widget creation, configuration, screen
      appearance, and event handling.  WCK widgets can be very fast and light-weight,
      since they can operate directly on Python data structures, without having to
      transfer data through the Tk/Tcl layer.
      
      是一个库，允许你使用纯Python编写新的Tkinter部件. WCK 框架让你完全控制
      部件的创建，配置, 屏幕外观, 和时间处理。WCK 部件非常快和轻量级, 因为
      它们能直接操作Python数据结构，而不需要通过Tk/Tcl这一层进行数据的传输。
      


The major cross-platform (Windows, Mac OS X, Unix-like) GUI toolkits that are
also available for Python:

Python的主要的跨平台(Windows, Mac OS X, 类Unix) GUI 工具包：


.. seealso::

参见：

   `PyGTK <http://www.pygtk.org/>`_
      is a set of bindings for the `GTK <http://www.gtk.org/>`_ widget set. It
      provides an object oriented interface that is slightly higher level than
      the C one. It comes with many more widgets than Tkinter provides, and has
      good Python-specific reference documentation. There are also bindings to
      `GNOME <http://www.gnome.org>`_.  One well known PyGTK application is
      `PythonCAD <http://www.pythoncad.org/>`_. An online `tutorial
      <http://www.pygtk.org/pygtk2tutorial/index.html>`_ is available.
      
      是一个绑定GTK部件的集合. 它提供了一个面向对象的接口，是略高于C的一种. 
      它配备比Tkinter更多的部件。有良好的Python文档。同样也有对GNOME的绑定。
      一个众所周知的流行的PyGTK应用程序是PythonCAD. 它的在线教程也是可用的.
      
      

   `PyQt <http://www.riverbankcomputing.co.uk/software/pyqt/>`_
      PyQt is a :program:`sip`\ -wrapped binding to the Qt toolkit.  Qt is an
      extensive C++ GUI application development framework that is
      available for Unix, Windows and Mac OS X. :program:`sip` is a tool
      for generating bindings for C++ libraries as Python classes, and
      is specifically designed for Python. The *PyQt3* bindings have a
      book, `GUI Programming with Python: QT Edition
      <http://www.commandprompt.com/community/pyqt/>`_ by Boudewijn
      Rempt. The *PyQt4* bindings also have a book, `Rapid GUI Programming
      with Python and Qt <http://www.qtrac.eu/pyqtbook.html>`_, by Mark
      Summerfield.
      
      PyQt是一个绑定于Qt的**sip**包装的工具集。Qt是一个广泛使用的C++ GUI应用程序
      开发框架。在Unix, Windows and Mac OS X都能用. **sip** 是一个生成从C++库到
      Python类的绑定的工具，是专门为Python设计的。*PyQt3* 绑定有一本《GUI Programming 
      with Python: QT Edition by Boudewijn Rempt》的书。 *PyQt4*绑定同样有一本 
      Mark Summerfield 写的《Rapid GUI Programming with Python and Qt》。
      
      

   `wxPython <http://www.wxpython.org>`_
      wxPython is a cross-platform GUI toolkit for Python that is built around
      the popular `wxWidgets <http://www.wxwidgets.org/>`_ (formerly wxWindows)
      C++ toolkit.  It provides a native look and feel for applications on
      Windows, Mac OS X, and Unix systems by using each platform's native
      widgets where ever possible, (GTK+ on Unix-like systems).  In addition to
      an extensive set of widgets, wxPython provides classes for online
      documentation and context sensitive help, printing, HTML viewing,
      low-level device context drawing, drag and drop, system clipboard access,
      an XML-based resource format and more, including an ever growing library
      of user-contributed modules.  wxPython has a book, `wxPython in Action
      <http://www.amazon.com/exec/obidos/ASIN/1932394621>`_, by Noel Rappin and
      Robin Dunn.
      
       wxPython 是一个跨平台的Python GUI 工具包。它是围绕流行的 wxWidgets 
      (原名wxWindows) C++工具包创建的。 它提供了本地外观和感觉上的应用程序。 
      在Windows, Mac OS X, and Unix 系统尽可能使用每个平台自己的原生部件。
      (GTK+ 在类Unix-like系统上). 此外它提供了广泛的部件。wxPython提供了
      联机文档和上下文相关帮助，打印，HTML查看，低级别设备上下文绘制，拖动和删除，
      访问系统剪切板，一个基于XML资源格式 等的类，它包含一个不断增长的用户贡献库模块。
      wxPython有一本《wxPython in Action》的书，是Noel Rappin 和 Robin Dunn写的.
      
      

PyGTK, PyQt, and wxPython, all have a modern look and feel and more
widgets than Tkinter. In addition, there are many other GUI toolkits for
Python, both cross-platform, and platform-specific. See the `GUI Programming
<http://wiki.python.org/moin/GuiProgramming>`_ page in the Python Wiki for a
much more complete list, and also for links to documents where the
different GUI toolkits are compared.

PyGTK, PyQt, and wxPython, 都有比Tkinter更多的具有现代化外观和感觉的部件. 
此外，Python中有更多的其他GUI工具包。它们都跨平台，也有特定平台。参见Python维基，
有更多的完整的GUI编程列表，并且有不同GUI工具包之间的比较的文章的链接。



