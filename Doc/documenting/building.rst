Building the documentation 建立文档
=====================================

---------------------------------------------------------------------------

You need to have Python 2.4 or higher installed; the toolset used to build the
docs is written in Python.  It is called *Sphinx*, it is not included in this
tree, but maintained separately.  Also needed are the docutils, supplying the
base markup that Sphinx uses, Jinja, a templating engine, and optionally
Pygments, a code highlighter.

---------------------------------------------------------------------------

你需要 Python 2.4 或更高的版本; 这个用于建立文档的工具是用 Python 写的.
它称为 *Sphinx* , 它并不包含于这个结构中, 但可以独立的获取.
同样还需要 docutils, 用以基本的标记, Jinja, 一个模板引擎, 和可选的 Pygments,
一个代码高亮器.

---------------------------------------------------------------------------


Using make 使用 make
-----------------------

Luckily, a Makefile has been prepared so that on Unix, provided you have
installed Python and Subversion, you can just run :

---------------------------------------------------------------------------

很幸运, 在 Unix 之类的系统中已提供了 Makefile, 假设你已经安了 Python,
及 Subversion, 你可以运行::

   make html

---------------------------------------------------------------------------

to check out the necessary toolset in the :file:`tools/` subdirectory and build
the HTML output files.  To view the generated HTML, point your favorite browser
at the top-level index :file:`build/html/index.html` after running "make".

---------------------------------------------------------------------------

来检查在 :file:`tools/` 下所必需的工具并且建立 HTML 输出文件.
要查看产生的 HTML, 在运行 "make" 之后, 使用浏览器打开 :file:`build/html/index.html`.

---------------------------------------------------------------------------

Available make targets are:

可以 make 的 target:

 * "html", which builds standalone HTML files for offline viewing.

---------------------------------------------------------------------------

   "html", 建立独立的 HTML 文件以供离线观看.

---------------------------------------------------------------------------

 * "htmlhelp", which builds HTML files and a HTML Help project file usable to
   convert them into a single Compiled HTML (.chm) file -- these are popular
   under Microsoft Windows, but very handy on every platform.

---------------------------------------------------------------------------

   "htmlhelp", 将生成的 HTML 文件转换成一个单独的编译过的 HTML 文件 (.chm) --
   这在微软下是很流行的, 但是在每个平台都很方便.

---------------------------------------------------------------------------

   To create the CHM file, you need to run the Microsoft HTML Help Workshop
   over the generated project (.hhp) file.

   要创建 CHM 文件, 你需要运行微软的 HTML Help Workshop 来打开生成的项目文件(.hhp).

---------------------------------------------------------------------------

 * "latex", which builds LaTeX source files as input to "pdflatex" to produce
   PDF documents.

   "latex", 建立 LaTeX 源文件, 可以使用 "pdflatex" 生成 PDF 文档.

---------------------------------------------------------------------------

 * "text", which builds a plain text file for each source file.

   "text", 创立纯文本的文件.

---------------------------------------------------------------------------

 * "linkcheck", which checks all external references to see whether they are
   broken, redirected or malformed, and outputs this information to stdout
   as well as a plain-text (.txt) file.

   "linkcheck", 将会检查所有外部的引用, 查看这些有无问题,
   并将其导出为纯文本文件 (.txt).

---------------------------------------------------------------------------

 * "changes", which builds an overview over all versionadded/versionchanged/
   deprecated items in the current version. This is meant as a help for the
   writer of the "What's New" document.

   "changes", 将会建立一个概述, 通过所有的 versionadded/versionchanged/
   deprecated 项目来生成. 这意味着用于编写 "What's New" 文档.

---------------------------------------------------------------------------

 * "coverage", which builds a coverage overview for standard library modules
   and C API.

   "coverage", 将会建立一个报导概要, 用于标准库模块和 C API.

---------------------------------------------------------------------------

 * "pydoc-topics", which builds a Python module containing a dictionary with
   plain text documentation for the labels defined in
   :file:`tools/sphinxext/pyspecific.py` -- pydoc needs these to show topic and
   keyword help.

   "pydoc-topics", 将会建立一个 Python 模块, 包含一个字典, 
   里面存放着在 :file:`tools/sphinxext/pyspecific.py` 指定的标签 --
   pydoc 需要这些显示主题和关键词帮助.

---------------------------------------------------------------------------

A "make update" updates the Subversion checkouts in :file:`tools/`.

"make update" 更新在 :file:`tools/` 由 Subversion 导出的文件.

---------------------------------------------------------------------------


Without make 没有 make
-------------------------

You'll need to install the Sphinx package, either by checking it out via :

---------------------------------------------------------------------------

你需要安装 Sphinx 包, 可以通过 svn check out 出来::

   svn co http://svn.python.org/projects/external/Sphinx-0.6.5/sphinx tools/sphinx

or by installing it from PyPI.

---------------------------------------------------------------------------

或者是从 PyPI 安装.

Then, you need to install Docutils, either by checking it out via :

---------------------------------------------------------------------------

然后, 需要安装 Docutils, 也可以通过 svn 导出::

   svn co http://svn.python.org/projects/external/docutils-0.6/docutils tools/docutils

or by installing it from http://docutils.sf.net/.

---------------------------------------------------------------------------

或从 http://docutils.sf.net/ 安装.

You also need Jinja2, either by checking it out via :

---------------------------------------------------------------------------

你还需要 Jinja2, 也可以 check out 出来::

   svn co http://svn.python.org/projects/external/Jinja-2.3.1/jinja2 tools/jinja2

or by installing it from PyPI.

---------------------------------------------------------------------------

或者从 PyPI 安装.

You can optionally also install Pygments, either as a checkout via :

---------------------------------------------------------------------------

你也可以选择安装 Pygments, 同样从 svn 中导出::

   svn co http://svn.python.org/projects/external/Pygments-1.3.1/pygments tools/pygments

or from PyPI at http://pypi.python.org/pypi/Pygments.

---------------------------------------------------------------------------

或者是从 PyPI 安装.


Then, make an output directory, e.g. under `build/`, and run :

---------------------------------------------------------------------------

然后, 建立一个输出目录, 比如 `build/` , 然后运行::

   python tools/sphinx-build.py -b<builder> . build/<outputdirectory>

where `<builder>` is one of html, text, latex, or htmlhelp (for explanations see
the make targets above).

---------------------------------------------------------------------------

此处的 `<builder>` 是 html, text, latex, 或者 htmlhelp (参考前面的解释).
