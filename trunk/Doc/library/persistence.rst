.. _persistence:

********************
数据持久化
********************




The modules described in this chapter support storing Python data in a
persistent form on disk.  The :mod:`pickle` and :mod:`marshal` modules can turn
many Python data types into a stream of bytes and then recreate the objects from
the bytes.  The various DBM-related modules support a family of hash-based file
formats that store a mapping of strings to other strings.

本章的这个模块,支持把Python数据持久的存储在磁盘上. ``pickle``和``marshal``
模块能把很多Python数据转成字节流,能通过这些字节重新创建对象. DBM相关的模块支持基于
哈希的文件格式的集合,这些集合存储了从一个字符串到另外一个字符串的映射. 


The list of modules described in this chapter is:

本章中这些模块的描述里列表: 


.. toctree::

   pickle.rst
   copyreg.rst
   shelve.rst
   marshal.rst
   dbm.rst
   sqlite3.rst

