.. _archiving:

******************************
数据压缩和存档
******************************

本模块描述了支持数据压缩的zlib,gzip,bzip2和如何创建ZIP，tar 格式文档。


.. toctree::

   zlib.rst
   gzip.rst
   bz2.rst
   zipfile.rst
   tarfile.rst


* ``zlib`` --- 兼容**gzip**压缩
* ``gzip`` --- 支持**gzip**文件
  * 使用示例
* ``bz2`` --- 兼容**bzip2**压缩
  * 解压缩/压缩文件
  * 连续解压缩/压缩
  * 只解压缩/压缩一次
* ``zipfile`` --- 处理ZIP归档
  * ZipFile 对象
  * PyZipFile 对象
  * ZipInfo 对象
* ``tarfile`` --- 读写tar归档文件
  * TarFile 对象
  * TarInfo 对象
  * 示例
  * tar格式
  * Unicode问题
