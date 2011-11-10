
:mod:`nis` ---Sun的NIS接口
====================================================

.. module:: nis
   :platform: Unix
   :synopsis: Interface to Sun's NIS (Yellow Pages) library.
.. moduleauthor:: Fred Gansevles <Fred.Gansevles@cs.utwente.nl>
.. sectionauthor:: Moshe Zadka <moshez@zadka.site.co.il>


The :mod:`nis` module gives a thin wrapper around the NIS library, useful for
central administration of several hosts.

``nis``模块提供了NIS库的一层很薄的包装, 对几台主机的中央管理来说有用. 


Because NIS exists only on Unix systems, this module is only available for Unix.

因为NIS仅存在在Unix系统, 这个模块仅对Unix有效.


The :mod:`nis` module defines the following functions:

The ``nis`` 定义了如下的方法:


.. function:: match(key, mapname[, domain=default_domain])

   Return the match for *key* in map *mapname*, or raise an error
   (:exc:`nis.error`) if there is none. Both should be strings, *key* is 8-bit
   clean. Return value is an arbitrary array of bytes (may contain ``NULL`` and
   other joys).
   
   返回*mapname*中*key*的匹配,或者抛出一个错误(``nis.error``)如果没有匹配到. 
   它们 (mapname 和 key) 都应该是字符串. *key* 是一个8位的. 返回值是一个任意的
   字节(可能包含``NULL``和其他的有意思的值).
   

   Note that *mapname* is first checked if it is an alias to another name.
   
    注意*mapname*如果另外一个名字的别名,会第一个被检查.
    

   The *domain* argument allows to override the NIS domain used for the lookup. If
   unspecified, lookup is in the default NIS domain.
   
    *domain*参数允许覆盖NIS域.  如果没有指定,查找默认的NIS域.


.. function:: cat(mapname[, domain=default_domain])

   Return a dictionary mapping *key* to *value* such that ``match(key,
   mapname)==value``. Note that both keys and values of the dictionary are
   arbitrary arrays of bytes.
   
    返回一个*key*到*value*隐射的字典, 用于``match(key, mapname)==value``. 
   注意字典中所有的keys和values都是任意的字节数组. 
   

   Note that *mapname* is first checked if it is an alias to another name.
   
   注意*mapname*如果另外一个名字的别名,会第一个被检查.
   

   The *domain* argument allows to override the NIS domain used for the lookup. If
   unspecified, lookup is in the default NIS domain.
   
   *domain*参数允许覆盖NIS域.  如果没有指定,查找默认的NIS域.


.. function:: maps([domain=default_domain])

   Return a list of all valid maps.
   
   返回一个所有正确隐射的列表.
   

   The *domain* argument allows to override the NIS domain used for the lookup. If
   unspecified, lookup is in the default NIS domain.
   
   *domain*参数允许覆盖NIS域.  如果没有指定,查找默认的NIS域.


.. function:: get_default_domain()

   Return the system default NIS domain.
   
   返回系统的默认NIS域.


The :mod:`nis` module defines the following exception:

``nis`` 模块定义了下面的异常:

.. exception:: error

   An error raised when a NIS function returns an error code.  
   
   当NIS函数返回一个错误代码,一个错误被抛出. 
   
   
   


