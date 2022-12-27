# Introduction to statistics

## Intro

**Statistics**.- practice and study of collecting and alayzing data. 

### Branches of statistics
* **Descriptive/Summary Statistics:** *Describe* or *summarize* data;
**population**
* **Inferential Statistics:** Use a sample to *draw conclusions* about a 
population; **sample**

<br />

### Types of data

* **Numeric**:
    
    * Continious. - FLOAT; measured on a continious scale
    * Interval/count data. - INT; measured in whole numbers

* **Categorical**
    
    * Nominal Data. - describes unordered categories (ex. eye color)
    * Ordinal Data. - follow an order (ex. opinions: bad - good)

<br />
<br />

## Measures of center

Describe the central tendency; the typical central value in the distribution. 

| Measure | Formula | Description |
| --- | --- | --- |
| mean | $$\mu = \frac{\sum_{i=i}^{n} x_{i}}{n}$$ | gives the average of values, use for normal distribution *i.e.* when the data is symetrical |
| median | $$\frac{middle\;value(s)}{\#\;of\;middle\;values}$$ | middle value of the data (sorted), use for non-symetrical data or when there are outliers|
| mode | $$most repeated value$$| most frequent value|

<br />

## Measures of spread

Spread describes how far apart datapoints are. 

| Measure | Formula | Description | 
| --- | --- | --- | 
| range | $$max - min$$ | maximum and minimum values |
| variance| $$\sigma^2 = \frac{\displaystyle\sum_{i=1}^{n}(x_i - \mu)^2} {n}$$| average distrance from each datapoint to the mean in squared units (to get rid of negatives) |
| standard deviation | $$\sigma = \sqrt{\sigma²}$$ | way of reverting the square transformation of the variance, average distance between datapoints and mean |
| quartiles | $$Q_i$$ | splits the information into four equal parts | 
| interquartile range| $$Q_3 - Q_1$$| distance between the 1st and 3rd quartile; this measure is less affected by extreme values |