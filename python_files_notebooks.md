# Python Files and Notebooks

> The ability to mix code cells with markdown cells is the greatest strength of the notebook, and it is the primary advantage of the notebook over a python script. We can include images, links and charts.
>
> Our notebooks record the choices we made, the alternatives we tried, the strengths and limitations of our approaches.   
>
> When we don't use this feature of notebook, we're riding extremely close to using notebooks as script IDE.
In which case, why not write our code in a python file and run it as a script?
>
> Notebooks give us a rarefied opportunity to fully, completely, and clearly document our scientific process. If we’re going to use notebook, we should utilize notebook features.

---

## Processing My Data with Jupyter Notebooks
Suppose we have 50,000 rows of labeled data. Each row has a paragraph of legal text in a column called base_content. We want to use this base_content to classify the rows into one of twelve categories of specific types of legal text.

We need to extract the data out of large CSV files, and then we need to do a little preprocessing to get them ready for testing models. There are a number of ways that we can do this.

---

### **Option 1. Do all our preprocessing in the same notebook as our analysis**

This is what the vast majority of Jupyter notebooks do.

<img src="https://i1.wp.com/chelseatroy.com/wp-content/uploads/2018/09/Screen-Shot-2018-09-10-at-5.33.37-PM.png?resize=1024%2C498&ssl=1">

From there, the same notebook goes straight into the analysis, like so:
<img src="https://i1.wp.com/chelseatroy.com/wp-content/uploads/2018/09/Screen-Shot-2018-09-10-at-5.36.07-PM.png?resize=1024%2C703&ssl=1">

This keeps everything in one place, which is nice. There are a couple of downsides:
1. **There is a high risk of things getting messed up if the cells run out of order.**

    In notebooks, you can run cells in whatever order you like. Theoretically this feature  allows you to move incrementally through code, or back and forth, while saving all your values in memory for later use in your exploration.

2. **Notebooks are hard to test.**

    Automated tests are an important piece of documentation for code that sees regular use, especially as the users become more distant from the writer (either different team members, or the same team member at a different point in time). When we ignore them, we expose ourselves to the risk of having zero information about how the system works if something ever breaks.

3. **This approach does not scale to multiple notebooks.**

    What if we need to do the same pre-processing for several models? Do we copy and paste this same litany of pre-processing steps into every notebook, and change it in all those places each time we modify our pre-processing regimen?

---

### **Option 2. Save processing in its own notebook and import that notebook into other notebooks**
<img src = "https://i1.wp.com/chelseatroy.com/wp-content/uploads/2018/09/Screen-Shot-2018-09-10-at-9.42.05-PM.png?resize=1024%2C575&ssl=1">

---

### **Option 3. Save processing in a csv that gets pulled by other notebooks**

What if we did our pre-processing in one notebook, put a cell at the end of the notebook that did <code>dataframe.to_csv('our_data.csv')</code>, and then did <code>pd.read_csv('our_data.csv')</code> at the beginning of each analysis notebook? You would just need to run the pre-processing notebook before the others—or even run it just once, ever, and then commit the resulting file.

---

### **Option 4. Save processing in its own python file with utility methods**

What if we have a notebook that explains all of our pre-processing steps, and then we stick that code inside a method in a .py file and call that in the notebook?

See the example below. On the left, we have the top of our explanatory notebook. On the upper right, inside <code>prepare_data.py</code>, I have copied the python lines of the notebook into a method called <code>.get_data_with_tfidf()</code>. On the lower right, I import that method into the notebook, call it, and assign the result to a dataframe.

<img src = "https://i2.wp.com/chelseatroy.com/wp-content/uploads/2018/09/Screen-Shot-2018-09-02-at-5.03.58-PM.png?resize=1024%2C497&ssl=1">

This strategy addresses some of the issues we talked about in *Option 1*. 
* First, a user cannot accidentally run the pre-processing steps out of order now because they happen in order in the method called from the client. 
* Second, we have lots of good ways to write automated tests for a method inside a python file. 
* Third, the approach scales to multiple notebooks because we can import and call this same method in as many notebooks as we want.

Our method approach also has a couple of limitations that we’d like to address for our use case:
1. It starts the extraction process from scratch every time we run it.
2. We do not get to make any choices about how our preprocessing is done for each analysis step.

---

Back at *Option 1*, we talked about how these data analysis projects go. Each analytical step requires some unique modifications to the data. It’s possible that we want to start with different pre-processing steps depending on the analysis we’re doing. With this method, though, we’re always getting the same steps—including tf-idf, which is computationally expensive.

We currently start out in notebooks to ask questions and document our answers. We then switch to scripts to finalize and reproduce our answers. When we do this, we shift our limitations. 

---

### Personal Notes
> Keep in mind that notebook for documenting our scientific process (Experimental purpose only), and then switch to python script for **finalize** and **reproduce** our process (Production purpose)

---

### References
- https://chelseatroy.com/2018/09/11/design-patterns-for-data-science-part-1-python-files-and-notebooks/