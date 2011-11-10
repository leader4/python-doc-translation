.. _documenting-index:

######################################
  Documenting Python 编写python的文档
######################################


---------------------------------------------------------------------------

The Python language has a substantial body of documentation, much of it
contributed by various authors. The markup used for the Python documentation is
`reStructuredText`_, developed by the `docutils`_ project, amended by custom
directives and using a toolset named `Sphinx`_ to postprocess the HTML output.

Python包含丰富的文档, 其中的大多数都是由多位作者一起完成的.
在python文档中使用的标记语言是 `reStructuredText`_ , 
在 `docutils`_ 项目中开发, 增加了某些特定的指令, 并且
使用一个名为 `Sphinx`_ 的工具生成HTML.

---------------------------------------------------------------------------

This document describes the style guide for our documentation as well as the
custom reStructuredText markup introduced by Sphinx to support Python
documentation and how it should be used.

这篇文档主要给出编写文档的样式规范, 如何使用 reStructuredText 标记语言, 
以及一些在Sphinx中增加的指令, 以及如何使用Sphinx. 

---------------------------------------------------------------------------

.. _reStructuredText: http://docutils.sf.net/rst.html
.. _docutils: http://docutils.sf.net/
.. _Sphinx: http://sphinx.pocoo.org/

.. note::

   If you're interested in contributing to Python's documentation, there's no
   need to write reStructuredText if you're not so inclined; plain text
   contributions are more than welcome as well.  Send an e-mail to
   docs@python.org or open an issue on the :ref:`tracker <reporting-bugs>`.

   如果你对编写python的文档感兴趣, 但不是很想用 reStructuredText, 
   那么用纯文本的格式会更好些. 
   发送邮件至 docs@python.org 或者发一个主题在 :ref:`tracker <reporting-bugs>` . 

---------------------------------------------------------------------------


.. toctree::
   :numbered:
   :maxdepth: 1

   intro.rst
   style.rst
   rest.rst
   markup.rst
   fromlatex.rst
   building.rst

