:mod:`webbrowser` --- 网络浏览器
=======================================================

.. module:: webbrowser
   :synopsis: Easy-to-use controller for Web browsers.
.. moduleauthor:: Fred L. Drake, Jr. <fdrake@acm.org>
.. sectionauthor:: Fred L. Drake, Jr. <fdrake@acm.org>

**Source code:** :source:`Lib/webbrowser.py`

--------------

The :mod:`webbrowser` module provides a high-level interface to allow displaying
Web-based documents to users. Under most circumstances, simply calling the
:func:`.open` function from this module will do the right thing.


 "网页浏览器" 模块提供一个高层次的的接口,该接口允许
基于Web的形式把文件显示给用户. 在大多数情况下,
简单地调用这个模块的 "open () " 函数功能将是
一件正确的事情. 

----------------------------------------------------------------------------------------------------

Under Unix, graphical browsers are preferred under X11, but text-mode browsers
will be used if graphical browsers are not available or an X11 display isn't
available.  If text-mode browsers are used, the calling process will block until
the user exits the browser.

在Unix下,图形化浏览器是基于X11图型窗口标准的首选,如果图形浏览
器不可用或者基于X11标准的图型窗口无法显示,则使用文本模式浏览器
. 如果使用文本模式浏览器,
在用户退出浏览器之前,被调用进程将被阻塞. 



---------------------------------------------------------------------------



If the environment variable :envvar:`BROWSER` exists, it is interpreted to
override the platform default list of browsers, as a :data:`os.pathsep`-separated
list of browsers to try in order.  When the value of a list part contains the
string ``%s``, then it is  interpreted as a literal browser command line to be
used with the argument URL substituted for ``%s``; if the part does not contain
``%s``, it is simply interpreted as the name of the browser to launch. [1]_


如果存在 "浏览器" 环境变量,它将
覆盖平台默认的浏览器列表,
 "os.pathsep" 尝试排序被分隔的浏览器列表. 当
列表的部分值包含字符串 "％s" 的时候,那么它解析
为浏览器命令行的url参数
,如果不包含
 "％s的``,它是简单地理解为浏览器发布的名称. 
[1]

---------------------------------------------------------------------------



For non-Unix platforms, or when a remote browser is available on Unix, the
controlling process will not wait for the user to finish with the browser, but
allow the remote browser to maintain its own windows on the display.  If remote
browsers are not available on Unix, the controlling process will launch a new
browser and wait.

对于非Unix平台上,或远程浏览器可访问unix的时候,
控制进程不会等待用户完成浏览动作,
但允许远程浏览器保持显示自己的窗口
显示屏上. 如果远程浏览器无法访问UNIX,
控制进程将推出一个新的浏览器和并且等待访问结果. 

---------------------------------------------------------------------------


The script :program:`webbrowser` can be used as a command-line interface for the
module. It accepts an URL as the argument. It accepts the following optional
parameters: ``-n`` opens the URL in a new browser window, if possible;
``-t`` opens the URL in a new browser page ("tab"). The options are,
naturally, mutually exclusive.


脚本**WebBrowser **是一个命令行接口的模块. 
它接受一个URL作为参数. 它接受
下列可选参数:  "- N"  URL在新的浏览窗口中被打开
窗口,如果可能的话; "- T" URL在浏览器新的tab中被打开,
显然,这些选项是相互排斥的. 

---------------------------------------------------------------------------

The following exception is defined:

定义以下异常: 

------------------------------------------------------------------------------------------------------------------------------------------------------

.. exception:: Error

   Exception raised when a browser control error occurs.

   当浏览器发生错误抛出该异常. 



---------------------------------------------------------------------------

定义以下函数: 


.. function:: open(url, new=0, autoraise=True)

   Display *url* using the default browser. If *new* is 0, the *url* is opened
   in the same browser window if possible.  If *new* is 1, a new browser window
   is opened if possible.  If *new* is 2, a new browser page ("tab") is opened
   if possible.  If *autoraise* is ``True``, the window is raised if possible
   (note that under many window managers this will occur regardless of the
   setting of this variable).


   使用默认的浏器览显示* URL* . 如果*new*等于0,则*url *
   会在同一浏览器窗口打开. 如果*new*等于 1,
   则打开新的浏览器窗口. 如果*新* 2,则在浏览器
   打开一个新的标签(tab). 如果* autoraise *
   ``true ``,则浏览器窗口显示在最前面 (注意,在多数
   窗口管理器中,如果设置了此变量,不管怎么样,这种情况就会出现. 

---------------------------------------------------------------------------

   Note that on some platforms, trying to open a filename using this function,
   may work and start the operating system's associated program.  However, this
   is neither supported nor portable.


   请注意,在某些平台上,使用此函数试图打开一个文件名的时候
   ,可能会启动系统的相关
   的程序然而,这是既不支持也不可移植. 

----------------------------------------------------------------------------------------------------



.. function:: open_new(url)

   Open *url* in a new window of the default browser, if possible, otherwise, open
   *url* in the only browser window.


   如果可能的话,使用默认的浏器览,在一个新的浏览窗口显示* URL* ,
   否则,仅在浏览器窗口中打开*网址*. 

----------------------------------------------------------------------------------------------------

.. function:: open_new_tab(url)

   Open *url* in a new page ("tab") of the default browser, if possible, otherwise
   equivalent to :func:`open_new`.


   如果可能的话,打开默认浏览器一个新 ( "tab" ) 窗口,并显示* URL *,
   否则相当于 "open_new ()``.

----------------------------------------------------------------------------------------------------


.. function:: get(using=None)

   Return a controller object for the browser type *using*.  If *using* is
   ``None``, return a controller for a default browser appropriate to the
   caller's environment.


   使用的浏览器类型*using *返回一个控制器对象. 如果如果
   *using*是 "None" ,则根据调用者的环境,返回一个合适默认浏览器的控制器. 

----------------------------------------------------------------------------------------------------



.. function:: register(name, constructor, instance=None)

   Register the browser type *name*.  Once a browser type is registered, the
   :func:`get` function can return a controller for that browser type.  If
   *instance* is not provided, or is ``None``, *constructor* will be called without
   parameters to create an instance when needed.  If *instance* is provided,
   *constructor* will never be called, and may be ``None``.


   注册浏览器类型*name*一旦注册了浏览器类型
   ,``get ()  "函数可以该浏览器类型的控制器
   . 如果*instance*没有提供,或者是 "None" ,
   当需要的时候,将根据无参构造函数*constructor*
   创建一个实例. 如果有提供*instance*,z则*constructor*
   将永远不会被调用,并且可能是``None``. 

----------------------------------------------------------------------------------------------------


   This entry point is only useful if you plan to either set the :envvar:`BROWSER`
   variable or call :func:`get` with a nonempty argument matching the name of a
   handler you declare.

   这个切入点只有在你打算设置
    "BROWSER" 变量或调用一个非空参数``get ()``函数并且该函数和声明的句柄名字符合的时候,才能生效. 
   你声明一个处理程序的名称相匹配的. 

---------------------------------------------------------------------------

A number of browser types are predefined.  This table gives the type names that
may be passed to the :func:`get` function and the corresponding instantiations
for the controller classes, all defined in this module.

已经预先定义好了若干个浏览器类型. 此表给出的类型
可以传递给``的get ()  "函数和
相应的控制器类的实例,此模块定义了所有类型. 

---------------------------------------------------------------------------


+-----------------------+-----------------------------------------+-------+
| Type Name             | Class Name                              | Notes |
+=======================+=========================================+=======+
| ``'mozilla'``         | :class:`Mozilla('mozilla')`             |       |
+-----------------------+-----------------------------------------+-------+
| ``'firefox'``         | :class:`Mozilla('mozilla')`             |       |
+-----------------------+-----------------------------------------+-------+
| ``'netscape'``        | :class:`Mozilla('netscape')`            |       |
+-----------------------+-----------------------------------------+-------+
| ``'galeon'``          | :class:`Galeon('galeon')`               |       |
+-----------------------+-----------------------------------------+-------+
| ``'epiphany'``        | :class:`Galeon('epiphany')`             |       |
+-----------------------+-----------------------------------------+-------+
| ``'skipstone'``       | :class:`BackgroundBrowser('skipstone')` |       |
+-----------------------+-----------------------------------------+-------+
| ``'kfmclient'``       | :class:`Konqueror()`                    | \(1)  |
+-----------------------+-----------------------------------------+-------+
| ``'konqueror'``       | :class:`Konqueror()`                    | \(1)  |
+-----------------------+-----------------------------------------+-------+
| ``'kfm'``             | :class:`Konqueror()`                    | \(1)  |
+-----------------------+-----------------------------------------+-------+
| ``'mosaic'``          | :class:`BackgroundBrowser('mosaic')`    |       |
+-----------------------+-----------------------------------------+-------+
| ``'opera'``           | :class:`Opera()`                        |       |
+-----------------------+-----------------------------------------+-------+
| ``'grail'``           | :class:`Grail()`                        |       |
+-----------------------+-----------------------------------------+-------+
| ``'links'``           | :class:`GenericBrowser('links')`        |       |
+-----------------------+-----------------------------------------+-------+
| ``'elinks'``          | :class:`Elinks('elinks')`               |       |
+-----------------------+-----------------------------------------+-------+
| ``'lynx'``            | :class:`GenericBrowser('lynx')`         |       |
+-----------------------+-----------------------------------------+-------+
| ``'w3m'``             | :class:`GenericBrowser('w3m')`          |       |
+-----------------------+-----------------------------------------+-------+
| ``'windows-default'`` | :class:`WindowsDefault`                 | \(2)  |
+-----------------------+-----------------------------------------+-------+
| ``'internet-config'`` | :class:`InternetConfig`                 | \(3)  |
+-----------------------+-----------------------------------------+-------+
| ``'macosx'``          | :class:`MacOSX('default')`              | \(4)  |
+-----------------------+-----------------------------------------+-------+


---------------------------------------------------------------------------

注意: 

(1)
   "Konqueror" is the file manager for the KDE desktop environment for Unix, and
   only makes sense to use if KDE is running.  Some way of reliably detecting KDE
   would be nice; the :envvar:`KDEDIR` variable is not sufficient.  Note also that
   the name "kfm" is used even when using the :program:`konqueror` command with KDE
   2 --- the implementation selects the best strategy for running Konqueror.


   1. "Konqueror的" 文件管理器是Unix系统中的KDE桌面环境,
   只有KDE在运行的时候才有意义. 有些可靠方式
   在检测KDE的时候效果不错,`` KDEDIR``变量是
   不足够的. 还要注意,
   使用KDE 2**konqueror**命令的时候,同时也要使用`kfm`` ---该实现会在Konqueror运行时选择
   最佳策略. 

---------------------------------------------------------------------------


(2)
   Only on Windows platforms.

   2.只有在Windows平台上. 

---------------------------------------------------------------------------

(3)
   Only on Mac OS platforms; requires the standard MacPython :mod:`ic` module.

   3.只有在Mac OS平台;要求标准MacPython "IC" 在这种组件上. 

---------------------------------------------------------------------------

(4)
   Only on Mac OS X platform.

   4. 仅适用于Mac OS X平台. 

---------------------------------------------------------------------------


下面是一些简单的例子::

   url = 'http://www.python.org/'

   # Open URL in a new tab, if a browser window is already open.
   webbrowser.open_new_tab(url + 'doc/')


   ＃如果一个浏览器窗口已经打开,则在新标签中打开网址. 

---------------------------------------------------------------------------

   # Open URL in new window, raising the window if possible.
   webbrowser.open_new(url)


   ＃该在新窗口中打开URL,如果可能的话,该窗口置顶. 

---------------------------------------------------------------------------


.. _browser-controllers:


浏览器的控制器对象
--------------------------

Browser controllers provide these methods which parallel three of the
module-level convenience functions:


浏览器控制器提供三个类似的模块级的便利功能,这些功能提供以下这些方法:

---------------------------------------------------------------------------


.. method:: controller.open(url, new=0, autoraise=True)

   Display *url* using the browser handled by this controller. If *new* is 1, a new
   browser window is opened if possible. If *new* is 2, a new browser page ("tab")
   is opened if possible.


   浏览器使用此控制器处理显示*url *. 如果*new*等于 1,则一个新的浏览器窗口打开,
   如*new* 等于如果*新*2,则打开一个新的浏览器页面 ( "标签" ) . 

---------------------------------------------------------------------------


.. method:: controller.open_new(url)

   Open *url* in a new window of the browser handled by this controller, if
   possible, otherwise, open *url* in the only browser window.  Alias
   :func:`open_new`.


   *使用此控制器处理显示*url *然后显示在新的浏览窗口,否则,
   在当前在浏览器中打开* URL *window.别名是 "open_new ()``.

---------------------------------------------------------------------------


.. method:: controller.open_new_tab(url)

   Open *url* in a new page ("tab") of the browser handled by this controller, if
   possible, otherwise equivalent to :func:`open_new`.


   *使用此控制器处理显示*url *然后显示在新的浏览窗口,否则就相当于`` open_new ()``.

---------------------------------------------------------------------------


.. rubric:: Footnotes


.. [1] Executables named here without a full path will be searched in the
       directories given in the :envvar:`PATH` environment variable.


   [1]在这里,如果没有指定一个完整的可执行路径,将根据环境变量``PATH``进行搜索. 





