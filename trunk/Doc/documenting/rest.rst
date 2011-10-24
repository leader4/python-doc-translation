.. highlightlang:: rest

reStructuredText Primer reStructuredText 入门
================================================

This section is a brief introduction to reStructuredText (reST) concepts and
syntax, intended to provide authors with enough information to author documents
productively.  Since reST was designed to be a simple, unobtrusive markup
language, this will not take too long.

---------------------------------------------------------------------------

本节将简要介绍 reStructuredText (reST) 的基本概念与语法, 
尽量使作者可以掌握足够信息并能够有效的编写文档. 因为 reST 设计的目标
就是简单, 没有太多要求的标记语言, 所以不会花费很长时间. 

.. seealso::

    The authoritative `reStructuredText User
    Documentation <http://docutils.sourceforge.net/rst.html>`_.


Paragraphs 段落
-----------------

The paragraph is the most basic block in a reST document.  Paragraphs are simply
chunks of text separated by one or more blank lines.  As in Python, indentation
is significant in reST, so all lines of the same paragraph must be left-aligned
to the same level of indentation.

---------------------------------------------------------------------------

在 reST 中, 段落是最基本的块. 段落是以一个或多个空行来分割. 
就像在 Python 中一样, 缩进也是 reST 中的一个特色, 所以所有具有相同
缩进级别的行属于同一段落. 

Inline markup 行内标记
--------------------------

The standard reST inline markup is quite simple: use

* one asterisk: ``*text*`` for emphasis (italics),
* two asterisks: ``**text**`` for strong emphasis (boldface), and
* backquotes: ````text```` for code samples.

---------------------------------------------------------------------------

标准的行内标记非常简单: 使用

* 一个星号: ``*text*`` 表强调 (斜体),
* 两个星号: ``**text**`` 表更强的强调 (粗体), 并且
* 反引号: ````text```` 表示代码实例.

If asterisks or backquotes appear in running text and could be confused with
inline markup delimiters, they have to be escaped with a backslash.

---------------------------------------------------------------------------

如果星号或者反引号出现在文本中, 但可能与行内标记有冲突,
那么他们需要使用一个反斜杠进行转义. 

Be aware of some restrictions of this markup:

* it may not be nested,
* content may not start or end with whitespace: ``* text*`` is wrong,
* it must be separated from surrounding text by non-word characters.  Use a
  backslash escaped space to work around that: ``thisis\ *one*\ word``.

---------------------------------------------------------------------------

注意这种标记的一些限制:

* 不能够嵌套,
* 里面的内容不能以空格开头或结尾: ``* test*`` 是错的,
* 在这些的周围必须要以非单词字符分割. 
  使用一个反斜杠来转义一个空格就可以了: ``thisis\ *one*\ word`` .

These restrictions may be lifted in future versions of the docutils.

---------------------------------------------------------------------------

这些限制可能会在以后被去掉.

reST also allows for custom "interpreted text roles"', which signify that the
enclosed text should be interpreted in a specific way.  Sphinx uses this to
provide semantic markup and cross-referencing of identifiers, as described in
the appropriate section.  The general syntax is ``:rolename:`content```.

---------------------------------------------------------------------------

reST 同样允许自定义的 "interpreted text roles", 
这就意味着围起来的文字可以以特殊的方式进行解释.
Sphinx 使用了这个提供了语义标记, 和交叉引用的标识符,
在下面的章节中将会提及. 一般的语法是 ``:rolename:`content``` .


 列表和引用
------------------------------

List markup is natural: just place an asterisk at the start of a paragraph and
indent properly.  The same goes for numbered lists; they can also be
autonumbered using a ``#`` sign:

---------------------------------------------------------------------------

列表的标记是很自然的: 把星号放在每个段落的起始处,并使用合适的缩进.
同样, 这也适合有序的列表; 它们可以使用 ``#`` 符号进行自动编号:

::

   * This is a bulleted list.
   * It has two items, the second
     item uses two lines.

   1. This is a numbered list.
   2. It has two items too.

   #. This is a numbered list.
   #. It has two items too.


Nested lists are possible, but be aware that they must be separated from the
parent list items by blank lines:

---------------------------------------------------------------------------

嵌套的列表是可以的, 但是注意它们必须要与父列表以空行分割:

::

   * this is
   * a list

     * with a nested list
     * and some subitems

   * and here the parent list continues

Definition lists are created as follows:

---------------------------------------------------------------------------

定义列表像下面这样定义:

::

   term (up to a line of text)
      Definition of the term, which must be indented

      and can even consist of multiple paragraphs

   next term
      Description.


Paragraphs are quoted by just indenting them more than the surrounding
paragraphs.

---------------------------------------------------------------------------

引用的段落只需相对于周围的段落有缩进就可以了.


 源代码
----------------------

Literal code blocks are introduced by ending a paragraph with the special marker
``::``.  The literal block must be indented:

---------------------------------------------------------------------------

源代码以一个特殊的标记 ``::`` 开始. 而且代码必须要缩进::

   This is a normal text paragraph. The next paragraph is a code sample::

      It is not processed in any way, except
      that the indentation is removed.

      It can span multiple lines.

   This is a normal text paragraph again.

The handling of the ``::`` marker is smart:

---------------------------------------------------------------------------

处理 ``::`` 会很智能:

* If it occurs as a paragraph of its own, that paragraph is completely left
  out of the document.
  
---------------------------------------------------------------------------

  如果在段落中出现, 那么这个段落还是完整的保留下来.

* If it is preceded by whitespace, the marker is removed.

---------------------------------------------------------------------------

  如果前面有空格, 那么这个标记就被删除了.

* If it is preceded by non-whitespace, the marker is replaced by a single
  colon.

---------------------------------------------------------------------------

  如果前面不是空格, 那么就会被替换成一个冒号.

That way, the second sentence in the above example's first paragraph would be
rendered as "The next paragraph is a code sample:".

---------------------------------------------------------------------------

那么, 上面的那句例子就会成为如 "The next paragraph is a code sample:"
的样子.


 超链接
---------------------

 外部链接
^^^^^^^^^^^^^^^^^^^^^^^^^

Use ```Link text <http://target>`_`` for inline web links.  If the link text
should be the web address, you don't need special markup at all, the parser
finds links and mail addresses in ordinary text.

---------------------------------------------------------------------------

使用 ```链接文字 <http:://target>`_`` 作为网页链接. 
如果链接文字是一个网页的地址, 那么你就不需要特殊的标记了,
解析器会帮助你找到链接和邮件地址.

Internal links 内部链接
^^^^^^^^^^^^^^^^^^^^^^^^^^

Internal linking is done via a special reST role, see the section on specific
markup, :ref:`doc-ref-role`.

---------------------------------------------------------------------------

内部链接可以使用 reST 的特殊标记, 参考特殊标记的那节, :ref:`doc-ref-role` .


Sections 章节
----------------

Section headers are created by underlining (and optionally overlining) the
section title with a punctuation character, at least as long as the text

---------------------------------------------------------------------------

章节的标题使用在标题下放置一个字符来创建, 而此字符至少要和文本一样长:

::

   =================
   This is a heading
   =================

Normally, there are no heading levels assigned to certain characters as the
structure is determined from the succession of headings.  However, for the
Python documentation, we use this convention:

---------------------------------------------------------------------------

一般来说, 没有非常明确的要求需要使用哪种符号来指明不同级别的标题.
但是, 对于 Python 文档来说, 我们使用这种约定:

* ``#`` with overline, for parts
* ``*`` with overline, for chapters
* ``=``, for sections
* ``-``, for subsections
* ``^``, for subsubsections
* ``"``, for paragraphs


Explicit Markup 显式的标记
---------------------------

"Explicit markup" is used in reST for most constructs that need special
handling, such as footnotes, specially-highlighted paragraphs, comments, and
generic directives.

---------------------------------------------------------------------------

在 reST 中, "Explicit markup" 用于那些需要额外处理的构造,
比如脚注, 特殊高亮的段落, 注释, 和通用的指示符.

An explicit markup block begins with a line starting with ``..`` followed by
whitespace and is terminated by the next paragraph at the same level of
indentation.  (There needs to be a blank line between explicit markup and normal
paragraphs.  This may all sound a bit complicated, but it is intuitive enough
when you write it.)

---------------------------------------------------------------------------

一个显式的标记块一般以 ``..`` 开始, 然后后面跟着空白,
并且以与其相同级别的缩进表示结束. (当然, 此处我们还需要一个空白行来分隔
标记块与正常的段落. 这可能听起来有点复杂, 但是当你写它的时候, 它就变得非常直观.)


 指示符
-------------------

A directive is a generic block of explicit markup.  Besides roles, it is one of
the extension mechanisms of reST, and Sphinx makes heavy use of it.

---------------------------------------------------------------------------

一个指示符就是一个普通的显式标记块. 它是一个 reST 可扩展的部分,
在 Sphinx 中使用了大量的这种标记.

Basically, a directive consists of a name, arguments, options and content. (Keep
this terminology in mind, it is used in the next chapter describing custom
directives.)  Looking at this example, 

---------------------------------------------------------------------------

最基本的, 一个指示符包含一个名字, 参数, 选项和内容.
(请记住这个术语, 它将在下一章描述) 请看下面的例子,

::

   .. function:: foo(x)
                 foo(y, z)
      :bar: no

      Return a line of text input from the user.

``function`` is the directive name.  It is given two arguments here, the
remainder of the first line and the second line, as well as one option ``bar``
(as you can see, options are given in the lines immediately following the
arguments and indicated by the colons).

---------------------------------------------------------------------------

``function`` 是一个指示符的名字. 此处给了两个参数 (即前面两行剩下的) ,
和选项 ``bar`` (就像你看到的, 选项是紧跟参数的, 并且通过冒号指明) .

The directive content follows after a blank line and is indented relative to the
directive start.

---------------------------------------------------------------------------

而后面的内容, 则是在一个空白行之后, 并且相对于指示符的开头有一定的缩进.


Footnotes 脚注
------------------

For footnotes, use ``[#]_`` to mark the footnote location, and add the footnote
body at the bottom of the document after a "Footnotes" rubric heading, like so:

---------------------------------------------------------------------------

对于脚注, 使用 ``[#]_`` 来标记脚注的位置, 并增加一个脚注的主体在文档的后面,
像这样:

::

   Lorem ipsum [#]_ dolor sit amet ... [#]_

   .. rubric:: Footnotes

   .. [#] Text of the first footnote.
   .. [#] Text of the second footnote.

You can also explicitly number the footnotes for better context.

---------------------------------------------------------------------------

你可以使用显式的数字.


Comments 注释
--------------

Every explicit markup block which isn't a valid markup construct (like the
footnotes above) is regarded as a comment.

---------------------------------------------------------------------------

每一个显示的标记块如果没有一个合法的标记构造就会被认为是注释.


Source encoding 代码的编码
------------------------------

Since the easiest way to include special characters like em dashes or copyright
signs in reST is to directly write them as Unicode characters, one has to
specify an encoding:

---------------------------------------------------------------------------

为了以最简单的方式包含一些特殊字符, 我们将使用 Unicode 字符, 但需要指明一种编码方式:

All Python documentation source files must be in UTF-8 encoding, and the HTML
documents written from them will be in that encoding as well.

---------------------------------------------------------------------------

所有的 Python 文档的源代码都将是 UTF-8 的编码, 而生成的 HTML 文档也将是这种编码.


Gotchas
-------

There are some problems one commonly runs into while authoring reST documents:

* **Separation of inline markup:** As said above, inline markup spans must be
  separated from the surrounding text by non-word characters, you have to use
  an escaped space to get around that.
