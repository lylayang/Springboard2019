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
_ = plt.hist(n_defaults, normed=True, bins=bins, histtype='step')
# Label axes
_ = plt.xlabel('number of defaults out of 100 loans')
_ = plt.ylabel('PMF')
plt.show()
```

## Possion process and the Possion distribution
Possion process: the timing of the next event is completely independent of when the previous event happened
Example of Possion process:
- Natural births in a given hospital
- Hit on a website during a given hour
- Aviation incidents
- Buses in Poissonville

### Possion distribution
- The number r of arrivals of a Poisson process in a given time interval with average rate of? arrivals per interval is Poisson distributed
- The number r of hits on a website in one hour with an average hit rate of 6 hits per hour is Poisson distributed
- Limit of the Binomial Distribution for low probability of success and large number of trials. That is, for rare events.
- the Poisson distribution is a limit of the Binomial distribution for rare events. 
- **Timing of one is independent of all others**

The possion CDF
```python
samples= np.random.possion(6, size=10000) # 'size' for multiple samples
x, y= ecdf(x, y, marker='.', linestyle='none')
plt.margins(0.02)
_=plt.xlabel('number of successes')
_=plt.ylabel('CDF')
plt.show()

# Draw 10,000 samples out of Poisson distribution: samples_poisson
samples_poisson = np.random.poisson(10, size=10000)

# Print the mean and standard deviation
print('Poisson:     ', np.mean(samples_poisson),
                       np.std(samples_poisson))

# Specify values of n and p to consider for Binomial: n, p
n = [20, 100, 1000]
p = [0.5, 0.1, 0.01]

# Draw 10,000 samples for each n,p pair: samples_binomial
for i in range(3):
    samples_binomial = np.random.binomial(n[i], p[i], size=10000)

    # Print results
    print('n =', n[i], 'Binom:', np.mean(samples_binomial),
                                 np.std(samples_binomial))
                                 
# Draw 10,000 samples out of Poisson distribution: n_nohitters
n_nohitters = np.random.poisson(251/115, size=10000)

# Compute number of samples that are seven or greater: n_large
n_large = np.sum(n_nohitters >= 7)

# Compute probability of getting seven or more: p_large
p_large = n_large / 10000

# Print the result
print('Probability of seven or more no-hitters:', p_large)

```

## Quantities (Continuous VS discrete): Continuous
PDF
- PDF(probability density function) ~ continuous analog to the PMF
- Mathematical description of the ralative likelihood of observing a value of a continuous variable

```python
# Generate CDFs
x_std1, y_std1 = ecdf(samples_std1)
x_std3, y_std3 = ecdf(samples_std3)
x_std10, y_std10 = ecdf(samples_std10)

# Plot CDFs
_ = plt.plot(x_std1, y_std1, marker='.', linestyle='none')
_ = plt.plot(x_std3, y_std3, marker='.', linestyle='none')
_ = plt.plot(x_std10, y_std10, marker='.', linestyle='none')

# Make a legend and show the plot
_ = plt.legend(('std = 1', 'std = 3', 'std = 10'), loc='lower right')
plt.show()
```
## The Normal distribution: Properties and warnings
- Plot histgrah
- Plot CDF
Check the overlay the theoretical Normal CDF on the ECDF of the data, see if its close or not.
**Light tails of the Normal distribution**, the probability of being more than four standard deviations from the mean is very small. This means that when you are modeling data as Normally distributed, outliers are extremely unlikely. But real data sets usually have extreme values.

#compare cdf of collected data set with normal distribution

```python
# Compute mean and standard deviation: mu, sigma
mu=belmont_no_outliers.mean()
sigma=belmont_no_outliers.std()

# Sample out of a normal distribution with this mu and sigma: samples
samples=np.random.normal(mu, sigma, size=10000)

# Get the CDF of the samples and of the data
x,y=ecdf(belmont_no_outliers)
x_theor, y_theor= ecdf(samples)

# Plot the CDFs and show the plot
_ = plt.plot(x_theor, y_theor)
_ = plt.plot(x, y, marker='.', linestyle='none')
_ = plt.xlabel('Belmont winning time (sec.)')
_ = plt.ylabel('CDF')
plt.show()
```
What are the chances of a horse matching or beating Secretariat's record?

Assume that the Belmont winners' times are Normally distributed (with the 1970 and 1973 years removed), what is the probability that the winner of a given Belmont Stakes will run it as fast or faster than Secretariat?

```python
# Take a million samples out of the Normal distribution: samples
samples = np.random.normal(mu, sigma, size=1000000)

# Compute the fraction that are faster than 144 seconds: prob
prob = np.sum(samples <= 144) / len(samples)

# Print the result
print('Probability of besting Secretariat:', prob)
```
## The Exponential distribution
We know that the number of buses that will arrive per hour are Possion Distributed
But the amount oftime between arrivals of buese is Exponentially distributed
- A single prameter: the mean waiting time
- The waiting time between arrivales of a Poisson process is Exponentially distributed 

Simulation: exponential inter-incident times
```python
mean = np.mean(inter_times)
samples = np.random.exponential(mean, size=10000)
x, y = ecdf(inter_times)
x_theor, y_theor = ecdf(samples)
_ = plt.plot(x_theor, y_theor)
_ = plt.plot(x, y, marker='.', linestyle='none')
_ = plt.xlabel('time (days)')
_ = plt.ylabel('CDF')
plt.show()
```
Q: Waiting for the next Secretariat
Unfortunately, Justin was not alive when Secretariat ran the Belmont in 1973. Do you think he will get to see a performance like that? To answer this, you are interested in how many years you would expect to wait until you see another performance like Secretariat's. How is the waiting time until the next performance as good or better than Secretariat's distributed? Choose the best answer.

Exponential: A horse as fast as Secretariat is a rare event, which can be modeled as a Poisson process, and the waiting time between arrivals of a Poisson process is Exponentially distributed.

```python
def successive_poisson(tau1, tau2, size=1):
    """Compute time for arrival of 2 successive Poisson processes."""
    # Draw samples out of first exponential distribution: t1
    t1 = np.random.exponential(tau1, size=size)

    # Draw samples out of second exponential distribution: t2
    t2 = np.random.exponential(tau2, size=size)

    return t1 + t2
 
# Draw samples of waiting times: waiting_times
waiting_times= successive_poisson(764, 715, size=100000)
# Make the histogram
_=plt.hist(waiting_times, normed=True, bins=100, histtype='step')

# Label axes
_=plt.xlabel('waiting times')
_=plt.ylabel('PDF')

# Show the plot
plt.show()
 ```
## Optimal Parameters
Useful python modules:
- scipy.stats
- statsmodels
- numpy

How is this parameter optimal?

```python
# Plot the theoretical CDFs
plt.plot(x_theor, y_theor)
plt.plot(x, y, marker='.', linestyle='none')
plt.margins(0.02)
plt.xlabel('Games between no-hitters')
plt.ylabel('CDF')

# Take samples with half tau: samples_half
samples_half = np.random.exponential(0.5*tau, size=10000)

# Take samples with double tau: samples_double
samples_double = np.random.exponential(2*tau, size=10000)

# Generate CDFs from these samples
x_half, y_half = ecdf(samples_half)
x_double, y_double = ecdf(samples_double)

# Plot these CDFs as lines
_ = plt.plot(x_half, y_half)
_ = plt.plot(x_double, y_double)

# Show the plot
plt.show()
```
Linear regression by least squres
The process of finding the parameters for which the sum of the squares of the residuals is minimal
Least squares with np.polyfit(), which performs least squares analysis with polynomial functions
Slope, intercept = np.polyfit(total_votes, dem_share, 1)
print(slope, intercept)

The function np.polyfit() that you used to get your regression parameters finds the optimal slope and intercept. It is optimizing the sum of the squares of the residuals, also known as RSS (for residual sum of squares).

```python
# Plot the illiteracy rate versus fertility
_ = plt.plot(illiteracy, fertility, marker='.', linestyle='none')
plt.margins(0.02)
_ = plt.xlabel('percent illiterate')
_ = plt.ylabel('fertility')

# Perform a linear regression using np.polyfit(): a, b
a, b = np.polyfit(illiteracy, fertility, 1)

# Print the results to the screen
print('slope =', a, 'children per woman / percent illiterate')
print('intercept =', b, 'children per woman')

# Make theoretical line to plot
x = np.array([0, 100])
y = a * x + b

# Add regression line to your plot
_ = plt.plot(x, y)

# Draw the plot
plt.show()
```

n this exercise, you will plot the function that is being optimized, the RSS, versus the slope parameter a. To do this, fix the intercept to be what you found in the optimization. Then, plot the RSS vs. the slope. Where is it minimal?

```python
# Specify slopes to consider: a_vals
a_vals = np.linspace(0, 0.1, 200)

# Initialize sum of square of residuals: rss
rss = np.empty_like(a_vals)

# Compute sum of square of residuals for each value of a_vals
for i, a in enumerate(a_vals):
    rss[i] = np.sum((fertility - a*illiteracy - b)**2)

# Plot the RSS
# 'RSS versus slope', RSS is the y-axis, and slope is the x-axis. 
plt.plot(a_vals, rss, '-')
plt.xlabel('slope (children per woman / percent illiterate)')
plt.ylabel('sum of square of residuals')

plt.show()
```

The importance of EDA: Anscombe's quartet
Same rss, same linear line but
￼
```python
# Perform linear regression: a, b
a, b = np.polyfit(x, y, 1)

# Print the slope and intercept
print(a, b)

# Generate theoretical x and y data: x_theor, y_theor
x_theor = np.array([3, 15])
y_theor = a * x_theor + b

# Plot the Anscombe data and theoretical line
_ = plt.plot(x, y, marker='.', linestyle='none')
_ = plt.plot(x_theor, y_theor, '-')

# Label the axes
plt.xlabel('x')
plt.ylabel('y')

# Show the plot
plt.show()
```

The data are stored in lists; anscombe_x = [x1, x2, x3, x4] and anscombe_y = [y1, y2, y3, y4], where, for example, x2 and y2 are the 
x
x
 and 
y
y
 values for the second Anscombe data set.
 
```python
# Iterate through x,y pairs
for x, y in zip(anscombe_x, anscombe_y):
    # Compute the slope and intercept: a, b
    a, b = np.polyfit(x, y,1)

    # Print the result
    print('slope:', a, 'intercept:', b)


Bootstrapping:
The use of resampled data to perform statistical inference

for _ in range(50):
    # Generate bootstrap sample: bs_sample
    bs_sample = np.random.choice(rainfall, size=len(rainfall))

    # Compute and plot ECDF from bootstrap sample
    x, y = ecdf(bs_sample)
    _ = plt.plot(x, y, marker='.', linestyle='none',
                 color='gray', alpha=0.1)

# Compute and plot ECDF from original data
x, y = ecdf(rainfall)
_ = plt.plot(x, y, marker='.')

# Make margins and label axes
plt.margins(0.02)
_ = plt.xlabel('yearly rainfall (mm)')
_ = plt.ylabel('ECDF')

# Show the plot
plt.show()
```
