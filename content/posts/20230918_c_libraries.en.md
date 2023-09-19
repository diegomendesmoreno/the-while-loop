+++
title = "C Libraries, Object files and Shared Objects"
date = "2023-09-18"
categories = ["Programming","C"]
type = ["posts","post"]
[ author ]
  name = "Diego Moreno"
draft = true
+++

## Introduction

This document shows the different ways to use C libraries.
- Compiling library source files with the application code
- Using an object file
- Using a shared object file


## Running the code example

You can run the code and see the results at [this Replit repository](https://replit.com/@diegommoreno/C-Libraries-Object-files-Shared-Objects). After opening the page, when you hit the button "Run", the terminal opens and you will be able to run the bash commands listed here.

Throughout the exercises, after running the executable (`./bin/main.out`), you should see the following message in the console:
```bash
Wow, it works!
```

At any time, you can delete the executables/binaries/object files by running:
```bash
make clean
```


## How to use your own libraries in C
This example shows how to use a library in your C code in 3 different ways. We will use a simple library that just prints string in the console.

### Compiling library source files
This is done by compiling the library source files with the application code. And finally, running a executable that contains both the application code and the library.
- If calling the toolchain from the command line, run:
```bash
gcc src/main.c src/hello.c -o bin/main.out
./bin/main.out
```
- If using the Makefile with `make`, run:
```bash
make
./bin/main.out
```

### Using an object file
This is done by precompiling the library into an object file (`.o`) and then compiling it together with the application code. The result is also a self-contained executable with the object file linked at the compilation. Despite this also being an executable that contains both the application code and the library, this way may result in a larger executable file. This could happen if the object file contain functions that are not used in the program and are included in the executable anyway because the compiler optimization doesn't act in the precompiled object.
- If calling the toolchain from the command line, run:
```bash
gcc -Wall -g -c src/hello.c -o lib/hello.o
gcc src/main.c lib/hello.o -o bin/main.out
./bin/main.out
```
- If using the Makefile with `make`, run:
```bash
make object
./bin/main.out
```

### Using a shared object file
This is done by precompiling the library into a shared object file (`.so`) or a dynamically linked library (`.dll` in Windows). Then we can compile with the application code, but the shared object will be loaded at run time and not packed together in the executable. Be aware that this may also result in a larger executable, since there is some overhead associated with dynamic linking. This includes the need to resolve symbols at runtime, which requires additional information in the executable.

First, we create the shared object file:
```bash
gcc -Wall -g -fPIC -shared -o lib/libhello.so src/hello.c -lc
```
- If using the Makefile with `make`, run:
```bash
make shared
```

Now, we need to specify where this shared object file is to be linked at run time. We can do this in two ways:

- Using the `-rpath` linker option: It is used to specify a folder/directory where the operating system will look for the necessary shared libraries to be loaded and linked at runtime. Run:
```bash
gcc -Wall -g -o bin/main.out src/main.c -L./lib -lhello -Wl,-rpath,./lib
./bin/main.out
```
- If using the Makefile with `make`, run:
```bash
make shared_rpath
./bin/main.out
```

- Using the `LD_LIBRARY_PATH` environment variable: it is an environment variable that tells the operating system where to look for shared libraries when running an executable. So, we can add the location of the shared object to `LD_LIBRARY_PATH` and simplify the command prompt. Run:
```bash
gcc -Wall -g -o bin/main.out src/main.c -L./lib -lhello
LD_LIBRARY_PATH=./lib:$LD_LIBRARY_PATH
./bin/main.out
```
- If using the Makefile with `make`, run:
```bash
make shared_ldpath
LD_LIBRARY_PATH=./lib:$LD_LIBRARY_PATH
./bin/main.out
```


## Conclusion
These are different strategies to use libraries in C. They vary in speed, memory consumption, size and complexity. It is up to you to analyse which way fits the requirements you have.


## Links
- [Shared libraries with GCC on Linux](https://www.cprogramming.com/tutorial/shared-libraries-linux-gcc.html)
- [How to write your own code libraries in C - Jacob Sorber YouTube video](https://youtu.be/JbHmin2Wtmc)
