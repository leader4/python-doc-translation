:tocdepth: 2

.. _windows-faq:

=====================
Python on Windows FAQ
=====================

.. contents::

.. XXX need review for Python 3.
   XXX need review for Windows Vista/Seven?


How do I run a Python program under Windows? 如何在 Windows 下运行 Python 程序
----------------------------------------------------------------------------------------

This is not necessarily a straightforward question. If you are already familiar
with running programs from the Windows command line then everything will seem
obvious; otherwise, you might need a little more guidance.  There are also
differences between Windows 95, 98, NT, ME, 2000 and XP which can add to the
confusion.

这并不是一个易懂的问题. 如果你对于命令行有一定的了解, 那么看起来会比较简单;
否则, 你可能需要一点点的帮助. 而在 Windows 的不同系统下, 也会有不同,
这就增加了更多的困扰.

.. sidebar:: |Python Development on XP|_
   :subtitle: `Python Development on XP`_

   This series of screencasts aims to get you up and running with Python on
   Windows XP.  The knowledge is distilled into 1.5 hours and will get you up
   and running with the right Python distribution, coding in your choice of IDE,
   and debugging and writing solid code with unit-tests.

   这一系列的截图是为了帮助你在 Windows XP 下运行 Python.
   此处会使用 1.5 个小时来帮助你获得并运行正确的发行版,
   在你选的 IDE 中进行编码, 调试和进行单元测试.

.. |Python Development on XP| image:: python-video-icon.png
.. _`Python Development on XP`:
   http://www.showmedo.com/videos/series?name=pythonOzsvaldPyNewbieSeries

Unless you use some sort of integrated development environment, you will end up
*typing* Windows commands into what is variously referred to as a "DOS window"
or "Command prompt window".  Usually you can create such a window from your
Start menu; under Windows 2000 the menu selection is :menuselection:`Start -->
Programs --> Accessories --> Command Prompt`.  You should be able to recognize
when you have started such a window because you will see a Windows "command
prompt", which usually looks like this:

除非你使用 IDE (Integrated Development Environment), 你一般需要键入 Windows
的命令到所谓的 "DOS 窗口" 或 "命令提示窗口". 通常, 你可以从你的开始菜单中创建;
在 Windows 2000 下菜单的选择过程是 :menuselection:`开始 --> 程序 -->
附件 --> 命令提示符`. 你应该能够知道, 因为你将会看到一个 "命令提示符",
通常看起来是这样的:

::

   C:\>

The letter may be different, and there might be other things after it, so you
might just as easily see something like:

字母可能看起来并不相同, 并且会有其他的东西跟着它,
你很容易看到这样的东西:

::

   D:\Steve\Projects\Python>

depending on how your computer has been set up and what else you have recently
done with it.  Once you have started such a window, you are well on the way to
running Python programs.

这依赖于你的计算机是如何设置的, 还有就是你对他做了什么.
如果你启动了这样的窗口, 那么你就会了解运行 Python 程序的方法.

You need to realize that your Python scripts have to be processed by another
program called the Python interpreter.  The interpreter reads your script,
compiles it into bytecodes, and then executes the bytecodes to run your
program. So, how do you arrange for the interpreter to handle your Python?

你需要知道, 你的 Python 脚本是被另一个称为 Python 解释器的程序运行.
这个解释器会读取你的脚本, 把它编译为字节码, 然后再执行这个字节码.
所以, 你应该如何让这个解释器来处理你的 Python 呢?

First, you need to make sure that your command window recognises the word
"python" as an instruction to start the interpreter.  If you have opened a
command window, you should try entering the command ``python`` and hitting
return.  You should then see something like:

首先, 你需要确保你的命令提示符能够认识 "python" 并且能启动解释器.
如果你已经打开了一个命令提示符窗口, 你应该尝试输入 "python"
并且敲回车. 然后就会看到这样的东西:

::

   Python 2.2 (#28, Dec 21 2001, 12:21:22) [MSC 32 bit (Intel)] on win32
   Type "help", "copyright", "credits" or "license" for more information.
   >>>

You have started the interpreter in "interactive mode". That means you can enter
Python statements or expressions interactively and have them executed or
evaluated while you wait.  This is one of Python's strongest features.  Check it
by entering a few expressions of your choice and seeing the results:

你以 "交互模式" 启动了解释器. 这意味着, 你可以交互式地输入 Python 的语句或者表达式,
然后执行它们. 这就是 Python 其中的一个强项. 你可以通过输入几个表达式, 
然后看看结果来检验这点强项:

::

    >>> print("Hello")
    Hello
    >>> "Hello" * 3
    HelloHelloHello

Many people use the interactive mode as a convenient yet highly programmable
calculator.  When you want to end your interactive Python session, hold the Ctrl
key down while you enter a Z, then hit the "Enter" key to get back to your
Windows command prompt.

很多人使用交互模式作为简单但是强大的可编程计算器.
当你要结束此次交互会话时, 你可以按 Ctrl-Z 后, 输入回车回到命令提示符窗口.

You may also find that you have a Start-menu entry such as :menuselection:`Start
--> Programs --> Python 2.2 --> Python (command line)` that results in you
seeing the ``>>>`` prompt in a new window.  If so, the window will disappear
after you enter the Ctrl-Z character; Windows is running a single "python"
command in the window, and closes it when you terminate the interpreter.

你也可能发现在你的开始菜单项中就有了这一项, 比如 :menuselection:`开始 -->
程序 --> Python 2.2 --> Python (command line)` 就可以是你直接打开一个窗口.
如果这样, 你按了 Ctrl-Z 后窗口就会消失; Windows 就运行了一个 "python",
当关掉解释器后就会自动关闭窗口.

If the ``python`` command, instead of displaying the interpreter prompt ``>>>``,
gives you a message like:

如果出现的不是提示符 ``>>>``, 而是:

::

   'python' is not recognized as an internal or external command,
   operable program or batch file.

.. sidebar:: |Adding Python to DOS Path|_
   :subtitle: `Adding Python to DOS Path`_

   Python is not added to the DOS path by default.  This screencast will walk
   you through the steps to add the correct entry to the `System Path`, allowing
   Python to be executed from the command-line by all users.

   Python 默认下并不会添加到 DOS 的搜索路径中.
   此处的截图会帮你了解如何添加合适的项到 `系统路径`,
   运行所有的用户都能直接运行 Python.

.. |Adding Python to DOS Path| image:: python-video-icon.png
.. _`Adding Python to DOS Path`:
   http://showmedo.com/videos/video?name=960000&fromSeriesID=96


or 或者::

   Bad command or filename

then you need to make sure that your computer knows where to find the Python
interpreter.  To do this you will have to modify a setting called PATH, which is
a list of directories where Windows will look for programs.

那么你就需要确保你的计算机知道从何处查找 Python 的解释器.
你需要修改一个称为 PATH 的设置, 这里面存放了 Windows 查找程序时的路径.

You should arrange for Python's installation directory to be added to the PATH
of every command window as it starts.  If you installed Python fairly recently
then the command:

你需要将 Python 的安装路径添加到 PATH 变量中.
如果你正确的安装了, 那么命令:

::

   dir C:\py*

will probably tell you where it is installed; the usual location is something
like ``C:\Python23``.  Otherwise you will be reduced to a search of your whole
disk ... use :menuselection:`Tools --> Find` or hit the :guilabel:`Search`
button and look for "python.exe".  Supposing you discover that Python is
installed in the ``C:\Python23`` directory (the default at the time of writing),
you should make sure that entering the command :

将会告诉你它装在什么地方; 一般的路径是类似于 ``C:\Python23`` 这样的东西.
否则你就需要搜索这个硬盘. 使用 :menuselection:`工具 --> 查找` 或点击
:guilabel:`Search` 按钮进行查找 "python.exe". 加入你发现 Python
被安装于 ``C:\Python23`` 目录 (那个时候的默认值),
你需要确保输入命令:

::

   c:\Python23\python

starts up the interpreter as above (and don't forget you'll need a "CTRL-Z" and
an "Enter" to get out of it). Once you have verified the directory, you need to
add it to the start-up routines your computer goes through.  For older versions
of Windows the easiest way to do this is to edit the ``C:\AUTOEXEC.BAT``
file. You would want to add a line like the following to ``AUTOEXEC.BAT``:

像前面那样正确的启动了解释器 (不要忘记需要用 "CTRL-Z" 再加个回车进行退出).
一旦你找到了这样的目录, 你就需要添加到启动项中. 对于早期版本的 Windows,
你要编辑 ``C:\AUTOEXEC.BAT`` 文件. 你需要在这个文件中添加类似下面的一行:

::

   PATH C:\Python23;%PATH%

For Windows NT, 2000 and (I assume) XP, you will need to add a string such as :

对于 Windows NT, 2000 和 XP 之类的, 你需要增加下面的字符串:

::

   ;C:\Python23

to the current setting for the PATH environment variable, which you will find in
the properties window of "My Computer" under the "Advanced" tab.  Note that if
you have sufficient privilege you might get a choice of installing the settings
either for the Current User or for System.  The latter is preferred if you want
everybody to be able to run Python on the machine.

到 PATH 环境变量中 (右击 "我的电脑" 点属性, 然后 "高级"). 
注意如需要有一定的权限才能够进行设置. 这里有当前用户和系统的环境变量.
系统的环境变量会作用于该机器上的每个用户.

If you aren't confident doing any of these manipulations yourself, ask for help!
At this stage you may want to reboot your system to make absolutely sure the new
setting has taken effect.  You probably won't need to reboot for Windows NT, XP
or 2000.  You can also avoid it in earlier versions by editing the file
``C:\WINDOWS\COMMAND\CMDINIT.BAT`` instead of ``AUTOEXEC.BAT``.

如果你对此并不是很有信心, 那么就寻求帮助!
然后你还可能需要重启计算机以确保设置都有效了.
在 Windows NT, 2000 或 XP 等更高的版本中可能并不需要炒年糕其.
你也可以通过编辑 ``C:\WINDOWS\COMMAND\CMDINIT.BAT`` 来避免重启.

You should now be able to start a new command window, enter ``python`` at the
``C:\>`` (or whatever) prompt, and see the ``>>>`` prompt that indicates the
Python interpreter is reading interactive commands.

你现在应该可以启动一个新的命令提示符, 然后输入 ``python`` 并可以看到 ``>>>``
这样的提示符, 这就说明 Python 解释器已经进入交换模式了.

Let's suppose you have a program called ``pytest.py`` in directory
``C:\Steve\Projects\Python``.  A session to run that program might look like
this:

我们假设有一个称为 ``pytest.py`` 的程序在目录 ``C:\Steve\Projects\Python`` 中.
这段会话看起来像这样:

::

   C:\> cd \Steve\Projects\Python
   C:\Steve\Projects\Python> python pytest.py

Because you added a file name to the command to start the interpreter, when it
starts up it reads the Python script in the named file, compiles it, executes
it, and terminates, so you see another ``C:\>`` prompt.  You might also have
entered :

因为在你启动解释器时额外添加了一个文件名, 解释器就会读取这个脚本,
编译它, 然后执行, 最后终止并返回至 ``C:\>`` 提示符.
你也可以输入:

::

   C:\> python \Steve\Projects\Python\pytest.py

if you hadn't wanted to change your current directory.

如果你不想更改你的当前目录.

Under NT, 2000 and XP you may well find that the installation process has also
arranged that the command ``pytest.py`` (or, if the file isn't in the current
directory, ``C:\Steve\Projects\Python\pytest.py``) will automatically recognize
the ".py" extension and run the Python interpreter on the named file. Using this
feature is fine, but *some* versions of Windows have bugs which mean that this
form isn't exactly equivalent to using the interpreter explicitly, so be
careful.

在 NT, 2000 和 XP 下面, 你会发现可以直接运行 ``pytest.py`` (或者,
如果文件不在当前目录, ``C:\Steve\Projects\Python\pytest.py``),
它会自动认出 ".py" 后缀并且使用 Python 解释器去运行.
使用这个特性很不错, 但是有些 Windows 版本是有些 bug 的, 
所以可能并不等于显示的使用解释器, 所以小心.

The important things to remember are:

有些事情最好记住:

1. Start Python from the Start Menu, or make sure the PATH is set correctly so
   Windows can find the Python interpreter. 

   从开始菜单中启动 Python, 或者保证 PATH 已经正确设置.
   
   ::

      python

   should give you a '>>>' prompt from the Python interpreter. Don't forget the
   CTRL-Z and ENTER to terminate the interpreter (and, if you started the window
   from the Start Menu, make the window disappear).

   从 Python 解释器应该会给出 '>>>' 提示符. 不要忘了用 CTRL-Z 和回车来退出 (
   并且, 如果你从开始菜单启动, 窗口将会消失).

2. Once this works, you run programs with commands:

   如果可以, 可以用下面的命令运行程序:
   
   ::

      python {program-file}

3. When you know the commands to use you can build Windows shortcuts to run the
   Python interpreter on any of your scripts, naming particular working
   directories, and adding them to your menus.  Take a look at :

   当你知道了要使用的命令, 那么你可以建立一个快捷方式,
   指明工作的目录并且增加到你的菜单中. 看看:
   
   ::

      python --help

   if your needs are complex.

   如果你的需求很复杂.

4. Interactive mode (where you see the ``>>>`` prompt) is best used for checking
   that individual statements and expressions do what you think they will, and
   for developing code by experiment.

   交互模式 (就是你可以看到 ``>>>``` 提示符) 最适合于,
   检查单独的语句和表达式, 和进行一些测试.


How do I make Python scripts executable? 如何让 Python 脚本可执行?
-----------------------------------------------------------------------

On Windows 2000, the standard Python installer already associates the .py
extension with a file type (Python.File) and gives that file type an open
command that runs the interpreter (``D:\Program Files\Python\python.exe "%1"
%*``).  This is enough to make scripts executable from the command prompt as
'foo.py'.  If you'd rather be able to execute the script by simple typing 'foo'
with no extension you need to add .py to the PATHEXT environment variable.

在 Windows 2000 下, 标准的 Python 安装包已经将 .py 后缀的文件和 Python.File
类型绑定, 在执行时会运行解释器 (``D:\Program Files\Python\python.exe "%1" %*``).
这样就可以在命令行下直接运行 'foo.py' 了. 如果你想用 'foo' 来运行这个脚本,
那么你只需要将 .py 添加到 PATHEXT 环境变量中就可以了.

On Windows NT, the steps taken by the installer as described above allow you to
run a script with 'foo.py', but a longtime bug in the NT command processor
prevents you from redirecting the input or output of any script executed in this
way.  This is often important.

在 Windows NT 下, 步骤和上面所说的一样, 但是此处有一个长期以来的 bug ,
就是当你进行重定向输入或输出时, NT 的命令解释器会对你进行阻止.
这经常很重要.

The incantation for making a Python script executable under WinNT is to give the
file an extension of .cmd and add the following as the first line:

一个在 WinNT 下让 Python 脚本可执行的方法, 就是给这个文件一个 .cmd 后缀,
并且增加下面这一行.

::

   @setlocal enableextensions & python -x %~f0 %* & goto :EOF


Why does Python sometimes take so long to start?
------------------------------------------------

Usually Python starts very quickly on Windows, but occasionally there are bug
reports that Python suddenly begins to take a long time to start up.  This is
made even more puzzling because Python will work fine on other Windows systems
which appear to be configured identically.

The problem may be caused by a misconfiguration of virus checking software on
the problem machine.  Some virus scanners have been known to introduce startup
overhead of two orders of magnitude when the scanner is configured to monitor
all reads from the filesystem.  Try checking the configuration of virus scanning
software on your systems to ensure that they are indeed configured identically.
McAfee, when configured to scan all file system read activity, is a particular
offender.


Where is Freeze for Windows?
----------------------------

"Freeze" is a program that allows you to ship a Python program as a single
stand-alone executable file.  It is *not* a compiler; your programs don't run
any faster, but they are more easily distributable, at least to platforms with
the same OS and CPU.  Read the README file of the freeze program for more
disclaimers.

You can use freeze on Windows, but you must download the source tree (see
http://www.python.org/download/source).  The freeze program is in the
``Tools\freeze`` subdirectory of the source tree.

You need the Microsoft VC++ compiler, and you probably need to build Python.
The required project files are in the PCbuild directory.


Is a ``*.pyd`` file the same as a DLL?
--------------------------------------

.. XXX update for py3k (PyInit_foo)

Yes, .pyd files are dll's, but there are a few differences.  If you have a DLL
named ``foo.pyd``, then it must have a function ``initfoo()``.  You can then
write Python "import foo", and Python will search for foo.pyd (as well as
foo.py, foo.pyc) and if it finds it, will attempt to call ``initfoo()`` to
initialize it.  You do not link your .exe with foo.lib, as that would cause
Windows to require the DLL to be present.

Note that the search path for foo.pyd is PYTHONPATH, not the same as the path
that Windows uses to search for foo.dll.  Also, foo.pyd need not be present to
run your program, whereas if you linked your program with a dll, the dll is
required.  Of course, foo.pyd is required if you want to say ``import foo``.  In
a DLL, linkage is declared in the source code with ``__declspec(dllexport)``.
In a .pyd, linkage is defined in a list of available functions.


How can I embed Python into a Windows application?
--------------------------------------------------

Embedding the Python interpreter in a Windows app can be summarized as follows:

1. Do _not_ build Python into your .exe file directly.  On Windows, Python must
   be a DLL to handle importing modules that are themselves DLL's.  (This is the
   first key undocumented fact.)  Instead, link to :file:`python{NN}.dll`; it is
   typically installed in ``C:\Windows\System``.  *NN* is the Python version, a
   number such as "23" for Python 2.3.

   You can link to Python in two different ways.  Load-time linking means
   linking against :file:`python{NN}.lib`, while run-time linking means linking
   against :file:`python{NN}.dll`.  (General note: :file:`python{NN}.lib` is the
   so-called "import lib" corresponding to :file:`python{NN}.dll`.  It merely
   defines symbols for the linker.)

   Run-time linking greatly simplifies link options; everything happens at run
   time.  Your code must load :file:`python{NN}.dll` using the Windows
   ``LoadLibraryEx()`` routine.  The code must also use access routines and data
   in :file:`python{NN}.dll` (that is, Python's C API's) using pointers obtained
   by the Windows ``GetProcAddress()`` routine.  Macros can make using these
   pointers transparent to any C code that calls routines in Python's C API.

   Borland note: convert :file:`python{NN}.lib` to OMF format using Coff2Omf.exe
   first.

   .. XXX what about static linking?

2. If you use SWIG, it is easy to create a Python "extension module" that will
   make the app's data and methods available to Python.  SWIG will handle just
   about all the grungy details for you.  The result is C code that you link
   *into* your .exe file (!)  You do _not_ have to create a DLL file, and this
   also simplifies linking.

3. SWIG will create an init function (a C function) whose name depends on the
   name of the extension module.  For example, if the name of the module is leo,
   the init function will be called initleo().  If you use SWIG shadow classes,
   as you should, the init function will be called initleoc().  This initializes
   a mostly hidden helper class used by the shadow class.

   The reason you can link the C code in step 2 into your .exe file is that
   calling the initialization function is equivalent to importing the module
   into Python! (This is the second key undocumented fact.)

4. In short, you can use the following code to initialize the Python interpreter
   with your extension module.

   .. code-block:: c

      #include "python.h"
      ...
      Py_Initialize();  // Initialize Python.
      initmyAppc();  // Initialize (import) the helper class.
      PyRun_SimpleString("import myApp") ;  // Import the shadow class.

5. There are two problems with Python's C API which will become apparent if you
   use a compiler other than MSVC, the compiler used to build pythonNN.dll.

   Problem 1: The so-called "Very High Level" functions that take FILE *
   arguments will not work in a multi-compiler environment because each
   compiler's notion of a struct FILE will be different.  From an implementation
   standpoint these are very _low_ level functions.

   Problem 2: SWIG generates the following code when generating wrappers to void
   functions:

   .. code-block:: c

      Py_INCREF(Py_None);
      _resultobj = Py_None;
      return _resultobj;

   Alas, Py_None is a macro that expands to a reference to a complex data
   structure called _Py_NoneStruct inside pythonNN.dll.  Again, this code will
   fail in a mult-compiler environment.  Replace such code by:

   .. code-block:: c

      return Py_BuildValue("");

   It may be possible to use SWIG's ``%typemap`` command to make the change
   automatically, though I have not been able to get this to work (I'm a
   complete SWIG newbie).

6. Using a Python shell script to put up a Python interpreter window from inside
   your Windows app is not a good idea; the resulting window will be independent
   of your app's windowing system.  Rather, you (or the wxPythonWindow class)
   should create a "native" interpreter window.  It is easy to connect that
   window to the Python interpreter.  You can redirect Python's i/o to _any_
   object that supports read and write, so all you need is a Python object
   (defined in your extension module) that contains read() and write() methods.


How do I use Python for CGI?
----------------------------

On the Microsoft IIS server or on the Win95 MS Personal Web Server you set up
Python in the same way that you would set up any other scripting engine.

Run regedt32 and go to::

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC\Parameters\ScriptMap

and enter the following line (making any specific changes that your system may
need)::

    .py :REG_SZ: c:\<path to python>\python.exe -u %s %s

This line will allow you to call your script with a simple reference like:
``http://yourserver/scripts/yourscript.py`` provided "scripts" is an
"executable" directory for your server (which it usually is by default).  The
:option:`-u` flag specifies unbuffered and binary mode for stdin - needed when
working with binary data.

In addition, it is recommended that using ".py" may not be a good idea for the
file extensions when used in this context (you might want to reserve ``*.py``
for support modules and use ``*.cgi`` or ``*.cgp`` for "main program" scripts).

In order to set up Internet Information Services 5 to use Python for CGI
processing, please see the following links:

   http://www.e-coli.net/pyiis_server.html (for Win2k Server)
   http://www.e-coli.net/pyiis.html (for Win2k pro)

Configuring Apache is much simpler.  In the Apache configuration file
``httpd.conf``, add the following line at the end of the file::

    ScriptInterpreterSource Registry

Then, give your Python CGI-scripts the extension .py and put them in the cgi-bin
directory.


How do I keep editors from inserting tabs into my Python source?
----------------------------------------------------------------

The FAQ does not recommend using tabs, and the Python style guide, :pep:`8`,
recommends 4 spaces for distributed Python code; this is also the Emacs
python-mode default.

Under any editor, mixing tabs and spaces is a bad idea.  MSVC is no different in
this respect, and is easily configured to use spaces: Take :menuselection:`Tools
--> Options --> Tabs`, and for file type "Default" set "Tab size" and "Indent
size" to 4, and select the "Insert spaces" radio button.

If you suspect mixed tabs and spaces are causing problems in leading whitespace,
run Python with the :option:`-t` switch or run ``Tools/Scripts/tabnanny.py`` to
check a directory tree in batch mode.


How do I check for a keypress without blocking?
-----------------------------------------------

Use the msvcrt module.  This is a standard Windows-specific extension module.
It defines a function ``kbhit()`` which checks whether a keyboard hit is
present, and ``getch()`` which gets one character without echoing it.


How do I emulate os.kill() in Windows?
--------------------------------------

Prior to Python 2.7 and 3.2, to terminate a process, you can use :mod:`ctypes`::

   import ctypes

   def kill(pid):
       """kill function for Win32"""
       kernel32 = ctypes.windll.kernel32
       handle = kernel32.OpenProcess(1, 0, pid)
       return (0 != kernel32.TerminateProcess(handle, 0))

In 2.7 and 3.2, :func:`os.kill` is implemented similar to the above function,
with the additional feature of being able to send CTRL+C and CTRL+BREAK
to console subprocesses which are designed to handle those signals. See
:func:`os.kill` for further details.


Why does os.path.isdir() fail on NT shared directories?
-------------------------------------------------------

The solution appears to be always append the "\\" on the end of shared
drives.

   >>> import os
   >>> os.path.isdir( '\\\\rorschach\\public')
   0
   >>> os.path.isdir( '\\\\rorschach\\public\\')
   1

It helps to think of share points as being like drive letters.  Example::

   k: is not a directory
   k:\ is a directory
   k:\media is a directory
   k:\media\ is not a directory

The same rules apply if you substitute "k:" with "\\conky\foo"::

   \\conky\foo  is not a directory
   \\conky\foo\ is a directory
   \\conky\foo\media is a directory
   \\conky\foo\media\ is not a directory


cgi.py (or other CGI programming) doesn't work sometimes on NT or win95!
------------------------------------------------------------------------

Be sure you have the latest python.exe, that you are using python.exe rather
than a GUI version of Python and that you have configured the server to execute::

   "...\python.exe -u ..."

for the CGI execution.  The :option:`-u` (unbuffered) option on NT and Win95
prevents the interpreter from altering newlines in the standard input and
output.  Without it post/multipart requests will seem to have the wrong length
and binary (e.g. GIF) responses may get garbled (resulting in broken images, PDF
files, and other binary downloads failing).


Why doesn't os.popen() work in PythonWin on NT?
-----------------------------------------------

The reason that os.popen() doesn't work from within PythonWin is due to a bug in
Microsoft's C Runtime Library (CRT). The CRT assumes you have a Win32 console
attached to the process.

You should use the win32pipe module's popen() instead which doesn't depend on
having an attached Win32 console.

Example::

   import win32pipe
   f = win32pipe.popen('dir /c c:\\')
   print(f.readlines())
   f.close()


Why doesn't os.popen()/win32pipe.popen() work on Win9x?
-------------------------------------------------------

There is a bug in Win9x that prevents os.popen/win32pipe.popen* from
working. The good news is there is a way to work around this problem.  The
Microsoft Knowledge Base article that you need to lookup is: Q150956. You will
find links to the knowledge base at: http://support.microsoft.com/.


PyRun_SimpleFile() crashes on Windows but not on Unix; why?
-----------------------------------------------------------

This is very sensitive to the compiler vendor, version and (perhaps) even
options.  If the FILE* structure in your embedding program isn't the same as is
assumed by the Python interpreter it won't work.

The Python 1.5.* DLLs (``python15.dll``) are all compiled with MS VC++ 5.0 and
with multithreading-DLL options (``/MD``).

If you can't change compilers or flags, try using :c:func:`Py_RunSimpleString`.
A trick to get it to run an arbitrary file is to construct a call to
:func:`execfile` with the name of your file as argument.

Also note that you can not mix-and-match Debug and Release versions.  If you
wish to use the Debug Multithreaded DLL, then your module *must* have an "_d"
appended to the base name.


Importing _tkinter fails on Windows 95/98: why?
------------------------------------------------

Sometimes, the import of _tkinter fails on Windows 95 or 98, complaining with a
message like the following::

   ImportError: DLL load failed: One of the library files needed
   to run this application cannot be found.

It could be that you haven't installed Tcl/Tk, but if you did install Tcl/Tk,
and the Wish application works correctly, the problem may be that its installer
didn't manage to edit the autoexec.bat file correctly.  It tries to add a
statement that changes the PATH environment variable to include the Tcl/Tk 'bin'
subdirectory, but sometimes this edit doesn't quite work.  Opening it with
notepad usually reveals what the problem is.

(One additional hint, noted by David Szafranski: you can't use long filenames
here; e.g. use ``C:\PROGRA~1\Tcl\bin`` instead of ``C:\Program Files\Tcl\bin``.)


How do I extract the downloaded documentation on Windows?
---------------------------------------------------------

Sometimes, when you download the documentation package to a Windows machine
using a web browser, the file extension of the saved file ends up being .EXE.
This is a mistake; the extension should be .TGZ.

Simply rename the downloaded file to have the .TGZ extension, and WinZip will be
able to handle it.  (If your copy of WinZip doesn't, get a newer one from
http://www.winzip.com.)


Missing cw3215mt.dll (or missing cw3215.dll)
--------------------------------------------

Sometimes, when using Tkinter on Windows, you get an error that cw3215mt.dll or
cw3215.dll is missing.

Cause: you have an old Tcl/Tk DLL built with cygwin in your path (probably
``C:\Windows``).  You must use the Tcl/Tk DLLs from the standard Tcl/Tk
installation (Python 1.5.2 comes with one).


Warning about CTL3D32 version from installer
--------------------------------------------

The Python installer issues a warning like this::

   This version uses CTL3D32.DLL which is not the correct version.
   This version is used for windows NT applications only.

Tim Peters:

   This is a Microsoft DLL, and a notorious source of problems.  The message
   means what it says: you have the wrong version of this DLL for your operating
   system.  The Python installation did not cause this -- something else you
   installed previous to this overwrote the DLL that came with your OS (probably
   older shareware of some sort, but there's no way to tell now).  If you search
   for "CTL3D32" using any search engine (AltaVista, for example), you'll find
   hundreds and hundreds of web pages complaining about the same problem with
   all sorts of installation programs.  They'll point you to ways to get the
   correct version reinstalled on your system (since Python doesn't cause this,
   we can't fix it).

David A Burton has written a little program to fix this.  Go to
http://www.burtonsys.com/downloads.html and click on "ctl3dfix.zip".

