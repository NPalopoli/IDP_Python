---
title: "DIY time"
teaching: 10
exercises: 60
questions:
- "Can I build my own program?"
objectives:
- "Write your own function."
keypoints:
- "Check your understanding without the exercises guidance."
---

## Writing your function
Now it is time to use everything you have learned to process disorder data. 
The full task is: plot a pie chart to show the distribution of disorder regions
in Escherichia coli in N-terminal, C-terminal and center of the proteins annotated
in DisProt. 
Do it step-by-step:
1. Retrieve data from the file `data/DisProt_data_Escherichia-coli.csv`.
2. Consider the center of each disorder regions. Does it lie on the first third 
(N-terminal), second third (center) or last third (C-terminal) of the protein 
sequence?
3. Check (print) the number of regions in each category to be able to check the 
pie chart later.
4. Plot the chart. 

> > ## Solution
> > No, there is no pre-cooked code here. We italians don't like pre-cooked things.
> > This section will just add some more explanations if you feel you need them.
> > 1. We learned how to import data from a file in the lesson "Reading Tabular Data into DataFrames".
> > 2. You will have to loop (see lesson "For Loops") through the file rows (DisProt regions).
> > Each file row contains the start and end of each region, as well as the total length of the protein.
> > Therefore, calculating where does the center lie is a (or a series of) mathematical operation. 
> > You will also need a way, to store the result of each row while looping through the file. 
> > To do this, you can use the list type (see lesson "Lists"), create three lists and add each instance
> > to the correct category.
> > 3. Once you have the numbers (which will be the length of each list, right?) don't forget to check that
> > they make sense. This is crucial whenever you are writing each little piece of your code (each function
> > and module) because you may have designed the function in such a way that there is no Python error but still
> > there is a logical error. To do so, always design a simple input with pre-defined output and run your 
> > function to compare your expectations with the function result. 
> > 4. Finally, we learned how to plot data in the lesson "Plotting". We know we haven't used the pie chart, 
> > but you can check some examples of code in the [Python Graph Gallery](https://python-graph-gallery.com/matplotlib/)
> > 
> > Feel free to ask us if you have any doubt. We will cook your code together!
> {: .solution}
{: .challenge}


## Additional tasks
If you finish early or you want to challenge yourself at home, here some possible 
variations:
1. Now, you would like to do the same with the dataset of Caenorhabditis. Encapsulate
all your code in a function that takes the name of the file as input.
2. You prefer to use different definitions for the N- and C-terminals, e.g. the 
N-terminal is defined as the first 20% of the sequence length and the C-terminal the 
last 20%. Add an optional parameter to your function that will allow you to test
different combinations.
3. Change the graph type. Plot the same data as a barplot.

