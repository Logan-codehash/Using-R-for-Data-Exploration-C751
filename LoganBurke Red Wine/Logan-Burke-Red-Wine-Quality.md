---
title: "Red Wine Quality Analysis"
author: "Logan Burke"
date: "May 2021"
output:
  html_document:
    fig_caption: TRUE
    keep_md: TRUE
    theme: readable
    toc: TRUE
---







# Introduction
This report will be using R and exploratory data analysis techniques to look at a dataset about red wine quality. The dataset that will be explored in this analysis is "Modeling wine preferences by data mining from physicochemical properties". The reference information can be found in the References section at the end of this report. 

The dataset contains several physicochemical attributes from samples of red wine of the Portuguese "Vinho Verde" and has sensory classifications made by wine experts.

# Univariate  overview and plots of the data
Taking a look at the data, this is what we find:


```
## 'data.frame':	1599 obs. of  12 variables:
##  $ fixed.acidity       : num  7.4 7.8 7.8 11.2 7.4 7.4 7.9 7.3 7.8 7.5 ...
##  $ volatile.acidity    : num  0.7 0.88 0.76 0.28 0.7 0.66 0.6 0.65 0.58 0.5 ...
##  $ citric.acid         : num  0 0 0.04 0.56 0 0 0.06 0 0.02 0.36 ...
##  $ residual.sugar      : num  1.9 2.6 2.3 1.9 1.9 1.8 1.6 1.2 2 6.1 ...
##  $ chlorides           : num  0.076 0.098 0.092 0.075 0.076 0.075 0.069 0.065 0.073 0.071 ...
##  $ free.sulfur.dioxide : num  11 25 15 17 11 13 15 15 9 17 ...
##  $ total.sulfur.dioxide: num  34 67 54 60 34 40 59 21 18 102 ...
##  $ density             : num  0.998 0.997 0.997 0.998 0.998 ...
##  $ pH                  : num  3.51 3.2 3.26 3.16 3.51 3.51 3.3 3.39 3.36 3.35 ...
##  $ sulphates           : num  0.56 0.68 0.65 0.58 0.56 0.56 0.46 0.47 0.57 0.8 ...
##  $ alcohol             : num  9.4 9.8 9.8 9.8 9.4 9.4 9.4 10 9.5 10.5 ...
##  $ quality             : Ord.factor w/ 6 levels "3"<"4"<"5"<"6"<..: 3 3 3 4 3 3 3 5 5 3 ...
```

There are 12 variables and 1599 observations as we can see in the data. The variables are all of the numeric type, with the Quality being explicitly of the integer type.

The variables are based on the physicochemical tests, and are as follows along with explanations of what they entail:

- Fixed acidity: acid fixed or nonvolatile (does not evaporate readily). Measured as tartaric acid grams per decimeters cubed.

- Volatile acidity: the amount of acetic acid in wine, which at too high of levels can lead to an unpleasant, vinegar taste. Measured as acetic acid grams per decimeters cubed.

- Citric acid: in small quantities, citric acid can add 'freshness' and flavor to wines. Measured as grams per decimeters cubed.

- Residual sugar: the amount of sugar remaining after fermentation stops, it is rare to find wines with less than 1 gram per liter and wines with greater than 45 grams per liter are considered sweet. Measured as grams per decimeters cubed.

- Chlorides: the amount of salt in the wine. Measured as grams per decimeters cubed.

- Free sulfur dioxide: the free form of SO2 exists in equilibrium between molecular SO2 (as a dissolved gas) and bisulfite ion; it prevents microbial growth and the oxidation of wine. Measured as milligrams per decimeters cubed.

- Total sulfur dioxide: amount of free and bound forms of S02; in low concentrations, SO2 is mostly undetectable in wine, but as free SO2 concentrations exeed 50 ppm, SO2 becomes evident in the nose and taste of wine. Measured as milligrams per decimeters cubed.

- Density: the density of wine is close to that of water depending on the percent of alcohol and sugar content. Measured as grams per centimeter cubed. 

- pH: describes how acidic or basic a wine is on a scale from 0 (very acidic) to 14 (very basic); most wines are between 3-4 on the pH scale. 

- Sulphates: a wine additive which can contribute to sulfur dioxide gas (S02) levels, which acts as an antimicrobial and antioxidant. Measured as potassium sulfate in grams per decimeters cubed.

- Alcohol: the alcoholic percentage content of the wine. Measured as percentage by volume.

- Quality: This is the score assigned by wine experts; Using a score between 0 and 10, with 0 being poor wine quality and 10 being exceptional wine quality. 

A summary of the data shows its variability as shown:


```
##  fixed.acidity   volatile.acidity  citric.acid    residual.sugar  
##  Min.   : 4.60   Min.   :0.1200   Min.   :0.000   Min.   : 0.900  
##  1st Qu.: 7.10   1st Qu.:0.3900   1st Qu.:0.090   1st Qu.: 1.900  
##  Median : 7.90   Median :0.5200   Median :0.260   Median : 2.200  
##  Mean   : 8.32   Mean   :0.5278   Mean   :0.271   Mean   : 2.539  
##  3rd Qu.: 9.20   3rd Qu.:0.6400   3rd Qu.:0.420   3rd Qu.: 2.600  
##  Max.   :15.90   Max.   :1.5800   Max.   :1.000   Max.   :15.500  
##    chlorides       free.sulfur.dioxide total.sulfur.dioxide    density      
##  Min.   :0.01200   Min.   : 1.00       Min.   :  6.00       Min.   :0.9901  
##  1st Qu.:0.07000   1st Qu.: 7.00       1st Qu.: 22.00       1st Qu.:0.9956  
##  Median :0.07900   Median :14.00       Median : 38.00       Median :0.9968  
##  Mean   :0.08747   Mean   :15.87       Mean   : 46.47       Mean   :0.9967  
##  3rd Qu.:0.09000   3rd Qu.:21.00       3rd Qu.: 62.00       3rd Qu.:0.9978  
##  Max.   :0.61100   Max.   :72.00       Max.   :289.00       Max.   :1.0037  
##        pH          sulphates         alcohol      quality
##  Min.   :2.740   Min.   :0.3300   Min.   : 8.40   3: 10  
##  1st Qu.:3.210   1st Qu.:0.5500   1st Qu.: 9.50   4: 53  
##  Median :3.310   Median :0.6200   Median :10.20   5:681  
##  Mean   :3.311   Mean   :0.6581   Mean   :10.42   6:638  
##  3rd Qu.:3.400   3rd Qu.:0.7300   3rd Qu.:11.10   7:199  
##  Max.   :4.010   Max.   :2.0000   Max.   :14.90   8: 18
```

A visualization of the variability of each variable by plotting each variable using a boxplot will provide a baseline:


```
## Using quality as id variables
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

The upper and lower box extend to the highest and lowest points within 1.5 times the interquartile range.

Now taking a look at each individual variable to explore it more closely. 

## Fixed Acidity


```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    4.60    7.10    7.90    8.32    9.20   15.90
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/fixed.acidity-1.png)<!-- -->

The median fixed acidity for the wines in the dataset is 7.90 g/dm^3. Most of the wines tested have an acidity between 7.10 and 9.20. The distribution of fixed acidity is slightly right skewed and there are some outliers in the higher range of roughly above 15 g/dm^3.

## Volatile acidity


```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.1200  0.3900  0.5200  0.5278  0.6400  1.5800
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/volatile.acidity-1.png)<!-- -->

The distribution for volatile acidity is roughly non-symmetric with two peaks at 0.4 and 0.6. The median value is 0.52. Most of the observations fall in the range 0.39 - 0.64 and outliers are on the higher end of the range roughly above the 1.3 g/dm^3.

## Citric acid


```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   0.000   0.090   0.260   0.271   0.420   1.000
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/citric.acid-1.png)<!-- -->

Most wines tested have 0 g/dm^3 of citric acid, this acid is usually only found in very small concentrations in wine it seems. There are two additional peaks at 0.250 and 0.500 which may hint at some bimodal behavior in the dataset. An outlier appears with 1 g/dm^3 of citric acid.

## Residual sugar


```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   0.900   1.900   2.200   2.539   2.600  15.500
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/residual.sugar-1.png)<!-- -->![](Logan-Burke-Red-Wine-Quality_files/figure-html/residual.sugar-2.png)<!-- -->

The distribution of residual sugar has a median value of 2.2 g/dm^3. The distribution is right skewed with a long tail on the right side. There are several observations that appear to possibly be outliers to the far right. A second plot with them removed is shown as well for clarity. 

## Chlorides


```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
## 0.01200 0.07000 0.07900 0.08747 0.09000 0.61100
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/chlorides-1.png)<!-- -->![](Logan-Burke-Red-Wine-Quality_files/figure-html/chlorides-2.png)<!-- -->

The distribution of chlorides in the wine samples has a median value of 0.079 g/dm^3. It looks like there are outliers to the right along its tail, with its max at 0.611 g/dm^3. A second plot with them removed is shown.

## Free sulfur dioxide


```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    1.00    7.00   14.00   15.87   21.00   72.00
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/free.sulfur.dioxide-1.png)<!-- -->

The distribution of free sulfur dioxide is shown, and is right skewed, with a maximum of 72. There appear to be some outliers as there are no observations between 57 and 66. The median value is 14 mg/dm^3 of free sulfur dioxide. 

## Total sulfur dioxide


```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    6.00   22.00   38.00   46.47   62.00  289.00
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/total.sulfur.dioxide-1.png)<!-- -->

The distribution of total sulfur dioxide is right skewed with a median value of 38 mg/dm^3. There appears to be some outliers, as there are no observations between roughly 165 and 278, and then only two observations with a concentration greater than that recorded.

## Density


```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.9901  0.9956  0.9968  0.9967  0.9978  1.0037
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/density-1.png)<!-- -->

The density of the observations varies only a little, with most of the values being between 0.9956 and 0.9967. This would make sense, as wine has a density close to that of water. The distributions median value is 0.9968 g/cm^3.

## pH


```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   2.740   3.210   3.310   3.311   3.400   4.010
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/pH-1.png)<!-- -->

All wines typically have a low pH level. Acids are produced through the fermentation process. The median value is 3.31, and most wines have a pH between 3.21 and 3.4.

## Sulphates


```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.3300  0.5500  0.6200  0.6581  0.7300  2.0000
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/sulphates-1.png)<!-- -->

The distribution of sulphates is slightly right skewed. it appears that there are some outliers on the right tail, as they have roughly more than 1.5 g/dm^3 of sulphates observed.
The median value of the sulphates is 0.62 and most of the wines have a concentration between 0.55 and 0.73.

## Alcohol


```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    8.40    9.50   10.20   10.42   11.10   14.90
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/alcohol-1.png)<!-- -->

The alcohol concentration distribution is right skewed a little. The highest peak of the distribution is at 9.5 percent alcohol and the median value is 10.20 percent. The maximum amount of alcohol present in the observations is 14.90 percent by volume.

## Quality

```
##   3   4   5   6   7   8 
##  10  53 681 638 199  18
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/quality-1.png)<!-- -->

It appears that the distribution of wine quality appears to be normal with many wines at an average quality rating of 5 or 6. There are no wines with a quality lower than 3 and no wines higher than a quality rating of 8.

# Univariate Analysis

**What is the structure of your dataset?**

The dataset has 12 variables regarding 1599 observations. Each observation corresponds to a red wine sample of the Portuguese "Vinho Verde". Of the variables, 11 correspond to the results of a physicochemical test and one variable (`quality`) corresponds to the result of a sensory panel rating by wine experts.

**What is/are the main feature(s) of interest in your dataset?**

The main feature of interest in the dataset is the quality rating of each sample.

**What other features in the dataset do you think will help support your investigation into your feature(s) of interest?**

The physicochemical test results may help support the investigation into the dataset. All of them are related to characteristics which may affect the flavor profile of the wine. They correspond to concentrations of molecules which may have an overall impact on taste, and by extention, the quality rating of the wine. Density is a physical property which will depend on the percentage of alcohol and sugar content, which will also affect taste of the wine.

Some variables may have a stronger correlation with each other. For instance, the pH will depend on the amount of acid concentration, while total sulfur dioxide may have a similar distribution to that of free sulfur dioxide levels.


**Did you create any new variables from existing variables in the dataset?**

No new variables were created in the dataset for this analysis.

**Of the features you investigated, were there any unusual distributions? Did you perform any operations on the data to tidy, adjust, or change the form of the data? If so, why did you do this?**

There were no unusual distributions. There were also no missing values and no need to adjust the data. It was already a tidy dataset.


# Bivariate exploration

## Fixed Acidity vs. Quality


```
## [1] "Median of fixed.acidity by quality:"
## wine$quality: 3
## [1] 7.5
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 7.5
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 7.8
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 7.9
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 8.8
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 8.25
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/fixed_acid_vs_quality-1.png)<!-- -->

There is a slight trend of a higher quality rating when there is a higher fixed acidity concentration. However, there are less observations at the quality ratings of 3 and 8 compared to middle observations, which may make the median value not very accurate. And there is a drop of fixed acidity from 7 to 8 in the quality ratings as well. Additionally, there is a big dispersion of acidity values across each quality scale. More exploration is needed, as this might indicate that the quality ratings are the combination of several other variables. 

## Volatile Acidity vs. Quality


```
## [1] "Median of volatile.acidity by quality:"
## wine$quality: 3
## [1] 0.845
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 0.67
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 0.58
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 0.49
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 0.37
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 0.37
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/volt_acid_vs_quality-1.png)<!-- -->

Observing the same limitations that were seen in the Fixed Acidity plot; namely that at the extremes there are less observations and the variability within each quality rating, there appear to be a more obvious trend here. Lower volatile acidity seems to mean higher wine quality.

## Citric Acid vs. Quality


```
## [1] "Median of citric.acid by quality:"
## wine$quality: 3
## [1] 0.035
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 0.09
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 0.23
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 0.26
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 0.4
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 0.42
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/citric_acid_vs_quality-1.png)<!-- -->

Higher levels of citric acid seems to mean a higher quality rating for the wine. In the univariate plot for Citric Acid, it was observed that the distribution peaked at or near the zero value.

Which proportion of observations have zero citric acid in them? That proportion is:


```
## [1] 0.08255159
```

For each quality rating, the proportions are:


```
## # A tibble: 6 x 2
##   quality n_zero
##   <ord>    <dbl>
## 1 3       0.3   
## 2 4       0.189 
## 3 5       0.0837
## 4 6       0.0846
## 5 7       0.0402
## 6 8       0
```

There is a decreasing proportion of observations with zero levels of citric acid in the wine as the quality rating goes higher. This seems to validate the trend that the higher the citric acid levels are, then the higher the related wine quality rating.

## Residual Sugar vs. Quality


```
## [1] "Median of residual.sugar by quality:"
## wine$quality: 3
## [1] 2.1
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 2.1
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 2.2
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 2.2
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 2.3
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 2.1
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/sugar_vs_quality-1.png)<!-- -->

```
## [1] "Median of residual.sugar by quality:"
## wine$quality: 3
## [1] 2.1
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 2.1
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 2.2
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 2.2
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 2.3
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 2.1
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/sugar_vs_quality-2.png)<!-- -->

Residual sugar seems to have a low impact on the quality rating of the wines.

## Chlorides vs. Quality


```
## [1] "Median of chlorides by quality:"
## wine$quality: 3
## [1] 0.0905
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 0.08
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 0.081
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 0.078
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 0.073
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 0.0705
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/chlor_vs_quality-1.png)<!-- -->

```
## [1] "Median of chlorides by quality:"
## wine$quality: 3
## [1] 0.0905
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 0.08
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 0.081
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 0.078
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 0.073
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 0.0705
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/chlor_vs_quality-2.png)<!-- -->

Possibly a slight relation. Seems that less chlorides could mean a higher quality wine rating.

## Free sulfur dioxide vs. Quality


```
## [1] "Median of free.sulfur.dioxide by quality:"
## wine$quality: 3
## [1] 6
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 11
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 15
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 14
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 11
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 7.5
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/free_sulf_vs_quality-1.png)<!-- -->

The middle quality ratings seem to have a higher concentration of free sulfur dioxide than both the low and high quality ratings.

According to the information that was provided with the dataset, when free SO2 is lower than 50 ppm (~ 50 mg/L), it is undetectable to humans. In the following plot there are very few wines that are above this level which suggests that the variations seen in this plot are not related to an effect of the free SO2, but to the unbalanced distribution of wines across the quality ratings.

![](Logan-Burke-Red-Wine-Quality_files/figure-html/free_sulf_vs_quality_other-1.png)<!-- -->

## Total sulfur dioxide  vs. Quality


```
## [1] "Median of total.sulfur.dioxide by quality:"
## wine$quality: 3
## [1] 15
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 26
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 47
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 35
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 27
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 21.5
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/total_sulf_dio_vs_quality-1.png)<!-- -->

Similar to free sulfur dioxide concentrations. The middle ratings have higher concentrations than both the lower and higher quality ratings.

## Density vs. Quality


```
## [1] "Median of density by quality:"
## wine$quality: 3
## [1] 0.997565
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 0.9965
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 0.997
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 0.99656
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 0.99577
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 0.99494
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/density_vs_quality-1.png)<!-- -->

Lower density seems to mean a higher quality rating. From the information provided with the dataset, it was stated that the density will depend on the percentage of alcohol and sugar content in the wine. This relationship will be checked later on, but seems like there could be a relation. 

## pH vs. Quality


```
## [1] "Median of pH by quality:"
## wine$quality: 3
## [1] 3.39
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 3.37
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 3.3
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 3.32
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 3.28
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 3.23
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/pH_vs_quality-1.png)<!-- -->

There seems to be a trend of a higher quality rating when there are lower pH levels. This could mean that a higher acid concentration in the wine will correlate to a higher quality of wine. This relationship will be checked later on.

## Sulphates vs. Quality


```
## [1] "Median of sulphates by quality:"
## wine$quality: 3
## [1] 0.545
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 0.56
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 0.58
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 0.64
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 0.74
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 0.74
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/sulph_vs_quality-1.png)<!-- -->

Now taking a more zoomed in look at the trend.


```
## [1] "Median of sulphates by quality:"
## wine$quality: 3
## [1] 0.545
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 0.56
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 0.58
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 0.64
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 0.74
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 0.74
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/sulph_vs_quality_zoomed-1.png)<!-- -->

Higher sulphate concentration seems that it could mean a higher quality rating for the wine, as it does seem to have a relationship in the graph.

## Alcohol vs. Quality


```
## [1] "Median of alcohol by quality:"
## wine$quality: 3
## [1] 9.925
## ------------------------------------------------------------ 
## wine$quality: 4
## [1] 10
## ------------------------------------------------------------ 
## wine$quality: 5
## [1] 9.7
## ------------------------------------------------------------ 
## wine$quality: 6
## [1] 10.5
## ------------------------------------------------------------ 
## wine$quality: 7
## [1] 11.5
## ------------------------------------------------------------ 
## wine$quality: 8
## [1] 12.15
```

![](Logan-Burke-Red-Wine-Quality_files/figure-html/alcohol_vs_quality-1.png)<!-- -->

Besides the small downward blip in the quality at the 5 rating level, the higher the alcohol content, the higher rating the wine seems to be given.

## Acidity and pH

![](Logan-Burke-Red-Wine-Quality_files/figure-html/acid_vs_pH-1.png)<!-- -->

As the concentration of the acid decreases, the pH will increase. It is an inverse relationship, as expected for how pH is measured. Fixed acidity accounts for most acids present in the wine samples, so this trend would make sense.

![](Logan-Burke-Red-Wine-Quality_files/figure-html/citric_acid_vs_pH-1.png)<!-- -->

A similar relationship is seen with citric acid. But the citric acid is at lower concentrations, the relationship is not as prominent.

![](Logan-Burke-Red-Wine-Quality_files/figure-html/volatile_acid_vs_pH-1.png)<!-- -->

The volatile acidity seems to have little to no effect on the pH level.

The correlation coefficient for this is:


```
## 
## 	Pearson's product-moment correlation
## 
## data:  pH and log10(volatile.acidity)
## t = 9.1468, df = 1597, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.1760195 0.2691923
## sample estimates:
##       cor 
## 0.2231154
```

The correlation coefficient shows a weak positive correlation of how the volatile acidity impacts the pH. Maybe when the volatile acids are present in higher concentrations, the concentration of the remaining acids is lower and that contributes to the increased level of the pH.

![](Logan-Burke-Red-Wine-Quality_files/figure-html/fix_acid_vs_vol_acid-1.png)<!-- -->

```
## 
## 	Pearson's product-moment correlation
## 
## data:  fixed.acidity and volatile.acidity
## t = -10.589, df = 1597, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  -0.3013681 -0.2097433
## sample estimates:
##        cor 
## -0.2561309
```

There is a weak negative correlation here. On the plot, both variables seem to have a natural limit on the lower sides of the data. We have seen on the univariate plots that both are right skewed as well.

## Density, Sugar and Alcohol Content

The density of wine should be close to that of the density of water, and will change depending on the percent of alcohol and sugar content as was explained in the initial information given with the dataset.

![](Logan-Burke-Red-Wine-Quality_files/figure-html/den_vs_res_sug-1.png)<!-- -->

And here is a zoomed in look:

![](Logan-Burke-Red-Wine-Quality_files/figure-html/den_vs_res_sug_zoomed-1.png)<!-- -->

There seems to be an increase in density as there is an increase of residual sugar.

![](Logan-Burke-Red-Wine-Quality_files/figure-html/den_vs_alcohol-1.png)<!-- -->

And there seems to be a decrease of density with an increase in the alcohol content of the wine.

![](Logan-Burke-Red-Wine-Quality_files/figure-html/residual_sug_vs_alco-1.png)<!-- -->

And now taking a zoomed look at it:

![](Logan-Burke-Red-Wine-Quality_files/figure-html/residual_sug_vs_alco_zoomed-1.png)<!-- -->

```
## 
## 	Pearson's product-moment correlation
## 
## data:  residual.sugar and alcohol
## t = 1.6829, df = 1597, p-value = 0.09258
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  -0.006960058  0.090909069
## sample estimates:
##        cor 
## 0.04207544
```

It was expected that a stronger correlation between the alcohol content and the residual sugar would be shown, since the alcohol should be coming from the fermentation of the sugars.

Possible some of the wines are fortified with extra alcohol added after the fermentation process, or the yeast behaves in such a way that do not allow the data to establish a linear relationship between sugar fermentation and alcohol production. There is also the fact that the data does not mention which types of grapes were used, which may have different sugar contents that could impact this relationship.


## Sulphates and sulfur oxide

The sulphates are an additive which can contribute to sulfur dioxide gas levels in wine.

![](Logan-Burke-Red-Wine-Quality_files/figure-html/total_sulf_vs_sulphates-1.png)<!-- -->

Compare this to free sulfur dioxide:

![](Logan-Burke-Red-Wine-Quality_files/figure-html/free_sulf_vs_sulphates-1.png)<!-- -->

Checking the correlation between total sulfur dioxide levels with sulphates:


```
## 
## 	Pearson's product-moment correlation
## 
## data:  total.sulfur.dioxide and sulphates
## t = 1.7178, df = 1597, p-value = 0.08602
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  -0.006087119  0.091774762
## sample estimates:
##        cor 
## 0.04294684
```

And now checking the correlation between the free sulfur dioxide levels with sulphates:


```
## 
## 	Pearson's product-moment correlation
## 
## data:  free.sulfur.dioxide and sulphates
## t = 2.0671, df = 1597, p-value = 0.03888
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.002643125 0.100424406
## sample estimates:
##        cor 
## 0.05165757
```

The relationship between the sulphates levels and sulfur dioxide is pretty weak.

# Bivariate Analysis

**Talk about some of the relationships you observed in this part of the investigation. How did the feature(s) of interest vary with other features in the dataset?**

The wine quality rating is higher and has a stronger relationship with the 4 variables of volatile acidity, citric acid, sulphates and alcohol content. The correlation coefficients show that the strength of the relationship with the variables is shown below.


```
##                             [,1]
## fixed.acidity         0.11408367
## volatile.acidity     -0.38064651
## citric.acid           0.21348091
## residual.sugar        0.03204817
## chlorides            -0.18992234
## free.sulfur.dioxide  -0.05690065
## total.sulfur.dioxide -0.19673508
## density              -0.17707407
## pH                   -0.04367193
## sulphates             0.37706020
## alcohol               0.47853169
```

For the free and total sulfur dioxide variables, in the plots already explored it was shown that the medium quality ratings (5 and 6) have both higher content than the low and higher quality ratings. This may indicate that there is some interaction with the other variables.

**Did you observe any interesting relationships between the other features (not the main feature(s) of interest)?**

The expected relationship between the pH level and acidity level was as expected.

It was of interest to observe the relationship between the density and the alcohol and sugar concentration of the wine.

It was unexpected to not find a stronger relationship between the residual sugar and alcohol concentration, since the alcohol should in theory come from the fermentation of sugars in the wine making process.

**What was the strongest relationship you found?**

The correlation coefficients show that the variable with the strongest relationship with quality rating is the alcohol concentration.

# Multivariate Plots Section

## Correlation Matrix

Here is a matrix of the correlations in the data:

![](Logan-Burke-Red-Wine-Quality_files/figure-html/corr_matrix-1.png)<!-- -->

## Alcohol, volatile acidity and quality

Quality strongly correlates with alcohol and volatile acidity. Volatile acidity seems to come from acetic acid which can give an unpleasant taste to the wine.

![](Logan-Burke-Red-Wine-Quality_files/figure-html/alco_vol_acid_and_quality-1.png)<!-- -->

The lowest quality wines have a low alcohol and high volatile acidity concentration. The middle quality wines (rated 5 and 6) can seem to be found spread throughout the plot area.

## Acidity, pH, quality

![](Logan-Burke-Red-Wine-Quality_files/figure-html/acid_ph_and_quality-1.png)<!-- -->

There does not seem to be much of a pattern in the quality ratings distribution in this plot.

## Citric acid, alcohol, quality

![](Logan-Burke-Red-Wine-Quality_files/figure-html/citric_alco_and_quality-1.png)<!-- -->

The increase of both citric acid and alcohol concentrations tends to give a higher quality wine rating. But there is also wines with a quality rating of 5 on a wide range of citric acid concentrations as well as at low alcohol concentrations. There are also high quality rated wines with low citric acid concentrations.

## Alcohol and Sulphates

![](Logan-Burke-Red-Wine-Quality_files/figure-html/alcohol_and_sulphates-1.png)<!-- -->

And here is the same but zoomed in:

![](Logan-Burke-Red-Wine-Quality_files/figure-html/alcohol_and_sulphates_zoomed-1.png)<!-- -->

Alcohol and sulphates appear to have a positive correlation, and higher alcohol combined with higher sulphates concentrations yields a higher quality wine rating for the range of sulphates between 0 and 1.


```
## 
## 	Pearson's product-moment correlation
## 
## data:  alcohol and sulphates
## t = 3.7568, df = 1597, p-value = 0.0001783
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.04477906 0.14196454
## sample estimates:
##        cor 
## 0.09359475
```

# Multivariate Analysis

**Talk about some of the relationships you observed in this part of the investigation. Were there features that strengthened each other in terms of looking at your feature(s) of interest?**

The main relationships explored were between the biggest correlations with quality.

It has been shown how alcohol and volatile acidity relate to quality. Higher alcohol and lower acidity concentrations give a typically better wine rating for quality.

Higher amounts of citric acid combined with higher alcohol content also typically give a better wine rating for quality.

There is also a trend to have a better quality rating for the wine with both the alcohol and the sulphates are higher.

# Final Plots and Summary

### Plot One
![](Logan-Burke-Red-Wine-Quality_files/figure-html/plot_one-1.png)<!-- -->

**Description One**

Here is the distribution of volatile acidity across the different quality ratings. The boxplot shows the quantile boundaries and median values, while the overlapping dots shows the actual distribution of the wine samples. There is an unbalanced amount between the middle ratings and the higher and lower quality ratings. There are much more middle quality rated wines than there are low and high quality rated wines. The line connects the median values and helps to show visually the decreasing trend of volatile acidity with quality rating. Wines with lower volatile acidity tend to be rated higher in quality.

### Plot Two

![](Logan-Burke-Red-Wine-Quality_files/figure-html/plot_two-1.png)<!-- -->

**Description Two**

Alcohol seems to have a significant relationship to the rated quality of the wines. For the quality ratings of 3 to 5, the effect seems to be somewhat limited. The quality rating is likely being impacted by another variable at these rating levels, but from the quality ratings of 5 to 8, there seems to be a higher increase in the alcohol concentration of the wine samples. There is a general trend that wines with a higher alcohol concentration will have a higher rated quality rating.

### Plot Three
![](Logan-Burke-Red-Wine-Quality_files/figure-html/plot_three-1.png)<!-- -->

**Description Three**

In the analysis, it has been shown that it appears that alcohol and volatile acidity play significant roles on the quality rating a wine will receive. Using this plot, it can be seen that there does seem to be a trend that the lower a wine's concentration of volatile acidity and the higher its alcohol concentration, the higher it is likely the wine sample will rate for its quality rating. There is also visible the inverse trend, that wine samples with a high concentration of volatile acidity and low alcohol concentration, the lower rating that wine sample will likely be for its quality rating.

# Reflection

This project allowed the exploration of a real life dataset using the knowledge of using R. The dataset was already organized and tidy, so it was ideal to focus on analyzing the data and exploring it without having to go through the tedium that sometimes accompanies data cleaning. 

Working with datasets has the challenge of deciding how to approach the exploration of the data. Because this dataset also came with a description file, it already outlined some possible variables that might lend themselves to exploration. This proved quite useful. For example, when the description file said that citric acid could add a freshness element to the taste of wines, while acetic acid would add an almost vinegar like taste, this provided context to many of the observations that would follow. This also shows just how important it is to have some knowledge of the subject matter when beginning an analysis, as some contexts could be lost on a casual observer. This knowledge helps structure the approach to the analysis, and allows better theory formation so that meaningful insights can be gotten from the data processing. 

Another challenge faced was figuring out how to communicate with the multivariate plots. When adding a third dimension to a plot, mentally visualizing that can become harder for individuals. Use of color helped with this challenging approach, which made it easier to grasp what information was being communicated by each step of the analysis, adding in clarity and depth of information.Using the correlation matrix was a neat addition that was quite welcome.This also helped to narrow down which variables should be focused on for further exploration. Overall, data needs to be communicated in such a way that its story can easily be understood and followed for the reader, so putting extra effort into making it legible proved a good use of time. 

As a way to expand the analysis, bringing in other different types of wines would be an interesting way to see if the trends are strengthened, or weakened, with this new data. Expanding the dataset with the same type of wine, but simply having more observations would also be interesting, as the dataset used in this analysis was not very large. It would also be of interest if some additional variables could be added to the total dataset, such as type of grape, location grown, and how long before the grape was harvested. 

In summary, having found and explored the main relationships in the dataset, using these trends to predict how other wines would fare as to their quality rating would be a logical next step. Then gathering data from those observations made on the predicted trends could be made to further refine the process of the prediction in the future. 

#  Refrences
P. Cortez, A. Cerdeira, F. Almeida, T. Matos and J. Reis. 
Modeling wine preferences by data mining from physicochemical properties.
In Decision Support Systems, Elsevier, 47(4):547-553. ISSN: 0167-9236.

Available at: [@Elsevier] http://dx.doi.org/10.1016/j.dss.2009.05.016
[Pre-press (pdf)] http://www3.dsi.uminho.pt/pcortez/winequality09.pdf
[bib] http://www3.dsi.uminho.pt/pcortez/dss09.bib

StackOverflow website for various questions and research.
Available at: http://www.Stackoverflow.com 

R Bloggers website for various guides to using R, including a correlation heatmap.
Available at: http://www.r-bloggers.com

R Markdown website by RStudio
Available at: https://rmarkdown.rstudio.com/authoring_pandoc_markdown.html
