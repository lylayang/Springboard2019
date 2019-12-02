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
- scatter plot and covariance(a measure of how two quantities vary together
![](https://raw.githubusercontent.com/lylayang/Springboard2019/master/L8%20%20Statistical%20Methods/data%20sets/covariance%20.png)
The covariance is the mean of the product of these differences. If x and y both tend ot be above or below their responsive means together, as they are in this dataset then the covariance is positive. This is means they are positively correlated. This means when county is more populus then more votes for Obama. Conversely, if x is high while y is low, the covariance is negative, and the data are negatively correlated, or anticorrelated which is not the case for this data set.

Easy to use - to be dimensionless, that is to not have any units.
- Person Correlation coefficient (from -1 to 1)
p=person correlation = covariance /(std of x)(std of y) = variability due to codependence/independant variability 

codependence, the covariance
independent variability, the standard deviations

```python
# Compute the covariance matrix: covariance_matrix
covariance_matrix = np.cov(versicolor_petal_length, versicolor_petal_width)
corr_mat=np.corrcoef(versicolor_petal_length, versicolor_petal_width)
correlation_coefficient=corr_mat[0,1]
```
6. Statistic inference - probabilistic thinking
Given a set of data, you describe probabilistically what you might expect if those data were acquired again and again and again. It's the process by which we go from measured data to probabilistic conclusions about what we might expect if we collected the same data again.

- simulation, resampling
The np.random module, suite of functions based on random number generation.
np.random.seed(), the same seed gives you same sequence of random numbers, hence the name, 'psudedorandom number generation'
Steps:
- Determine how to simulate data
- Simulate many many times
- Probability is approximately fracion of trials with the outcome of interest

Simulating 4 coin flips
```python
import numpy as np
np.random.seed(42)
random_numbers=np.random.random(size=4)
print(random_numbers)

heads=random_numbers <0.5
print(heads)

print(np.sum(heads))

#method 2: using for loop
"""
Calculate the probability of getting four heads if we were to repeat four flips over and over again
"""
n_all_heads=0 # initialize number of 4-heads trials
for _ in range(10000):
        heads=np.random.random(size=4) <0.5
        n_heads=np.sum(heads)
        if n_heads==4:
           n_all_heads +=1
print("probability of getting all four heads ", n_all_heads/10000)
```

```python
#simulation1
# Seed the random number generator
np.random.seed(42)
# Initialize random numbers: random_numbers
random_numbers = np.empty(100000)
# Generate random numbers by looping over range(100000)
for i in range(100000):
    random_numbers[i] = np.random.random()

# Plot a histogram
_ = plt.hist(random_numbers)

# Show the plot
plt.show()

#simulation 2
def perform_bernoulli_trials(n, p):
    """Perform n Bernoulli trials with success probability p
    and return number of successes."""
    # Initialize number of successes: n_success
    n_success = 0
    # Perform trials
    for i in range(n):
        # Choose random number between zero and one: random_number
        random_number=np.random.random()
        # If less than p, it's a success so add one to n_success
        if random_number < p:
            n_success +=1

    return n_success

#simulation3 (put simulatoin1, 2 together)
# Seed random number generator
np.random.seed(42)

# Initialize the number of defaults: n_defaults
n_defaults=np.empty(1000)

# Compute the number of defaults
for i in range(1000):
    n_defaults[i] = perform_bernoulli_trials(100,0.05) # per 100 loads, defaul rate is 0.05
    
_ = plt.hist(n_defaults, normed=True)
_ = plt.xlabel('number of defaults out of 100 loans')
_ = plt.ylabel('probability')

# Show the plot
plt.show()
```
**Exercise: Will the bank fail?** 
Plot the number of defaults you got from the previous exercise, in your namespace as n_defaults, as a CDF. The ecdf() function you wrote in the first chapter is available.

If interest rates are such that the bank will lose money if 10 or more of its loans are defaulted upon, what is the probability that the bank will lose money?


```python
# Compute ECDF: x, y
x, y = ecdf(n_defaults)

# Plot the CDF with labeled axes
_ = plt.plot(x, y, marker='.', linestyle='none')
_ = plt.xlabel('number of defaults out of 100')
_ = plt.ylabel('CDF')
plt.show()

# Compute the number of 100-loan simulations with 10 or more defaults: n_lose_money
n_lose_money=np.sum(n_defaults >=10)
# Compute and print probability of losing money
print('Probability of losing money =', n_lose_money / len(n_defaults))
```

7. PMF (Probability mass function) and CDF

sampling from the Binomial distribution
```python
#flip coins times and get 4 heads
np.random.binomial(4, 0.5, size=10)

# the binomial PMF
# we do 60 Bernoulli trials with a probability of success of 0.1
samples=np.random.binomial(60, 0.1, size=10000)
n=60
p=0.1

#the binomial CDF
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()
x, y= ecdf(samples)
_= plt.plot(x, y, marker='.', linestyle='none')
plt.margins(0.02)
_=plt.xlabel("number of successes")
_=plt.ylabel('CDF')
plt.show()
```


Plotting the Binomial PMF
As mentioned in the video, plotting a nice looking PMF requires a bit of matplotlib trickery that we will not go into here. Instead, we will plot the PMF of the Binomial distribution as a histogram with skills you have already learned. The trick is setting up the edges of the bins to pass to plt.hist() via the bins keyword argument. We want the bins centered on the integers. So, the edges of the bins should be -0.5, 0.5, 1.5, 2.5, ... up to max(n_defaults) + 1.5. You can generate an array like this using np.arange() and then subtracting 0.5 from the array.

You have already sampled out of the Binomial distribution during your exercises on loan defaults, and the resulting samples are in the NumPy array n_defaults.
```python
# Compute bin edges: bins
bins = np.arange(0, max(n_defaults) + 1.5) - 0.5
# Generate histogram
_ = plt.hist(n_defaults, normed=True, bins=bins)
# Label axes
_ = plt.xlabel('number of defaults out of 100 loans')
_ = plt.ylabel('PMF')
plt.show()
```

