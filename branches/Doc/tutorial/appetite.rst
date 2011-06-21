.. _tut-intro:

***********************************
Whetting Your Appetite 激起你的食欲
***********************************

If you do much work on computers, eventually you find that there's some task
you'd like to automate.  For example, you may wish to perform a
search-and-replace over a large number of text files, or rename and rearrange a
bunch of photo files in a complicated way. Perhaps you'd like to write a small
custom database, or a specialized GUI application, or a simple game.

如果你在电脑上做很多工作, 最终你会发现有一些任务你想把它自动化.  例如, 你可能想
要在大量文本文件中执行搜索和替换操作, 或者用一种超复杂的方法重命名和重新排列一串
照片文件.  也许你想要写一个小的定制的数据库, 或者一个专门的 GUI 应用, 或者一个简
单的游戏.

If you're a professional software developer, you may have to work with several
C/C++/Java libraries but find the usual write/compile/test/re-compile cycle is
too slow.  Perhaps you're writing a test suite for such a library and find
writing the testing code a tedious task.  Or maybe you've written a program that
could use an extension language, and you don't want to design and implement a
whole new language for your application.

如果你是个职业软件开发人员, 你可能不得不用几个 C/C++/Java 库来工作, 但发现通常的
写/编译/测试/重编译循环太慢了.  或许你正在为一个库写测试套件, 而发现写测试代码是
个乏味的任务.  或者你可能协力一个可以使用扩展语言的程序, 而你不想为你的应用设计和
实现一整种新语言.

Python is just the language for you.

Python 就是你要的语言.

You could write a Unix shell script or Windows batch files for some of these
tasks, but shell scripts are best at moving around files and changing text data,
not well-suited for GUI applications or games. You could write a C/C++/Java
program, but it can take a lot of development time to get even a first-draft
program.  Python is simpler to use, available on Windows, Mac OS X, and Unix
operating systems, and will help you get the job done more quickly.

你可以写一个 Uinx shell 脚本或者 Windows 批处理文件来完成其中一些任务, 但是 shell 
脚本最擅长到处移动文件和改变文本数据, 并不适合 GUI 应用或游戏. 你可以写个 C/C++/Java 
程序, 但甚至得到第一个草案程序都需要花费很多开发时间.  Python 使用起来更简单, 支
持 Windows, Mac OS X, 和 Unix 操作系统, 并能帮你更快速的完成工作.

Python is simple to use, but it is a real programming language, offering much
more structure and support for large programs than shell scripts or batch files
can offer.  On the other hand, Python also offers much more error checking than
C, and, being a *very-high-level language*, it has high-level data types built
in, such as flexible arrays and dictionaries.  Because of its more general data
types Python is applicable to a much larger problem domain than Awk or even
Perl, yet many things are at least as easy in Python as in those languages.

Python 使用简单, 但是一种真正的编程语言, 相比 shell 脚本或批处理文件, 它多了许多
数据结构和对大程序的支持. 另一方面, Python 比 C 提供了更多的错误检查, 且, 作为一
中*非常高级语言*, 它内建了高级数据类型, 例如灵活的数组和字典. 因为它更一般的数据
类型, 

Python allows you to split your program into modules that can be reused in other
Python programs.  It comes with a large collection of standard modules that you
can use as the basis of your programs --- or as examples to start learning to
program in Python.  Some of these modules provide things like file I/O, system
calls, sockets, and even interfaces to graphical user interface toolkits like
Tk.

Python is an interpreted language, which can save you considerable time during
program development because no compilation and linking is necessary.  The
interpreter can be used interactively, which makes it easy to experiment with
features of the language, to write throw-away programs, or to test functions
during bottom-up program development. It is also a handy desk calculator.

Python enables programs to be written compactly and readably.  Programs written
in Python are typically much shorter than equivalent C,  C++, or Java programs,
for several reasons:

* the high-level data types allow you to express complex operations in a single
  statement;

* statement grouping is done by indentation instead of beginning and ending
  brackets;

* no variable or argument declarations are necessary.

Python is *extensible*: if you know how to program in C it is easy to add a new
built-in function or module to the interpreter, either to perform critical
operations at maximum speed, or to link Python programs to libraries that may
only be available in binary form (such as a vendor-specific graphics library).
Once you are really hooked, you can link the Python interpreter into an
application written in C and use it as an extension or command language for that
application.

By the way, the language is named after the BBC show "Monty Python's Flying
Circus" and has nothing to do with reptiles.  Making references to Monty
Python skits in documentation is not only allowed, it is encouraged!

Now that you are all excited about Python, you'll want to examine it in some
more detail.  Since the best way to learn a language is to use it, the tutorial
invites you to play with the Python interpreter as you read.

In the next chapter, the mechanics of using the interpreter are explained.  This
is rather mundane information, but essential for trying out the examples shown
later.

The rest of the tutorial introduces various features of the Python language and
system through examples, beginning with simple expressions, statements and data
types, through functions and modules, and finally touching upon advanced
concepts like exceptions and user-defined classes.


