:mod:`winsound` --- Windows声音播放接口
=======================================================

.. module:: winsound
   :platform: Windows
   :synopsis: Access to the sound-playing machinery for Windows.
.. moduleauthor:: Toby Dickenson <htrd90@zepler.org>
.. sectionauthor:: Fred L. Drake, Jr. <fdrake@acm.org>


---------------------------------------------------------------------------

The :mod:`winsound` module provides access to the basic sound-playing machinery
provided by Windows platforms.  It includes functions and several constants.

``winsound``模块访问Windows平台的基本声音装置. 它包含了一些方法和一些常量。

--------------------------------------------------------------------------- 

.. function:: Beep(frequency, duration)

   Beep the PC's speaker. The *frequency* parameter specifies frequency, in hertz,
   of the sound, and must be in the range 37 through 32,767. The *duration*
   parameter specifies the number of milliseconds the sound should last.  If the
   system is not able to beep the speaker, :exc:`RuntimeError` is raised.

   PC扬声器蜂鸣.
   *frequency* 参数规定了声音的赫兹频率，必须在37到32767的范围。
   *duration* 参数规定了声音的持续毫秒数.
        如果系统扬声器不能蜂鸣，抛出``RuntimeError``。

---------------------------------------------------------------------------


.. function:: PlaySound(sound, flags)

   Call the underlying :c:func:`PlaySound` function from the Platform API.  The
   *sound* parameter may be a filename, audio data as a string, or ``None``.  Its
   interpretation depends on the value of *flags*, which can be a bitwise ORed
   combination of the constants described below. If the *sound* parameter is
   ``None``, any currently playing waveform sound is stopped. If the system
   indicates an error, :exc:`RuntimeError` is raised.


   从平台API调用底层的``PlaySound()``方法.
   *sound* 参数可能是一个文件名, 音频数据的一个字符串, 或者``None``.
   其解释依赖于*flags*的值，这个值能够是一个位级别的或者位级别组合的常量。如下文的
   描述。如果*sound*参数是``None``, 停止任何当前正在播放的声音. 如果系统出现一个错误,
   ``RuntimeError``将被抛出。

---------------------------------------------------------------------------


.. function:: MessageBeep(type=MB_OK)

   Call the underlying :c:func:`MessageBeep` function from the Platform API.  This
   plays a sound as specified in the registry.  The *type* argument specifies which
   sound to play; possible values are ``-1``, ``MB_ICONASTERISK``,
   ``MB_ICONEXCLAMATION``, ``MB_ICONHAND``, ``MB_ICONQUESTION``, and ``MB_OK``, all
   described below.  The value ``-1`` produces a "simple beep"; this is the final
   fallback if a sound cannot be played otherwise.

   从平台API调用底层的``PlaySound()``方法.
   *sound* 参数可能是一个文件名, 音频数据的一个字符串, 或者``None``.
   其解释依赖于*flags*的值，这个值能够是一个位级别的或者位级别组合的常量。如下文的
   描述。如果*sound*参数是``None``, 停止任何当前正在播放的声音. 如果系统出现一个错误,
   ``RuntimeError``将被抛出。

---------------------------------------------------------------------------


.. data:: SND_FILENAME

   The *sound* parameter is the name of a WAV file. Do not use with
   :const:`SND_ALIAS`.

   *sound* 参数是一个WAV文件的名称. 不要使用``SND_ALIAS``.

---------------------------------------------------------------------------



.. data:: SND_ALIAS

   The *sound* parameter is a sound association name from the registry.  If the
   registry contains no such name, play the system default sound unless
   :const:`SND_NODEFAULT` is also specified. If no default sound is registered,
   raise :exc:`RuntimeError`. Do not use with :const:`SND_FILENAME`.

   *sound*是注册表中关联的一个声音. 如果注册表没有它, ``SND_NODEFAULT``也没有
   指定，播放系统默认声音. 如果没有注册默认声音，抛出``RuntimeError``. 不要使用
   ``SND_FILENAME``.

---------------------------------------------------------------------------

   All Win32 systems support at least the following; most systems support many
   more:

   所有的32系统至少支持如下; 大多数系统支持得更多：

   +--------------------------+----------------------------------------+
   | :func:`PlaySound` *name* | Corresponding Control Panel Sound name |
   +==========================+========================================+
   | ``'SystemAsterisk'``     | Asterisk                               |
   +--------------------------+----------------------------------------+
   | ``'SystemExclamation'``  | Exclamation                            |
   +--------------------------+----------------------------------------+
   | ``'SystemExit'``         | Exit Windows                           |
   +--------------------------+----------------------------------------+
   | ``'SystemHand'``         | Critical Stop                          |
   +--------------------------+----------------------------------------+
   | ``'SystemQuestion'``     | Question                               |
   +--------------------------+----------------------------------------+



---------------------------------------------------------------------------

  
    PlaySound()                     对应控制面版的声音名称                

        ``'SystemAsterisk'``        星号                                      
        
        ``'SystemExclamation'``     感叹                                      
        
        ``'SystemExit'``            离开                                      
        
        ``'SystemHand'``            重要的停止                                 
        
        ``'SystemQuestion'``        问题                                      
        

---------------------------------------------------------------------------




   For example::

      import winsound
      # Play Windows exit sound.

      # 播放Windows推出的声音

---------------------------------------------------------------------------


      winsound.PlaySound("SystemExit", winsound.SND_ALIAS)


      # Probably play Windows default sound, if any is registered (because
      # "*" probably isn't the registered name of any sound).

       # 可能播放Windows默认声音, 如果这个声音在注册表中注册(因为
      # "*" 可能不是一个注册的声音名字).

---------------------------------------------------------------------------


      winsound.PlaySound("*", winsound.SND_ALIAS)


---------------------------------------------------------------------------

.. data:: SND_LOOP

   Play the sound repeatedly.  The :const:`SND_ASYNC` flag must also be used to
   avoid blocking.  Cannot be used with :const:`SND_MEMORY`.

    重复播放声音. ``SND_ASYNC``标志必须使用防止阻塞。不能使用``SND_MEMORY``.

---------------------------------------------------------------------------


.. data:: SND_MEMORY

   The *sound* parameter to :func:`PlaySound` is a memory image of a WAV file, as a
   string.

   *sound*作为``PlaySound()``的参数是一个WAV文件的内存镜像的字符窜。

   .. note::

      This module does not support playing from a memory image asynchronously, so a
      combination of this flag and :const:`SND_ASYNC` will raise :exc:`RuntimeError`.

      注意: 这个模块不支持异步播放内存镜像, 所以同时使用它和``SND_ASYNC``将抛出``RuntimeError``.

---------------------------------------------------------------------------


.. data:: SND_PURGE

   Stop playing all instances of the specified sound.

    停止所有正在播放指定声音的实例.

---------------------------------------------------------------------------

   .. note::

      This flag is not supported on modern Windows platforms.

       注意：这个标志不支持最新的Windows平台。

---------------------------------------------------------------------------


.. data:: SND_ASYNC

   Return immediately, allowing sounds to play asynchronously.

   立即返回，允许异步播放声音

---------------------------------------------------------------------------


.. data:: SND_NODEFAULT

   If the specified sound cannot be found, do not play the system default sound.

    如果制定的声音没有被找到，不会播放系统默认声音。

---------------------------------------------------------------------------


.. data:: SND_NOSTOP

   Do not interrupt sounds currently playing.

   不中断正在播放的声音。

---------------------------------------------------------------------------


.. data:: SND_NOWAIT

   Return immediately if the sound driver is busy.

    如果声音驱动忙立即返回

---------------------------------------------------------------------------


.. data:: MB_ICONASTERISK

   Play the ``SystemDefault`` sound.

    播放``SystemDefault``声音.

---------------------------------------------------------------------------


.. data:: MB_ICONEXCLAMATION

   Play the ``SystemExclamation`` sound.

    播放``SystemExclamation``声音.

---------------------------------------------------------------------------


.. data:: MB_ICONHAND

   Play the ``SystemHand`` sound.

    播放``SystemHand``声音.

---------------------------------------------------------------------------

.. data:: MB_ICONQUESTION

   Play the ``SystemQuestion`` sound.

   播放``SystemQuestion``声音.


---------------------------------------------------------------------------

.. data:: MB_OK

   Play the ``SystemDefault`` sound.

   播放``SystemDefault``声音






