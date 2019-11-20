---
title: "Regular expressions"
teaching: 20
exercises: 20
questions:
- ?
objectives:
- ?
keypoints:
- ?
- ?
---

## Regular expressions
* A regular expression is a sequence of characters that define a search pattern
* Regular expression language is more or less universal
* Python can work with regular expressions thanks to its [`re`](https://docs.python.org/3.8/library/re.html) library
* In most cases, to work with regular expression, you just need this 4 lines of code 

~~~
import re
my_regex = re.compile('regex pattern')
match = my_regex.search('haystack string')
print(match.group())
~~~
{: language-python}

## Find patterns in text
* Imagine you need to systematically extract telephone numbers from messages

~~~
text = """Alice,
 My number is 415-730-0000.
 Call me when it's convenient.
 -Bob"""
~~~
{: .language-python}

* If you wanted to write a generic function to do it, it would look like this

~~~
def isPhoneNumber(text):
    if len(text) != 12:
        return False
    for i in range(0, 3): # check area code
        if not text[i].isdecimal():
            return False
    if text[3] != '-':
        return False
    for i in range(4, 7): # check first 3 digits
        if not text[i].isdecimal():
        return False
    if text[7] != '-':
        return False
    for i in range(8, 12): # check last 4 digits
        if not text[i].isdecimal():
            return False
        return True
  
text = """Alice,
 My number is 415-730-0000.
 Call me when it's convenient.
 -Bob"""
 
for i, _ in enumerate(text):
    if isPhoneNumber(text[i:i+12]):
        print(text[i:i+12])
~~~
{: .language-python}





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
