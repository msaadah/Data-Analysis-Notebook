# How to Manipulate HTML/PHP File



***

# 1. Importing packages

***



For HTML/PHP data, you need to import "BeautifulSoup" which is a Python package used to parse through HTML/website. "request" is another module to open up data from a URL.   




***

# 2. Reading Data

***

For HTML/PHP data, you use BeautifulSoup with an html parser to get extract the data you want from a website.


   


***

# 3. Gathering Basic Info

***

After reading through the dataset and getting a table of values, we use a few commands to extract basic information.  

data.info()
data.dtypes
data.describe()
data.isnull().sum()
data.nunique()
data.Column.value_counts()




***

# 4. Cleaning data into readable format

***

After importing data values, you may need to adjust or remove the data for certain functions to be applied. 


### A)


UCI Database Data (Shallow): Replacing empty spaces with 'none' + changing column types



### B)

After cleaning data, it's best to gather basic information done in step 2 to make sure the data is properly cleaned and column names are changed the way they are specified. 


data.dtypes
data.describe()


***

# 5. Manipulating Data

***


### *Groupby*: 

This method clusters the output of the data depending on a specific category. For example, in the Obesity data the number of samples from each state was given. 




***

# 6. Data Visualization

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

  
        
