# How to Manipulate JSON File



***

# 1. Importing packages and Reading Data

***



For a JSON file, you need to import "json" which helps to read and extract data from json files and put into a pandas dataframe. "urllib" is another module to import which helps to open URLs.   
   


***

# 2. Gathering Basic Info

***

After reading through the dataset and getting a table of values, we use a few commands to extract basic information. 




***

# 3. Cleaning data into readable format

***


After importing data values, you may need to adjust or remove the data for certain functions to be applied. 


### A)


Obesity Data: Changing column names + column types



### B)

After cleaning data, it's best to gather basic information done in step 2 to make sure the data is properly cleaned and column names are changed the way they are specified. 



***

# 4. Manipulating Data

***


### *Groupby*: 

This method clusters the output of the data depending on a specific category. For example, in the Obesity data the number of samples from each state was given. 





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

  

### *G) Pie Charts*

If the data you have can be presented in percentages or proportions, you may use a pie chart to present the data in a more visual way.

   
You can have multiple pie charts to compare percentages relative to each other if there is data for multiple entries


   

### *H) Box Plot*

A box plot is a nice way of showing the shape of the data distribution. There are 5 different numbers obtained from this figure: minimum, lower (first) quartile, median, upper (third) quartile, and maximum. The box spans the interquartile range and the ends of the box are the upper and lower quartiles. The line through the middle indicates the median.



### *I) Wordcloud*

A word cloud is a nice way of visualizing words depending on the frequency of their use. The size of the word correlates to the number of times the word is found in the category.


   

### *J) World Projection*

A word cloud is a nice way of visualizing words depending on the frequency of their use. The size of the word correlates to the number of times the word is found in the category.



