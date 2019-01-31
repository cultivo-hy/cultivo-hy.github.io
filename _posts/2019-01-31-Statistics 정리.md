---
layout: post

title: "[Introduction to Statistics]"

tags:

- statistics
- distribution
- python

category:

- statistics
---

* toc
{:toc}

# 1.Explaining Standard Deviation
Have you ever had to explain to anyone what a standard deviation is? Or perhaps, you aren't sure yourself what it is. What is it used for? Why is it important in process improvement? When using control charts, the standard deviation, as well as the average, is a very important parameter. One must understand what is meant by the standard deviation. This newsletter addresses this. We will start with describing what an average is.

### AVERAGE

The average (also called the mean) is probably well understood by most. It represents a "typical" value. For example, the average temperature for the day based on the past is often given on weather reports. It represents a typical temperature for the time of year.

The average is calculated by adding up the results you have and dividing by the number of results. For example, suppose we have wire cable that is cut to different lengths for a customer. These lengths, in feet, are 5, 6, 2, 3, and 8. The average is determined by adding up these five numbers and dividing by 5. In this case, the average (X) is:

X = (5+6+2+3+8)/5 = 4.8

The average length of wire for these five pieces is 4.8 feet.


### STANDARD DEVIATION

While the average is understood by most, the standard deviation is understood by few. To begin to understand what a standard deviation is, consider the two histograms. Histogram 1 has more variation than Histogram 2.

![sample histogram 1](https://www.spcforexcel.com/files/images/morevar.gif) 

![sample histogram 2](https://www.spcforexcel.com/files/images/lessvar.gif)

In the first histogram, the largest value is 9, while the smallest value is 1. The overall range of data is 9 - 1 = 8. In the second histogram, the overall range is 7 - 3 = 4. The range is larger for Histogram 1. Ranges are often used in control charts for variation (for example, the X-R charts). In fact, the average range from a control chart can be used to calculate the process standard deviation.

The average of the data in each histogram is 5. So, in this case, the highest bar is the average. We can also see that Histogram 1 has more variation than Histogram 2 because the distance, on average, of the individual observations from the overall average (5) is greater in Histogram 1 than Histogram 2. This distance is usually referred to as a deviation. If your have a result X = 3, the deviation of this value from the average is 3 - 5 = - 2 or the value "3" is two units below the average.

One can view the standard deviation as being an "average" distance each individual measurement is from the average, X. Let's return to the numbers we found the average for earlier to see how we can estimate this average deviation from X. These numbers were the length of wire cable we had cut. We want to find the average distance each number is from X = 4.8. To do this, we can determine the deviation of each number from the average as shown below.

| **Length (X)** | **Deviation from Average (X- X)** |
| -------------- | --------------------------------- |
| 5              | 5 - 4.8 = 0.2                     |
| 6              | 6 - 4.8 = 1.2                     |
| 2              | 2 - 4.8 = -2.8                    |
| 3              | 3 - 4.8 = -1.8                    |
| 8              | 8 - 4.8 = 3.2                     |

 

If we add up the deviations from average, we discover that:

Sum of the deviations from average = 0.2 + 1.2 + (- 2.8) + (-1.8) + 3.2 = 0

The sum of the deviations from average added up to zero! This is not a coincidence. The fact is that when the deviations from the average are added up, the sum will always be zero if the average was calculated from the data. We cannot use this method to determine the standard deviation. The negative signs in the sum of deviations are what cause the sum to be zero. Suppose instead that we square each deviation from the average (i.e., multiply the deviation by itself). Squaring a negative number makes it positive. In this case the square of the **deviations are:**

| **Length (X)** | **Square of Deviation from Average (X-X)2** |
| -------------- | ------------------------------------------- |
| 5              | (5 - 4.8)2 = (0.2)2 = .04                   |
| 6              | (6 - 4.8)2 = (1.2)2 = 1.44                  |
| 2              | (2 - 4.8)2 = (-2.8)2 = 7.84                 |
| 3              | (3 - 4.8)2 = (-1.8)2 = 3.24                 |
| 8              | (8 - 4.8)2 = (3.2)2 = 10.24                 |

 



### MORE ON STANDARD DEVIATION

The sum of these squares of deviations from the average is 22.8. This number can now be used to determine the "average" distance each individual result is from X. The temptation here is to divide by n = 5 since there are five lengths. Unfortunately, this would be incorrect. The reason is that we would actually be underestimating the true standard deviation. This is primarily because we used the data to calculate an average (we don't know the true average of the process). This means that there are only n-1 independent pieces of information. If we know the average and four of the individual results, the fifth result can be determined. Thus, the correct number to divide by is n - 1 = 4.

Thus, the sum of the squares of the deviation from the average divided by 4 is 22.8/4 = 5.7. Remember, this number contains the squares of the deviations. To get to the standard deviation, we must take the square root of that number. Thus, the standard deviation is square root of 5.7 = 2.4.

The equation for a sample standard deviation we just calculated is shown in the figure. Control charts are used to estimate what the process standard deviation is. For example, the average range on the X-R chart can be used to estimate the standard deviation using the equation s = R/d2 where d2 is a control chart constant (see March 2005 newsletter).

![sigma equation](https://www.spcforexcel.com/files/images/sigmaequation.gif)



### USE OF STANDARD DEVIATION

![normal distribution diagram](https://www.spcforexcel.com/files/images/nd.gif)

So, why are the standard deviation and, for that matter, the average important? There are many types of control charts that are based on variables data. The basic probability distribution that governs the calculation of the control limits on these charts is the normal distribution. The normal distribution is the familiar bell shaped curve shown in the above figure.

The normal distribution has several interesting characteristics. The shape of the distribution is determined by the average, X, and the standard deviation, s. The highest point on the curve is the average. The distribution is symmetrical about the average. Most of the area under the curve (99.7%) lies between -3s and +3s of the average. In addition, about 95.44% of the curve is between -2s and +2s of the average, while 68.26% of the curve is between -1s and +1s of the average.

To determine if a normal distribution exists, you can make a histogram of the individual results. If this histogram is bell-shaped, you can assume that the individual measurements are normally distributed. For example, suppose you are monitoring how long it takes to approve or disapprove a credit application for a customer. From your control charts (assume the process is in control), you have estimated the process average to be 14 working days and the standard deviation to be 2 days. After constructing a histogram on the days to approve or disapprove a credit application, you discover that it is bell-shaped. You can now replace the histogram with the normal distribution shown in the figure below.

![normal distribution diagram 2](https://www.spcforexcel.com/files/images/ndnum.gif) 

Since the process is in statistical control, you know that about 67% of the time, it will take 12 to 16 days to process a credit application; 95% of the time it will take 10 to 18 days; and 99.7% of the time it will take 8 to 20 days. So, the average and standard deviation (for a normal distribution) allow you to begin to consider process capability. Can the process meet our customer specifications? Take a look at our three part newsletter on process capability.




---
# 2. Are the Skewness and Kurtosis Useful Statistics?



You have a set of samples.  Maybe you took 15 samples from a batch of finished product and measured those samples for density.  Now you are armed with data you can analyze.  And your software package has a feature that will generate the descriptive statistics for these data.  You enter the data into your software package and run the descriptive statistics.  You get a lot of numbers – the sample size, average, standard deviation, range, maximum, minimum and a host of other numbers.  You spy two numbers: the skewness and kurtosis.  What do these two statistics tell you about your sample?

This month's publication covers the skewness and kurtosis statistics.  These two statistics are called "shape" statistics, i.e., they describe the shape of the distribution. What do the skewness and kurtosis really represent? And do they help you understand your process any better? Are they useful statistics? Let’s take a look

You may download a pdf copy of this publication [at this link](https://www.spcforexcel.com/publications/are-skewness-and-kurtosis-useful-statistics/index.html).  You may also download an Excel workbook containing the impact of sample size on skewness and kurtosis at the end of this publication.  You may also leave a comment at the end of the publication.



### INTRODUCTION

Skewness and kurtosis are two commonly listed values when you run a software’s descriptive statistics function.  Many books say that these two statistics give you insights into the shape of the distribution. 

Skewness is a measure of the symmetry in a distribution.  A symmetrical dataset will have a skewness equal to 0.  So, a normal distribution will have a skewness of 0.   Skewness essentially measures the relative size of the two tails. 

![histogram](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/histogram%20nd.png)Kurtosis is a measure of the combined sizes of the two tails.  It measures the amount of probability in the tails.  The value is often compared to the kurtosis of the normal distribution, which is equal to 3.  If the kurtosis is greater than 3, then the dataset has heavier tails than a normal distribution (more in the tails).  If the kurtosis is less than 3, then the dataset has lighter tails than a normal distribution (less in the tails).  Careful here.  Kurtosis is sometimes reported as “excess kurtosis.”  Excess kurtosis is determined by subtracting 3 form the kurtosis.  This makes the normal distribution kurtosis equal 0.  Kurtosis originally was thought to measure the peakedness of a distribution.   Though you will still see this as part of the definition in many places, this is a misconception.

Skewness and kurtosis involve the tails of the distribution.  These are presented in more detail below.



### SKEWNESS

Skewness is usually described as a measure of a dataset’s symmetry – or lack of symmetry.   A perfectly symmetrical data set will have a skewness of 0.   The normal distribution has a skewness of 0. 

The skewness is defined as (Advanced Topics in Statistical Process Control, Dr. Donald Wheeler, [www.spcpress.com](http://www.spcpress.com/)):

![skewness equation](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/a3.png)

where n is the sample size, Xi is the ith X value, X is the average and s is the sample standard deviation.  Note the exponent in the summation.  It is “3”.  The skewness is referred to as the “third standardized central moment for the probability model.”

Most software packages use a formula for the skewness that takes into account sample size:

![skewness equation](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/skewness-above-below.png)

This sample size formula is used here.  It is also what Microsoft Excel uses.  The difference between the two formula results becomes very small as the sample size increases.

Figure 1 is a symmetrical data set.  It was created by generating a set of data from 65 to 135 in steps of 5 with the number of each value as shown in Figure 1.  For example, there are 3 65’s, 6 75’s, etc. 

**Figure 1: Symmetrical Dataset with Skewness = 0**

![symmetrical distribution](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/Figure-1.png)

A truly symmetrical data set has a skewness equal to 0.  It is easy to see why this is true from the skewness formula. Look at the term in the numerator after the summation sign.  Each individual X value is subtracted from the average.  So, if a set of data is truly symmetrical, for each point that is a distance “d” above the average, there will be a point that is a distance “-d” below the average. 

Consider the value of 65 and value of 135.  The average of the data in Figure 1 is 100.  So, the following is true when X = 65:

![negative skewness](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/negative-skewness-equation.png)

For X = 135, the following is true:

![positive skewness](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/positive%20skewness%20equation.png)

So, the -4278 and +4278 even out at 0.  So, a truly symmetrical data set will have a skewness of 0.

To explore positive and negative values of skewness, let’s define the following term:

![skewness above and below](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/sabove-sbelow.png)

So, Sabove can be viewed as the “size” of the deviations from average when Xi is above the average.    Likewise, Sbelow can be viewed as the “size” of the deviations from average when Xi is below the average.  Then, skewness becomes the following:

![skewness equation 2](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/skewness-above-below.png)

If Sabove is larger than Sbelow, then skewness will be positive.   This typically means that the right-hand tail will be longer than the left-hand tail.  Figure 2 is an example of this.  The skewness for this dataset is 0.514.  A positive skewness indicates that the size of the right-handed tail is larger than the left-handed tail.

**Figure 2: Dataset with Positive Skewness**

![distribution with positive skewness](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/Figure-2.png)

Figure 3 is an example of dataset with negative skewness.  It is the mirror image essentially of Figure 2.  The skewness is -0.514.   In this case, Sbelow is larger than Sabove.  The left-hand tail will typically be longer than the right-hand tail.

**Figure 3: Dataset with Negative Skewness**

**![distribution with negative skewness](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/Figure-3.png)**

So, when is the skewness too much?  The rule of thumb seems to be:

- If the skewness is between -0.5 and 0.5, the data are fairly symmetrical
- If the skewness is between -1 and – 0.5 or between 0.5 and 1, the data are moderately skewed
- If the skewness is less than -1 or greater than 1, the data are highly skewed



### KURTOSIS

How to define kurtosis?  This is really the reason this article was updated.  If you search for definitions of kurtosis, you will see some definitions that includes the word “peakedness” or other similar terms.  For example,

- “Kurtosis is the degree of peakedness of a distribution” – [Wolfram MathWorld](http://mathworld.wolfram.com/Kurtosis.html)
- “We use kurtosis as a measure of peakedness (or flatness)” – [Real Statistics Using Excel](http://www.real-statistics.com/descriptive-statistics/symmetry-skewness-kurtosis/)

You can find other definitions that include peakedness or flatness when you search the web.  The problem is these definitions are not correct.  Dr. Peter Westfall published an article that addresses why kurtosis does not measure peakedness ([link to article](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC4321753/)).  He said:

*“Kurtosis tells you virtually nothing about the shape of the peak – its only unambiguous interpretation is in terms of tail extremity.”*

Dr. Westfall includes numerous examples of why you cannot relate the peakedness of the distribution to the kurtosis.

Dr. Donald Wheeler also discussed this in his[ two-part series on skewness and kurtosis](http://www.qualitydigest.com/inside/quality-insider-article/problems-skewness-and-kurtosis-part-two.html).  He said:

![looking at ipad](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/man-woman-ipad.png) “Kurtosis was originally thought to be a measure the “peakedness” of a distribution.  However, since the central portion of the distribution is virtually ignored by this parameter, kurtosis cannot be said to measure peakedness directly.  While there is a correlation between peakedness and kurtosis, the relationship is an indirect and imperfect one at best.”

Dr. Wheeler defines kurtosis as:

*“The kurtosis parameter is a measure of the combined weight of the tails relative to the rest of the distribution.”*

So, kurtosis is all about the tails of the distribution – not the peakedness or flatness.  It measures the tail-heaviness of the distribution.

Kurtosis is defined as:

![kurtosis equation](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/a4.png)

where n is the sample size, Xi is the ith X value, X is the average and s is the sample standard deviation.  Note the exponent in the summation.  It is “4”.  The kurtosis is referred to as the “fourth standardized central moment for the probability model.”  

Here is where is gets a little tricky.  If you use the above equation, the kurtosis for a normal distribution is 3.  Most software packages (including Microsoft Excel) use the formula below.

![sample kurtosis equation](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/kurtosis-equation.png)

This formula does two things.  It takes into account the sample size and it subtracts 3 from the kurtosis.  With this equation, the kurtosis of a normal distribution is 0.  This is really the excess kurtosis, but most software packages refer to it as simply kurtosis.   The last equation is used here.  So, if a dataset has a positive kurtosis, it has more in the tails than the normal distribution.  If a dataset has a negative kurtosis, it has less in the tails than the normal distribution. 

Since the exponent in the above is 4, the term in the summation will always be positive – regardless of whether Xi is above or below the average.  Xi values close to the average contribute very little to the kurtosis.    The tail values of Xi contribute much more to the kurtosis.

Look back at Figures 2 and 3.  They are essentially mirror images of each other.  The skewness of these datasets is different: 0.514 and -0.514.  But the kurtosis is the same.  Both have a kurtosis of -0.527.  This is because kurtosis looks at the combined size of the tails. 

The kurtosis decreases as the tails become lighter.  It increases as the tails become heavier.  Figure 4 shows an extreme case.  In this dataset, each value occurs 10 times.  The values are 65 to 135 in increments of 5.  The kurtosis of this dataset is -1.21.  Since this value is less than 0, it is considered to be a “light-tailed” dataset.  It has as much data in each tail as it does in the peak.  Note that this is a symmetrical distribution, so the skewness is zero.

**Figure 4: Negative Kurtosis Example**

![distribution with negative kurtosis](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/Figure-4.png)

Figure 5 is shows a dataset with more weight in the tails.  The kurtosis of this dataset is 1.86.

**Figure 5: Positive Kurtosis Example**

![distribution with positive kurtosis](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/Figure-5.png)

Most often, kurtosis is measured against the normal distribution.  If the kurtosis is close to 0, then a normal distribution is often assumed.  These are called mesokurtic distributions.  If the kurtosis is less than zero, then the distribution is light tails and is called a platykurtic distribution.  If the kurtosis is greater than zero, then the distribution has heavier tails and is called a leptokurtic distribution.

The problem with both skewness and kurtosis is the impact of sample size.  This is described below.



### OUR POPULATION

Are the skewness and kurtosis any value to you?  You take a sample from your process and look at the calculated values for the skewness and kurtosis.  What can you tell from these two results?  To explore this, a data set of 5000 points was randomly generated. The goal was to have a mean of 100 and a standard deviation of 10. The random generation resulted in a data set with a mean of 99.95 and a standard deviation of 10.01. The histogram for these data is shown in Figure 6 and looks fairly bell-shaped.

**Figure 6: Population Histogram**

![population histogram](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/figure-6.png)

![data word](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/data.png)The skewness of the data is 0.007. The kurtosis is 0.03. Both values are close to 0 as you would expect for a normal distribution. These two numbers represent the "true" value for the skewness and kurtosis since they were calculated from all the data. In real life, you don't know the real skewness and kurtosis because you have to sample the process. This is where the problem begins for skewness and kurtosis. Sample size has a big impact on the results.



### IMPACT OF SAMPLE SIZE ON SKEWNESS AND KURTOSIS

The 5,000-point dataset above was used to explore what happens to skewness and kurtosis based on sample size. For example, suppose we wanted to determine the skewness and kurtosis for a sample size of 5.  5 results were randomly selected from the data set above and the two statistics calculated. This was repeated for the sample sizes shown in Table 1. 

**Table 1: Impact of Sample Size on Skewness and Kurtosis**

| **Sample Size** | **Skewness** | **Kurtosis** |
| --------------- | ------------ | ------------ |
| 5               | 1.983        | 3.974        |
| 10              | -0.078       | -1.468       |
| 15              | -0.384       | 0.127        |
| 25              | -0.356       | -0.025       |
| 50              | -0.169       | -0.752       |
| 75              | -0.489       | 0.615        |
| 100             | -0.346       | 0.671        |
| 250             | 0.089        | 0.061        |
| 500             | 0.186        | 0.232        |
| 750             | -0.02        | 0.042        |
| 1000            | -0.138       | 0.062        |
| 1250            | 0.085        | 0.079        |
| 1500            | -0.017       | 0.001        |
| 2000            | -0.059       | -0.009       |
| 2500            | 0.037        | 0.096        |
| 3000            | 0.009        | 0.005        |
| 3500            | -0.015       | 0.004        |
| 4000            | -0.015       | -0.009       |
| 4500            | 0.009        | 0.036        |
| 5000            | 0.007        | 0.03         |

 

Notice how much different the results are when the sample size is small compared to the "true" skewness and kurtosis for the 5,000 results. For a sample size of 25, the skewness was -.356 compared to the true value of 0.007 while the kurtosis was -0.025. Both signs are opposite of the true values which would lead to wrong conclusions about the shape of the distribution. There appears to be a lot of variation in the results based on sample size.

Figure 7 shows how the skewness changes with sample size.  Figure 8 is the same but for kurtosis. 

**Figure 7: Skewness versus Sample Size**

![skewness versus sample size](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/Figure-7.png)

**Figure 8: Kurtosis versus Sample Size**

![kurtosis versus sample size](https://www.spcforexcel.com/files/images/Skewness-Kurtosis-Figures/Figure-8.png)

30 is a common number of samples used for process capability studies. A subgroup size of 30 was randomly selected from the data set. This was repeated 100 times. The skewness varied from -1.327 to 1.275 while the kurtosis varied from -1.12 to 2.978. What kind of decisions can you make about the shape of the distribution when the skewness and kurtosis vary so much? Essentially, no decisions. 



### CONCLUSIONS

The skewness and kurtosis statistics appear to be very dependent on the sample size. The table above shows the variation. In fact, even several hundred data points didn't give very good estimates of the true kurtosis and skewness. Smaller sample sizes can give results that are very misleading. Dr Wheeler wrote in his book mentioned above:

*"In short, skewness and kurtosis are practically worthless. Shewhart made this observation in his first book. The statistics for skewness and kurtosis simply do not provide any useful information beyond that already given by the measures of location and dispersion."*

Walter Shewhart was the "Father" of SPC. So, don't put much emphasis on skewness and kurtosis values you may see. And remember, the more data you have, the better you can describe the shape of the distribution.  But, in general, it appears there is little reason to pay much attention to skewness and kurtosis statistics.  Just look at the histogram.  It often gives you all the information you need.

To download the workbook containing the macro and results that generated the above tables, please [click here](https://www.spcforexcel.com/files/Data-for-Kurtosis-and-Skewness.xls).



===
# 3.Normal distribution

### INTRODUCTION TO THE NORMAL DISTRIBUTION

If you search for "normal distribution" on Google, you will get a lot of hits. Wikipedia, the free encyclopedia, starts out its normal distribution with:

> "In probability theory and statistics, the normal distribution or Gaussian distribution is a continuous probability distribution that describes data that clusters around a mean or average. The graph of the associated probability density function is bell-shaped, with a peak at the mean, and is known as the Gaussian function or bell curve."

So, what does this mean to us and how do we use normal distributions? This month's newsletter examines the normal distribution.

The normal distribution is shown below. It is shaped like a bell

![normal distribution](https://www.spcforexcel.com/files/images/nd.png)

 

The normal distribution has several interesting characteristics:

- The shape of the distribution is determined by the average, μ (or X), and the standard deviation, σ.
- The highest point on the curve is the average.
- The distribution is symmetrical about the average.
- As you move away from the average, the points occur with less frequency.
- Most of the area under the curve (99.7%) lies between -3σ and +3σ of the average.

### 

### PROBABILITY DENSITY FUNCTION

The probability density function for the normal distribution is given by:

![normal distribution probabilty function](https://www.spcforexcel.com/files/images/ndprobdensityfunction.png)

where x is some value between -∞ to +∞, μ is the average and σ is the standard deviation. The standard deviation is an indication of how wide the normal distribution is. The average gives the location of the normal distribution.

The distributions below show how the normal distribution changes as the standard deviation changes. The average is 100 and there are three different distributions with standard deviations of 5, 10, and 20. Note that the larger the standard deviation, the wider the distribution. When you are making a control chart, the range chart is actually monitoring the "width" of the distribution. The range chart answers the following question: is the spread in my data staying the same over time (in control) or is the spread getting smaller or larger (out of control)?

 

![three normal distributions with different standard deviations](https://www.spcforexcel.com/files/images/threenormaldistributions.png)

 

The distributions above were generated using Excel's NORMSDIST function using an average of 100, one of the three standard deviations above and an X values range from 20 to 180. So, if you know your process average and process standard deviation, you can easily draw the normal distribution for your process. This, of course, assumes that your process is normally distributed.

### 

### STANDARD NORMAL DISTRIBUTION

For an average of 0 and a standard deviation of 1, the formula above becomes:

![standard normal function](https://www.spcforexcel.com/files/images/standardnormalfunction.png)

This is known as the standard normal distribution. For this distribution, the area under the curve from -∞ to +∞ is equal to 1.0. In addition, the area under the curve is proportional to the fraction of measurements that fall in that region. These two facts will be used to help determine the fraction of measurements that fall above some value (such as a specification limit), below some value, or between two values. The standard normal distribution is shown below.

 

![standard normal distribution](https://www.spcforexcel.com/files/images/standardnormaldistribution.png)

 

### 

### HOW TO USE THE NORMAL DISTRIBUTION

Suppose you are interested in a certain quality characteristic, X. You have been monitoring this characteristic using an X-R chart. Both the X chart and the range chart are in statistical control. This means that you can predict what your process will make in the near future. You also know that you have good estimates of the process average (from the X chart) and the process standard deviation (from the range chart). Suppose that X = 100 and that σ = 10.

In addition, you have constructed a histogram for the last month's data. The histogram is shown below.

 

![histogram of data](https://www.spcforexcel.com/files/images/histogram_nd.png)

 

This histogram appears to be bell-shaped so you assume that you are dealing with a normal distribution. You can then draw the normal distribution for this process because you know the average and standard deviation (from your control charts). The normal distribution for this process is shown below.

 

![normal distribution with percentages](https://www.spcforexcel.com/files/images/nd_with_percentages.png)

 

A normal distribution has the following properties:

- 68% of the data is within +/- 1 standard deviation of the average
- 95% of the data is within +/- 2 standard deviations of the average
- 99.7% of the data is within +/- 3 standard deviations of the average

For your process, the following calculations can be done:

![normal distribution calculations: adding sigma](https://www.spcforexcel.com/files/images/ndcalculations.png) 

Thus, 68% of the data lies between 90 and 110; 95% of the data between 80 and 120; and 99.7% of the data between 70 and 130. The specifications for the process have been 65 to 140. Life is good - everything has been within specifications.

### 

### NORMAL DISTRIBUTION AND SPECIFICATIONS

Now suppose a customer has decided that the upper specification limit for your process should be 112. You can easily see from the histogram and the normal distribution that some of your product will be out of specification. The question is how much will be out of specification. This is where the z value becomes important. z is defined by the equation:

![z value formula](https://www.spcforexcel.com/files/images/zvalueformula.png)

For our example, x = 112, so

z = (112-100)/10 = 12/10 = 1.2

z represents the number of standard deviations some value is away from the average. So, 112 is 1.2 standard deviations above the average. If z is negative, it means that the value is below the average.

To find out how much product is more than 1.2 standard deviations above the average, you can use what is called the "z table." The z table gives the fraction of process output that is beyond some value x that is z standard deviations from the average. The z table is given below.

**z Table: Standard Normal Distribution**

![z table](https://www.spcforexcel.com/files/images/ztables.png)

 

 

The table above returns a value of .1151 for a value of z = 1.2. This means that 11.51% of the data will be above the upper specification limit of 112. This is shown in the figure below. The area in red represents material that is out of specifications on the high side.

![area above usl](https://www.spcforexcel.com/files/images/percentabove.png)

You can also use the NORMSDIST function in Excel to find the above result. This function in Excel returns the fraction of results less than a value. So, you must use 1 - NORMSDIST(z) if you want the fraction of data above z. In this example: 1- NORMSDIST(1.2) = .11507

To make matters worse, the customer has now set the lower specification as 83. You want to find out what percent of the product will be out of specification on the low side. The process is the same as above. In this case:

​								z = (83 - 100)/10 = -17/10 = -1.7

This means that 83 is 1.7 standard deviations below the average. From the z tables, the fraction of data below 1.7 is 0.0446 or 4.46%. In Excel, NORMSDIST(-1.7) = 0.044565. The figure below shows this situation. The area in red represents the product that is below the lower specification limit.

![percent below lsl](https://www.spcforexcel.com/files/images/percentbelow.png)

To find out the area between two values, you use the fact that the area under the standard normal curve is 1. For example, suppose you want to find out the area between 83 and 112. We know that the area below 83 is .0446 and the area above 112 is .1151. Then the area between 83 and 112 is given by:

​								1 - .0446 - .1151 = 0.8403

Thus, 84.03% of the data lies between 83 and 112. Thus, with the new specifications, 84% of your product will be within specification. The remaining 16% will be out specification. And since the process is in statistical control, this will continue to be true until the process is changed fundamentally.
