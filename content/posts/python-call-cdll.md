---
title: 'Python call dll library'
date: 2019-02-18T21:49:58+08:00
draft: false
slug: python-call-dll-library
categories:
  - 'Programming'
tags:
  - 'Python'
  - 'ctypes'
---

## Install MingGW and gcc

Install MingGW if you are using Windows, which I am. After intalling MingGW, you need to install gcc compiler. With all this done, you need to add your MingGW Bin directory to your PATH environment. Lauch cmd.exe, type "gcc -v", you will see some information if you installed gcc successfully.

## Write some code in C

A simple c file.

```c
__declspec( dllexport ) int add2(int a, int b)
{
    return a+b;
}
```

Do not add extern "C", unless you are writing this in c++, otherwise there would be an error.

`"error: expected identifier or '(' before string constant extern C"`

## Compile your C file

Compile your c file with gcc.

`gcc -shared -o mytest.dll test.c`

"-shared" means making this a dll file.

## Call DLL in python

Using module `ctypes` to load a dll file.

```python
import ctypes

dllPath = r'C:\Users\Administrator\Desktop\mylib.dll'
dll = ctypes.CDLL(dllPath)

#Change your c argument type to ctypes type
dll.add2.argtypes=[ctypes.c_int,ctypes.c_int]
print dll.add2(1,2)
```

If you want to load your dll by using ctype.windll.LoadLibrary(), you have to change your C code to

```c
__declspec( dllexport ) int __stdcall add2(int a, int b)
{
    return a+b;
}
```

otherwise, there would be an error.

`"ValueError: Procedure probably called with too many arguments (8 bytes in excess)"`
