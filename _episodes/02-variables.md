---
title: "Variables and Assignment"
teaching: 50
exercises: 30
questions:
- "How can I store and use data in programs?"
objectives:
- "Assign values to variables."
- "Check the value of a variable."
- "Do basic operations with variables."
keypoints:
- "Use `variable = value` to assign a value to a variable in order to record it in memory."
- "Variables are created on demand whenever a value is assigned to them."
- "Use `print(something)` to display the value of `something`."
- "Variables persist between cells."
- "Variables must be created before they are used."
- "Variables can be used in calculations."
- "Python is case-sensitive."
- "Use meaningful variable names."
- "Use an index to get a single character from a string."
- "Use a slice to get a substring."
---
## Variables

Any Python interpreter can be used as a calculator:
~~~
3 + 5 * 4
~~~
{: .language-python}
~~~
23
~~~
{: .output}

This is great but not very interesting.
To do anything useful with data, we need to assign its value to a _variable_.
In Python, we can [assign]({{ page.root }}/reference/#assign) a value to a
[variable]({{ page.root }}/reference/#variable), using the equals sign `=`.
For example, to assign value `60` to a variable `weight_kg`, we would execute:

~~~
weight_kg = 60
~~~
{: .language-python}

From now on, whenever we use `weight_kg`, Python will substitute the value we assigned to
it. In essence, **a variable is just a name for a value**.

In Python, variable names:

 - can include letters, digits, and underscores
 - cannot start with a digit
 - are [case sensitive]({{ page.root }}/reference/#case-sensitive).

This means that, for example:
 - `weight0` is a valid variable name, whereas `0weight` is not
 - `weight` and `Weight` are different variables

## Types of data
Python knows various types of data. Three common ones are:

* integer numbers
* floating point numbers, and
* strings.

In the example above, variable `weight_kg` has an integer value of `60`.
To create a variable with a floating point value, we can execute:

~~~
weight_kg = 60.0
~~~
{: .language-python}

And to create a string we simply have to add single or double quotes around some text, for example:

~~~
weight_kg_text = 'weight in kilograms:'
~~~
{: .language-python}

## Using Variables in Python
To display the value of a variable to the screen in Python, we can use the `print` function:

~~~
print(weight_kg)
~~~
{: .language-python}

~~~
60.0
~~~
{: .output}

We can display multiple things at once using only one `print` command:

~~~
print(weight_kg_text, weight_kg)
~~~
{: .language-python}
~~~
weight in kilograms: 60.0
~~~
{: .output}

Moreover, we can do arithmetic with variables right inside the `print` function:

~~~
print('weight in pounds:', 2.2 * weight_kg)
~~~
{: .language-python}

~~~
weight in pounds: 132.0
~~~
{: .output}

The above command, however, did not change the value of `weight_kg`:
~~~
print(weight_kg)
~~~
{: .language-python}

~~~
60.0
~~~
{: .output}

To change the value of the `weight_kg` variable, we have to
**assign** `weight_kg` a new value using the equals `=` sign:

~~~
weight_kg = 65.0
print('weight in kilograms is now:', weight_kg)
~~~
{: .language-python}

~~~
weight in kilograms is now: 65.0
~~~
{: .output}

> ## Variables as Sticky Notes
>
> A variable is analogous to a sticky note with a name written on it:
> assigning a value to a variable is like putting that sticky note on a particular value.
>
> ![Variables as Sticky Notes](../fig/python-sticky-note-variables-01.svg)
>
> This means that assigning a value to one variable does **not** change
> values of other variables.
> For example, let's store the subject's weight in pounds in its own variable:
>
> ~~~
> # There are 2.2 pounds per kilogram
> weight_lb = 2.2 * weight_kg
> print(weight_kg_text, weight_kg, 'and in pounds:', weight_lb)
> ~~~
> {: .language-python}
>
> ~~~
> weight in kilograms: 65.0 and in pounds: 143.0
> ~~~
> {: .output}
>
> ![Creating Another Variable](../fig/python-sticky-note-variables-02.svg)
>
> Let's now change `weight_kg`:
>
> ~~~
> weight_kg = 100.0
> print('weight in kilograms is now:', weight_kg, 'and weight in pounds is still:', weight_lb)
> ~~~
> {: .language-python}
>
> ~~~
> weight in kilograms is now: 100.0 and weight in pounds is still: 143.0
> ~~~
> {: .output}
>
> ![Updating a Variable](../fig/python-sticky-note-variables-03.svg)
>
> Since `weight_lb` doesn't "remember" where its value comes from,
> it is not updated when we change `weight_kg`.
{: .callout}

Words are useful, but what's more useful are the sentences and stories we build with them.
Similarly, while a lot of powerful, general tools are built into Python,
specialized tools built up from these basic units live in
[libraries]({{ page.root }}/reference/#library)
that can be called upon when needed.

## Variables must be created before they are used

*   If a variable doesn't exist yet, or if the name has been mis-spelled,
    Python reports an error. (Unlike some languages, which "guess" a default value.)

~~~
print(last_name)
~~~
{: .language-python}
~~~
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-1-c1fbb4e96102> in <module>()
----> 1 print(last_name)

NameError: name 'last_name' is not defined
~~~
{: .error}

*   The last line of an error message is usually the most informative.
*   We will look at error messages in detail [later]({{ page.root }}/15-scope/#reading-error-messages).

> ## Variables Persist Between Cells
>
> Be aware that it is the *order* of execution of cells that is important in a Jupyter notebook, not the order
> in which they appear. Python will remember *all* the code that was run previously, including any variables you have
> defined, irrespective of the order in the notebook. Therefore if you define variables lower down the notebook and then
> (re)run cells further up, those defined further down will still be present. As an example, create two cells with the
> following content, in this order:
>
> ~~~
> print(myval)
> ~~~
> {: .language-python}
>
> ~~~
> myval = 1
> ~~~
> {: .language-python}
>
> If you execute this in order, the first cell will give an error. However, if you run the first cell *after* the second
> cell it will print out `1`. To prevent confusion, it can be helpful to use the `Kernel` -> `Restart & Run All` option which
> clears the interpreter and runs everything from a clean slate going top to bottom.
{: .callout}

## basic operations with strings

We can perform operations on variables, here some examples with strings. 
*   The characters (individual letters, numbers, and so on) in a string are
    ordered. For example, the string `'AB'` is not the same as `'BA'`. Because of
    this ordering, we can treat the string as a list of characters.
*   Each position in the string (first, second, etc.) is given a number. This
    number is called an **index** or sometimes a subscript.
*   Indices are numbered from 0.
*   Use the position's index in square brackets to get the character at that
    position.
    
![an illustration of indexing](../fig/string1.png)

~~~
message = 'hello'
print(message[0])
~~~
{: .language-python}
~~~
h
~~~
{: .output}

*   A part of a string is called a **substring**. A substring can be as short as a
    single character.
*   An item in a list is called an element. Whenever we treat a string as if it
    were a list, the string's elements are its individual characters.
*   A slice is a part of a string (or, more generally, any list-like thing).
*   We take a slice by using `[start:stop]`, where `start` is replaced with the
    index of the first element we want and `stop` is replaced with the index of
    the element just after the last element we want.
*   Mathematically, you might say that a slice selects `[start:stop)`.
*   The difference between `stop` and `start` is the slice's length.
*   Taking a slice does not change the contents of the original string. Instead,
    the slice is a copy of part of the original string.

~~~
print(message[0:3])
~~~
{: .language-python}
~~~
hell
~~~
{: .output}

## Use meaningful variable names.

*   Python doesn't care what you call variables as long as they obey the rules
    (alphanumeric characters and the underscore).

~~~
flabadab = 42
ewr_422_yY = 'Ahmed'
print(ewr_422_yY, 'is', flabadab, 'years old')
~~~
{: .language-python}

*   Use meaningful variable names to help other people understand what the program does.
*   The most important "other person" is your future self.

> ## Swapping Values
>
> Fill the table showing the values of the variables in this program
> *after* each statement is executed.
>
> ~~~
> # Command  # Value of x   # Value of y   # Value of swap #
> x = 1.0    #              #              #               #
> y = 3.0    #              #              #               #
> swap = x   #              #              #               #
> x = y      #              #              #               #
> y = swap   #              #              #               #
> ~~~
> {: .language-python}
> > ## Solution
> >
> > ~~~
> > # Command  # Value of x   # Value of y   # Value of swap #
> > x = 1.0    # 1.0          # not defined  # not defined   #
> > y = 3.0    # 1.0          # 3.0          # not defined   #
> > swap = x   # 1.0          # 3.0          # 1.0           #
> > x = y      # 3.0          # 3.0          # 1.0           #
> > y = swap   # 3.0          # 1.0          # 1.0           #
> > ~~~
> > {: .output}
> > 
> > These three lines exchange the values in `x` and `y` using the `swap`
> > variable for temporary storage. This is a fairly common programming idiom.
>{: .solution}
{: .challenge}

> ## Predicting Values
>
> What is the final value of `position` in the program below?
> (Try to predict the value without running the program,
> then check your prediction.)
>
> ~~~
> initial = 'left'
> position = initial
> initial = 'right'
> ~~~
> {: .language-python}
> > ## Solution
> >
> > ~~~
> > 'left'
> > ~~~
> > {: .output}
> >
>> The `initial` variable is assigned the value `'left'`.
> > In the second line, the `position` variable also receives
>> the string value `'left'`. In third line, the `initial` variable is given the
>> value `'right'`, but the `position` variable retains its string value
>> of `'left'`.
>{: .solution}
{: .challenge}

> ## Challenge
>
> If you assign `a = 123`,
> what happens if you try to get the second digit of `a` via `a[1]`?
>
> > ## Solution
> > Numbers are not strings or sequences and Python will raise an error if you try to perform an index operation on a
> > number. In the [next lesson on types and type conversion]({{ page.root }}/03-types-conversion/#convert-numbers-and-strings)
> > we will learn more about types and how to convert between different types. If you want the Nth digit of a number you
> > can convert it into a string using the `str` built-in function and then perform an index operation on that string.
> >
> > ~~~
> > a = 123
> > print(a[1])
> > ~~~
> > {: .language-python}
> > ~~~
> > TypeError: 'int' object is not subscriptable
> > ~~~
> > {: .error}
> > 
> > 
> > ~~~
> > a = str(123)
> > print(a[1])
> > ~~~
> > {: .language-python}
> > ~~~
> > 1
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

> ## Choosing a Name
>
> Which is a better variable name, `m`, `min`, or `minutes`?
> Why?
> Hint: think about which code you would rather inherit
> from someone who is leaving the lab:
>
> 1. `ts = m * 60 + s`
> 2. `tot_sec = min * 60 + sec`
> 3. `total_seconds = minutes * 60 + seconds`
>
> > ## Solution
> >
> > `minutes` is better because `min` might mean something like "minimum"
> > (and actually is an existing built-in function in Python that we will cover later).
> {: .solution}
{: .challenge}

> ## Slicing practice
>
> What does the following program print?
>
> ~~~
> atom_name = 'carbon'
> print('atom_name[1:3] is:', atom_name[1:3])
> ~~~
> {: .language-python}
>
> > ## Solution
> >
> > ~~~
> > atom_name[1:3] is: ar
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

> ## Slicing concepts
>
> 1.  What does `thing[low:high]` do?
> 2.  What does `thing[low:]` (without a value after the colon) do?
> 3.  What does `thing[:high]` (without a value before the colon) do?
> 4.  What does `thing[:]` (just a colon) do?
> 5.  What does `thing[number:some-negative-number]` do?
> 6.  What happens when you choose a `high` value which is out of range? (i.e., try `atom_name[0:15]`) 
>
> > ## Solutions
> >
> > 1. `thing[low:high]` returns a slice from `low` to the value before `high`
> > 2. `thing[low:]` returns a slice from `low` all the way to the end of `thing`
> > 3. `thing[:high]` returns a slice from the beginning of `thing` to the value before `high`
> > 4. `thing[:]` returns all of `thing`
> > 5. `thing[number:some-negative-number]` returns a slice from `number` to `some-negative-number` values from the end of `thing`
> > 6. If a part of the slice is out of range, the operation does not fail. `atom_name[0:15]` gives the same result as `atom_name[0:]`.
> >
> {: .solution}
{: .challenge}
