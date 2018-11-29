findpython
==========

[![CRAN Status Badge](https://www.r-pkg.org/badges/version/findpython)](https://cran.r-project.org/package=findpython)

[![Build Status](https://travis-ci.org/trevorld/findpython.png?branch=master)](https://travis-ci.org/trevorld/findpython)

[![Coverage Status](https://img.shields.io/codecov/c/github/trevorld/findpython.svg)](https://codecov.io/github/trevorld/findpython?branch=master)

[![RStudio CRAN mirror downloads](https://cranlogs.r-pkg.org/badges/findpython)](https://cran.r-project.org/package=findpython)

[![Project Status: Active -- The project has reached a stable, usable state and is being actively developed.](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active)

R package currently designed to find acceptable Python binaries for your
program. Since there are often multiple python binaries installed on any
given system and they aren\'t always added to the path this can be a
non-trivial task.

To install the development version use:

    devtools::install_github("trevorld/findpython")

Usage
=====

find\_python\_cmd is the main function. It returns the path to a python
binary that meets certain requirements you specify. Below are some
examples.

If you need to find a Python 2 binary which is at least Python 2.4:

    find_python_cmd(minimum_version = '2.4', maximum_version = '2.7')

If you need to find a version of Python at least Python 2.5 (but don\'t
care if it is a Python 3 binary):

    find_python_cmd(minimum_version = '2.5')

If you don\'t care what version of Python you use but it needs to have
access to a argparse module as well as either the json OR simplejson
module:

    find_python_cmd(required_modules = c('argparse', 'json | simplejson'))

Although find\_python\_cmd will create a basic default message if left
unspecified you can use the error\_message argument to specify what
error message your program will output if it is unable to find an
acceptable binary:

    find_python_cmd(minimum_version = '4.0', 
          error_message = 'Was unable to find the Python 4 binary dependency.  See file INSTALL for more information')

There is also a wrapper for find\_python\_cmd that instead of throwing
an error upon failing to find an appropriate Python command will return
FALSE and if it finds an appropriate command will return TRUE. If
successful it attaches the appropriate binary path as an attribute
\`python\_cmd\`:

    did_find_python <- can_find_python_cmd()
    python_cmd <- attr(did_find_python, "python_cmd")

The default error message will be something like:

    > find_python_cmd(min='4.0')
    Error in find_python_cmd(min = "4.0") : 
      Couldn't find a sufficient Python binary. If you haven't installed the Python dependency yet please do so. If you have but it isn't on the system path (as is default on Windows) please add it to path or set options('python_cmd'='/path/to/binary')  or set the PYTHON, PYTHON2, or PYTHON3 environmental variables. Python must be at least version 4.0  

If you already have a python binary you want to check you can use
is\_python\_sufficient to test whether it has sufficient features. It
has the same arguments minimum\_version, maximum\_version, and
required\_modules as \`find\_python\_cmd\`:

    is_python_sufficient(path_to_binary, minimum_version = '2.6', required_modules = 'scipy')
