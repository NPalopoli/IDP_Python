---
title: "Regular expressions"
teaching: 20
exercises: 20
questions:
- "How do I define regular expressions in Python?"
- "How do I use them?"
objectives:
- Understand what a regular expression is and its use-cases.
- Write regular expressions patterns.
- Find single and multiple instances of a pattern in a string with the `re` library.
keypoints:
- The `re` library is used to find regex in a string with Python.
- Alternative characters are defined with `[]`
- Repetitions are defined with `{}`
- Other syntax symbols are well documented at [https://docs.python.org/3/library/re.html](https://docs.python.org/3/library/re.html)
---

## Regular expressions
* A regular expression is a sequence of characters that defines a search pattern
* Regular expression language is more or less universal
* Python can work with regular expressions thanks to its [`re`](https://docs.python.org/3.8/library/re.html) library
* In most cases, to work with regular expression, you just need these 4 lines of code 

~~~
# import regular expression library
import re
# compile a regular expression from a string
regex = re.compile('regex pattern')
# search the compiled regular expression in a target string
match = regex.search('haystack string')
# print out the exact match of your regular expression
print(match.group())
~~~
{: .language-python}

## Find patterns in text without regex
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
def is_phone_number(text):
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
    if is_phone_number(text[i:i+12]):
        print(text[i:i+12])
~~~
{: .language-python}


* This function is still readable, but our pattern is very simple.
* Code complexity will increase quickly with your pattern complexity.

## Using a regex to extract an american phone number
* This pattern is quite simple, we have:
    * 3 digits for the area code
    * a dash
    * 3 more digits
    * another dash 
    * 4 more digits
* In regular expressions `\d` is the regex syntax for a digit


~~~
# r before a string converts it to a raw string. 
# In a raw string you don't need to escape the \
# In a normal string you would need to escape each \ with another \ 
regex = re.compile(r"\d")
~~~
{: .language-python}


* `\d` will match a single digit
* Our phone number pattern looks like this


~~~
regex = re.compile(r"\d\d\d-\d\d\d\-\d\d\d\d")
~~~
{: .language-python}


* To search our pattern in a target string we will use the
 [`search`](https://docs.python.org/3/library/re.html#re.search) method of the compiled `regex`
 
~~~
text = """Alice,
 My number is 415-730-0000.
 Call me when it's convenient.
 -Bob"""

match = regex.search(text)
~~~
{: .language-python}

* The `search` method can either return:
    * a [match object](https://docs.python.org/3/library/re.html#match-objects) if `search` finds a matching pattern
    * `None` if `search` finds nothing
* You should check if `search` results are not `None`
* When we call `group()` on the match object it will return the text that matches the regex

~~~
if match is not None:
    print(match.group(0))
~~~
{: .language-python}

~~~
415-730-0000
~~~
{: .ouput}

## Character class
* Character class represent a range (or class) of characters

| `\d` | Digit characters (numbers)             |
| `\w` | Word characters (letters & numbers, _) |
| `\s` | Space characters (space, tab, \n)      |
| `\D` | Non-digit                              |
| `\W` | Non-word                               |
| `\S` | Non-space                              |

* You can create your own character classes using square braces `[]`

| `[aeiouAeiou]`  | Matches vowels                   |
| `^[aeiouAeiou]` | Matches non-vowels               |
| `[0-9a-zA-Z]`   | Matches vowels, same as `\w`     |
| `^[0-9a-zA-Z]`  | Matches non-vowels, same as `\W` |

* We are not literally looking for `^` or `-` characters, they represent something
* In general, punctuation marks in regular expression have specific meaning
* If you were to actually look for a punctuation mark you would need to escape them (`\^`, `\?`, `\\`, ...)

| `[\(\)]` | Matches `(` or `)` |

## Specifying quantity
* You can use curly braces `{}` to specify repetitions of a pattern
* Quantity is always specified after the pattern that you are looking for
* We can rewrite our phone number regex as:

~~~
regex = re.compile(r"\d{3}-\d{3}-\d{4}")
~~~
{: .language-python}

* There are a number of options when it comes to specifying quantities

| `\d`      | One digit                         |
| `\d?`     | Zero or one digits                |
| `\d*`     | Zero or more digits               |
| `\d+`     | One or more digits                |
| `\d{3}`   | Exactly three digits              |
| `\d{3,5}` | Between three and five digits     |
| `\d{3,}`  | Three or more digits              |

* Of course quantity pattern will work with any character and with any character class

| `\s`                 | One space                        |
| `\S?`                | Zero or one non-space            |
| `\w*`                | Zero or more word characters     |
| `\W+`                | One or more non-word characters  |
| `\D{3}`              | Exactly three non-digits         |
| `\[aeoiuAEOIU]{3,5}` | Between three and five vowels    |

## Grouping
* In Japanese letters are usually consonant-vowels combinations (**sayonara = sa - yo - na - ra**)
* Based on what we now up to this point, our regex would look like `[^aeoiu][aeoiu]+`
* However the quantifier refers only to what comes immediately before it
* That regex would also match something like
    * saaaaaaaaaaa
    * saoiaioiueai
* We can group the two character classes together and apply quantifier to the group

~~~
regex = re.compile(r"[^aeoiu][aeoiu])+")
match = regex.search("sayonara")
print(match.group())
~~~
{: .language-python}

~~~
sayonara
~~~
{: .output}
    
## Regex buddy
* Testing your regex writing code can be slow and frustrating
* There are online tools called regex buddy or regex testers that make the process of creating a regex much easier
    * [Pyregex](http://pyregex.com)
    * [Regex101](https://regex101.com/)
    * many others
    
* Let's say that we want to match comma formatted numbers
* Our regex should encode for: “One to three digits, followed by zero or more groups of comma-digit-digit-digit” 
* This translates to `\d{1,3}(,\d{3)*`, let's try it out on one of the linked tool with this sentence: "UniProt 
has 2 types of entries. It currently has 181,787,788 automatically annotated entries and 561,356 curated entries"

  

>
> ## Finding strings in a sequence
> Write a function that detects the restriction site "GAATTC" in any input sequence
> and test it with two examples: one that contains the string and the other that doesn't.
> > ## Solution
> > ~~~
> > import re
> > def find_rs(dna):
> >     if re.search(r"GAATTC", dna):
> >         print("restriction site found!")
> > find_rs("ATCGCGAATTCAC")
> > find_rs("ATCGCGAATTAAC")
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

>
> ## Alternative characters
> AvaII recognition site recognizes two different sequences: GGACC and GGTCC. 
> Modify your previous function to detect both of them.
> > ## Solution
> > ~~~
> > def find_rs(dna):
> >     if re.search(r"GG[AT]CC", dna):
> >         print("restriction site found!")
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

>
> ## Multiple characters
> The following pattern may represent full-length eukaryotic messenger RNA sequences:
> `^AUG[AUGC]{30,1000}A{5,10}$`
> Describe all the syntax symbols.
> > ## Solution
> > `^` refers to the sequence beginning. It starts with `AUG` followed by between 30 and 1000 bases 
> > which can be A, U, G or C. Finally, a poly-A tail of 5 to 10 bases closes the sequence. The sequence 
> > end is represented by `$`. 
> {: .solution}
{: .challenge}


> ## Let's move to protein sequences!
> The transcriptional co-repressor TOPLESS (TPL) binds to short motifs in 
> proteins. LxLxL (where x is any amino acid) is a well characterized TPL 
> binding motif. But other motifs exist, such as LxLxPP, (R/K)LFGV and TLxLF.
> Check (with Python) if the moss (Physcomitrella patens) Aux/IAA protein, which 
> represses auxin-mediated gene expression contains the LxLxPP 
> repression motif. The protein sequence is: <br>
> KFSNEVVHKSMNITEDCSALTGALLKYSTDKSNMNFETLYRDAAVESPQHEVSNESGSTLKEHDYFGLSEVSSS
> NSSSGKQPEKCCREELNLNESATTLQLGPPAAVKPSGHADGADAHDEGAGPENPAKRPAHHMQQESLADGRKAA
> AEMGSFKIQRKNILEEFRAMKAQAHMTKSPKPVHTMQHNMHASFSGAQMAFGGAKNNGVKRVFSEAVGGNHIAAS
> GVGVGVREGNDDVSRCEEMNGTEQLDLKVHLPKGMGMARMAPVSGGQNGSAWRNLSFDNMQGPLNPFFRKSLVSK
> MPVPDGGDSSANASNDCANRKGMVASPSVQPPPAQNQTVGWPPVKNFNKMNTPAPPASTPARACPSVQRKGASTS
> SSGNLVKIYMDGVPFGRKVDLKTNDSYDKLYSMLEDMFQQYISGQYCGGRSSSSGESHWVASSRKLNFLEGSEYV
> LIYEDHEGDSMLVGDVPWELFVNAVKRLRIMKGSEQVNLAPKNADPTKVQVAVG
> > ## Solution
> > ~~~
> > peptide = "MKFSNEVVHKSMNITEDCSALTGALLKYSTDKSNMNFETLYRDAAVESPQHEVSNESGSTLKEHDYFGLSEVSSSNSSSGKQPEKCCREELNLNESATTLQLGPPAAVKPSGHADGADAHDEGAGPENPAKRPAHHMQQESLADGRKAAAEMGSFKIQRKNILEEFRAMKAQAHMTKSPKPVHTMQHNMHASFSGAQMAFGGAKNNGVKRVFSEAVGGNHIAASGVGVGVREGNDDVSRCEEMNGTEQLDLKVHLPKGMGMARMAPVSGGQNGSAWRNLSFDNMQGPLNPFFRKSLVSKMPVPDGGDSSANASNDCANRKGMVASPSVQPPPAQNQTVGWPPVKNFNKMNTPAPPASTPARACPSVQRKGASTSSSGNLVKIYMDGVPFGRKVDLKTNDSYDKLYSMLEDMFQQYISGQYCGGRSSSSGESHWVASSRKLNFLEGSEYVLIYEDHEGDSMLVGDVPWELFVNAVKRLRIMKGSEQVNLAPKNADPTKVQVAVG"
> > if re.search(r"L*L*PP", peptide):
> >     print("Repression domain found")
> > else:
> >     print("Nope, nothing")
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}


> ## Want to find multiple instances?
> Analise the same sequence of the previous exercise to count how many polyproline
> motifs `PxxP` does it contain. At the end, print instance and then their number.
> To find multiple instances of a regex, use the function `finditer` instead of `search`.
> > ## Solution
> > ~~~
> > matches = re.finditer(r"P**P", peptide)
> > result = [] 
> > for m in matches: 
> >     result.append(m.group())
> > print(result) 
> > print(len(result))
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}
