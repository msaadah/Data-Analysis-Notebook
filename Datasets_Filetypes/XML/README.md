# Data-Analysis-Notebook



# <font color=blue>1. Importing packages and Reading Data

The first thing you will find at the top of all dataset files are packages. This is done to keep the file organized and to keep track of which modules have been imported already. 

In python, there are many built-in functions which kind of act like commands that can be utilized to access the file or manipulate the data. Most of these functions can be accessed through "import" statements. Once you import a certain module/package, you have access to a lot of functions that can be applied to the data.    

### *Parsing through different datafile types* 

Of course, the first step to analyzing data is to access it. Since there are datasets coming from different formats (ie CSV, XML,JSON, etc) you need to import the proper commands that allow you to parse through the file. Parsing essentially means having the ability to understand and differentiate different objects in the dataset, such as identifying column and row names and knowing which datapoints correspond to what inputs. 

One method of extracting the data can be by saving the datafile to a location on your computer and accessing it from there. However, to refrain from having to access the file from different directories on different computers (and make the code transferrable and functionable), the best method would be to have the datafile extracted given a URL as that is a more universal approach. Depending on the different datafiles, there are various modules packages that can be used to read through the file. The following are some examples explored:

## <font color=orange>Importing packages:


***XML : parser***

    import requests                               #makes data from URLs easy to gather
    import xml.etree.ElementTree as ET 

   

## <font color=orange>Reading Data

To output the data, it is best to use "data.head()" to get the first 5 rows of the dataset. Displaying the entire dataset takes a lot of processing time and is unnecessary especially before cleaning the data.


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
    xml_df.head()



### *Pandas Dataframe:* 
The best way to manipulate data in such an organized way is through a pandas dataframe. As the name suggests, this "dataframe" structure is similar to a table to keep track of values. The columns are organized in a structure called Series. This information is useful because if you want to input/remove/adjust anything in the data, you need to search how to do so in a pandas dataframe. So the best method is to place the data from a url of a specific datatype into a dataframe.



# <font color=blue>2. Gathering Basic Info

After reading through the dataset and getting a table of values, we want to get a basic understanding of what the data is presenting. The following commands give a general idea as to what information is being extracted. 

<font color=orange>A) .info() : number of non-null values in column + datatype

<font color=orange>B) .dtypes() : datatype (category, float64, string, object)

<font color=orange>C) .describe() : count, mean, std, 25/50/75th percentiles, min and max values

<font color=orange>D) .isnull().sum() : total number of non-null values
     
<font color=orange>E) .nunique() : number of non-unique values in series
     
<font color=orange>F) .value_counts() : lists out all of the categories and how many values are in each
        
***

# <font color=blue>3. Cleaning data into readable format

***

After importing data values, you may need to adjust or remove the data for certain functions to be applied. 

## 3A

#### XML:
PlantsSet3XML : data was already clean

FinVizStreamData : code incorporated in parser

WeatherHrly : code incorporated in parser


## 3B

After cleaning data, it's best to gather basic information done in step 2 to make sure the data is properly cleaned. 

<font color=orange>1) .dtypes() : datatype (category, float64, string, object)

<font color=orange>2) .describe() : count, mean, std, 25/50/75th percentiles, min and max values


***

# <font color=blue>4. Manipulating Data

***

## <font color=orange>*Groupby*: 
This structure extracts specific data from within the dataframe and categorizes all of the data by grouping to a certain value in a category. This returns multiple tables grouping the different outputs in the category you specify. The best way to use this approach is by identifying which category has the smallest number of unique values because the number of categories will be reasonable (typically choose a column with 3-20 unique values so as not to produce more than 20 tables). You can gain more information by choosing to groupby and getting the mean, max, min, etc in each category of the dataframe.


![alt text](images/groupby.JPG)


## <font color=orange>*Tokenizing*:
Tokenizing is a useful technique if the data you have includes strings. What this method does is it takes the values in a category and makes it into a list. Within that list, each word is separated into a single string.    


    from nltk.tokenize import word_tokenize
    tokens = xml_df['LIGHT']. apply(word_tokenize) 


![alt text](images/tokenizing.JPG)


As mentioned, this method produces a large list with sublists consisting of the strings. To have all of the strings combined into a single list, you can do so by adding the following code:


    flattened_list = []
    for x in tokens:
        for y in x:
            flattened_list.append(y)
    print flattened_list


![alt text](images/tokenizing_list.JPG)



## <font color=orange>*Label Encoding*:
Label encoding is a useful way of reassigning different results in a category to a numerical value. Note: the category results need to be a "category" datatype before applying the code to encode the results.

    xml_df['LIGHT']= xml_df['LIGHT'].astype('category')      #convert 'LIGHT' from obj to category
    xml_df["LIGHT_LE"] = xml_df["LIGHT"].cat.codes              #label encoding


![alt text](images/label_encoding.JPG)


## <font color=orange> *Binary*:

The point of binary coding serves as a similar purpose to label encoding where results get reassigned to different values. The only difference is that instead of having different numbers assigned according to the different results, binary encoding labels only one result with 1 and everything else is 0. This makes any comparison focused only on the column labeled "1" since all values don't produce an output.


    xml_df["ZONE_binary"] = np.where(xml_df["ZONE"].str.contains("4"), 1, 0)


![alt text](images/binary.JPG)


***

# <font color=blue>5. Data Visualization

***


## <font color=orange>*A) General Plot*

The first thing you can do is plot all the values on a single plot so you can get an idea of general trends and values in the data.

    df.plot()

![alt text](images/all_plot.png)


## <font color=orange>*B) Log Plot*

It is useful to make a log plot to see the scale of the results.

    df.plot()
    plt.yscale('log')

![alt text](images/log_plot.png)


## <font color=orange>*C) Histogram*

A histogram distributes the results into bins depending on what the output result is.

    num_bins = 5
    n, bins, patches = plt.hist(x, num_bins, alpha=0.5)

![alt text](images/histogram.png)


## <font color=orange>*D) Scatterplot*

This is good for comparing two parameters and seeing if there are any strong correlations.

    x = df['MaxHR']
    y = df['RestBP']
    plt.scatter(x, y, alpha=0.5)

![alt text](images/scatterplot.png)


## <font color=orange>*E) Bar Graph*

This may look similar to a histogram but doesn't necessarily try to fit the results into a bell curve. Instead it just gives the actual value for each category. Representing the data in a horizontal bar graph gives a nice comparison for the different categories.


    import matplotlib.pyplot as plt; plt.rcdefaults()
    plt.figure(figsize=(10,7))

    plt.barh(np.arange(len(light.index)), light.values)
    plt.yticks(np.arange(len(light.index)), light.index)

![alt text](images/barh.png)



## <font color=orange>*F) Heatmap*

This heatmap gives a correlation value for all parameters relative to each other. It is very nice to visually determine whether one category is correlated to another based on the color. Note: it is expected that categories compared against each other are going to be 1 meaning exact same correlation). In the plot, there should be a diagonal of 1s and both sides opposite of the diagonal should be symmetric to each other. 

    f,ax = plt.subplots(figsize=(18, 16))
    sns.heatmap(df.corr(), annot=True, linewidths=.8, fmt= '.1f',ax=ax)
    plt.show()

![alt text](images/heatmap.png)



## <font color=orange>*I) Wordcloud*

A word cloud is a nice way of visualizing words depending on the frequency of their use. The size of the word correlates to the number of times the word is found in the category.


    from wordcloud import WordCloud, STOPWORDS 
    wordcloud = WordCloud().generate(' '.join(xml_df['LIGHT']))
    plt.imshow(wordcloud)
    plt.axis("off")
    plt.show()

![alt text](images/wordcloud.png)



***

# <font color=blue>6. Statistical testing

***

For the following tests to take place, the values within the categories have to be integars or floats. In the case that they are not, you can always apply label encoding or binary code to reassign the results into an integar value.

## <font color=orange>T-test

A T-test is used to determine if there's significant difference between the mean of 2 groups. For the 2 groups, there's always an independent (eg gender) and dependent (eg test scores) variable. If the independent variable has more than one outcome, use ANOVA. Testing done to determine to what degree of confidence the difference between means of 2 groups did not occur by chance. Infer that dependent variable fits normal distribution so we can always predict what the outcome is.


    cat1 = xml_df['LIGHT_LE']
    cat2 = xml_df['ZONE_LE']
    ttest_ind(cat1, cat2)

This test returns (t-score, two-tailed p-value)

Larger t score -> Larger difference between both groups
Smaller p value -> Less probability that results were by chance


## <font color=orange>Pearson Correlation

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

