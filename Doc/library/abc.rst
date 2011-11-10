:mod:`abc` --- 抽象基类
====================================

.. module:: abc
   :synopsis: Abstract base classes according to PEP 3119.
.. moduleauthor:: Guido van Rossum
.. sectionauthor:: Georg Brandl
.. much of the content adapted from docstrings

**Source code:** :source:`Lib/abc.py`

--------------

This module provides the infrastructure for defining an :term:`abstract base
class` (ABCs) in Python, as outlined in :pep:`3119`; see the PEP for why this
was added to Python. (See also :pep:`3141` and the :mod:`numbers` module
regarding a type hierarchy for numbers based on ABCs.)

**这个模块提供了一个在Python中定义抽象基类的基础设施, 参见PEP 3119, 可以明白
为什么这个会被添加到Python中. 
 (也可以见*PEP 3141*, ``numbers``模块的类型层次的基础就是ABCs. **

The :mod:`collections` module has some concrete classes that derive from
ABCs; these can, of course, be further derived. In addition the
:mod:`collections` module has some ABCs that can be used to test whether
a class or instance provides a particular interface, for example, is it
hashable or a mapping.

**``collections``模块有一些具体的基于ABCs的派生类; 
他们当然也可以被进一步派生. 此外, 
``collections``模块有一些抽象基类, 可以用来测试
一个类或实例是否提供了某个特定的接口, 例如哈希或映射. **


这个模块提供以下类: 

.. class:: ABCMeta

   Metaclass for defining Abstract Base Classes (ABCs).
   
    定义抽象基类的元类. 

   Use this metaclass to create an ABC.  An ABC can be subclassed directly, and
   then acts as a mix-in class.  You can also register unrelated concrete
   classes (even built-in classes) and unrelated ABCs as "virtual subclasses" --
   these and their descendants will be considered subclasses of the registering
   ABC by the built-in :func:`issubclass` function, but the registering ABC
   won't show up in their MRO (Method Resolution Order) nor will method
   implementations defined by the registering ABC be callable (not even via
   :func:`super`). [#]_

   **使用这个元类来创建一个抽象基类. ABC可以被直接继承, 
   , 然后作为一个mix-in类. 您也可以注册
   无关的具体类 (甚至内置类) 和无关
   ABC作为 "虚拟子类"  - 这些和他们的后代将
   通过内置的 "issubclass () " 函数被处理为已注册抽象基类的子类, 但注册的抽象基类将不会显示在
   他们的MRO (方法解析顺序) , 也没有具体的由注册的ABC定义来调用方法实现 (不是通过
   ``super()``).**

   Classes created with a metaclass of :class:`ABCMeta` have the following method:

    由ABCMeta创建的类有以下方法:


   .. method:: register(subclass)

      Register *subclass* as a "virtual subclass" of this ABC. For
      example::

        from abc import ABCMeta

        class MyABC(metaclass=ABCMeta):
            pass

        MyABC.register(tuple)

        assert issubclass(tuple, MyABC)
        assert isinstance((), MyABC)

    你也可以覆盖这些在抽象基类中的方法:

   .. method:: __subclasshook__(subclass)

      (Must be defined as a class method.)
      
      必须被定义为类方法.

      Check whether *subclass* is considered a subclass of this ABC.  This means
      that you can customize the behavior of ``issubclass`` further without the
      need to call :meth:`register` on every class you want to consider a
      subclass of the ABC.  (This class method is called from the
      :meth:`__subclasscheck__` method of the ABC.)

      **检查子类是否被认为是这个ABC的一个子类. 
      这意味着, 您可以自定义的 "issubclass行为" 
      而不需要在每个你想让其作为ABC子类的类上都调用 "register () " .
      (这个类的方法
      其实调用的是ABC的 "__subclasscheck__ () " 方法. ) **

      This method should return ``True``, ``False`` or ``NotImplemented``.  If
      it returns ``True``, the *subclass* is considered a subclass of this ABC.
      If it returns ``False``, the *subclass* is not considered a subclass of
      this ABC, even if it would normally be one.  If it returns
      ``NotImplemented``, the subclass check is continued with the usual
      mechanism.
      
         这个方法应该返回 "True" ,  "False" 或
       "NotImplemented" . 如果它返回``True``, 这个*子类*会被
      认为是ABC的一个子类. 如果返回 "False``, 
      子类则不是这个ABC的一个子类, 虽然
      通常不会发生这样的事. 如果它返回的`` NotImplemented``, 
      子类的检查将采用普通的机制. 



      .. XXX explain the "usual mechanism"


   For a demonstration of these concepts, look at this example ABC definition::

      class Foo:
          def __getitem__(self, index):
              ...
          def __len__(self):
              ...
          def get_iterator(self):
              return iter(self)

      class MyIterable(metaclass=ABCMeta):

          @abstractmethod
          def __iter__(self):
              while False:
                  yield None

          def get_iterator(self):
              return self.__iter__()

          @classmethod
          def __subclasshook__(cls, C):
              if cls is MyIterable:
                  if any("__iter__" in B.__dict__ for B in C.__mro__):
                      return True
              return NotImplemented

      MyIterable.register(Foo)

   The ABC ``MyIterable`` defines the standard iterable method,
   :meth:`__iter__`, as an abstract method.  The implementation given here can
   still be called from subclasses.  The :meth:`get_iterator` method is also
   part of the ``MyIterable`` abstract base class, but it does not have to be
   overridden in non-abstract derived classes.
   
   ABC "MyIterable" 定义的标准迭代的方法, 
    "__iter__()``,作为一个抽象的方法. 
   这里仍然可以被子类调用执行.  ``get_iterator () ``
   方法也是`` MyIterable "抽象基类的一部分, 但
   它不必在非抽象的派生类中重写. 

   The :meth:`__subclasshook__` class method defined here says that any class
   that has an :meth:`__iter__` method in its :attr:`__dict__` (or in that of
   one of its base classes, accessed via the :attr:`__mro__` list) is
   considered a ``MyIterable`` too.

   这里定义类方法``__subclasshook__ ()  "表明, 任何
   类只要有一个 "__iter__ () " 方法在其``__dict__ " (或
   通过访问其基类的 "__mro__" 列表) 
   都可被认为是一个``MyIterable`` . 

   Finally, the last line makes ``Foo`` a virtual subclass of ``MyIterable``,
   even though it does not define an :meth:`__iter__` method (it uses the
   old-style iterable protocol, defined in terms of :meth:`__len__` and
   :meth:`__getitem__`).  Note that this will not make ``get_iterator``
   available as a method of ``Foo``, so it is provided separately.
   
  最后, 最后一行创建的 "Foo" , "MyIterable" 的虚拟子类
   , 甚至没有定义一个``__iter__ () ``
   方法 (它使用的旧式迭代的协议, 在条款中定义
    "__len__ () " 和 "__getitem__()``).请注意, 这不会
   使 "get_iterator" 成为一个 "Foo" 的可用方法, 所以它的方法
   另行规定,单独提供. 


It also provides the following decorators:

.. decorator:: abstractmethod(function)

   A decorator indicating abstract methods.

   Using this decorator requires that the class's metaclass is :class:`ABCMeta` or
   is derived from it.
   A class that has a metaclass derived from :class:`ABCMeta`
   cannot be instantiated unless all of its abstract methods and
   properties are overridden.
   The abstract methods can be called using any of the normal 'super' call
   mechanisms.

   使用这种装饰类的要求是其类的元类是
    "ABCMeta" , 或者是从它派生的.  A类, 有一个元类
   从 "派生ABCMeta" 不能被实例化, 除非其所有
   抽象方法和属性被覆盖. 这些抽象方法
   可以使用任何正常的 "super" 调用机制来调用.

   Dynamically adding abstract methods to a class, or attempting to modify the
   abstraction status of a method or class once it is created, are not
   supported.  The :func:`abstractmethod` only affects subclasses derived using
   regular inheritance; "virtual subclasses" registered with the ABC's
   :meth:`register` method are not affected.
   
     动态添加一类的抽象方法, 或试图
   修改已创建的一个方法或类的抽象状态
   , 是不被支持的.   "abstractmethod () ``只影响
   使用常规继承的派生子类, "虚拟子类" 
   通过ABC的 "register () " 方法不会受到影响. 

   Usage::

      class C(metaclass=ABCMeta):
          @abstractmethod
          def my_abstract_method(self, ...):
              ...

   .. note::

      Unlike Java abstract methods, these abstract
      methods may have an implementation. This implementation can be
      called via the :func:`super` mechanism from the class that
      overrides it.  This could be useful as an end-point for a
      super-call in a framework that uses cooperative
      multiple-inheritance.

     不同于Java的抽象方法, 这些抽象的方法可能有一个
     实现. 这个实现可以通过调用
     类的 "super () " 机制来覆盖它.这
     可作为在一个使用合作的多重继承框架super-call 的终点. 


.. decorator:: abstractclassmethod(function)

   A subclass of the built-in :func:`classmethod`, indicating an abstract
   classmethod. Otherwise it is similar to :func:`abstractmethod`.

   Usage::

      class C(metaclass=ABCMeta):
          @abstractclassmethod
          def my_abstract_classmethod(cls, ...):
              ...

   .. versionadded:: 3.2


.. decorator:: abstractstaticmethod(function)

   A subclass of the built-in :func:`staticmethod`, indicating an abstract
   staticmethod. Otherwise it is similar to :func:`abstractmethod`.
   
   内置``staticmethod()``的一个子类,显示为一个抽象静态方法.否则,它会比较像``abstractmethod()``.

   Usage::

      class C(metaclass=ABCMeta):
          @abstractstaticmethod
          def my_abstract_staticmethod(...):
              ...

   .. versionadded:: 3.2


.. function:: abstractproperty(fget=None, fset=None, fdel=None, doc=None)

   A subclass of the built-in :func:`property`, indicating an abstract property.
   
    内置''property()''的一个子类,显示为一个抽象属性.


   Using this function requires that the class's metaclass is :class:`ABCMeta` or
   is derived from it.
   A class that has a metaclass derived from :class:`ABCMeta` cannot be
   instantiated unless all of its abstract methods and properties are overridden.
   The abstract properties can be called using any of the normal
   'super' call mechanisms.

   使用此功能需要的类的元类是
    "ABCMeta" , 或者是从它派生的.  A类, 有一个元类
   从 "派生ABCMeta" 不能被实例化, 除非其所有
   抽象方法和属性被覆盖. 这些抽象属性
   可以使用任何正常的 "super" 调用机制来调用.


   Usage::

      class C(metaclass=ABCMeta):
          @abstractproperty
          def my_abstract_property(self):
              ...

   
    这里定义了一个只读属性,你也可以使用'long'--属性声明的形式来定义一个可读写的抽象属性::

      class C(metaclass=ABCMeta):
          def getx(self): ...
          def setx(self, value): ...
          x = abstractproperty(getx, setx)


.. rubric:: Footnotes

.. [#] C++ programmers should note that Python's virtual base class
   concept is not the same as C++'s.

