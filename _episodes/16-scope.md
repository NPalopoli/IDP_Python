---
title: "Variable Scope"
teaching: 10
exercises: 0
questions:
- "How do function calls actually work?"
- "How can I determine where errors occurred?"
objectives:
- "Identify local and global variables."
- "Identify parameters as local variables."
- "Read a traceback and determine the file, function, and line number on which the error occurred, the type of error, and the error message."
keypoints:
- "The scope of a variable is the part of a program that can 'see' that variable."
---
## Variable scope

*   There are only so many sensible names for variables.
*   People using functions shouldn't have to worry about
    what variable names the author of the function used.
*   People writing functions shouldn't have to worry about
    what variable names the function's caller uses.
*   The part of a program in which a variable is visible is called its *scope*.

Scope refers to the 'visibility' of variables. In other words, which parts of your program can see or use it. 
Normally, every variable has a global scope. Once defined, every part of your program can access a variable. 
It is very useful to be able to limit a variable's scope to a single function. The variable wil have a limited scope.
*   Example: the same word may have two meanings in different sentences / text / languages.

~~~
pressure = 103.9

def adjust(t):
    temperature = t * 1.43 / pressure
    return temperature
~~~
{: .language-python}

*   `pressure` is a *global variable*.
    *   Defined outside any particular function.
    *   Visible everywhere.
*   `t` and `temperature` are *local variables* in `adjust`.
    *   Defined in the function.
    *   Not visible in the main program.
    *   Remember: a function parameter is a variable
        that is automatically assigned a value when the function is called.

~~~
print('adjusted:', adjust(0.9))
print('temperature after call:', temperature)
~~~
{: .language-python}
~~~
adjusted: 0.01238691049085659
~~~
{: .output}
~~~
Traceback (most recent call last):
  File "/Users/swcarpentry/foo.py", line 8, in <module>
    print('temperature after call:', temperature)
NameError: name 'temperature' is not defined
~~~
{: .error}

> ## Local and Global Variable Use
>
> Trace the values of all variables in this program as it is executed.
>
> ~~~
> limit = 100
>
> def clip(value):
>     return min(max(0.0, value), limit)
>
> myvalue = -22.5
> print(clip(myvalue))
> ~~~
> {: .language-python}
{: .challenge}


