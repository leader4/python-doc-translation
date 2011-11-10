:mod:`aifc` ---读、写音频交换文件格式AIFF和AIFC
==================================================

.. module:: aifc
   :synopsis: Read and write audio files in AIFF or AIFC format.


.. index::
   single: Audio Interchange File Format
   single: AIFF
   single: AIFF-C

**Source code:** :source:`Lib/aifc.py`
**源代码: **Lib/aifc.py

--------------

This module provides support for reading and writing AIFF and AIFF-C files.
AIFF is Audio Interchange File Format, a format for storing digital audio
samples in a file.  AIFF-C is a newer version of the format that includes the
ability to compress the audio data.

这个模块提供了对读, 写AIFF和AIFF-C文件的支持. AIFF指音频交换文件格式 (Audio Interchange File Format) , 是一种用来在一个文件内存储数字音频样本的格式. 
AIFF-C是一个具有压缩音频数据功能的AIFF格式的新版本. 

.. note::

   Some operations may only work under IRIX; these will raise :exc:`ImportError`
   when attempting to import the :mod:`cl` module, which is only available on
   IRIX.

   注意: 有些功能只能在IRIX(UNIX的一种) 上工作, 比如, 当您在非IRIX系统上试图导入'cl'模块 (仅能在IRIX时使用) 时, 就会出现 "导入错误" . 

Audio files have a number of parameters that describe the audio data. The
sampling rate or frame rate is the number of times per second the sound is
sampled.  The number of channels indicate if the audio is mono, stereo, or
quadro.  Each frame consists of one sample per channel.  The sample size is the
size in bytes of each sample.  Thus a frame consists of
*nchannels*\**samplesize* bytes, and a second's worth of audio consists of
*nchannels*\**samplesize*\**framerate* bytes.

声音文件有许多描述音频数据的参数. 采样率或帧速率是指每秒钟声音被采样的次数.
声道的的数量表明声音是单声道, 立体声, 或者quadro.每一帧包含了每一声道的一个采样. 
采样的大小是指采样所包含的字节数. 所以一帧包含的字节数为 n*通道数*采样大小 字节. 


For example, CD quality audio has a sample size of two bytes (16 bits), uses two
channels (stereo) and has a frame rate of 44,100 frames/second.  This gives a
frame size of 4 bytes (2\*2), and a second's worth occupies 2\*2\*44100 bytes
(176,400 bytes).

例如, CD品质的音效的采样大小为2字节 (16位) , 使用两个声道 (立体声) . 帧速率为44,100
帧/秒. 每一帧的大小为4字节 (2*2) , 每秒钟的值为2*2*44100字节 (176,400字节) 
Module ``aifc`` defines the following function:
模块'aifc'定义了下面的模块:

Module :mod:`aifc` defines the following function:


.. function:: open(file, mode=None)

   Open an AIFF or AIFF-C file and return an object instance with methods that are
   described below.  The argument *file* is either a string naming a file or a
   :term:`file object`.  *mode* must be ``'r'`` or ``'rb'`` when the file must be
   opened for reading, or ``'w'``  or ``'wb'`` when the file must be opened for writing.
   If omitted, ``file.mode`` is used if it exists, otherwise ``'rb'`` is used.  When
   used for writing, the file object should be seekable, unless you know ahead of
   time how many samples you are going to write in total and use
   :meth:`writeframesraw` and :meth:`setnframes`.

   函数打开一个AIFF或AIFF-C 文件并且返回一个具有下面描述的方法的对象实例. 
   参数*file*或者是一个表示文件名字的字符串或者是一个*file object*.当为了读而打开文件的
   时候, 参数*mode* 必须是'r'或 'rb', 当是为了写而打开文件的时候, 参数*mode*必须是 'w'
   或 'wb'.参数*mode*缺省时, 如果file.mode存在, 就用file.mode,否则, 使用'rb'.
   当文件用来写时, 该文件对象必须是可查找的, 除非你提前知道你总共将有多少样品要写入并且使用
    'wirteframeraw() 和 'setnframes()'方法. 


Objects returned by :func:`.open` when a file is opened for reading have the
following methods:
当为了读而用'open()' 打开一个文件时返回的对象具有下面的方法: 

.. method:: aifc.getnchannels()

   Return the number of audio channels (1 for mono, 2 for stereo).

   返回声道数 (1代表单声道, 2代表立体声) 


.. method:: aifc.getsampwidth()

   Return the size in bytes of individual samples.

   返回单个采样的字节大小


.. method:: aifc.getframerate()

   Return the sampling rate (number of audio frames per second).
   返回采样率 (每秒钟的音频帧的个数) 


.. method:: aifc.getnframes()

   Return the number of audio frames in the file.
   	返回文件中音频帧的个数


.. method:: aifc.getcomptype()

   Return a bytes array of length 4 describing the type of compression
   used in the audio file.  For AIFF files, the returned value is
   ``b'NONE'``.
   
    返回一个用以描述在文件中使用的压缩方法的4字节数组. 对AIFF文件来说, 返回值是
	 "b'NONE'".

.. method:: aifc.getcompname()

   Return a bytes array convertible to a human-readable description
   of the type of compression used in the audio file.  For AIFF files,
   the returned value is ``b'not compressed'``.
   
   返回一个用以描述在文件中使用的压缩方法的字节数组, 该字节数组的内容是易于被人所理解
	的. 对AIFF文件来说, 返回值是"b'not compressed'"


.. method:: aifc.getparams()

   Return a tuple consisting of all of the above values in the above order.
   
   返回一个元组, 该院组包含了以上所述的所有参数, 并以以上所述的顺序排列. 

.. method:: aifc.getmarkers()

   Return a list of markers in the audio file.  A marker consists of a tuple of
   three elements.  The first is the mark ID (an integer), the second is the mark
   position in frames from the beginning of the data (an integer), the third is the
   name of the mark (a string).
   
   返回声音文件中标记的一个列表. 一个标记包含了一个有三个元素的元组. 第一个是标记ID(
	一个整型数) , 第二个是在帧中自数据段开始的标记位置 (一个整型数) , 第三个是标记的
	名字 (一个字符串) . 

.. method:: aifc.getmark(id)

   Return the tuple as described in :meth:`getmarkers` for the mark with the given
   *id*.
   
 返回由参数id指定的标记的描述元组 (格式如getmarkers函数中所示) 

.. method:: aifc.readframes(nframes)

   Read and return the next *nframes* frames from the audio file.  The returned
   data is a string containing for each frame the uncompressed samples of all
   channels.
   
   读出并且返回下n个音频文件中的帧. 返回的数据为字符串, 它包含了每一帧未经压缩的所有声道的采样. 

.. method:: aifc.rewind()

   Rewind the read pointer.  The next :meth:`readframes` will start from the
   beginning.
   
   到会读文件的指针. 接下来的'readframes()函数会作用于文件开始处. 

.. method:: aifc.setpos(pos)

   Seek to the specified frame number.
   
  寻找指定序号的帧

.. method:: aifc.tell()

   Return the current frame number.

返回当前帧的序号

.. method:: aifc.close()

   Close the AIFF file.  After calling this method, the object can no longer be
   used.
   
   关闭AIFF文件. 当使用此函数后, 该对象就不能再被使用

Objects returned by :func:`.open` when a file is opened for writing have all the
above methods, except for :meth:`readframes` and :meth:`setpos`.  In addition
the following methods exist.  The :meth:`get\*` methods can only be called after
the corresponding :meth:`set\*` methods have been called.  Before the first
:meth:`writeframes` or :meth:`writeframesraw`, all parameters except for the
number of frames must be filled in.


    为了写而使用'open'函数打开的一个文件所返回的对象可以使用以上的所有方法, 除了'
	readframes()'和'setpos()'.此外, 有下面的方法存在.  'get* () ' 方法只有在与其
	相关连的'set*() '方法被调用之后才能被调用. 在'writeframes() '或' writeframes-
	raw()'第一次被调用之前, 除了帧数之外的所有参数都必须被填写. 


.. method:: aifc.aiff()

   Create an AIFF file.  The default is that an AIFF-C file is created, unless the
   name of the file ends in ``'.aiff'`` in which case the default is an AIFF file.

创建一个AIFF文件. 默认创建的是AIFF-C文件, 除非创建的文件的后缀名为 '.aiff',该种情
   况下创建的是AIFF文件. 

.. method:: aifc.aifc()

   Create an AIFF-C file.  The default is that an AIFF-C file is created, unless
   the name of the file ends in ``'.aiff'`` in which case the default is an AIFF
   file.
   
创建一个AIFF-C文件. 默认创建的是AIFF-C文件, 除非创建的文件的后缀名为 '.aiff',
	该种情况下创建的是AIFF文件. 

.. method:: aifc.setnchannels(nchannels)

   Specify the number of channels in the audio file.
   指定音频文件中使用的声道数

.. method:: aifc.setsampwidth(width)

   Specify the size in bytes of audio samples.
   指定音频文件的大小 (字节) 

.. method:: aifc.setframerate(rate)

   Specify the sampling frequency in frames per second.
	指定帧中每秒钟的采样率

.. method:: aifc.setnframes(nframes)

   Specify the number of frames that are to be written to the audio file. If this
   parameter is not set, or not set correctly, the file needs to support seeking.

   指定将要写入音频文件中帧的个数. 

.. method:: aifc.setcomptype(type, name)

   .. index::
      single: u-LAW
      single: A-LAW
      single: G.722

   Specify the compression type.  If not specified, the audio data will
   not be compressed.  In AIFF files, compression is not possible.
   The name parameter should be a human-readable description of the
   compression type as a bytes array, the type parameter should be a
   bytes array of length 4.  Currently the following compression types
   are supported: ``b'NONE'``, ``b'ULAW'``, ``b'ALAW'``, ``b'G722'``.
   
   指定压缩方式. 如果未指定, 音频数据将不会被压缩. 在AIFF文件中, 数据不会被压缩. 参数
   'name'应该是一个容易理解的描述压缩方式的字节数组, 'type'参数应该是一个4字节长的
   数组. 当前版本支持以下压缩方式:'b' NONE,'b' ULAW,'b'ALAW, 'b'G722


.. method:: aifc.setparams(nchannels, sampwidth, framerate, comptype, compname)

   Set all the above parameters at once.  The argument is a tuple consisting of the
   various parameters.  This means that it is possible to use the result of a
   :meth:`getparams` call as argument to :meth:`setparams`.
   
立即设定以上的参数. 参数是一个包含各种参数的元组. 这意味着可以用调用'getparams()'
	函数获得的结果作为'setparams()'函数的参数

.. method:: aifc.setmark(id, pos, name)

   Add a mark with the given id (larger than 0), and the given name at the given
   position.  This method can be called at any time before :meth:`close`.
   
	利用给定的id(大于0) 和给定的名在 (name)在给定的地方设置标记. 这个函数可以在
	'close()'函数被调用之前的任何地方调用

.. method:: aifc.tell()

   Return the current write position in the output file.  Useful in combination
   with :meth:`setmark`.
   
   返回当前在输出的文件中写 (指针) 的位置.应与'setmark()'函数一起使用. 

.. method:: aifc.writeframes(data)

   Write data to the output file.  This method can only be called after the audio
   file parameters have been set.
   
	向输出文件写入数据. 这个函数只能在音频文件参数设置之后才能调用

.. method:: aifc.writeframesraw(data)

   Like :meth:`writeframes`, except that the header of the audio file is not
   updated.
   
	类似于'writeframes()',除了音频文件的头部不被更新. 

.. method:: aifc.close()

   Close the AIFF file.  The header of the file is updated to reflect the actual
   size of the audio data. After calling this method, the object can no longer be
   used.
   
	关闭AIFF文件. 文件的头部信息会被更新以反映音频文件的实际大小. 调用这个函数之后, 
	文件对象将不能再被使用. 

