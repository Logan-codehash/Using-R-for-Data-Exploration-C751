---
title: "White Wine Quality Analysis"
author: "Logan Burke"
date: "May 2021"
output:
  html_document:
    fig_caption: TRUE
    theme: readable
    toc: TRUE
---

```{r echo=FALSE, message=FALSE, warning=FALSE, packages}
# echo set to false so it does not show in html
library(ggplot2)
library(GGally)
library(RColorBrewer)
library(reshape)
library(dplyr)
library(gridExtra)
library(scales)
library(memisc)
library(tidyr)
library(grid)
library(ggcorrplot)
```

```{r echo=FALSE, loading_the_data}
# load the csv file
csv_data <- read.csv("wineQualityWhites.csv",  row.names = 1,
                     stringsAsFactors = FALSE)
#reorder the variables to make more logical sense
wine = csv_data[c("alcohol","residual.sugar","chlorides","sulphates",
                  "free.sulfur.dioxide","total.sulfur.dioxide",
                  "citric.acid","volatile.acidity","fixed.acidity",
                  "pH","density","quality")]
quality_as_int = as.numeric(wine$quality)
quality_rating = as.factor(wine$quality)
```

```{r echo=FALSE, functions_setup} 
# some functions to keep from duplicating work for some plots

# creating a histogram  
creating_a_histo <- function(variable, ...){
ggplot(data = wine, aes_q(as.name(variable)))+
geom_histogram(...,fill = '#cda1a7',color = '#7b3c58')
}

# creates a summary
creating_sum <- function(variable){
print(summary(wine[[variable]]))
}

# creating plot with a summary
creating_plot_sum <- function(variable, ...){
creating_sum(variable)
creating_a_histo(variable, ...)
}

# creating a box plot 
creating_box_plot <- function(variable){
ggplot(data = wine, aes_q(x = ~quality_rating, 
                          y = as.name(variable)))+
stat_boxplot()+
geom_jitter(width = .25, alpha = .05)+
geom_boxplot(fill = "#cda1a7", color = "#7b3c58", alpha = 0.3)+
stat_summary(aes(group = 1), geom = "line", fun = "median", 
             color = "#34bde1", size = 3, alpha = 0.5)
}

# creating separate plot for other comparison 
diffrent_plot <- function(var1, var2){
ggplot(data = wine, aes_q(x = as.name(var1), y = as.name(var2)))+
geom_point(fill = "#cda1a7", color = "#7b3c58", alpha = 0.3)+
geom_smooth(method = "gam", se = TRUE)
}

# creating median summary
median_func <- function(variable){
print(paste("Median of", variable, "by quality:"))
print(by(wine[[variable]], wine$quality, median))
}

# boxplot that has a median summary
boxplot_and_median <- function(variable){
median_func(variable)
creating_box_plot(variable)
}

# scatter plot creation
creating_scat_plot <- function(x, y){
ggplot(wine, aes_q(x = as.name(x), y = as.name(y), 
                   color = ~quality_rating))+
geom_point(alpha = 0.7, size = 2)+
geom_smooth(method = "lm", se = FALSE, size = .75)+
scale_color_brewer(type = "div", palette = "RdYlBu")+
labs(color = "Quality Rating")
}

# correlation matrix
creating_matrix <- function(variable){
temp_data = cor(variable, method = c("spearman"))
ggcorrplot(temp_data, colors = c("#7f0000", "white", "#000099"))
}
```

# Introduction
This report will be using R and exploratory data analysis techniques to look at a dataset about white wine quality. The dataset that will be explored in this analysis is "Modeling wine preferences by data mining from physicochemical properties". The reference information can be found in the References section at the end of this report. 

The dataset contains several physicochemical attributes from samples of white wine of the Portuguese "Vinho Verde" and has sensory classifications made by wine experts.

# Univariate exploration and plots of the data
Taking a look at the data, this is what we find:

```{r echo=FALSE, var_all}
str(wine)
```

There are `r ncol(wine)` variables and `r nrow(wine)` observations as we can see in the data. The variables are all of the numeric type, with the Quality being explicitly of the integer type.

The variables are based on the physicochemical tests, and are as follows along with explanations of what they entail:

- Alcohol: the alcoholic percentage content of the wine. Measured as percentage by volume.

- Residual sugar: the amount of sugar remaining after fermentation stops, it is rare to find wines with less than 1 gram per liter and wines with greater than 45 grams per liter are considered sweet. Measured as grams per decimeters cubed.

- Chlorides: the amount of salt in the wine. Measured as grams per decimeters cubed.

- Sulphates: a wine additive which can contribute to sulfur dioxide gas (S02) levels, which acts as an antimicrobial and antioxidant. Measured as potassium sulfate in grams per decimeters cubed.

- Free sulfur dioxide: the free form of SO2 exists in equilibrium between molecular SO2 (as a dissolved gas) and bisulfite ion; it prevents microbial growth and the oxidation of wine. Measured as milligrams per decimeters cubed.

- Total sulfur dioxide: amount of free and bound forms of S02; in low concentrations, SO2 is mostly undetectable in wine, but as free SO2 concentrations exceed 50 ppm, SO2 becomes evident in the nose and taste of wine. Measured as milligrams per decimeters cubed.

- Citric acid: in small quantities, citric acid can add 'freshness' and flavor to wines. Measured as grams per decimeters cubed.

- Volatile acidity: the amount of acetic acid in wine, which at too high of levels can lead to an unpleasant, vinegar taste. Measured as acetic acid grams per decimeters cubed.

- Fixed acidity: acid fixed or nonvolatile (does not evaporate readily). Measured as tartaric acid grams per decimeters cubed.

- pH: describes how acidic or basic a wine is on a scale from 0 (very acidic) to 14 (very basic); most wines are between 3-4 on the pH scale. 

- Density: the density of wine is close to that of water depending on the percent of alcohol and sugar content. Measured as grams per centimeter cubed. 

- Quality: This is the score assigned by wine experts and is the output from all the other variables; Using a score between 0 and 10, with 0 being poor wine quality and 10 being exceptional wine quality. 

A summary of the data shows its variability as shown:

```{r echo=FALSE, a_summary}
summary(wine)
```

A visualization of the variability of each variable by plotting each using a boxplot will provide a baseline:

```{r echo=FALSE, box_vis_all}
# using the data to create boxplots of each variable
temp_data <- melt(wine)
ggplot(temp_data, aes(factor(variable), value))+ 
geom_boxplot(fill = "#cda1a7", color = "#7b3c58") + 
  facet_wrap(~variable, scale="free")
```

Now taking a look at each individual variable to explore it more closely. 

## Alcohol

```{r echo=FALSE, alcohol}
creating_plot_sum("alcohol", binwidth = 0.1)
```

The alcohol concentration distribution is right skewed a little. The highest peak of the distribution is at 9.5 percent alcohol and the median value is 10.40 percent. The maximum amount of alcohol present in the observations is 14.20 percent by volume.

## Residual sugar

```{r echo=FALSE, residual_sugar}
creating_plot_sum("residual.sugar", binwidth = 0.2)
```

Taking a closer look:

```{r echo=FALSE, residual_sugar_zoomed}
#limiting the range so it is more visible
creating_a_histo("residual.sugar", binwidth = 0.2)+
coord_cartesian(xlim = c(0, 20))
```

The distribution of residual sugar has a median value of 5.2 g/dm^3. The distribution is right skewed with a long tail on the right side. There are several observations that appear to possibly be outliers to the far right. A second plot with them removed is shown as well for clarity. 

## Chlorides

```{r echo=FALSE, chlorides}
creating_plot_sum("chlorides", binwidth = 0.01)
```

Taking a closer look:

```{r echo=FALSE, chlorides_zoomed}
#limiting the range again
creating_a_histo("chlorides", binwidth = 0.002)+
coord_cartesian(xlim = c(0, 0.1))
```

The distribution of chlorides in the wine samples has a median value of 0.043 g/dm^3. It looks like there are outliers to the right along its tail, with its max at 0.346 g/dm^3. A second plot with them removed is shown.

## Sulphates

```{r echo=FALSE, sulphates}
creating_plot_sum("sulphates", binwidth = 0.05)
```

And here is a closer look:

```{r echo=FALSE, sulphates_zoomed}
#limiting the range another time
creating_a_histo("sulphates", binwidth = 0.01)+
coord_cartesian(xlim = c(0.15, 1))
```

The distribution of sulphates is slightly right skewed. The median value of the sulphates is 0.470 and most of the wines have a concentration between 0.410 and 0.550.

## Free sulfur dioxide

```{r echo=FALSE, free_sulfur_dioxide}
creating_plot_sum("free.sulfur.dioxide", binwidth = 2)
```

A closer look at it:

```{r echo=FALSE, free_sulfur_dioxide_zoomed}
#limiting the range another time
creating_a_histo("free.sulfur.dioxide", binwidth = 1)+
coord_cartesian(xlim = c(0, 100))
```

The distribution of free sulfur dioxide is shown, and is right skewed, with a maximum of 289. There appear to be some outliers as there are few observations between 100 and 289. The median value is 34 mg/dm^3 of free sulfur dioxide. 

## Total sulfur dioxide

```{r echo=FALSE, total_sulfur_dioxide}
creating_plot_sum("total.sulfur.dioxide", binwidth = 5)
```

A zoomed in look:

```{r echo=FALSE, total_sulfur_dioxide_zoomed}
#limiting the range another time
creating_a_histo("total.sulfur.dioxide", binwidth = 2)+
coord_cartesian(xlim = c(0, 300))
```

The distribution of total sulfur dioxide is right skewed with a median value of 134 mg/dm^3. There appears to be some outliers, as there are few observations between roughly 260 and 440.

## Citric acid

```{r echo=FALSE, citric_acid}
creating_plot_sum("citric.acid", binwidth = 0.03)
```

Looking closer at it:

```{r echo=FALSE, citric_acid_zoomed}
#limiting the range another time
creating_a_histo("citric.acid", binwidth = 0.01)+
coord_cartesian(xlim = c(0, .8))
```

Median of the wines tested have .320 g/dm^3 of citric acid, this acid is usually only found in very small concentrations in wine it seems. There appear to be some outliers with above 1 g/dm^3 of citric acid.

## Volatile acidity

```{r echo=FALSE, volatile_acidity}
creating_plot_sum("volatile.acidity", binwidth = 0.02)
```

Looking at it zoomed in:

```{r echo=FALSE, volatile_acidity_zoomed}
#limiting the range another time
creating_a_histo("volatile.acidity", binwidth = 0.01)+
coord_cartesian(xlim = c(0.1, .9))
```

The median value is 0.260. Most of the observations fall in the range 0.210 - 0.320 and outliers are on the higher end of the range roughly above the .9 g/dm^3.

## Fixed Acidity

```{r echo=FALSE, fixed_acidity}
creating_plot_sum("fixed.acidity", binwidth = 0.2)
```

Getting a closer look:

```{r echo=FALSE, fixed.acidity_zoomed}
#limiting the range another time
creating_a_histo("fixed.acidity", binwidth = 0.1)+
coord_cartesian(xlim = c(4, 11))
```

The median fixed acidity for the white wines in the dataset is 6.80 g/dm^3. Most of the wines tested have an acidity between 6.30 and 7.30. The distribution of fixed acidity is slightly right skewed and there are some outliers in the higher range of roughly above 10.5 g/dm^3. There is a maxium of 14.20

## pH

```{r echo=FALSE, pH}
creating_plot_sum("pH", binwidth = 0.02)
```

All wines typically have a low pH level. Acids are produced through the fermentation process. The median value is 3.180, and most wines have a pH between 3.090 and 3.280.

## Density

```{r echo=FALSE, density}
creating_plot_sum("density", binwidth = 0.0005)
```

A zoomed in view:

```{r echo=FALSE, density_zoomed}
#limiting the range another time
creating_a_histo("density", binwidth = 0.0005)+
coord_cartesian(xlim = c(0.985, 1.005))
```

The density of the observations varies only a little, with most of the values being between 0.9917 and 0.9961. This would make sense, as wine has a density close to that of water. The distributions median value is 0.9937 g/cm^3.

## Quality
```{r echo=FALSE, quality}
wine_quality = factor(wine$quality, ordered = TRUE)
summary(wine_quality)
ggplot(data = wine, aes(x = quality))+
geom_bar(fill = '#cda1a7',color = '#7b3c58')
```

It appears that the distribution of wine quality appears to be normal with many wines at an average quality rating of 5 or 6. There are no wines with a quality lower than 3 and no wines higher than a quality rating of 9.

# Univariate Analysis

**What is the structure of your dataset?**

The dataset has `r ncol(wine)` variables regarding `r nrow(wine)` observations. Each observation corresponds to a white wine sample of the Portuguese "Vinho Verde". Of the variables, 11 correspond to the results of a physicochemical test and one variable (`quality`) corresponds to the result of a sensory panel rating by wine experts.

**What is/are the main feature(s) of interest in your dataset?**

The main feature of interest in the dataset is the quality rating of each sample.

**What other features in the dataset do you think will help support your investigation into your feature(s) of interest?**

The physicochemical test results may help support the investigation into the dataset. All of them are related to characteristics which may affect the flavor profile of the wine. They correspond to concentrations of molecules which may have an overall impact on taste, and by extension, the quality rating of the wine. Density is a physical property which will depend on the percentage of alcohol and sugar content, which will also affect taste of the wine.

Some variables may have a stronger correlation with each other. For instance, the pH will depend on the amount of acid concentration, while total sulfur dioxide may have a similar distribution to that of free sulfur dioxide levels.


**Did you create any new variables from existing variables in the dataset?**

No new variables were created in the dataset for this analysis.

**Of the features you investigated, were there any unusual distributions? Did you perform any operations on the data to tidy, adjust, or change the form of the data? If so, why did you do this?**

There were no unusual distributions. There were also no missing values and no need to adjust the data. It was already a tidy dataset. There were some outliers in the data that were noted, and these might have been due to an input error when recording the data. 


# Bivariate exploration and plots of the data

## Alcohol vs. Quality

```{r echo=FALSE, alcohol_vs_quality}
boxplot_and_median("alcohol")
```

Besides the small downward dip in the quality at the 5 rating level, the higher the alcohol content, the higher rating the wine seems to be given.

```{r echo=FALSE, warning=FALSE, message=FALSE, alco_corrilation_test}
cor.test(~quality_as_int+alcohol, data = wine)
```

As we can see from the Pearson correlation test, there is a decent positive correlation between the alcohol content of a wine sample and what quality rating it receives. 

## Residual Sugar vs. Quality

```{r echo=FALSE, warning=FALSE, sugar_vs_quality}
boxplot_and_median("residual.sugar")
```

Now taking a closer look by limiting the Y axis:

```{r echo=FALSE, warning=FALSE, sugar_vs_quality_top_10perct}
#removing the top 10% to be able to have a better look. 
#This is what throws the warnings about multiple rows being dropped. 
creating_box_plot("residual.sugar")+
ylim(1, 10)
```

Residual sugar seems to have a low impact on the quality rating of the wines. It is interesting that at the rating level of 9, the residual sugar tends to be lower than at the rating level of 3, even though for the mid ranged rating levels it tends to go up. 

```{r echo=FALSE, warning=FALSE, message=FALSE, res.sugar_corr_test}
cor.test(~quality_as_int+residual.sugar, data = wine)
```
The correlation test shows similar insights into the data.


## Chlorides vs. Quality

```{r echo=FALSE, warning=FALSE, chlor_vs_quality}
boxplot_and_median("chlorides")
```

Now looking at it a little closer:

```{r echo=FALSE, warning=FALSE, chlor_vs_quality_zoomed}
creating_box_plot("chlorides")+
ylim(.02, .06)
```

Possibly a slight relation. Seems that less chlorides could mean a higher quality wine rating. Interesting that it seems to first be a positive relation up until the level 5 rating, then begin declining as a possible negative relationship.

```{r echo=FALSE, warning=FALSE, message=FALSE, chlorides_corr_test}
cor.test(~quality_as_int+chlorides, data = wine)
```

The correlation test shows similar insights into the data, with a slight negative correlation found.


## Sulphates vs. Quality

```{r echo=FALSE, warning=FALSE, sulph_vs_quality}
boxplot_and_median("sulphates")
```

Now taking a more zoomed in look at the trend.

```{r echo=FALSE, warning=FALSE, sulph_vs_quality_zoomed}
creating_box_plot("sulphates")+
ylim(NA, quantile(wine$sulphates, 0.90))
```

There seems to be very little relationship between sulphates and quality.

```{r echo=FALSE, warning=FALSE, message=FALSE, sulphates_corr_test}
cor.test(~quality_as_int+sulphates, data = wine)
```
Very little correlation is found. 

## Free sulfur dioxide vs. Quality

```{r echo=FALSE, free_sulf_vs_quality}
boxplot_and_median("free.sulfur.dioxide")
```

There seems to be little relation here.

According to the information that was provided with the dataset, when free SO2 is lower than 50 ppm (~ 50 mg/L), it is undetectable to humans. In the following plot there are very few wines that are above this level which suggests that the variations seen in this plot are not related to an effect of the free SO2, but to the unbalanced distribution of wines across the quality ratings.

```{r echo=FALSE, warning=FALSE, free_sulf_vs_quality_other}
creating_box_plot("free.sulfur.dioxide")+
geom_hline(yintercept = 50, color = "#ddc126",
           linetype = 2, size = 1.5)+
ylim(NA, 60)
```

So only a little correlation would be expected.

```{r echo=FALSE, warning=FALSE, message=FALSE, free.sulfur_cor_test}
cor.test(~quality_as_int+free.sulfur.dioxide, data = wine, 
         method = "spearman")
```

And little correlation is found.

## Total sulfur dioxide  vs. Quality

```{r echo = FALSE, warning=FALSE, total_sulf_dio_vs_quality}
boxplot_and_median("total.sulfur.dioxide")+
ylim(NA, 200)
```

Similar to free sulfur dioxide concentrations.

```{r echo=FALSE, warning=FALSE, message=FALSE, tot.sulfur_corr_test}
cor.test(~quality_as_int+total.sulfur.dioxide, data = wine)
```

Interestingly, there is a bit more of a negative correlation found than comparatively to the correlation of that for free sulfur dioxide. 

## Citric Acid vs. Quality

```{r echo=FALSE, citric_acid_vs_quality}
boxplot_and_median("citric.acid")
```

A zoomed in look:

```{r echo=FALSE,warning=FALSE, citric_acid_vs_quality_zoomed}
creating_box_plot("citric.acid")+
ylim(NA, .75)
```

There seems to not be a relationship here.

```{r echo=FALSE, warning=FALSE, message=FALSE, citric.acid_corr_test}
cor.test(~quality_as_int+citric.acid, data = wine)
```
The correlation test confirms the previous observation of the graph. 

## Volatile Acidity vs. Quality

```{r echo=FALSE, volt_acid_vs_quality}
boxplot_and_median("volatile.acidity")
```

And now a zoomed in look:

```{r echo=FALSE, warning=FALSE, volt_acid_vs_quality_zoomed}
creating_box_plot("volatile.acidity")+
ylim(NA,.5)
```

There seems to be a slight downward trend until the rating at level 9, which could be to a more limited sample size of quality level 9 wines.

```{r echo=FALSE, warning=FALSE, message=FALSE, vol.acid_corr_test}
cor.test(~quality_as_int+volatile.acidity, data = wine)
```
A very slight negative correlation is found. 


## Fixed Acidity vs. Quality

```{r echo=FALSE, fixed_acid_vs_quality}
boxplot_and_median("fixed.acidity")
```

A closer look:

```{r echo=FALSE,warning=FALSE,fixed_acid_vs_quality_zoomed}
creating_box_plot("fixed.acidity")+
ylim(4.5,9)
```

There is a slight trend of a higher quality rating when there is a lower fixed acidity concentration. However, there are less observations at the quality ratings of 3 and 9 compared to middle observations, which may make the median value not very accurate. Additionally, there is a big dispersion of acidity values across each quality scale.

```{r echo=FALSE, warning=FALSE, message=FALSE, fixed.acid_corr_test}
cor.test(~quality_as_int+fixed.acidity, data = wine)
```
Only a little negative correlattion is found. 


## pH vs. Quality

```{r echo=FALSE, pH_vs_quality}
boxplot_and_median("pH")
```

And now a closer look:

```{r echo=FALSE,warning=FALSE, pH_vs_quality_zoomed}
creating_box_plot("pH")+
ylim(3, 3.75)
```

There seems to be an upward trend here. This could mean that a higher acid concentration in the wine will correlate to a higher quality of wine. This relationship will be checked later on.

```{r echo=FALSE, warning=FALSE, message=FALSE, pH_corrilation_test}
cor.test(~quality_as_int+pH, data = wine)
```
However, seems there is little correlation found.

## Density vs. Quality

```{r echo=FALSE, density_vs_quality}
boxplot_and_median("density")
```

And zooming in shows:

```{r echo=FALSE,warning=FALSE, density_vs_quality_zoomed}
creating_box_plot("density")+
ylim(.985,1.005)
```

Lower density seems to mean a higher quality rating. There is a trend at the rating of a quality of 5 that breaks this trend slightly though. From the information provided with the dataset, it was stated that the density will depend on the percentage of alcohol and sugar content in the wine. This relationship will be checked later on, but seems like there could be a relation. 

```{r echo=FALSE, warning=FALSE, message=FALSE, density_corr_test}
cor.test(~quality_as_int+density, data = wine)
```

Decent negative correlation is found. 

## Alcohol vs. Residual Sugar 

```{r echo=FALSE, warning=FALSE, message=FALSE, residual_sug_vs_alco}
diffrent_plot("alcohol","residual.sugar")
```

And now taking a zoomed look at it:

```{r echo=FALSE, warning=FALSE, message=FALSE, sug_vs_alco_zoomed}
diffrent_plot("alcohol","residual.sugar")+
ylim(1, 25)
```

And its correlation test results:

```{r echo=FALSE, warning=FALSE, message=FALSE, sug_vs_alco_corr_test}
cor.test(~ residual.sugar + alcohol, data = wine)
```

It was expected that a stronger correlation between the alcohol content and the residual sugar would be shown, since the alcohol should be coming from the fermentation of the sugars. However, this is still a decent negative correlation.

Possibly some of the wines are fortified with extra alcohol added after the fermentation process, or the yeast behaves in such a way that does not allow the data to establish a linear relationship between sugar fermentation and alcohol production. There is also the fact that the data does not mention which types of grapes were used, which may have different sugar contents that could impact this relationship.

## Sulphates vs. Total Sulfur Dioxide 

```{r echo=FALSE, warning=FALSE, message=FALSE, sulphate_vs_total_so2}
diffrent_plot("sulphates", "total.sulfur.dioxide")
```

And now taking a zoomed look at it:

```{r echo=FALSE, warning=FALSE, message=FALSE, sulph_vs_tot_so2_zoomed}
diffrent_plot("sulphates", "total.sulfur.dioxide")+
ylim(NA, 250)
```

And its correlation test results:

```{r echo=FALSE, warning=FALSE, message=FALSE, sul_vs_totso2_corr_test}
cor.test(~ sulphates + total.sulfur.dioxide, data = wine)
```

Seems that the addition of the sulphate additive does not have a large correlation to the total sulfur dioxide in the wine samples.  

## Sulphates vs. Free Sulfur Dioxide 

```{r echo=FALSE, warning=FALSE, message=FALSE, sulphates_vs_free_so2}
diffrent_plot("sulphates", "free.sulfur.dioxide")
```

And now taking a zoomed look at it:

```{r echo=FALSE, warning=FALSE, message=FALSE, sul_vs_freeso2_zoomed}
diffrent_plot("sulphates", "free.sulfur.dioxide")+
ylim(NA, 75)
```

And its correlation test results:

```{r echo=FALSE, warning=FALSE, message=FALSE, sul_vs_freeso2_corr_test}
cor.test(~ sulphates + free.sulfur.dioxide, data = wine)
```

Seems that the addition of the sulphate additive does not have a correlation to the free sulfur dioxide in the white wine samples. 

# Bivariate Analysis

**Talk about some of the relationships you observed in this part of the investigation. How did the feature(s) of interest vary with other features in the dataset?**

The wine quality rating is higher, and has a stronger relationship, with the 3 variables of chlorides, density and alcohol content. There are two others that are noteworthy, but not as impactful, are the variables of Total Sulfur Dioxide and Volatile Acidity. The correlation coefficients show that the strength of the relationship with the variables is shown below.

```{r echo=FALSE, corr_with_quality}
cor(x = wine[1:11],
y = as.numeric(wine$quality),
method = "spearman")
```

There is a negative correlation for density, which makes sense as alcohol would have an inverse relationship to density. And alcohol makes sense as a large contributor to the quality of a wine as well. Chlorides having a negative relation would make a wine sample less salty as its concentration goes down, which could explain how higher rated wines would have a lower concentration of chlorides. Total Sulfur Dioxide is interesting, as is volatile acidity.

**Did you observe any interesting relationships between the other features (not the main feature(s) of interest)?**

The expected relationship between the alcohol level and density was as expected.

It was of interest to observe the relationship between chlorides and quality.

It was unexpected to not find a stronger relationship between the residual sugar and alcohol concentration, since the alcohol should in theory come from the fermentation of sugars in the wine making process.

**What was the strongest relationship you found?**

The correlation coefficients show that the variable with the strongest relationship with quality rating is the alcohol concentration.

# Multivariate exploration and plots of the data

## Correlation Matrix

Here is a correlation matrix of the data:

```{r echo=FALSE, create_corr_matrix}
creating_matrix(wine)
```


## Alcohol, Density and Quality

Quality strongly correlates with alcohol content. And density should go down as the alcohol goes up. So as density decreases, alcohol goes up and the quality rating goes up for the wine in general.

```{r echo=FALSE, warning=FALSE, message=FALSE, alco_dens_and_quality}
creating_scat_plot("alcohol", "density")+
ylim(.985 , 1.005)
```

The lowest quality wines have a low alcohol and high density. The middle quality wines (rated 5 and 6) can seem to be found spread throughout the plot area, but more quality level 3 can been seen on the left side of the graph, and more blue (ratings of 8 or 9) towards the right side of the graph.

## Alcohol, Chlorides and Quality

```{r echo=FALSE, warning=FALSE, message=FALSE, alco_chlor_and_quality}
creating_scat_plot("alcohol", "chlorides")+
ylim(NA, .15)
```

The trend does seem to be that as the chloride concentration goes down and the alcohol concentration goes up, the quality increases.

## Alcohol, Total Sulfur Dioxide and Quality

```{r echo=FALSE, warning=FALSE, message=FALSE, alco_totsulf_and_quality}
creating_scat_plot("alcohol", "total.sulfur.dioxide")+
ylim(50, 250)
```

The total sulfur dioxide does not have much effect it seems on the quality of the wine.

## Alcohol, Volatile Acidity and Quality

```{r echo=FALSE, warning=FALSE, message=FALSE, alco_volacid_and_qual}
creating_scat_plot("alcohol", "volatile.acidity")+
ylim(.1, .66)
```

The volatile acidity of the wine does not seem to have too much of an impact, although it looks like generally the lower the volatile acidity, the better qualaty rating the wine will receive. 

## Chlorides, Volatile Acidity and Quality

```{r echo=FALSE, warning=FALSE, message=FALSE, chlo_volacid_and_qual}
creating_scat_plot("volatile.acidity", "chlorides")+
ylim(NA, .10)+
xlim(NA,.7)
```

Looks like if chlorides are lower, and volatile acidity is lower, the better quality rating the wine will be given. 

## Chlorides, Total Sulfur Dioxide and Quality

```{r echo=FALSE, warning=FALSE, message=FALSE, chlor_totsulf_and_qual}
creating_scat_plot("total.sulfur.dioxide", "chlorides")+
ylim(NA, .2)+
xlim(25,260)
```

Lower chlorides and total sulfur dioxide levels between 100 to 200 mg/dm^3 seem to be where the higher quality rated wines samples will fall. 

## Chlorides, Density and Quality

```{r echo=FALSE, warning=FALSE, message=FALSE, chlor_dens_and_qual}
creating_scat_plot("density", "chlorides")+
ylim(NA, .2)+
xlim(NA,1.0005)
```

A lower density and lower chlorides tend to a higher quality wine rating.

# Multivariate Analysis

**Talk about some of the relationships you observed in this part of the investigation. Were there features that strengthened each other in terms of looking at your feature(s) of interest?**

The main relationships explored was between the biggest correlations with quality.

It has been shown how alcohol, chlorides, density, total sulfur dioxide, and volatile acidity relate to quality. Higher alcohol, lower density, and low chloride concentration will typically give a better wine rating for quality.

There tends to be a range of total sulfur dioxide between 100 to 200 that also gives a better quality of wine. 

# Final Plots and Summary

### Plot One

```{r echo=FALSE, plot_one, warning=FALSE}
creating_box_plot("chlorides")+
xlab("Quality Rating")+
ylab("Chlorides (measured as g/dm^3)")+
ggtitle("Less chloride tends to mean a higher quality rating")+
ylim(.015, .08)+
theme(text = element_text(size = 14))
```

As chlorides go down, there is an increase in the quality rating of the wines. It is not a very strong relationship, but it is noteworthy nonetheless.

### Plot Two

```{r echo=FALSE, plot_three, warning=FALSE}
creating_box_plot("alcohol")+
xlab("Quality Rating")+
ylab("Alcohol (as % by volume)")+
ylim(8.75, 13.25)+
ggtitle("More alcohol tends to mean a higher quality rating")+
theme(text = element_text(size = 14))
```

Here is the distribution of alcohol across the different quality ratings. The boxplot shows the quantile boundaries and median values, while the overlapping dots show the actual distribution of the wine samples. There is an odd decline in quality ratings from 3 to 5 as alcohol concentration goes down, but then it significantly goes up from there. There is an unbalanced amount of samples between the middle ratings and the higher and lower quality ratings. There are much more middle quality rated wines than there are low and high quality rated wines. The line connects the median values and helps to show visually the increasing trend of alcohol with quality rating. 

### Plot Three

```{r echo=FALSE, plot_two, warning=FALSE, message=FALSE,}
creating_scat_plot(x = "chlorides", y = "alcohol")+
xlab("Chlorides (measured as g/dm^3")+
ylab("Alcohol (% by Volume)")+
ggtitle("Quality seems to be affected by chlorides and alcohol")+
labs(color = "Quality Rating")+
xlim(.01,.1)+
ylim(8,14)+
theme(text = element_text(size = 14))
```

In the analysis, it has been shown that it appears that alcohol and chlorides play a significant role on the quality rating a wine will receive. Using this plot, it can be seen that there does seem to be a trend that the lower a wine has of a concentration of chlorides and the higher its concentration of alcohol, the higher it is likely the wine sample will rate for its quality rating. There is also visible the inverse trend, that wine samples with a high concentration of chloride and a low alcohol concentration will have a lower rating for that wine sample.

# Reflection

Working with datasets has the challenge of deciding how to approach the exploration of the data. Because this dataset also came with a description file, it already outlined some possible variables that might lend themselves to exploration. This proved quite useful. For example, when the description file said that citric acid could add a freshness element to the taste of wines, while acetic acid would add an almost vinegar like taste, this provided context to many of the observations that would follow. Another example would be the density of wine being close to water, but lower as its alcohol content increased. This then explained the inverse relationship seen between the variables for density and alcohol in the graphs. This also shows just how important it is to have some knowledge of the subject matter when beginning an analysis, as some contexts could be lost on a casual observer. This knowledge helps structure the approach to the analysis, and allows better theory formation so that meaningful insights can be gotten from the data processing. 

Another challenge faced was figuring out how to communicate with the multivariate plots. When adding a third dimension to a plot, mentally visualizing that can become harder for individuals. Use of color helped with this challenging approach, which made it easier to grasp what information was being communicated by each step of the analysis, adding in clarity and depth of information.Using the correlation matrix was a neat addition that was quite welcome.This also helped to narrow down which variables should be focused on for further exploration. Overall, data needs to be communicated in such a way that its story can easily be understood and followed for the reader, so putting extra effort into making it legible proved a good use of time. The dataset already being clean and tidy made working with it significantly easier as a whole as well.

As a way to expand the analysis, bringing in other different types of wines would be an interesting way to see if the trends are strengthened, or weakened, with this new data. Expanding the dataset with the same type of wine, but simply having more observations would also be interesting, as the dataset used in this analysis was not very large. It would also be of interest if some additional variables could be added to the total dataset, such as type of grape, location grown, and how long before the grape was harvested. 

In summary, having found and explored the main relationships in the dataset, using these trends to predict how other wines would fare as to their quality rating would be a logical next step. Then gathering data from those observations made on the predicted trends could be made to further refine the process of the prediction in the future. 

#  References
P. Cortez, A. Cerdeira, F. Almeida, T. Matos and J. Reis. 
  Modeling wine preferences by data mining from physicochemical properties.
  In Decision Support Systems, Elsevier, 47(4):547-553. ISSN: 0167-9236.

  Available at: [@Elsevier] http://dx.doi.org/10.1016/j.dss.2009.05.016
  [Pre-press (pdf)] http://www3.dsi.uminho.pt/pcortez/winequality09.pdf
  [bib] http://www3.dsi.uminho.pt/pcortez/dss09.bib

StackOverflow website for various questions and research.
Available at: http://www.Stackoverflow.com 

R Bloggers website for various guides to using R.
Available at: http://www.r-bloggers.com

R Markdown website by RStudio
Available at: https://rmarkdown.rstudio.com/authoring_pandoc_markdown.html
