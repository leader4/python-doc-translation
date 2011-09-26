.. _crypto:

**********************
Cryptographic Services

加密服务 译者 Lemon,゛ღ

**********************

.. index:: single: cryptography

The modules described in this chapter implement various algorithms of a
cryptographic nature.  They are available at the discretion of the installation.
Here's an overview:

本章中描述的这个模块用来实现各种性质的加密算法.他们可以被自由的安装使用.
以下是概述


.. toctree::

   hashlib.rst
   安全的散列和消息摘要


   hmac.rst
   消息键值散列验证


.. index::
   pair: AES; algorithm
   single: cryptography
   single: Kuchling, Andrew

Hardcore cypherpunks will probably find the cryptographic modules written by
A.M. Kuchling of further interest; the package contains modules for various
encryption algorithms, most notably AES.  These modules are not distributed with
Python but available separately.  See the URL
http://www.amk.ca/python/code/crypto.html  for more information.

骨灰级的密码爱好者们如果感兴趣可以找到A.M. Kuchling所写的
加密模块;这个包,涵盖了各种加密算法模块,最值得注意的是AES.
这些模块没有随Python一起分发,可以单独获取.请查看
网址 http://www.amk.ca/python/code/crypto.html  获取更多信息.

