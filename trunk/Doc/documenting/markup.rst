.. highlightlang:: rest

Additional Markup Constructs 额外的标记
=========================================

Sphinx adds a lot of new directives and interpreted text roles to standard reST
markup.  This section contains the reference material for these facilities.
Documentation for "standard" reST constructs is not included here, though
they are used in the Python documentation.

---------------------------------------------------------------------------

除了标准 reST 的标记, Sphinx 增加了很多的控制指令和解释的文字. 
本节将包含一个参考手册, 对此进行具体描述. "标准" 的 reST 文档
是不会在这里介绍的, 尽管他们会在 Python 文档中用到.

.. note::

   This is just an overview of Sphinx' extended markup capabilities; full
   coverage can be found in `its own documentation
   <http://sphinx.pocoo.org/contents.html>`_.

---------------------------------------------------------------------------

   这个仅仅是对 Sphinx 扩展标记的一个概述; 
   完全的参考手册可以在 `它自己的文档 <http://sphinx.pocoo.org/contents.html>`_ 
   中找到.


 元信息标记
-----------------------------------

.. describe:: sectionauthor

   Identifies the author of the current section.  The argument should include
   the author's name such that it can be used for presentation (though it isn't)
   and email address.  The domain name portion of the address should be lower
   case.  Example::

      .. sectionauthor:: Guido van Rossum <guido@python.org>

---------------------------------------------------------------------------

   本节作者的标识符. 参数应该包含作者的名字和邮箱地址.
   而地址的域名应该是小写的. 例如::

      .. sectionauthor:: Guido van Rossum <guido@python.org>


   Currently, this markup isn't reflected in the output in any way, but it helps
   keep track of contributions.

---------------------------------------------------------------------------

目前, 这个标记并没有在输出时有什么反映, 但是它记录了谁对此有贡献.

Module-specific markup 关于模块的标记
--------------------------------------

The markup described in this section is used to provide information about a
module being documented.  Each module should be documented in its own file.
Normally this markup appears after the title heading of that file; a typical
file might start like this:

---------------------------------------------------------------------------

这个标记描述了关于文档中模块的相关信息. 每个模块都应该在自己的文件中描述.
一般来说, 这个标记出现在文件的标题后面; 一个典型的文件会看起来像这样::

   :mod:`parrot` -- Dead parrot access
   ===================================

   .. module:: parrot
      :platform: Unix, Windows
      :synopsis: Analyze and reanimate dead parrots.
   .. moduleauthor:: Eric Cleese <eric@python.invalid>
   .. moduleauthor:: John Idle <john@python.invalid>

As you can see, the module-specific markup consists of two directives, the
``module`` directive and the ``moduleauthor`` directive.

---------------------------------------------------------------------------

就像你看到的, 关于模块的标记包含有两个指令, 一个是 ``module`` ,  另一个
``moduleauthor`` 指令.

.. describe:: module

   This directive marks the beginning of the description of a module, package,
   or submodule. The name should be fully qualified (i.e. including the
   package name for submodules).

---------------------------------------------------------------------------

       这个指令标记了模块, 包, 或子模块的描述. 这个名字应该完整 (
      也就是说对于子模块应该包含包名) .

   The ``platform`` option, if present, is a comma-separated list of the
   platforms on which the module is available (if it is available on all
   platforms, the option should be omitted).  The keys are short identifiers;
   examples that are in use include "IRIX", "Mac", "Windows", and "Unix".  It is
   important to use a key which has already been used when applicable.

---------------------------------------------------------------------------

   ``platform`` 选项, 如果存在, 是一个用逗号分隔的列表, 这个指明了能够使用
   该模块的平台 (如果在所有平台都可以使用, 那么就应该忽略这个选项).
   而此处使用的应该是简短的标识符; 比如 "IRIX", "Mac", "Windows", and "Unix".
   使用正确的键值是很重要的.

   The ``synopsis`` option should consist of one sentence describing the
   module's purpose -- it is currently only used in the Global Module Index.

---------------------------------------------------------------------------

   ``synopsis`` 选项应该包含一个模块目的的描述 -- 目前这个只用于全局模块索引.

   The ``deprecated`` option can be given (with no value) to mark a module as
   deprecated; it will be designated as such in various locations then.

---------------------------------------------------------------------------

   ``deprecated`` 选项可以使用 (而不需要值) 用于标记一个模块是被反对的;
   它将会在合适的地方被指定.

.. describe:: moduleauthor

   The ``moduleauthor`` directive, which can appear multiple times, names the
   authors of the module code, just like ``sectionauthor`` names the author(s)
   of a piece of documentation.  It too does not result in any output currently.

---------------------------------------------------------------------------

   ``moduleauthor`` 指令, 可以多次出现, 指明了模块编写者, 就像 ``sectionauthor``
   指明了文档的作者一样. 这个同样也没有任何输出. 

.. note::

   It is important to make the section title of a module-describing file
   meaningful since that value will be inserted in the table-of-contents trees
   in overview files.

---------------------------------------------------------------------------

     下面这点很重要, 每个模块的章节标题应该有意义, 因为这个将被插入到目录中.


 信息单元
----------------------------

There are a number of directives used to describe specific features provided by
modules.  Each directive requires one or more signatures to provide basic
information about what is being described, and the content should be the
description.  The basic version makes entries in the general index; if no index
entry is desired, you can give the directive option flag ``:noindex:``.  The
following example shows all of the features of this directive type:

---------------------------------------------------------------------------

在模块中会有很多用以特殊作用的指示符. 每个指示符都需要一个甚至更多的签名
用于提供基本的信息, 而其内容应该是具体的描述. 最基本的版本应该是有索引的;
如果没有索引, 那么你可以给指示符一个选项 ``:noindex:`` . 
下面的例子给出了所有的特征:

::

    .. function:: spam(eggs)
                  ham(eggs)
       :noindex:

       Spam or ham the foo.

The signatures of object methods or data attributes should always include the
type name (``.. method:: FileInput.input(...)``), even if it is obvious from the
context which type they belong to; this is to enable consistent
cross-references.  If you describe methods belonging to an abstract protocol,
such as "context managers", include a (pseudo-)type name too to make the
index entries more informative.

---------------------------------------------------------------------------

对象的方法或成员的签名应该总包含其类型的名称 (``.. method:: FileInput.input(...)``),
尽管它可能是从上下文中很容易看出; 这个就可以使用交叉引用了. 如果你描述的方法
属于一个抽象的协议, 像 "context managers", 同样请包含一个 (伪) 类型名用以
在索引中给出更多信息.

The directives are:

.. describe:: c:function

   Describes a C function. The signature should be given as in C, e.g.:

---------------------------------------------------------------------------

   描述一个 C 的函数. 其签名应该是以 C 的形式给出, 比如::

      .. c:function:: PyObject* PyType_GenericAlloc(PyTypeObject * type, Py_ssize_t nitems)

   This is also used to describe function-like preprocessor macros.  The names
   of the arguments should be given so they may be used in the description.

---------------------------------------------------------------------------

   同样, 这也可以用于描述类似函数的预处理宏. 参数的名称也应该给出,
   这样可以方便下面的描述. 

   Note that you don't have to backslash-escape asterisks in the signature,
   as it is not parsed by the reST inliner.

---------------------------------------------------------------------------

   注意, 你不需要在签名中使用反斜杠来转义星号, 因为此处的不会被 reST 处理.
   ( 译者注: 当然, 如果你不放心, 可以在星号两边留下一个空格. )

.. describe:: c:member

   Describes a C struct member. Example signature:

---------------------------------------------------------------------------

   描述 C 结构体的成员. 像下面的签名::

      .. c:member:: PyObject* PyTypeObject.tp_bases

   The text of the description should include the range of values allowed, how
   the value should be interpreted, and whether the value can be changed.
   References to structure members in text should use the ``member`` role.

---------------------------------------------------------------------------

   此处的描述应该包含值所允许的范围, 这个值是如何被解释的, 以及这个值能否改变.
   引用这个结构体的成员需要使用 ``member`` .

.. describe:: c:macro

   Describes a "simple" C macro.  Simple macros are macros which are used
   for code expansion, but which do not take arguments so cannot be described as
   functions.  This is not to be used for simple constant definitions.  Examples
   of its use in the Python documentation include :c:macro:`PyObject_HEAD` and
   :c:macro:`Py_BEGIN_ALLOW_THREADS`.

---------------------------------------------------------------------------

   描述一个 "简单的" C 宏. 简单的宏就是那些用于代码扩展, 但是不接受参数的宏.
   这个不会用于简单的常量定义. 举个例子, 使用的有 :c:macro:`PyObject_HEAD`
   和 :c:macro:`Py_BEGIN_ALLOW_THREADS` .

.. describe:: c:type

   Describes a C type. The signature should just be the type name.

---------------------------------------------------------------------------

   描述一个 C 的类型. 签名就是这个类型名.

.. describe:: c:var

   Describes a global C variable.  The signature should include the type, such
   as:

---------------------------------------------------------------------------

   描述一个全局的 C 变量. 这个签名应该包含其类型, 像这样:
   
   ::

      .. cvar:: PyObject* PyClass_Type

.. describe:: data

   Describes global data in a module, including both variables and values used
   as "defined constants".  Class and object attributes are not documented
   using this environment.

---------------------------------------------------------------------------

   描述一个模块中的全局数据, 包括变量和那些当作常量的值.
   类和对象的属性不应该在此环境中出现.

.. describe:: exception

   Describes an exception class.  The signature can, but need not include
   parentheses with constructor arguments.

---------------------------------------------------------------------------

   描述一个异常类. 这个签名可以, 但不需要包含构造是的参数及括号.

.. describe:: function

   Describes a module-level function.  The signature should include the
   parameters, enclosing optional parameters in brackets.  Default values can be
   given if it enhances clarity.  For example:

---------------------------------------------------------------------------

   描述一个模块级别的函数. 签名应该包含参数, 以方括号括起的可选参数.
   默认的值可以给出. 例如::

      .. function:: Timer.repeat([repeat=3[, number=1000000]])

   Object methods are not documented using this directive. Bound object methods
   placed in the module namespace as part of the public interface of the module
   are documented using this, as they are equivalent to normal functions for
   most purposes.

---------------------------------------------------------------------------

   对象的方法不应该在此处使用. 作为模块命名空间中的接口的绑定对象的方法, 
   应该使用这个, 因为大多时候它们与普通的函数相同.

   The description should include information about the parameters required and
   how they are used (especially whether mutable objects passed as parameters
   are modified), side effects, and possible exceptions.  A small example may be
   provided.

---------------------------------------------------------------------------

   描述的信息应该包含如所需参数的信息, 它们如何被使用 (特别是那些对象被出过去后,
   能否被改变), 副作用, 和可能的异常. 也应该提供一个简单的例子.

.. describe:: decorator

   Describes a decorator function.  The signature should *not* represent the
   signature of the actual function, but the usage as a decorator.  For example,
   given the functions

---------------------------------------------------------------------------

   描述一个修饰器函数. 此签名不应该给出真实函数的签名, 但是需要给出作为一个
   修饰器应该如何使用. 举个例子给这样的函数

   .. code-block:: python

      def removename(func):
          func.__name__ = ''
          return func

      def setnewname(name):
          def decorator(func):
              func.__name__ = name
              return func
          return decorator

   the descriptions should look like this:

---------------------------------------------------------------------------

   而其描述应该这样子::

      .. decorator:: removename

         Remove name of the decorated function.

---------------------------------------------------------------------------

         移除被修饰函数的名程

      .. decorator:: setnewname(name)

         Set name of the decorated function to *name*.

---------------------------------------------------------------------------

         设置被修饰的名称为 *name*.

   There is no ``deco`` role to link to a decorator that is marked up with
   this directive; rather, use the ``:func:`` role.

---------------------------------------------------------------------------

   在此处, 没有 ``deco`` 这样的东西可以直接链接到一个修饰器;
   但你可以使用 ``:func:`` .

.. describe:: class

   Describes a class.  The signature can include parentheses with parameters
   which will be shown as the constructor arguments.

---------------------------------------------------------------------------

   描述一个类. 这个签名应该包含构造时所需的参数.

.. describe:: attribute

   Describes an object data attribute.  The description should include
   information about the type of the data to be expected and whether it may be
   changed directly.

---------------------------------------------------------------------------

   描述一个对象的数据属性. 此处的描述应该包含数据所需的类型,
   以及是否可以直接改变.

.. describe:: method

   Describes an object method.  The parameters should not include the ``self``
   parameter.  The description should include similar information to that
   described for ``function``.

---------------------------------------------------------------------------

   描述一个对象的方法. 参数无须包含 ``self`` . 其描述和 ``function`` 类似就可以了. 

.. describe:: decoratormethod

   Same as ``decorator``, but for decorators that are methods.

---------------------------------------------------------------------------

   和 ``decorator`` 一样,但是修饰的是方法.

   Refer to a decorator method using the ``:meth:`` role.

---------------------------------------------------------------------------

   引用修饰器方法要使用 ``:meth:`` .

.. describe:: opcode

   Describes a Python :term:`bytecode` instruction.

---------------------------------------------------------------------------

   描述 Python 的 :term:`bytecode` .

.. describe:: cmdoption

   Describes a Python command line option or switch.  Option argument names
   should be enclosed in angle brackets.  Example:

---------------------------------------------------------------------------

   描述 Python 命令行选项. 选项参数的名称需要以尖括号括起来.
   例子:
   
   ::

      .. cmdoption:: -m <module>

         Run a module as a script.

.. describe:: envvar

   Describes an environment variable that Python uses or defines.

---------------------------------------------------------------------------

   描述一个 Python 使用或定义的环境变量.


There is also a generic version of these directives:

---------------------------------------------------------------------------

还有指示符的普遍版本:

.. describe:: describe

   This directive produces the same formatting as the specific ones explained
   above but does not create index entries or cross-referencing targets.  It is
   used, for example, to describe the directives in this document. Example:

---------------------------------------------------------------------------

   这个指示符和前面所说的产生的格式化效果一样, 但是 *不会* 创建索引项
   或是交叉引用. 这一般用于描述文档中的指示符. 比如:
   
   ::

      .. describe:: opcode

         Describes a Python bytecode instruction.


显示示例代码
--------------------------------------

Examples of Python source code or interactive sessions are represented using
standard reST literal blocks.  They are started by a ``::`` at the end of the
preceding paragraph and delimited by indentation.

---------------------------------------------------------------------------

Python 的源码或者是交互的会话都以 reST 的 ``literal block`` 来表示.
通过 ``::`` 开头, 然后内容要进行缩进, 最后以同级的缩进表示结束.

Representing an interactive session requires including the prompts and output
along with the Python code.  No special markup is required for interactive
sessions.  After the last line of input or output presented, there should not be
an "unused" primary prompt; this is an example of what *not* to do:

---------------------------------------------------------------------------

交互式会话的表示需要包含提示和输出. 此处不需要特殊的标记. 
在输入和输出结束后, 最后一行不要放上一个未使用的提示符; 下面是一个不要那样的例子:

::

   >>> 1 + 1
   2
   >>>

Syntax highlighting is handled in a smart way:

语法的高亮会以一种智能的方式处理:

* There is a "highlighting language" for each source file.  Per default,
  this is ``'python'`` as the majority of files will have to highlight Python
  snippets.

  对于每个文档源文件, 都会有一个 "高亮的语言" . 在此处, 默认的都是设为 Python.

* Within Python highlighting mode, interactive sessions are recognized
  automatically and highlighted appropriately.

  在 Python 高亮的模式下, 交互会话会被自动识别并且进行合适的高亮.

* The highlighting language can be changed using the ``highlightlang``
  directive, used as follows:

  高亮的语言可以使用 ``highlightlang`` 进行控制, 像下面这样使用:
  
  ::

     .. highlightlang:: c

  This language is used until the next ``highlightlang`` directive is
  encountered.

  这样这个语言就会被使用了, 直到遇到下一个 ``highlightlang`` 为止.

* The values normally used for the highlighting language are:

  一般会用到的:

  * ``python`` (the default)
  * ``c``
  * ``rest``
  * ``none`` (no highlighting)

* If highlighting with the current language fails, the block is not highlighted
  in any way.

  如果以某种语言高亮失败了, 那么这个块就不被任何形式高亮.

Longer displays of verbatim text may be included by storing the example text in
an external file containing only plain text.  The file may be included using the
``literalinclude`` directive. [1]_ For example, to include the Python source file
:file:`example.py`, use:

如果在使用时需要很大篇幅的源代码, 我们可以使用额外导入的方式将代码引入进来.
需要导入的文件可以使用 ``literalinclude`` 指示符导入. [1]_ 举个例子, 
要导入一个 Python 源码 :file:`example.py` , 使用:

::

   .. literalinclude:: example.py

The file name is relative to the current file's path.  Documentation-specific
include files should be placed in the ``Doc/includes`` subdirectory.

这个文件的路径是相对于当前文件的路径的. 包含的文件最好放到 ``Doc/includes`` 下面.


Inline markup 行内标记
-----------------------

As said before, Sphinx uses interpreted text roles to insert semantic markup in
documents.

在前面也说过, Sphinx 使用特殊的标记来指明语义.

Names of local variables, such as function/method arguments, are an exception,
they should be marked simply with ``*var*``.

一个局部变量的名字, 比如函数/方法的参数, 它们是一个例外, 只需要用星号括起来即可,
如 ``*var*`` .

For all other roles, you have to write ``:rolename:`content```.

对于其他的, 你需要写 ``:rolename:`content```.

There are some additional facilities that make cross-referencing roles more
versatile:

有一些额外的设施可以以多种方式使交叉引用:

* You may supply an explicit title and reference target, like in reST direct
  hyperlinks: ``:role:`title <target>``` will refer to *target*, but the link
  text will be *title*.

  你可以提供一个显式的标题及一个引用对象, 像 reST 中直接的超链接: 
  ``:role:`title <target>``` 将会指向 *target* , 但是链接显示的会是 *title*.

* If you prefix the content with ``!``, no reference/hyperlink will be created.

  如果你在内容前面加了个 ``!``, 那么将不会创建引用/链接.

* For the Python object roles, if you prefix the content with ``~``, the link
  text will only be the last component of the target.  For example,
  ``:meth:`~Queue.Queue.get``` will refer to ``Queue.Queue.get`` but only
  display ``get`` as the link text.

  对于 Python 对象来说, 如果你在内容前面放上一个 ``~`` , 链接的文字将只是最后一个.
  比如 ``:meth:`~Queue.Queue.get``` 将会指向 ``Queue.Queue.get`` ,
  但是将只显示 ``get`` .

  In HTML output, the link's ``title`` attribute (that is e.g. shown as a
  tool-tip on mouse-hover) will always be the full target name.

  在输出的 HTML 中, 链接的 ``title`` 属性 (也就是在你将鼠标放到链接上时,
  显示的文字) 将永远是全名.

The following roles refer to objects in modules and are possibly hyperlinked if
a matching identifier is found:

下面的将会指向一个模块中的对象, 并且如果有匹配的标识符就会生成超链接:

.. describe:: mod

   The name of a module; a dotted name may be used.  This should also be used for
   package names.

   模块的名称; 有点的名称将被使用. 同样这也用于一个包名称.

.. describe:: func

   The name of a Python function; dotted names may be used.  The role text
   should not include trailing parentheses to enhance readability.  The
   parentheses are stripped when searching for identifiers.

   一个 Python 函数的名称; 有点的名称可以使用. 这里所用的内容
   不应该包含后面的括号. 当搜索标识符时, 后面的括号是被去除了的.

.. describe:: data

   The name of a module-level variable or constant.

   模块级别的变量或常量的名称.

.. describe:: const

   The name of a "defined" constant.  This may be a C-language ``#define``
   or a Python variable that is not intended to be changed.

   一个定义过的常量的名称. 它可能是 C 语言中的 ``#define`` 
   或是 Python 中设为不可变的变量.

.. describe:: class

   A class name; a dotted name may be used.

   一个类名; 可以使用有点的名称.

.. describe:: meth

   The name of a method of an object.  The role text should include the type
   name and the method name.  A dotted name may be used.

   一个对象方法的名称. 此处应该包括类型名和方法名. 有点的也可以使用.

.. describe:: attr

   The name of a data attribute of an object.

   一个对象的数据属性.

.. describe:: exc

   The name of an exception. A dotted name may be used.

   一个异常的名称. 有点的可以使用.

The name enclosed in this markup can include a module name and/or a class name.
For example, ``:func:`filter``` could refer to a function named ``filter`` in
the current module, or the built-in function of that name.  In contrast,
``:func:`foo.filter``` clearly refers to the ``filter`` function in the ``foo``
module.

在这些标记中的名称可以包含一个模块名 和/或 一个类名称.
举个例子, ``:func:`filter``` 可以指向一个名为 ``filter`` 的函数,
或者指向内置的函数. 相反, ``:func:`foo.filter``` 很明确的指向 ``foo`` 模块中的
``filter`` 函数.

Normally, names in these roles are searched first without any further
qualification, then with the current module name prepended, then with the
current module and class name (if any) prepended.  If you prefix the name with a
dot, this order is reversed.  For example, in the documentation of the
:mod:`codecs` module, ``:func:`open``` always refers to the built-in function,
while ``:func:`.open``` refers to :func:`codecs.open`.

一般的, 这些名称将会先没有限制的搜索, 然后考虑当前的模块,
然后是当前的模块和类. 如果你用有一个点号的前缀, 那么顺序会反转.
举个例子, 在 :mod:`codecs` 这个模块的文档中, ``:func:`open``` 
将总是指向内置的函数, 而 ``:func:`.open``` 将指向 :func:`codecs.open`.

A similar heuristic is used to determine whether the name is an attribute of
the currently documented class.

一个类似的启发也用于决定当前的名称是否是当前编写文档类的属性.

The following roles create cross-references to C-language constructs if they
are defined in the API documentation:

下面的这些将会创建到 C 语言的交叉引用, 前提是在 API 文档中有定义:

.. describe:: c:data

   The name of a C-language variable.

   C 中变量的名称.

.. describe:: c:func

   The name of a C-language function. Should include trailing parentheses.

   C 中的函数. 需要包含后面的括号.

.. describe:: c:macro

   The name of a "simple" C macro, as defined above.

   一个简单的宏的名称, 像前面那样定义.

.. describe:: c:type

   The name of a C-language type.

   C 中类型的名称.

.. describe:: c:member

   The name of a C type member, as defined above.

   C 中类型的成员.


The following role does possibly create a cross-reference, but does not refer
to objects:

下面的会创建交叉引用, 但是不会指向对象:

.. describe:: token

   The name of a grammar token (used in the reference manual to create links
   between production displays).

   语法表示的名称 (在参考手册中创建链接).


The following role creates a cross-reference to the term in the glossary:

下面的会创建交叉引用至单词表中的一项:

.. describe:: term

   Reference to a term in the glossary.  The glossary is created using the
   ``glossary`` directive containing a definition list with terms and
   definitions.  It does not have to be in the same file as the ``term``
   markup, in fact, by default the Python docs have one global glossary
   in the ``glossary.rst`` file.

   指向单词表中的一项. 单词表在使用 ``glossary`` 指示符后会创建出来.
   不需要再每个文件中都创建一个, 事实上, Python 文档默认就有个全局
   的单词表, 就在 ``glossary.rst`` 文件中.

   If you use a term that's not explained in a glossary, you'll get a warning
   during build.

   如果你使用了 ``term`` 但是却没有在单词表中解释, 
   在建立时你将得到一个警告.

---------

The following roles don't do anything special except formatting the text
in a different style:

后面的则不会做任何特殊的处理, 除了以不同的样式格式化文本:

.. describe:: command

   The name of an OS-level command, such as ``rm``.

   系统级别的命令, 比如 ``rm``.

.. describe:: dfn

   Mark the defining instance of a term in the text.  (No index entries are
   generated.)

   标记一个术语的定义实例. (不会有索引项生成.)

.. describe:: envvar

   An environment variable.  Index entries are generated.

   一个环境变量. 将生成索引项.

.. describe:: file

   The name of a file or directory.  Within the contents, you can use curly
   braces to indicate a "variable" part, for example:

   一个文件或目录的名称. 在内容中, 你可以使用大括号表示可变的部分,
   举个例子:
   
   ::

      ... is installed in :file:`/usr/lib/python2.{x}/site-packages` ...

   In the built documentation, the ``x`` will be displayed differently to
   indicate that it is to be replaced by the Python minor version.

   在建立文档时, ``x`` 将会以不同的样子显示, 以表示区别.

.. describe:: guilabel

   Labels presented as part of an interactive user interface should be marked
   using ``guilabel``.  This includes labels from text-based interfaces such as
   those created using :mod:`curses` or other text-based libraries.  Any label
   used in the interface should be marked with this role, including button
   labels, window titles, field names, menu and menu selection names, and even
   values in selection lists.

   作为交互式用户界面的标签应该使用 ``guilabel`` 标记. 这包括基于文本的界面,
   像使用 :mod:`curses` 或其他库创建的. 任何标签都应该以此标记, 包括按钮的, 
   窗口标题的, 域的名字, 菜单和菜单选择项, 以及选择列表中的值.

.. describe:: kbd

   Mark a sequence of keystrokes.  What form the key sequence takes may depend
   on platform- or application-specific conventions.  When there are no relevant
   conventions, the names of modifier keys should be spelled out, to improve
   accessibility for new users and non-native speakers.  For example, an
   *xemacs* key sequence may be marked like ``:kbd:`C-x C-f```, but without
   reference to a specific application or platform, the same sequence should be
   marked as ``:kbd:`Control-x Control-f```.

   标记一系列的按键. 使用什么样的按键要取决于平台或程序的约定. 当没有相关的约定,
   修饰的按键应该全拼, 以提高可读性. 举个例子, 一个 *xemacs* 的按键可以标记为
   ``:kbd:`C-x C-f```, 但是如果没有其他的相关性, 就应该写为 
   ``:kbd:`Control-x Control-f```.

.. describe:: keyword

   The name of a keyword in Python.

   在 Python 中的关键词.

.. describe:: mailheader

   The name of an RFC 822-style mail header.  This markup does not imply that
   the header is being used in an email message, but can be used to refer to any
   header of the same "style".  This is also used for headers defined by the
   various MIME specifications.  The header name should be entered in the same
   way it would normally be found in practice, with the camel-casing conventions
   being preferred where there is more than one common usage. For example:
   ``:mailheader:`Content-Type```. 

   一个 RFC 822 式的邮件头名称. 这个标记没有暗指在邮件消息中被使用的头,
   但是可以指示它们的样式. 这也用于那些 MIME 的值.
   头的名称应该是以相同的方式输入, 并且应该按约定来使用.
   举个例子: ``:mailheader:`Content-Type```. (译注: 不是很明白)

.. describe:: makevar

   The name of a :command:`make` variable.

   :command:`make` 变量的名称.

.. describe:: manpage

   A reference to a Unix manual page including the section,
   e.g. ``:manpage:`ls(1)```.

   Unix 上的参考手册, 包括那一节, 比如 ``:manpage:`ls(1)```.

.. describe:: menuselection

   Menu selections should be marked using the ``menuselection`` role.  This is
   used to mark a complete sequence of menu selections, including selecting
   submenus and choosing a specific operation, or any subsequence of such a
   sequence.  The names of individual selections should be separated by
   ``-->``.

   菜单的选择应该用 ``menuselection`` 来标记. 
   这用于标记一个完整的菜单选择项, 包括选择子菜单和选择特殊的操作,
   或者是任何的一个子序列. 不同的选项之间应该以 ``-->`` 分割.

   For example, to mark the selection "Start > Programs", use this markup:

   比如, 要标记 "开始 > 程序", 那么就用:
   
   ::

      :menuselection:`开始 --> 程序`

   When including a selection that includes some trailing indicator, such as the
   ellipsis some operating systems use to indicate that the command opens a
   dialog, the indicator should be omitted from the selection name.

.. describe:: mimetype

   The name of a MIME type, or a component of a MIME type (the major or minor
   portion, taken alone).

   一个 MIME 类型的名字, 或者一个部分 (主要或最小的部分).

.. describe:: newsgroup

   The name of a Usenet newsgroup.

   一个 Usenet newsgroup 的名称.

.. describe:: option

   A command-line option of Python.  The leading hyphen(s) must be included.
   If a matching ``cmdoption`` directive exists, it is linked to.  For options
   of other programs or scripts, use simple ````code```` markup.

   一个命令行的选项. 选项前的横线必须要包括进来. 如果匹配到 ``cmdoption`` ,
   那么就会自动链接过去. 对于其他语言或脚本, 使用 ````code```` 标记.

.. describe:: program

   The name of an executable program.  This may differ from the file name for
   the executable for some platforms.  In particular, the ``.exe`` (or other)
   extension should be omitted for Windows programs.

   可执行程序的名称. 这在不同的平台上, 可能会有所不同.
   特别像在 Windows 平台, 后缀 ``.exe`` 就可以省略.

.. describe:: regexp

   A regular expression. Quotes should not be included.

   一个正则表达式. 引号不需要包括.

.. describe:: samp

   A piece of literal text, such as code.  Within the contents, you can use
   curly braces to indicate a "variable" part, as in ``:file:``.

   一个片段, 像代码之类. 在内容中, 你可以使用尖括号表示变的部分, 
   像在 ``:file:`` 中一样.

   If you don't need the "variable part" indication, use the standard
   ````code```` instead.

   如果你不需要可变的, 那么久使用 ````code```` 替代.


The following roles generate external links:

下面的用于产生外部链接:

.. describe:: pep

   A reference to a Python Enhancement Proposal.  This generates appropriate
   index entries. The text "PEP *number*\ " is generated; in the HTML output,
   this text is a hyperlink to an online copy of the specified PEP.

   指向 Python Enhancement Proposal . 这将生成一个合适的索引项.
   将会生成如 "PEP *number*\ " 的文字; 在 HTML 输出的结果中,
   这个文字会链接到特定的 PEP 上.

.. describe:: rfc

   A reference to an Internet Request for Comments.  This generates appropriate
   index entries. The text "RFC *number*\ " is generated; in the HTML output,
   this text is a hyperlink to an online copy of the specified RFC.

   指向 Internet Request for Comments . 这也产生合适的索引项.
   生成入 "RFC *number*\ " 的文字; 在 HTML 输出的结果中,
   这个文字会链接到特定的 RFC 上.


Note that there are no special roles for including hyperlinks as you can use
the standard reST markup for that purpose.

注意, 在标准的 reST 中, 是没有特殊的标记可以实现上面的功能.


.. _doc-ref-role:

Cross-linking markup 交叉链接标记
-----------------------------------

To support cross-referencing to arbitrary sections in the documentation, the
standard reST labels are "abused" a bit: Every label must precede a section
title; and every label name must be unique throughout the entire documentation
source.

为了在文档中实现任意章节的交叉引用, 在标准的 reST 中这样的标签有些 "不好" :
每个标签必须先于一个章节的标题; 每个标签的名字也必须在整篇文档中不同.

You can then reference to these sections using the ``:ref:`label-name``` role.

你可以使用 ``:ref:`label-name``` 指向这些章节.

Example:

例如:

::

   .. _my-reference-label:

   Section to cross-reference
   --------------------------

   This is the text of the section.

   It refers to the section itself, see :ref:`my-reference-label`.

The ``:ref:`` invocation is replaced with the section title.

此处的 ``:ref:`` 将被换成标题名.

Alternatively, you can reference any label (not just section titles)
if you provide the link text ``:ref:`link text <reference-label>```.

当然, 你也可以指向任意的标签 (不止是章节的题目)
如果你提供链接的文字 ``:ref:`link text <reference-label>```.

Paragraph-level markup 段落级别的标记
----------------------------------------

These directives create short paragraphs and can be used inside information
units as well as normal text:

这些指令创建短的段落并可以像正常的段落一样被包含:

.. describe:: note

   An especially important bit of information about an API that a user should be
   aware of when using whatever bit of API the note pertains to.  The content of
   the directive should be written in complete sentences and include all
   appropriate punctuation.

   当使用任何的 API 时所需要注意的问题及信息. 指令的内容应该写完整并包含合适的标点.

   Example:

   例子:
   
   ::

      .. note::

         This function is not suitable for sending spam e-mails.

.. describe:: warning

   An important bit of information about an API that a user should be aware of
   when using whatever bit of API the warning pertains to.  The content of the
   directive should be written in complete sentences and include all appropriate
   punctuation.  In the interest of not scaring users away from pages filled
   with warnings, this directive should only be chosen over ``note`` for
   information regarding the possibility of crashes, data loss, or security
   implications.

   和上面所描述的一样, 给出一些警告.
   在不引起用户恐慌的情况下使用, 给出某些可能如冲突, 数据丢失或安全问题.

.. describe:: versionadded

   This directive documents the version of Python which added the described
   feature to the library or C API. When this applies to an entire module, it
   should be placed at the top of the module section before any prose.

   这个用于描述 Python 新版本引入的新特性, 如库或 C 的 API.
   如果在一个完整的模块中使用, 请将此放到文档最前面.

   The first argument must be given and is the version in question; you can add
   a second argument consisting of a *brief* explanation of the change.

   第一个参数必须要给定, 并且是那个版本号; 你可以增加第二个参数,
   用以包含一个简明的改变.

   Example:
   
   例如::

      .. versionadded:: 3.1
         The *spam* parameter.

   Note that there must be no blank line between the directive head and the
   explanation; this is to make these blocks visually continuous in the markup.

   注意, 此处不能够有空行; 这是为了使得标记看起来连续.

.. describe:: versionchanged

   Similar to ``versionadded``, but describes when and what changed in the named
   feature in some way (new parameters, changed side effects, etc.).

   和 ``versionadded`` 类似, 但是描述了什么时候和什么东西改变了.
   比如新的参数, 更改的副作用等等.

--------------

.. describe:: impl-detail

   This directive is used to mark CPython-specific information.  Use either with
   a block content or a single sentence as an argument, i.e. either ::

   这个用于标记 CPython 特定的信息. 参数可以是一个块或者只是一句.

      .. impl-detail::

         This describes some implementation detail.

         More explanation.

   or 或者::

      .. impl-detail:: This shortly mentions an implementation detail.

   "\ **CPython implementation detail:**\ " is automatically prepended to the
   content.

   "\ **CPython implementation detail:**\ " 会自动的增加.

.. describe:: seealso

   Many sections include a list of references to module documentation or
   external documents.  These lists are created using the ``seealso`` directive.

   很多章节包含了一个列表, 放着一些参考的文档.
   它们就被放在 ``seealso`` 中.

   The ``seealso`` directive is typically placed in a section just before any
   sub-sections.  For the HTML output, it is shown boxed off from the main flow
   of the text.

   ``seealso`` 指示符一般防在一个章节到另一个字章节的前面.
   在 HTML 输出时, 它会防在一个悬浮的框中.

   The content of the ``seealso`` directive should be a reST definition list.
   Example:
   
   在 ``seealso`` 中应该是一个 reST 的定义列表. 比如::

      .. seealso::

         Module :mod:`zipfile`
            Documentation of the :mod:`zipfile` standard module.

         `GNU tar manual, Basic Tar Format <http://link>`_
            Documentation for tar archive files, including GNU tar extensions.

.. describe:: rubric

   This directive creates a paragraph heading that is not used to create a
   table of contents node.  It is currently used for the "Footnotes" caption.

   这个控制符创建一个段落头, 但是不会放到目录下.
   这个经常用于脚注的标题.

.. describe:: centered

   This directive creates a centered boldfaced paragraph.  Use it as follows:
   
   这个指示符用于创建一个居中黑体的段落. 像下面这样使用::

      .. centered::

         Paragraph contents.


Table-of-contents markup 目录标记
-----------------------------------

Since reST does not have facilities to interconnect several documents, or split
documents into multiple output files, Sphinx uses a custom directive to add
relations between the single files the documentation is made of, as well as
tables of contents.  The ``toctree`` directive is the central element.

因为 reST 并没有将多个文档连在一起的能力, 或者是将文档拆分成多个输出文件,
Sphinx 使用了自定义的指示符来讲多个文件联系起来, 
就像一个目录一样. ``toctree`` 指示符是最主要的元素.

.. describe:: toctree

   This directive inserts a "TOC tree" at the current location, using the
   individual TOCs (including "sub-TOC trees") of the files given in the
   directive body.  A numeric ``maxdepth`` option may be given to indicate the
   depth of the tree; by default, all levels are included.

   这个用于插入一个 "TOC tree" 在当前的位置, 使用独立的给定文件的 TOC 
   (包括 "sub-TOC tree") . 一个数字 ``maxdepth`` 选项可以用于指明其层数;
   默认情况下会插入所有的.

   Consider this example (taken from the library reference index):
   
   考虑下面的例子(z摘自库函数索引)::

      .. toctree::
         :maxdepth: 2

         intro
         strings
         datatypes
         numeric
         (many more files listed here)

   This accomplishes two things:

   这里完成了两件事情:

   * Tables of contents from all those files are inserted, with a maximum depth
     of two, that means one nested heading.  ``toctree`` directives in those
     files are also taken into account.

     从所有这些文件生成的目录将被插入进去, 并且其层数是 2 ,
     这意味只有一层嵌套的标题. 在这些文件中的 ``toctree`` 指示符也会被考虑进来.

   * Sphinx knows that the relative order of the files ``intro``,
     ``strings`` and so forth, and it knows that they are children of the
     shown file, the library index.  From this information it generates "next
     chapter", "previous chapter" and "parent chapter" links.

     Sphinx 知道这些文件相对的顺序, 并且它知道它们之间的关系.
     从这里, 会生成像 "next chapter" , "previous chapter" 和 "parent chapter" 
     的链接.

   In the end, all files included in the build process must occur in one
   ``toctree`` directive; Sphinx will emit a warning if it finds a file that is
   not included, because that means that this file will not be reachable through
   standard navigation.

   在最后, 所有被包含的文件都会被包含; Sphinx 在没找到文件时将会发出一个警告,
   因为这意味着这个文件无法导航.

   The special file ``contents.rst`` at the root of the source directory is the
   "root" of the TOC tree hierarchy; from it the "Contents" page is generated.

   在源目录的最顶层的特殊文件 ``contents.rst`` , 是整个层次的根;
   在这里 "Contents" 页将被生成.


Index-generating markup 索引标记
------------------------------------

Sphinx automatically creates index entries from all information units (like
functions, classes or attributes) like discussed before.

Sphinx 会自动生成索引, 从前面提及的所有的信息单元 (像函数, 类或属性) 产生.

However, there is also an explicit directive available, to make the index more
comprehensive and enable index entries in documents where information is not
mainly contained in information units, such as the language reference.

但是, 也有一个显示的指令, 来产生更易理解的索引, 并且开关在文档中的某些条目.

The directive is ``index`` and contains one or more index entries.  Each entry
consists of a type and a value, separated by a colon.

这个指令是 ``index`` 并且包含多个索引条目. 每个条目包含一个类型和值,
以冒号分割.

For example:

举个例子::

   .. index::
      single: execution; context
      module: __main__
      module: sys
      triple: module; search; path

This directive contains five entries, which will be converted to entries in the
generated index which link to the exact location of the index statement (or, in
case of offline media, the corresponding page number).

这里包含了五个条目, 这将会转换到生成的索引中, 并且链接至真正出现的位置
(或者, 在离线情况下, 将是其相应的页码).

The possible entry types are:

可能的类型有:

single
   Creates a single index entry.  Can be made a subentry by separating the
   subentry text with a semicolon (this notation is also used below to describe
   what entries are created).

   创建一个单独的索引项. 可以通用一个分号分割子项目 (这也用于下面描述的条目).

pair
   ``pair: loop; statement`` is a shortcut that creates two index entries,
   namely ``loop; statement`` and ``statement; loop``.

   ``pair: loop; statement`` 是一个快捷方式, 创建两个索引项,
   叫做 ``loop; statement`` 和 ``statement; loop``.

triple
   Likewise, ``triple: module; search; path`` is a shortcut that creates three
   index entries, which are ``module; search path``, ``search; path, module`` and
   ``path; module search``.

   同样, ``triple: module; search; path`` 是一个快捷方式, 创建三个索引项,
   ``module; search patch``, ``search; path module`` 和 ``path; module search``.

module, keyword, operator, object, exception, statement, builtin
   These all create two index entries.  For example, ``module: hashlib`` creates
   the entries ``module; hashlib`` and ``hashlib; module``.

   这些创建两个索引项. 举个例子, ``module: hashlib`` 将会创建
   ``module; hashlib`` 和 ``hashlib; module`` 两项.

For index directives containing only "single" entries, there is a shorthand
notation:

对于之包含 "single" 的条目, 可以用下面的快捷方式::

   .. index:: BNF, grammar, syntax, notation

This creates four index entries.

这将创建四项索引项.


Grammar production displays 语法显示
--------------------------------------

Special markup is available for displaying the productions of a formal grammar.
The markup is simple and does not attempt to model all aspects of BNF (or any
derived forms), but provides enough to allow context-free grammars to be
displayed in a way that causes uses of a symbol to be rendered as hyperlinks to
the definition of the symbol.  There is this directive:

现在有个特殊的标记, 可以用以显示一个正规的语法.
这个标记很简单, 但是不尝试对所有 BNF (或继承的形式) 进行模仿,
但是提供足够的东西, 允许显示自由内容的语法, 以一种方式, 
可以链接至定义的符号列表中. 有一个这样的指示符:

.. describe:: productionlist

   This directive is used to enclose a group of productions.  Each production is
   given on a single line and consists of a name, separated by a colon from the
   following definition.  If the definition spans multiple lines, each
   continuation line must begin with a colon placed at the same column as in the
   first line.

   这个指示符用于包括一组 production . 每个 production 都给定单独一行,
   并包含一个名字, 以冒号分割, 接下来则是定义. 如果定义跨越多行, 
   每个续行必须以一个与上面的冒号对齐的形式给出.

   Blank lines are not allowed within ``productionlist`` directive arguments.

   在 ``productionlist`` 中空行是不允许的.

   The definition can contain token names which are marked as interpreted text
   (e.g. ``unaryneg ::= "-" `integer```) -- this generates cross-references
   to the productions of these tokens.

   定义可以包含解释的文字, 像 ``unaryneg ::= "-" `integer``` , 
   这样就会生成交叉引用.

   Note that no further reST parsing is done in the production, so that you
   don't have to escape ``*`` or ``|`` characters.

   注意此处的没有其他会被 reST 解析了. 所有像 ``*`` 或 ``|`` 都不需要转义.


.. XXX describe optional first parameter

The following is an example taken from the Python Reference Manual:

下面是一个例子:

::

   .. productionlist::
      try_stmt: try1_stmt | try2_stmt
      try1_stmt: "try" ":" `suite`
               : ("except" [`expression` ["," `target`]] ":" `suite`)+
               : ["else" ":" `suite`]
               : ["finally" ":" `suite`]
      try2_stmt: "try" ":" `suite`
               : "finally" ":" `suite`


Substitutions 替换
-------------------

The documentation system provides three substitutions that are defined by default.
They are set in the build configuration file :file:`conf.py`.

默认的定义下, 文档系统提供了三种替换.
它们可以在配置文件 :file:`conf.py` 中设置.

.. describe:: |release|

   Replaced by the Python release the documentation refers to.  This is the full
   version string including alpha/beta/release candidate tags, e.g. ``2.5.2b3``.

   替换成此文档对应的 Python 发行版本. 这是一个完整版本的字符串, 包括 alpha/beta/release
   candidate 标记, 比如 ``2.5.2.b3``.

.. describe:: |version|

   Replaced by the Python version the documentation refers to. This consists
   only of the major and minor version parts, e.g. ``2.5``, even for version
   2.5.1.

   替换成对应的 Python 版本. 这只包含主要及次要的版本部分, 比如 ``2.5``,
   也包含了 2.5.1.

.. describe:: |today|

   Replaced by either today's date, or the date set in the build configuration
   file.  Normally has the format ``April 14, 2007``.

   替换成当前的日期, 或者是在配置文件中设置的日期. 一般的格式为 ``April 14, 2007``.


.. rubric:: Footnotes

.. [1] There is a standard ``.. include`` directive, but it raises errors if the
       file is not found.  This one only emits a warning.

       有一个标准的 ``.. include`` 指示符, 但是如果找不到文件就会产生一个错误.
       而此处的指示一些提示.

