---
title: "Error messages"
teaching: 20
exercises: 20
questions:
- "How do I read an error message?"
- "How do I find the faulty line in a piece of code"
- "How do I handle expected errors"
objectives:
- "Identify important parts of an error message"
- "Readily modify the code that produces errors"
- "Predict and handle runtime errors that could come up in your code"
keypoints:
- "Understand the importance of errors"
- "Handle expected errors"
---

## Syntax Errors
* Syntax errors, also known as parsing errors, are the most common kind of complaint you get while you are learning Python:

~~~
if True print('Hello world')
~~~
{: .language-python}

~~~
 File "<stdin>", line 1
   if True print('Hello world')
                  ^
SyntaxError: invalid syntax
~~~
{: .error}

* The ‘arrow’ `^` points at the earliest point in the line where the error was detected.
* The error is caused by (or at least detected at) the token preceding the arrow: in the example,
the error is detected at the function print(), since a colon (':') is missing 
before it. 
* File name and line number are printed so you know where to look.


## Exceptions
* Even if a statement or expression is syntactically correct, it may cause an error when an attempt is made to execute it (runtime errors). 
* Runtime errors are called exceptions.
* Exceptions can be handled in Python programs. 
* Most exceptions are not handled by programs and result in error messages.

~~~
10 * (1/0)
~~~
{: .language-python}
~~~
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero
~~~
{: .error}

## Traceback
* Errors reports can be longer than 3 lines.
* A traceback is a report containing the function calls made in your code at a specific point
* Known by many names (stack trace, stack traceback, backtrace)

~~~
def greet(someone):
    print('Hello, ' + someon)

greet('Chad')
~~~
{: .language-python}

~~~
Traceback (most recent call last):
  File "/path/to/example.py", line 4, in <module>
    greet('Chad')
  File "/path/to/example.py", line 2, in greet
    print('Hello, ' + someon)
NameError: name 'someon' is not defined
~~~
{: .error}

* In Python, it’s best to read the traceback from the bottom up
* The last line of the traceback is the error message line. It contains the exception name that was raised.
* After the exception name is the error message that should help understanding the reason for the exception being raised.
* Further up are the function calls moving from most recent to least recent. 
* Each traced function call contains specifies the file name, line number, and module name.

## Handling Exceptions
* It is possible to write programs that handle selected exceptions. 
* Exceptions are handled with the `try ... except` statement:

~~~
for divider in [1, 4, 2, 6, 0, 8]:
    try:
        print(3/divider)
    except ZeroDivisionError:
        print("cannot divide by 0")
~~~
{: .language-python}
~~~
3.0
0.75
1.5
0.5
cannot divide by 0
0.375
~~~
{: .output}

* First, the `try` clause is executed.
* If no exception occurs, the `except` clause is skipped and execution of the try statement is finished.
* If an exception occurs during execution of the try clause, the rest of the clause is skipped. 
* If exception type matches the exception named after `except`, the except clause is executed.

>
> ## Reading Error Messages
>
> Read the traceback below, and identify the following:
>
> 1. How many levels does the traceback have?
> 2. What is the file name where the error occurred?
> 3. What is the function name where the error occurred?
> 4. On which line number in this function did the error occur?
> 5. What is the type of error?
> 6. What is the error message?
>
> ~~~
> ---------------------------------------------------------------------------
> KeyError                                  Traceback (most recent call last)
> <ipython-input-2-e4c4cbafeeb5> in <module>()
>       1 import errors_02
> ----> 2 errors_02.print_friday_message()
>
> /Users/ghopper/thesis/code/errors_02.py in print_friday_message()
>      13
>      14 def print_friday_message():
> ---> 15     print_message("Friday")
>
> /Users/ghopper/thesis/code/errors_02.py in print_message(day)
>       9         "sunday": "Aw, the weekend is almost over."
>      10     }
> ---> 11     print(messages[day])
>      12
>      13
>
> KeyError: 'Friday'
> ~~~
> {: .error}
> > ## Solution
> > 1. It has 3 levels
> > 2. The name is <ipython-input-...> indicating that the code is being run on 
> an interactive shell rather than from a file 
> > 3. `print_message()`  function
> > 4. 11
> > 5. KeyError
> > 6. 'Friday', the entity used as key 
> {: .solution}
{: .challenge}


> ## Write the code
>
> Write the small piece of code that produces the error shown.
> ~~~
> ---------------------------------------------------------------------------
> NameError                                 Traceback (most recent call last)
> <ipython-input-2-792d33226c70> in <module>
>       7 
> ----> 8 count()
> 
> <ipython-input-2-792d33226c70> in count()
>       1 def count():
> ----> 2     count_to_ten()
>       3 
>
> <ipython-input-2-792d33226c70> in count_to_ten()
>       4 def count_to_ten():
>       5     for i in range(10):
> ----> 6         print(ii)
>       7
>
> NameError: name 'ii' is not defined
> ~~~
> {: .error}
>
> > ## Solution
> > ~~~
> > def count():
> >     count_to_ten()
> >    
> > def count_to_ten():
> >     for i in range(10):
> >         print(ii)
> >
> > count()
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}
>
> ## Fill in the Blanks
>
> Fill in the blanks so the error is correct for the provided code. Try to complete the exercise
> without testing the code in your notebook
>
> ~~~
> numbers = [2, 4, 5, 1, 2, 4]
> numbers = numbers.sort()
> 
> for n in numbers:
>    print(n)
> ~~~
> {: .language-python}
>
> ~~~
> ---------------------------------------------------------------------------
> TypeError                                 Traceback (most recent call last)
> <_______-_____-6-22c0b8ebd443> in <module>
>       2 numbers = numbers.sort()
>       3 
> ----> 4 for n in numbers:
>       5     print(n)
> 
> _________: '________' object is not iterable
> ~~~
> {: .error}
>
>
> > ## Solution
> > ~~~
> > ---------------------------------------------------------------------------
> > TypeError                                 Traceback (most recent call last)
> > <ipython-input-6-22c0b8ebd443> in <module>
> >       2 numbers = numbers.sort()
> >       3 
> > ----> 4 for n in numbers:
> >       5     print(n)
> >
> > TypeError: 'NoneType' object is not iterable
> > ~~~
> > {: .error}
> {: .solution}
{: .challenge}


> ## Write the function
>
> Complete the function body following the instructions:
> * check `if` divisor is not 0
> * `else` return None and `print` a message informing that the function couldn't divide by 0
> * `try` to perform the division; then return the result
> * `except` the type is incorrect for the operation; then return None and `print` a message informing 
> that the type was wrong
>
> ~~~
> divisors = [1, 2, 3, 0, "s"]
> 
> def divide(dividend, divisor):
>   ___
> 
> for divisor in divisors:
>     print(divide(2, divisor))
> ~~~
> {: .language-python}
>
> ~~~
> 2.0
> 1.0
> 0.6666666666666666
> cannot divide by 0
> None
> divisor should be a number, instead it's a <class 'str'>
> None
> ~~~
> {: .output}
>
>
> > ## Solution
> > ~~~
> >  divisors = [1, 2, 3, 0, "s"]
> >  
> >  def divide(dividend, divisor):
> >      if divisor != 0:
> >          try:
> >              result = dividend / divisor
> >          except TypeError:
> >              print("divisor should be a number, instead it's a {}".format(type(divisor)))
> >              result = None
> >      else:
> >          print("cannot divide by 0")
> >          result = None
> >      return result
> >  
> >  for divisor in divisors:
> >      print(divide(2, divisor))
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}
