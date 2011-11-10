.. _reporting-bugs:

***********************
报告Bug
***********************

Python是一种成熟的编程语言, 它的可靠性和健壮性都已获得相当的认可. 为了保持这种声誉, 开发者们很想知道你在Python中发现的任何缺陷. 


文档错误
==========================

If you find a bug in this documentation or would like to propose an improvement,
please send an e-mail to docs@python.org describing the bug and where you found
it.  If you have a suggestion how to fix it, include that as well.

docs@python.org is a mailing list run by volunteers; your request will be
noticed, even if it takes a while to be processed.

Of course, if you want a more persistent record of your issue, you can use the
issue tracker for documentation bugs as well.

------------------------------------------------------------------------------------------------------------------------------------------------------

如果您发现本文档中的一个错误, 或想提出一个改进, 
请发送电子邮件docs@python.org描述的bug和您发现
它. 如果你有一个建议, 如何解决它, 包括这点. 

docs@python.org是由志愿者运行的邮件列表, 您的请求将被
注意到, 即使它需要一段时间来处理. 

当然, 如果你想要一个更持久的记录您的问题, 您可以使用
问题跟踪器以及文档的错误. 


使用Python的问题跟踪系统
==============================

Bug reports for Python itself should be submitted via the Python Bug Tracker
(http://bugs.python.org/).  The bug tracker offers a Web form which allows
pertinent information to be entered and submitted to the developers.

Python 自身的错误应当通过 Python Bug Tracker (http://bugs.python.org/) 来提交.  
该 bug tracker 提供了一个 Web 表单, 它允许相关信息被输入并提交给开发者.

------------------------------------------------------------------------------------------------------------------------------------------------------

The first step in filing a report is to determine whether the problem has
already been reported.  The advantage in doing so, aside from saving the
developers time, is that you learn what has been done to fix it; it may be that
the problem has already been fixed for the next release, or additional
information is needed (in which case you are welcome to provide it if you can!).
To do this, search the bug database using the search box on the top of the page.

提交报告的第一步是确认该问题是否已经被报告. 这样做的好处是, 不仅能够节省开发者的时间,
还让你知道为了修复它做了些什么; 可能该问题已经在下个版本里解决, 
或者修复它需要更多的信息 (这种情况下, 如果你能的话, 我们十分欢迎你提供这些信息!).
做这一步的方法是, 使用页面顶部的搜索框来搜索 bug 数据库. 

------------------------------------------------------------------------------------------------------------------------------------------------------

If the problem you're reporting is not already in the bug tracker, go back to
the Python Bug Tracker and log in.  If you don't already have a tracker account,
select the "Register" link or, if you use OpenID, one of the OpenID provider
logos in the sidebar.  It is not possible to submit a bug report anonymously.

如果你报告的问题还不在 bug tracker 里的话, 回到 Python Bug Tracker, 然后登录.  
如果你还没有一个帐号, 选择 "Register" 链接, 你也可以使用 OpenID, 
在侧边栏里的任意一个 OpenID 提供商提供的都行. 匿名提交 bug 报告是不可能的.

------------------------------------------------------------------------------------------------------------------------------------------------------

Being now logged in, you can submit a bug.  Select the "Create New" link in the
sidebar to open the bug reporting form.

在登录后, 你可以提交 bug 了.  选择侧边栏里的 "Create New" 链接来打开 bug 报告表单.

------------------------------------------------------------------------------------------------------------------------------------------------------

The submission form has a number of fields.  For the "Title" field, enter a
*very* short description of the problem; less than ten words is good.  In the
"Type" field, select the type of your problem; also select the "Component" and
"Versions" to which the bug relates.

该表单有若干项.  在 "Title" 项, 输入对问题的一个*十分*短的描述; 最好不要超过十个单词.
在 "Type" 项, 选择问题的类型; 还要选择 bug 相关的 "Component" 和 "Versions".

------------------------------------------------------------------------------------------------------------------------------------------------------

In the "Comment" field, describe the problem in detail, including what you
expected to happen and what did happen.  Be sure to include whether any
extension modules were involved, and what hardware and software platform you
were using (including version information as appropriate).

在 "Comment"项, 详细描述问题, 要包括你希望发生的和实际发生的.  
一定要包括是否涉及到扩展模块, 以及你所使用的硬件和软件平台 (酌情包含版本信息).

------------------------------------------------------------------------------------------------------------------------------------------------------

Each bug report will be assigned to a developer who will determine what needs to
be done to correct the problem.  You will receive an update each time action is
taken on the bug.  See http://www.python.org/dev/workflow/ for a detailed
description of the issue workflow.

每一个 bug 报告都将被分配给一个开发人员, Ta 将确定纠正这个问题需要做些什么.  
在每一次对该 bug 采取行动时, 你将收到一个更新.  参见 http://www.python.org/dev/workflow/ 
得到 issue 工作流程的详细描述.

------------------------------------------------------------------------------------------------------------------------------------------------------

.. seealso::

   `How to Report Bugs Effectively <http://www.chiark.greenend.org.uk/~sgtatham/bugs.html>`_
      Article which goes into some detail about how to create a useful bug report.
      This describes what kind of information is useful and why it is useful.
      
   `如何有效的报告错误 <http://www.chiark.greenend.org.uk/~sgtatham/bugs.html>`_
      这篇文章探讨了如何创建一份有用的 bug 报告的一些细节. 它描述了什么类型的信息是有用的以及它为什么有用.   
         
------------------------------------------------------------------------------------------------------------------------------------------------------

   `Bug Writing Guidelines <http://developer.mozilla.org/en/docs/Bug_writing_guidelines>`_
      Information about writing a good bug report.  Some of this is specific to the
      Mozilla project, but describes general good practices.

   `Bug 书写指导 <http://developer.mozilla.org/en/docs/Bug_writing_guidelines>`_
      有关如何写一个好的 bug 报告的信息.  其中有一些专门针对 Mozilla 项目, 但也描述了一般性的好的做法.

