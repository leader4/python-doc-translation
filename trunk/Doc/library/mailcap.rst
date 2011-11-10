:mod:`mailcap` --- Mailcap文件处理 
========================================

.. module:: mailcap
   :synopsis: Mailcap file handling.

**Source code:** :source:`Lib/mailcap.py`

--------------

Mailcap files are used to configure how MIME-aware applications such as mail
readers and Web browsers react to files with different MIME types. (The name
"mailcap" is derived from the phrase "mail capability".)  For example, a mailcap
file might contain a line like ``video/mpeg; xmpeg %s``.  Then, if the user
encounters an email message or Web document with the MIME type
:mimetype:`video/mpeg`, ``%s`` will be replaced by a filename (usually one
belonging to a temporary file) and the :program:`xmpeg` program can be
automatically started to view the file.

Mailcap文件是被用来配置像邮件阅读器和WEB浏览器这种MIME-aware应用程序对不同
MIME类型作出反应. ("mailcap"的是由"mail capability"衍生而来. )例如,一个
mailcap文件可能包含像``video/mpeg;xmpeg%s``这样的行. 然后,如果用户碰到了
一封邮件信息或者WEB文档包含有*video/mpeg*的MIME类型,``%s``将会被一个文件
名替换 (通常文件属于临时文件),并且**xmpeg**程序会自动运行访问文件. 



The mailcap format is documented in :rfc:`1524`, "A User Agent Configuration
Mechanism For Multimedia Mail Format Information," but is not an Internet
standard.  However, mailcap files are supported on most Unix systems.

mailcap的格式被记载在**RFC 1524**中,"多媒体邮件格式信息的用户代理配置机制"
并不是Internet标准. 但是,mailcap文件支持大多数unix系统. 



.. function:: findmatch(caps, MIMEtype, key='view', filename='/dev/null', plist=[])

   Return a 2-tuple; the first element is a string containing the command line to
   be executed (which can be passed to :func:`os.system`), and the second element
   is the mailcap entry for a given MIME type.  If no matching MIME type can be
   found, ``(None, None)`` is returned.

   返回一个二元组;第一个元素是一个字符串,其中包含要执行的命令行(可以传递给
   ``os.system()``),第二个元素是给定MIME类型的mailcap条目. 如果没有匹配
   的MIME类型,``(None,None)``会被返回. 



   *key* is the name of the field desired, which represents the type of activity to
   be performed; the default value is 'view', since in the  most common case you
   simply want to view the body of the MIME-typed data.  Other possible values
   might be 'compose' and 'edit', if you wanted to create a new body of the given
   MIME type or alter the existing body data.  See :rfc:`1524` for a complete list
   of these fields.

   *key*是需要被填入的名称,它表示要被执行的活动类型; 默认值是'view',因为在
   最常见的情况下,你只是想查看MIME类型的数据. 其他可能的值是'compose'和
   'edit',如果你想创建一个给定MIME类型的新机构或者想改变现有机构的数据. 请
   在**RFC 1524**上查找相关领域的完整清单. 


   *filename* is the filename to be substituted for ``%s`` in the command line; the
   default value is ``'/dev/null'`` which is almost certainly not what you want, so
   usually you'll override it by specifying a filename.

    *filename*在命令行中要被替代为``%s``; 默认值是``'/dev/null'``,这个值
   几乎肯定不是你想要的,所以通常你会用一个指定的文件名覆盖它. 


   *plist* can be a list containing named parameters; the default value is simply
   an empty list.  Each entry in the list must be a string containing the parameter
   name, an equals sign (``'='``), and the parameter's value.  Mailcap entries can
   contain  named parameters like ``%{foo}``, which will be replaced by the value
   of the parameter named 'foo'.  For example, if the command line ``showpartial
   %{id} %{number} %{total}`` was in a mailcap file, and *plist* was set to
   ``['id=1', 'number=2', 'total=3']``, the resulting command line would be
   ``'showpartial 1 2 3'``.

     *plist*可以是一个包含指定参数的列表; 默认值仅仅是一个空列表. 在列表中的每个
   条目必须是字符串,其中包含参数的名称,一个等号(``'='``)和参数的值. Mailcap
   条目可以包含像``%{foo}``这样的指定参数,它会被名为'foo'的参数替代. 例如,
   如果在命令行``showpartial %{id} %{number} %{total}``是在一个mailcap
   文件中,并且*plist*被设置成``['id=1','number=2','total=3'],命令行输出
   结果就是``'showpartial 1 2 3'``. 



   In a mailcap file, the "test" field can optionally be specified to test some
   external condition (such as the machine architecture, or the window system in
   use) to determine whether or not the mailcap line applies.  :func:`findmatch`
   will automatically check such conditions and skip the entry if the check fails.

   在mailcap文件中,*test*可以选择性的指定测试一些外部条件(如机器架构,或者
   使用中的视窗系统),以确定是否应用mailcap行. ``findmatch()``会自动检测这些
   条件,如果检查失败,会自动跳过条目. 



.. function:: getcaps()

   Returns a dictionary mapping MIME types to a list of mailcap file entries. This
   dictionary must be passed to the :func:`findmatch` function.  An entry is stored
   as a list of dictionaries, but it shouldn't be necessary to know the details of
   this representation.

   返回一个字典映射MIME类型到mailcap文件列表条目. 这个字典必须被传递到
   ``findmatch()``函数. 一个条目被储存为一个字典的列表,但他不需要知道这种
   表示的细节. 



   The information is derived from all of the mailcap files found on the system.
   Settings in the user's mailcap file :file:`$HOME/.mailcap` will override
   settings in the system mailcap files :file:`/etc/mailcap`,
   :file:`/usr/etc/mailcap`, and :file:`/usr/local/etc/mailcap`.

   该信息是从系统发现的所有mailcap文件中衍生出来. 在``$HOME/.mailcap``中用户
   的mailcap文件配置将覆盖掉在系统中``/etc/mailcap``,``/usr/etc/mailcap``
   和``/usr/local/etc/mailcap``的mailcap文件配置. 



An example usage::

一个例子的用法: 


   >>> import mailcap
   >>> d=mailcap.getcaps()
   >>> mailcap.findmatch(d, 'video/mpeg', filename='/tmp/tmp1223')
   ('xmpeg /tmp/tmp1223', {'view': 'xmpeg %s'})




