
:mod:`__main__` --- 最高级脚本环境
================================================

.. module:: __main__
   :synopsis: The environment where the top-level script is run.


This module represents the (otherwise anonymous) scope in which the
interpreter's main program executes --- commands read either from standard
input, from a script file, or from an interactive prompt.  It is this
environment in which the idiomatic "conditional script" stanza causes a script
to run::

这个模块决定了（否则会任意执行）解释器执行的主程序的范围---
无论是从读取标准输入，或是脚本文件，或是交互式命令行。
这样，真正符合“条件的脚本”才会被运行::

   if __name__ == "__main__":
       main()

