# How to Manipulate CSV File



***

# 1. Importing packages

***

For a CSV file, importing 'pandas' is sufficient for reading and organizing the data into a dataframe. The rest of the imported packages help with data manipulation. 



***

# 2. Reading Data

***

For a CSV file, "pd.read_csv(filename)" is enough to read through the datafile.

For the "Heart Disease" dataset, the file is a .data. You can open it through Excel by searching for all filetypes.



***

# 3. Gathering Basic Info

***

After reading the data into a dataframe, we can apply basic functions to get quantitative information.



        
***

# 4. Cleaning data into readable format

***

After importing data values, you may need to adjust, remove, or clean the data.


## 4A

#### CSV:

*Countries of the World:* 

1. Removing commas to convert numbers from object to type category or float to perform functions easier (ie 11,23 --> 11.23)



*TimeUse:* 

1. Convert time data to HH:MM:SS format to be readable in dataframe (ie 10:30:00 -->  10.5 hours, not 10.3)


2. Combining similar column name columns together to organize the data 



*HeartDiseaseSet1CSV :* Data was already clean



## 4B

After cleaning data, it's best to gather basic information again as in Part 2 to make sure the data is properly cleaned. 




***

# 5. Manipulating Data

***


## *Groupby*: 

This method clusters the output of the data depending on a specific category. For example, in the TimeUse data, similar country names were combined and the mean was found for all of those output values to get a single value.




***

# 6. Data Visualization

***


## *A) General Plot*

The first thing you can do is plot all the values on a single plot so you can get an idea of general trends and values in the data.




## *B) Log Plot*

It is useful to make a log plot to see the scale of the results.

  


## *C) Histogram*

A histogram distributes the results into bins depending on what the output result is.

   


## *D) Scatterplot*

This is good for comparing two parameters and seeing if there are any strong correlations.

  

## *E) Bar Graph*

This may look similar to a histogram but doesn't necessarily try to fit the results into a bell curve. Instead it just gives the actual value for each category. Representing the data in a horizontal bar graph gives a nice comparison for the different categories.


   


## *F) Heatmap*

This heatmap gives a correlation value for all parameters relative to each other. It is very nice to visually determine whether one category is correlated to another based on the color. Note: it is expected that categories compared against each other are going to be 1 meaning exact same correlation). In the plot, there should be a diagonal of 1s and both sides opposite of the diagonal should be symmetric to each other. 



## G) Pie Charts
specific to 'TimeUse' dataset

If the data you have can be presented in percentages or proportions, you may use a pie chart to present the data in a more visual way. You can have multiple pie charts to compare percentages relative to each other if there is data for multiple entries




## *H) Box Plot*
specific to 'Countries of the World' dataset

A box plot is a nice way of showing the shape of the data distribution. There are 5 different numbers obtained from this figure: minimum, lower (first) quartile, median, upper (third) quartile, and maximum. The box spans the interquartile range and the ends of the box are the upper and lower quartiles. The line through the middle indicates the median.




## *J) World Projection*
specific to 'Countries of the World' dataset

A word cloud is a nice way of visualizing words depending on the frequency of their use. The size of the word correlates to the number of times the word is found in the category.


   


***

# 7. Statistical testing

***


For the following tests to take place, the values within the categories have to be integars or floats. In the case that they are not, you can always apply label encoding or binary code to reassign the results into an integar value.


## T-test

A T-test is used to determine if there's significant difference between the mean of 2 groups. For the 2 groups, there's always an independent (eg gender) and dependent (eg test scores) variable. If the independent variable has more than one outcome, use ANOVA. Testing done to determine to what degree of confidence the difference between means of 2 groups did not occur by chance. Infer that dependent variable fits normal distribution so we can always predict what the outcome is.


This test returns (t-score, two-tailed p-value)

Larger t score -> Larger difference between both groups
Smaller p value -> Less probability that results were by chance

## Pearson Correlation

Pearson correlation test gives strongest linear correlation between 2 groups, expressed in terms of r between and including -1 and +1. 0 means there's no correlation. The sign indicates that there can be either positively or negatively correlated. 

   
The result is gives: (Pearson's correlation coefficient, 2-tailed p-value)





***

## Useful things for Jupyter Notebook/GitHub interface

***


### How to upload figures to README file

To insert an image into a README file, you first have to save the file from the jupyter notebook using the following command:

    plt.savefig('image_name.png')
    
Tip: This code needs to go *before* plt.show() or else the image won't display when saved

Check to see where the image is in the jupyter notebook. You can either keep it with all the other files or move it to an images folder. In the README file, you can add the following code to the area you want the image to be placed. 

    ![alt text](image_name.png)
    ![alt text](images/image_name.png)    #use if placed plot images in another folder labeled "images"

