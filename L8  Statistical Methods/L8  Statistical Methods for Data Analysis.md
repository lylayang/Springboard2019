# L8 Statistical Methods for Data Analysis
Welcome to the second unit in the statistical methods for data analysis theme. In the last unit, you explored presenting visual descriptions of data using plots. In this unit, you’ll dive deeper into descriptive statistics before moving on to inferential statistics. When keeping these two kinds of stats straight, you can think of them this way: descriptive stats tells you (and your audience) something about the data, while inferential stats tells you what’s going on behind the data. Or, only a little whimsically, inferential stats is the art of jumping to conclusions with (un)certainty!

This unit explores three different types of statistical inference and examines their similarities and differences through hands-on work and tutorials to help you better understand which kind of inference would be most useful to you for any given project. These three types of inference are essentially different ways that you can draw conclusions about a large dataset, or population, from a sample of that data.

1. **Frequentist inference** regards probability as the expected frequency of a particular outcome when an experiment is run multiple times. You’ll generally use this type of inference when your data satisfies certain conditions and there is a theoretical result known for your statistic.



2. **Bootstrap inference** simulates such reruns of an experiment to observe the outcomes. This type of inference is particularly useful when theoretical results for your statistic don’t apply (or are difficult to calculate) such as when you want to calculate a confidence interval for a minimum or maximum value for a dataset.   

3. **Bayesian inference** treats probability as a reflection of your belief in a specific outcome. This type of inference can be particularly useful when you have limited data but do have prior estimates from domain expertise, or when you want to construct and validate probabilistic models. Bayesian inference can more naturally deal with smaller amounts of data.
You’ll find each type of inference useful in different scenarios. In this unit, you’ll learn what those scenarios are and how best to use these types of inference to understand data, make predictions, and more.

For each type of statistical inference, we've paired resources with a mini-project that is meant to both build on and introduce new concepts related to those covered in the resources. You may find yourself Googling topics discussed in the mini-projects; we encourage you to do this and have explicitly designed the mini-projects this way so that you form a habit of seeking out answers — a critical skill that every data scientist should have in their toolbox. 

Unit Plan (What you’ll learn, Words to know, What will help)

**Work to Submit:**

1. Frequentist Inference Mini-Projects A and B
2. Bootstrap Inference Mini-Project
3. Bayesian Inference Mini-Project 
4. Applying Stastical Inference Skills to Your Capstone Project
5. Report on the inferential statistics methods used on the capstone project and its results
 
 
 ## DataCamp Statistical Thinking in Python Part 1
 
 1. Binning bias
 The same data may be interpreted differently depending on choice of bins
 The remedy for this problem - **Bee swarm plot**
 sns.swarmplot(x=, y=, data=)
 
 2. Bee sawrm plot problem
  The edges have overlapping data points, which was necessary in order to fit all points on to the plot.
  
  The remedy for this - **Making an ECDF**, the empirical cumulative distribution functions.
  
  When the number of data are very large and bee swarm plots are too cluttered, boxplot is a greate alternative.
  
  3.ECDF
  Always plot ecdf first. It shows all the data and it gives a complete picture of how the data are distributed.
  ![alt text](https://raw.githubusercontent.com/lylayang/Springboard2019/master/L8%20%20Statistical%20Methods/data%20sets/ecdf.png)
  
  ```python
import numpy as np
  
#method 1
x=np.sort(df_swing['dem_share'])
y=np.arange(1, len(x)+1)/len(x)
_=plt.plot(x, y, marker='.', linestyle='none')
_=plt.xlabel('percent of vote for Obama')
_=plt.ylabel('ECDF')
plt.margins(0.02)#keeps data off plot edges
plt.show()
  
#method2
def ecdf(data):
    """Compute ECDF for a one-dimensional array of measurements."""
    # Number of data points: n
    n = len(data)

    # x-data for the ECDF: x
    x = np.sort(data)

    # y-data for the ECDF: y
    y = np.arange(1, n+1) / n

    return x, y
    
# Compute ECDF for versicolor data: x_vers, y_vers
x_vers, y_vers = ecdf(versicolor_petal_length)

# Generate plot
plt.plot(x_vers, y_vers, marker='.', linestyle='none')

# Label the axes
plt.ylabel('ECDF')


# Display the plot

plt.show()

#method3, plot all ECDFs together on the same plot
# Compute ECDFs
x_set, y_set = ecdf(setosa_petal_length)
x_vers, y_vers = ecdf(versicolor_petal_length)
x_virg, y_virg = ecdf(virginica_petal_length)

# Plot all ECDFs on the same plot
_ = plt.plot(x_set, y_set, marker='.', linestyle='none')
_ = plt.plot(x_vers, y_vers, marker='.', linestyle='none')
_ = plt.plot(x_virg, y_virg, marker='.', linestyle='none')

# Annotate the plot
_ = plt.legend(('setosa', 'versicolor', 'virginica'), loc='lower right')
_ = plt.xlabel('petal length (cm)')
_ = plt.ylabel('ECDF')

# Display the plot
plt.show()
  ```

4. Percentiles, outliers, and box plots
  median, the middle value of a data set is immune to extrame data points
  
  
![alt text](https://raw.githubusercontent.com/lylayang/Springboard2019/master/L8%20%20Statistical%20Methods/percentiles%20of%20edf.png)
  
![alt text](https://raw.githubusercontent.com/lylayang/Springboard2019/master/L8%20%20Statistical%20Methods/boxplot.png)
  
While there is no single definition for an outliers, being more than 2IQRs away from the median is a common criterion.

```python
# Specify array of percentiles: percentiles
percentiles=np.array([2.5, 25, 50, 75, 97.5])

# Compute percentiles: ptiles_vers
ptiles_vers=np.percentile(versicolor_petal_length, q=percentiles)

# Plot the ECDF
_ = plt.plot(x_vers, y_vers, '.')
_ = plt.xlabel('petal length (cm)')
_ = plt.ylabel('ECDF')

# Overlay percentiles as red diamonds.
_ = plt.plot(ptiles_vers, percentiles/100, marker='D', color='red',
         linestyle='none')

```

5. Covariance and the Pearson correlation coefficient
- scatter plot and covariance
![](https://raw.githubusercontent.com/lylayang/Springboard2019/master/L8%20%20Statistical%20Methods/data%20sets/covariance%20.png)



