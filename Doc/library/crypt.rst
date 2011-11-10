:mod:`crypt` --- 访问UNIX口令的函数
=================================================

.. module:: crypt
   :platform: Unix


   :synopsis: The crypt() function used to check Unix passwords.
.. moduleauthor:: Steven D. Majewski <sdm7g@virginia.edu>
.. sectionauthor:: Steven D. Majewski <sdm7g@virginia.edu>
.. sectionauthor:: Peter Funk <pf@artcom-gmbh.de>


.. index::
   single: crypt(3)
   pair: cipher; DES

这一模块实现了到crypt(3)例程的接口,这一例程是一个基于修改过的DES算法的单路哈希函
数,更多详细信息请参见Unix man手册.此一函数可能的用法包括允许Python 脚本接收用户
键入的密码,或使用字典试图破解UNIX口令

.. index:: single: crypt(3)

注意本模块的行为取决于实际运行系统的crypt(3)例程的实际实现方法.因此,任何对当前实
现的可能扩展也将对本模块可用


.. function:: crypt(word, salt)

   *word* will usually be a user's password as typed at a prompt or  in a graphical
   interface.  *salt* is usually a random two-character string which will be used
   to perturb the DES algorithm in one of 4096 ways.  The characters in *salt* must
   be in the set ``[./a-zA-Z0-9]``.  Returns the hashed password as a string, which
   will be composed of characters from the same alphabet as the salt (the first two
   characters represent the salt itself).

   word参数通常都是用户在命令提示符下或图形界面下键入的口令.salt参数则通常是用于扰乱
   DES算法的4096种方法之一的随机两字府的串.salt串中的字符必须在集合(./a-zA-Z0-9)之
   中.本函数返回串形式的哈希后口令,其由与salt参数相同的字符组成.(首两字符代表salt参数自己)

   .. index:: single: crypt(3)

   Since a few :manpage:`crypt(3)` extensions allow different values, with
   different sizes in the *salt*, it is recommended to use  the full crypted
   password as salt when checking for a password.

   由于一些crypt(3)扩展允许不同的值,并允许salt参数不同的大小,因此通常建议当检查口令
   时,使用完全加密过的口令作为salt参数


一个演示典型应用的简单的例子::


   import crypt, getpass, pwd

   def login():
       username = input('Python login:')
       cryptedpasswd = pwd.getpwnam(username)[1]
       if cryptedpasswd:
           if cryptedpasswd == 'x' or cryptedpasswd == '*':
               raise "Sorry, currently no support for shadow passwords"
           cleartext = getpass.getpass()
           return crypt.crypt(cleartext, cryptedpasswd) == cryptedpasswd
       else:
           return 1


