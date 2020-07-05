# ffbase

ffbase is a fast C library which implements base containers & algorithms.

C language doesn't have this functionality by default (contrary to almost all other languages which have their rich standard library out of the box) and ffbase attempts to fill this hole.

ffbase is header-only (`.h`-only) and doesn't need to be built into `.a/.so/.dll` before use - you just include `.h` file and that's all.


Contents:
* Naming
* Features
* Requirements
* Where to use
* How to use
* Configure
* Develop


## Naming

As there are no namespaces in C, everything here starts with an `ff` prefix.  Then follows the short prefix for the namespace (or a context) and then the name of the function.  For example:

* `ffstr` is a string container
* `ffstr_*()` are functions which work with it, e.g. `ffstr_cmp()` compares strings

All components follow this convention.


## Features

String/text:
* `string.h` - string container
* `stringz.h` - NULL-terminated string functions
* `unicode.h` - Unicode functions

Containers:
* `slice.h` - simple array container
* `vector.h` - array container
* `sort.h` - array sorting (merge-sort)
* `chain.h` - simple chain
* `list.h` - doubly-linked list
* `rbtree.h` - red-black tree
* `map.h` - hash table
* `ring.h` - ring buffer

JSON:
* `json.h` - low-level JSON parser
* `json-scheme.h` - JSON parser with scheme
* `json-writer.h` - JSON writer

Atomic:
* `atomic.h` - atomic operations
* `lock.h` - spinlock


## Requirements:

* gcc or clang


## Where to use

In C and C++ projects that require fast and small code without unnecessary dependencies.


## How to use

1. Clone ffbase repo:

		$ git clone https://github.com/stsaz/ffbase

2. In your build script:

		-IFFBASE_DIR

where `FFBASE_DIR` is your ffbase/ directory.

3. And then just use the necessary files:

		#include <ffbase/slice.h>


### Use just 1 file

To avoid copying the whole ffbase repo into your project while all you need is, for example, "slices", you may just copy a few files to your project directory.

1. Clone ffbase repo and copy the necessary files:

		$ git clone https://github.com/stsaz/ffbase
		$ mkdir YOUR_PROJECT/include/ffbase/
		$ cp ffbase/ffbase/slice.h ffbase/ffbase/base.h  YOUR_PROJECT/include/ffbase/

where `YOUR_PROJECT` is your project's directory.

2. Then, edit `YOUR_PROJECT/include/ffbase/slice.h` and remove the dependencies (marked as optional) you don't need, for example:

		#ifndef _FFBASE_BASE_H
		#include <ffbase/base.h>
		#endif
		-#include <ffbase/sort.h> // optional
		+// #include <ffbase/sort.h> // optional

Or you can copy `sort.h` file too if you need it.

3. In your build script:

		-IYOUR_PROJECT/include

4. And then just use the necessary files:

		#include <ffbase/slice.h>


## Configure

Use these preprocessor definitions to enable new functionality or to change the existing logic:

* `FF_DEBUG` - use additional assert() checks when debugging
* `FFBASE_HAVE_FFERR_STR` - enable "%E" for format strings


## Develop

* code
* tests
* in-code documentation
* examples in doc/

### Add new file

Header:

	/** ffbase: description
	Year, Author Name
	*/

Unique ID for the optional `.h` files, so their includers can disable the functionality:

	#define _FFBASE_SORT_H

Don't use any standard functions directly, otherwise it will be impossible to substitute the lower level functions.  For example, don't use `memcpy()` - use `ffmem_copy()` instead.  This gives us the ability to easily add functionality such as counting the number of bytes copied per thread, before actually calling `memcpy()`.

### Test

	mkdir /tmp/ffbase
	cd /tmp/ffbase
	make -j8 -Rrf MYSRC/ffbase/Makefile SRCDIR=MYSRC/ffbase
	./fftest all

or for Windows:

	make -j8 -Rrf MYSRC/ffbase/Makefile SRCDIR=MYSRC/ffbase CPREFIX=x86_64-w64-mingw32- OS=win

### Commit

Message:

* `+ component: message` - new feature
* `* component: message` - change, small improvement
* `- component: message` - bugfix


## History

I started FFOS and FF libraries in 2013, and since then they've grown and become too complex to be used in small projects.  Also, it's impossible to just copy a couple of necessary files from FF, because the internal dependencies are quite deep.  I hope ffbase library will solve these problems.

1. It's convenient to just copy several .h files into another project.
2. There's no need to include its Makefile when using ffbase.
3. ffbase's functionality is restricted to base containers and algorithms, so it won't become too large.