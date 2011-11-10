:mod:`tty` ---  终端控制功能 
=========================================

.. module:: tty
   :platform: Unix
   :synopsis: Utility functions that perform common terminal control operations.
   
.. moduleauthor:: Steen Lumholt
.. sectionauthor:: Moshe Zadka <moshez@zadka.site.co.il>


The :mod:`tty` module defines functions for putting the tty into cbreak and raw
modes.

 "TTY" 模块定义功能投入的tty cbreak 和RAW模式. 

Because it requires the :mod:`termios` module, it will work only on Unix.

因为它要求 "termios的" 模块, 它将仅在UNIX上工作. 


The :mod:`tty` module defines the following functions:

 "TTY" 模块定义了以下功能: 


.. function:: setraw(fd, when=termios.TCSAFLUSH)

   Change the mode of the file descriptor *fd* to raw. If *when* is omitted, it
   defaults to :const:`termios.TCSAFLUSH`, and is passed to
   :func:`termios.tcsetattr`.

   文件描述符模式* FD* row . 如果*when*
   省略, 则默认为 "termios.TCSAFLUSH``, 并传递给
   `` termios.tcsetattr()``.


.. function:: setcbreak(fd, when=termios.TCSAFLUSH)

   Change the mode of file descriptor *fd* to cbreak. If *when* is omitted, it
   defaults to :const:`termios.TCSAFLUSH`, and is passed to
   :func:`termios.tcsetattr`.

   更改模式的文件的描述符* FD* cbreak. 如果*when*
   省略, 则默认为 "termios.TCSAFLUSH``, 并传递给
   `` termios.tcsetattr()``.



.. seealso::

另参见: 

   Module :mod:`termios`
      Low-level terminal control interface.

       模块 "termios的``
      低级别的终端控制接口. 


