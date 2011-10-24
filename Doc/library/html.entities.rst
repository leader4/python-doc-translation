:mod:`html.entities` --- 定义HTML实体内容 
=============================================================

.. module:: html.entities
   :synopsis: Definitions of HTML general entities.
.. sectionauthor:: Fred L. Drake, Jr. <fdrake@acm.org>

**Source code:** :source:`Lib/html/entities.py`

**源代码：** LIB / HTML / entities.py

--------------

This module defines three dictionaries, ``name2codepoint``, ``codepoint2name``,
and ``entitydefs``. ``entitydefs`` is used to provide the :attr:`entitydefs`
member of the :class:`html.parser.HTMLParser` class.  The definition provided
here contains all the entities defined by XHTML 1.0 that can be handled using
simple textual substitution in the Latin-1 character set (ISO-8859-1).

这个模块定义了三个字典，“name2codepoint”
“codepoint2name```` entitydefs”。 “entitydefs”是用来
提供“entitydefs```` html.parser.HTMLParser成员”
类。这里提供的定义，包含所有的实体定义
XHTML 1.0的使用可以处理简单的文本替换
的Latin - 1字符集（ISO- 8859 - 1）。


.. data:: entitydefs

   A dictionary mapping XHTML 1.0 entity definitions to their replacement text in
   ISO Latin-1.

   字典映射的XHTML 1.0实体定义其
   替换文本中的ISO Latin -1。


.. data:: name2codepoint

   A dictionary that maps HTML entity names to the Unicode codepoints.

 一个字典， 映射HTML实体名称的Unicode代码点。


.. data:: codepoint2name

   A dictionary that maps Unicode codepoints to HTML entity names.

   Unicode代码点映射到HTML实体名称的字典。





