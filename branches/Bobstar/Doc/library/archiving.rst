.. _archiving:

******************************
数据压缩和存档 译者:北漂
******************************

The modules described in this chapter support data compression with the zlib,
gzip, and bzip2 algorithms, and  the creation of ZIP- and tar-format archives.

本模块描述了支持数据压缩的zlib,gzip,bzip2和如何创建ZIP，tar 格式文档。


.. toctree::

   zlib.rst
   gzip.rst
   bz2.rst
   zipfile.rst
   tarfile.rst

* ``zlib`` --- Compression compatible with **gzip**
* ``zlib`` --- 兼容**gzip**压缩
* ``gzip`` --- Support for **gzip** files
* ``gzip`` --- 支持**gzip**文件
  * Examples of usage
  * 使用示例
* ``bz2`` --- Compression compatible with **bzip2**
* ``bz2`` --- 兼容**bzip2**压缩
  * (De)compression of files
  * 解压缩/压缩文件
  * Sequential (de)compression
  * 连续解压缩/压缩
  * One-shot (de)compression
  * 只解压缩/压缩一次
* ``zipfile`` --- Work with ZIP archives
* ``zipfile`` --- 处理ZIP归档
  * ZipFile Objects
  * ZipFile 对象
  * PyZipFile Objects
  * PyZipFile 对象
  * ZipInfo Objects
  * ZipInfo 对象
* ``tarfile`` --- Read and write tar archive files
* ``tarfile`` --- 读写tar归档文件
  * TarFile Objects
  * TarFile 对象
  * TarInfo Objects
  * TarInfo 对象
  * Examples
  * 示例
  * Supported tar formats
  * tar格式
  * Unicode issues
  * Unicode问题
