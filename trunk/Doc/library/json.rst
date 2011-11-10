:mod:`json` --- JSON 解码编码
========================================

.. module:: json
   :synopsis: Encode and decode the JSON format.
.. moduleauthor:: Bob Ippolito <bob@redivi.com>
.. sectionauthor:: Bob Ippolito <bob@redivi.com>

..模块:: JSON
    :简介: JSON格式的编码与解码. 

JSON (JavaScript Object Notation) <http://json.org> is a subset of JavaScript
syntax (ECMA-262 3rd edition) used as a lightweight data interchange format.

JSON (JavaScript对象符号) <http://json.org>是JavaScript语法 (ECMA - 262第三版) 的子集, 
常用于轻量级的数据交换格式. 

:mod:`json` exposes an API familiar to users of the standard library
:mod:`marshal` and :mod:`pickle` modules.

:mod:`json`开放一个用户熟悉的API标准库, 分别是:mod:`marshal`和:mod:`pickle`模块.

Encoding basic Python object hierarchies:

编码的基本Python对象层次::

    >>> import json
    >>> json.dumps(['foo', {'bar': ('baz', None, 1.0, 2)}])
    '["foo", {"bar": ["baz", null, 1.0, 2]}]'
    >>> print(json.dumps("\"foo\bar"))
    "\"foo\bar"
    >>> print(json.dumps('\u1234'))
    "\u1234"
    >>> print(json.dumps('\\'))
    "\\"
    >>> print(json.dumps({"c": 0, "b": 0, "a": 0}, sort_keys=True))
    {"a": 0, "b": 0, "c": 0}
    >>> from io import StringIO
    >>> io = StringIO()
    >>> json.dump(['streaming API'], io)
    >>> io.getvalue()
    '["streaming API"]'

Compact encoding:

压缩编码::

    >>> import json
    >>> json.dumps([1,2,3,{'4': 5, '6': 7}], separators=(',',':'))
    '[1,2,3,{"4":5,"6":7}]'

Pretty printing:

漂亮的打印::

    >>> import json
    >>> print(json.dumps({'4': 5, '6': 7}, sort_keys=True, indent=4))
    {
        "4": 5,
        "6": 7
    }

Decoding JSON:

解码JSON::

    >>> import json
    >>> json.loads('["foo", {"bar":["baz", null, 1.0, 2]}]')
    ['foo', {'bar': ['baz', None, 1.0, 2]}]
    >>> json.loads('"\\"foo\\bar"')
    '"foo\x08ar'
    >>> from io import StringIO
    >>> io = StringIO('["streaming API"]')
    >>> json.load(io)
    ['streaming API']

Specializing JSON object decoding:

特殊化的JSON对象编码::

    >>> import json
    >>> def as_complex(dct):
    ...     if '__complex__' in dct:
    ...         return complex(dct['real'], dct['imag'])
    ...     return dct
    ...
    >>> json.loads('{"__complex__": true, "real": 1, "imag": 2}',
    ...     object_hook=as_complex)
    (1+2j)
    >>> import decimal
    >>> json.loads('1.1', parse_float=decimal.Decimal)
    Decimal('1.1')

Extending :class:`JSONEncoder`:

扩展的`JSONEncoder`类::

    >>> import json
    >>> class ComplexEncoder(json.JSONEncoder):
    ...     def default(self, obj):
    ...         if isinstance(obj, complex):
    ...             return [obj.real, obj.imag]
    ...         return json.JSONEncoder.default(self, obj)
    ...
    >>> json.dumps(2 + 1j, cls=ComplexEncoder)
    '[2.0, 1.0]'
    >>> ComplexEncoder().encode(2 + 1j)
    '[2.0, 1.0]'
    >>> list(ComplexEncoder().iterencode(2 + 1j))
    ['[2.0', ', 1.0', ']']


.. highlight:: none

Using json.tool from the shell to validate and pretty-print:

在shell里使用json.tool验证及漂亮的打印::

    $ echo '{"json":"obj"}' | python -mjson.tool
    {
        "json": "obj"
    }
    $ echo '{ 1.2:3.4}' | python -mjson.tool
    Expecting property name: line 1 column 2 (char 2)
    
    预期属性的名称: 第1行第2列 (第2个字符) 

.. highlight:: python

.. note::


   The JSON produced by this module's default settings is a subset of
   YAML, so it may be used as a serializer for that as well.

   这个模块的默认设置所产生的JSON是YAML的子集, 因此可能比较合适用于序列化. 


 基本用法
--------------------------------------------

.. function:: dump(obj, fp, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None, indent=None, separators=None, default=None, **kw)

   Serialize *obj* as a JSON formatted stream to *fp* (a ``.write()``-supporting
   file-like object).

   序列化*obj*作为JSON格式的数据流*fp* (例如 ``.write()``-支持类文件对象) 

   If *skipkeys* is ``True`` (default: ``False``), then dict keys that are not
   of a basic type (:class:`str`, :class:`int`, :class:`float`, :class:`bool`,
   ``None``) will be skipped instead of raising a :exc:`TypeError`.

   如果*skipkeys*是``True`` (默认: ``False``) , 那么字典键值不是
   基本类型 (如 :class:`str`, :class:`int`, :class:`float`, :class:`bool`,
   ``None``) 将被忽略, 而不是抛出:exc:`TypeError`异常. 

   The :mod:`json` module always produces :class:`str` objects, not
   :class:`bytes` objects. Therefore, ``fp.write()`` must support :class:`str`
   input.

   :mod:`json`模块总是生成 :class:`str`对象, 而不是:class:`bytes`对象. 因此, ``fp.write()``必须支持 :class:`str`输入. 

   If *check_circular* is ``False`` (default: ``True``), then the circular
   reference check for container types will be skipped and a circular reference
   will result in an :exc:`OverflowError` (or worse).

   如果*check_circular* 是``False`` (默认: ``True``) , 那么容器类型的循环检测将会被忽略而且会抛出一个:exc:`OverflowError`异常 (或更糟) 的结果. 

   If *allow_nan* is ``False`` (default: ``True``), then it will be a
   :exc:`ValueError` to serialize out of range :class:`float` values (``nan``,
   ``inf``, ``-inf``) in strict compliance of the JSON specification, instead of
   using the JavaScript equivalents (``NaN``, ``Infinity``, ``-Infinity``).

   如果*allow_nan*是``False`` (默认值为``True``) , 那么
   在严格的JSON规范里, 序列化超出范围的 :class:`float`值 (``nan``,
   ``inf``, ``-inf``) 将会抛出:exc:`ValueError`异常, 而不是
   使用JavaScript的等价值 (``NaN``, ``Infinity``, ``-Infinity``) . 

   If *indent* is a non-negative integer or string, then JSON array elements and
   object members will be pretty-printed with that indent level.  An indent level
   of 0 or ``""`` will only insert newlines.  ``None`` (the default) selects the
   most compact representation. Using an integer indent indents that many spaces
   per level.  If *indent* is a string (such at '\t'), that string is used to indent
   each level.

   如果*indent*是非负整数或字符串, 那么JSON数组元素和对象成员将会indent级别漂亮的打印. 
   0或``""``的缩进级别表示只插入新行. 
   ``None`` (默认) 选择最紧凑的表示. 
   整数缩进表示每一级使用大量空格缩进. 
   如果*indent*是字符串 (如'\t') , 那么该字符串将用于缩进每一级. 

   If *separators* is an ``(item_separator, dict_separator)`` tuple, then it
   will be used instead of the default ``(', ', ': ')`` separators.  ``(',',
   ':')`` is the most compact JSON representation.

   如果*separators*是一个``(item_separator, dict_separator)``元组, 那么默认``(', ', ': ')``分隔符将被取代. 
   ``(',',':')``是最紧凑的JSON表示方式. 

   *default(obj)* is a function that should return a serializable version of
   *obj* or raise :exc:`TypeError`.  The default simply raises :exc:`TypeError`.

   *default(obj)*是一个函数, 它应该返回一个*obj*或抛出:exc:`TypeError`异常的序列化版本. 
   默认简单地抛出:exc:`TypeError`异常. 

   To use a custom :class:`JSONEncoder` subclass (e.g. one that overrides the
   :meth:`default` method to serialize additional types), specify it with the
   *cls* kwarg; otherwise :class:`JSONEncoder` is used.

   要使用自定义:class:`JSONEncoder`子类 (例如, 覆盖:meth:`default`方法到其他类型的序列化) , 设定*cls*参数;  否则使用 :class:`JSONEncoder` . 


.. function:: dumps(obj, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None, indent=None, separators=None, default=None, **kw)

   Serialize *obj* to a JSON formatted :class:`str`.  The arguments have the
   same meaning as in :func:`dump`.
   
   序列化*obj*到JSON格式:class:`str`. 参数和:func:`dump`的意思一样. 

.. function:: load(fp, cls=None, object_hook=None, parse_float=None, parse_int=None, parse_constant=None, object_pairs_hook=None, **kw)

   Deserialize *fp* (a ``.read()``-supporting file-like object containing a JSON
   document) to a Python object.

   反序列化*fp* (例如: ``.read()``-支持包含JSON文化化的类文件对象) 到Python对象. 

   *object_hook* is an optional function that will be called with the result of
   any object literal decoded (a :class:`dict`).  The return value of
   *object_hook* will be used instead of the :class:`dict`.  This feature can be used
   to implement custom decoders (e.g. JSON-RPC class hinting).

   *object_hook*将与任何对象 (例如`dict`类) 解码文字的结果称为是一个可选功能. 
   返回值* object_hook *将被用来代替:class:`dict`. 
   该特征常用于实现自定义的解码器 (如JSON-RPC类的暗示) . 

   *object_pairs_hook* is an optional function that will be called with the
   result of any object literal decoded with an ordered list of pairs.  The
   return value of *object_pairs_hook* will be used instead of the
   :class:`dict`.  This feature can be used to implement custom decoders that
   rely on the order that the key and value pairs are decoded (for example,
   :func:`collections.OrderedDict` will remember the order of insertion). If
   *object_hook* is also defined, the *object_pairs_hook* takes priority.

   *object_pairs_hook*解码一个已排序列表对的任何文字对象的结果将被称为是一个可选功能. . 
   返回值*object_pairs_hook*将被用来代替:class:`dict`. 
   此特征常用于实现自定义响应已被解码的键值对的解码器 (例如, :func:`collections.OrderedDict`会记住元素插入的顺序) . 
   如果也定义了*object_hook*, 那么会优先选择*object_pairs_hook*. 

   .. versionchanged:: 3.1
      Added support for *object_pairs_hook*.


   *parse_float*, if specified, will be called with the string of every JSON
   float to be decoded.  By default, this is equivalent to ``float(num_str)``.
   This can be used to use another datatype or parser for JSON floats
   (e.g. :class:`decimal.Decimal`).

   *parse_float*, 如果指定的话, 将要求每个JSON浮点数的字符串进行解码. 
   默认情况下相当于``float(num_str)``. 
   常用于其他数据类型或JSON浮点数分析 (例如: :class:`decimal.Decimal`) 

   *parse_int*, if specified, will be called with the string of every JSON int
   to be decoded.  By default, this is equivalent to ``int(num_str)``.  This can
   be used to use another datatype or parser for JSON integers
   (e.g. :class:`float`).

  *parse_int*,  如果指定的话, 将要求每个JSON整数的字符串进行解码. 
  默认情况下相当于``int(num_str)``. 
  常用于其他数据类型或JSON整数分析 (例如: :class:`float`) 

   *parse_constant*, if specified, will be called with one of the following
   strings: ``'-Infinity'``, ``'Infinity'``, ``'NaN'``, ``'null'``, ``'true'``,
   ``'false'``.  This can be used to raise an exception if invalid JSON numbers
   are encountered.

   *parse_constant*, 如果指定的话, 以下字符串将会调用: 
   ``'-Infinity'``, ``'Infinity'``, ``'NaN'``, ``'null'``, ``'true'``, ``'false'``. 
   常用于如果遇到无效的JSON成员时就抛出异常. 

   To use a custom :class:`JSONDecoder` subclass, specify it with the ``cls``
   kwarg; otherwise :class:`JSONDecoder` is used.  Additional keyword arguments
   will be passed to the constructor of the class.

   要使用自定义:class:`JSONDecoder`子类, 必须指定``cls``参数; 否则使用:class:`JSONDecoder`. 
   额外关键字参数将被传递给这个类的构造函数. 


.. function:: loads(s, encoding=None, cls=None, object_hook=None, parse_float=None, parse_int=None, parse_constant=None, object_pairs_hook=None, **kw)

   Deserialize *s* (a :class:`str` instance containing a JSON document) to a
   Python object.

   反序列化*s* (一个包含JSON文档的:class:`str`实例) 到一个Python对象. 

   The other arguments have the same meaning as in :func:`load`, except
   *encoding* which is ignored and deprecated.

   其他参数与:func:`load`含义相同, 除了被忽略和过时的*encoding*. 

 编码器和解码器
------------------------------------------------------------------

.. class:: JSONDecoder(object_hook=None, parse_float=None, parse_int=None, parse_constant=None, strict=True, object_pairs_hook=None)

   Simple JSON decoder.

   简易JSON解码器. 

   Performs the following translations in decoding by default:

   默认情况下, 在解码时按下面方式进行转换: 

   +---------------+-------------------+
   | JSON          | Python            |
   +===============+===================+
   | object        | dict              |
   +---------------+-------------------+
   | array         | list              |
   +---------------+-------------------+
   | string        | str               |
   +---------------+-------------------+
   | number (int)  | int               |
   +---------------+-------------------+
   | number (real) | float             |
   +---------------+-------------------+
   | true          | True              |
   +---------------+-------------------+
   | false         | False             |
   +---------------+-------------------+
   | null          | None              |
   +---------------+-------------------+

   It also understands ``NaN``, ``Infinity``, and ``-Infinity`` as their
   corresponding ``float`` values, which is outside the JSON spec.

   它也理解 ``NaN``, ``Infinity``以及 ``-Infinity``作为相应的``float``值, 这超出JSON规范了. 

   *object_hook*, if specified, will be called with the result of every JSON
   object decoded and its return value will be used in place of the given
   :class:`dict`.  This can be used to provide custom deserializations (e.g. to
   support JSON-RPC class hinting).

   *object_hook*, 如果指定的话, 将要求每个JSON的结果对象解码, 其返回值常被用于已给定:class:`dict`的位置. 
   常用于提供自定义反序列化 (例如支持JSON-RPC类的暗示) . 

   *object_pairs_hook*, if specified will be called with the result of every
   JSON object decoded with an ordered list of pairs.  The return value of
   *object_pairs_hook* will be used instead of the :class:`dict`.  This
   feature can be used to implement custom decoders that rely on the order
   that the key and value pairs are decoded (for example,
   :func:`collections.OrderedDict` will remember the order of insertion). If
   *object_hook* is also defined, the *object_pairs_hook* takes priority.   

   * object_pairs_hook *, 如果指定的话, 将对每个JSON对象解码成有序列表对的结果. 
   返回值*object_pairs_hook*将被用来代替:class:`dict`. 
   此特征功能可用于实现自定义的解码器, 其依赖顺序键和值对解码 (例如, :func:`collections.OrderedDict`会记住元素插入的顺序) . 
   如果也定义了*object_hook*, 那么会优先选择*object_pairs_hook*. 

   .. versionchanged:: 3.1
      Added support for *object_pairs_hook*.


   *parse_float*, if specified, will be called with the string of every JSON
   float to be decoded.  By default, this is equivalent to ``float(num_str)``.
   This can be used to use another datatype or parser for JSON floats
   (e.g. :class:`decimal.Decimal`).

   *parse_float*, 如果指定的话, 将要求每个JSON浮点数的字符串进行解码. 
   默认情况下相当于``float(num_str)``. 
   常用于其他数据类型或JSON浮点数分析 (例如: :class:`decimal.Decimal`) 

   *parse_int*, if specified, will be called with the string of every JSON int
   to be decoded.  By default, this is equivalent to ``int(num_str)``.  This can
   be used to use another datatype or parser for JSON integers
   (e.g. :class:`float`).

  *parse_int*,  如果指定的话, 将要求每个JSON整数的字符串进行解码. 
  默认情况下相当于``int(num_str)``. 
  常用于其他数据类型或JSON整数分析 (例如: :class:`float`) 

   *parse_constant*, if specified, will be called with one of the following
   strings: ``'-Infinity'``, ``'Infinity'``, ``'NaN'``, ``'null'``, ``'true'``,
   ``'false'``.  This can be used to raise an exception if invalid JSON numbers
   are encountered.

   *parse_constant*, 如果指定的话, 以下字符串将会调用: 
   ``'-Infinity'``, ``'Infinity'``, ``'NaN'``, ``'null'``, ``'true'``, ``'false'``. 
   常用于如果遇到无效的JSON成员时就抛出异常. 

   If *strict* is ``False`` (``True`` is the default), then control characters
   will be allowed inside strings.  Control characters in this context are
   those with character codes in the 0-31 range, including ``'\t'`` (tab),
   ``'\n'``, ``'\r'`` and ``'\0'``.

   如果*strict*是``False`` (``True``是默认值) , 然后控制字符将被允许​​在字符串内. 
   控制字符在0-31范围内, 包括``'\t'`` (tab), ``'\n'``, ``'\r'`` 和 ``'\0'``. 


   .. method:: decode(s)

      Return the Python representation of *s* (a :class:`str` instance
      containing a JSON document)

      返回*s*的Python表示 (例如, 包含一个JSON文档的:class:`str`实例) 

   .. method:: raw_decode(s)

      Decode a JSON document from *s* (a :class:`str` beginning with a
      JSON document) and return a 2-tuple of the Python representation
      and the index in *s* where the document ended.

      从*s*处解码JSON文档 (例如, :class:`str`的开始带有JSON文档) , 并返回一个在*s*中Python表示2元组的文件结束索引. 

      This can be used to decode a JSON document from a string that may have
      extraneous data at the end.

      常用于从字符串解码JSON文档, 它可能有无关的数据在结束处. 


.. class:: JSONEncoder(skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, sort_keys=False, indent=None, separators=None, default=None)

   Extensible JSON encoder for Python data structures.

   Python数据结构的扩展JSON编码器. 

   Supports the following objects and types by default:

   默认情况下, 支持下面的对象和类型: 

   +-------------------+---------------+
   | Python            | JSON          |
   +===================+===============+
   | dict              | object        |
   +-------------------+---------------+
   | list, tuple       | array         |
   +-------------------+---------------+
   | str               | string        |
   +-------------------+---------------+
   | int, float        | number        |
   +-------------------+---------------+
   | True              | true          |
   +-------------------+---------------+
   | False             | false         |
   +-------------------+---------------+
   | None              | null          |
   +-------------------+---------------+

   To extend this to recognize other objects, subclass and implement a
   :meth:`default` method with another method that returns a serializable object
   for ``o`` if possible, otherwise it should call the superclass implementation
   (to raise :exc:`TypeError`).

   为了 扩展识别其他对象, 子类和实现一个:meth:`default`方法, 
   以及如果可能的话返回其他序列化对象``o``的方法, 否则它应调用父类的实现 (即抛出:exc:`TypeError`异常`) . 

   If *skipkeys* is ``False`` (the default), then it is a :exc:`TypeError` to
   attempt encoding of keys that are not str, int, float or None.  If
   *skipkeys* is ``True``, such items are simply skipped.

   如果*skipkeys*是``False`` (默认值) , 那么它是:exc:`TypeError`异常而不是str, int, float 或 None的伪编码. 
   如果*skipkeys*是``True``, 则简单地忽略此项目. 

   If *ensure_ascii* is ``True`` (the default), the output is guaranteed to
   have all incoming non-ASCII characters escaped.  If *ensure_ascii* is
   ``False``, these characters will be output as-is.

   如果*ensure_ascii*是``True`` (默认值) , 则输出时保证转义所有输入的非ASCII字符. 
   如果*ensure_ascii*是``False``, 这些字符将被输出. 

   If *check_circular* is ``True`` (the default), then lists, dicts, and custom
   encoded objects will be checked for circular references during encoding to
   prevent an infinite recursion (which would cause an :exc:`OverflowError`).
   Otherwise, no such check takes place.

   如果*check_circular*是``True`` (默认值) , lists、dicts和自定义编码的对象将在循环引用的编码中检查, 
   以防止无限递归 (这会抛出:exc:`OverflowError`异常) . 否则, 没有这样的检查发生. 

   If *allow_nan* is ``True`` (the default), then ``NaN``, ``Infinity``, and
   ``-Infinity`` will be encoded as such.  This behavior is not JSON
   specification compliant, but is consistent with most JavaScript based
   encoders and decoders.  Otherwise, it will be a :exc:`ValueError` to encode
   such floats.

   如果*allow_nan*是``True`` (默认值) , 则`NaN``、``Infinity``和``-Infinity``将被编码. 
   该行为不是JSON规范, 但与大多数基于JavaScript编码及解码是一致的. 
   否则, 在编码时会抛出:exc:`ValueError`异常, 就如floats一样. 

   If *sort_keys* is ``True`` (default ``False``), then the output of dictionaries
   will be sorted by key; this is useful for regression tests to ensure that
   JSON serializations can be compared on a day-to-day basis.

   如果*sort_keys*是``True`` (默认值为``False``) , 则输出字典时会按键值排序; 
   这是有用的回归测试, 以确保JSON序列化可以经常性地进行比较. 

   If *indent* is a non-negative integer (it is ``None`` by default), then JSON
   array elements and object members will be pretty-printed with that indent
   level.  An indent level of 0 will only insert newlines.  ``None`` is the most
   compact representation.

   如果*indent*是非负整数 (默认为``None``) , 那么JSON数组元素和对象成员将会按照指定的缩进级别进行漂亮的打印. 
   0级缩进表示插入新行.  ``None``则表示最紧凑压缩. 

   If specified, *separators* should be an ``(item_separator, key_separator)``
   tuple.  The default is ``(', ', ': ')``.  To get the most compact JSON
   representation, you should specify ``(',', ':')`` to eliminate whitespace.

   If specified, *default* is a function that gets called for objects that can't
   otherwise be serialized.  It should return a JSON encodable version of the
   object or raise a :exc:`TypeError`.

   如果指定的话, *default*是一个被调用的函数的对象, 不能否则被序列化. 
   它应该返回一个可编码的JSON版本对象或者抛出:exc:`TypeError`异常. 


   .. method:: default(o)


      Implement this method in a subclass such that it returns a serializable
      object for *o*, or calls the base implementation (to raise a
      :exc:`TypeError`).

      在子类中实现该方法, 以便返回一个可序列化的*o*对象, 或调用基类实现 (抛出:exc:`TypeError`异常) . 

      For example, to support arbitrary iterators, you could implement default
      like this::

      例如, 支持任意的迭代器, 可以实现默认是这样的: 

         def default(self, o):
            try:
                iterable = iter(o)
            except TypeError:
                pass
            else:
                return list(iterable)
            return json.JSONEncoder.default(self, o)


   .. method:: encode(o)


      Return a JSON string representation of a Python data structure, *o*.  For
      example::

      返回一个表示Python数据结构的JSON字符串, *o*, 例如: 

        >>> json.JSONEncoder().encode({"foo": ["bar", "baz"]})
        '{"foo": ["bar", "baz"]}'


   .. method:: iterencode(o)


      Encode the given object, *o*, and yield each string representation as
      available.  For example:

      编码指定对象*o*, 和生成每个可用的字符串形式. 例如::

            for chunk in json.JSONEncoder().iterencode(bigobject):
                mysocket.write(chunk)

