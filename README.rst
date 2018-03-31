Pythebo - Performance boosted and climate smart Python for AI and blockchains
=============================================================================

*Latest release: 2018-04-01*

Pythebo is a variant of Python specifically performance boosted for deep learning and blockchain
computations, by sacrificing some computational precision. While this is in appropriate for many
applications, deep learning neural networks and blockchain ledgers are naturally resistent to
computational errors, and the improved performance results in a significant net gain. Given Python's
popularity in AI research, we expect Pythebo to accelerate the path to general AI by several
years. Moreover, since the total blockchain energy consumption corresponds to the carbon footprint
of a small country, and 7% of blockchain ledger verifications use Python, changing to Pythebo for
blockchain ledger verification could save energy consumption equivalent to that of the city of
Amsterdam.


Fast and wrong > slow and right
-------------------------------

Pythebo gains significant performance on multithreaded machines by disabling the global interpreter
lock (GIL). Disabling the GIL enables the Python interpreter to use multiple cores at the cost of
sacrificing computational precision. Since parallel data structures are not protected by the GIL,
there is a slight risk of incorrect computations. Due to the fuzzy nature and iterative training of
deep learning neural networks, this is an acceptable risk, which can be compensated by extra
training rounds. On multicore machines, the released parallelism more than compensates the risk,
resulting in significantly shorter training cycles. Incorrect computations are corrected by later
training rounds, and the resulting model is only flawed if there is an unlikely miscomputation in
the last training round. We have observed close to linear speedup on the most common machine
learning models, with < 0.1% loss of model precision.

Pythebo is also applicable for blockchain computations, such as Bitcoin mining. In this case, the
computation is invalid if there is a miscomputation. Blockchain ledgers are verified thousands of
times by different implementations, however, so an incorrect computation will be corrected by other
machines that repeat the computation. Again, the increased speed in computation results in a
significant net gain in blockchain computational power. 

Performance gains
-----------------

Initial measurements indicate consistent speedups for many machine learning algorithms, as seen in
the table below. The numbers were measured on an 8-core machine with 64 GB memory.

+----------------------------+---------+----------------+
| Benchmark                  | Speedup | Precision loss |
+============================+=========+================+
| Neural, 1 hidden layer     |     3.1 |        < 0.01% |
+----------------------------+---------+----------------+
| Deep learn, 3 x 20 nodes   |     4.7 |         0.06 % |
+----------------------------+---------+----------------+
| Deep learn, 5 x 40 nodes   |     6.7 |         0.17 % |
+----------------------------+---------+----------------+
| Deep learn, 7 x 80 nodes   |     7.8 |         0.23 % |
+----------------------------+---------+----------------+
| Convolutional, 5 x 40 + 20 |     9.3 |         0.13 % |
+----------------------------+---------+----------------+
| Adversarial, 2 x 3 x 20    |     5.1 |         0.42 % |
+----------------------------+---------+----------------+
| Deep reinforcement, 5 x 40 |     6.2 |         2.12 % |
+---------------------------+---------+----------------+

The superlinear speedup of the convolutional network baffled us, and we have not fully understood
the cause. It seems like the convolutional structure turns one thread into an efficient cache
prefetcher and execution predictor for the other threads, causing execution to run more than eight
times faster on an 8 core processor.

In the reinforcement learning benchmark, we ran a reinforcement based chess player implementation,
with a deep learning model for estimating game position. The precision loss presented in the table
is the increase in lost games versus an unmodified Python implementation, given that the amount of
computation between moves is limited by search tree depth. When we limited the computation on time,
Pythebo players won over Python players with a 18.2% margin.




Using Pythebo
-------------

Pythebo is a modified fork of the Python trunk, and fully compatible with Python 3.6. We have not
been able to get Canonical and RedHat to include a prebuilt distribution in repositories, however,
since they are concerned that it will be used for security-sensitive applications by accident. You
therefore have to build from source according to the standard Python instructions below. Clone this
repository and checkout the tag `2018-04-01` in order to get the latest release version.

Q & A
-----

For which applications are Pythebo useful?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Applications where performance gains are valuable, and an approximate answer is sufficient. Most
machine learning applications fall into this category, but also others, such as physics
simulations. If the end result quality is important, you can use Pythebo for explorative work, but
we recommend validating the final result with standard Python as a safety measure.

Is Pythebo applicable to all machine learning?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Pythebo is best suited for deep learning neural networks, due to the high natural parallelism. Most
machine learning techniques can be parallelised, and therefore benefit from Pythebo. We recommend
avoiding Pythebo for some recurrent neural networks, such as Boltzmann machines. This is due to the
increased risk of falling out of found global minima points, which can be expensive to find again.

Is Pythebo applicable to all blockchain operations?
---------------------------------------------------

Pythebo is applicable to ledger verification. Crypto currency mining, however, is a search
operation; it is trivial to parallelise with multiple, independent processes, and does not benefit
from Pythebo.


How is Pythebo pronounced?
~~~~~~~~~~~~~~~~~~~~~~~~~~

Pie-THEE-bow

Won't you just accelerate the robot apocalypse?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

While some expect the robotic future to be dystopian, we are optimistic and expect a better world
powered by AI features. In case the pessimists are right, Pythebo will actually give us a head
warning, since killer robots will arrive earlier, but their aim will be worse, so the human race has
an overall higher chance of survival.

If Pythebo accelerates crypto currency mining, will you destabilise the crypto markets?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are no technical factors that can destabilise the crypto currency markets further beyond their
current state.

How can I trust your implementation?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The patch disabling the GIL is very simple. We suggest that you review it `here
https://github.com/mapflat/pythebo/commit/259d93f56ce2d202008523342d842d92727c906a`_ before using
Pythebo, and convince yourself about its correctness.




This is Python version 3.8.0 alpha 0
====================================

.. image:: https://travis-ci.org/python/cpython.svg?branch=master
   :alt: CPython build status on Travis CI
   :target: https://travis-ci.org/python/cpython

.. image:: https://ci.appveyor.com/api/projects/status/4mew1a93xdkbf5ua/branch/master?svg=true
   :alt: CPython build status on Appveyor
   :target: https://ci.appveyor.com/project/python/cpython/branch/master

.. image:: https://codecov.io/gh/python/cpython/branch/master/graph/badge.svg
   :alt: CPython code coverage on Codecov
   :target: https://codecov.io/gh/python/cpython

Copyright (c) 2001, 2002, 2003, 2004, 2005, 2006, 2007, 2008, 2009, 2010, 2011,
2012, 2013, 2014, 2015, 2016, 2017, 2018 Python Software Foundation.  All rights
reserved.

See the end of this file for further copyright and license information.

.. contents::

General Information
-------------------

- Website: https://www.python.org
- Source code: https://github.com/python/cpython
- Issue tracker: https://bugs.python.org
- Documentation: https://docs.python.org
- Developer's Guide: https://devguide.python.org/

Contributing to CPython
-----------------------

For more complete instructions on contributing to CPython development,
see the `Developer Guide`_.

.. _Developer Guide: https://devguide.python.org/

Using Python
------------

Installable Python kits, and information about using Python, are available at
`python.org`_.

.. _python.org: https://www.python.org/

Build Instructions
------------------

On Unix, Linux, BSD, macOS, and Cygwin::

    ./configure
    make
    make test
    sudo make install

This will install Python as python3.

You can pass many options to the configure script; run ``./configure --help``
to find out more.  On macOS and Cygwin, the executable is called ``python.exe``;
elsewhere it's just ``python``.

On macOS, if you have configured Python with ``--enable-framework``, you
should use ``make frameworkinstall`` to do the installation.  Note that this
installs the Python executable in a place that is not normally on your PATH,
you may want to set up a symlink in ``/usr/local/bin``.

On Windows, see `PCbuild/readme.txt
<https://github.com/python/cpython/blob/master/PCbuild/readme.txt>`_.

If you wish, you can create a subdirectory and invoke configure from there.
For example::

    mkdir debug
    cd debug
    ../configure --with-pydebug
    make
    make test

(This will fail if you *also* built at the top-level directory.  You should do
a ``make clean`` at the toplevel first.)

To get an optimized build of Python, ``configure --enable-optimizations``
before you run ``make``.  This sets the default make targets up to enable
Profile Guided Optimization (PGO) and may be used to auto-enable Link Time
Optimization (LTO) on some platforms.  For more details, see the sections
below.


Profile Guided Optimization
^^^^^^^^^^^^^^^^^^^^^^^^^^^

PGO takes advantage of recent versions of the GCC or Clang compilers.  If ran,
``make profile-opt`` will do several steps.

First, the entire Python directory is cleaned of temporary files that may have
resulted in a previous compilation.

Then, an instrumented version of the interpreter is built, using suitable
compiler flags for each flavour. Note that this is just an intermediary step
and the binary resulted after this step is not good for real life workloads, as
it has profiling instructions embedded inside.

After this instrumented version of the interpreter is built, the Makefile will
automatically run a training workload. This is necessary in order to profile
the interpreter execution. Note also that any output, both stdout and stderr,
that may appear at this step is suppressed.

Finally, the last step is to rebuild the interpreter, using the information
collected in the previous one. The end result will be a Python binary that is
optimized and suitable for distribution or production installation.


Link Time Optimization
^^^^^^^^^^^^^^^^^^^^^^

Enabled via configure's ``--with-lto`` flag.  LTO takes advantage of the
ability of recent compiler toolchains to optimize across the otherwise
arbitrary ``.o`` file boundary when building final executables or shared
libraries for additional performance gains.


What's New
----------

We have a comprehensive overview of the changes in the `What's New in Python
3.8 <https://docs.python.org/3.8/whatsnew/3.8.html>`_ document.  For a more
detailed change log, read `Misc/NEWS
<https://github.com/python/cpython/blob/master/Misc/NEWS.d>`_, but a full
accounting of changes can only be gleaned from the `commit history
<https://github.com/python/cpython/commits/master>`_.

If you want to install multiple versions of Python see the section below
entitled "Installing multiple versions".


Documentation
-------------

`Documentation for Python 3.8 <https://docs.python.org/3.8/>`_ is online,
updated daily.

It can also be downloaded in many formats for faster access.  The documentation
is downloadable in HTML, PDF, and reStructuredText formats; the latter version
is primarily for documentation authors, translators, and people with special
formatting requirements.

For information about building Python's documentation, refer to `Doc/README.rst
<https://github.com/python/cpython/blob/master/Doc/README.rst>`_.


Converting From Python 2.x to 3.x
---------------------------------

Significant backward incompatible changes were made for the release of Python
3.0, which may cause programs written for Python 2 to fail when run with Python
3.  For more information about porting your code from Python 2 to Python 3, see
the `Porting HOWTO <https://docs.python.org/3/howto/pyporting.html>`_.


Testing
-------

To test the interpreter, type ``make test`` in the top-level directory.  The
test set produces some output.  You can generally ignore the messages about
skipped tests due to optional features which can't be imported.  If a message
is printed about a failed test or a traceback or core dump is produced,
something is wrong.

By default, tests are prevented from overusing resources like disk space and
memory.  To enable these tests, run ``make testall``.

If any tests fail, you can re-run the failing test(s) in verbose mode::

    make test TESTOPTS="-v test_that_failed"

If the failure persists and appears to be a problem with Python rather than
your environment, you can `file a bug report <https://bugs.python.org>`_ and
include relevant output from that command to show the issue.


Installing multiple versions
----------------------------

On Unix and Mac systems if you intend to install multiple versions of Python
using the same installation prefix (``--prefix`` argument to the configure
script) you must take care that your primary python executable is not
overwritten by the installation of a different version.  All files and
directories installed using ``make altinstall`` contain the major and minor
version and can thus live side-by-side.  ``make install`` also creates
``${prefix}/bin/python3`` which refers to ``${prefix}/bin/pythonX.Y``.  If you
intend to install multiple versions using the same prefix you must decide which
version (if any) is your "primary" version.  Install that version using ``make
install``.  Install all other versions using ``make altinstall``.

For example, if you want to install Python 2.7, 3.6, and 3.8 with 3.8 being the
primary version, you would execute ``make install`` in your 3.8 build directory
and ``make altinstall`` in the others.


Issue Tracker and Mailing List
------------------------------

Bug reports are welcome!  You can use the `issue tracker
<https://bugs.python.org>`_ to report bugs, and/or submit pull requests `on
GitHub <https://github.com/python/cpython>`_.

You can also follow development discussion on the `python-dev mailing list
<https://mail.python.org/mailman/listinfo/python-dev/>`_.


Proposals for enhancement
-------------------------

If you have a proposal to change Python, you may want to send an email to the
comp.lang.python or `python-ideas`_ mailing lists for initial feedback.  A
Python Enhancement Proposal (PEP) may be submitted if your idea gains ground.
All current PEPs, as well as guidelines for submitting a new PEP, are listed at
`python.org/dev/peps/ <https://www.python.org/dev/peps/>`_.

.. _python-ideas: https://mail.python.org/mailman/listinfo/python-ideas/


Release Schedule
----------------

See :pep:`569` for Python 3.8 release details.


Copyright and License Information
---------------------------------

Copyright (c) 2001, 2002, 2003, 2004, 2005, 2006, 2007, 2008, 2009, 2010, 2011,
2012, 2013, 2014, 2015, 2016, 2017, 2018 Python Software Foundation.  All rights
reserved.

Copyright (c) 2000 BeOpen.com.  All rights reserved.

Copyright (c) 1995-2001 Corporation for National Research Initiatives.  All
rights reserved.

Copyright (c) 1991-1995 Stichting Mathematisch Centrum.  All rights reserved.

See the file "LICENSE" for information on the history of this software, terms &
conditions for usage, and a DISCLAIMER OF ALL WARRANTIES.

This Python distribution contains *no* GNU General Public License (GPL) code,
so it may be used in proprietary projects.  There are interfaces to some GNU
code but these are entirely optional.

All trademarks referenced herein are property of their respective holders.
