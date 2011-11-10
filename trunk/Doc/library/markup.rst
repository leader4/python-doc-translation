.. _markup:

**********************************
结构的标记处理工具
**********************************

Python supports a variety of modules to work with various forms of structured
data markup.  This includes modules to work with the Standard Generalized Markup
Language (SGML) and the Hypertext Markup Language (HTML), and several interfaces
for working with the Extensible Markup Language (XML).

Python支持一定数量的模块与各种形式的结构化数据标记一起工作. 这些与模块一起工作
的有标准通用标记语言(SGML)和超文本标记语言(HTML), 以及几个为可扩展标记语言(XML)
工作的接口. 



It is important to note that modules in the :mod:`xml` package require that
there be at least one SAX-compliant XML parser available. The Expat parser is
included with Python, so the :mod:`xml.parsers.expat` module will always be
available.

重要的是要注意在``xml``包中的模块需要至少一个可用的SAX兼容的XML解析器. Expat
解析器被Python包含在内, 所以``xml.parsers.expat``模块将始终可用. 



The documentation for the :mod:`xml.dom` and :mod:`xml.sax` packages are the
definition of the Python bindings for the DOM and SAX interfaces.

关于``xml.dom``和``xml.sax``包的文档定义了Python绑定的DOM和SAX接口. 


.. toctree::

   html.rst
   html.parser.rst
   html.entities.rst
   pyexpat.rst
   xml.dom.rst
   xml.dom.minidom.rst
   xml.dom.pulldom.rst
   xml.sax.rst
   xml.sax.handler.rst
   xml.sax.utils.rst
   xml.sax.reader.rst
   xml.etree.elementtree.rst

