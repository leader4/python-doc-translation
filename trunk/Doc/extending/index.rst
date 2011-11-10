.. _extending-index:

##################################################
 扩展和嵌入Python解释器
##################################################

:Release: |version|
:Date: |today|

本文介绍了如何用C语言或者C++编写的模块来扩展Python的
解释其. 这些模块可以定义新的功能,也可以创建
新的对象类型和它们的方法. 该文件还介绍了如何嵌入Python解释器到
另一个将其作为扩展语言使用的应用程序. 
最后,它显示了如何编译和链接扩展模块,使他们可以
 (在运行时动态加载) 到解释器,如果底层
操作系统支持此功能的话. 

本文档假定你明白关于Python的基本知识. 一个非正式
语言的介绍,请参阅 :ref:`标准库` .  
它会给出了一个更正式的定义. 


.. toctree::
   :maxdepth: 2
   :numbered:

   extending.rst
   newtypes.rst
   building.rst
   windows.rst
   embedding.rst

