---
layout: page
title: UNIX Memory Model Analyzer
description: A memory model visualization tool
img: assets/img/1.jpg
importance: 3
category:
---

This tool performs a static analysis of a file containing C source code, identifying the location in memory of the variables defined within the code.

For more details, please visit the [GitHub repository](https://github.com/AB20CS/MemoryModelAnalyzer).

### Sample
**Source Code (Input):**

    // evil global variables
    int evil_glob_var_1;
    float evil_glob_var_2;

    void fun1(int x)
    {
    int y;
    int z;
    printf("%d \n", x+y+z); 
    }

    int fun2(float z)
    {
    float x;
    return (int)(z+x);
    }

    int main(int argc, char** argv)
    {
    int w;

    fun1(w);
    fun2();

    return 0;
    }

**Output:**
    >>> Memory Model Layout <<<
    ***  exec // text ***
    prog1.c

    ### ROData ###       scope  type  size

    ### static data ###
    evil_glob_var_1   global   int   4
    evil_glob_var_2   global   float   4

    ### heap ###

    ####################
    ### unused space ###
    ####################

    ### stack ###
    x   fun1   int   4
    y   fun1   int   4
    z   fun1   int   4
    z   fun2   float   4
    x   fun2   float   4
    argc   main   int   4
    argv   main   char**   8
    w   main   int   4

    **** STATS ****
    - Total number of lines in the file: 26
    - Total number of functions: 3
        fun1, fun2, main
    - Total number of lines per functions:
        fun1: 3
        fun2: 2
        main: 6
    - Total number of variables per function:
        fun1: 3
        fun2: 2
        main: 3
    //////////////////////////////
