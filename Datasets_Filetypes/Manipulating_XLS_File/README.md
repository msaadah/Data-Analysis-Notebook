# How to Manipulate XLS File



***

# 1. Importing packages and Reading Data

***



For an XLS file, you need to import the same packages as you would with a CSV. The only exception is when you read the file instead of using "pd.read_csv(filename)" you use "pd.read_excel(filename)".  
   


***

# 2. Gathering Basic Info

***

After reading through the dataset and getting a table of values, we use a few commands to extract basic information. 





***

# 3. Cleaning data into readable format

***


This file doesn't require any cleaning so data will remain as is.  




***

# 4. Manipulating Data

***


## *Groupby*: 

This method clusters the output of the data depending on a specific category. For example, the number of deaths for each Cause_name was given.  




***

# 5. Data Visualization

***


### *A) General Plot*

The first thing you can do is plot all the values on a single plot so you can get an idea of general trends and values in the data.

   

### *B) Log Plot*

It is useful to make a log plot to see the scale of the results.

   

### *C) Histogram*

A histogram distributes the results into bins depending on what the output result is.

   

### *D) Scatterplot*

This is good for comparing two parameters and seeing if there are any strong correlations.

  

### *E) Bar Graph*

This may look similar to a histogram but doesn't necessarily try to fit the results into a bell curve. Instead it just gives the actual value for each category. Representing the data in a horizontal bar graph gives a nice comparison for the different categories.


   
### *F) Heatmap*

This heatmap gives a correlation value for all parameters relative to each other. It is very nice to visually determine whether one category is correlated to another based on the color. Note: it is expected that categories compared against each other are going to be 1 meaning exact same correlation). In the plot, there should be a diagonal of 1s and both sides opposite of the diagonal should be symmetric to each other. 
