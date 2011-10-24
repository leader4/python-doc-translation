.. highlightlang:: rest

Style Guide 规范指导
====================

The Python documentation should follow the `Apple Publications Style Guide`_
wherever possible. This particular style guide was selected mostly because it
seems reasonable and is easy to get online.

---------------------------------------------------------------------------

python文档应该尽可能遵循 `Apple Publications Style Guide`_ .
这个规范经常被使用, 因为它很有道理, 而且在网上很容易获得.

Topics which are not covered in Apple's style guide will be discussed in
this document.

---------------------------------------------------------------------------

而在苹果的规范中没有提及的主题, 将在这里讨论.

All reST files use an indentation of 3 spaces.  The maximum line length is 80
characters for normal text, but tables, deeply indented code samples and long
links may extend beyond that.

---------------------------------------------------------------------------

所有的 reST 文档使用3个空格作为缩进. 而每行最大的长度是80个字符.
如果是表格, 缩进很深的代码或者长的链接可以超过它.

Make generous use of blank lines where applicable; they help grouping things
together.

---------------------------------------------------------------------------

在合适的地方, 多多使用空白行; 他们可以帮助分组.

A sentence-ending period may be followed by one or two spaces; while reST
ignores the second space, it is customarily put in by some users, for example
to aid Emacs' auto-fill mode.

---------------------------------------------------------------------------

一个句子的结尾可能尾随一两个空格; 而 reST 会忽略第二个空格,
它很可能是用户习惯的输入, 比如如 Emacs 的自动补全.

Footnotes are generally discouraged, though they may be used when they are the
best way to present specific information. When a footnote reference is added at
the end of the sentence, it should follow the sentence-ending punctuation. The
reST markup should appear something like this::

This sentence has a footnote reference. [#]_ This is the next sentence.

---------------------------------------------------------------------------

一般不鼓励使用脚注, 尽管他们是给定特殊信息最好的方式.
当脚注引用被添加到一个句子的末尾, 它应该跟在段落结束的标点后面.
reST 的标记应该看起来像这样::

这个句子有一个脚注引用. [#]_ 这是下一个句子.

Footnotes should be gathered at the end of a file, or if the file is very long,
at the end of a section. The docutils will automatically create backlinks to
the footnote reference.

---------------------------------------------------------------------------

脚注应该收集在一个文件的末尾, 或者如果这个文件非常长, 
那么就放在一个结的后面. 而 docutils 会在两者之间自动创建链接.

Footnotes may appear in the middle of sentences where appropriate.

---------------------------------------------------------------------------

脚注可能出现在一个句子里面, 如果合适的话. 

Many special names are used in the Python documentation, including the names of
operating systems, programming languages, standards bodies, and the like. Most
of these entities are not assigned any special markup, but the preferred
spellings are given here to aid authors in maintaining the consistency of
presentation in the Python documentation.

---------------------------------------------------------------------------

很多特殊的名字会用在 Python 的文档中, 包括一些操作系统的名字, 
编程语言的名字, 标准体的名字, 等等. 这里的大多数条目是没有指明特殊
的标记的, 但是为了保持一致性, 我们在这里给出一些首选的拼写.

Other terms and words deserve special mention as well; these conventions should
be used to ensure consistency throughout the documentation:

---------------------------------------------------------------------------

其他的也是值得提及的; 这些约定会保证一致性:

CPU
    For "central processing unit." Many style guides say this should be spelled
    out on the first use (and if you must use it, do so!). For the Python
    documentation, this abbreviation should be avoided since there's no
    reasonable way to predict which occurrence will be the first seen by the
    reader. It is better to use the word "processor" instead.

POSIX
    The name assigned to a particular group of standards. This is always
    uppercase.

Python
    The name of our favorite programming language is always capitalized.

Unicode
    The name of a character set and matching encoding. This is always written
    capitalized.

Unix
    The name of the operating system developed at AT&T Bell Labs in the early
    1970s.


.. _Apple Publications Style Guide: http://developer.apple.com/mac/library/documentation/UserExperience/Conceptual/APStyleGuide/APSG_2009.pdf

