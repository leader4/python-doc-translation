:mod:`pipes` ---  shell管道接口 
=============================================

.. module:: pipes
   :platform: Unix
   :synopsis: A Python interface to Unix shell pipelines.
.. sectionauthor:: Moshe Zadka <moshez@zadka.site.co.il>

**Source code:** :source:`Lib/pipes.py`



--------------

The :mod:`pipes` module defines a class to abstract the concept of a *pipeline*
--- a sequence of converters from one file to  another.

``pipes``模块定义了一个抽象了管道的概念类.
*pipeline* --- 从一个文件到另外一个文件的序列的转化器

Because the module uses :program:`/bin/sh` command lines, a POSIX or compatible
shell for :func:`os.system` and :func:`os.popen` is required.

因为模块使用**/bin/sh**命令行, 必须要一个POSIX或者兼容``os.system()``和
``os.popen()``的shell. 

The :mod:`pipes` module defines the following class:

``pipes``模块定义了下面的类:


.. class:: Template()

   An abstraction of a pipeline.

   一个管道的抽象

Example::

   >>> import pipes
   >>> t=pipes.Template()
   >>> t.append('tr a-z A-Z', '--')
   >>> f=t.open('/tmp/1', 'w')
   >>> f.write('hello world')
   >>> f.close()
   >>> open('/tmp/1').read()
   'HELLO WORLD'


.. _template-objects:

Template Objects

Template 对象
----------------

Template objects following methods:

Template 对象有下面的方法:


.. method:: Template.reset()

   Restore a pipeline template to its initial state.

   恢复管道模板到初始状态


.. method:: Template.clone()

   Return a new, equivalent, pipeline template.

   恢复管道模板到初始状态


.. method:: Template.debug(flag)

   If *flag* is true, turn debugging on. Otherwise, turn debugging off. When
   debugging is on, commands to be executed are printed, and the shell is given
   ``set -x`` command to be more verbose.

    如果*flag*是true, 打开调试. . 其他情况, 调试关闭. 当调试打开, 执行的命令被打印, 
   shell给了一个``set -x``命令, 用于提供更多信息. 


.. method:: Template.append(cmd, kind)

   Append a new action at the end. The *cmd* variable must be a valid bourne shell
   command. The *kind* variable consists of two letters.

   在最后追加一个新的动作. 
   *cmd*变量必须是一个有效的bourne shell命令. 
   *kind*变量是2个字母的常量.



   The first letter can be either of ``'-'`` (which means the command reads its
   standard input), ``'f'`` (which means the commands reads a given file on the
   command line) or ``'.'`` (which means the commands reads no input, and hence
   must be first.)

   第一个字幕可以是``'-'`` (意味着从标准输入读取命令), ``'f'`` (意味着从命令行提供
   一个文件读取命令) 或``'.'`` (意味着不从命令不读读任何输入, 因而必须是第一个参数)



   Similarly, the second letter can be either of ``'-'`` (which means  the command
   writes to standard output), ``'f'`` (which means the  command writes a file on
   the command line) or ``'.'`` (which means the command does not write anything,
   and hence must be last.)

   类似的, 第2个字母可以是``'-'``(意味着命令写入标准输出), ``'f'`` (意味着命令写入
   一个命令行提供的文件) 或者``'.'`` (意味者命令不写入任何东西, 因而必须是最后一个参数.)


.. method:: Template.prepend(cmd, kind)

   Add a new action at the beginning. See :meth:`append` for explanations of the
   arguments.

   在开始添加一个新的动作. 参见``append()``中参数的解释. 


.. method:: Template.open(file, mode)

   Return a file-like object, open to *file*, but read from or written to by the
   pipeline.  Note that only one of ``'r'``, ``'w'`` may be given.

   返回一个像文件一个对象, 打开*file*, 但是从管道读或写. 注意: mode只能是``'r'``和
   ``'w'``中的一个.


.. method:: Template.copy(infile, outfile)

   Copy *infile* to *outfile* through the pipe.

    通过管道复制*infile*到*outfile*.







