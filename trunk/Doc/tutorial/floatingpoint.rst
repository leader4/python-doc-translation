.. _tut-fp-issues:

**************************************************
浮点运算: 问题和限制
**************************************************

.. sectionauthor:: Tim Peters <tim_one@users.sourceforge.net>


Floating-point numbers are represented in computer hardware as base 2 (binary)
fractions.  For example, the decimal fraction ::

浮点数在计算机硬件中以二进制的小数来表示. 比如, 十进制的小数::

   0.125

has value 1/10 + 2/100 + 5/1000, and in the same way the binary fraction ::

它的值为 1/10 + 2/100 + 5/1000, 同样在二进制小数中::

   0.001

has value 0/2 + 0/4 + 1/8.  These two fractions have identical values, the only
real difference being that the first is written in base 10 fractional notation,
and the second in base 2.

它的值是 0/2 + 0/4 + 1/8. 这两个小数的值相等, 唯一的差别是前一个是10进制的, 后一个是二进制的.

Unfortunately, most decimal fractions cannot be represented exactly as binary
fractions.  A consequence is that, in general, the decimal floating-point
numbers you enter are only approximated by the binary floating-point numbers
actually stored in the machine.

不幸的是, 多数的十进制小数不能被精确的表示为二进制小数. 导致的结果是, 计算机中存储的浮点数只是我们输入的十进制浮点数的近似值.

The problem is easier to understand at first in base 10.  Consider the fraction
1/3.  You can approximate that as a base 10 fraction::

为了便于的理解, 我们举个十进制的例子. 考虑一下分数 1/3. 你可以把它写成十进制小数形式::

   0.3

or, better, ::

再精确一点::

   0.33

or, better, ::

更加精确::

   0.333

and so on.  No matter how many digits you're willing to write down, the result
will never be exactly 1/3, but will be an increasingly better approximation of
1/3.

有可能变得更精确. 但是无论写多少个数字, 结果永远不会正好等于分数 1/3, 我们只能得到更近似于 1/3 的值.

In the same way, no matter how many base 2 digits you're willing to use, the
decimal value 0.1 cannot be represented exactly as a base 2 fraction.  In base
2, 1/10 is the infinitely repeating fraction ::

同样, 不管使用多少个二进制数字, 十进制的值 0.1 无法精确的用二进制小数来表示. 在二进制中, 1/10 是一个无限循环小数::

   0.0001100110011001100110011001100110011001100110011...

Stop at any finite number of bits, and you get an approximation.  On most
machines today, floats are approximated using a binary fraction with
the numerator using the first 53 bits starting with the most significant bit and
with the denominator as a power of two.  In the case of 1/10, the binary fraction
is ``3602879701896397 / 2 ** 55`` which is close to but not exactly
equal to the true value of 1/10.

在任意有限的位上停止, 可以得到一个近似值. 在今天的大多数机子上,
浮点数近似地使用二元的分数来表示, 其分子是一个 53 位的数字,
分母则是 2 的幂. 像 1/10, 其二进制表示为 ``3602879701896397 / 2 ** 55``.
这个很接近, 但不是准确的等于.

Many users are not aware of the approximation because of the way values are
displayed.  Python only prints a decimal approximation to the true decimal
value of the binary approximation stored by the machine.  On most machines, if
Python were to print the true decimal value of the binary approximation stored
for 0.1, it would have to display ::

取有限的位数, 得到一个近似值.对于大多数的计算机来说, 当你在Python提示符上输入 0.1 你会看到, 你会看到上面的. 如果不是, 那可能是因为在不同机器上硬件用来存储浮点数的内存大小不一样, 另外, Python 只打印机器存储的二进制对应十进制的近似值. 在大多数机器上, 如果Python打印出二进制 0.1 近似值对应的真实十进制的话, 那么会显示
权文博
   >>> 0.1
   0.1000000000000000055511151231257827021181583404541015625

That is more digits than most people find useful, so Python keeps the number
of digits manageable by displaying a rounded value instead ::

   >>> 1 / 10
   0.1

Just remember, even though the printed result looks like the exact value
of 1/10, the actual stored value is the nearest representable binary fraction.

只要记住, 尽管打印的值看起来是准确的 1/10 , 但是真正存储的只是最接近的二元分数罢了.

Interestingly, there are many different decimal numbers that share the same
nearest approximate binary fraction.  For example, the numbers ``0.1`` and
``0.10000000000000001`` and
``0.1000000000000000055511151231257827021181583404541015625`` are all
approximated by ``3602879701896397 / 2 ** 55``.  Since all of these decimal
values share the same approximation, any one of them could be displayed
while still preserving the invariant ``eval(repr(x)) == x``.

Historically, the Python prompt and built-in :func:`repr` function would choose
the one with 17 significant digits, ``0.10000000000000001``.   Starting with
Python 3.1, Python (on most systems) is now able to choose the shortest of
these and simply display ``0.1``.

历史原因, Python 的提示符和内置的 :func:`repr` 函数会选择 17 位,
``0.10000000000000001`` . 在 Python 3.1 开始, Python (在绝大数的系统上),
会选择最简的表示 ``0.1``.

Note that this is in the very nature of binary floating-point: this is not a bug
in Python, and it is not a bug in your code either.  You'll see the same kind of
thing in all languages that support your hardware's floating-point arithmetic
(although some languages may not *display* the difference by default, or in all
output modes).

注意, 二进制表示的浮点数在此处是非常自然的: 这个不是 Python 中的 bug,
也不是你代码中的 bug . 在很多支持硬件浮点型的语言中也可以看到这样的事情
(尽管有些语言默认下不会显示不同, 或在所有的输出模式下).

For more pleasant output, you may may wish to use string formatting to produce a limited number of significant digits:

对于更多友好的输出, 你可能需要使用字符串格式化来产生一个有限制的数字::

   >>> format(math.pi, '.12g')  # give 12 significant digits
   '3.14159265359'

   >>> format(math.pi, '.2f')   # give 2 digits after the point
   '3.14'

   >>> repr(math.pi)
   '3.141592653589793'


It's important to realize that this is, in a real sense, an illusion: you're
simply rounding the *display* of the true machine value.

有一点很重要, 你需要意识到, 在真实情况下, 这是个幻觉:
你仅仅是四舍五入了显示的真实值.

One illusion may beget another.  For example, since 0.1 is not exactly 1/10,
summing three values of 0.1 may not yield exactly 0.3, either::

   >>> .1 + .1 + .1 == .3
   False

Also, since the 0.1 cannot get any closer to the exact value of 1/10 and
0.3 cannot get any closer to the exact value of 3/10, then pre-rounding with
:func:`round` function cannot help::

   >>> round(.1, 1) + round(.1, 1) + round(.1, 1) == round(.3, 1)
   False

Though the numbers cannot be made closer to their intended exact values,
the :func:`round` function can be useful for post-rounding so that results
with inexact values become comparable to one another::

    >>> round(.1 + .1 + .1, 10) == round(.3, 10)
    True

Binary floating-point arithmetic holds many surprises like this.  The problem
with "0.1" is explained in precise detail below, in the "Representation Error"
section.  See `The Perils of Floating Point <http://www.lahey.com/float.htm>`_
for a more complete account of other common surprises.

二进制浮点运算给我们带来很多这样的 "惊喜" . " 0.1" 带来的问题在本文的" 表现错误" 小节中有着详细的解释. 另外,`The Perils of Floating Point <http://www.lahey.com/float.htm>`_ 中也有更加完整的描述. 

As that says near the end, "there are no easy answers."  Still, don't be unduly
wary of floating-point!  The errors in Python float operations are inherited
from the floating-point hardware, and on most machines are on the order of no
more than 1 part in 2\*\*53 per operation.  That's more than adequate for most
tasks, but you do need to keep in mind that it's not decimal arithmetic and
that every float operation can suffer a new rounding error.

最后我们要说, "没有完美的方法" . 但是,不要过分的拒绝浮点数! Python浮点操作中的错误是由硬件本身限制的,在大多数的机器上每次运算的误差不会超过2的53次方之一. 这样的误差在大多数的任务中是可以被接受的. 但是,我们还是要牢记我们做的并不是十进制运算,任何一个浮点预算都有可能产生一个新的错误. 

While pathological cases do exist, for most casual use of floating-point
arithmetic you'll see the result you expect in the end if you simply round the
display of your final results to the number of decimal digits you expect.
:func:`str` usually suffices, and for finer control see the :meth:`str.format`
method's format specifiers in :ref:`formatstrings`.

当不出意外的时候,通常情况下大多数的浮点运算我们会得到我们期望的结果,你只需要简单的取小数位,最终得到和十进制的一样的显示结果. str 函数基本上够用了,为了更好的控制我们还可以看看Python的``%``格式化操作符: ``%g``, ``%f``和``%e``格式化符能让我们灵活而简单的得到我们想要显示的结果. 

For use cases which require exact decimal representation, try using the
:mod:`decimal` module which implements decimal arithmetic suitable for
accounting applications and high-precision applications.

在需要严格的数值表示时, 试试使用 :mod:`decimal` 模块, 
这个模块实现了用于账目运算或更高精度时用到的数值算法.

Another form of exact arithmetic is supported by the :mod:`fractions` module
which implements arithmetic based on rational numbers (so the numbers like
1/3 can be represented exactly).

另一种就是 :mod:`fractions` 模块, 它实现了基于有理数的算法
( 所以 1/3 就可以准确的表述 ).

If you are a heavy user of floating point operations you should take a look
at the Numerical Python package and many other packages for mathematical and
statistical operations supplied by the SciPy project. See <http://scipy.org>.

如果你在工作中频繁的使用浮点数,你应该看一看为数学准备的Numerical Python库和为统计学操作准备的SciPy项目,参见 <http://scipy.org>.

Python provides tools that may help on those rare occasions when you really
*do* want to know the exact value of a float.  The
:meth:`float.as_integer_ratio` method expresses the value of a float as a
fraction:

Python 提供了工具来帮助你获得浮点数的准确值.
你可以使用 :meth:`float.as_integer_ratio` 方法来表示一个分数::

   >>> x = 3.14159
   >>> x.as_integer_ratio()
   (3537115888337719, 1125899906842624)

Since the ratio is exact, it can be used to losslessly recreate the
original value:

因为这个比率是准确的, 它就可以用来比较原始的数字::

    >>> x == 3537115888337719 / 1125899906842624
    True

The :meth:`float.hex` method expresses a float in hexadecimal (base
16), again giving the exact value stored by your computer:

:meth:`float.hex` 方法以十六进制表述,
这也同样给出了一个被你计算机准确存储的值::

   >>> x.hex()
   '0x1.921f9f01b866ep+1'

This precise hexadecimal representation can be used to reconstruct
the float value exactly:

前面的十六进制表示, 可以用来重新建立一个浮点值::

    >>> x == float.fromhex('0x1.921f9f01b866ep+1')
    True

Since the representation is exact, it is useful for reliably porting values
across different versions of Python (platform independence) and exchanging
data with other languages that support the same format (such as Java and C99).

因为这个表示是严格的, 所以对于不同版本的 Python (跨平台) 都是兼容的,
而且也可以和其他的语言进行交换 (比如 java 和 C99).

Another helpful tool is the :func:`math.fsum` function which helps mitigate
loss-of-precision during summation.  It tracks "lost digits" as values are
added onto a running total.  That can make a difference in overall accuracy
so that the errors do not accumulate to the point where they affect the
final total:

另一个有用的工具就是 :func:`math.fsum` 函数. 它可以在计算总和时减少精度的丢失.
它会记录在求和时丢失的精度. 这样误差就不会积累而最终影响结果了::

   >>> sum([0.1] * 10) == 1.0
   False
   >>> math.fsum([0.1] * 10) == 1.0
   True

.. _tut-fp-error:

表示错误
====================

This section explains the "0.1" example in detail, and shows how you can perform
an exact analysis of cases like this yourself.  Basic familiarity with binary
floating-point representation is assumed.

本节会更详细的解释 "0.1" 的例子, 并且教你如何进行准确的分析.
此处假设你已有了基本的二元浮点数表示的基础.

:dfn:`Representation error` refers to the fact that some (most, actually)
decimal fractions cannot be represented exactly as binary (base 2) fractions.
This is the chief reason why Python (or Perl, C, C++, Java, Fortran, and many
others) often won't display the exact decimal number you expect.

`表示错误` 源于事实上一些 (事实上是大多) 十进制分数不能精确表示为二进制分数. 这就是Python (

以及 Perl,C,C++,Java,Fortran 等等) 语言通常不会显示出你期望的十进制数值的原因: 

Why is that?  1/10 is not exactly representable as a binary fraction. Almost all
machines today (November 2000) use IEEE-754 floating point arithmetic, and
almost all platforms map Python floats to IEEE-754 "double precision".  754
doubles contain 53 bits of precision, so on input the computer strives to
convert 0.1 to the closest fraction it can of the form *J*/2**\ *N* where *J* is
an integer containing exactly 53 bits.  Rewriting ::

这是为什么?  1/10 不能被精确表示为二进制分数. 今天 (2000年十一月) 的大多数机器使用 IEEE-754 浮点算法,大多平台将 Python 浮点数对映为 IEEE-754  "双精度浮点数" .  754 双精度浮点数有 53 比特位精度,所以计算机输入时将 0.1 尽可能的转为最接近的 J/2***N* 形式.  J 是一个包含53比特的整数. 如下::

   1 / 10 ~= J / (2**N)

as ::

   J ~= 2**N / 10

and recalling that *J* has exactly 53 bits (is ``>= 2**52`` but ``< 2**53``),
the best value for *N* is 56::

前面提到 J 需要 53 比特位 (``大于等于 2**52`` 且 ``小于53``) , N 的最佳值是 56: 

    >>> 2**52 <=  2**56 // 10  < 2**53
    True

That is, 56 is the only value for *N* that leaves *J* with exactly 53 bits.  The
best possible value for *J* is then that quotient rounded::

于是,对于 J 期待的 53 位,56是 N 的唯一可选值.  J 的最佳可用值是下面计算出的范围: 

   >>> q, r = divmod(2**56, 10)
   >>> r
   6

Since the remainder is more than half of 10, the best approximation is obtained
by rounding up::

因为余数大于10,最好的逼近方法是由上界逼近 : 

   >>> q+1
   7205759403792794

Therefore the best possible approximation to 1/10 in 754 double precision is::

因此 1/10 在 754 双精度下最接近的是它比 2**56 ,即::

   7205759403792794 / 2 ** 56

Dividing both the numerator and denominator by two reduces the fraction to::

   3602879701896397 / 2 ** 55

Note that since we rounded up, this is actually a little bit larger than 1/10;
if we had not rounded up, the quotient would have been a little bit smaller than
1/10.  But in no case can it be *exactly* 1/10!

注意,因为这里我们取上界,所以它实际上比 1/10 大一点点. 如果我们取下界,就会比 1/10 小一点. 不过它不会 恰好 是 1/10 ! 

So the computer never "sees" 1/10:  what it sees is the exact fraction given
above, the best 754 double approximation it can get::

所以计算机无法 "理解"  1/10: 它理解收到的分数,给出它所能得到的最佳精度: 

   >>> 0.1 * 2 ** 55
   3602879701896397.0

If we multiply that fraction by 10\*\*55, we can see the value out to
55 decimal digits::

如果我们将这个分数乘10**55,我们可以看到 (被截断) 的55位十进制有效数字: 

   >>> 3602879701896397 * 10 ** 55 // 2 ** 55
   1000000000000000055511151231257827021181583404541015625

meaning that the exact number stored in the computer is equal to
the decimal value 0.1000000000000000055511151231257827021181583404541015625.
Instead of displaying the full decimal value, many languages (including
older versions of Python), round the result to 17 significant digits::

这意味着保存在计算机中的精确数值约等于十进制值 0.1000000000000000055511151231257827021181583404541015625::

   >>> format(0.1, '.17f')
   '0.10000000000000001'

The :mod:`fractions` and :mod:`decimal` modules make these calculations
easy:

:mod:`fractions` 和 :mod:`decimal` 模块使这些计算变得简单::

   >>> from decimal import Decimal
   >>> from fractions import Fraction

   >>> Fraction.from_float(0.1)
   Fraction(3602879701896397, 36028797018963968)

   >>> (0.1).as_integer_ratio()
   (3602879701896397, 36028797018963968)

   >>> Decimal.from_float(0.1)
   Decimal('0.1000000000000000055511151231257827021181583404541015625')

   >>> format(Decimal.from_float(0.1), '.17')
   '0.10000000000000001'

