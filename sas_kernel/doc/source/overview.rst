######################
Overview of SAS kernel
######################
.. I used http://documentation-style-guide-sphinx.readthedocs.io/en/latest/style-guide.html


*************
What is this?
*************

A SAS kernel for `Jupyter Notebooks <http://www.jupyter.org>`_. Jupyter Notebooks
are capable of running programs in a variety of programming languages and it is
the kernel that enables this ability. The SAS kernel enables Jupyter Notebook to
provide the following programming experience:

* syntax highlighting for SAS programming statements
* store the input and output from an interactive SAS session

After installing the SAS kernel, you can use a notebook and a SAS installation to
write, document, and submit SAS programming statements.


************
Dependencies
************

-  Python3.X or higher.
-  Jupyter
-  SAS 9.4 or higher. SAS Viya 3.1 or higher is also supported.

Previous release of the SAS kernel supported connecting to SAS on Linux only. For this release,
you can connect to SAS on any platform that is supported for the specified SAS releases.

Jupyter has a number of dependencies. See the subsections for steps on installing Jupyter on
your system.


**************
Jupyter magics
**************
The %%prompt4var magic is written specifically for the SAS kernel. The purpose of
the magic is to prompt for sensitive information such as a password and store
the value in a SAS macro variable.

Seasoned SAS programmers might notice that Python magics begin with a percent
sign (%) and that SAS macro variables also begin with a percent sign. To ensure
that magics are interpreted by Python, they must be specified in the first line
of a notebook cell. Otherwise, the magic (%xxxxx) is submitted to SAS.

If you need to run a SAS macro as the first statement, then insert a blank line
as the first line in the notebook cell and the macro on the second line. The
blank line prevents Python from intrepreting the macro variable as a magic.


************
NBExtensions
************
There are a few NBExtensions to make working with notebooks more productive and pleasant.
These are largely the result of pain points and gotchas. These include:

SAS Log
  This extension shows the SAS log for the last executed cell or the entire log
  since the last restart of the notebook.

Themes
  This extension enables you to change the color scheme for your code to match
  the traditional SAS theme.

You can install the extensions after you install the SAS kernel. Installation information
is provided in this documentation. The source code for the extensions can be found at 
https://github.com/sassoftware/sas_kernel/tree/master/sas_kernel/nbextensions.


****************************************
Integration with other notebook software
****************************************

JupyterHub
==========

The SAS kernel can be used with JupyterHub. For more information, see
https://jupyterhub.readthedocs.org.

NBGrader
========

NBGrader is a system for assigning and grading notebooks and extends 
Jupyter Notebook. For more information, see http://nbgrader.readthedocs.org.

This forked repo (https://github.com/jld23/nbgrader) includes
a number of contributions that are used for
teaching SAS programming in a classroom setting.


***
FAQ
***

-  Do I need to buy SAS to use this kernel?

   The SAS kernel is simply a program that enables Jupyter to communicate 
   with SAS. As such, if SAS is not installed, then this kernel is not helpful.
   For information about purchasing SAS, see 
   http://www.sas.com/en_us/software/how-to-buy.html.

-  How does Jupyter communicate with SAS?

   Behind a Jupyter Notebook is a Python session. The Python session
   submits code to SAS and receives responses through socket I/O communication.
   The submit and receive strategy leverages the stdin, stdout, and stderr 
   capabilities that have been supported in SAS for a long time.

-  If stdin, stdout, and stderr have been supported for so long why do I
   need to have SAS 9.4 or newer?

   First, SAS 9.4 was released in July 2013, so it is not a bleeding edge
   requirement. The reason for the prerequisite is that SAS 9.4 introduced
   support for creating of HTML5 documents. The SAS kernel relies on the
   HTML5 output so that it can render attractive tables and graphs automagically.

   For information about SAS Viya, see http://www.sas.com/viya. 

-  How can I see my SAS log, I only see the listing output?

   SAS is different from many other programming languages in that it has
   two useful information streams, the log (which details the technical
   details of what happened and how long it took) and the lst (which
   includes the tables and graphics from the analysis). The SAS kernel
   attempts to show you what we *think* you want. Here are the rules:


   +-------------------------------+-------+---------------------------------------------------------------------+-----------------------------------------------------------------------------+
   | LOG                           | LST   | DISPLAYED                                                           | NOTES                                                                       |
   +===============================+=======+=====================================================================+=============================================================================+
   | Yes                           | No    | LOG                                                                 | This happens when you run DATA step or a PROC with the ``noprint`` option.  |
   +-------------------------------+-------+---------------------------------------------------------------------+-----------------------------------------------------------------------------+
   | Yes                           | Yes   | LST                                                                 | ---                                                                         |
   +-------------------------------+-------+---------------------------------------------------------------------+-----------------------------------------------------------------------------+
   | Yes (with ERROR message(s))   | Yes   | ERROR messages with context from the log, then the listing output.  | ---                                                                         |
   +-------------------------------+-------+---------------------------------------------------------------------+-----------------------------------------------------------------------------+
   | Yes (with ERROR message(s))   | No    | LOG                                                                 | ---                                                                         |
   +-------------------------------+-------+---------------------------------------------------------------------+-----------------------------------------------------------------------------+

   If you want to see the log but it was not displayed, you can use
   the SAS log NBExtension. The extension shows the log for the last 
   executed cell or the entire log since the last (re)start of the notebook.

-  Will this leave a bunch of SAS sessions hanging around?

   A SAS session is started for each notebook that you have open. For example,
   if you open 5 notebooks, you start 5 SAS sessions. Those sessions remain
   active as long as the notebook is running. If you shut down your notebook,
   the associated SAS session terminates. In JupyterHub, there are configuration
   options to shut down inactive sessions and the SAS kernel complies
   with those directives.

-  I restarted my SAS kernel and now my Work library is now empty. What
   happened?

   When you restart the kernel in a notebook, you terminate the SAS session
   and start a new one. All of the temporary artifacts, such as data sets in
   the Work library, assigned librefs, filerefs, Work macros, and so on,
   are destroyed.
