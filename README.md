<h1 align="center">SQLite源码</h1>

https://www.sqlite.org/src/doc/trunk/README.md

## 获取源码

## 编译

推荐的方法是首先新建文件夹来存放build文件。Cd进入bld，然后运行configure命令和make命令。

For example:

        tar xzf sqlite.tar.gz    ;#  Unpack the source tree into "sqlite"
        mkdir bld                ;#  Build will occur in a sibling directory
        cd bld                   ;#  Change to the build directory
        ../sqlite/configure      ;#  Run the configure script
        make                     ;#  Run the makefile.
        make sqlite3.c           ;#  Build the "amalgamation" source file
        make test                ;#  Run some tests (requires Tcl)

See the makefile for additional targets.

## 源码漫游

大部分源代码在/src文件夹下。/src包含"shell.c"文件作为命令行的入口文件。

Key files:

  *  **sqlite.h.in** - This file defines the public interface to the SQLite
     library.  Readers will need to be familiar with this interface before
     trying to understand how the library works internally.

  *  **sqliteInt.h** - this header file defines many of the data objects
     used internally by SQLite.

  *  **parse.y** - This file describes the LALR(1) grammer that SQLite uses
     to parse SQL statements, and the actions that are taken at each step
     in the parsing process.

  *  **vdbe.c** - This file implements the virtual machine that runs
     prepared statements.  There are various helper files whose names
     begin with "vdbe".  The VDBE has access to the vdbeInt.h header file
     which defines internal data objects.  The rest of SQLite interacts
     with the VDBE through an interface defined by vdbe.h.

  *  **where.c** - This file analyzes the WHERE clause and generates
     virtual machine code to run queries efficiently.  This file is
     sometimes called the "query optimizer".  It has its own private
     header file, whereInt.h, that defines data objects used internally.

  *  **btree.c** - This file contains the implementation of the B-Tree
     storage engine used by SQLite.

  *  **pager.c** - This file contains the "pager" implementation, the
     module that implements transactions.

  *  **os_unix.c** and **os_win.c** - These two files implement the interface
     between SQLite and the underlying operating system using the run-time
     pluggable VFS interface.

  *  **shell.c** - This file is not part of the core SQLite library.  This
     is the file that, when linked against sqlite3.a, generates the
     "sqlite3.exe" command-line shell.

  *  **tclsqlite.c** - This file implements the Tcl bindings for SQLite.  It
     is not part of the core SQLite library.  But as most of the tests in this
     repository are written in Tcl, the Tcl language bindings are important.

There are many other source files.  Each has a suscinct header comment that
describes its purpose and role within the larger system.

