# Data-Analysis-Notebook



## 1. Importing packages and Reading Data

The first thing you will find at the top of all dataset files are packages. This is done to keep the file organized and to keep track of which modules have been imported already. 

In python, there are many built-in functions which kind of act like commands that can be utilized to access the file or manipulate the data. Most of these functions can be accessed through "import" statements. Once you import a certain module/package, you have access to a lot of functions that can be applied to the data.    

### *Parsing through different datafile types* 

Of course, the first step to analyzing data is to access it. Since there are datasets coming from different formats (ie CSV, XML,JSON, etc) you need to import the proper commands that allow you to parse through the file. Parsing essentially means having the ability to understand and differentiate different objects in the dataset, such as identifying column and row names and knowing which datapoints correspond to what inputs. 

One method of extracting the data can be by saving the datafile to a location on your computer and accessing it from there. However, to refrain from having to access the file from different directories on different computers (and make the code transferrable and functionable), the best method would be to have the datafile extracted given a URL as that is a more universal approach. Depending on the different datafiles, there are various modules packages that can be used to read through the file. The following are some examples explored:

## Importing packages:



***WEBSITE : parser, requests, BeautifulSoup***

    import requests, re
    from bs4 import BeautifulSoup

   

## Reading Data

To output the data, it is best to use "data.head()" to get the first 5 rows of the dataset. Displaying the entire dataset takes a lot of processing time and is unnecessary especially before cleaning the data.


***WEBSITE***

    #https://github.com/redklouds/Finviz-API/blob/master/finviz.py
    #request = requests.get('http://finviz.com/')

    class FinViz:

        def __init__(self):
            """
            Function Name: Default Constructor
            PRECONDITIONS: None
            POSTCONDITIONS: 
                ->Initializes the object data at the current time
            ASSUMPTIONS: None
            """
            #make the request
            request = requests.get('http://finviz.com/')
            #maxe the soup
            soup = BeautifulSoup(request.text,'html5lib')
            #soup has been brewed
            self._html = soup
            #self._html = BeautifulSoup(requests.get('http://finviz.com/').text,'html5lib')
            self._data = list()


        def refresh(self):
            """
                Function Name: refresh
                Descriptions:
                    -> Polls the webservice for new/updated data
                PRECONDITIONS: None
                POSTCONDITIONS: 
                    -> Object reinitialized with new data
                ASSUMPTIONS: None
            """        
            self.__reinitialize()

        def __reinitialize(self):
            """
                Function Name: (Helper) _reinitialize 
                Description:
                    -> calls another get requsests to poll refreshed data manually
                PRECONDITIONS: None
                POSTCONDITIONS: 
                    -> Refreshes data
                ASSUMPTIONS: None
            """        
            request = requests.get('http://finviz.com/')
            #maxe the soup
            soup = BeautifulSoup(request.text,'html5lib')
            #soup has been brewed
            self._html = soup
            #self._html = BeautifulSoup(requests.get('http://finviz.com/').text,'html5lib')
            self._data = list()


        def _parseColumnData(self, data):
            """
                Function Name: 
                PRECONDITIONS:
                POSTCONDITIONS:
                ASSUMPTIONS:
            """        
            ret_data = data.findChild()
            ret_data = list(ret_data.children)
            result = list()
            for idx in ret_data:
                try:
                    #parse the given data, into 
                    result.append(self._parseText(idx.getText()))                
                except:
                    #None Type, scrapping None Object, skip.
                    pass
            return result  

        def _parseText(self, text):
            """
                Function Name: 
                PRECONDITIONS:
                POSTCONDITIONS:
                ASSUMPTIONS:
            """        
            #use regex for faster parsing of text, searching
            #for numbers and words, better and faster.

            #define regEx pattern
            #"find all alpha upper and lower words
            #+ one and unlimited timees
            #match words who may or may not have spcaes betweent hem and
            #are mixed caes zero to unliited times
            regExText = '[A-Z a-z]+'
            #match a number with at least 1 to unlimited length
            ##the number must have a period and 1 through 2 numbers after it
            #match fully if there is a '+' OR '-' ZERO or 1 times
            regExDigit = '(\+|-?\d+.\d{1,2})'
            #regExDigit = '(\d+.\d{1,2})'
            listText = re.findall(regExText, text)
            listDigit = re.findall(regExDigit, text)
            resultSet = {
                         'index':listText[0],
                         'price':listDigit[0],
                         'change':listDigit[1],
                         'volume':listDigit[2],
                         'signal':listText[1]
                         }
            #return the resulting dictionary
            return resultSet

        def getLeftColumn(self):
            """
                Function Name: getLeftColumn
                Description:
                    -> get get's the raw data in the left column of the website
                PRECONDITIONS: None
                POSTCONDITIONS:
                    ->returns a dictionary containing all the parsed data
                    -> in the form:
                        Stock Index
                        Current Price
                        Percent Change
                        Volume
                        Signal
                ASSUMPTIONS: None
            """        
            data = self._getMainColumnData(0)
            a = self._parseColumnData(data)
            return a

        def getRightColumn(self):
            """
                Function Name: getLeftColumn
                Description:
                    -> get get's the raw data in the right column of the website
                PRECONDITIONS: None
                POSTCONDITIONS:
                    ->returns a dictionary containing all the parsed data
                    -> in the form:
                        Earnings before
                        Stock Index
                        Current Price
                        Percent Change
                        Volume Signal
                ASSUMPTIONS: None
            """  
            data = self._getMainColumnData(1)
            a = self._parseColumnData(data)
            return a

        def _getMainColumnData(self,column):
            """
                Function Name: _getMainColumnData
                Description:
                   -> our switch helper function depending on which 
                   parameter we get, this function returns the respective data set
                   pertaining to that column

                PRECONDITIONS: Integer 1 or 0( left will be 0, 1 will be right)
                POSTCONDITIONS:
                    Left:
                    Top Gainers
                    New high
                    Overbought
                    Unusual Volume
                    Upgrades
                    Earnings Before

                    The column on the right are:
                    Top Losers
                    New Low
                    Oversold
                    Most Volitile
                    Most Active
                    Downgrades
                    Earnings After
                    Insider Selling
                ASSUMPTIONS: None
            """        
            #scrape the specific elements
            searchResult = self._html.findAll('table', {'class':'t-home-table'})

            # we just want the first or second matches
            return searchResult[column]

        def marketStatus():
            """
                TODO: get Market status |Positive|Negative
                Function Name: 
                PRECONDITIONS:
                POSTCONDITIONS:
                ASSUMPTIONS:
            """        
            pass
        def getTrends(self):
            left_col = self.getLeftColumn()
            right_col = self.getRightColumn()

            combined_dict = list()

            for i in left_col:
                combined_dict.append(i)
            for i in right_col:
                combined_dict.append(i)

            #print(combined_dict)
            #return_dict = {"right_column": right_col, "left_column":left_col}
            return combined_dict



### *Pandas Dataframe:* 
The best way to manipulate data in such an organized way is through a pandas dataframe. As the name suggests, this "dataframe" structure is similar to a table to keep track of values. The columns are organized in a structure called Series. This information is useful because if you want to input/remove/adjust anything in the data, you need to search how to do so in a pandas dataframe. So the best method is to place the data from a url of a specific datatype into a dataframe.



## 2. Gathering Basic Info

After reading through the dataset and getting a table of values, we want to get a basic understanding of what the data is presenting. The following commands give a general idea as to what information is being extracted. 

A) .info() : number of non-null values in column + datatype

B) .dtypes() : datatype (category, float64, string, object)

C) .describe() : count, mean, std, 25/50/75th percentiles, min and max values

D) .isnull().sum() : total number of non-null values
     
E) .nunique() : number of non-unique values in series
     
F) .value_counts() : lists out all of the categories and how many values are in each
        
        
## 3. Cleaning data into readable format

After importing data values, you may need to adjust or remove the data for certain functions to be applied. 

### A)

#### HTML:

UCI Shallow
1. Replacing empty spaces with 'none'

        data = data.replace('', np.nan)
        
2. Converting certain columns from type object to either category or float

        for i in data.columns:
        if i== 'Name' or i=='Data Types' or i=='Default Task' or i=='Attribute Types':  
            data[i] = data[i].str.strip().astype('category')
        else:
            data[i] = data[i].astype(float)


### B)

After cleaning data, it's best to gather basic information done in step 2 to make sure the data is properly cleaned. 

1) .dtypes() : datatype (category, float64, string, object)

2) .describe() : count, mean, std, 25/50/75th percentiles, min and max values



## 4. Manipulating Data


### *Groupby*: 
This structure extracts specific data from within the dataframe and categorizes all of the data by grouping to a certain value in a category. This returns multiple tables grouping the different outputs in the category you specify. The best way to use this approach is by identifying which category has the smallest number of unique values because the number of categories will be reasonable (typically choose a column with 3-20 unique values so as not to produce more than 20 tables). You can gain more information by choosing to groupby and getting the mean, max, min, etc in each category of the dataframe.


![alt text](images/groupby.JPG)



## 5. Data Visualization


### *A) General Plot*

The first thing you can do is plot all the values on a single plot so you can get an idea of general trends and values in the data.

    df.plot()

![alt text](images/all_plot.png)


### *B) Log Plot*

It is useful to make a log plot to see the scale of the results.

    df.plot()
    plt.yscale('log')

![alt text](images/log_plot.png)


### *C) Histogram*

A histogram distributes the results into bins depending on what the output result is.

    num_bins = 5
    n, bins, patches = plt.hist(x, num_bins, alpha=0.5)

![alt text](images/histogram.png)


### *D) Scatterplot*

This is good for comparing two parameters and seeing if there are any strong correlations.

    x = df['MaxHR']
    y = df['RestBP']
    plt.scatter(x, y, alpha=0.5)

![alt text](images/scatterplot.png)


### *E) Bar Graph*

This may look similar to a histogram but doesn't necessarily try to fit the results into a bell curve. Instead it just gives the actual value for each category. Representing the data in a horizontal bar graph gives a nice comparison for the different categories.


    import matplotlib.pyplot as plt; plt.rcdefaults()
    plt.figure(figsize=(10,7))

    plt.barh(np.arange(len(light.index)), light.values)
    plt.yticks(np.arange(len(light.index)), light.index)

![alt text](images/barh.png)



### *F) Heatmap*

This heatmap gives a correlation value for all parameters relative to each other. It is very nice to visually determine whether one category is correlated to another based on the color. Note: it is expected that categories compared against each other are going to be 1 meaning exact same correlation). In the plot, there should be a diagonal of 1s and both sides opposite of the diagonal should be symmetric to each other. 

    f,ax = plt.subplots(figsize=(18, 16))
    sns.heatmap(df.corr(), annot=True, linewidths=.8, fmt= '.1f',ax=ax)
    plt.show()

![alt text](images/heatmap.png)



### *G) Pie Charts*

If the data you have can be presented in percentages or proportions, you may use a pie chart to present the data in a more visual way.

    plt.pie()

![alt text](images/single_pie_plot.png)


You can have multiple pie charts to compare percentages relative to each other if there is data for multiple entries


    fig, ((ax1, ax2), (ax3, ax4), (ax5, ax6), (ax7, ax8), (ax9, ax10), (ax11, ax12), (ax13, ax14)) = plt.subplots(7, 2, figsize = (70,70), subplot_kw={'aspect':'equal'})

    axX.pie(dataX)

    plt.subplots_adjust(left=None, bottom=None, right=None, top=None, wspace=None, hspace=None)

![alt text](images/multiple_pie_plot.png)


### *H) Box Plot*

A box plot is a nice way of showing the shape of the data distribution. There are 5 different numbers obtained from this figure: minimum, lower (first) quartile, median, upper (third) quartile, and maximum. The box spans the interquartile range and the ends of the box are the upper and lower quartiles. The line through the middle indicates the median.


    trace0 = go.Box(
        y = data.Birthrate,
        name = "Birthrates",
        boxpoints='all',
        pointpos=0,
        fillcolor='rgba(255, 255, 255, 0.8)',
        marker = dict(color = 'rgba(0, 255, 0, 0.8)')
        )
    
    plotdata = go.Figure(data = [trace0])
    iplot(plotdata)

![alt text](images/box_plot.png)


### *I) Wordcloud*

A word cloud is a nice way of visualizing words depending on the frequency of their use. The size of the word correlates to the number of times the word is found in the category.


    from wordcloud import WordCloud, STOPWORDS 
    wordcloud = WordCloud().generate(' '.join(xml_df['LIGHT']))
    plt.imshow(wordcloud)
    plt.axis("off")
    plt.show()

![alt text](images/wordcloud.png)


### *J) World Projection*

A word cloud is a nice way of visualizing words depending on the frequency of their use. The size of the word correlates to the number of times the word is found in the category.


    from wordcloud import WordCloud, STOPWORDS 
    wordcloud = WordCloud().generate(' '.join(xml_df['LIGHT']))
    plt.imshow(wordcloud)
    plt.axis("off")
    plt.show()

![alt text](images/world_projection_plot.png)



## 6. Statistical testing

For the following tests to take place, the values within the categories have to be integars or floats. In the case that they are not, you can always apply label encoding or binary code to reassign the results into an integar value.

### T-test

A T-test is used to determine if there's significant difference between the mean of 2 groups. For the 2 groups, there's always an independent (eg gender) and dependent (eg test scores) variable. If the independent variable has more than one outcome, use ANOVA. Testing done to determine to what degree of confidence the difference between means of 2 groups did not occur by chance. Infer that dependent variable fits normal distribution so we can always predict what the outcome is.


    cat1 = xml_df['LIGHT_LE']
    cat2 = xml_df['ZONE_LE']
    ttest_ind(cat1, cat2)

This test returns (t-score, two-tailed p-value)

Larger t score -> Larger difference between both groups
Smaller p value -> Less probability that results were by chance

### Pearson Correlation

Pearson correlation test gives strongest linear correlation between 2 groups, expressed in terms of r between and including -1 and +1. 0 means there's no correlation. The sign indicates that there can be either positively or negatively correlated. 

    x = xml_df['ZONE_LE']
    y = xml_df['PRICE']
    pearsonr(x, y)
    
The result is gives: (Pearson's correlation coefficient, 2-tailed p-value)






## Useful things for Jupyter Notebook/GitHub interface

### How to upload figures to README file

To insert an image into a README file, you first have to save the file from the jupyter notebook using the following command:

    plt.savefig('image_name.png')
    
Tip: This code needs to go *before* plt.show() or else the image won't display when saved

Check to see where the image is in the jupyter notebook. You can either keep it with all the other files or move it to an images folder. In the README file, you can add the following code to the area you want the image to be placed. 

    ![alt text](image_name.png)
    ![alt text](images/image_name.png)    #use if placed plot images in another folder labeled "images"

