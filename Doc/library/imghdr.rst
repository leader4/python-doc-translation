:mod:`imghdr` --- 确认图片类型
================================================

.. module:: imghdr
   :synopsis: Determine the type of image contained in a file or byte stream.

**Source code:** :source:`Lib/imghdr.py`

--------------

The :mod:`imghdr` module determines the type of image contained in a file or
byte stream.

The :mod:`imghdr` module defines the following function:


.. function:: what(filename, h=None)

   Tests the image data contained in the file named by *filename*, and returns a
   string describing the image type.  If optional *h* is provided, the *filename*
   is ignored and *h* is assumed to contain the byte stream to test.

The following image types are recognized, as listed below with the return value
from :func:`what`:

+------------+-----------------------------------+
| Value      | Image format                      |
+============+===================================+
| ``'rgb'``  | SGI ImgLib Files                  |
+------------+-----------------------------------+
| ``'gif'``  | GIF 87a and 89a Files             |
+------------+-----------------------------------+
| ``'pbm'``  | Portable Bitmap Files             |
+------------+-----------------------------------+
| ``'pgm'``  | Portable Graymap Files            |
+------------+-----------------------------------+
| ``'ppm'``  | Portable Pixmap Files             |
+------------+-----------------------------------+
| ``'tiff'`` | TIFF Files                        |
+------------+-----------------------------------+
| ``'rast'`` | Sun Raster Files                  |
+------------+-----------------------------------+
| ``'xbm'``  | X Bitmap Files                    |
+------------+-----------------------------------+
| ``'jpeg'`` | JPEG data in JFIF or Exif formats |
+------------+-----------------------------------+
| ``'bmp'``  | BMP files                         |
+------------+-----------------------------------+
| ``'png'``  | Portable Network Graphics         |
+------------+-----------------------------------+

You can extend the list of file types :mod:`imghdr` can recognize by appending
to this variable:


.. data:: tests

   A list of functions performing the individual tests.  Each function takes two
   arguments: the byte-stream and an open file-like object. When :func:`what` is
   called with a byte-stream, the file-like object will be ``None``.

   The test function should return a string describing the image type if the test
   succeeded, or ``None`` if it failed.

Example::

   >>> import imghdr
   >>> imghdr.what('/tmp/bass.gif')
   'gif'


