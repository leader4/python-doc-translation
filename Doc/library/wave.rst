:mod:`wave` --- 读写WAV文件
========================================

.. module:: wave
   :synopsis: Provide an interface to the WAV sound format.
.. sectionauthor:: Moshe Zadka <moshez@zadka.site.co.il>
.. Documentations stolen from comments in file.

**Source code:** :source:`Lib/wave.py`

--------------

The :mod:`wave` module provides a convenient interface to the WAV sound format.
It does not support compression/decompression, but it does support mono/stereo.

:mod "wave" 模块为WAV声音格式提供了一个方便的接口. 它不支持压缩/解压缩,但它确实
支持单声道/立体声. 


The :mod:`wave` module defines the following function and exception:

``wave``模块定义了以下的功能和异常: 


.. function:: open(file, mode=None)

   If *file* is a string, open the file by that name, otherwise treat it as a
   seekable file-like object.  *mode* can be any of

    如果*file*是一个字符串,打开该名称的文件,否则视
   它为一个可搜索的文件一样的对象.  *mode*可为任意

   ``'r'``, ``'rb'``
      Read only mode.

       只读模式.

   ``'w'``, ``'wb'``
      Write only mode.

       只写模式.

   Note that it does not allow read/write WAV files.

    注意,它不容许读/写WAV文件. 


   A *mode* of ``'r'`` or ``'rb'`` returns a :class:`Wave_read` object, while a
   *mode* of ``'w'`` or ``'wb'`` returns a :class:`Wave_write` object.  If
   *mode* is omitted and a file-like object is passed as *file*, ``file.mode``
   is used as the default value for *mode* (the ``'b'`` flag is still added if
   necessary).

    一个*mode*的``'r'`` 或 ``'rb'``返回一个 "Wave_read '对象,
   虽然一个 *mode* 的``'w'`` '或``'wb'``返回一个 "Wave_write '
   对象. 如果*mode*省略和一个file-like对象是通过 *file*, ``file.mode``将作为默认值(
   ``'b'``如果有必要还需要标识补充). 


   If you pass in a file-like object, the wave object will not close it when its
   :meth:`close` method is called; it is the caller's responsibility to close
   the file object.

   如果你通过一个file-like对象,当``close()``方法被调用时wave对象不会关闭它. 
   ,它是关闭文件对象调用者的责任. 


.. function:: openfp(file, mode)


   A synonym for :func:`.open`, maintained for backwards compatibility.

     ``open()``的代名词,保持向后兼容了.


.. exception:: Error

   An error raised when something is impossible because it violates the WAV
   specification or hits an implementation deficiency.

    提出错误的某件事情是不可能时,因为它违反WAV规格或点击缺乏执行情况. 


.. _wave-read-objects:

Wave_read Objects
-----------------

Wave_read objects, as returned by :func:`.open`, have the following methods:

``open()``,返回Wave_read对象,有以下几种方法: 


.. method:: Wave_read.close()

   Close the stream if it was opened by :mod:`wave`, and make the instance
   unusable.  This is called automatically on object collection.

   如果它被 "wave" 打开了则关闭流,使实例无法使用. 
   这就是所谓的自动对象的集合. 


.. method:: Wave_read.getnchannels()

   Returns number of audio channels (``1`` for mono, ``2`` for stereo).

    返回的音频通道数 (``1``单声道,``2``立体声) . 


.. method:: Wave_read.getsampwidth()

   Returns sample width in bytes.

   返回以字节为单位的样试宽度. 


.. method:: Wave_read.getframerate()

   Returns sampling frequency.

   返回采样频率. 


.. method:: Wave_read.getnframes()

   Returns number of audio frames.

     返回音频帧的数量. 


.. method:: Wave_read.getcomptype()

   Returns compression type (``'NONE'`` is the only supported type).

   返回压缩类型 (``'NONE'``是唯一支持的类型) . 


.. method:: Wave_read.getcompname()

   Human-readable version of :meth:`getcomptype`. Usually ``'not compressed'``
   parallels ``'NONE'``.

   人们可读的版本`` getcomptype()``.一般``'notcompressed'``与``'NONE'``相似.


.. method:: Wave_read.getparams()

   Returns a tuple ``(nchannels, sampwidth, framerate, nframes, comptype,
   compname)``, equivalent to output of the :meth:`get\*` methods.

   返回一个元组 " (nchannels,sampwidth,framerate,nframes,comptype,compname) " ,
   相当于输出``get*()`` 方法. 


.. method:: Wave_read.readframes(n)

   Reads and returns at most *n* frames of audio, as a string of bytes.

    Reads and returns at most *n* frames of audio, as a string of
   bytes.
   在最多的*n*音频信号帧,读取并返回一个字符串. 


.. method:: Wave_read.rewind()

   Rewind the file pointer to the beginning of the audio stream.

    倒回文件指针的音频数据流的开始. 


The following two methods are defined for compatibility with the :mod:`aifc`
module, and don't do anything interesting.

以下两种方法被定义为与 "AIFC" 模块的兼容性,并没有做什么有趣的. 


.. method:: Wave_read.getmarkers()

   Returns ``None``.

    返回``None``.


.. method:: Wave_read.getmark(id)

   Raise an error.

    抛出一个错误.

The following two methods define a term "position" which is compatible between
them, and is otherwise implementation dependent.

以下两种方法定义一个长期的 "位置" ,这是他们之间的兼容,另有依赖于实现. 


.. method:: Wave_read.setpos(pos)

   Set the file pointer to the specified position.

   设置文件指针到指定位置. 


.. method:: Wave_read.tell()

   Return current file pointer position.

   返回当前文件指针的位置. 


.. _wave-write-objects:

Wave_write Objects
------------------

Wave_write objects, as returned by :func:`.open`, have the following methods:

``open()``返回Wave_write对象,有以下几种方法: 


.. method:: Wave_write.close()

   Make sure *nframes* is correct, and close the file if it was opened by
   :mod:`wave`.  This method is called upon object collection.

   确保*nframes* 是正确的,如果它是由``wave``打开并关闭该文件. 
   这种方法被称为对对象集合. 


.. method:: Wave_write.setnchannels(n)

   Set the number of channels.

    设置的通道数量


.. method:: Wave_write.setsampwidth(n)

   Set the sample width to *n* bytes.

   给*n*设置样本宽度字节. 


.. method:: Wave_write.setframerate(n)

   Set the frame rate to *n*.

   给*N*设置帧速率


   .. versionchanged:: 3.2
      A non-integral input to this method is rounded to the nearest
      integer.

      在3.2版本中更改: 这种方法是一个非整数输入四舍五入到最接近的整数. 


.. method:: Wave_write.setnframes(n)

   Set the number of frames to *n*. This will be changed later if more frames are
   written.

   以后如果有更多的帧被写入,那么*n*设置帧的数量这将被改变,. 


.. method:: Wave_write.setcomptype(type, name)

   Set the compression type and description. At the moment, only compression type
   ``NONE`` is supported, meaning no compression.

   设置的压缩类型和描述. 目前,仅有压缩型``NONE``支持,
   这意味着没有压缩..



.. method:: Wave_write.setparams(tuple)

   The *tuple* should be ``(nchannels, sampwidth, framerate, nframes, comptype,
   compname)``, with values valid for the :meth:`set\*` methods.  Sets all
   parameters.

   *tuple*应 "`(nchannels, sampwidth, framerate, nframes,
   comptype, compname)``,有效的``set*()``的方法与价值观. 设置的所有参数. 


.. method:: Wave_write.tell()

   Return current position in the file, with the same disclaimer for the
   :meth:`Wave_read.tell` and :meth:`Wave_read.setpos` methods.

    返回文件的当前位置,
   ``Wave_read.tell()``和``Wave_read.setpos()``方法有相同的免责声明. 


.. method:: Wave_write.writeframesraw(data)

   Write audio frames, without correcting *nframes*.

   写的音频帧,不纠正* nframes*.


.. method:: Wave_write.writeframes(data)

   Write audio frames and make sure *nframes* is correct.

    写的音频帧,并确保*nframes* 是正确的. 


Note that it is invalid to set any parameters after calling :meth:`writeframes`
or :meth:`writeframesraw`, and any attempt to do so will raise
:exc:`wave.Error`.

请注意,任何参数设置后调用``writeframes()`` 或者 ``writeframesraw()``它将会是无效的,
任何尝试只做会提高 "wave.Error" . 








