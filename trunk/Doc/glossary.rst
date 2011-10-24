.. _glossary:

**************************
术语 
**************************

.. if you add new entries, keep the alphabetical sorting!

.. glossary::

   ``>>>``
      默认的Python交互shell的提示符。
      
    ------------------------------------------------------------------------------------------------------------------------------------------------------

   ``...``
      默认的Python的交互式shell提示符，常用于输入代码时提示代码块缩进，并配对左右两边分隔符，比如括号、方括号，和大括号。
      代码缩进的代码块或在左边的一对匹配和右分隔符（括号，方括号或卷曲大括号）。
            
    ------------------------------------------------------------------------------------------------------------------------------------------------------

   2to3
      一种尝试将Python 2.x代码转为3.x的代码的工具。将会对不兼容的部分进行关键字及语句替换。
            
    ------------------------------------------------------------------------------------------------------------------------------------------------------


   抽象基类
      :ref:`abstract-base-classes` complement :term:`duck-typing` by
      providing a way to define interfaces when other techniques like
      :func:`hasattr` would be clumsy. Python comes with many built-in ABCs for
      data structures (in the :mod:`collections` module), numbers (in the
      :mod:`numbers` module), and streams (in the :mod:`io` module). You can
      create your own ABC with the :mod:`abc` module.
            
    ------------------------------------------------------------------------------------------------------------------------------------------------------

      ABC，抽象基类，补充:*duck-typing*，若像其他技术`` hasattr（）``提供一种方式来定义接口时，将会是后一种笨拙的办法。
      Python提供了许多内置的ABC
      数据结构（在``collections``模块），数字（在``numbers``模块），流（在``io ``模块）。你您可以使用abc模块创建您自己的ABC。

   参数
      A value passed to a function or method, assigned to a named local
      variable in the function body.  A function or method may have both
      positional arguments and keyword arguments in its definition.
      Positional and keyword arguments may be variable-length: ``*`` accepts
      or passes (if in the function definition or call) several positional
      arguments in a list, while ``**`` does the same for keyword arguments
      in a dictionary.
            
    ------------------------------------------------------------------------------------------------------------------------------------------------------

      传递给一个函数或方法的值，被分配在函数体内作为局部变量。变量在函数一个函数或方法在定义时可以有位置参数和关键字参数两种形式。
      位置参数和关键字参数在它的定义。位置和关键字参数个数可以增加：``*``接收或传递（如果在函数中被定义或调用）
      列表中的几个位置参数，而``**``的作用和字典中的关键字参数一样。字典中的关键字参数。



      任何表达式都可以用在参数列表，并运算值可被传递到本地变量。

   属性
      一个使用点符号“.”分隔的关联对象名称的值。星罗棋布的表达。例如，如果一个对象*o *有一个属性*a *它表示为"o.a".

   BDFL
      终身慈善统治者（Benevolent Dictator For Life），指的是Guido van Rossum，Python的创造者。

   字节码
      Python source code is compiled into bytecode, the internal representation
      of a Python program in the CPython interpreter.  The bytecode is also
      cached in ``.pyc`` and ``.pyo`` files so that executing the same file is
      faster the second time (recompilation from source to bytecode can be
      avoided).  This "intermediate language" is said to run on a
      :term:`virtual machine` that executes the machine code corresponding to
      each bytecode. Do note that bytecodes are not expected to work between
      different Python virtual machines, nor to be stable between Python
      releases.
            
    ------------------------------------------------------------------------------------------------------------------------------------------------------

      Python源代码会被编译成字节码，是Python代码在CPython解释器中的一种“中间形式”。代表中的一个CPython的解释Python程序。该
      字节码也可被缓存在``.pyc文件``和``.pyo``文件中，以便下一次执行相同的文件时更快（可以避免重新编译
      源代码到字节码）这个“中间语言“，运行在*virtual machine*中，每一个字节码都被转化为对应的机器码（0和1）来执行。
      机器代码对应于每个字节码。请注意，字节码在不同的Python的虚拟机上无法工作，在不同Python版本中也不稳定。

      A list of bytecode instructions can be found in the documentation for
      :ref:`the dis module <bytecodes>`.

      有一个包含所有字节码指令的具体列表，在教程*the dis module* 部分可以找到。

   类
      A template for creating user-defined objects. Class definitions
      normally contain method definitions which operate on instances of the
      class.

      一个创建定义对象的模板。类的定义通常包含方法定义，用于操作类的实例类。

   强制转换
      The implicit conversion of an instance of one type to another during an
      operation which involves two arguments of the same type.  For example,
      ``int(3.15)`` converts the floating point number to the integer ``3``, but
      in ``3+4.5``, each argument is of a different type (one int, one float),
      and both must be converted to the same type before they can be added or it
      will raise a ``TypeError``.  Without coercion, all arguments of even
      compatible types would have to be normalized to the same value by the
      programmer, e.g., ``float(3)+4.5`` rather than just ``3+4.5``.

      一个类型的实例的一个隐式转换到另一个操作过程中涉及两个相同类型的参数。例如，``int（3.15）``转换为浮点数``3 `` 的整数，
      但在 ``3 4.5 ``，每个参数一个是不同类型（一个int，一个浮点），都必须转换为同一类型才可以添加或会提出`` TypeError异常`` 。
      没有强制转换，所有的参数甚至兼容类型都必须由归为相同的值程序员，例如，``浮（3）4.5，而不是仅仅``3 +4.5 `` `` 。

   复数
      An extension of the familiar real number system in which all numbers are
      expressed as a sum of a real part and an imaginary part.  Imaginary
      numbers are real multiples of the imaginary unit (the square root of
      ``-1``), often written ``i`` in mathematics or ``j`` in
      engineering.  Python has built-in support for complex numbers, which are
      written with this latter notation; the imaginary part is written with a
      ``j`` suffix, e.g., ``3+1j``.  To get access to complex equivalents of the
      :mod:`math` module, use :mod:`cmath`.  Use of complex numbers is a fairly
      advanced mathematical feature.  If you're not aware of a need for them,
      it's almost certain you can safely ignore them.

      一个我们常见的整数的拓展形式，复数表示为一个实部和虚总两部分。段落虚数是虚单位的实际倍数
      （``-1`` 的平方根），通常在数学中写为``i`` 或在工程中写为 ``j ``。Python内置支持复数，
      用的符号是“j”。虚数部分是一个``j ``后缀，例如，3 +1 Ĵ `` ``。要使用
      ``math ``模块中的复数形式，请使用``cmath ``。是复数的应用具有相当高级的数学特性。
      if () {}你没勇气使用它们，最安全的方式就是无视它。安全地忽略它们。

   context manager 上下文管理器
      An object which controls the environment seen in a :keyword:`with`
      statement by defining :meth:`__enter__` and :meth:`__exit__` methods.
      See :pep:`343`.

      一个定义在with语句中用于控制编程语言环境的对象，声明定义在`` __enter__（）``和`` __exit__（）``方法中。见 **Python优化建议 343 **

   CPython
      Python编程语言官方C语言版本的实现，发布在python.org。使用“CPython”，是为了区分其他形式的实现版本，比如Jython或IronPython的。

   装饰器
      A function returning another function, usually applied as a function
      transformation using the ``@wrapper`` syntax.  Common examples for
      decorators are :func:`classmethod` and :func:`staticmethod`.

      一个函数返回另一个函数，通常用"@wrapper"语法来转化函数。职能转变使用`` `` @包装语法。一般用于装饰的例子是`` classmethod（）``和`` staticmethod ()`.

      The decorator syntax is merely syntactic sugar, the following two
      function definitions are semantically equivalent::

      装饰器的语法只是语法糖（为了书写方便，理解方便的可选编码方式），以下两个函数的定义方式与装饰器有相同效果。函数定义在语义上是等价的::

         def f(...):
             ...
         f = staticmethod(f)

         @staticmethod
         def f(...):
             ...

      The same concept exists for classes, but is less commonly used there.  See
      the documentation for :ref:`function definitions <function>` and
      :ref:`class definitions <class>` for more about decorators.

      类也有类似的概念，但不常用。。请参阅 *function definitions*和*class definitions* 获得更多信息。定义*有关装饰更多。

   描述器
      Any object which defines the methods :meth:`__get__`, :meth:`__set__`, or
      :meth:`__delete__`.  When a class attribute is a descriptor, its special
      binding behavior is triggered upon attribute lookup.  Normally, using
      *a.b* to get, set or delete an attribute looks up the object named *b* in
      the class dictionary for *a*, but if *b* is a descriptor, the respective
      descriptor method gets called.  Understanding descriptors is a key to a
      deep understanding of Python because they are the basis for many features
      including functions, methods, properties, class methods, static methods,
      and reference to super classes.

      任何对象，它定义的方法`` get()``,`` __set__ ()``, __get__ ()``,或`` __delete__ ()``.当一个类属性是一个描述符，
      其特别是具有约束力的行为时触发属性查找。
      通常情况下，使用*从头*获取，设置或删除属性查找名为类* b *值的对象字典*一*，但如果是* b *值一个描述符，分别描述方法被调用。
      了解描述符是一个深刻的认识的关键Python的，因为它们是许多特点的基础上，包括函数，方法，属性，类方法，静态方法，引用超类。

      For more information about descriptors' methods, see :ref:`descriptors`.

   字典
      An associative array, where arbitrary keys are mapped to values.  The keys
      can be any object with :meth:`__hash__` function and :meth:`__eq__`
      methods. Called a hash in Perl.

      一个关联数组，一个键都映射一个值。这些键可以是任何拥有`` __hash__（）函数和''__eq__（）''方法的对象`，``` __eq__（）``方法。在Perl中被称为hash。

   文档字符串
      A string literal which appears as the first expression in a class,
      function or module.  While ignored when the suite is executed, it is
      recognized by the compiler and put into the :attr:`__doc__` attribute
      of the enclosing class, function or module.  Since it is available via
      introspection, it is the canonical place for documentation of the
      object.

      它是以一个字符串的形式出现在类、函数或模块的第一个表达式，函数或模块。虽然运行时，解释器将忽略它，但仍
      将被编译器提取到类、函数或模块的``__doc__ ``属性中。封闭的阶级属性，函数或模块。虽然在代码中可见，但在对象问的那文档的对象。

   duck-typing
      A programming style which does not look at an object's type to determine
      if it has the right interface; instead, the method or attribute is simply
      called or used ("If it looks like a duck and quacks like a duck, it
      must be a duck.")  By emphasizing interfaces rather than specific types,
      well-designed code improves its flexibility by allowing polymorphic
      substitution.  Duck-typing avoids tests using :func:`type` or
      :func:`isinstance`.  (Note, however, that duck-typing can be complemented
      with :term:`abstract base class`\ es.)  Instead, it typically employs
      :func:`hasattr` tests or :term:`EAFP` programming.

      一种编程风格，不看对象的类型
      确定它是否有正确的接口，相反，该方法或
      属性是简单地调用或使用（“如果它看起来像一只鸭子
      叫声像鸭子，它必须是一个鸭子。“）通过强调接口
      而不是具体的类型，精心设计的代码提高其
      灵活性，允许多态取代。Duck typing
      避免使用``type（）``或`` isinstance ()``.（请注意，但是，
      duck-typing 可以辅之以*抽象基类 * 537。）
      相反，它通常采用`` hasattr（）``测试或* EAFP *
      用途安排

   EAFP
      Easier to ask for forgiveness than permission.  This common Python coding
      style assumes the existence of valid keys or attributes and catches
      exceptions if the assumption proves false.  This clean and fast style is
      characterized by the presence of many :keyword:`try` and :keyword:`except`
      statements.  The technique contrasts with the :term:`LBYL` style
      common to many other languages such as C.

      更容易要求比许可宽恕。
      这种常见的Python
      编码风格担负着有效的密钥或属性和存在
      捕捉异常，如果虚假证明的假设。这次清理和
      快速风格的特点是存在很多``try ``和
      `` except``。该技术对比*与* LBYL
      常见的风格如C许多其他语言.

   表达式
      A piece of syntax which can be evaluated to some value.  In other words,
      an expression is an accumulation of expression elements like literals,
      names, attribute access, operators or function calls which all return a
      value.  In contrast to many other languages, not all language constructs
      are expressions.  There are also :term:`statement`\s which cannot be used
      as expressions, such as :keyword:`if`.  Assignments are also statements,
      not expressions.

      一种可以通过计算获得某个值的语法片段。在其他
      也就是说，一个表情是表达元素积累喜欢
      文字，名称，属性访问，经营者或函数调用
      所有返回值。相反，许多其他语言，而不是
      所有的语言结构是表达式。也有
      *声明* s不能作为表达式中使用，例如，如果`` ``。
      作业也声明，不是表达式。

   扩展模块
      用C或C + +编写的Python模块，使用Python的C API与内核和用户写的代码进行交互。与用户代码的核心和。

   文件对象
      An object exposing a file-oriented API (with methods such as
      :meth:`read()` or :meth:`write()`) to an underlying resource.  Depending
      on the way it was created, a file object can mediate access to a real
      on-disk file or to another other type of storage or communication device
      (for example standard input/output, in-memory buffers, sockets, pipes,
      etc.).  File objects are also called :dfn:`file-like objects` or
      :dfn:`streams`.

      暴露的对象与方法一面向文件的API（如
      ``阅读（）``或``写()``)到基础资源。根据
      它的方式是创建一个文件对象可以进入到一个真正的调解
      磁盘上的文件或其他储存或其他类型的通信
      设备（例如标准输入/输出，内存中的缓冲区，
      插座，管道等）。文件对象也称为*类文件
      流对象*或* *.

      There are actually three categories of file objects: raw binary files,
      buffered binary files and text files.  Their interfaces are defined in the
      :mod:`io` module.  The canonical way to create a file object is by using
      the :func:`open` function.

      实际上有三种类型的文件对象：原始的二进制
      文件，缓存的二进制文件和文本文件。它们的接口
      定义在`` IO``模块中。创建一个文件规范的方式是
      使用``open（）``函数。

   file-like object
      A synonym for :term:`file object`.

   finder
      An object that tries to find the :term:`loader` for a module. It must
      implement a method named :meth:`find_module`. See :pep:`302` for
      details and :class:`importlib.abc.Finder` for an
      :term:`abstract base class`.

      一个对象，试图寻找*loader*模块。它必须
      拥有名为`` find_module方法()``.** Python优化建议302 **可看到更多
      细节和它的抽象基类importlib.abc.Finder“。发现者`` *.一个抽象基类*

   floor division
      Mathematical division that rounds down to nearest integer.  The floor
      division operator is ``//``.  For example, the expression ``11 // 4``
      evaluates to ``2`` in contrast to the ``2.75`` returned by float true
      division.  Note that ``(-11) // 4`` is ``-3`` because that is ``-2.75``
      rounded *downward*. See :pep:`238`.

      数学除法是到最接近的整数轮。该
      地板除法运算符是``//``.例如，表达式
      `` 11 / / 4的计算结果与此相反的`` `` 2 `` ``的`` 2.75返回
      漂浮的真正分裂。请注意，``（-11）/ / 4是`` `` ``因为-3
      这是圆的`` `` -2.75向下*. *见义Python优化建议 238 **. **


   函数
      A series of statements which returns some value to a caller. It can also
      be passed zero or more arguments which may be used in the execution of
      the body. See also :term:`argument` and :term:`method`.

      一系列的语句，它返回某个值给调用者。它可以
      通过零个或多个参数
      执行函数体。另见*参数*和*方法*.

   __future__
      A pseudo-module which programmers can use to enable new language features
      which are not compatible with the current interpreter.

      一个伪模块，程序员可以启用新的语言特性，与目前的解释器不兼容。

      By importing the :mod:`__future__` module and evaluating its variables,
      you can see when a new feature was first added to the language and when it
      becomes the default::

      通过导入`` __future__``模块和运算其
      变量，你可以看到一个新特点是何时首次加入
      编程语言，当成为默认::

         >>> import __future__
         >>> __future__.division
         _Feature((2, 2, 0, 'alpha', 2), (3, 0, 0, 'alpha', 0), 8192)

   垃圾收集
      The process of freeing memory when it is not used anymore.  Python
      performs garbage collection via reference counting and a cyclic garbage
      collector that is able to detect and break reference cycles.

      当进程在内存不再被使用，它将被释放。Python
      通过一个循环执行引用计数和垃圾回收的
      垃圾收集器，能够检测并跳出引用
      周期。

      .. index:: single: generator

   生成器
      A function which returns an iterator.  It looks like a normal function
      except that it contains :keyword:`yield` statements for producing a series
      a values usable in a for-loop or that can be retrieved one at a time with
      the :func:`next` function. Each :keyword:`yield` temporarily suspends
      processing, remembering the location execution state (including local
      variables and pending try-statements).  When the generator resumes, it
      picks-up where it left-off (in contrast to functions which start fresh on
      every invocation.

      一个函数，它返回一个迭代器。它看起来像一个正常的
      函数，只是它包含`` ``生产产量报表
      一个可用的一系列价值观念在一个环或任何可获取一
      在与``下（）``功能的时间。每个`` ``暂时屈服
      暂停处理，想起了位置的执行状态
      （包括局部变量和等待试语句）。当
      发电机恢复时，夹带，它离开的地方起飞（相对于
      职能每次调用新的开始。

      .. index:: single: generator expression

   生成器表达式
      An expression that returns an iterator.  It looks like a normal expression
      followed by a :keyword:`for` expression defining a loop variable, range,
      and an optional :keyword:`if` expression.  The combined expression
      generates values for an enclosing function::


      该表达式返回一个迭代器。它看起来像一个正常的
      其次表现为一个循环定义``表达了一个``
      变量，范围，以及可选的表达式，如果`` ``。合并
      表达式生成一个封闭的函数值::

         >>> sum(i*i for i in range(10))         # sum of squares 0, 1, 4, ... 81
         285

   GIL 全局解释器锁
      The mechanism used by the :term:`CPython` interpreter to assure that
      only one thread executes Python :term:`bytecode` at a time.
      This simplifies the CPython implementation by making the object model
      (including critical built-in types such as :class:`dict`) implicitly
      safe against concurrent access.  Locking the entire interpreter
      makes it easier for the interpreter to be multi-threaded, at the
      expense of much of the parallelism afforded by multi-processor
      machines.

      由* CPython *的解释所使用的机制，以保证同一时间只有
      一个线程执行一次Python*字节码*。这简化了
      通过使对象模型（包括CPython的实施
      关键的内置类型，如``dict ``）隐式安全反对
      并发访问。锁定整个解释更容易
      该解释器是多线程，在大部分费用
      通过给予的并行多处理器的机器。

      However, some extension modules, either standard or third-party,
      are designed so as to release the GIL when doing computationally-intensive
      tasks such as compression or hashing.  Also, the GIL is always released
      when doing I/O.

      然而，一些扩展模块，标准或第三方库
      都被设计用以释放GIL，
      比如压缩或散列密集型的运算。此外，GIL
      总是在I/O运算时被自动释放。

      Past efforts to create a "free-threaded" interpreter (one which locks
      shared data at a much finer granularity) have not been successful
      because performance suffered in the common single-processor case. It
      is believed that overcoming this performance issue would make the
      implementation much more complicated and therefore costlier to maintain.

      曾有人努力去创造一个“自由线程的”解释器（
      在一个更精细的粒度的锁上共享数据）但尚未
      成功，因为这是单核
      处理器的通病。据认为，创造这个细粒度解释器
      将让问题更加复杂，
      并且难以维护。

   可哈希的
      An object is *hashable* if it has a hash value which never changes during
      its lifetime (it needs a :meth:`__hash__` method), and can be compared to
      other objects (it needs an :meth:`__eq__` method).  Hashable objects which
      compare equal must have the same hash value.

      一个对象如果是*可哈希化的*，它有一个哈希值，在其生命周期中永远不会改变
      （它需要一个`` __hash__（）``方法），并且可以
      和其他对象进行比较（它需要一个`` __eq__（）``方法）。
      相等的哈希对象必定会具有相同的哈希值。

      Hashability makes an object usable as a dictionary key and a set member,
      because these data structures use the hash value internally.

      Hashability使一个对象可用作一个字典的键和一组成员，因为这些数据结构内部使用哈希值。

      All of Python's immutable built-in objects are hashable, while no mutable
      containers (such as lists or dictionaries) are.  Objects which are
      instances of user-defined classes are hashable by default; they all
      compare unequal, and their hash value is their :func:`id`.

      所有Python的可执行的内置对象均可进行哈希运算，但不包括可变容器（如列表或字典）。
      对象这些由用户定义的类的实例执行默认哈希运算时一般都无固定值，其哈希值是他们的"id（）"。

   IDLE
      An Integrated Development Environment for Python.  IDLE is a basic editor
      and interpreter environment which ships with the standard distribution of
      Python.

      一个Python集成开发环境。IDLE也是一个基本的编辑器和解释器环境，和Python标准发布版本一起发布。Python的分布。

   不可变
      An object with a fixed value.  Immutable objects include numbers, strings and
      tuples.  Such an object cannot be altered.  A new object has to
      be created if a different value has to be stored.  They play an important
      role in places where a constant hash value is needed, for example as a key
      in a dictionary.

      具有固定的值对象。不可变对象包括数字，
      字符串和元组。这样的对象不能被改变。阿新
      如果要存储一个不同的值，则必须创建一个新的对象。
      他们发挥了重要作用表现在保持一个固定的哈希值
      ，比如字典里面的键。

   importer
      An object that both finds and loads a module; both a
      :term:`finder` and :term:`loader` object.

      一个对象，同时查找并加载一个模块，既是一个*finder*也是一个*loader*。

   交互式
      Python has an interactive interpreter which means you can enter
      statements and expressions at the interpreter prompt, immediately
      execute them and see their results.  Just launch ``python`` with no
      arguments (possibly by selecting it from your computer's main
      menu). It is a very powerful way to test out new ideas or inspect
      modules and packages (remember ``help(x)``).

      Python有一个对互动的解释，这意味着你可以在解释器中随时输入语句和表达式，
      并可立即执行它们，查看它们的结果。只需运行Python且无需参数
      可能通过选择从参数（您的电脑的主
      選單这是一个非常强大的测试新想法或检查
      模块和包的方式（记住``help（）``).

   解释性
      Python is an interpreted language, as opposed to a compiled one,
      though the distinction can be blurry because of the presence of the
      bytecode compiler.  This means that source files can be run directly
      without explicitly creating an executable which is then run.
      Interpreted languages typically have a shorter development/debug cycle
      than compiled ones, though their programs generally also run more
      slowly.  See also :term:`interactive`.

      Python是一种解释语言，而不是一个编译的，
      虽然两者的区别因为
      字节码编译器而变得模糊。这意味着，
      在没有创建一个可执行的程序情况下，源文件可以直接运行，
      運行中...解释语言通常更简练，
      开发/调试周期比编译的要短，当然了他们的程序
      一般也运行更慢。另见*interactive*.

   可迭代
      An object capable of returning its members one at a
      time. Examples of iterables include all sequence types (such as
      :class:`list`, :class:`str`, and :class:`tuple`) and some non-sequence
      types like :class:`dict` and :class:`file` and objects of any classes you
      define with an :meth:`__iter__` or :meth:`__getitem__` method.  Iterables
      can be used in a :keyword:`for` loop and in many other places where a
      sequence is needed (:func:`zip`, :func:`map`, ...).  When an iterable
      object is passed as an argument to the built-in function :func:`iter`, it
      returns an iterator for the object.  This iterator is good for one pass
      over the set of values.  When using iterables, it is usually not necessary
      to call :func:`iter` or deal with iterator objects yourself.  The ``for``
      statement does that automatically for you, creating a temporary unnamed
      variable to hold the iterator for the duration of the loop.  See also
      :term:`iterator`, :term:`sequence`, and :term:`generator`.

      每次返回某个对象的一个成员的能力。示例
      包括所有的iterables序列类型（`` ``如乙方`` ``名单，
      元组和`` ``）快译通`` ``像一些非序列类型和
      `` ``文件，以及任何你定义类的对象与
      `` __iter__（）``或``的__getitem__（）``方法。Iterables可以使用
      在一个循环`` ``和许多其他地方，一个序列
      需要（邮编()``, `` ``地图()``, ...).当一个可迭代对象
      作为参数传递给参数内置的功能它``国际热核实验堆()``,
      返回一个迭代器对象。这是一个很好的迭代器
      通过对一组值。当使用iterables，它通常是
      没有必要调用``国际热核实验堆（）``和迭代器对象或处理
      自己。声明为`` ``这是否会自动为你，
      创建临时无名变量来保存的迭代器
      时间循环。另见迭代* * * *序列，并生成器
      
   iterator
      An object representing a stream of data.  Repeated calls to the iterator's
      :meth:`__next__` method (or passing it to the built-in function
      :func:`next`) return successive items in the stream.  When no more data
      are available a :exc:`StopIteration` exception is raised instead.  At this
      point, the iterator object is exhausted and any further calls to its
      :meth:`__next__` method just raise :exc:`StopIteration` again.  Iterators
      are required to have an :meth:`__iter__` method that returns the iterator
      object itself so every iterator is also iterable and may be used in most
      places where other iterables are accepted.  One notable exception is code
      which attempts multiple iteration passes.  A container object (such as a
      :class:`list`) produces a fresh new iterator each time you pass it to the
      :func:`iter` function or use it in a :keyword:`for` loop.  Attempting this
      with an iterator will just return the same exhausted iterator object used
      in the previous iteration pass, making it appear like an empty container.

      一个对象，表示一个数据流。重复调用
      迭代器的`` __next__（）``方法（或传递到内置的
      功能``下次()``)返回流中的连续项。当
      没有更多数据可用一个`` `` StopIteration异常引发异常
      改為在这一点上，迭代器对象是筋疲力尽，任何
      还呼吁其`` __next__（）``方法只是提高
      `` `` StopIteration异常了。迭代器是必须有一
      `` __iter__（）``方法返回的迭代器对象本身，所以
      每一个迭代器也可迭代，可在大多数地方使用
      在其他iterables被接受。一个值得注意的例外是代码
      它试图通过多次迭代。一个容器对象（例如
      作为一个`` ``名单）产生一个全新的每次迭代器传给
      国际热核实验堆的``（）函数或使用`` ``的``在一个循环中。尝试
      与这只是一个迭代器返回的迭代器相同的疲惫
      在前面的迭代通过使用对象，使得它看上去像
      一个空的容器。

      More information can be found in :ref:`typeiter`.

   关键字
      A key function or collation function is a callable that returns a value
      used for sorting or ordering.  For example, :func:`locale.strxfrm` is
      used to produce a sort key that is aware of locale specific sort
      conventions.

	一个关键字函数或位置函数是一个可调用的返回
	值用于排序或订购。例如，
	`` locale.strxfrm（）``是用来产生一个排序的关键是知道
	区域设置特定的排序约定。

      A number of tools in Python accept key functions to control how elements
      are ordered or grouped.  They include :func:`min`, :func:`max`,
      :func:`sorted`, :meth:`list.sort`, :func:`heapq.nsmallest`,
      :func:`heapq.nlargest`, and :func:`itertools.groupby`.

	在Python的工具号码接受键功能来控制如何元素是有序或分组。它们包括``min()``,
	`` max()``,sorted()``, ``list.sort `` ,''heapq.nsmallest ()``, 
	`` heapq.nlargest ()``,和`` itertools.groupby ()``.

      There are several ways to create a key function.  For example. the
      :meth:`str.lower` method can serve as a key function for case insensitive
      sorts.  Alternatively, an ad-hoc key function can be built from a
      :keyword:`lambda` expression such as ``lambda r: (r[0], r[2])``.  Also,
      the :mod:`operator` module provides three key function constuctors:
      :func:`~operator.attrgetter`, :func:`~operator.itemgetter`, and
      :func:`~operator.methodcaller`.  See the :ref:`Sorting HOW TO
      <sortinghowto>` for examples of how to create and use key functions.

	有几种方法可以创建一个关键作用。例如。的
	`` str.lower（）``方法可以作为本案的关键功能
	不敏感的排序。另外，一个特设的关键功能，可
	从`` ``拉姆达表达式建成等``拉姆达记：相关（r [0]
	R.3此外，经营者`` ``提供三个主要功能模块
	constuctors：`` `` itemgetter ()``, attrgetter ()``,和
	`` methodcaller ()``.请参阅*排序*为示例如何如何创建和使用的关键功能。

   关键字参数
      Arguments which are preceded with a ``variable_name=`` in the call.
      The variable name designates the local name in the function to which the
      value is assigned.  ``**`` is used to accept or pass a dictionary of
      keyword arguments.  See :term:`argument`.

	在调用之前通过 ``variable_name=``确定参数值。
	变量和函数中命名相同的变量名的值配对，
	``**``用于接受或传递
	数据结构为字典的关键字参数。见*argument*.

   lambda
      An anonymous inline function consisting of a single :term:`expression`
      which is evaluated when the function is called.  The syntax to create
      a lambda function is ``lambda [arguments]: expression``

      一个匿名的内联函数，由单个在函数被调用时执行运算的*表达式*组成，这是评价函数时调用。的语法创建一个lambda函数的语法是``lambda[参数]：表达式``

   LBYL
      Look before you leap.  This coding style explicitly tests for
      pre-conditions before making calls or lookups.  This style contrasts with
      the :term:`EAFP` approach and is characterized by the presence of many
      :keyword:`if` statements.

      In a multi-threaded environment, the LBYL approach can risk introducing a
      race condition between "the looking" and "the leaping".  For example, the
      code, ``if key in mapping: return mapping[key]`` can fail if another
      thread removes *key* from *mapping* after the test, but before the lookup.
      This issue can be solved with locks or by using the EAFP approach.

   列表
      A built-in Python :term:`sequence`.  Despite its name it is more akin
      to an array in other languages than to a linked list since access to
      elements are O(1).

	一个内置的Python*序列 *。尽管它的名字它更像是其他编程语言中的数组。
	一比一，因为访问链表数组在其他语言元素是O（1）。

   列表推导式
      A compact way to process all or part of the elements in a sequence and
      return a list with the results.  ``result = ['{:#04x}'.format(x) for x in
      range(256) if x % 2 == 0]`` generates a list of strings containing
      even hex numbers (0x..) in the range from 0 to 255. The :keyword:`if`
      clause is optional.  If omitted, all elements in ``range(256)`` are
      processed.

	一个紧凑的方式来处理序列中的所有或部分的元素
	并返回一个结果列表。 ``结果=
	['{:# 04X转换至用户}'。格式的范围X（十）（256）如果x％2 == 0] ``生成
	一种含有甚至进制数字的字符串列表（0x.。）范围
	从0到255。如果``的``子句是可选的。如果省略，所有
	``范围中的元素（256）``进行处理。

   装载器
      An object that loads a module. It must define a method named
      :meth:`load_module`. A loader is typically returned by a
      :term:`finder`. See :pep:`302` for details and
      :class:`importlib.abc.Loader` for an :term:`abstract base class`.

	用来加载其他模块的对象。它必须定义一个方法，名为
	`` load_module ()``.装载器通常返回一个*finder*.
	见 ** Python优化建议 302**的详细信息和``importlib.abc.Loader`` for an *abstract base class*.为装载机``
	*抽象基类*.

   映射
      A container object that supports arbitrary key lookups and implements the
      methods specified in the :class:`Mapping` or :class:`MutableMapping`
      :ref:`abstract base classes <abstract-base-classes>`. Examples include
      :class:`dict`, :class:`collections.defaultdict`,
      :class:`collections.OrderedDict` and :class:`collections.Counter`.

   元类
      The class of a class.  Class definitions create a class name, a class
      dictionary, and a list of base classes.  The metaclass is responsible for
      taking those three arguments and creating the class.  Most object oriented
      programming languages provide a default implementation.  What makes Python
      special is that it is possible to create custom metaclasses.  Most users
      never need this tool, but when the need arises, metaclasses can provide
      powerful, elegant solutions.  They have been used for logging attribute
      access, adding thread-safety, tracking object creation, implementing
      singletons, and many other tasks.

	类的类。这个类定义会创建一个类名，一个类的字典，以及基类的列表。
	元类是负责用这三个参数创建“其他类”原型的类。
	<path_two>\classes大多数面向对象编程语言都提供了
	某个默认的实现方式。Python的特别之处在于，它
	可以创建自定义元类。大多数用户不会需要这
	工具，但在有需要时，元类可以提供强大的，
	优雅的解决方案。它们被用于记录属性
	访问，增加线程安全性，跟踪对象的创建过程，
	实施单步运算，以及许多其他任务。


      More information can be found in :ref:`metaclasses`.

   方法
      A function which is defined inside a class body.  If called as an attribute
      of an instance of that class, the method will get the instance object as
      its first :term:`argument` (which is usually called ``self``).
      See :term:`function` and :term:`nested scope`.

      是类体里面定义的函数。如果被一个该类的实例属性调用，该方法将得到实例对象作为其第一个*参数*（通常名称为`` self``）。见*函数*和*嵌套范围*.

   MRO method resolution order
      Method Resolution Order is the order in which base classes are searched
      for a member during lookup. See `The Python 2.3 Method Resolution Order
      <http://www.python.org/download/releases/2.3/mro/>`_.

      方法解析顺序是其基类在搜索过程中的得到的执行顺序号。请查询Python 2.3方法---解析的顺序。

   易变的
      Mutable objects can change their value but keep their :func:`id`.  See
      also :term:`immutable`.

      可变对象可以改变他们的价值，但保留其``id()``（对象在内存中的唯一标识，类似门牌号或身份证）另见*不变*.

   named tuple
      Any tuple-like class whose indexable elements are also accessible using
      named attributes (for example, :func:`time.localtime` returns a
      tuple-like object where the *year* is accessible either with an
      index such as ``t[0]`` or with a named attribute like ``t.tm_year``).

	任何元组类类，它的可转位元素也访问
	使用命名属性（例如，`` time.localtime（）``返回
	元组类对象，其中的*年*为便于利用的一个非此即彼
	如``吨指数与命名属性，如[0] ``或`` `` t.tm_year）。

      A named tuple can be a built-in type such as :class:`time.struct_time`,
      or it can be created with a regular class definition.  A full featured
      named tuple can also be created with the factory function
      :func:`collections.namedtuple`.  The latter approach automatically
      provides extra features such as a self-documenting representation like
      ``Employee(name='jones', title='programmer')``.

	一个名为元组可以是一个内置类型，例如`` `` time.struct_time，
	或者它可以创建一个普通的类的定义。阿满精选命名为元组也可以创建功能与工厂`` collections.namedtuple ()``.后一种方法会自动
	提供额外的功能，如自我陈述记录喜欢``员工（姓名='琼斯，标题='程序员')``.

   命名空间
      The place where a variable is stored.  Namespaces are implemented as
      dictionaries.  There are the local, global and built-in namespaces as well
      as nested namespaces in objects (in methods).  Namespaces support
      modularity by preventing naming conflicts.  For instance, the functions
      :func:`builtins.open` and :func:`os.open` are distinguished by their
      namespaces.  Namespaces also aid readability and maintainability by making
      it clear which module implements a function.  For instance, writing
      :func:`random.seed` or :func:`itertools.izip` makes it clear that those
      functions are implemented by the :mod:`random` and :mod:`itertools`
      modules, respectively.
	
	一个存放变量的地方。命名空间是通过字典方式实现的。作为字典。还有一些局部，全局和内置
	命名空间以及对象中嵌套的命名空间（在方法中）。命名空间，支持模块化，防止了命名冲突。对于
	例如，函数`` builtins.open（）``和`` os.open（）``是区别在于它们的命名空间。命名空间也有助于可读性
	和可维护性，明确由哪个模块实现了一个功能例如，写`` random.seed（）``或
	`` itertools.izip（）``清楚地表明，这些职能随机实施的`` `` ``和`` itertools模块，分别。


   嵌套范围
      The ability to refer to a variable in an enclosing definition.  For
      instance, a function defined inside another function can refer to
      variables in the outer function.  Note that nested scopes by default work
      only for reference and not for assignment.  Local variables both read and
      write in the innermost scope.  Likewise, global variables read and write
      to the global namespace.  The :keyword:`nonlocal` allows writing to outer
      scopes.
	
	嵌套的范围取决于一个变量在一个封闭定义的范围。
	例如，在一个函数中定义的函数可以引用
	外部函数的变量。请注意，嵌套作用域
	默认仅限引用，而不会被分配空间。本地
	局部变量的读写在最内层的范围内。同理，
	全局变量会在全局命名空间读取和写入。该
	`` nonlocal``可以书面形式向非局部范围外。


   new-style 类
      Old name for the flavor of classes now used for all class objects.  In
      earlier Python versions, only new-style classes could use Python's newer,
      versatile features like :attr:`__slots__`, descriptors, properties,
      :meth:`__getattribute__`, class methods, and static methods.

	针对类的旧风格的称呼方式，现在可用于所有的类。
	在早期的Python版本中，只有新型类可以使用
	Python的各种新特性，比如`` __slots__``，描述，特点，
	`` __getattribute__ ()``,类的方法，和静态方法。


   对象
      Any data with state (attributes or value) and defined behavior
      (methods).  Also the ultimate base class of any :term:`new-style
      class`.

	任何有（属性或值）和定义的行为方法包括所有类的最终父类也是对象。

   位置参数
      The arguments assigned to local names inside a function or method,
      determined by the order in which they were given in the call.  ``*`` is
      used to either accept multiple positional arguments (when in the
      definition), or pass several arguments as a list to a function.  See
      :term:`argument`.

      函数或方法内根据其所在位置而被分配相应值的参数。在确定他们是在给定的顺序调用。``*``用于接受多个位置参数（已定义），或绕过普通参数而只接收列表功能见*argument*.


   Python 3000
      Nickname for the Python 3.x release line (coined long ago when the release
      of version 3 was something in the distant future.)  This is also
      abbreviated "Py3k".

      Python 3.x版本昵称，在版本3远未出炉的情况下就已经这么称呼了。也简称“Py3k”。

   Pythonic
      An idea or piece of code which closely follows the most common idioms
      of the Python language, rather than implementing code using concepts
      common to other languages.  For example, a common idiom in Python is
      to loop over all elements of an iterable using a :keyword:`for`
      statement.  Many other languages don't have this type of construct, so
      people unfamiliar with Python sometimes use a numerical counter instead::

          for i in range(len(food)):
              print(food[i])

      As opposed to the cleaner, Pythonic method::

         for piece in food:
             print(piece)

   引用计数
      The number of references to an object.  When the reference count of an
      object drops to zero, it is deallocated.  Reference counting is
      generally not visible to Python code, but it is a key element of the
      :term:`CPython` implementation.  The :mod:`sys` module defines a
      :func:`~sys.getrefcount` function that programmers can call to return the
      reference count for a particular object.

      对一个对象的引用。当引用计数一个对象下降到零，它被释放。
      引用计数是通常不可见的Python代码，但它是一个关键元素
	*在* CPython的执行情况。``的``系统模块定义一
	`` getrefcount（）``函数，程序员可以调用返回引用计数为特定对象。

   __slots__
      A declaration inside a class that saves memory by pre-declaring space for
      instance attributes and eliminating instance dictionaries.  Though
      popular, the technique is somewhat tricky to get right and is best
      reserved for rare cases where there are large numbers of instances in a
      memory-critical application.

   序列
      An :term:`iterable` which supports efficient element access using integer
      indices via the :meth:`__getitem__` special method and defines a
      :meth:`len` method that returns the length of the sequence.
      Some built-in sequence types are :class:`list`, :class:`str`,
      :class:`tuple`, and :class:`bytes`. Note that :class:`dict` also
      supports :meth:`__getitem__` and :meth:`__len__`, but is considered a
      mapping rather than a sequence because the lookups use arbitrary
      :term:`immutable` keys rather than integers.

	一个可迭代的元素的访问，支持高效使用整数通过``的__getitem__（）指数``特殊的方法，并确定了
	`` len（）``方法，它返回序列的长度。有些内置的序列类型`` ``list，`` ``str，`` ``tuple，
	" bytes"。请注意，也支持``dict `` 的__getitem__（）``和`` __len__ ()``方法,但其被视为一个映射，而不是一个序列因为查找使用任意keys，而不是整数。

   切片
      An object usually containing a portion of a :term:`sequence`.  A slice is
      created using the subscript notation, ``[]`` with colons between numbers
      when several are given, such as in ``variable_name[1:3:5]``.  The bracket
      (subscript) notation uses :class:`slice` objects internally.

	一个对象，通常包含了一个序列的一部分*. *切片使用下标符号来创建，与冒号之间``[]``
	给出了几个数字时，如``变量名[1时03分○五秒] ``。托架（下标）符号使用`` ``对象内部片。

   special method 特殊方法
      A method that is called implicitly by Python to execute a certain
	operation on a type, such as addition.  Such methods have names starting
	and ending with double underscores.  Special methods are documented in
	:ref:`specialnames`.
	
	一个称为由Python含蓄的方式来执行某操作上，如加成型。这种方法有名字
	双下划线开始和结束。特殊方法在*特殊方法名文件*.

   语句
      A statement is part of a suite (a "block" of code).  A statement is either
      an :term:`expression` or a one of several constructs with a keyword, such
      as :keyword:`if`, :keyword:`while` or :keyword:`for`.

	声明语句是一个套件的一部分（一个“块”的代码）。一个声明要么是表达式，要么是一种或几种带有关键字的构造结构，如``if``，``while ``or ``for ``。

   三重引号的字符串
      A string which is bound by three instances of either a quotation mark
      (") or an apostrophe (').  While they don't provide any functionality
      not available with single-quoted strings, they are useful for a number
      of reasons.  They allow you to include unescaped single and double
      quotes within a string and they can span multiple lines without the
      use of the continuation character, making them especially useful when
      writing docstrings.

	这是一个必然的三个实例也以一个字符串报价标记（“）或单引号（'）。虽然他们没有提供任何
	功能不与单引号可用，它们都有用的原因。它们允许你包含
	在字符串转义单引号和双引号，他们可以未经继续使用跨越多行字符，使他们写作时特别有用文档字符串。


   类型
      The type of a Python object determines what kind of object it is; every
      object has a type.  An object's type is accessible as its
      :attr:`__class__` attribute or can be retrieved with ``type(obj)``.

	一个Python对象的类型决定了什么样的对象是;每个对象都有一个类型。一个对象的类型作为其访问
	`` __class__``属性，也可以检索与``type（obj）``。

   视图
      The objects returned from :meth:`dict.keys`, :meth:`dict.values`, and
      :meth:`dict.items` are called dictionary views.  They are lazy sequences
      that will see changes in the underlying dictionary.  To force the
      dictionary view to become a full list use ``list(dictview)``.  See
      :ref:`dict-views`.

   虚拟机
      A computer defined entirely in software.  Python's virtual machine
      executes the :term:`bytecode` emitted by the bytecode compiler.

      计算机定义一个完全由软件。Python的虚拟机* *执行字节码编译器产生的字节码。

   Python之禅
      Listing of Python design principles and philosophies that are helpful in
      understanding and using the language.  The listing can be found by typing
      "``import this``" at the interactive prompt.

      Python的设计原则和理念是有助于了解和使用的语言。该清单可发现通过键入“``import this ``”在交互提示。
      
   类方法
   
   静态方法
   
   面向兑现编程
   
   
