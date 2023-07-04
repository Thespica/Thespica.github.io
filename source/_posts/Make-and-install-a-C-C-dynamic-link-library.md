---
title: Build a C/C++ dynamic-link library
date: 2023-06-16 14:11:51
tags: C++
categories: Default
---

Sometimes we need to provide or use a third-party C/C++ dynamic-link libraries. This blog aims to teach how to build, install and use a C/C++ dynamic-link library.<!--more-->

# Build and use manually

First, this is our construction:

```shell
.
├── hello.cc
├── hello.h
└── main.cc
```

Now we have a source file called **hello.cc** and a head file called **hello.h**:

```cpp
// hello.cc

#include <iostream>

void hello() {
    std::cout << "Hello form dynamic-link libaray!" << std::endl;
}
```

```cpp
// hello.h

#ifndef HELLO_H
#define HELLO_H
void hello();
#endif
```

## Compile source code to object(.o) file

```shell
g++ hello.cc -c -fPIC
```

* **-c**: only compile the source code and not link it with other files;
* **-fPIC**: the flag tells the complier to generate PIC(position-independent code), this is common in building dynamic library.

## Link object file to library(libxxx.so) file

```shell
g++ hello.o -shared -o libhello.so
```

* **-shared**: to create a shared library;
* **-o libxxx.so**: to name the output file as **libxxx.so**.

## Use the shared library

There is a source code called `main.cc` use this shared library:

```cpp
#inlcude "hello.h"

int main() {
    hello();
    return 0;
}
```

Now we can compile and link it to **libhello.so**:

```shell
g++ main.cc -o main.out -L . -l hello
```

* **-L**: tells the linker to look for library files in which directory, now it is current directory.
* **-l xxx**: tells the linker to link with the library file **libxxx.so**.

After we run command above, we can get a executable file called **main.out**, so we can execute it:

```shell
./main.out
```

If met error that can't load shared library like this:

```shell
./main.out: error while loading shared libraries: libhello.so: cannot open shared object file: No such file or directory
```

It's related with default **LD_LIBRARY_PATH**, export it as current directory:

```shell
export LD_LIBRARY_PATH=`pwd`
```

Then we can get the sequence as we hope:

```
Hello form dynamic-link libaray!
```

## Install header file

There is a new problem: if we use this shared library in other directories, source code can't find needed head file like:

```shell
main.cc:1:10: fatal error: hello.h: No such file or directory
    1 | #include "hello.h"
      |          ^~~~~~~~~
compilation terminated.
```

In order to make it convenient to include header file, we need to copy our head file to default search path, such as `usr/local/include` or `usr/include`.

```shell
sudo cp ~/shared-lib-demo/hello.h /usr/local/include
```

and run:

```shell
g++ main.cc -o main.out -L ../shared-lib-demo/ -l hello
```

Similar as we copy header file to default header file search path, we can also copy **libhello.so** to default library search path or add a link which point to **libxxx.so** in **LD_LIBRARY_PATH**, so that we don't need to use `-L ../shared-lib-demo/`.

Then we can get what we want:

```shell
./main
Hello form dynamic-link libaray!
```

# Build and use with CMake

Dynamic-link library construction:

```shell
.
├── CMakeLists.txt
├── include
│   └── hello.h
├── src
│   └── hello.cc
└── test.cc
```

**hello.h** and **hello.cc** are as same as above, and **test.cc** is similar to **main.cc**.

The differences between building manually and building with CMake is that we just need to write a **CMakeLists.txt** rather than run command line by line. Of course, we should install **Cmake** and **GNU make** before we use it.

Let's take a consideration about what we need do to build and use a dynamic-link library:

- Adding source code to shared library;
- Including header directories when write code that use the library;
- Linking new project with our library.

## Build source code for shared library

In order to link it conveniently, we can install our library so that we can use it directly.

So the **CMakeLists.txt** looks like:

```cmake
# set version requirement and project name
cmake_minimum_required(VERSION 3.16)
project(hello_lib)

# set CXX standard
set(CMAKE_CXX_STANDARD 17)

# add a library called hello_lib, SHARED means dynamic-link library
add_library(hello_lib SHARED src/hello.cc)
# let new project who use this library can find header needed
target_link_directories(hello_lib PUBLIC include)

# add executalble file and link it with library
add_executable(test.out test.cc)
target_link_libraries(test.out hello_lib)

# install the library target and the header files
install(TARGETS hello_lib LIBRARY DESTINATION lib)
install(FILES include/hello.h DESTINATION include)
```

For test it, we can make a directory called build and `cd build`, run:

```
cmake ..    # out source compile
make
```

Then you well get a executable called **test.out** which can let you run and test it.

To install, just run `make install` after run `make`.

## Use installed library in new project

New project directory:

```shell
.
├── CMakeLists.txt
└── main.cc
```

**main.cc**:

```cpp
#include "hello.h"

int main() {
    hello();
    return 0;
}
```

It's no doubt that we need tell the new project that it should depend the shard library, so write it in cmake:

```cmake
cmake_minimum_required(VERSION 3.16)
project(cmake_dynamic_lib_test)

set(CMAKE_CXX_STANDARD 17)

add_executable(main.out main.cc)

# find installed lib, link it to executalble file
find_library(HELLO_LIB hello_lib)
target_link_libraries(main.out ${HELLO_LIB})
```

Run

```shell
mkdir build
cd build
cmake ..
make
```

You can get **main.out** as we expect.