.. highlightlang:: rest

Differences to the LaTeX markup 与LaTeX标记的区别
===================================================

Though the markup language is different, most of the concepts and markup types
of the old LaTeX docs have been kept -- environments as reST directives, inline
commands as reST roles and so forth.

---------------------------------------------------------------------------

尽管标记语言不一样, 但是大多数旧式的 LaTeX 文档的概念, 
及其标记的类型都被保留下来 -- environment 类似于 reST 的 directive,
inline command 类似于 reST 的 role, 等等.

---------------------------------------------------------------------------

However, there are some differences in the way these work, partly due to the
differences in the markup languages, partly due to improvements in Sphinx.  This
section lists these differences, in order to give those familiar with the old
format a quick overview of what they might run into.

---------------------------------------------------------------------------

但是, 在它们工作的方式上是有所不同的, 部分原因是标记语言的不同,
另一部分原因是在 Sphinx 中的升级. 本节列出了这些不同, 
为了使那些熟悉旧格式的可以快速地了解.

Inline markup 行内标记
-------------------------

These changes have been made to inline markup:

在行内标记中有这些改变:

* **Cross-reference roles**

  Most of the following semantic roles existed previously as inline commands,
  but didn't do anything except formatting the content as code.  Now, they
  cross-reference to known targets (some names have also been shortened):

---------------------------------------------------------------------------

  下面语义的 role 中大部分在先前都有命令相对应, 但是除了格式化其内容作为代码之外,
  就不做任何的事了. 现在, 它们交叉引用到已知的目标上 (有些名字也缩短了):

  | *mod* (previously *refmodule* or *module* 先前是 *refmodule* 或 *module* )
  | *func* (previously *function* 先前是 *function* )
  | *data* (new 新增)
  | *const*
  | *class*
  | *meth* (previously *method* 先前是 *method* )
  | *attr* (previously *member* 先前是 *member* )
  | *exc* (previously *exception* 先前是 *exception* )
  | *cdata*
  | *cfunc* (previously *cfunction* 先前是 *cfunction* )
  | *cmacro* (previously *csimplemacro* 先前是 *csimplemacro* )
  | *ctype*

  Also different is the handling of *func* and *meth*: while previously
  parentheses were added to the callable name (like ``\func{str()}``), they are
  now appended by the build system -- appending them in the source will result
  in double parentheses.  This also means that ``:func:`str(object)``` will not
  work as expected -- use ````str(object)```` instead!

---------------------------------------------------------------------------

  另外的不同是处理 *func* 和 *meth* , 前者的括号需要增加至调用的名字上 (像
  ``\func{str()}``),它们现在有构建的系统增加 -- 在源代码中添加它们会导致出现两个括号.
  这意味着 ``:func:`str(object)``` 不会像想象的那样工作 -- 使用 ````str(object)````
  替换.

* **Inline commands implemented as directives**

  These were inline commands in LaTeX, but are now directives in reST:

---------------------------------------------------------------------------

  下面的在 LaTeX 中是行内命令, 但是在 reST 中则是 directive:

  | *deprecated*
  | *versionadded*
  | *versionchanged*

  These are used like so:
  
---------------------------------------------------------------------------

  它们像这样使用::

     .. deprecated:: 2.5
        Reason of deprecation.

  Also, no period is appended to the text for *versionadded* and
  *versionchanged*.

---------------------------------------------------------------------------

  同样, 对于 *versionadded* 和 *versionchanged* 没有句号增加到文本中.

  | *note*
  | *warning*

  These are used like so:
  
---------------------------------------------------------------------------

  这些用起来像这样::

     .. note::

        Content of note.

* **Otherwise changed commands**

  The *samp* command previously formatted code and added quotation marks around
  it.  The *samp* role, however, features a new highlighting system just like
  *file* does:

---------------------------------------------------------------------------

  *samp* 命令以前用于格式化代码, 并在其周围增加引号.
  但现在的 *samp* , 其特点是一个新的高亮系统,
  就像 *file* 那样:

     ``:samp:`open({filename}, {mode})``` results in :samp:`open({filename}, {mode})`

* **Dropped commands**

  These were commands in LaTeX, but are not available as roles:

---------------------------------------------------------------------------

  这些是 LaTeX 中的命令, 但不是 role :

  | *bfcode*
  | *character* (use :samp:`\`\`'c'\`\``)
  | *citetitle* (use ```Title <URL>`_``)
  | *code* (use ````code````)
  | *email* (just write the address in body text)
  | *filenq*
  | *filevar* (use the ``{...}`` highlighting feature of *file*)
  | *programopt*, *longprogramopt* (use *option*)
  | *ulink* (use ```Title <URL>`_``)
  | *url* (just write the URL in body text)
  | *var* (use ``*var*``)
  | *infinity*, *plusminus* (use the Unicode character)
  | *shortversion*, *version* (use the ``|version|`` and ``|release|`` substitutions)
  | *emph*, *strong* (use the reST markup)

* **Backslash escaping**

  In reST, a backslash must be escaped in normal text, and in the content of
  roles.  However, in code literals and literal blocks, it must not be escaped.
  Example: ``:file:`C:\\Temp\\my.tmp``` vs. ````open("C:\Temp\my.tmp")````.

---------------------------------------------------------------------------

  在 reST 中, 一个反斜杠在正常的文本中和在 role 的内容中都必须转义,
  但是在代码块中, 就不需要转义了. 比如: ``:file:`C:\\Temp\\my.tmp``` vs. 
  ````open("C:\Temp\my.tmp")````.


 信息单元
--------------------------------

Information units (*...desc* environments) have been made reST directives.
These changes to information units should be noted:

---------------------------------------------------------------------------

信息单元 ( *...desc* 环境 ) 做成了 reST 的 directive.
这些改变需要注意:

* **New names**

  "desc" has been removed from every name.  Additionally, these directives have
  new names:

---------------------------------------------------------------------------

  "desc" 已经从每个名字中移除. 另外, 这些指示符有了新的名字:

  | *cfunction* (previously *cfuncdesc*)
  | *cmacro* (previously *csimplemacrodesc*)
  | *exception* (previously *excdesc*)
  | *function* (previously *funcdesc*)
  | *attribute* (previously *memberdesc*)

  The *classdesc\** and *excclassdesc* environments have been dropped, the
  *class* and *exception* directives support classes documented with and without
  constructor arguments.

* **Multiple objects**

  The equivalent of the *...line* commands is:
  
---------------------------------------------------------------------------

  与 *...line* 命令等同的是::

     .. function:: do_foo(bar)
                   do_bar(baz)

        Description of the functions.

  IOW, just give one signatures per line, at the same indentation level.

---------------------------------------------------------------------------

  换句话说, 仅要每行一个签名, 并在同一缩进级别.

* **Arguments**

  There is no *optional* command.  Just give function signatures like they
  should appear in the output:
  
---------------------------------------------------------------------------

  没有 *optional* 命令. 只要将它们写出输出的样子就可以了::

     .. function:: open(filename[, mode[, buffering]])

        Description.

  Note: markup in the signature is not supported.

---------------------------------------------------------------------------

  注意: 在签名中不能使用标记.

* **Indexing**

  The *...descni* environments have been dropped.  To mark an information unit
  as unsuitable for index entry generation, use the *noindex* option like so:
  
  *...descni* 环境已经被移除了. 为了使一个信息单元不作为索引项,
  使用 *noindex* 选项, 像这样::

     .. function:: foo_*
        :noindex:

        Description.

* **New information units**

  There are new generic information units: One is called "describe" and can be
  used to document things that are not covered by the other units:
  
  有些新的信息单元: 一个称为 "describe" , 可以用以其他的信息单元::

     .. describe:: a == b

        The equals operator.

  The others are:
  
  另外的是::

     .. cmdoption:: -O

        Describes a command-line option.

     .. envvar:: PYTHONINSPECT

        Describes an environment variable.


Structure 结构
-------------------

The LaTeX docs were split in several toplevel manuals.  Now, all files are part
of the same documentation tree, as indicated by the *toctree* directives in the
sources (though individual output formats may choose to split them up into parts
again).  Every *toctree* directive embeds other files as subdocuments of the
current file (this structure is not necessarily mirrored in the filesystem
layout).  The toplevel file is :file:`contents.rst`.

LaTeX 文档一般以文档级别进行拆分. 现在, 所有文件都是同一文档树的一部分,
就是在 *toctree* 中指明的 (尽管可以在输出时又将他们拆分).
每一个 *toctree* 嵌于其他文件作为当前文件的子文档 (在文件系统的布置上,
这个结构并不是必须要被映出). 最顶层的文件是 :file:`contents.rst`.

However, most of the old directory structure has been kept, with the
directories renamed as follows:

但是, 许多旧式的目录结构被保存下来, 名字像下面一样重命名为:

* :file:`api` -> :file:`c-api`
* :file:`dist` -> :file:`distutils`, with the single TeX file split up
* :file:`doc` -> :file:`documenting`
* :file:`ext` -> :file:`extending`
* :file:`inst` -> :file:`installing`
* :file:`lib` -> :file:`library`
* :file:`mac` -> merged into :file:`library`, with :file:`mac/using.tex`
  moved to :file:`using/mac.rst`
* :file:`ref` -> :file:`reference`
* :file:`tut` -> :file:`tutorial`, with the single TeX file split up


.. XXX more (index-generating, production lists, ...)

