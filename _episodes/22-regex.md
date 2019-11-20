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
