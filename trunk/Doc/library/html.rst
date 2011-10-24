:mod:`html` --- 超文本标记语言的支持
=================================================

.. module:: html
   :synopsis: Helpers for manipulating HTML.

.. versionadded:: 3.2

**Source code:** :source:`Lib/html/__init__.py`

**源代码：** LIB / HTML /__init__.py
--------------

This module defines utilities to manipulate HTML.

这个模块定义了操作HTML的公用功能。


.. function:: escape(s, quote=True)

html.escape（S，引用= TRUE）


   Convert the characters ``&``, ``<`` and ``>`` in string *s* to HTML-safe
   sequences.  Use this if you need to display text that might contain such
   characters in HTML.  If the optional flag *quote* is true, the characters
   (``"``) and (``'``) are also translated; this helps for inclusion in an HTML
   attribute value delimited by quotes, as in ``<a href="...">``.

   转换的字符``&``,``<``和``>``字符串**
   HTML安全序列。如果您需要显示的文字，使用此
   可能包含在HTML字符。如果可选的标志
   *资料是真实的，字符(``"``)和(``'``)也
   翻译，这有助于在一个HTML属性值列入
   用引号分隔，如在“<a href="...">”。
