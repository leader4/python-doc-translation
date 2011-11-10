:mod:`importlib` -- import的实现
==========================================================

.. module:: importlib
   :synopsis: An implementation of the import machinery.

.. moduleauthor:: Brett Cannon <brett@python.org>
.. sectionauthor:: Brett Cannon <brett@python.org>

.. versionadded:: 3.1


简介
------------

The purpose of the :mod:`importlib` package is two-fold. One is to provide an
implementation of the :keyword:`import` statement (and thus, by extension, the
:func:`__import__` function) in Python source code. This provides an
implementation of :keyword:`import` which is portable to any Python
interpreter. This also provides a reference implementation which is easier to
comprehend than one implemented in a programming language other than Python.

------------------------------------------------------------------------------------------------------------------------------------------------------

importlib包的引入有两种意义.首先,它为Python程序中的`import`语句提供了一个接口,该接口适合于任何Python解释器.该包也提供了一个更容易理解的,应用在其他非Python语言的相关接口.

Two, the components to implement :keyword:`import` are exposed in this
package, making it easier for users to create their own custom objects (known
generically as an :term:`importer`) to participate in the import process.
Details on custom importers can be found in :pep:`302`.

------------------------------------------------------------------------------------------------------------------------------------------------------

其次,`import`接口的组件会暴露在该包之中,使用户更容易的创建他们自己的自定义对象来参与到import进程. 自定义importers的相关信息可以在:pep:`302`中找到. 

.. seealso::

    :ref:`import`
        The language reference for the :keyword:`import` statement.

------------------------------------------------------------------------------------------------------------------------------------------------------

        import属性的语言参考.
        
    `Packages specification <http://www.python.org/doc/essays/packages.html>`__
        Original specification of packages. Some semantics have changed since
        the writing of this document (e.g. redirecting based on ``None``
        in :data:`sys.modules`).

------------------------------------------------------------------------------------------------------------------------------------------------------

        关于包的详细的原始说明.当在写作本文档时一些术语的语义就已经改变了(如`sys.modules`)
        
    The :func:`.__import__` function
        The :keyword:`import` statement is syntactic sugar for this function.

------------------------------------------------------------------------------------------------------------------------------------------------------

        这个函数的import属性的含糖语法(译者注:syntactic sugar,指的是为一门计算机语言的语法中添加的附加物或附加成分,它不会影响语言的功能,但却能使人类使用起该语言来" 更甜美" 一些).
        
    :pep:`235`
        Import on Case-Insensitive Platforms
        
------------------------------------------------------------------------------------------------------------------------------------------------------      
        
        引入到Case-Insensitive平台.
        
    :pep:`263`
        Defining Python Source Code Encodings

    :pep:`302`
        New Import Hooks

    :pep:`328`
        Imports: Multi-Line and Absolute/Relative

    :pep:`366`
        Main module explicit relative imports

    :pep:`3120`
        Using UTF-8 as the Default Source Encoding

    :pep:`3147`
        PYC Repository Directories


函数
---------

.. function:: __import__(name, globals={}, locals={}, fromlist=list(), level=0)

    An implementation of the built-in :func:`__import__` function.

------------------------------------------------------------------------------------------------------------------------------------------------------

   __import__函数的一个内置接口
   
.. function:: import_module(name, package=None)

    Import a module. The *name* argument specifies what module to
    import in absolute or relative terms
    (e.g. either ``pkg.mod`` or ``..mod``). If the name is
    specified in relative terms, then the *package* argument must be set to
    the name of the package which is to act as the anchor for resolving the
    package name (e.g. ``import_module('..mod', 'pkg.subpkg')`` will import
    ``pkg.mod``).

------------------------------------------------------------------------------------------------------------------------------------------------------

        导入了一个模块.参数name指定了那些通过绝对或者相对方式被导入的模块.参数name一旦被指定,那么参数package就必须设置成表现为anchor的包的名字.
    
    The :func:`import_module` function acts as a simplifying wrapper around
    :func:`importlib.__import__`. This means all semantics of the function are
    derived from :func:`importlib.__import__`, including requiring the package
    from which an import is occurring to have been previously imported
    (i.e., *package* must already be imported). The most important difference
    is that :func:`import_module` returns the most nested package or module
    that was imported (e.g. ``pkg.mod``), while :func:`__import__` returns the
    top-level package or module (e.g. ``pkg``).

------------------------------------------------------------------------------------------------------------------------------------------------------

    import_module函数表现为importlib.__import__函数的一个简化版本,这意味着这个函数的语义派生自importlib.__import__函数,including requiring the package from which an import is occurring to have been previously imported. 
          两者最大的不同为: import_module函数返回引入的包或模块的嵌套路径(如 ``pkg.mod``),而`__import__`函数只返回所引入的包或模块的最顶层(如 ``pkg``).

:mod:`importlib.abc` -- Abstract base classes related to import
---------------------------------------------------------------

.. module:: importlib.abc
    :synopsis: Abstract base classes related to import

------------------------------------------------------------------------------------------------------------------------------------------------------

    与import有关的抽象基类
    
The :mod:`importlib.abc` module contains all of the core abstract base classes
used by :keyword:`import`. Some subclasses of the core abstract base classes
are also provided to help in implementing the core ABCs.

------------------------------------------------------------------------------------------------------------------------------------------------------

importlib.abc模块涵盖了被关键字import所使用的所有核心的抽象基类,这些核心的抽象基类中的一些超类也被提供以去帮助实现核心ABCs的接口

.. class:: Finder

    An abstract base class representing a :term:`finder`.
    See :pep:`302` for the exact definition for a finder.

    .. method:: find_module(fullname, path=None)

        An abstract method for finding a :term:`loader` for the specified
        module. If the :term:`finder` is found on :data:`sys.meta_path` and the
        module to be searched for is a subpackage or module then *path* will
        be the value of :attr:`__path__` from the parent package. If a loader
        cannot be found, ``None`` is returned.


------------------------------------------------------------------------------------------------------------------------------------------------------

        抽象方法,在一个在指定模块寻找loader,如果loader在sys.meta_path中被找到,且寻找的模块是一个子包或子模块, 那么父包的__path__ 的值就会变为参数path.     
        
.. class:: Loader

    An abstract base class for a :term:`loader`.
    See :pep:`302` for the exact definition for a loader.

------------------------------------------------------------------------------------------------------------------------------------------------------

    finder抽象基类
    
    .. method:: load_module(fullname)

        An abstract method for loading a module. If the module cannot be
        loaded, :exc:`ImportError` is raised, otherwise the loaded module is
        returned.

------------------------------------------------------------------------------------------------------------------------------------------------------

                    加载模块的一个抽象方法.如果该模块不能被引导,就会出现ImportError错误,反之就会返回一个已加载的模块 
            
        If the requested module already exists in :data:`sys.modules`, that
        module should be used and reloaded.
        Otherwise the loader should create a new module and insert it into
        :data:`sys.modules` before any loading begins, to prevent recursion
        from the import. If the loader inserted a module and the load fails, it
        must be removed by the loader from :data:`sys.modules`; modules already
        in :data:`sys.modules` before the loader began execution should be left
        alone. The :func:`importlib.util.module_for_loader` decorator handles
        all of these details.

------------------------------------------------------------------------------------------------------------------------------------------------------

                    如果被请求的模块已经存在于sys.modules中,那么模块应该被使用并重新加载.除此之外,loader应该创建一个新的模块并在任何加载开始之前就把它插入到sys.modules中,来防止import的递归.
                    如果loader插入了一个模块,一旦加载失败,它就会被loader从sys.modules中移除.--------- (重新审查) -
        
        The loader should set several attributes on the module.
        (Note that some of these attributes can change when a module is
        reloaded.)

------------------------------------------------------------------------------------------------------------------------------------------------------

        loader应该为该模块设置一些属性.(注意当模块被重载时这些属性中的一些会改变.
        
        - :attr:`__name__`
            The name of the module.

------------------------------------------------------------------------------------------------------------------------------------------------------

            模块的名字.
            
        - :attr:`__file__`
            The path to where the module data is stored (not set for built-in
            modules).

------------------------------------------------------------------------------------------------------------------------------------------------------

            储存模块数据的路径(不是为内置模块设置的).
            
        - :attr:`__path__`
            A list of strings specifying the search path within a
            package. This attribute is not set on modules.

------------------------------------------------------------------------------------------------------------------------------------------------------

            明确说明包中的搜索路径的字符串队列,这个属性并未在模块中设置.
            
        - :attr:`__package__`
            The parent package for the module/package. If the module is
            top-level then it has a value of the empty string. The
            :func:`importlib.util.set_package` decorator can handle the details
            for :attr:`__package__`.

------------------------------------------------------------------------------------------------------------------------------------------------------

            该模块/包的父包.如果一个模块位于最高层,那么它会拥有一个空字符串类型的值.importlib.util.set_package修饰符可以处理__package__属性的细节.
            
        - :attr:`__loader__`
            The loader used to load the module.
            (This is not set by the built-in import machinery,
            but it should be set whenever a :term:`loader` is used.)


------------------------------------------------------------------------------------------------------------------------------------------------------

            该loader用于引导模块.(它并不由内置的导入机制设置,而是当使用loader时才被设置)
            
.. class:: ResourceLoader

    An abstract base class for a :term:`loader` which implements the optional
    :pep:`302` protocol for loading arbitrary resources from the storage
    back-end.

------------------------------------------------------------------------------------------------------------------------------------------------------

    loader的一个抽象基类,它提供了一个可选的方式,来从后端的(back-end)储存设备加载任意资源.
    
    .. method:: get_data(path)

        An abstract method to return the bytes for the data located at *path*.
        Loaders that have a file-like storage back-end
        that allows storing arbitrary data
        can implement this abstract method to give direct access
        to the data stored. :exc:`IOError` is to be raised if the *path* cannot
        be found. The *path* is expected to be constructed using a module's
        :attr:`__file__` attribute or an item from a package's :attr:`__path__`.


------------------------------------------------------------------------------------------------------------------------------------------------------

        一个返回数据字节并定位在path上的数据的抽象基类.loader含有一个file-like的后端的(back-end)储存设备,它允许任意的数据都可以通过实现这个抽象方法来直接访问数据储存设备(the data stored)------
        如果path未找到时就会出现IOError错误.path被期望使用模块的__file__属性或包的__path__属性中的一个条目来构造--------------------------------item翻译的不好------------------
        
.. class:: InspectLoader

    An abstract base class for a :term:`loader` which implements the optional
    :pep:`302` protocol for loaders that inspect modules.

    .. method:: get_code(fullname)

        An abstract method to return the :class:`code` object for a module.
        ``None`` is returned if the module does not have a code object
        (e.g. built-in module).  :exc:`ImportError` is raised if loader cannot
        find the requested module.

------------------------------------------------------------------------------------------------------------------------------------------------------


    .. method:: get_source(fullname)

        An abstract method to return the source of a module. It is returned as
        a text string with universal newlines. Returns ``None`` if no
        source is available (e.g. a built-in module). Raises :exc:`ImportError`
        if the loader cannot find the module specified.

------------------------------------------------------------------------------------------------------------------------------------------------------


    .. method:: is_package(fullname)

        An abstract method to return a true value if the module is a package, a
        false value otherwise. :exc:`ImportError` is raised if the
        :term:`loader` cannot find the module.


------------------------------------------------------------------------------------------------------------------------------------------------------

        该抽象方法会在当一个模块为一个包时返回true,反之返回false.当loader无法找到模块时会引起导入ImportError错误.
        
.. class:: ExecutionLoader

    An abstract base class which inherits from :class:`InspectLoader` that,
    when implemented, helps a module to be executed as a script. The ABC
    represents an optional :pep:`302` protocol.

------------------------------------------------------------------------------------------------------------------------------------------------------

    一个继承自InspectLoader类的抽象基类(abstract base class).当它被实现时,会帮助模块以脚本的方式运行.ABC代表一个可选的协议.-------------------------------------------------------------------感觉不是很好.---------------
    
    .. method:: get_filename(fullname)

        An abstract method that is to return the value of :attr:`__file__` for
        the specified module. If no path is available, :exc:`ImportError` is
        raised.

------------------------------------------------------------------------------------------------------------------------------------------------------

        该抽象方法为特定的模块返回__file__的值.如果path无法访问就会引起ImportError错误.
        
        If source code is available, then the method should return the path to
        the source file, regardless of whether a bytecode was used to load the
        module.


------------------------------------------------------------------------------------------------------------------------------------------------------

        如果源代码可被访问,那么应该向源文件返回path的值.而不管一个bytecode是否在加载该模块.-------------------------翻译不太对.
        
.. class:: SourceLoader

    An abstract base class for implementing source (and optionally bytecode)
    file loading. The class inherits from both :class:`ResourceLoader` and
    :class:`ExecutionLoader`, requiring the implementation of:

------------------------------------------------------------------------------------------------------------------------------------------------------

    实现源 (和可选的bytecode) 文件的加载的抽象基类,该类继承自两个类: 类ResourceLoader和类ExecutionLoader,并需要以下实现: 
    
    * :meth:`ResourceLoader.get_data`
    * :meth:`ExecutionLoader.get_filename`
          Should only return the path to the source file; sourceless
          loading is not supported.

------------------------------------------------------------------------------------------------------------------------------------------------------

        应该只返回源文件路径,无路径的加载是不被支持的.
        
    The abstract methods defined by this class are to add optional bytecode
    file support. Not implementing these optional methods causes the loader to
    only work with source code. Implementing the methods allows the loader to
    work with source *and* bytecode files; it does not allow for *sourceless*
    loading where only bytecode is provided.  Bytecode files are an
    optimization to speed up loading by removing the parsing step of Python's
    compiler, and so no bytecode-specific API is exposed.

------------------------------------------------------------------------------------------------------------------------------------------------------

    这个类所定义的抽象方法可以增加可选的字节码(bytecode)文件支持.没实现该抽象方法时会导致loader只能与source code打交道.实现该抽象方法时则允许loader与source code和bytecode交互(work).它不允许当只提供bytecode时无源文件 (sourceless) 的加载.
    
    .. method:: path_mtime(self, path)

        Optional abstract method which returns the modification time for the
        specified path.

------------------------------------------------------------------------------------------------------------------------------------------------------

        可选的抽象方法,为特定路径返回返回一个修正后的时间(the modification time)
        
    .. method:: set_data(self, path, data)

        Optional abstract method which writes the specified bytes to a file
        path. Any intermediate directories which do not exist are to be created
        automatically.

------------------------------------------------------------------------------------------------------------------------------------------------------

        可选的抽象方法,为文件路径写入指定的字节.任何不存在的中间路径都会被自动创建.
        
        When writing to the path fails because the path is read-only
        (:attr:`errno.EACCES`), do not propagate the exception.

------------------------------------------------------------------------------------------------------------------------------------------------------

        当因为路径是只读而导致路径写入失败时,不要传播(propagate)这个异常.
        
    .. method:: get_code(self, fullname)

        Concrete implementation of :meth:`InspectLoader.get_code`.

------------------------------------------------------------------------------------------------------------------------------------------------------

        具体实现InspectLoader.get_code方法
        
    .. method:: load_module(self, fullname)

        Concrete implementation of :meth:`Loader.load_module`.

------------------------------------------------------------------------------------------------------------------------------------------------------

        具体实现Loader.load_module方法
        
    .. method:: get_source(self, fullname)

        Concrete implementation of :meth:`InspectLoader.get_source`.

------------------------------------------------------------------------------------------------------------------------------------------------------

        具体实现InspectLoader.get_source方法
        
    .. method:: is_package(self, fullname)

        Concrete implementation of :meth:`InspectLoader.is_package`. A module
        is determined to be a package if its file path is a file named
        ``__init__`` when the file extension is removed.


------------------------------------------------------------------------------------------------------------------------------------------------------

        具体实现InspectLoader.is_package方法.
        
.. class:: PyLoader

    An abstract base class inheriting from
    :class:`ExecutionLoader` and
    :class:`ResourceLoader` designed to ease the loading of
    Python source modules (bytecode is not handled; see
    :class:`SourceLoader` for a source/bytecode ABC). A subclass
    implementing this ABC will only need to worry about exposing how the source
    code is stored; all other details for loading Python source code will be
    handled by the concrete implementations of key methods.

------------------------------------------------------------------------------------------------------------------------------------------------------

    .. deprecated:: 3.2
        This class has been deprecated in favor of :class:`SourceLoader` and is
        slated for removal in Python 3.4. See below for how to create a
        subclass that is compatible with Python 3.1 onwards.

------------------------------------------------------------------------------------------------------------------------------------------------------


    If compatibility with Python 3.1 is required, then use the following idiom
    to implement a subclass that will work with Python 3.1 onwards (make sure
    to implement :meth:`ExecutionLoader.get_filename`)::

------------------------------------------------------------------------------------------------------------------------------------------------------

    如果要保持与Python 3.1的兼容,可以使用以下习语来实现一个超类,以与Python 3.1向前兼容
    
        try:
            from importlib.abc import SourceLoader
        except ImportError:
            from importlib.abc import PyLoader as SourceLoader


        class CustomLoader(SourceLoader):
            def get_filename(self, fullname):
                """Return the path to the source file."""
                # Implement ...

            def source_path(self, fullname):
                """Implement source_path in terms of get_filename."""
                try:
                    return self.get_filename(fullname)
                except ImportError:
                    return None

            def is_package(self, fullname):
                """Implement is_package by looking for an __init__ file
                name as returned by get_filename."""
                filename = os.path.basename(self.get_filename(fullname))
                return os.path.splitext(filename)[0] == '__init__'


    .. method:: source_path(fullname)

        An abstract method that returns the path to the source code for a
        module. Should return ``None`` if there is no source code.
        Raises :exc:`ImportError` if the loader knows it cannot handle the
        module.

------------------------------------------------------------------------------------------------------------------------------------------------------

        为模块返回源代码路径的抽象方法.如果没源代码则返回None,如果loader不能处理模块则会引起ImportError.
        
    .. method:: get_filename(fullname)

        A concrete implementation of
        :meth:`importlib.abc.ExecutionLoader.get_filename` that
        relies on :meth:`source_path`. If :meth:`source_path` returns
        ``None``, then :exc:`ImportError` is raised.

------------------------------------------------------------------------------------------------------------------------------------------------------

        依赖于source_path方法的importlib.abc.ExecutionLoader.get_filename方法的一个具体实现,如果是source_path返回None,就会导致ImportError.
        
    .. method:: load_module(fullname)

        A concrete implementation of :meth:`importlib.abc.Loader.load_module`
        that loads Python source code. All needed information comes from the
        abstract methods required by this ABC. The only pertinent assumption
        made by this method is that when loading a package
        :attr:`__path__` is set to ``[os.path.dirname(__file__)]``.

------------------------------------------------------------------------------------------------------------------------------------------------------

        引导Python源代码的importlib.abc.Loader.load_module方法的一个具体实现.
        
    .. method:: get_code(fullname)

        A concrete implementation of
        :meth:`importlib.abc.InspectLoader.get_code` that creates code objects
        from Python source code, by requesting the source code (using
        :meth:`source_path` and :meth:`get_data`) and compiling it with the
        built-in :func:`compile` function.

------------------------------------------------------------------------------------------------------------------------------------------------------

    .. method:: get_source(fullname)

        A concrete implementation of
        :meth:`importlib.abc.InspectLoader.get_source`. Uses
        :meth:`importlib.abc.ResourceLoader.get_data` and :meth:`source_path`
        to get the source code.  It tries to guess the source encoding using
        :func:`tokenize.detect_encoding`.


------------------------------------------------------------------------------------------------------------------------------------------------------

.. class:: PyPycLoader

    An abstract base class inheriting from :class:`PyLoader`.
    This ABC is meant to help in creating loaders that support both Python
    source and bytecode.

------------------------------------------------------------------------------------------------------------------------------------------------------

    .. deprecated:: 3.2
        This class has been deprecated in favor of :class:`SourceLoader` and to
        properly support :pep:`3147`. If compatibility is required with
        Python 3.1, implement both :class:`SourceLoader` and :class:`PyLoader`;
        instructions on how to do so are included in the documentation for
        :class:`PyLoader`. Do note that this solution will not support
        sourceless/bytecode-only loading; only source *and* bytecode loading.

------------------------------------------------------------------------------------------------------------------------------------------------------

        这个类已经被舍弃.如果需要与Python兼容,可以实现deprecated类与PyLoader类.PyLoader类的文档中已经包含了如何去做的指导.
        
    .. method:: source_mtime(fullname)

        An abstract method which returns the modification time for the source
        code of the specified module. The modification time should be an
        integer. If there is no source code, return ``None``. If the
        module cannot be found then :exc:`ImportError` is raised.

------------------------------------------------------------------------------------------------------------------------------------------------------

        抽象方法,返回指定的模块的源代码返回修改时间.如果没有源代码,则返回"None".如果没有找到该模块,就会引起ImportError.
        
    .. method:: bytecode_path(fullname)

        An abstract method which returns the path to the bytecode for the
        specified module, if it exists. It returns ``None``
        if no bytecode exists (yet).
        Raises :exc:`ImportError` if the loader knows it cannot handle the
        module.

------------------------------------------------------------------------------------------------------------------------------------------------------

        抽象方法,为指定的模块返回bytecode的路径.如果该路径存在,那么返回None,如果不存在,在loader不能处理的情况下会引起ImportError.
        
    .. method:: get_filename(fullname)

        A concrete implementation of
        :meth:`ExecutionLoader.get_filename` that relies on
        :meth:`PyLoader.source_path` and :meth:`bytecode_path`.
        If :meth:`source_path` returns a path, then that value is returned.
        Else if :meth:`bytecode_path` returns a path, that path will be
        returned. If a path is not available from both methods,
        :exc:`ImportError` is raised.

------------------------------------------------------------------------------------------------------------------------------------------------------

    .. method:: write_bytecode(fullname, bytecode)

        An abstract method which has the loader write *bytecode* for future
        use. If the bytecode is written, return ``True``. Return
        ``False`` if the bytecode could not be written. This method
        should not be called if :data:`sys.dont_write_bytecode` is true.
        The *bytecode* argument should be a bytes string or bytes array.

------------------------------------------------------------------------------------------------------------------------------------------------------

        抽象方法,
        如果bytecode可写,返回True,反之返回bytecode.如果sys.dont_write_bytecode是true,则不应调用该方法.bytecode参数应该是bytes string或bytes string.

:mod:`importlib.machinery` -- Importers and path hooks
------------------------------------------------------

.. module:: importlib.machinery
    :synopsis: Importers and path hooks

This module contains the various objects that help :keyword:`import`
find and load modules.

------------------------------------------------------------------------------------------------------------------------------------------------------

.. class:: BuiltinImporter

    An :term:`importer` for built-in modules. All known built-in modules are
    listed in :data:`sys.builtin_module_names`. This class implements the
    :class:`importlib.abc.Finder` and :class:`importlib.abc.InspectLoader`
    ABCs.

------------------------------------------------------------------------------------------------------------------------------------------------------

内置模块的importer术语.一切内置模块都列在sys.builtin_module_names里,该类实现了importlib.abc.Finder类和importlib.abc.InspectLoader类

    Only class methods are defined by this class to alleviate the need for
    instantiation.


------------------------------------------------------------------------------------------------------------------------------------------------------

.. class:: FrozenImporter

    An :term:`importer` for frozen modules. This class implements the
    :class:`importlib.abc.Finder` and :class:`importlib.abc.InspectLoader`
    ABCs.

------------------------------------------------------------------------------------------------------------------------------------------------------

    Only class methods are defined by this class to alleviate the need for
    instantiation.


------------------------------------------------------------------------------------------------------------------------------------------------------

.. class:: PathFinder

    :term:`Finder` for :data:`sys.path`. This class implements the
    :class:`importlib.abc.Finder` ABC.
------------------------------------------------------------------------------------------------------------------------------------------------------

    This class does not perfectly mirror the semantics of :keyword:`import` in
    terms of :data:`sys.path`. No implicit path hooks are assumed for
    simplification of the class and its semantics.

------------------------------------------------------------------------------------------------------------------------------------------------------

    这个类并没有在sys.path上完美的反映出import的语义.没有内含的path hooks被设想为简化类和它的语义
    
    Only class methods are defined by this class to alleviate the need for
    instantiation.

------------------------------------------------------------------------------------------------------------------------------------------------------

    只有被该类定义的类方法来减少实例化的需要.
    
    .. classmethod:: find_module(fullname, path=None)

        Class method that attempts to find a :term:`loader` for the module
        specified by *fullname* on :data:`sys.path` or, if defined, on
        *path*. For each path entry that is searched,
        :data:`sys.path_importer_cache` is checked. If an non-false object is
        found then it is used as the :term:`finder` to look for the module
        being searched for. If no entry is found in
        :data:`sys.path_importer_cache`, then :data:`sys.path_hooks` is
        searched for a finder for the path entry and, if found, is stored in
        :data:`sys.path_importer_cache` along with being queried about the
        module. If no finder is ever found then ``None`` is returned.


------------------------------------------------------------------------------------------------------------------------------------------------------

:mod:`importlib.util` -- Utility code for importers
---------------------------------------------------

.. module:: importlib.util
    :synopsis: Importers and path hooks

This module contains the various objects that help in the construction of
an :term:`importer`.

.. decorator:: module_for_loader

    A :term:`decorator` for a :term:`loader` method,
    to handle selecting the proper
    module object to load with. The decorated method is expected to have a call
    signature taking two positional arguments
    (e.g. ``load_module(self, module)``) for which the second argument
    will be the module **object** to be used by the loader.
    Note that the decorator
    will not work on static methods because of the assumption of two
    arguments.

------------------------------------------------------------------------------------------------------------------------------------------------------

    The decorated method will take in the **name** of the module to be loaded
    as expected for a :term:`loader`. If the module is not found in
    :data:`sys.modules` then a new one is constructed with its
    :attr:`__name__` attribute set. Otherwise the module found in
    :data:`sys.modules` will be passed into the method. If an
    exception is raised by the decorated method and a module was added to
    :data:`sys.modules` it will be removed to prevent a partially initialized
    module from being in left in :data:`sys.modules`. If the module was already
    in :data:`sys.modules` then it is left alone.

------------------------------------------------------------------------------------------------------------------------------------------------------

    Use of this decorator handles all the details of which module object a
    loader should initialize as specified by :pep:`302`.

------------------------------------------------------------------------------------------------------------------------------------------------------

.. decorator:: set_loader

    A :term:`decorator` for a :term:`loader` method,
    to set the :attr:`__loader__`
    attribute on loaded modules. If the attribute is already set the decorator
    does nothing. It is assumed that the first positional argument to the
    wrapped method is what :attr:`__loader__` should be set to.

.. decorator:: set_package

    A :term:`decorator` for a :term:`loader` to set the :attr:`__package__`
    attribute on the module returned by the loader. If :attr:`__package__` is
    set and has a value other than ``None`` it will not be changed.
    Note that the module returned by the loader is what has the attribute
    set on and not the module found in :data:`sys.modules`.

------------------------------------------------------------------------------------------------------------------------------------------------------

    Reliance on this decorator is discouraged when it is possible to set
    :attr:`__package__` before the execution of the code is possible. By
    setting it before the code for the module is executed it allows the
    attribute to be used at the global level of the module during
    initialization.

------------------------------------------------------------------------------------------------------------------------------------------------------


