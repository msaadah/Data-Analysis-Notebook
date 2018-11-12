# Data-Analysis-Notebook

The process taken for each these datasets is similar as they follow this pattern:


1. Importing packages

The first thing you will find at the top of all dataset files are packages. This is done to keep the file organized and to keep track of which modules have been imported already. 

In python, there are many built-in functions which kind of act like commands that can be utilized to access the file or manipulate the data. Most of these functions can be accessed through "import" statements. Once you import a certain module/package, you have access to a lot of functions that can be applied to the data.    

Of course, the first step to analyzing data is to access it. Since there are datasets coming from different formats (ie csv, xml, json, etc) you need to import the proper commands that allow you to parse through the file. Parsing essentially means having the ability to understand and differentiate different objects in the dataset, such as identifying column and row names and knowing which datapoints correspond to what inputs. 

2. Gathering Basic Info

After reading through the dataset and getting a table of values, we want to get a basic understanding of what the data is presenting. The following commands give a general idea as to what information is being extracted. 
        .info() :
    .describe() :
.isnull().sum() :
      .dtypes() :
     .nunique() : 
        .plot() :