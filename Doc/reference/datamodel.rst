
.. _datamodel:

**********
数据模型
**********


.. _objects:

对象, 值, 类型
=========================

.. index::
   single: object
   single: data

:dfn:`Objects` are Python's abstraction for data.  All data in a Python program
is represented by objects or by relations between objects. (In a sense, and in
conformance to Von Neumann's model of a "stored program computer," code is also
represented by objects.)

Python中的 :dfn:`对象` 是对数据的抽象. 所有Python程序中的数据都用对象或者对象关系表示 (Python中连代码也是对象, 这与诺依曼的" 存储程序计算机" 模型在某种意义上是一致的) . 

.. index::
   builtin: id
   builtin: type
   single: identity of an object
   single: value of an object
   single: type of an object
   single: mutable object
   single: immutable object

.. XXX it *is* now possible in some cases to change an object's
   type, under certain controlled conditions

Every object has an identity, a type and a value.  An object's *identity* never
changes once it has been created; you may think of it as the object's address in
memory.  The ':keyword:`is`' operator compares the identity of two objects; the
:func:`id` function returns an integer representing its identity (currently
implemented as its address). An object's :dfn:`type` is also unchangeable. [#]_
An object's type determines the operations that the object supports (e.g., "does
it have a length?") and also defines the possible values for objects of that
type.  The :func:`type` function returns an object's type (which is an object
itself).  The *value* of some objects can change.  Objects whose value can
change are said to be *mutable*; objects whose value is unchangeable once they
are created are called *immutable*. (The value of an immutable container object
that contains a reference to a mutable object can change when the latter's value
is changed; however the container is still considered immutable, because the
collection of objects it contains cannot be changed.  So, immutability is not
strictly the same as having an unchangeable value, it is more subtle.) An
object's mutability is determined by its type; for instance, numbers, strings
and tuples are immutable, while dictionaries and lists are mutable.

每个对象都有一个标识、一种类型和一个值. 一旦建立对象的 *标识* 就不能改变了, 你可以认为它就是对象的内存地址.  ':keyword:`is`' 操作符可以比较两个对象的标识;  :func:`id` 函数会返回对象标识 (目前是用地址实现的) 的一个整数表示. 对象的 :dfn:`类型 (type) ` 也是不可变的.   对象的类型确定了对象能够支持的操作 (例如, " 它有长度吗? ") , 同时它也定义了该种对象的取值范围.  :func:`type` 函数返回对象的类型 (类型本身也是一个对象) ,  某些对象的 *值* 可以改变, 值可以改变的对象称为是 *可变的 (mutable) * , 一旦创建完成值就不能改变的对象称为是 *不可变的 (immutable) *  (不可变容器对象如果引用了可变对象, 当可变对象改变了时, 它其实也是被修改了. 但它仍被看作是可变对象, 这是因为它所包含的对象集合是不能变的, 所以不可变对象与值不可变并不完全一样, 这有些比较微妙) 一个对象的可变性由它的类型决定, 例如数值、字符串和元组是不可变的, 而字典和列表是可变的. 

.. index::
   single: garbage collection
   single: reference counting
   single: unreachable object

Objects are never explicitly destroyed; however, when they become unreachable
they may be garbage-collected.  An implementation is allowed to postpone garbage
collection or omit it altogether --- it is a matter of implementation quality
how garbage collection is implemented, as long as no objects are collected that
are still reachable.  

对象从来不会被显式的的释放 (destroyed) , 但处于不可达状态的对象会被垃圾回收掉. 实现可以选择推迟垃圾回收甚至忽略掉这个过程 —— 这是实现垃圾回收机制的质量问题, 与语言本身无关. 只要还处于可达状态的对象不被回收就满足Python语言的基本要求. 

.. impl-detail::

   CPython currently uses a reference-counting scheme with (optional) delayed
   detection of cyclically linked garbage, which collects most objects as soon
   as they become unreachable, but is not guaranteed to collect garbage
   containing circular references.  See the documentation of the :mod:`gc`
   module for information on controlling the collection of cyclic garbage.
   Other implementations act differently and CPython may change.

   当前CPython实现使用引用计数机制和一个可选的循环垃圾延时检测机制, 只要对象进入不可达状态, 它就会尽量回收对象, 但不能保证回收含有循环引用的垃圾对象. 关于如何控制循环垃圾对象回收的详细情况, 可以参考 :mod:`gc` 模块) . 其他实现的行为与之不同, 而且CPython以后可能也会改变这个行为. 

Note that the use of the implementation's tracing or debugging facilities may
keep objects alive that would normally be collectable. Also note that catching
an exception with a ':keyword:`try`...\ :keyword:`except`' statement may keep
objects alive.

注意, 使用实现提供的跟踪和调试工具时可能会导致本该回收的对象不被回收. 此外, 语句 ':keyword:`try`...\ :keyword:`except`' 也可能导致此情况. 

Some objects contain references to "external" resources such as open files or
windows.  It is understood that these resources are freed when the object is
garbage-collected, but since garbage collection is not guaranteed to happen,
such objects also provide an explicit way to release the external resource,
usually a :meth:`close` method. Programs are strongly recommended to explicitly
close such objects.  The ':keyword:`try`...\ :keyword:`finally`' statement
and the ':keyword:`with`' statement provide convenient ways to do this.

有些对象包括对 "外部" 资源的引用, 例如文件或窗口. 垃圾回收会释放这些资源是顺其自然的做法, 但因为并不保证垃圾回收一定会发生, 所以这样的对象一般都提供了显式的方法释放这些资源, 通常是用 :meth:`close` 方法. 高度推荐使用这种方法释放引用了外部资源的对象. ':keyword:`try`...\ :keyword:`finally`' 和 ':keyword:`with`' 语句为执行这种方法提供了方便. 

.. index:: single: container

Some objects contain references to other objects; these are called *containers*.
Examples of containers are tuples, lists and dictionaries.  The references are
part of a container's value.  In most cases, when we talk about the value of a
container, we imply the values, not the identities of the contained objects;
however, when we talk about the mutability of a container, only the identities
of the immediately contained objects are implied.  So, if an immutable container
(like a tuple) contains a reference to a mutable object, its value changes if
that mutable object is changed.

引用了其它对象的对象叫做 *容器* , 容器的例子有元组、列表和字典. 引用是容器值的一部分. 大多数情况下, 当我们谈及一个容器的值时, 指的只是值, 而不是被包含对象的标识符. 但是, 当我们谈及容器对象可变性的时候, 指的就是被直接包含的对象的标识了. 因此, 如果一个不可变对象 (如元组) 包含了可变对象, 只要这个可变对象的值变了则容器的值就也改变了. 

Types affect almost all aspects of object behavior.  Even the importance of
object identity is affected in some sense: for immutable types, operations that
compute new values may actually return a reference to any existing object with
the same type and value, while for mutable objects this is not allowed.  E.g.,
after ``a = 1; b = 1``, ``a`` and ``b`` may or may not refer to the same object
with the value one, depending on the implementation, but after ``c = []; d =
[]``, ``c`` and ``d`` are guaranteed to refer to two different, unique, newly
created empty lists. (Note that ``c = d = []`` assigns the same object to both
``c`` and ``d``.)

类型影响了对象的绝大多数行为, 甚至在某种程度上对对象标识也有重要影响. 对于不可变对象, 计算新值的操作符实际返回的可能是, 一个指向已存在的具有相同类型和值的对象的引用. 对于可变对象来说, 这是不允许的. 例如: 在 ``a = 1; b = 1`` 之后,  ``a`` 和 ``b`` 可能指向同一个具有 ``1`` 值的对象, 具体如何取决于实现. 但 ``c = []; d =[]`` 之后,  ``c`` 和 ``d`` 可以保证是两个不同的、独立的、新建的空列表 (注意 ``c = d = []`` 是把相同的对象赋给了 ``c`` 和 ``d`` ) . 

.. _types:

标准类型层次结构
===========================

.. index::
   single: type
   pair: data; type
   pair: type; hierarchy
   pair: extension; module
   pair: C; language

Below is a list of the types that are built into Python.  Extension modules
(written in C, Java, or other languages, depending on the implementation) can
define additional types.  Future versions of Python may add types to the type
hierarchy (e.g., rational numbers, efficiently stored arrays of integers, etc.),
although such additions will often be provided via the standard library instead.

以下是Python内置类型的列表, 扩展模块 (根据不同实现的情况, 可能是C、Java或者其他语言写的) 可以定义其它内置类型. 未来版本的Python可能会在此类型层次中增加新的类型 (例如: 有理数、高效存储的整数数组等) , 不过这些类型通常是在标准库中定义的. 

.. index::
   single: attribute
   pair: special; attribute
   triple: generic; special; attribute

Some of the type descriptions below contain a paragraph listing 'special
attributes.'  These are attributes that provide access to the implementation and
are not intended for general use.  Their definition may change in the future.

以下个别类型描述中可能有介绍" 特殊属性" 的段落, 它们是供实现访问的, 不作为一般用途. 这些定义在未来有可能发生改变: 

None
   .. index:: object: None

   This type has a single value.  There is a single object with this value. This
   object is accessed through the built-in name ``None``. It is used to signify the
   absence of a value in many situations, e.g., it is returned from functions that
   don't explicitly return anything. Its truth value is false.

   这个类型只具有一个值, 并且这种类型也只有一个对象, 这个对象可以通过内置名字 ``None`` 访问, 在许多场合里它表示无值, 例如, 没有显式返回值的函数会返回 ``None`` . 这个对象的真值为假. 

NotImplemented
   .. index:: object: NotImplemented

   This type has a single value.  There is a single object with this value. This
   object is accessed through the built-in name ``NotImplemented``. Numeric methods
   and rich comparison methods may return this value if they do not implement the
   operation for the operands provided.  (The interpreter will then try the
   reflected operation, or some other fallback, depending on the operator.)  Its
   truth value is true.

   这个类型只具有一个值, 并且这种类型也只有一个对象. 这个对象可以通过内置名字 ``NotImplemented`` 访问. 如果操作数没有对应实现, 数值方法和厚比较 (rich comparison) 方法就会可能返回这个值  (依赖于操作符, 解释器然后会尝试反射操作 (见后) 、或者其它后备操作) . 它的真值为真. 

Ellipsis
   .. index:: object: Ellipsis

   This type has a single value.  There is a single object with this value. This
   object is accessed through the literal ``...`` or the built-in name
   ``Ellipsis``.  Its truth value is true.

   这个类型只具有一个值, 并且这种类型也只有一个对象. 这个对象可以通过字面值 ``...`` 或者内置名字 ``Ellipsis`` 访问. 它的真值为真. 

:class:`numbers.Number`
   .. index:: object: numeric

   These are created by numeric literals and returned as results by arithmetic
   operators and arithmetic built-in functions.  Numeric objects are immutable;
   once created their value never changes.  Python numbers are of course strongly
   related to mathematical numbers, but subject to the limitations of numerical
   representation in computers.

   它们由数值型字面值产生, 或者是算术运算符和内置算术函数的返回值. 数值型对象是不可变的, 即一旦创建, 其值就不可改变. Python数值型和数学上的数字关系当然是非常密切的, 但也受到计算机数值表达能力的限制. 

   Python distinguishes between integers, floating point numbers, and complex
   numbers:

   Python区分整数, 浮点数和复数: 

   :class:`numbers.Integral`
      .. index:: object: integer

      These represent elements from the mathematical set of integers (positive and
      negative).

      描述了数学上的整数集 (正负数) . 

      There are two types of integers:

      有两类整数: 

      Integers (:class:`int`)

         These represent numbers in an unlimited range, subject to available (virtual)
         memory only.  For the purpose of shift and mask operations, a binary
         representation is assumed, and negative numbers are represented in a variant of
         2's complement which gives the illusion of an infinite string of sign bits
         extending to the left.

         整数类型. 表示不限范围的数字. 移位和掩码操作符可以认为整数是这样组织的: 负数用二进制补码的一种变体表示, 符号位会扩展至左边无限多位. 

      Booleans (:class:`bool`)
         .. index::
            object: Boolean
            single: False
            single: True

         These represent the truth values False and True.  The two objects representing
         the values False and True are the only Boolean objects. The Boolean type is a
         subtype of the integer type, and Boolean values behave like the values 0 and 1,
         respectively, in almost all contexts, the exception being that when converted to
         a string, the strings ``"False"`` or ``"True"`` are returned, respectively.

         布尔类型. 这种类型表示两个真值: 假 (False) 和真 (True) . 这个类型只有这两个的对象. Boolean类型是整数类型的一个子类, 在绝大多数情况下, Boolean类型值的行为分别与0和1差不多. 但转换为字符串时是个例外, 它们分别对应 ``"False"`` 和 ``"True"`` . 

      .. index:: pair: integer; representation

      The rules for integer representation are intended to give the most meaningful
      interpretation of shift and mask operations involving negative integers.

      如此设计整数表示方法的一个目的是, 使得负数在移位和掩码操作中能够更有意义. 

   :class:`numbers.Real` (:class:`float`)
      .. index::
         object: floating point
         pair: floating point; number
         pair: C; language
         pair: Java; language

      These represent machine-level double precision floating point numbers. You are
      at the mercy of the underlying machine architecture (and C or Java
      implementation) for the accepted range and handling of overflow. Python does not
      support single-precision floating point numbers; the savings in processor and
      memory usage that are usually the reason for using these is dwarfed by the
      overhead of using objects in Python, so there is no reason to complicate the
      language with two kinds of floating point numbers.

      浮点数. 本类型表示了机器级的双精度浮点数. 硬件的底层体系结构 (和C、Java实现) 对你隐藏了浮点数取值范围和溢出处理的复杂细节. Python不支持单精度浮点数. 使用单精度浮点数的原因一般是为了降低CPU负荷和节省内存, 但是这个努力会被Python的对象处理代价所抵消, 因此没有必要同时支持两种浮点数, 使Python复杂化. 

   :class:`numbers.Complex` (:class:`complex`)
      .. index::
         object: complex
         pair: complex; number

      These represent complex numbers as a pair of machine-level double precision
      floating point numbers.  The same caveats apply as for floating point numbers.
      The real and imaginary parts of a complex number ``z`` can be retrieved through
      the read-only attributes ``z.real`` and ``z.imag``.

      复数. 本类型用一对机器级的双精度浮点数表示复数. 关于浮点数的介绍也适用于复数类型. 复数 ``z`` 的实部和虚部可以通过属性 ``z.real`` 和 ``z.imag`` 获得. 

Sequences
   .. index::
      builtin: len
      object: sequence
      single: index operation
      single: item selection
      single: subscription

   These represent finite ordered sets indexed by non-negative numbers. The
   built-in function :func:`len` returns the number of items of a sequence. When
   the length of a sequence is *n*, the index set contains the numbers 0, 1,
   ..., *n*-1.  Item *i* of sequence *a* is selected by ``a[i]``.

   有序类型. 本类型描述的是, 以非负数作为元素索引, 由有限元素构成的有序集合. 内置函数 :func:`len` 返回有序类型数据中的元素数. 当有序类型长度为 *n* 时, 索引号为0,1, ...,  *n* -1. 有序类型 *a* 中的项 *i* , 用 ``a[i]`` 表示. 

   .. index:: single: slicing

   Sequences also support slicing: ``a[i:j]`` selects all items with index *k* such
   that *i* ``<=`` *k* ``<`` *j*.  When used as an expression, a slice is a
   sequence of the same type.  This implies that the index set is renumbered so
   that it starts at 0.

   有序类型也支持片断:  ``a[i:j]`` 表示满足 *i* ``<=`` *k* ``<`` *j* 的所有项 ``a[k]`` . 在作为表达式使用时, 这个片断与原始的有序类型类型相同, 这隐含着会重新编号索引, 即从零开始. 

   Some sequences also support "extended slicing" with a third "step" parameter:
   ``a[i:j:k]`` selects all items of *a* with index *x* where ``x = i + n*k``, *n*
   ``>=`` ``0`` and *i* ``<=`` *x* ``<`` *j*.

   个别有序类型还支持有第三个"步长"参数的 ``扩展片断`` :  ``a[i:j:k]`` 选择了所有索引 *x* :  ``x = i + n*k``, *n*
   ``>=`` ``0`` 并且 *i* ``<=`` *x* ``<`` *j* . 

   Sequences are distinguished according to their mutability:

   有序类型按照可变性可以分为: 

   Immutable sequences
      .. index::
         object: immutable sequence
         object: immutable

      An object of an immutable sequence type cannot change once it is created.  (If
      the object contains references to other objects, these other objects may be
      mutable and may be changed; however, the collection of objects directly
      referenced by an immutable object cannot change.)

      不可变有序类型. 一旦建立不可变对象的值就不可修改. (如果这个对象引用了其它对象, 这个被引用的对象可以是可变对象, 并且这个对象的值可以变化. 但是, 不可变对象所包括的可变对象集合是不能变的. ) 

      The following types are immutable sequences:

      以下是不可变序列类型: 

      Strings
         .. index::
            builtin: chr
            builtin: ord
            builtin: str
            single: character
            single: integer
            single: Unicode

         The items of a string object are Unicode code units.  A Unicode code
         unit is represented by a string object of one item and can hold either
         a 16-bit or 32-bit value representing a Unicode ordinal (the maximum
         value for the ordinal is given in ``sys.maxunicode``, and depends on
         how Python is configured at compile time).  Surrogate pairs may be
         present in the Unicode object, and will be reported as two separate
         items.  The built-in functions :func:`chr` and :func:`ord` convert
         between code units and nonnegative integers representing the Unicode
         ordinals as defined in the Unicode Standard 3.0. Conversion from and to
         other encodings are possible through the string method :meth:`encode`.

         字符串对象的项是Unicode code unit. Unicode code unit是只有一项的字符串, 项可以是表示Unicode ordinal的16位或者32位值 ( ``sys.maxunicode`` 指定了ordinal的最大值, 具体值依赖于Python是如何编译的) . Unicode对象可以表示Surrogate pair, 它们会被处理成分开的两项. 内置函数 :func:`chr` 和 :func:`ord` 可以在code unit与Unicode 3.0标准中表示unicode ordinal的非负整数之间互相转换. 与其它编码的相互转换可以通过字符串方法 :meth:`encode` 进行. 

      Tuples
         .. index::
            object: tuple
            pair: singleton; tuple
            pair: empty; tuple

         The items of a tuple are arbitrary Python objects. Tuples of two or
         more items are formed by comma-separated lists of expressions.  A tuple
         of one item (a 'singleton') can be formed by affixing a comma to an
         expression (an expression by itself does not create a tuple, since
         parentheses must be usable for grouping of expressions).  An empty
         tuple can be formed by an empty pair of parentheses.

         元组. 元组的项可以是任意Python对象. 包括多个项 (两个及以上) 的元组由逗号分隔的表达式的列表构成. 只有一项的元组 ( ``独元`` ) , 可以在项后加一个逗号表示 (单个表达式本身并不能创建元组, 因为圆括号本身也可以用于表达式的分组) . 一个空元组可以用一对空圆括号表示. 

      Bytes
         .. index:: bytes, byte

         A bytes object is an immutable array.  The items are 8-bit bytes,
         represented by integers in the range 0 <= x < 256.  Bytes literals
         (like ``b'abc'`` and the built-in function :func:`bytes` can be used to
         construct bytes objects.  Also, bytes objects can be decoded to strings
         via the :meth:`decode` method.

         字节序列. 字节序列是一个不可变的数组. 每项都是值在0 <= x < 256之内的8位字节整数. 字节字面值 (例如,  ``b'abc'`` ) 和内置函数 :func:`bytes` 可用于构造字节对象. 可以使用 :meth:`decode` 方法将字节序列对象解码为字符串对象. 

   Mutable sequences
      .. index::
         object: mutable sequence
         object: mutable
         pair: assignment; statement
         single: delete
         statement: del
         single: subscription
         single: slicing

      Mutable sequences can be changed after they are created.  The subscription and
      slicing notations can be used as the target of assignment and :keyword:`del`
      (delete) statements.

      可变对象可以在创建后改变, 其下标表示和片断表示可以作为赋值语句和 :keyword:`del`  (删除) 语句的目标. 

      There are currently two intrinsic mutable sequence types:

      目前, 有两种内置的可变序列对象: 

      Lists
         .. index:: object: list

         The items of a list are arbitrary Python objects.  Lists are formed by
         placing a comma-separated list of expressions in square brackets. (Note
         that there are no special cases needed to form lists of length 0 or 1.)

         列表. 列表的项可以是Python的任意类型对象. 列表由在方括号之间的用逗号分开的表达式的列表构成.  (注意, 构造长度为0或者1的列表不要求特别写法) 

      Byte Arrays
         .. index:: bytearray

         A bytearray object is a mutable array. They are created by the built-in
         :func:`bytearray` constructor.  Aside from being mutable (and hence
         unhashable), byte arrays otherwise provide the same interface and
         functionality as immutable bytes objects.

         字节数组. 这是一个可变数组, 可以用内置函数 :func:`bytearray` 构造. 除了是可变的 (因此, 也是不可散列的) , 字节数组提供了与不可变的字节序列类型相同的接口. 
      .. index:: module: array

      The extension module :mod:`array` provides an additional example of a
      mutable sequence type, as does the :mod:`collections` module.

      扩展模块 :mod:`array` 提供另一种可变序列类型, 模块 :mod:`collections` 也是如此. 

Set types
   .. index::
      builtin: len
      object: set type

   These represent unordered, finite sets of unique, immutable objects. As such,
   they cannot be indexed by any subscript. However, they can be iterated over, and
   the built-in function :func:`len` returns the number of items in a set. Common
   uses for sets are fast membership testing, removing duplicates from a sequence,
   and computing mathematical operations such as intersection, union, difference,
   and symmetric difference.

   集合类型. 这个类型描述的是由有限数量的不可变对象构成的无序集合, 对象不能在集合中重复. 它们不能用任何索引作为下标, 但它们可以被迭代, 内置函数 :func:`len` 可以计算集合里的元素数. 集合的常用场合是快速测试某元素是否在集合中, 或者是从一个有序类型中删除重复元素, 或者是做一些数学运算, 比如求集合的交集、并集、差和对称差. 

   For set elements, the same immutability rules apply as for dictionary keys. Note
   that numeric types obey the normal rules for numeric comparison: if two numbers
   compare equal (e.g., ``1`` and ``1.0``), only one of them can be contained in a
   set.

   集合的元素与字典键一样, 都遵循不可变性对象的规则. 注意, 数值类型遵守数值比较的正常规则. 即比较相等的两个数值型对象, 只有一个能存在于集合中, 例如,  ``1`` 和 ``1.0`` . 

   There are currently two intrinsic set types:

   当前有两种内置的集合类型: 

   Sets
      .. index:: object: set

      These represent a mutable set. They are created by the built-in :func:`set`
      constructor and can be modified afterwards by several methods, such as
      :meth:`add`.

      集合. 这表示可变集合, 可以用内置函数 :func:`set` 构造, 之后也可以使用用一系列方法修改这个集合, 比如 :meth:`add` . 

   Frozen sets
      .. index:: object: frozenset

      These represent an immutable set.  They are created by the built-in
      :func:`frozenset` constructor.  As a frozenset is immutable and
      :term:`hashable`, it can be used again as an element of another set, or as
      a dictionary key.

      冻结集合. 这表示一个不可变集合. 由内置函数 :func:`frozenset` 构造. 这种类型的对象是不可变的, 并且是可散列的 ( :term:`hashable` ) , 因此它可以作为另一个集合的元素, 或者作为字典健使用. 

Mappings
   .. index::
      builtin: len
      single: subscription
      object: mapping

   These represent finite sets of objects indexed by arbitrary index sets. The
   subscript notation ``a[k]`` selects the item indexed by ``k`` from the mapping
   ``a``; this can be used in expressions and as the target of assignments or
   :keyword:`del` statements. The built-in function :func:`len` returns the number
   of items in a mapping.

   映射类型. 表示由任意类型作索引的有限对象集合. 下标记法 ``a[k]`` 表示在映射类型对象 ``a`` 中选择以 ``k`` 为索引的项, 这该项可以用于表达式、作为赋值语句和 :keyword:`del` 语句的目标. 内置函数 :func:`len` 返回映射对象的元素数量. 

   There is currently a single intrinsic mapping type:
   
   目前只有一种内置映射类型: 

   Dictionaries
      .. index:: object: dictionary

      These represent finite sets of objects indexed by nearly arbitrary values.  The
      only types of values not acceptable as keys are values containing lists or
      dictionaries or other mutable types that are compared by value rather than by
      object identity, the reason being that the efficient implementation of
      dictionaries requires a key's hash value to remain constant. Numeric types used
      for keys obey the normal rules for numeric comparison: if two numbers compare
      equal (e.g., ``1`` and ``1.0``) then they can be used interchangeably to index
      the same dictionary entry.

      字典类型. 表示一个有限对象集合, 几乎可以用任意值索引其中的对象. 包括列表和字典的值可以是值, 但不能是键, 或者其它通过值比较而不是以对象标识比较的可变对象也不能作为键, 其原因是字典的实现效率要求键的散列值保持不变. 数值比较结果相等的两个数值型对象, 例如, ``1`` 和 ``1.0`` , 在作为字典值的索引 (键) 时是等效的. 

      Dictionaries are mutable; they can be created by the ``{...}`` notation (see
      section :ref:`dict`).

      字典是可变的, 可以用 ``{...}`` 语法创建它们, 参见 :ref:`dict` . 

      .. index::
         module: dbm.ndbm
         module: dbm.gnu

      The extension modules :mod:`dbm.ndbm` and :mod:`dbm.gnu` provide
      additional examples of mapping types, as does the :mod:`collections`
      module.

      扩展模块 :mod:`dbm.ndbm` 、 :mod:`dbm.gnu` 和 :mod:`collections` 提供了其他映射类型的例子. 

Callable types
   .. index::
      object: callable
      pair: function; call
      single: invocation
      pair: function; argument

   These are the types to which the function call operation (see section
   :ref:`calls`) can be applied:

   可调用类型. 这是表示功能调用操作的类型, 见 :ref:`calls` . 

   User-defined functions
      .. index::
         pair: user-defined; function
         object: function
         object: user-defined function

      A user-defined function object is created by a function definition (see
      section :ref:`function`).  It should be called with an argument list
      containing the same number of items as the function's formal parameter
      list.

      用户定义函数对象由函数定义 (见 :ref:`function` ) 创建. 调用函数时的参数数量, 应该与定义时的形式参数量相同. 

      Special attributes:

      特殊属性: 

      +-------------------------+-------------------------------+-----------+
      | Attribute               | Meaning                       |           |
      +=========================+===============================+===========+
      | :attr:`__doc__`         | The function's documentation  | Writable  |
      |                         | string, or ``None`` if        |           |
      |                         | unavailable                   |           |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__name__`        | The function's name           | Writable  |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__module__`      | The name of the module the    | Writable  |
      |                         | function was defined in, or   |           |
      |                         | ``None`` if unavailable.      |           |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__defaults__`    | A tuple containing default    | Writable  |
      |                         | argument values for those     |           |
      |                         | arguments that have defaults, |           |
      |                         | or ``None`` if no arguments   |           |
      |                         | have a default value          |           |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__code__`        | The code object representing  | Writable  |
      |                         | the compiled function body.   |           |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__globals__`     | A reference to the dictionary | Read-only |
      |                         | that holds the function's     |           |
      |                         | global variables --- the      |           |
      |                         | global namespace of the       |           |
      |                         | module in which the function  |           |
      |                         | was defined.                  |           |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__dict__`        | The namespace supporting      | Writable  |
      |                         | arbitrary function            |           |
      |                         | attributes.                   |           |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__closure__`     | ``None`` or a tuple of cells  | Read-only |
      |                         | that contain bindings for the |           |
      |                         | function's free variables.    |           |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__annotations__` | A dict containing annotations | Writable  |
      |                         | of parameters.  The keys of   |           |
      |                         | the dict are the parameter    |           |
      |                         | names, or ``'return'`` for    |           |
      |                         | the return annotation, if     |           |
      |                         | provided.                     |           |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__kwdefaults__`  | A dict containing defaults    | Writable  |
      |                         | for keyword-only parameters.  |           |
      +-------------------------+-------------------------------+-----------+

---------------------------------------------------------------------------------------------------------------------------------------------

      +-------------------------+-------------------------------+-----------+
      | 属性                    | 含义                          |           |
      +=========================+===============================+===========+
      | :attr:`__doc__`         | 函数的文档. 字符串, 如果没有  | 可写      |
      |                         | 的话就为 ``None``             |           |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__name__`        | 函数名                        | 可写      |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__module__`      | 定义函数的模块名, 或者如果没有| 可写      |
      |                         | 对应模块名, 就为 ``None``     |           |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__defaults__`    | 如果任何参数有默认值, 这个分组| 可写      |
      |                         | 保存默认值, 否则为 ``None``   |           |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__code__`        | 表示编译后的函数体的代码对象  | 可写      |      
      +-------------------------+-------------------------------+-----------+
      | :attr:`__globals__`     | 函数的全局变量字典引用, 即函数| 只读      |
      |                         | 定义处的全局名字空间.         |           | 
      +-------------------------+-------------------------------+-----------+
      | :attr:`__dict__`        | 支持任意函数属性的名字空间.   | 可写      |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__closure__`     | 元组, 含有函数自由变量绑定, 如| 只读      |
      |                         | 果没有自由变量, 就为 ``None`` |           |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__annotations__` | 一个含有参数注解              | 可写      |
      |                         |  (annotations) 的字典, 键为参 |           |
      |                         | 数名. 如果有返回值, 返回值的键|           |
      |                         | 为 ``return``                 |           |
      +-------------------------+-------------------------------+-----------+
      | :attr:`__kwdefaults__`  | 只包括关键字参数默认值的字典  | 可写      |
      +-------------------------+-------------------------------+-----------+

---------------------------------------------------------------------------------------------------------------------------------------------

      Most of the attributes labelled "Writable" check the type of the assigned value.

                以上大多数标记为 "可写" 的属性都会对赋的值做类型检查. 

      Function objects also support getting and setting arbitrary attributes, which
      can be used, for example, to attach metadata to functions.  Regular attribute
      dot-notation is used to get and set such attributes. *Note that the current
      implementation only supports function attributes on user-defined functions.
      Function attributes on built-in functions may be supported in the future.*

      函数对象也支持用获得 (getting) 和设置(setting)任意合法属性(attribute), 比如可以用这种方法将函数与元信息关联起来. 常规的 "点＋属性" 就可以获取和设置这些属性.  *注意, 当前实现只在用户自定义函数上支持函数属性, 未来版本可能会支持内置函数的函数属性. * 

      Additional information about a function's definition can be retrieved from its
      code object; see the description of internal types below.

      函数定义的其它信息可通过它的代码对象获得, 参考下面关于内部类型的介绍. 

      .. index::
         single: __doc__ (function attribute)
         single: __name__ (function attribute)
         single: __module__ (function attribute)
         single: __dict__ (function attribute)
         single: __defaults__ (function attribute)
         single: __closure__ (function attribute)
         single: __code__ (function attribute)
         single: __globals__ (function attribute)
         single: __annotations__ (function attribute)
         single: __kwdefaults__ (function attribute)
         pair: global; namespace

   Instance methods
      .. index::
         object: method
         object: user-defined method
         pair: user-defined; method

      An instance method object combines a class, a class instance and any
      callable object (normally a user-defined function).

      实例方法对象把类、类实例和任意可调用对象 (通常是用户定义函数) 组合到了一起. 

      .. index::
         single: __func__ (method attribute)
         single: __self__ (method attribute)
         single: __doc__ (method attribute)
         single: __name__ (method attribute)
         single: __module__ (method attribute)

      Special read-only attributes: :attr:`__self__` is the class instance object,
      :attr:`__func__` is the function object; :attr:`__doc__` is the method's
      documentation (same as ``__func__.__doc__``); :attr:`__name__` is the
      method name (same as ``__func__.__name__``); :attr:`__module__` is the
      name of the module the method was defined in, or ``None`` if unavailable.

      只读特殊属性:  :attr:`__self__` 是类实例对象,  :attr:`__func__` 是函数对象,  :attr:`__doc__` 是方法的文档 (与 :attr:`__func__.__doc__` 相同) ;  :attr:`__name__` 是方法的名字 (与 ``__func__.__name__`` 相同) ;  :attr:`__module__` 函数定义所在的模块名字, 如果没有对应模块, 就为 ``None`` . 

      Methods also support accessing (but not setting) the arbitrary function
      attributes on the underlying function object.

      方法也支持对底层函数对象任意属性的访问, 但不支持设置. 

      User-defined method objects may be created when getting an attribute of a
      class (perhaps via an instance of that class), if that attribute is a
      user-defined function object or a class method object.

      用户定义方法对象可以通过获取类属性 (也可能是通过该类的一个实例) 创建, 但前提是这个属性是用户定义函数对象, 或者类方法对象. 

      When an instance method object is created by retrieving a user-defined
      function object from a class via one of its instances, its
      :attr:`__self__` attribute is the instance, and the method object is said
      to be bound.  The new method's :attr:`__func__` attribute is the original
      function object.

      通过获取一个类实例的用户定义函数, 创建新实例方法对象的时候, 新对象的属性 :attr:`__self__` 指向该类实例, 这个方法称为是 "被绑定的" . 这个方法的属性 :attr:`__func__` 指向底层的函数对象. 

      When a user-defined method object is created by retrieving another method
      object from a class or instance, the behaviour is the same as for a
      function object, except that the :attr:`__func__` attribute of the new
      instance is not the original method object but its :attr:`__func__`
      attribute.

      当用户定义方法对象是通过获取类、或者类实例的另一个方法对象创建新方法对象的时候 (? ) , 它的行为与函数对象的行为相同, 除了新实例的属性 :attr:`__func__`  指向新对象本身的 :attr:`__func__` , 而不是原始的方法对象. 

      When an instance method object is created by retrieving a class method
      object from a class or instance, its :attr:`__self__` attribute is the
      class itself, and its :attr:`__func__` attribute is the function object
      underlying the class method.

      当实例方法对象是通过获取类或者实例的类方法对象创建时, 它的属性 :attr:`__self__` 指向类本身, 属性 :attr:`__func__` 指向类方法底层的函数对象. 

      When an instance method object is called, the underlying function
      (:attr:`__func__`) is called, inserting the class instance
      (:attr:`__self__`) in front of the argument list.  For instance, when
      :class:`C` is a class which contains a definition for a function
      :meth:`f`, and ``x`` is an instance of :class:`C`, calling ``x.f(1)`` is
      equivalent to calling ``C.f(x, 1)``.

      调用实例方法对象时会调用底层的方法 ( :attr:`__func__` ) , 还会把类实例 ( :attr:`__self__` ) 插入到其参数列表的前面. 例如, 如果类 :class:`C` 是定义了函数 :meth:`f` , 并且 ``x`` 是 :class:`C` 的一个实例, 那么调用 ``x.f(1)`` 等价于调用 ``C.f(x,1)`` . 

      When an instance method object is derived from a class method object, the
      "class instance" stored in :attr:`__self__` will actually be the class
      itself, so that calling either ``x.f(1)`` or ``C.f(1)`` is equivalent to
      calling ``f(C,1)`` where ``f`` is the underlying function.

      当实例方法对象是从类方法对象继承的时候, 属性 :attr:`__self__` 保存的 "类实例" 实际上是类自身, 所以调用 ``x.f(1)`` 或 ``C.f(1)`` 等价于调用 ``f(C,1)`` , 其中 ``f`` 是底层函数. 

      Note that the transformation from function object to instance method
      object happens each time the attribute is retrieved from the instance.  In
      some cases, a fruitful optimization is to assign the attribute to a local
      variable and call that local variable. Also notice that this
      transformation only happens for user-defined functions; other callable
      objects (and all non-callable objects) are retrieved without
      transformation.  It is also important to note that user-defined functions
      which are attributes of a class instance are not converted to bound
      methods; this *only* happens when the function is an attribute of the
      class.

      注意每次从实例获取属性时都会发生从函数对象到实例方法对象的转换. 在某些情况下, 一种有效优化方法是把属性赋给一个局部变量, 然后调用这个局部变量. 同时也要注意, 这种转换只会在用户定义函数上发生, 获取其它可调用对象 (和所有不可调用对象) 是不经转换的. 而且, 这种转换在作为类实例属性的用户定义函数是不会转换成绑定方法的, 它 *只* 发生在函数是类属性的时候. 

   Generator functions
      .. index::
         single: generator; function
         single: generator; iterator

      A function or method which uses the :keyword:`yield` statement (see section
      :ref:`yield`) is called a :dfn:`generator function`.  Such a function, when
      called, always returns an iterator object which can be used to execute the
      body of the function:  calling the iterator's :meth:`__next__` method will
      cause the function to execute until it provides a value using the
      :keyword:`yield` statement.  When the function executes a
      :keyword:`return` statement or falls off the end, a :exc:`StopIteration`
      exception is raised and the iterator will have reached the end of the set of
      values to be returned.

       (原文此段有误, 译文已更正) 使用了 :keyword:`yield` 语句 (见 :ref:`yield` ) 的函数或方法叫做 :dfn:`generator function` . 这个函数会返回一个用于继续调用这个函数体的产生器 (generator) 对象, 使用内置函数 :func:`next` 调用这个产生器对象 `next(generator)` 会得到每次 :keyword:`yield` 语句的返回值, 如果函数遇到 :keyword:`return` 语句, 或者到达结束处, 它就会抛出 :exc:`StopIteration` 异常. 

   Built-in functions
      .. index::
         object: built-in function
         object: function
         pair: C; language

      A built-in function object is a wrapper around a C function.  Examples of
      built-in functions are :func:`len` and :func:`math.sin` (:mod:`math` is a
      standard built-in module). The number and type of the arguments are
      determined by the C function. Special read-only attributes:
      :attr:`__doc__` is the function's documentation string, or ``None`` if
      unavailable; :attr:`__name__` is the function's name; :attr:`__self__` is
      set to ``None`` (but see the next item); :attr:`__module__` is the name of
      the module the function was defined in or ``None`` if unavailable.

      内置函数. 内置函数对象就是C函数的包装. 内置函数的例子有 :func:`len` 和 :func:`math.sin`  ( :mod:`math` 是一个标准内置模块) . 参数的类型和数量由对应的C函数决定. 只读特殊属性有 :attr:`__doc__` , 是函数文档字符串或者 ``None`` ;  :attr:`__name__` 是函数名;  :attr:`__self__` 为 ``None``  (请留意下面关于 "内置方法" 的介绍) ;  :attr:`__module__` 是函数定义所在模块的名字, 或者是 ``None`` . 

   Built-in methods
      .. index::
         object: built-in method
         object: method
         pair: built-in; method

      This is really a different disguise of a built-in function, this time containing
      an object passed to the C function as an implicit extra argument.  An example of
      a built-in method is ``alist.append()``, assuming *alist* is a list object. In
      this case, the special read-only attribute :attr:`__self__` is set to the object
      denoted by *alist*.

      内置方法. 这实际上内置函数的一个包装, 调用时对应对象作为隐藏的额外参数被传递到C函数内. 内置方法的一个例子是 ``alist.append()`` , 这里假定 ``alist`` 是一个列表对象. 这时, 只读特殊属性 :attr:`__self__` 被设置为 *列表对象* . 

   Classes
      Classes are callable.  These objects normally act as factories for new
      instances of themselves, but variations are possible for class types that
      override :meth:`__new__`.  The arguments of the call are passed to
      :meth:`__new__` and, in the typical case, to :meth:`__init__` to
      initialize the new instance.

      类是可调用的. 通常用作类实例的工厂使用. 不同对象间的差异可以通过重载类的方法 :meth:`__new__` 做到. 调用参数都会传递给  :meth:`__new__` , 但一般情况下, 由 :meth:`__init__` 初始化新实例. 

   Class Instances
      Instances of arbitrary classes can be made callable by defining a
      :meth:`__call__` method in their class.

      通过定义 :meth:`__call__` 方法, 可以使任何类实例变成可调用的. 

Modules
   .. index::
      statement: import
      object: module

   Modules are imported by the :keyword:`import` statement (see section
   :ref:`import`). A module object has a
   namespace implemented by a dictionary object (this is the dictionary referenced
   by the __globals__ attribute of functions defined in the module).  Attribute
   references are translated to lookups in this dictionary, e.g., ``m.x`` is
   equivalent to ``m.__dict__["x"]``. A module object does not contain the code
   object used to initialize the module (since it isn't needed once the
   initialization is done).

   模块可以用 :keyword:`import` 语句 (见 :ref:`import` ) 语句导入. 每个模块都有一个用字典对象实现的名字空间 (在模块中定义的函数的__global__属性引用的就是这个字典) . 模块属性的访问被转换成查找这个字典, 例如,  ``m.x`` 等价于 ``m.__dict__[" x" ]`` . 模块对象不包含初始化该模块的代码对象 (因为初始化完成后就不再需要它了) . 

   Attribute assignment updates the module's namespace dictionary, e.g., ``m.x =
   1`` is equivalent to ``m.__dict__["x"] = 1``.

   对模块属性的赋值会更新模块的名字空间, 例如 ``m.x = 1`` 等价于 ``m.__dict__[" x" ] = 1`` . 

   .. index:: single: __dict__ (module attribute)

   Special read-only attribute: :attr:`__dict__` is the module's namespace as a
   dictionary object.

   .. impl-detail::

      Because of the way CPython clears module dictionaries, the module
      dictionary will be cleared when the module falls out of scope even if the
      dictionary still has live references.  To avoid this, copy the dictionary
      or keep the module around while using its dictionary directly.

   .. index::
      single: __name__ (module attribute)
      single: __doc__ (module attribute)
      single: __file__ (module attribute)
      pair: module; namespace

   Predefined (writable) attributes: :attr:`__name__` is the module's name;
   :attr:`__doc__` is the module's documentation string, or ``None`` if
   unavailable; :attr:`__file__` is the pathname of the file from which the module
   was loaded, if it was loaded from a file. The :attr:`__file__` attribute is not
   present for C modules that are statically linked into the interpreter; for
   extension modules loaded dynamically from a shared library, it is the pathname
   of the shared library file.

   预定义的可写属性:  :attr:`__name__` 是模块名;  :attr:`__doc__` 是模块的文档字符串或 ``None`` . 如果模块是由文件加载的,  :attr:`__file__` 是对应文件的路径名, 用C语言编写的静态链接进解释器的模块没有这个属性, 而对于从共享库加载的模块, 这个属性的值就是共享库的路径. 

Custom classes
   Custom class types are typically created by class definitions (see section
   :ref:`class`).  A class has a namespace implemented by a dictionary object.
   Class attribute references are translated to lookups in this dictionary, e.g.,
   ``C.x`` is translated to ``C.__dict__["x"]`` (although there are a number of
   hooks which allow for other means of locating attributes). When the attribute
   name is not found there, the attribute search continues in the base classes.
   This search of the base classes uses the C3 method resolution order which
   behaves correctly even in the presence of 'diamond' inheritance structures
   where there are multiple inheritance paths leading back to a common ancestor.
   Additional details on the C3 MRO used by Python can be found in the
   documentation accompanying the 2.3 release at

   定制类类型. 定制类, 一般是由类定义创建的 (见 :ref:`class` ). 类用字典对象实现其名字空间, 对类属性的访问会转换成对该字典的查找, 例如 ``C.x`` 被解释成 ``C.__dict__[" x" ]`` (但也有许多钩子机制允许我们用其它方式访问属性). 当此查找没有找到属性时, 搜索会在基类中继续进行. 基类中的搜索方法使用C3方法解析顺序, 这种方法即便是多重继承里出现了公共祖先类的 "菱形" 结构也能保持正确行为. 关于Python使用的C3 MRO额外细节可以在 2.3 版本的附带文档中找到: 

   http://www.python.org/download/releases/2.3/mro/.

   .. XXX: Could we add that MRO doc as an appendix to the language ref?

   .. index::
      object: class
      object: class instance
      object: instance
      pair: class object; call
      single: container
      object: dictionary
      pair: class; attribute

   When a class attribute reference (for class :class:`C`, say) would yield a
   class method object, it is transformed into an instance method object whose
   :attr:`__self__` attributes is :class:`C`.  When it would yield a static
   method object, it is transformed into the object wrapped by the static method
   object. See section :ref:`descriptors` for another way in which attributes
   retrieved from a class may differ from those actually contained in its
   :attr:`__dict__`.

   当一个类 (假如是类 :class:`C` ) 的属性引用会产生类方法对象时, 它就会被转换成实例方法对象, 并将这个对象的 :attr:`__self__` 属性指向 :class:`C` . 当要产生静态方法对象时, 它会被转换成用静态方法对象包装的对象. 另一种获取与 :attr:`__dict__` 实际内容不同的属性的方法可以参考 :ref:`descriptors` . 

   .. index:: triple: class; attribute; assignment

   Class attribute assignments update the class's dictionary, never the dictionary
   of a base class.

   类属性的赋值会更新类的字典, 而不是基类的字典. 

   .. index:: pair: class object; call

   A class object can be called (see above) to yield a class instance (see below).

   一个类对象可以被调用 (如上所述) , 以产生一个类实例 (下述) . 

   .. index::
      single: __name__ (class attribute)
      single: __module__ (class attribute)
      single: __dict__ (class attribute)
      single: __bases__ (class attribute)
      single: __doc__ (class attribute)

   Special attributes: :attr:`__name__` is the class name; :attr:`__module__` is
   the module name in which the class was defined; :attr:`__dict__` is the
   dictionary containing the class's namespace; :attr:`__bases__` is a tuple
   (possibly empty or a singleton) containing the base classes, in the order of
   their occurrence in the base class list; :attr:`__doc__` is the class's
   documentation string, or None if undefined.

   特殊属性: :attr:`__name__` 是类名,  :attr:`__module__` 是类定义所在的模块名;  :attr:`__dict__` 是类的名字空间字典.  :attr:`__bases__` 是基类元组 (可能为空或独元) , 基类的顺序以定义时基类列表中的排列次序为准.  :attr:`__doc__` 是类的文档字符串或者 ``None`` . 

Class instances
   .. index::
      object: class instance
      object: instance
      pair: class; instance
      pair: class instance; attribute

   A class instance is created by calling a class object (see above).  A class
   instance has a namespace implemented as a dictionary which is the first place
   in which attribute references are searched.  When an attribute is not found
   there, and the instance's class has an attribute by that name, the search
   continues with the class attributes.  If a class attribute is found that is a
   user-defined function object, it is transformed into an instance method
   object whose :attr:`__self__` attribute is the instance.  Static method and
   class method objects are also transformed; see above under "Classes".  See
   section :ref:`descriptors` for another way in which attributes of a class
   retrieved via its instances may differ from the objects actually stored in
   the class's :attr:`__dict__`.  If no class attribute is found, and the
   object's class has a :meth:`__getattr__` method, that is called to satisfy
   the lookup.

   类实例是用类对象调用创建的. 类实例有一个用字典实现的名字空间, 它是进行属性搜索的第一个地方. 如果属性没在那找到, 但实例的类中有那个名字的属性, 就继续在类属性中查找. 如果找到的是一个用户定义函数对象, 它被转换成实例方法对象, 这个对象的 :attr:`__self__` 属性指向实例本身. 静态方法和类方法对象也会按上面 "Classes" 中的介绍那样进行转换. 另一种获取与 :attr:`__dict__` 实际内容不同的属性的方法可以参考 :ref:`descriptors` . 如果没有找到匹配的类属性, 但对象的类提供了 :meth:`__getattr__` 方法, 那么最后就会调用它完成属性搜索. 

   .. index:: triple: class instance; attribute; assignment

   Attribute assignments and deletions update the instance's dictionary, never a
   class's dictionary.  If the class has a :meth:`__setattr__` or
   :meth:`__delattr__` method, this is called instead of updating the instance
   dictionary directly.

   属性的赋值和删除会更新实例字典, 而不是类的字典. 如果类具有方法 :meth:`__setattr__` 或者 :meth:`__delattr__` 就会调用它们, 而不是直接更新字典. 

   .. index::
      object: numeric
      object: sequence
      object: mapping

   Class instances can pretend to be numbers, sequences, or mappings if they have
   methods with certain special names.  See section :ref:`specialnames`.

   如果提供了相应特别方法的定义, 类实例可以伪装成数值、有序类型或者映射类型, 参见 :ref:`specialnames` . 

   .. index::
      single: __dict__ (instance attribute)
      single: __class__ (instance attribute)

   Special attributes: :attr:`__dict__` is the attribute dictionary;
   :attr:`__class__` is the instance's class.

I/O objects (also known as file objects)
   .. index::
      builtin: open
      module: io
      single: popen() (in module os)
      single: makefile() (socket method)
      single: sys.stdin
      single: sys.stdout
      single: sys.stderr
      single: stdio
      single: stdin (in module sys)
      single: stdout (in module sys)
      single: stderr (in module sys)

   A :term:`file object` represents an open file.  Various shortcuts are
   available to create file objects: the :func:`open` built-in function, and
   also :func:`os.popen`, :func:`os.fdopen`, and the :meth:`makefile` method
   of socket objects (and perhaps by other functions or methods provided
   by extension modules).

   文件对象表示已经打开的文件. 创建文件对象有许多不同方法: 内置函数 :func:`open` 、 :func:`os.popen` 、 :func:`os.fdopen` 和 socket对象的 :meth:`makefile` 方法创建 (其它扩展模块的方法或函数也可以) . 
   
   The objects ``sys.stdin``, ``sys.stdout`` and ``sys.stderr`` are
   initialized to file objects corresponding to the interpreter's standard
   input, output and error streams; they are all open in text mode and
   therefore follow the interface defined by the :class:`io.TextIOBase`
   abstract class.

   对象 ``sys.stdin`` ,  ``sys.stdout``  和  ``sys.stderr`` 被初始化为解释器相应的标准输入流、标准输出流和标准错误输出流. 它们都以文本模式打开, 因此都遵循抽象类  :class:`io.TextIOBase` 定义的接口. 

Internal types
   .. index::
      single: internal type
      single: types, internal

   A few types used internally by the interpreter are exposed to the user. Their
   definitions may change with future versions of the interpreter, but they are
   mentioned here for completeness.

   有少量解释器内部使用的类型是用户可见的, 它们的定义可能会在未来版本中改变, 出于完整性的考虑这里也会提一下它们. 

   Code objects
      .. index::
         single: bytecode
         object: code

      Code objects represent *byte-compiled* executable Python code, or :term:`bytecode`.
      The difference between a code object and a function object is that the function
      object contains an explicit reference to the function's globals (the module in
      which it was defined), while a code object contains no context; also the default
      argument values are stored in the function object, not in the code object
      (because they represent values calculated at run-time).  Unlike function
      objects, code objects are immutable and contain no references (directly or
      indirectly) to mutable objects.

  代码对象表示 *字节编译* 过的可执行Python代码, 或者称为 :term:`bytecode` . 代码对象与函数对象的不同在于函数对象包含了函数全局变量的引用 (所在模块定义的) , 而代码对象不包括上下文. 默认参数值也保存在函数对象里, 而不在代码对象中 (因为它们表示的是运行时计算出来的值) . 不像函数对象, 代码对象是不可变的, 并且不包括对可变对象的 (直接或间接的) 引用. 

      .. index::
         single: co_argcount (code object attribute)
         single: co_code (code object attribute)
         single: co_consts (code object attribute)
         single: co_filename (code object attribute)
         single: co_firstlineno (code object attribute)
         single: co_flags (code object attribute)
         single: co_lnotab (code object attribute)
         single: co_name (code object attribute)
         single: co_names (code object attribute)
         single: co_nlocals (code object attribute)
         single: co_stacksize (code object attribute)
         single: co_varnames (code object attribute)
         single: co_cellvars (code object attribute)
         single: co_freevars (code object attribute)

      Special read-only attributes: :attr:`co_name` gives the function name;
      :attr:`co_argcount` is the number of positional arguments (including arguments
      with default values); :attr:`co_nlocals` is the number of local variables used
      by the function (including arguments); :attr:`co_varnames` is a tuple containing
      the names of the local variables (starting with the argument names);
      :attr:`co_cellvars` is a tuple containing the names of local variables that are
      referenced by nested functions; :attr:`co_freevars` is a tuple containing the
      names of free variables; :attr:`co_code` is a string representing the sequence
      of bytecode instructions; :attr:`co_consts` is a tuple containing the literals
      used by the bytecode; :attr:`co_names` is a tuple containing the names used by
      the bytecode; :attr:`co_filename` is the filename from which the code was
      compiled; :attr:`co_firstlineno` is the first line number of the function;
      :attr:`co_lnotab` is a string encoding the mapping from bytecode offsets to
      line numbers (for details see the source code of the interpreter);
      :attr:`co_stacksize` is the required stack size (including local variables);
      :attr:`co_flags` is an integer encoding a number of flags for the interpreter.

    只读特殊属性:  :attr:`co_name` 给出了函数名;  :attr:`co_argcount` 是位置参数的数目 (包括有默认值的参数) ;  :attr:`co_nlocals` 是函数使用的局部变量的数目 (包括参数) .  :attr:`co_varnames` 是一个包括局部变量名的元组 (从参数的名字开始) ;  :attr:`co_cellvars` 是一个元组, 包括由嵌套函数引用的局部变量名;  :attr:`co_freevals` 元组包括了自由变量的名字;  :attr:`co_code` 是字节编译后的指令序列的字符串表示;  :attr:`co_consts` 元组包括字节码中使用的字面值;  :attr:`co_names` 元组包括字节码中使用的名字;  :attr:`co_ﬁlename` 记录了字节码来自于什么文件;  :attr:`co_ﬁrstlineno` 是函数首行号;  :attr:`co_lnotab` 是一个字符串, 它表示从字节码偏移到行号的映射 (细节可以在解释器代码中找到) ;  :attr:`co_stacksize` 是需要的堆栈尺寸 (包括局部变量) ;  :attr:`co_ﬂags` 是一个表示解释器各种标志的整数. 

      .. index:: object: generator

      The following flag bits are defined for :attr:`co_flags`: bit ``0x04`` is set if
      the function uses the ``*arguments`` syntax to accept an arbitrary number of
      positional arguments; bit ``0x08`` is set if the function uses the
      ``**keywords`` syntax to accept arbitrary keyword arguments; bit ``0x20`` is set
      if the function is a generator.

      :attr:`co_ﬂags` 定义了如下标志位: 如果函数使用了 ``*arguments`` 语法接收任意数目的位置参数就会把 ``0x04`` 置位; 如果函数使用了 ``**keywords`` 语法接收任意数量的关键字参数, 就会把 ``0x08`` 置位. 如果函数是一个产生器 (generator) , 就会置位 ``0x20`` . 

      Future feature declarations (``from __future__ import division``) also use bits
      in :attr:`co_flags` to indicate whether a code object was compiled with a
      particular feature enabled: bit ``0x2000`` is set if the function was compiled
      with future division enabled; bits ``0x10`` and ``0x1000`` were used in earlier
      versions of Python.

       "Future功能声明"  ( ``from __future__ import division`` ) 也使用了 :attr:`co_flags` 的标志位指出代码对象在编译时是否打开某些特定功能: 如果函数是打开了future division编译的, 就会把 ``0x2000`` 置位; 之前版本的Python使用过位 ``0x10`` 和 ``0x1000`` . 

      Other bits in :attr:`co_flags` are reserved for internal use.

      :attr:`co_flags` 中其它位由解释器内部保留. 

      .. index:: single: documentation string

      If a code object represents a function, the first item in :attr:`co_consts` is
      the documentation string of the function, or ``None`` if undefined.

      如果代码对象表示的是函数, 那么 :attr:`co_consts` 的第一个项是函数的文档字符串, 或者为 ``None`` . 

.. _frame-objects:

   Frame objects
      .. index:: object: frame

      Frame objects represent execution frames.  They may occur in traceback objects
      (see below).

      栈桢对象表示执行时的栈桢, 它们会在回溯对象中出现 (下述) . 

      .. index::
         single: f_back (frame attribute)
         single: f_code (frame attribute)
         single: f_globals (frame attribute)
         single: f_locals (frame attribute)
         single: f_lasti (frame attribute)
         single: f_builtins (frame attribute)

      Special read-only attributes: :attr:`f_back` is to the previous stack frame
      (towards the caller), or ``None`` if this is the bottom stack frame;
      :attr:`f_code` is the code object being executed in this frame; :attr:`f_locals`
      is the dictionary used to look up local variables; :attr:`f_globals` is used for
      global variables; :attr:`f_builtins` is used for built-in (intrinsic) names;
      :attr:`f_lasti` gives the precise instruction (this is an index into the
      bytecode string of the code object).

      只读特殊属性: 属性 :attr:`f_back` 指向前一个栈桢 (朝着调用者的方向) , 如果位于堆栈底部它就是 ``None`` ; 属性 :attr:`f_code` 指向在这个栈桢结构上执行的代码对象. 属性 :attr:`f_locals` 是用于查找局部变量的字典; 属性 :attr:`f_globals` 字典用于查找全局变量; 属性 :attr:`f_builtins` 字典用于查找内置名字; 属性 :attr:`lasti` 以代码对象里指令字符串的索引的形式给出了精确的指令. 

      .. index::
         single: f_trace (frame attribute)
         single: f_lineno (frame attribute)

      Special writable attributes: :attr:`f_trace`, if not ``None``, is a function
      called at the start of each source code line (this is used by the debugger);
      :attr:`f_lineno` is the current line number of the frame --- writing to this
      from within a trace function jumps to the given line (only for the bottom-most
      frame).  A debugger can implement a Jump command (aka Set Next Statement)
      by writing to f_lineno.

      可写特殊属性: 属性 :attr:`f_trace` 如果不是 ``None`` , 就是这个栈桢所在函数的名称 (用于调试器) . 属性 :attr:`f_lineno` 是此栈帧当前行的行号, 在跟踪函数里如果写入这个属性, 可以使程序跳转到新行上 (只能用于最底部的栈桢) , 调试器可以这样实现跳转命令 (即 "指定下一步" 语句) . 

   Traceback objects
      .. index::
         object: traceback
         pair: stack; trace
         pair: exception; handler
         pair: execution; stack
         single: exc_info (in module sys)
         single: last_traceback (in module sys)
         single: sys.exc_info
         single: sys.last_traceback

      Traceback objects represent a stack trace of an exception.  A traceback object
      is created when an exception occurs.  When the search for an exception handler
      unwinds the execution stack, at each unwound level a traceback object is
      inserted in front of the current traceback.  When an exception handler is
      entered, the stack trace is made available to the program. (See section
      :ref:`try`.) It is accessible as the third item of the
      tuple returned by ``sys.exc_info()``. When the program contains no suitable
      handler, the stack trace is written (nicely formatted) to the standard error
      stream; if the interpreter is interactive, it is also made available to the user
      as ``sys.last_traceback``.

      回溯对象表示一个 "异常" 的栈回溯. 回溯对象会在发生异常时创建. 当我们在栈桢内搜索异常处理器时, 每当要搜索一个栈桢就会把一个回溯对象会插入到当前回溯对象的前面. 在进行异常处理器时, 回溯对象对程序也就可用了 (参见 :ref:`try` ) . 这些回溯对象可以通过 ``sys.exc_info()`` 返回元组的第三项访问. 当程序中没有适当的异常处理器, 回溯对象就被打印到标准错误输出上. 如果工作在交互模式上, 也可以通过 ``sys.last_traceback`` 访问. 

      .. index::
         single: tb_next (traceback attribute)
         single: tb_frame (traceback attribute)
         single: tb_lineno (traceback attribute)
         single: tb_lasti (traceback attribute)
         statement: try

      Special read-only attributes: :attr:`tb_next` is the next level in the stack
      trace (towards the frame where the exception occurred), or ``None`` if there is
      no next level; :attr:`tb_frame` points to the execution frame of the current
      level; :attr:`tb_lineno` gives the line number where the exception occurred;
      :attr:`tb_lasti` indicates the precise instruction.  The line number and last
      instruction in the traceback may differ from the line number of its frame object
      if the exception occurred in a :keyword:`try` statement with no matching except
      clause or with a finally clause.

      只读特殊属性:  :attr:`tb_text` 是堆栈回溯的下一级 (向着发生异常的那个栈桢) , 或者如果没有下一级就为 ``None`` . 属性 :attr:`tb_frame` 指向当前的栈桢对象; 属性 :attr:`tb_lineno` 给出发生异常的行号; 属性 :attr:`tb_lasti` 精确地指出对应的指令. 如果异常发生在没有匹配 :keyword:`except` 或 :keyword:`finally` 子句的 :keyword:`try` 语句中, 回溯对象中的行号和指令可能与栈桢对象中的行号和指令不同. 

   Slice objects
      .. index:: builtin: slice

      Slice objects are used to represent slices for :meth:`__getitem__`
      methods.  They are also created by the built-in :func:`slice` function.

      片断对象, 用于在 :meth:`__getitem__` 方法中表示片断信息, 也可以用内置函数 :func:`slice` 创建. 

      .. index::
         single: start (slice object attribute)
         single: stop (slice object attribute)
         single: step (slice object attribute)

      Special read-only attributes: :attr:`start` is the lower bound; :attr:`stop` is
      the upper bound; :attr:`step` is the step value; each is ``None`` if omitted.
      These attributes can have any type.

      只读特殊属性:  :attr:`start` 是下界;   :attr:`stop` 是上界;  :attr:`step` 是步长, 如果忽略任何一个, 就取 ``None`` 值. 这些属性可以是任意类型. 

      Slice objects support one method:

      片断对象支持一个方法: 

      .. method:: slice.indices(self, length)

         This method takes a single integer argument *length* and computes
         information about the slice that the slice object would describe if
         applied to a sequence of *length* items.  It returns a tuple of three
         integers; respectively these are the *start* and *stop* indices and the
         *step* or stride length of the slice. Missing or out-of-bounds indices
         are handled in a manner consistent with regular slices.

	 这个方法根据整数参数 *length* 判断片断对象是否能够描述 *length* 长的元素序列. 它返回一个包含三个整数的元组, 分别是索引 *start* 、 *stop* 和步长 *step* . 对于索引不足或者说越界的情况, 返回值提供的是片断对象中能够提供的最大 (最小) 边界索引. 

   Static method objects
      Static method objects provide a way of defeating the transformation of function
      objects to method objects described above. A static method object is a wrapper
      around any other object, usually a user-defined method object. When a static
      method object is retrieved from a class or a class instance, the object actually
      returned is the wrapped object, which is not subject to any further
      transformation. Static method objects are not themselves callable, although the
      objects they wrap usually are. Static method objects are created by the built-in
      :func:`staticmethod` constructor.

      静态方法对象. 这种对象提供一种可以绕过上面函数对象到方法对象转换的方法. 静态方法对象一般是其他对象的包装, 通常是用户定义方法. 当从一个类或者类实例获取静态方法对象时, 返回的对象通常是包装过的, 没有经过前面介绍的其他转换. 虽然它所包装的对象经常是可调用的, 但静态方法对象本身是不可调用的. 静态方法对象可以用内置函数 :func:`staticmethod` 创建. 

   Class method objects
      A class method object, like a static method object, is a wrapper around another
      object that alters the way in which that object is retrieved from classes and
      class instances. The behaviour of class method objects upon such retrieval is
      described above, under "User-defined methods". Class method objects are created
      by the built-in :func:`classmethod` constructor.

      类方法对象. 类似于静态方法对象, 也用来包装其他对象的. 是从类或者类实例获取对象的另一种候选方案. 获取对象的具体行为已经在 "用户定义方法" 中介绍过了. 类方法对象可以使用内置函数 :func:`classmethod` 创建. 

.. _specialnames:

特殊方法名 (Special method names) 
======================================

.. index::
   pair: operator; overloading
   single: __getitem__() (mapping object method)

A class can implement certain operations that are invoked by special syntax
(such as arithmetic operations or subscripting and slicing) by defining methods
with special names. This is Python's approach to :dfn:`operator overloading`,
allowing classes to define their own behavior with respect to language
operators.  For instance, if a class defines a method named :meth:`__getitem__`,
and ``x`` is an instance of this class, then ``x[i]`` is roughly equivalent
to ``type(x).__getitem__(x, i)``.  Except where mentioned, attempts to execute an
operation raise an exception when no appropriate method is defined (typically
:exc:`AttributeError` or :exc:`TypeError`).

通过定义特殊方法, 类能够实现特殊语法所调用的操作 (例如算术运算、下标及片断操作) . 这是Python方式的运算符重载 :dfn:`operator overloading` , 允许类能够针对语言运算符定义自己的行为. 例如, 某个类定义了方法 :meth:`__getitem__` , 并且 ``x`` 是这个类的实例, 那么 ``x[i]`` 就粗略等价于 ``type(x).__getitem__(x, i)`` . 除非特别标示, 在没有适当定义方法的类上执行操作会导致抛出异常, 一般是 :exc:`AttributeError` 或者 :exc:`TypeError` . 

When implementing a class that emulates any built-in type, it is important that
the emulation only be implemented to the degree that it makes sense for the
object being modelled.  For example, some sequences may work well with retrieval
of individual elements, but extracting a slice may not make sense.  (One example
of this is the :class:`NodeList` interface in the W3C's Document Object Model.)

在实现要模拟任意内置类型的类时, 需要特别指出的是 "模拟" 只是达到了满足使用的程度, 这点需要特别指出. 例如, 获取某些有序类型的单个元素是正常的, 但使用片断却是没有意义的 (一个例子是在W3C文档对象模型中的 :class:`NodeList` 接口. ) 

.. _customization:

基本定制 (Basic customization) 
---------------------------------------------------

.. method:: object.__new__(cls[, ...])

   .. index:: pair: subclassing; immutable types

   Called to create a new instance of class *cls*.  :meth:`__new__` is a static
   method (special-cased so you need not declare it as such) that takes the class
   of which an instance was requested as its first argument.  The remaining
   arguments are those passed to the object constructor expression (the call to the
   class).  The return value of :meth:`__new__` should be the new object instance
   (usually an instance of *cls*).

   用于创建类 *cls* 的新实例.  :meth:`__new__` 是静态方法 (但你并不需要显式地这样声明) , 它的第一个参数是新实例的类, 其余的参数就是传递给类构造器 (即类调用) 的那些参数.  :meth:`__new__` 的返回值应该是新对象实例 (一般来说是类 *cls* 的实例) . 

   Typical implementations create a new instance of the class by invoking the
   superclass's :meth:`__new__` method using ``super(currentclass,
   cls).__new__(cls[, ...])`` with appropriate arguments and then modifying the
   newly-created instance as necessary before returning it.

   这个方法的典型实现是用适当的参数通过 ``super(currentclass, cls).__new__(cls[, ...])`` 调用父类的 :meth:`__new__` 方法创建新实例, 在其基础上做可能的修改, 再返回之. 

   If :meth:`__new__` returns an instance of *cls*, then the new instance's
   :meth:`__init__` method will be invoked like ``__init__(self[, ...])``, where
   *self* is the new instance and the remaining arguments are the same as were
   passed to :meth:`__new__`.

   如果 :meth:`__new__` 返回了 *cls* 的一个实例, 之后会以 ``__init__(self[, ...])`` 的方式调用新实例的 :meth:`__init__` 方法, 其中 *self* 是新实例, 其余参数与传递给 :meth:`__new__` 的相同. 

   If :meth:`__new__` does not return an instance of *cls*, then the new instance's
   :meth:`__init__` method will not be invoked.

   如果 :meth:`__new__` 没有返回 *cls* 的实例, 就不会调用新实例的 :meth:`__init__` . 

   :meth:`__new__` is intended mainly to allow subclasses of immutable types (like
   int, str, or tuple) to customize instance creation.  It is also commonly
   overridden in custom metaclasses in order to customize class creation.

   引入 :meth:`__new__` 主要是为了允许对不可变类型 (如整数、字符串和元组) 的子类定制实例. 另外, 它通常也在元类 (metaclass) 定制化时被重载, 目的是定制类的创建. 

.. method:: object.__init__(self[, ...])

   .. index:: pair: class; constructor

   Called when the instance is created.  The arguments are those passed to the
   class constructor expression.  If a base class has an :meth:`__init__` method,
   the derived class's :meth:`__init__` method, if any, must explicitly call it to
   ensure proper initialization of the base class part of the instance; for
   example: ``BaseClass.__init__(self, [args...])``.  As a special constraint on
   constructors, no value may be returned; doing so will cause a :exc:`TypeError`
   to be raised at runtime.

   在创建新实例时调用. 参数与传递给类构造表达式的参数相同. 如果基类中定义了 :meth:`__init__` 方法, 那么必须显式地调用它以确保完成对实例基础部分的初始化. 例如,  ``BaseClass.__init__(self, [args...])`` . 作为一个构造时的特殊限制, 这个方法不会返回任何值, 否则会导致运行时抛出异常 :exc:`TypeError` . 

.. method:: object.__del__(self)

   .. index::
      single: destructor
      statement: del

   Called when the instance is about to be destroyed.  This is also called a
   destructor.  If a base class has a :meth:`__del__` method, the derived class's
   :meth:`__del__` method, if any, must explicitly call it to ensure proper
   deletion of the base class part of the instance.  Note that it is possible
   (though not recommended!) for the :meth:`__del__` method to postpone destruction
   of the instance by creating a new reference to it.  It may then be called at a
   later time when this new reference is deleted.  It is not guaranteed that
   :meth:`__del__` methods are called for objects that still exist when the
   interpreter exits.

   在实例要被释放 (destroy) 时被调用, 也称为析构器. 如果基类中也有 :meth:`__del__` 方法, 那么子类应该显式地调用它以确保正确删除实例的基础部分. 注意, 在 :meth:`__del__` 里可以创建本对象的新引用来达到推迟删除的目的, 但这并不是推荐做法.  :meth:`__del__` 方法在删除最后一个引用后不久调用. 但不能保证, 在解释器退出时所有存活对象的 :meth:`__del__` 方法都能被调用. 

   .. note::

      ``del x`` doesn't directly call ``x.__del__()`` --- the former decrements
      the reference count for ``x`` by one, and the latter is only called when
      ``x``'s reference count reaches zero.  Some common situations that may
      prevent the reference count of an object from going to zero include:
      circular references between objects (e.g., a doubly-linked list or a tree
      data structure with parent and child pointers); a reference to the object
      on the stack frame of a function that caught an exception (the traceback
      stored in ``sys.exc_info()[2]`` keeps the stack frame alive); or a
      reference to the object on the stack frame that raised an unhandled
      exception in interactive mode (the traceback stored in
      ``sys.last_traceback`` keeps the stack frame alive).  The first situation
      can only be remedied by explicitly breaking the cycles; the latter two
      situations can be resolved by storing ``None`` in ``sys.last_traceback``.
      Circular references which are garbage are detected when the option cycle
      detector is enabled (it's on by default), but can only be cleaned up if
      there are no Python- level :meth:`__del__` methods involved. Refer to the
      documentation for the :mod:`gc` module for more information about how
      :meth:`__del__` methods are handled by the cycle detector, particularly
      the description of the ``garbage`` value.

      ``del x`` 并不直接调用 ``x.__del__()`` ——— 前者将引用计数减一, 而后者只有在引用计数减到零时才被调用. 引用计数无法达到零的一些常见情况有: 对象之间的循环引用 (例如, 一个双链表或一个具有父子指针的树状数据结构) ; 对出现异常的函数的栈桢上对象的引用 ( ``sys.ext_info()[2]`` 中的回溯对象保证了栈桢不会被删除) ; 或者交互模式下出现未拦截异常的栈桢上的对象的引用 ( ``sys.last_traceback`` 中的回溯对象保证了栈桢不会被删除) . 第一种情况只有能通过地打破循环才能解决. 后两种情况, 可以通过将 ``sys.last_traceback`` 赋予 ``None`` 解决. 只有在打开循环检查器选项时 (这是默认的) , 循环引用才能被垃圾回收机制发现, 但前提是Python脚本中的 :meth:`__del__` 方法不要参与进来. 关于 :meth:`__del__` 与循环检查器是如何相互影响的详细信息, 可以参见 :mod:`gc` 模块的介绍, 尤其是其中的 ``garbage`` 值的描述. 

   .. warning::

      Due to the precarious circumstances under which :meth:`__del__` methods are
      invoked, exceptions that occur during their execution are ignored, and a warning
      is printed to ``sys.stderr`` instead.  Also, when :meth:`__del__` is invoked in
      response to a module being deleted (e.g., when execution of the program is
      done), other globals referenced by the :meth:`__del__` method may already have
      been deleted or in the process of being torn down (e.g. the import
      machinery shutting down).  For this reason, :meth:`__del__` methods
      should do the absolute
      minimum needed to maintain external invariants.  Starting with version 1.5,
      Python guarantees that globals whose name begins with a single underscore are
      deleted from their module before other globals are deleted; if no other
      references to such globals exist, this may help in assuring that imported
      modules are still available at the time when the :meth:`__del__` method is
      called.

      因为调用 :meth:`__del__` 方法时环境的不确定性, 它执行时产生的异常会被忽略掉, 只是在 ``sys.stderr`` 打印警告信息. 另外, 当因为删除模块而调用 :meth:`__del__` 方法时 (例如, 程序退出时) , 有些 :meth:`__del__` 所引用的全局名字可能已经删除了, 或者正在删除 (例如, 正在清理import关系) . 由于这些原因,  :meth:`__del__` 方法对外部不变式的要求应该保持最小. 从Python1.5开始, Python可以保证以单下划线开始的全局名字一定在其它全局名字之前从该模块中删除, 如果没有其它对这种全局名字的引用, 这个功能有助于保证导入的模块在调用 :meth:`__del__` 时还是有效的. 

.. method:: object.__repr__(self)

   .. index:: builtin: repr

   Called by the :func:`repr` built-in function to compute the "official" string
   representation of an object.  If at all possible, this should look like a
   valid Python expression that could be used to recreate an object with the
   same value (given an appropriate environment).  If this is not possible, a
   string of the form ``<...some useful description...>`` should be returned.
   The return value must be a string object. If a class defines :meth:`__repr__`
   but not :meth:`__str__`, then :meth:`__repr__` is also used when an
   "informal" string representation of instances of that class is required.

   使用内置函数 :func:`repr` 计算对象的 "正式" 字符串表示时会调用这个方法. 尽可能地, 结果应该是一个能够重建具有相同值的对象的有效Python表达式 (在适当环境下) . 如果这不可能, 也应该是返回一个形如 ``<... 一些有用的描述 ...>`` 的字符串. 返回值必须是一个字符串对象. 如果类定义了 :meth:`__repr__` 方法, 但没有定义 :meth:`__str__` , 那么 :meth:`__repr__` 也可以用于产生类实例的 "说明性 "字符串描述. 

   This is typically used for debugging, so it is important that the representation
   is information-rich and unambiguous.

   一般来说, 这通常用于调试, 所以描述字符串的信息丰富性和无歧义性是很重要的. 

.. method:: object.__str__(self)

   .. index::
      builtin: str
      builtin: print

   Called by the :func:`str` built-in function and by the :func:`print` function
   to compute the "informal" string representation of an object.  This differs
   from :meth:`__repr__` in that it does not have to be a valid Python
   expression: a more convenient or concise representation may be used instead.
   The return value must be a string object.

   由内置函数 :func:`str` 和 :func:`print` 调用, 用于计算一个对象的" 说明性" 字符串描述. 与 :meth:`__repr__` 不同, 这里并不要求一定是有效的Python表达式, 可以采用比较通俗简洁的表述方式. 返回值必须是一个字符串对象. 

   .. XXX what about subclasses of string?


.. method:: object.__format__(self, format_spec)

   .. index::
      pair: string; conversion
      builtin: str
      builtin: print

   Called by the :func:`format` built-in function (and by extension, the
   :meth:`format` method of class :class:`str`) to produce a "formatted"
   string representation of an object. The ``format_spec`` argument is
   a string that contains a description of the formatting options desired.
   The interpretation of the ``format_spec`` argument is up to the type
   implementing :meth:`__format__`, however most classes will either
   delegate formatting to one of the built-in types, or use a similar
   formatting option syntax.

   由内置函数 :func:`format` (和 :class:`str` 类的方法 :meth:`format` )调用, 用来构造对象的 "格式化" 字符串描述.  ``format_spec`` 参数是描述格式选项的字符串.  ``format_spec`` 的解释依赖于实现 :meth:`__format__` 的类型, 但一般来说, 大多数类要么把格式化任务委托 (转交) 给某个内置类型, 或者使用与内置类型类似的格式化选项. 

   See :ref:`formatspec` for a description of the standard formatting syntax.

   The return value must be a string object.

   返回值必须是字符串对象. 

.. _richcmpfuncs:
.. method:: object.__lt__(self, other)
            object.__le__(self, other)
            object.__eq__(self, other)
            object.__ne__(self, other)
            object.__gt__(self, other)
            object.__ge__(self, other)

   .. index::
      single: comparisons

   These are the so-called "rich comparison" methods. The correspondence between
   operator symbols and method names is as follows: ``x<y`` calls ``x.__lt__(y)``,
   ``x<=y`` calls ``x.__le__(y)``, ``x==y`` calls ``x.__eq__(y)``, ``x!=y`` calls
   ``x.__ne__(y)``, ``x>y`` calls ``x.__gt__(y)``, and ``x>=y`` calls
   ``x.__ge__(y)``.

   它们称为" 厚比较" 方法. 运算符与方法名的对应关系如下:  ``x<y`` 调用 ``x.__lt__(y)`` 、  ``x<=y`` 调用 ``x.__le__(y)`` 、 ``x==y`` 调用 ``x.__eq__(y)`` 、 ``x!=y`` 调用  ``x.__ne__(y)`` 、 ``x>y`` 调用 ``x.__gt__(y)`` 、 ``x>=y`` 调用 ``x.__ge__(y)`` . 

   A rich comparison method may return the singleton ``NotImplemented`` if it does
   not implement the operation for a given pair of arguments. By convention,
   ``False`` and ``True`` are returned for a successful comparison. However, these
   methods can return any value, so if the comparison operator is used in a Boolean
   context (e.g., in the condition of an ``if`` statement), Python will call
   :func:`bool` on the value to determine if the result is true or false.

   不是所有厚比较方法都要同时实现的, 如果个别厚比较方法没有实现, 可以直接返回 ``NotImplemented`` . 从习惯上讲, 一次成功的比较应该返回 ``False`` 或 ``True`` . 但是这些方法也可以返回任何值, 所以如果比较运算发生在布尔上下文中 (例如 ``if`` 语句中的条件测试) , Python会在返回值上调用函数 :func:`bool` 确定返回值的真值. 

   There are no implied relationships among the comparison operators. The truth
   of ``x==y`` does not imply that ``x!=y`` is false.  Accordingly, when
   defining :meth:`__eq__`, one should also define :meth:`__ne__` so that the
   operators will behave as expected.  See the paragraph on :meth:`__hash__` for
   some important notes on creating :term:`hashable` objects which support
   custom comparison operations and are usable as dictionary keys.

   在比较运算符之间并没有潜在的相互关系.  ``x==y`` 为真并不意味着 ``x!=y`` 为假. 因此, 如果定义了方法 :meth:`__eq__` , 那么也应该定义 :meth:`__ne__` , 这样才可以得到期望的效果. 关于如何创建可以作为字典键使用的 :term:`hashable` 对象, 还需要参考 :meth:`__hash__` 的介绍. 

   There are no swapped-argument versions of these methods (to be used when the
   left argument does not support the operation but the right argument does);
   rather, :meth:`__lt__` and :meth:`__gt__` are each other's reflection,
   :meth:`__le__` and :meth:`__ge__` are each other's reflection, and
   :meth:`__eq__` and :meth:`__ne__` are their own reflection.

   没有参数交换版本的方法定义 (这可以用于当左边参数不支持操作, 但右边参数支持的情况) .  :meth:`__lt__` 和 :meth:`__gt__` 相互反射 (即互为参数交换版本) ;   :meth:`__le__` 和 :meth:`__ge__` 相互反射;  :meth:`__eq__` 和 :meth:`__ne__` 相互反射. 

   Arguments to rich comparison methods are never coerced.

   传递给厚比较方法的参数不能是被自动强制类型转换的 (coerced) . 

   To automatically generate ordering operations from a single root operation,
   see :func:`functools.total_ordering`.
   
   关于如何从一个根操作自动生成顺序判定操作, 可以参考 :func:`functools.total_ordering` . 

.. method:: object.__hash__(self)

   .. index::
      object: dictionary
      builtin: hash

   Called by built-in function :func:`hash` and for operations on members of
   hashed collections including :class:`set`, :class:`frozenset`, and
   :class:`dict`.  :meth:`__hash__` should return an integer.  The only required
   property is that objects which compare equal have the same hash value; it is
   advised to somehow mix together (e.g. using exclusive or) the hash values for
   the components of the object that also play a part in comparison of objects.

   由内置函数 :func:`hash` , 或者是在可散列集合 (hashed collections, 包括 :class:`set` 、 :class:`frozenset` 和 :class:`dict` ) 成员上的操作调用. 这个方法应该返回一个整数. 只有一个要求, 具有相同值的对象应该有相同的散列值. 应该考虑以某种方式 (例如排斥或) 把在对象比较中起作用的部分与散列值关联起来. 

   If a class does not define an :meth:`__eq__` method it should not define a
   :meth:`__hash__` operation either; if it defines :meth:`__eq__` but not
   :meth:`__hash__`, its instances will not be usable as items in hashable
   collections.  If a class defines mutable objects and implements an
   :meth:`__eq__` method, it should not implement :meth:`__hash__`, since the
   implementation of hashable collections requires that a key's hash value is
   immutable (if the object's hash value changes, it will be in the wrong hash
   bucket).

   如果类没有定义 :meth:`__eq__` 方法, 那么它也不应该定义 :meth:`__hash__` 方法; 如果一个类只定义了 :meth:`__eq__` 方法, 那么它是不适合作散列键的. 如果可变对象实现了 :meth:`__eq__` 方法, 它也不应该实现 :meth:`__hash__` 方法, 因为可散列集合要求键值是不可变的 (如果对象的散列值发生了改变, 它会被放在错误的桶 (bucket) 中) . 

   User-defined classes have :meth:`__eq__` and :meth:`__hash__` methods
   by default; with them, all objects compare unequal (except with themselves)
   and ``x.__hash__()`` returns ``id(x)``.

   所有用户定义类默认都定义了方法 :meth:`__eq__` 和 :meth:`__hash__` , 这样, 所有对象都可以进行相等比较 (除了与自身比较) .   ``x.__hash__()`` 返回 ``id(x)`` . 

   Classes which inherit a :meth:`__hash__` method from a parent class but
   change the meaning of :meth:`__eq__` such that the hash value returned is no
   longer appropriate (e.g. by switching to a value-based concept of equality
   instead of the default identity based equality) can explicitly flag
   themselves as being unhashable by setting ``__hash__ = None`` in the class
   definition. Doing so means that not only will instances of the class raise an
   appropriate :exc:`TypeError` when a program attempts to retrieve their hash
   value, but they will also be correctly identified as unhashable when checking
   ``isinstance(obj, collections.Hashable)`` (unlike classes which define their
   own :meth:`__hash__` to explicitly raise :exc:`TypeError`).

   如果子类从父类继承了方法 :meth:`__hash__` , 但修改了 :meth:`__eq__` , 这时子类继承的散列值就不再正确了 (例如, 可能从默认的标识相等的比较切换成了值相等的比较) , 这时在在类定义时显式地将 :meth:`__hash__` 设置成 ``None`` 就行了. 这样, 在使用这个子类对象作为散列键时就会抛出 :exc:`TypeError` 异常, 或者也可以使用常规的可散列检查 ``isinstance(obj, collections.Hashable)`` 确定它的不可散列性 (使用这种检查方式时, 这种达到不可散列的方法与在 :meth:`__hash__` 中显式抛出异常的方法是的计算结果就不一样了) . 

   If a class that overrides :meth:`__eq__` needs to retain the implementation
   of :meth:`__hash__` from a parent class, the interpreter must be told this
   explicitly by setting ``__hash__ = <ParentClass>.__hash__``. Otherwise the
   inheritance of :meth:`__hash__` will be blocked, just as if :attr:`__hash__`
   had been explicitly set to :const:`None`.

   如果子类修改了 :meth:`__eq__` 方法, 但需要保留父类的 :meth:`__hash__` , 它必须显式地告诉解释器 ``__hash__ = <ParentClass>.__hash__`` , 否则 :meth:`__hash__` 的继承会被阻止, 就像设置 :attr:`__hash__` 为 ``None`` . 

.. method:: object.__bool__(self)

   .. index:: single: __len__() (mapping object method)

   Called to implement truth value testing and the built-in operation
   ``bool()``; should return ``False`` or ``True``.  When this method is not
   defined, :meth:`__len__` is called, if it is defined, and the object is
   considered true if its result is nonzero.  If a class defines neither
   :meth:`__len__` nor :meth:`__bool__`, all its instances are considered
   true.

   在实现真值测试和内置操作 ``bool()``  中调用, 应该返回 ``False`` 和 ``True`` . 如果没有这个定义方法, 转而使用 :meth:`__len__` . 非零返回值, 当作 "真" . 如果这两个方法都没有定义, 就认为该实例为 "真" . 

.. _attribute-access:

Customizing attribute access
----------------------------

The following methods can be defined to customize the meaning of attribute
access (use of, assignment to, or deletion of ``x.name``) for class instances.

以下方法可以用于定制访问类实例属性的含义 (例如, 赋值、或删除 ``x.name`` ) 

.. XXX explain how descriptors interfere here!


.. method:: object.__getattr__(self, name)

   Called when an attribute lookup has not found the attribute in the usual places
   (i.e. it is not an instance attribute nor is it found in the class tree for
   ``self``).  ``name`` is the attribute name. This method should return the
   (computed) attribute value or raise an :exc:`AttributeError` exception.

   在正常方式访问属性无法成功时 (就是说, self属性既不是实例的, 在类树结构中找不到) 使用.  ``name`` 是属性名. 应该返回一个计算好的属性值, 或抛出一个 :exc:`AttributeError` 异常. 

   Note that if the attribute is found through the normal mechanism,
   :meth:`__getattr__` is not called.  (This is an intentional asymmetry between
   :meth:`__getattr__` and :meth:`__setattr__`.) This is done both for efficiency
   reasons and because otherwise :meth:`__getattr__` would have no way to access
   other attributes of the instance.  Note that at least for instance variables,
   you can fake total control by not inserting any values in the instance attribute
   dictionary (but instead inserting them in another object).  See the
   :meth:`__getattribute__` method below for a way to actually get total control
   over attribute access.

   注意如果属性可以通过正常方法访问,  :meth:`__getattr__` 是不会被调用的 (是有意将 :meth:`__getattr__` 和 :meth:`__setattr__` 设计成不对称的) . 这样做的原因是基于效率的考虑, 并且这样也不会让 :meth:`__getattr__` 干涉正常属性. 注意, 至少对于类实例而言, 不必非要更新实例字典伪装属性 (但可以将它们插入到其它对象中) . 需要全面控制属性访问, 可以参考以下 :meth:`__getattribute__` 的介绍. 

.. method:: object.__getattribute__(self, name)

   Called unconditionally to implement attribute accesses for instances of the
   class. If the class also defines :meth:`__getattr__`, the latter will not be
   called unless :meth:`__getattribute__` either calls it explicitly or raises an
   :exc:`AttributeError`. This method should return the (computed) attribute value
   or raise an :exc:`AttributeError` exception. In order to avoid infinite
   recursion in this method, its implementation should always call the base class
   method with the same name to access any attributes it needs, for example,
   ``object.__getattribute__(self, name)``.

   在访问类实例的属性时无条件调用这个方法. 如果类也定义了方法 :meth:`__getattr__` , 那么除非 :meth:`__getattribute__` 显式地调用了它, 或者抛出了 :exc:`AttributeError` 异常, 否则它就不会被调用. 这个方法应该返回一个计算好的属性值, 或者抛出异常  :exc:`AttributeError`  . 为了避免无穷递归, 对于任何它需要访问的属性, 这个方法应该调用基类的同名方法, 例如,  ``object.__getattribute__(self, name)`` . 
   .. note::

      This method may still be bypassed when looking up special methods as the
      result of implicit invocation via language syntax or built-in functions.
      See :ref:`special-lookup`.

      但是, 通过特定语法或者内置函数, 做隐式调用搜索特殊方法时, 这个方法可能会被跳过, 参见 :ref:`special-lookup` . 

.. method:: object.__setattr__(self, name, value)

   Called when an attribute assignment is attempted.  This is called instead of
   the normal mechanism (i.e. store the value in the instance dictionary).
   *name* is the attribute name, *value* is the value to be assigned to it.

   在属性要被赋值时调用. 这会替代正常机制 (即把值保存在实例字典中) .  *name* 是属性名,  *vaule* 是要赋的值. 

   If :meth:`__setattr__` wants to assign to an instance attribute, it should
   call the base class method with the same name, for example,
   ``object.__setattr__(self, name, value)``.

   如果在 :meth:`__setattr__` 里要对一个实例属性赋值, 它应该调用父类的同名方法, 例如,  ``object.__setattr__(self, name, value)`` . 

.. method:: object.__delattr__(self, name)

   Like :meth:`__setattr__` but for attribute deletion instead of assignment.  This
   should only be implemented if ``del obj.name`` is meaningful for the object.

   与 :meth:`__setattr__` 类似, 但它的功能是删除属性. 当 ``del obj.name`` 对对象有意义时, 才需要实现它. 

.. method:: object.__dir__(self)

   Called when :func:`dir` is called on the object.  A list must be returned.

   在对象上调用 :func:`dir` 时调用, 它需要返回一个列表. 

.. _descriptors:

Implementing Descriptors
^^^^^^^^^^^^^^^^^^^^^^^^

The following methods only apply when an instance of the class containing the
method (a so-called *descriptor* class) appears in the class dictionary of
another class, known as the *owner* class.  In the examples below, "the
attribute" refers to the attribute whose name is the key of the property in the
owner class' :attr:`__dict__`.

以下方法只能使用在 "描述符类" 中,  "描述子类" 的实例出现在其他类 (称为 "所有者类" ) 的类字典中, 而这个类包括以下方法. 在下面的例子里,  "属性" 专指在所有者类字典中的属性.   [译注: 注意是出现所有类的字典中, 不是所有类的实例的字典. ] 

.. method:: object.__get__(self, instance, owner)

   Called to get the attribute of the owner class (class attribute access) or of an
   instance of that class (instance attribute access). *owner* is always the owner
   class, while *instance* is the instance that the attribute was accessed through,
   or ``None`` when the attribute is accessed through the *owner*.  This method
   should return the (computed) attribute value or raise an :exc:`AttributeError`
   exception.

   在获取所有者类属性或实例属性时调用这个方法.  *owner* 是所有者类,  *instance* 用于访问的所有者类的实例, 如果是通过 ``owner`` 访问的话, 这个参数为 ``None`` . 这个方法应该返回一个计算好的属性值, 或者抛出异常 :exc:`AttributeError` . 

.. method:: object.__set__(self, instance, value)

   Called to set the attribute on an instance *instance* of the owner class to a
   new value, *value*.

   在给所有者类的一个实例 *instance* 设置属性时调用这个方法,  *value* 代表新值. 

.. method:: object.__delete__(self, instance)

   Called to delete the attribute on an instance *instance* of the owner class.

   删除所有者类实例 *instance* 的属性时调用这个方法. 

.. _descriptor-invocation:

调用描述符 (Invoking Descriptors) 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In general, a descriptor is an object attribute with "binding behavior", one
whose attribute access has been overridden by methods in the descriptor
protocol:  :meth:`__get__`, :meth:`__set__`, and :meth:`__delete__`. If any of
those methods are defined for an object, it is said to be a descriptor.

一般来说, 描述符就是一个有 "绑定行为" 的对象属性, 这种属性的访问操作会以描述符协议的方式替代, 即方法 :meth:`__get__` 、 :meth:`__set__` 和 :meth:`__delete__` . 如果一个对象定义了任何以上方法之一, 就称它为 "描述符" . 

The default behavior for attribute access is to get, set, or delete the
attribute from an object's dictionary. For instance, ``a.x`` has a lookup chain
starting with ``a.__dict__['x']``, then ``type(a).__dict__['x']``, and
continuing through the base classes of ``type(a)`` excluding metaclasses.

属性访问的默认行为是从对象字典中获取、设置、删除. 例如,  ``a.x`` 会在导致以下的搜索链: 先 ``a.__dict__['x']`` 后 ``type(a).__dict__['x']`` , 之后再从 ``type(a)`` 的父类中搜索, 但不搜索元类 (metaclass) . 

However, if the looked-up value is an object defining one of the descriptor
methods, then Python may override the default behavior and invoke the descriptor
method instead.  Where this occurs in the precedence chain depends on which
descriptor methods were defined and how they were called.

但是, 如果搜索的是一个定义了描述符方法的对象, Python会放弃默认方案转而调用描述符方法. 调用在以上搜索链上的位置取决于定义了什么描述符方法及调用方式的. 

The starting point for descriptor invocation is a binding, ``a.x``. How the
arguments are assembled depends on ``a``:

描述符调用始于 "绑定" ,  ``a.x`` , 方法参数的组织取决于 ``a`` : 

Direct Call
   The simplest and least common call is when user code directly invokes a
   descriptor method:    ``x.__get__(a)``.

   直接调用. 这是最简单但也是最不常用的方法, 用户直接调用一个描述符方法, 例如 ``x.__get__(a)`` . 

Instance Binding
   If binding to an object instance, ``a.x`` is transformed into the call:
   ``type(a).__dict__['x'].__get__(a, type(a))``.

   实例绑定. 如果与实例绑定,  ``a.x`` 会转换为以下调用:   ``type(a).__dict__['x'].__get__(a, type(a))`` . 

Class Binding
   If binding to a class, ``A.x`` is transformed into the call:
   ``A.__dict__['x'].__get__(None, A)``.

   类绑定. 如果与类绑定,  ``A.x`` 会转换为以下调用:  ``A.__dict__['x'].__get__(None, A)`` . 

Super Binding
   If ``a`` is an instance of :class:`super`, then the binding ``super(B,
   obj).m()`` searches ``obj.__class__.__mro__`` for the base class ``A``
   immediately preceding ``B`` and then invokes the descriptor with the call:
   ``A.__dict__['m'].__get__(obj, A)``.

   超级绑定. 如果 ``a`` 是类 :class:`super` 的一个实例, 那么绑定 ``super(B, obj).m()`` 会在 ``obj.__class__.__mro__`` 里直接搜索基类 ``A`` , 而不是先搜索  ``B`` , 并调用 ``A.__dict__['m'].__get__(obj, A)`` . 

For instance bindings, the precedence of descriptor invocation depends on the
which descriptor methods are defined.  A descriptor can define any combination
of :meth:`__get__`, :meth:`__set__` and :meth:`__delete__`.  If it does not
define :meth:`__get__`, then accessing the attribute will return the descriptor
object itself unless there is a value in the object's instance dictionary.  If
the descriptor defines :meth:`__set__` and/or :meth:`__delete__`, it is a data
descriptor; if it defines neither, it is a non-data descriptor.  Normally, data
descriptors define both :meth:`__get__` and :meth:`__set__`, while non-data
descriptors have just the :meth:`__get__` method.  Data descriptors with
:meth:`__set__` and :meth:`__get__` defined always override a redefinition in an
instance dictionary.  In contrast, non-data descriptors can be overridden by
instances.

对于实例绑定, 描述符调用的优先顺序依赖于定义了什么描述符方法. 描述符可以定义 :meth:`__get__` 、 :meth:`__set__` 和 :meth:`__delete__` 的一个任意组合. 如果它没有定义 :meth:`__get__` , 并且在对象实例的字典中没有这个值, 访问该属性就直接会返回描述符本身. 如果描述符定义了 :meth:`__set__` 和 (或)  :meth:`__delete__` , 它就是一个数据描述符. 如果两者都没有定义, 它就是非数据描述符. 正常情况下, 数据描述符会定义两个方法 :meth:`__get__` 和 :meth:`__set__` , 而非数据描述符只会定义 :meth:`__get__` . ; 定义了 :meth:`__set__` and :meth:`__get__` 数据描述符会覆盖实例的字典, 相比之下, 实例字典会反之覆盖掉非数据描述符. 

Python methods (including :func:`staticmethod` and :func:`classmethod`) are
implemented as non-data descriptors.  Accordingly, instances can redefine and
override methods.  This allows individual instances to acquire behaviors that
differ from other instances of the same class.

Python方法 (包括 :func:`staticmethod` 和 :func:`classmethod` ) 是以非数据描述符实现的. 因此, 实例可以重新定义或者说覆盖方法. 这允许同一个类的不同实例可以有不同的行为. 

The :func:`property` function is implemented as a data descriptor. Accordingly,
instances cannot override the behavior of a property.

函数 :func:`property` 是用数据描述符实现的, 因此, 实例不能覆写特性 (property) 的行为. 

.. _slots:

__slots__
^^^^^^^^^

By default, instances of classes have a dictionary for attribute storage.  This
wastes space for objects having very few instance variables.  The space
consumption can become acute when creating large numbers of instances.

默认情况下, 类实例使用字典管理属性. 在对象只有少量实例变量时这就会占用不少空间, 当有大量实例时, 空间消耗会变得更为严重. 

The default can be overridden by defining *__slots__* in a class definition.
The *__slots__* declaration takes a sequence of instance variables and reserves
just enough space in each instance to hold a value for each variable.  Space is
saved because *__dict__* is not created for each instance.

这个默认行为可以通过在类定义中定义 *__slots__* 修改.  *__slots__* 声明只为该类的所有实例预留刚刚够用的空间. 因为不会为每个实例创建 *__dict__* , 因此空间节省下来了. 

.. data:: object.__slots__

   This class variable can be assigned a string, iterable, or sequence of
   strings with variable names used by instances.  If defined in a
   class, *__slots__* reserves space for the declared variables and prevents the
   automatic creation of *__dict__* and *__weakref__* for each instance.

   这个类变量可以赋值为一个字符串, 一个可迭代对象, 或者一个字符串序列 (每个字符串表示实例所用的变量名) . 如果定义了 *__slots__* , Python就会为实例预留出存储声明变量的空间, 并且不会为每个实例自动创建 *__dict__* 和 *__weakref__* . 

使用 *__slots__* 的注意事项 (Notes on using *__slots__* ) 
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

* When inheriting from a class without *__slots__*, the *__dict__* attribute of
  that class will always be accessible, so a *__slots__* definition in the
  subclass is meaningless.

  如果从一个没有定义 *__slots__* 的基类继承, 子类一定存在 *__dict__* 属性. 所以, 在这种子类中定义 *__slots__* 是没有意义的. 

* Without a *__dict__* variable, instances cannot be assigned new variables not
  listed in the *__slots__* definition.  Attempts to assign to an unlisted
  variable name raises :exc:`AttributeError`. If dynamic assignment of new
  variables is desired, then add ``'__dict__'`` to the sequence of strings in
  the *__slots__* declaration.

  没有定义 *__dict__* 的实例不支持对不在 *__slot__* 中的属性赋值. 如果需要支持这个功能, 可以把 ``'__dict__'`` 放到 *__slots__* 声明中. 

* Without a *__weakref__* variable for each instance, classes defining
  *__slots__* do not support weak references to its instances. If weak reference
  support is needed, then add ``'__weakref__'`` to the sequence of strings in the
  *__slots__* declaration.

  没有定义 *__weakref__* 的, 使用 *__slot__* 的实例不支持对它的 "弱引用" . 如果需要支持弱引用, 可以把 ``'__weakref__'`` 放到 *__slots__* 声明中. 

* *__slots__* are implemented at the class level by creating descriptors
  (:ref:`descriptors`) for each variable name.  As a result, class attributes
  cannot be used to set default values for instance variables defined by
  *__slots__*; otherwise, the class attribute would overwrite the descriptor
  assignment.

  *__slots__* 是在类这一级实现的, 通过为每个实例创建描述符 ( :ref:`descriptors` ) . 因此, 不能使用类属性为实例的 *__slots__* 中定义的属性设置默认值 . 否则, 类属性会覆盖描述符的赋值操作. 

* The action of a *__slots__* declaration is limited to the class where it is
  defined.  As a result, subclasses will have a *__dict__* unless they also define
  *__slots__* (which must only contain names of any *additional* slots).

  *__slots__* 声明的行为只限于其定义所在的类. 因此, 如果子类没有定义自己的 *__slots__*  (它必须只包括那些 *额外* 的 slots ) , 子类仍然会使用 *__dict__* . 

* If a class defines a slot also defined in a base class, the instance variable
  defined by the base class slot is inaccessible (except by retrieving its
  descriptor directly from the base class). This renders the meaning of the
  program undefined.  In the future, a check may be added to prevent this.

* Nonempty *__slots__* does not work for classes derived from "variable-length"
  built-in types such as :class:`int`, :class:`str` and :class:`tuple`.

* Any non-string iterable may be assigned to *__slots__*. Mappings may also be
  used; however, in the future, special meaning may be assigned to the values
  corresponding to each key.

  任何非字符串可迭代对象都可以赋给 *__slots__* . 映射类型也是允许的, 但是, 以后版本的Python可能给 "键" 赋予特殊意义. 

* *__class__* assignment works only if both classes have the same *__slots__*.

  只有在两个类的 *__slots__* 相同时,  *__class__* 赋值才会正常工作. 

.. _metaclasses:

类创建的定制 (Customizing class creation) 
------------------------------------------------

By default, classes are constructed using :func:`type`. A class definition is
read into a separate namespace and the value of class name is bound to the
result of ``type(name, bases, dict)``.

默认情况下, 类是由 :func:`type` 构造的. 类的定义会读入一个独立的名字空间, 并且类名称的值会与  ``type(name, bases, dict)`` 的返回结果绑定. 

When the class definition is read, if a callable ``metaclass`` keyword argument
is passed after the bases in the class definition, the callable given will be
called instead of :func:`type`.  If other keyword arguments are passed, they
will also be passed to the metaclass.  This allows classes or functions to be
written which monitor or alter the class creation process:

在读取类定义时, 如果在基类名之后给出了可调用类型的关键字参数 ``metaclass``  (元类) , 这时就不再调用 :func:`type` 转而使用这个可调用对象. 如果有其它关键字参数, 它们也会传递给 ``metaclass`` . 这就允许我们使用类或者函数来监视或修改类创建过程: 

* Modifying the class dictionary prior to the class being created.

  在类创建之前修改类的字典. 

* Returning an instance of another class -- essentially performing the role of a
  factory function.

  返回其它类的实例 -- 基本上这里执行的就是工厂方法. 

These steps will have to be performed in the metaclass's :meth:`__new__` method
-- :meth:`type.__new__` can then be called from this method to create a class
with different properties.  This example adds a new element to the class
dictionary before creating the class:

以上步骤必须在元类的 :meth:`__new__` 方法内完成 -- 然后从这个方法中调用 :meth:`type.__new__` 创建具有不同特性 (properties) 的新类.  下面的例子会在创建类之前在类字典中增加一个新元素 ::

  class metacls(type):
      def __new__(mcs, name, bases, dict):
          dict['foo'] = 'metacls was here'
          return type.__new__(mcs, name, bases, dict)

You can of course also override other class methods (or add new methods); for
example defining a custom :meth:`__call__` method in the metaclass allows custom
behavior when the class is called, e.g. not always creating a new instance.

你当然也可以覆盖其它类方法 (或是增加新方法) , 例如, 在元类中定制 :meth:`__call__` 方法就可以控制类在调用时的行为, 例如, 不会每次调用都返回一个实例. 

If the metaclass has a :meth:`__prepare__` attribute (usually implemented as a
class or static method), it is called before the class body is evaluated with
the name of the class and a tuple of its bases for arguments.  It should return
an object that supports the mapping interface that will be used to store the
namespace of the class.  The default is a plain dictionary.  This could be used,
for example, to keep track of the order that class attributes are declared in by
returning an ordered dictionary.

如果元类具有 :meth:`__prepare__` 属性 (一般是用类或者静态方法实现的) , 它会在对类定义体 (结合类名、父类和参数) 估值之前调用. 这个方法应该返回一个支持映射接口的对象, 这个对象用于存储类的名字空间. 默认是一个普通字典. 例如, 可以通过返回一个有序字典跟踪类属性的声明顺序. 

The appropriate metaclass is determined by the following precedence rules:

使用的元类是按以下优先顺序确定的: 

* If the ``metaclass`` keyword argument is passed with the bases, it is used.

  如果在基类位置上使用了 ``metaclass`` 关键字参数, 就使用它. 

* Otherwise, if there is at least one base class, its metaclass is used.

  否则, 如果至少有一个基类, 就用基类的元类. 

* Otherwise, the default metaclass (:class:`type`) is used.

  否则, 使用默认元类 :class:`type` . 

The potential uses for metaclasses are boundless. Some ideas that have been
explored including logging, interface checking, automatic delegation, automatic
property creation, proxies, frameworks, and automatic resource
locking/synchronization.

元类的用途非常广泛, 目前已知的用法有记录日志、接口检查、自动委托、特性 (property) 自动创建、代理、框架、自动资源锁定及同步. 

Here is an example of a metaclass that uses an :class:`collections.OrderedDict`
to remember the order that class members were defined:

这里是一个使用 :class:`collections.OrderedDict` 的元类例子, 它可以记住类成员的定义顺序 ::

    class OrderedClass(type):

         @classmethod
         def __prepare__(metacls, name, bases, **kwds):
            return collections.OrderedDict()

         def __new__(cls, name, bases, classdict):
            result = type.__new__(cls, name, bases, dict(classdict))
            result.members = tuple(classdict)
            return result

    class A(metaclass=OrderedClass):
        def one(self): pass
        def two(self): pass
        def three(self): pass
        def four(self): pass

    >>> A.members
    ('__module__', 'one', 'two', 'three', 'four')

When the class definition for *A* gets executed, the process begins with
calling the metaclass's :meth:`__prepare__` method which returns an empty
:class:`collections.OrderedDict`.  That mapping records the methods and
attributes of *A* as they are defined within the body of the class statement.
Once those definitions are executed, the ordered dictionary is fully populated
and the metaclass's :meth:`__new__` method gets invoked.  That method builds
the new type and it saves the ordered dictionary keys in an attribute
called ``members``.

在类定义 *A* 执行时, 进程开始于调用元类的 :meth:`__prepare__` 方法并返回一个空 :class:`collections.OrderedDict` , 这个映射会记录在 ``class`` 语句中定义的 *A* 的方法和属性. 一旦执行完这个定义, 有序字典就完全设置好了, 并调用元类的 :meth:`__new__` 方法, 这个方法会创建新的类型, 并把这个有序字典的键保存在属性 *members* 中. 

对实例和子类检查的定制 (Customizing instance and subclass checks) 
-------------------------------------------------------------------------------------

The following methods are used to override the default behavior of the
:func:`isinstance` and :func:`issubclass` built-in functions.

以下函数可以用于定制内置函数 :func:`isinstance` 和 :func:`issubclass` 的默认行为. 

In particular, the metaclass :class:`abc.ABCMeta` implements these methods in
order to allow the addition of Abstract Base Classes (ABCs) as "virtual base
classes" to any class or type (including built-in types), including other
ABCs.

具体地, 元类 :class:`abc.ABCMeta` 实现了这些方法, 以允许把抽象基类 (ABC) 作为任何类或者类型 (包括内置类型) , 也包括其他ABC的" 虚拟基类 ". 

.. method:: class.__instancecheck__(self, instance)

   Return true if *instance* should be considered a (direct or indirect)
   instance of *class*. If defined, called to implement ``isinstance(instance,
   class)``.

　　如果 *instance* 应该被看作 *class* 的一个直接或者间接的实例, 就返回真. 如果定义了这个方法, 它就会被用于实现 ``isinstance(instance, class)`` . 

.. method:: class.__subclasscheck__(self, subclass)

   Return true if *subclass* should be considered a (direct or indirect)
   subclass of *class*.  If defined, called to implement ``issubclass(subclass,
   class)``.

   如果 *subclass* 应该被看作是 *class* 的一个直接或者间接的基类, 就返回真. 如果定义了这个方法, 它就会被用于实现 ``issubclass(subclass, class)`` . 

Note that these methods are looked up on the type (metaclass) of a class.  They
cannot be defined as class methods in the actual class.  This is consistent with
the lookup of special methods that are called on instances, only in this
case the instance is itself a class.

注意, 这个方法会在一个类的类型 (元类) 中查找. 它们不能作为一个类方法在类中定义. 这个机制, 与在实例上查找要调用的特殊方法的机制是一致的, 因为在这个情况下实例本身就是一个类. 

.. seealso::

   :pep:`3119` - Introducing Abstract Base Classes
      Includes the specification for customizing :func:`isinstance` and
      :func:`issubclass` behavior through :meth:`__instancecheck__` and
      :meth:`__subclasscheck__`, with motivation for this functionality in the
      context of adding Abstract Base Classes (see the :mod:`abc` module) to the
      language.

包括定制通过 :meth:`__instancecheck__` 和 :meth:`__subclasscheck__`　定制 :func:`isinstance` 和 :func:`issubclass` 行为的规范, 以及在语言上增加抽象基类 (见 :mod:`abc` 模块) 这个背景设置这个功能的动机. 

.. _callable-types:

Emulating callable objects
--------------------------


.. method:: object.__call__(self[, args...])

   .. index:: pair: call; instance

   Called when the instance is "called" as a function; if this method is defined,
   ``x(arg1, arg2, ...)`` is a shorthand for ``x.__call__(arg1, arg2, ...)``.

   当实例作为函数使用时调用本方法. 如果定义了这个方法,  ``x(arg1, arg2, ...)`` 就相当于 ``x.__call__(arg1, arg2, ...)`` . 

.. _sequence-types:

模拟容器对象 (Emulating container types) 
---------------------------------------------------------------

The following methods can be defined to implement container objects.  Containers
usually are sequences (such as lists or tuples) or mappings (like dictionaries),
but can represent other containers as well.  The first set of methods is used
either to emulate a sequence or to emulate a mapping; the difference is that for
a sequence, the allowable keys should be the integers *k* for which ``0 <= k <
N`` where *N* is the length of the sequence, or slice objects, which define a
range of items.  It is also recommended that mappings provide the methods
:meth:`keys`, :meth:`values`, :meth:`items`, :meth:`get`, :meth:`clear`,
:meth:`setdefault`, :meth:`pop`, :meth:`popitem`, :meth:`copy`, and
:meth:`update` behaving similar to those for Python's standard dictionary
objects.  The :mod:`collections` module provides a :class:`MutableMapping`
abstract base class to help create those methods from a base set of
:meth:`__getitem__`, :meth:`__setitem__`, :meth:`__delitem__`, and :meth:`keys`.
Mutable sequences should provide methods :meth:`append`, :meth:`count`,
:meth:`index`, :meth:`extend`, :meth:`insert`, :meth:`pop`, :meth:`remove`,
:meth:`reverse` and :meth:`sort`, like Python standard list objects.  Finally,
sequence types should implement addition (meaning concatenation) and
multiplication (meaning repetition) by defining the methods :meth:`__add__`,
:meth:`__radd__`, :meth:`__iadd__`, :meth:`__mul__`, :meth:`__rmul__` and
:meth:`__imul__` described below; they should not define other numerical
operators.  It is recommended that both mappings and sequences implement the
:meth:`__contains__` method to allow efficient use of the ``in`` operator; for
mappings, ``in`` should search the mapping's keys; for sequences, it should
search through the values.  It is further recommended that both mappings and
sequences implement the :meth:`__iter__` method to allow efficient iteration
through the container; for mappings, :meth:`__iter__` should be the same as
:meth:`keys`; for sequences, it should iterate through the values.

通过定义以下方法可以实现容器对象. 容器通常指有序类型 (如列表或元组) 或映射类型 (如字典) , 不过也可以表示其它容器. 第一个方法集用于模拟有序类型或映射: 有序类型的区别在于, 键只能是整数 *k* ( ``0 <= k < N`` ,  *N* 是有序类型的长度) 或者是定义了处在一个范围内的项的片断对象. 另外, 也推荐模拟映射类型时实现方法 :meth:`keys` 、 :meth:`values` 、 :meth:`items` 、 :meth:`get` 、 :meth:`clear` 、 :meth:`setdefault` 、 :meth:`pop` 、 :meth:`popitem` 、 :meth:`copy` 和 :meth:`update` , 这使得模拟出来的行为与Python标准字典对象类似. 模块 :mod:`collections` 提供了 :class:`MutableMapping` 抽象基类可用于帮助从基本方法 :meth:`__getitem__` 、 :meth:`__setitem__` 、 :meth:`__delitem__` 和 :meth:`keys` 中创建这些方法. 可变有序类型应该提供方法 :meth:`append` 、 :meth:`count` 、 :meth:`index` 、  :meth:`extend` 、 :meth:`insert` 、 :meth:`pop` 、 :meth:`remove` 、 :meth:`reverse` 和 :meth:`sort` , 就像Python标准列表对象那样. 最后, 有序类型应该用下述的方法  :meth:`__add__` 、 :meth:`__radd__` 、 :meth:`__iadd__` 、 :meth:`__mul__` 、 :meth:`__rmul__` 和 
:meth:`__imul__` 实现 "加" 操作 (即连接) 和 "乘" 操作 (即重复) . 它们也可以实现其它算术运算符. 推荐映射和有序类型实现 :meth:`__contains__` 方法以实现 ``in`` 操作符的有效使用, 对于映射类型,  ``in`` 应该搜索映射的键, 对于有序类型, 它应该搜索值 . 进一步的建议是映射和有序类型实现 :meth:`__iter__` 方法, 以支持在容器中有效地进行迭代. 对于映射,  :meth:`__iter__` 应该与 :meth:`keys` 相同, 对于有序类型, 它应该在值之间迭代. 

.. method:: object.__len__(self)

   .. index::
      builtin: len
      single: __bool__() (object method)

   Called to implement the built-in function :func:`len`.  Should return the length
   of the object, an integer ``>=`` 0.  Also, an object that doesn't define a
   :meth:`__bool__` method and whose :meth:`__len__` method returns zero is
   considered to be false in a Boolean context.

   实现内置函数 :func:`len` 相仿的功能. 应该返回对象的长度, 一个大于等于0的整数. 另外, 如果对象没有定义 :meth:`__bool__` 方法, 那么在布尔上下文中,  :meth:`__len__` 返回的 *0* 将被看作是 "假" . 

.. note::

   Slicing is done exclusively with the following three methods.  A call like ::

      a[1:2] = b

   is translated to ::

      a[slice(1, 2, None)] = b

   and so forth.  Missing slice items are always filled in with ``None``.

   以此类推. 缺少的片断项会被替代为 ``None`` . 

.. method:: object.__getitem__(self, key)

   .. index:: object: slice

   Called to implement evaluation of ``self[key]``. For sequence types, the
   accepted keys should be integers and slice objects.  Note that the special
   interpretation of negative indexes (if the class wishes to emulate a sequence
   type) is up to the :meth:`__getitem__` method. If *key* is of an inappropriate
   type, :exc:`TypeError` may be raised; if of a value outside the set of indexes
   for the sequence (after any special interpretation of negative values),
   :exc:`IndexError` should be raised. For mapping types, if *key* is missing (not
   in the container), :exc:`KeyError` should be raised.

   用于实现 ``self[key]`` . 对于有序类型, 可接受的 *key* 应该有整数和片断对象. 注意对负数索引 (如果类希望模拟有序类型) 的特殊解释也依赖于 :meth:`__getitem__` 方法. 如果 *key* 的类型不合适, 可以抛出异常 :exc:`TypeError` . 如果 *key* 的值在有序类型的索引范围之外 (在任何负值索引的特殊解释也行不通的情况下) , 可以抛出 :exc:`IndexError` 异常. 对于映射类型, 如果没有给出 *key*  (或者不在容器之内) , 应该抛出异常 :exc:`KeyError` . 
   .. note::

      :keyword:`for` loops expect that an :exc:`IndexError` will be raised for illegal
      indexes to allow proper detection of the end of the sequence.

      :keyword:`for` 循环根据由于无效索引导致的 :exc:`IndexError` 异常检测有序类型的结尾. 

.. method:: object.__setitem__(self, key, value)

   Called to implement assignment to ``self[key]``.  Same note as for
   :meth:`__getitem__`.  This should only be implemented for mappings if the
   objects support changes to the values for keys, or if new keys can be added, or
   for sequences if elements can be replaced.  The same exceptions should be raised
   for improper *key* values as for the :meth:`__getitem__` method.

   在对 ``self[key]`` 赋值时调用. 与 :meth:`__getitem__` 有着相同的注意事项. 如果映射类型对象要支持改变键的值或者增加新键, 或者有序类型要支持可替换元素时, 应该实现这个方法. 在使用无效的 *key* 值时, 会抛出与 :meth:`__getitem__` 相同的异常. 

.. method:: object.__delitem__(self, key)

   Called to implement deletion of ``self[key]``.  Same note as for
   :meth:`__getitem__`.  This should only be implemented for mappings if the
   objects support removal of keys, or for sequences if elements can be removed
   from the sequence.  The same exceptions should be raised for improper *key*
   values as for the :meth:`__getitem__` method.

   在删除 ``self[key]`` 时调用, 与 :meth:`__getitem__` 有相同的注意事项. 如果映射对象要支持删除键, 或者有序类型对象要支持元素的删除, 就应该实现这个方法. 在使用无效的 *key* 值时, 会抛出与 :meth:`__getitem__` 相同的异常. 

.. method:: object.__iter__(self)

   This method is called when an iterator is required for a container. This method
   should return a new iterator object that can iterate over all the objects in the
   container.  For mappings, it should iterate over the keys of the container, and
   should also be made available as the method :meth:`keys`.

   在使用容器的迭代器时会调用这个方法. 本方法应该返回一个可以遍历容器内所有对象的迭代器对象. 对于映射类型, 应该在容器的键上迭代, 并且也应该定义 :meth:`keys` 方法. 

   Iterator objects also need to implement this method; they are required to return
   themselves.  For more information on iterator objects, see :ref:`typeiter`.

   迭代器对象也需要实现这个方法, 它们应该返回它自身. 关于迭代器对象的更多信息, 可以参考  :ref:`typeiter` . 

.. method:: object.__reversed__(self)

   Called (if present) by the :func:`reversed` built-in to implement
   reverse iteration.  It should return a new iterator object that iterates
   over all the objects in the container in reverse order.

   在使用内置函数 :func:`reversed` 实现反向迭代时调用这个方法. 它应该返回一个以相反顺序在容器内迭代所有对象的新迭代器对象. 

   If the :meth:`__reversed__` method is not provided, the :func:`reversed`
   built-in will fall back to using the sequence protocol (:meth:`__len__` and
   :meth:`__getitem__`).  Objects that support the sequence protocol should
   only provide :meth:`__reversed__` if they can provide an implementation
   that is more efficient than the one provided by :func:`reversed`.

   如果没有定义方法 :meth:`__reversed__` , 内置函数 :func:`reversed` 会切换到备用方案: 使用序列协议 ( :meth:`__len__` 和  :meth:`__getitem__` ) . 支持序列协议的对象只在有更高效的  :func:`reversed` 实现方法时, 才有必要实现  :meth:`__reversed__` . 


The membership test operators (:keyword:`in` and :keyword:`not in`) are normally
implemented as an iteration through a sequence.  However, container objects can
supply the following special method with a more efficient implementation, which
also does not require the object be a sequence.

成员测试运算符 ( :keyword:`in` 和 :keyword:`not in` ) 一般是在对有序类型进行迭代实现的. 但是容器也可以实现方法提供更高效率的实现, 并不要求对象一定是有序类型. 

.. method:: object.__contains__(self, item)

   Called to implement membership test operators.  Should return true if *item*
   is in *self*, false otherwise.  For mapping objects, this should consider the
   keys of the mapping rather than the values or the key-item pairs.

   For objects that don't define :meth:`__contains__`, the membership test first
   tries iteration via :meth:`__iter__`, then the old sequence iteration
   protocol via :meth:`__getitem__`, see :ref:`this section in the language
   reference <membership-test-details>`.
 
   在成员测试时调用这个方法. 如果 *item* 在 *self* 之内应该返回真, 否则返回假. 对于映射类型的对象, 测试应该是针对键的, 而不是值, 或者键值对. 

   没有定义 :meth:`__contains__` 方法的对象, 在进行成员测试时首先尝试使用 :meth:`__iter__` 方法进行迭代, 然后尝试使用旧式的有序对象迭代协议 :meth:`__getitem__` , 参见 :ref:`这里的讨论<membership-test-details>`.

.. _numeric-types:

模拟数值类型 (Emulating numeric types) 
------------------------------------------------------------

The following methods can be defined to emulate numeric objects. Methods
corresponding to operations that are not supported by the particular kind of
number implemented (e.g., bitwise operations for non-integral numbers) should be
left undefined.

以下方法用于模拟数值类型. 不同数值类型所支持的操作符并不完全相同, 如果一个类型不支持某些操作符 (例如非整数值上的位运算) , 对应的方法就不应该被实现. 

.. method:: object.__add__(self, other)
            object.__sub__(self, other)
            object.__mul__(self, other)
            object.__truediv__(self, other)
            object.__floordiv__(self, other)
            object.__mod__(self, other)
            object.__divmod__(self, other)
            object.__pow__(self, other[, modulo])
            object.__lshift__(self, other)
            object.__rshift__(self, other)
            object.__and__(self, other)
            object.__xor__(self, other)
            object.__or__(self, other)

   .. index::
      builtin: divmod
      builtin: pow
      builtin: pow

   These methods are called to implement the binary arithmetic operations (``+``,
   ``-``, ``*``, ``/``, ``//``, ``%``, :func:`divmod`, :func:`pow`, ``**``, ``<<``,
   ``>>``, ``&``, ``^``, ``|``).  For instance, to evaluate the expression
   ``x + y``, where *x* is an instance of a class that has an :meth:`__add__`
   method, ``x.__add__(y)`` is called.  The :meth:`__divmod__` method should be the
   equivalent to using :meth:`__floordiv__` and :meth:`__mod__`; it should not be
   related to :meth:`__truediv__`.  Note that :meth:`__pow__` should be defined
   to accept an optional third argument if the ternary version of the built-in
   :func:`pow` function is to be supported.

   这些方法用于二元算术操作 ( ``+`` 、 ``-`` 、 ``*`` 、 ``/`` 、 ``//`` 、 ``%`` ,  :func:`divmod` 、 :func:`pow` 、 ``**`` 、 ``<<`` 、 ``>>`` 、 ``&`` 、 ``^`` 、 ``|`` ) . 例如, 在计算表达式 ``x + y`` 时,  *x* 是一个定义了 :meth:`__add__` 的类的实例, 那么就会调用  ``x.__add__(y)`` . 方法 :meth:`__divmod__` 应该与使用 :meth:`__floordiv__` 和 :meth:`__mod__` 的结果相同, 但应该与 :meth:`__truediv__` 无关. 注意如果要支持三参数版本的内置函数 :func:`pow` 的话, 方法 :meth:`__pow__` 应该被定义成可以接受第三个可选参数的. 

   If one of those methods does not support the operation with the supplied
   arguments, it should return ``NotImplemented``.

   如果以上任一方法无法处理根据参数完成计算的话, 就应该返回 ``NotImplemented`` . 

.. method:: object.__radd__(self, other)
            object.__rsub__(self, other)
            object.__rmul__(self, other)
            object.__rtruediv__(self, other)
            object.__rfloordiv__(self, other)
            object.__rmod__(self, other)
            object.__rdivmod__(self, other)
            object.__rpow__(self, other)
            object.__rlshift__(self, other)
            object.__rrshift__(self, other)
            object.__rand__(self, other)
            object.__rxor__(self, other)
            object.__ror__(self, other)

   .. index::
      builtin: divmod
      builtin: pow

   These methods are called to implement the binary arithmetic operations (``+``,
   ``-``, ``*``, ``/``, ``//``, ``%``, :func:`divmod`, :func:`pow`, ``**``,
   ``<<``, ``>>``, ``&``, ``^``, ``|``) with reflected (swapped) operands.
   These functions are only called if the left operand does not support the
   corresponding operation and the operands are of different types. [#]_  For
   instance, to evaluate the expression ``x - y``, where *y* is an instance of
   a class that has an :meth:`__rsub__` method, ``y.__rsub__(x)`` is called if
   ``x.__sub__(y)`` returns *NotImplemented*.

   这些方法用于实现二元算术操作 (``+`` 、 ``-`` 、 ``*`` 、 ``/`` 、 ``//`` 、 ``%`` 、 :func:`divmod` 、 :func:`pow` 、 ``**`` 、  ``<<`` 、 ``>>`` 、 ``&`` 、 ``^`` 、 ``|`` ) , 但用于操作数反射 (即参数顺序是相反的) . 这些函数只有在左操作数不支持相应操作, 并且是参数是不同类型时才会被使用. [3] 例如, 计算表达式 ``x - y`` ,  *y* 是一个定义了方法 :meth:`__rsub__` 的类实例, 那么在 ``x.__sub__(y)`` 返回 *NotImplemented* 时才会调用 ``y.__rsub__(x)`` . 
   
   .. index:: builtin: pow

   Note that ternary :func:`pow` will not try calling :meth:`__rpow__` (the
   coercion rules would become too complicated).

   注意三参数版本的 :func:`pow` 不会试图调用 :meth:`__rpow__` .  (这会导致类型自动转换规则过于复杂) 

   .. note::

      If the right operand's type is a subclass of the left operand's type and that
      subclass provides the reflected method for the operation, this method will be
      called before the left operand's non-reflected method.  This behavior allows
      subclasses to override their ancestors' operations.

      注意: 如果右操作数的类型是左操作数的一个子类, 并且这个子类提供了操作数反射版本的方法. 那么, 子类的操作数反射方法将在左操作数的非反射方法之前调用. 这个行为允许了子类可以覆盖祖先类的操作. 

.. method:: object.__iadd__(self, other)
            object.__isub__(self, other)
            object.__imul__(self, other)
            object.__itruediv__(self, other)
            object.__ifloordiv__(self, other)
            object.__imod__(self, other)
            object.__ipow__(self, other[, modulo])
            object.__ilshift__(self, other)
            object.__irshift__(self, other)
            object.__iand__(self, other)
            object.__ixor__(self, other)
            object.__ior__(self, other)

   These methods are called to implement the augmented arithmetic assignments
   (``+=``, ``-=``, ``*=``, ``/=``, ``//=``, ``%=``, ``**=``, ``<<=``, ``>>=``,
   ``&=``, ``^=``, ``|=``).  These methods should attempt to do the operation
   in-place (modifying *self*) and return the result (which could be, but does
   not have to be, *self*).  If a specific method is not defined, the augmented
   assignment falls back to the normal methods.  For instance, to execute the
   statement ``x += y``, where *x* is an instance of a class that has an
   :meth:`__iadd__` method, ``x.__iadd__(y)`` is called.  If *x* is an instance
   of a class that does not define a :meth:`__iadd__` method, ``x.__add__(y)``
   and ``y.__radd__(x)`` are considered, as with the evaluation of ``x + y``.

   这些方法用于实现参数化算术赋值操作 ( ``+=`` 、 ``-=`` 、 ``*=`` 、 ``/=`` 、 ``//=`` 、 ``%=`` 、 ``**=`` 、 ``<<=`` 、 ``>>=`` 、  ``&=`` 、 ``^=`` 、 ``|=``)) . 这些方法应该是就地操作的 (即直接修改 *self* ) 并返回结果 (一般来讲, 这里应该是对 *self* 直接操作, 但并不是一定要求如此) . 如果没有实现某个对应方法的话, 参数化赋值会蜕化为正常方法. 例如, 执行语句 ``x += y`` 时,  *x* 是一个实现了 :meth:`__iadd__` 方法的实例,  ``x.__iadd__(y)`` 就会被调用. 如果 *x* 没有定义 :meth:`__iadd__` , 就会选择 ``x.__add__(y)`` 或者  ``y.__radd__(x)`` , 与 ``x + y`` 类似. 

.. method:: object.__neg__(self)
            object.__pos__(self)
            object.__abs__(self)
            object.__invert__(self)

   .. index:: builtin: abs

   Called to implement the unary arithmetic operations (``-``, ``+``, :func:`abs`
   and ``~``).

   用于实现一元算术操作 ( ``-`` 、 ``+`` 、 :func:`abs` 和 ``~`` ) . 

.. method:: object.__complex__(self)
            object.__int__(self)
            object.__float__(self)
            object.__round__(self, [,n])

   .. index::
      builtin: complex
      builtin: int
      builtin: float
      builtin: round

   Called to implement the built-in functions :func:`complex`,
   :func:`int`, :func:`float` and :func:`round`.  Should return a value
   of the appropriate type.

   用于实现内置函数 :func:`complex` 、 :func:`int` 、 :func:`float` 和 :func:`round` . 应该返回对应的类型值. 

.. method:: object.__index__(self)

   Called to implement :func:`operator.index`.  Also called whenever Python needs
   an integer object (such as in slicing, or in the built-in :func:`bin`,
   :func:`hex` and :func:`oct` functions). Must return an integer.

   用于实现函数 :func:`operator.index` , 或者在Python需要一个整数对象时调用 (例如在分片时 (slicing) , 或者在内置函数函数 :func:`bin` 、  :func:`hex` 和 :func:`oct` 中) . 这个方法必须返回一个整数. 

.. _context-managers:

with语句的上下文管理器 (With Statement Context Managers)
----------------------------------------------------------------------

A :dfn:`context manager` is an object that defines the runtime context to be
established when executing a :keyword:`with` statement. The context manager
handles the entry into, and the exit from, the desired runtime context for the
execution of the block of code.  Context managers are normally invoked using the
:keyword:`with` statement (described in section :ref:`with`), but can also be
used by directly invoking their methods.

上下文管理器 ( :dfn:`context manager` ) 是一个对象, 这个对象定义了执行 :keyword:`with` 语句时要建立的运行时上下文. 上下文管理器负责处理执行某代码块时对应的运行时上下文进入和退出. 运行时上下文的使用一般通过 :keyword:`with` 语句 (参见 :ref:`with` ) , 但也可以直接调用它的方法. 

.. index::
   statement: with
   single: context manager

Typical uses of context managers include saving and restoring various kinds of
global state, locking and unlocking resources, closing opened files, etc.

上下文管理器的典型用途包括保存和恢复各种全局状态, 锁定和解锁资源, 关闭打开的文件等等. 

For more information on context managers, see :ref:`typecontextmanager`.

关于上下文管理的更多信息, 可以参考 :ref:`typecontextmanager` . 

.. method:: object.__enter__(self)

   Enter the runtime context related to this object. The :keyword:`with` statement
   will bind this method's return value to the target(s) specified in the
   :keyword:`as` clause of the statement, if any.

   进入与这个对象关联的运行时上下文.  :keyword:`with` 语句会把这个方法的返回值与 :keyword:`as` 子句指定的目标绑定在一起 (如果指定了的话) . 

.. method:: object.__exit__(self, exc_type, exc_value, traceback)

   Exit the runtime context related to this object. The parameters describe the
   exception that caused the context to be exited. If the context was exited
   without an exception, all three arguments will be :const:`None`.

   退出与这个对象相关的运行时上下文. 参数描述了导致上下文退出的异常, 如果是无异常退出, 则这三个参数都为 :const:`None` . 

   If an exception is supplied, and the method wishes to suppress the exception
   (i.e., prevent it from being propagated), it should return a true value.
   Otherwise, the exception will be processed normally upon exit from this method.

   如果给出了一个异常, 而这个方法决定要压制它 (即防止它把传播出去) , 那么它应该返回真. 否则, 在退出这个方法时, 这个异常会按正常方式处理. 

   Note that :meth:`__exit__` methods should not reraise the passed-in exception;
   this is the caller's responsibility.

   注意方法 :meth:`__exit__` 不应该把传入的异常重新抛出, 这是调用者的责任. 

.. seealso::

   :pep:`0343` - The "with" statement
      The specification, background, and examples for the Python :keyword:`with`
      statement.

      Python :keyword:`with` 语句的规范、背景和例子. 

.. _special-lookup:

搜索特殊方法 (Special method lookup) 
--------------------------------------------------------

For custom classes, implicit invocations of special methods are only guaranteed
to work correctly if defined on an object's type, not in the object's instance
dictionary.  That behaviour is the reason why the following code raises an
exception:

对于定制类, 只有在对象类型的字典里定义好, 才能保证成功调用特殊方法. 这是以下代码发生异常的原因::

   >>> class C:
   ...     pass
   ...
   >>> c = C()
   >>> c.__len__ = lambda: 5
   >>> len(c)
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: object of type 'C' has no len()

The rationale behind this behaviour lies with a number of special methods such
as :meth:`__hash__` and :meth:`__repr__` that are implemented by all objects,
including type objects. If the implicit lookup of these methods used the
conventional lookup process, they would fail when invoked on the type object
itself:

这个行为的原因在于, 有不少特殊方法在所有对象中都得到了实现, 例如 :meth:`__hash__` 和 :meth:`__repr__` . 如果按照常规的搜索过程搜索这些方法, 在涉及到类型对象时就会出错::

   >>> 1 .__hash__() == hash(1)
   True
   >>> int.__hash__() == hash(int)
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: descriptor '__hash__' of 'int' object needs an argument

Incorrectly attempting to invoke an unbound method of a class in this way is
sometimes referred to as 'metaclass confusion', and is avoided by bypassing
the instance when looking up special methods:

试图以这种错误方式调用一个类的未绑定方法有时叫作 "元类含混" , 可以通过在搜索特殊方法时跳过实例避免::

   >>> type(1).__hash__(1) == hash(1)
   True
   >>> type(int).__hash__(int) == hash(int)
   True

In addition to bypassing any instance attributes in the interest of
correctness, implicit special method lookup generally also bypasses the
:meth:`__getattribute__` method even of the object's metaclass:

除了因为正确性的原因而跳过实例属外之外, 特殊方法搜索也会跳过 :meth:`__getattribute__` 方法, 甚至是元类中的::

   >>> class Meta(type):
   ...    def __getattribute__(*args):
   ...       print("Metaclass getattribute invoked")
   ...       return type.__getattribute__(*args)
   ...
   >>> class C(object, metaclass=Meta):
   ...     def __len__(self):
   ...         return 10
   ...     def __getattribute__(*args):
   ...         print("Class getattribute invoked")
   ...         return object.__getattribute__(*args)
   ...
   >>> c = C()
   >>> c.__len__()                 # Explicit lookup via instance
   Class getattribute invoked
   10
   >>> type(c).__len__(c)          # Explicit lookup via type
   Metaclass getattribute invoked
   10
   >>> len(c)                      # Implicit lookup
   10

Bypassing the :meth:`__getattribute__` machinery in this fashion
provides significant scope for speed optimisations within the
interpreter, at the cost of some flexibility in the handling of
special methods (the special method *must* be set on the class
object itself in order to be consistently invoked by the interpreter).

这种跳过 :meth:`__getattribute__` 的机制为解释器的时间性能优化提供了充分的余地, 代价是牺牲了处理特殊方法时的部分灵活性 (为了保持与解释器的一致, 特殊方法必须在类对象中定义) . 

.. rubric:: Footnotes

.. [#] It *is* possible in some cases to change an object's type, under certain
   controlled conditions. It generally isn't a good idea though, since it can
   lead to some very strange behaviour if it is handled incorrectly.

   是在某些受控制的条件下, 修改对象类型是有可能的. 但一般这不是个好做法, 因为一旦处理不周, 就有可能导致一些非常奇怪的行为. 

.. [#] For operands of the same type, it is assumed that if the non-reflected method
   (such as :meth:`__add__`) fails the operation is not supported, which is why the
   reflected method is not called.

   对于相同类型的操作数, 假定如果非反射方法失败就意味着并不支持这个运算符. 这就是没有调用反射方法的原因. 

