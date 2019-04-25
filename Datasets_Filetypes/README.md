# Data-Analysis-Notebook



This notebook looks at how to manipulate data depending on the file type. The following filetypes are explored: 

### **CSV**: tables, spreadsheets

### **XML**: similar to websites but structure already set

### **XLS**: spreadsheets

### **JSON**: data interchange, websites, data objects, similar to Java

### **HTML**: websites, tags




# Additional Features

There are additional features you can do with a notebook such as:


#### Increasing the width of the notebook to the entire page: 

    from IPython.core.display import display, HTML
    display(HTML("<style>.container { width:100% !important; }</style>"))
    

#### Inserting an image in a README file

First you have to save the image from the notebook using: 

    plt.savefig('image_name.png')
    
Tip: This code needs to go *before* plt.show() or else the image won't display when saved

Then, you insert this line in the README page

    ![alt text](image_name.png)

