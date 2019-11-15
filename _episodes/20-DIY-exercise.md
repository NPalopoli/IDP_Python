# DYI time

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
