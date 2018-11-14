# Data-Analysis-Notebook


HOW TO PUT IMAGES ON GITHUB, INCLUDE EXAMPLES FOR DATA MANIPULATION (GROUPBY, TOKENIZING, ENCODING STUFF)

The process taken for each these datasets is similar as they follow this pattern:


## 1. Importing packages

The first thing you will find at the top of all dataset files are packages. This is done to keep the file organized and to keep track of which modules have been imported already. 

In python, there are many built-in functions which kind of act like commands that can be utilized to access the file or manipulate the data. Most of these functions can be accessed through "import" statements. Once you import a certain module/package, you have access to a lot of functions that can be applied to the data.    

### *Parsing through different datafile types* 

Of course, the first step to analyzing data is to access it. Since there are datasets coming from different formats (ie csv, xml, json, etc) you need to import the proper commands that allow you to parse through the file. Parsing essentially means having the ability to understand and differentiate different objects in the dataset, such as identifying column and row names and knowing which datapoints correspond to what inputs. 

One method of extracting the data can be by saving the datafile to a location on your computer and accessing it from there. However, to refrain from having to access the file from different directories on different computers (and make the code transferrable and functionable), the best method would be to have the datafile extracted given a URL as that is a more universal approach. Depending on the different datafiles, there are various modules packages that can be used to read through the file. The following are some examples explored:


***DATA : wget***

    import wget
    url = 'https://archive.ics.uci.edu/ml/machine-learning-databases/heart-disease/processed.cleveland.data'
    cleveland_data = wget.download(url)
    df = pd.read_csv(cleveland_data, header = None, index_col = False, delimiter = ',')


***CSV : pandas***

    import pandas as pd
    df = pd.read_csv('https://perso.telecom-paristech.fr/eagan/class/igr204/data/TimeUse.csv')

*if using a Mac with Py3, use the following commands:*

    import io
    import requests
    url = 'https://perso.telecom-paristech.fr/eagan/class/igr204/data/TimeUse.csv'
    s = requests.get(url).content
    df = pd.read_csv(io.StringIO(s.decode('utf-8')))


***XML : parser***

    user_agent_url = 'https://www.w3schools.com/xml/plant_catalog.xml'
    xml_data = requests.get(user_agent_url).content

    class XML2DataFrame:

        def __init__(self, xml_data):                    #reading into file
            self.root = ET.XML(xml_data)



        def parse_root(self, root):                      #list of dictionaries and attributes of the children under this XML root.           
            return [self.parse_element(child) for child in iter(root)]


        def parse_element(self, element, parsed=None):   #Collect {key:attribute} and {tag:text} from XML elements and all children into a single dictionary of strings.
            if parsed is None:
                parsed = dict()
            for key in element.keys():
                parsed[key] = element.attrib.get(key)
            if element.text:
                parsed[element.tag] = element.text

            for child in list(element):                  #apply recursion
                self.parse_element(child, parsed)
            return parsed

        def process_data(self):                          #Initiate the root XML, parse it, and return a dataframe
            structure_data = self.parse_root(self.root)
            return pd.DataFrame(structure_data)

    xml2df = XML2DataFrame(xml_data)
    xml_df = xml2df.process_data()


***JSON : urllib, json***

    import urllib, json
    from pandas.io.json import json_normalize
    url = "https://api.weatherbit.io/v2.0/forecast/hourly?city=Champaign,IL&key=b884eed1798f46feab4effc907e537a7&hours=1"
    response = urllib.urlopen(url)
    data = json.loads(response.read())


    def get_row(url):
        #url = "https://api.weatherbit.io/v2.0/forecast/hourly?city=Champaign,IL&key=b884eed1798f46feab4effc907e537a7&hours=1"
        response = urllib.urlopen(url)
        data = json.loads(response.read())
        columns = []
        data_list = []
        for key in data:
            value1= data[key]
            #print key+'---->'
            #print value1

            if isinstance(value1,list):
                for key2 in value1:
                    #print key2 
                    if isinstance(key2,dict):
                        for key3 in key2:
                    #        print "**********" +key3+'---->'
                            value3 = key2[key3]


                            if isinstance(value3,dict):
                                for key4 in value3:
                                    columns.append(key4)
                                    data_list.append(value3[key4])
                     #               print "======================================"+key4 +'---->'
                     #               print "))))))))))))" + str(value3[key4])
                            else:
                                columns.append(key3)
                                data_list.append(value3)
                                #print columns
                      #          print " &&&&&&&&&&&&" +str(value3)
            else:
                columns.append(key)
                data_list.append(value1)
                #print columns
                #print value1

        return columns,data_list
    col,row= get_row("https://api.weatherbit.io/v2.0/forecast/hourly?city=Champaign,IL&key=b884eed1798f46feab4effc907e537a7&hours=1")




***WEBSITE : parser, requests, BeautifulSoup***

    import requests, re
    from bs4 import BeautifulSoup

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

.info() : number of non-null values in column + datatype

.describe() : count, mean, std, 25/50/75th percentiles, min and max values

.isnull().sum() : total number of non-null values
  
.dtypes() : datatype (category, float64, string, object)
    
.nunique() : number of non-unique values in series
     
.plot() : plot of all data values in dataframe

.value_counts() : lists out all of the categories and how many values are in each
        
        
## 3. Cleaning data into readable format

After importing data values, you may need to adjust or remove the data for certain functions to be applied. 

HeartDiseaseSet1CSV : data was already clean

TimeUseSet2CSV : 
1. Time was presented as HH:MM, however in a pandas dataframe python is only able to manipulate the data if it is in HH:MM:SS format. After converting to HH:MM:SS, the time value need to be converted to an hour value (eg 10:30:00 = 10.5 hours, not 10.3)
2. Combining similar column name columns together: there were a lot of misc columns that could be combined to a single one. 

PlantsSet3XML : data was already clean

FinVizStreamData : code incorporated in parser

WeatherHrly : code incorporated in parser



## 4. Manipulating Data

### *Groupby*: 
This structure extracts specific data from within the dataframe and categorizes all of the data by grouping to a certain value in a category. This returns multiple tables grouping the different outputs in the category you specify. The best way to use this approach is by identifying which category has the smallest number of unique values because the number of categories will be reasonable (typically choose a column with 3-20 unique values so as not to produce more than 20 tables). You can gain more information by choosing to groupby and getting the mean, max, min, etc in each category of the dataframe.


### *Tokenizing*:
Tokenizing is a useful technique if the data you have includes strings. What this method does is it takes the values in a category and makes it into a list. Within that list, each word is separated into a single string.    


    from nltk.tokenize import word_tokenize
    tokens = xml_df['LIGHT']. apply(word_tokenize) 


As mentioned, this method produces a large list with sublists consisting of the strings. To have all of the strings combined into a single list, you can do so by adding the following code:


    flattened_list = []
    for x in tokens:
        for y in x:
            flattened_list.append(y)
    print flattened_list



### *Label Encoding*:
Label encoding is a useful way of reassigning different results in a category to a numerical value. Note: the category results need to be a "category" datatype before applying the code to encode the results.

    xml_df['LIGHT']= xml_df['LIGHT'].astype('category')      #convert 'LIGHT' from obj to category
    xml_df["LIGHT_LE"] = xml_df["LIGHT"].cat.codes              #label encoding

### *Binary*:

The point of binary coding serves as a similar purpose to label encoding where results get reassigned to different values. The only difference is that instead of having different numbers assigned according to the different results, binary encoding labels only one result with 1 and everything else is 0. This makes any comparison focused only on the column labeled "1" since all values don't produce an output.


    xml_df["ZONE_binary"] = np.where(xml_df["ZONE"].str.contains("Annual"), 1, 0)


## 5. Data Visualization

### *General Plot*

The first thing you can do is plot all the values on a single plot so you can get an idea of general trends and values in the data.

    df.plot()

![alt text](https://www.python.org/static/community_logos/python-logo-master-v3-TM-flattened.png)





### *Log Plot*

It is useful to make a log plot to see the scale of the results.

    df.plot()
    plt.yscale('log')

### *Histogram*

A histogram distributes the results into bins depending on what the output result is.

    num_bins = 5
    n, bins, patches = plt.hist(x, num_bins, alpha=0.5)


### *Scatterplot*

This is good for comparing two parameters and seeing if there are any strong correlations.

    x = df['MaxHR']
    y = df['RestBP']
    plt.scatter(x, y, alpha=0.5)


### *Heatmap*

This heatmap gives a correlation value for all parameters relative to each other. It is very nice to visually determine whether one category is correlated to another based on the color. Note: it is expected that categories compared against each other are going to be 1 meaning exact same correlation). In the plot, there should be a diagonal of 1s and both sides opposite of the diagonal should be symmetric to each other. 

    f,ax = plt.subplots(figsize=(18, 16))
    sns.heatmap(df.corr(), annot=True, linewidths=.8, fmt= '.1f',ax=ax)
    plt.show()


### *Pie Charts*

If the data you have can be presented in percentages or proportions, you may use a pie chart to present the data in a more visual way.

    plt.pie()

You can have multiple pie charts to compare percentages relative to each other if there is data for multiple entries


    fig, ((ax1, ax2), (ax3, ax4), (ax5, ax6), (ax7, ax8), (ax9, ax10), (ax11, ax12), (ax13, ax14)) = plt.subplots(7, 2, figsize = (70,70), subplot_kw={'aspect':'equal'})

    axX.pie(dataX)

    plt.subplots_adjust(left=None, bottom=None, right=None, top=None, wspace=None, hspace=None)


### *Bar Graph*

This may look similar to a histogram but doesn't necessarily try to fit the results into a bell curve. Instead it just gives the actual value for each category. Representing the data in a horizontal bar graph gives a nice comparison for the different categories.


    import matplotlib.pyplot as plt; plt.rcdefaults()
    plt.figure(figsize=(10,7))

    plt.barh(np.arange(len(light.index)), light.values)
    plt.yticks(np.arange(len(light.index)), light.index)


### *Wordcloud*

A word cloud is a nice way of visualizing words depending on the frequency of their use. The size of the word correlates to the number of times the word is found in the category.


    from wordcloud import WordCloud, STOPWORDS 
    wordcloud = WordCloud().generate(' '.join(xml_df['LIGHT']))
    plt.imshow(wordcloud)
    plt.axis("off")
    plt.show()



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
