---
title: "Data Visulization: ggplot in R"
knit: (function(input_file, encoding) {
  out_dir <- 'docs';
  rmarkdown::render(input_file,
 encoding=encoding,
 output_file=file.path(dirname(input_file), out_dir, 'index.html'))})
output: 
  html_document: 
    keep_md: yes
---



# Data preps

Dataset 1: Midwest

Midwest is a one of the preset dataframes in ggplot2 package which contains demographic information of midwest counties. The dataframe contains 437 rows and 28 variables. Execute `?midwest` for details and help. Remember, always call `library(tidyverse)` before calling `library(ggplot2)`.


```r
library(tidyverse)
data(midwest)
```

Dataset 2: Titanic 
Note you should install the "titanic" package by calling `install.packages("titanic")`. Then call the library to use the data


```r
library(titanic)
data(titanic_train)
```

Titanic_train provides the outcome (also known as the “ground truth”) for each passenger. The data dictionary is shown below:

Column name | Description
---------| -------------
Survived  | 0 = No, 1 =Yes
Pclass | Ticket class. 1=1st, 2=2nd, 3=3rd
Sex | Gender information
Age | Passenger age
sibsp | num of siblings
parch| number of parents/children
ticket|Ticket number
fare| Passenger fare
cabin | Carbin number
Embarked | Port of Embarkation. C=Cherbourg, Q= Queenstown, S= Southsampton

Dataset 3: General Social Survey (GSS) data, 2016

The GSS dataset is a long-running survey of American adults that asks about a range of topics of interest to social scientists. By calling `gss_sm`, we will get a small subset of the questions from GSS, 2016. This datseta is included in the package called "socviz". See http://gss.norc.org/GetDocumentation for full documentation of the variables. 

Execute the commands below to load packages and datasets. Make sure you installed the `socviz` and `ggrepel` packages first. 



```r
library(tidyverse)
library(socviz)
library(ggrepel)
data(gss_sm)
```

This `gss_sm` data frame contains 2538 rows and 26 variables.
Try `?gss_sm` to learn more about each column.


# Basic graphics in R

Let's use the titanic data as examples. By executing the commands below, you will get a histogram, a pie chart and a boxplot:


```r
hist(titanic_train$Age)
```

<img src="F:/Course/UIUC/Fall2021/UP494/Lab/Lab4/Lab-exercise-ggplot-for-data-visualization/docs/index_files/figure-html/unnamed-chunk-4-1.png" width="350px" />

```r
pie(table(titanic_train$Sex))
```

<img src="F:/Course/UIUC/Fall2021/UP494/Lab/Lab4/Lab-exercise-ggplot-for-data-visualization/docs/index_files/figure-html/unnamed-chunk-4-2.png" width="350px" />

```r
boxplot(titanic_train$Age)
```

<img src="F:/Course/UIUC/Fall2021/UP494/Lab/Lab4/Lab-exercise-ggplot-for-data-visualization/docs/index_files/figure-html/unnamed-chunk-4-3.png" width="350px" />

Let's add a title and labels on x-y. 


```r
boxplot(titanic_train$Age, 
        main="This is a title of the boxplot ", 
        xlab="Edit here for x", 
        ylab="Here is label for y")
```

<img src="F:/Course/UIUC/Fall2021/UP494/Lab/Lab4/Lab-exercise-ggplot-for-data-visualization/docs/index_files/figure-html/unnamed-chunk-5-1.png" width="350px" />

Now, try this on your own: use the script below to generate a boxplot for different sub-groups of data: what is the distribution of age by gender? 


```r
boxplot(Age~Sex, data=titanic_train)
```


Now you are able to generate boxplots, pie charts and histogram plots (with customized axis labels and title) in R without installing any external packages.

# Introduce gglot

`ggplot` function is: 

1. a Well-developed package to create complex plots from data stored in a data frame

2. the most elegant and aesthetically pleasing graphics framework available in R

3. **Well-structured** data will save you lots of time when making figures with ggplot2


Please try this e-book: Data Visualization: A practical introduction. Click [here](https://socviz.co/)

Note you always need to call the `library (tidyverse)` first.

## 1: Initializing a ggplot object

Use the template below to initialize a ggplot object:


```r
ggplot(data = <DATA>, mapping = aes(<MAPPINGS>)) +  <GEOM_FUNCTION>()
```

- Specify the data frame 
- Aethetics: a visual property of the objects in the plot.
- Be careful with the position of the *parentheses.*
- **+** here to connect each object. 
- The `geom` function is the geometrical object that a plot uses to represent the data. People often describe plots by the type of geom that the plot uses. 



### Common plot aesthetics

The table below shows some common plot aesthetics of `ggplot()`.

Item | Description
---------| -------------
x	| Position on x-axis
y	| Position on y-axis
shape	| Shape
color |	Color of border of elements
fill | 	Color of inside of elements (changes the color of the graph by group/facor)
size |	Size
alpha | 	Transparency (1: opaque; 0: transparent)
linetype  |	Type of line (e.g., solid, dashed)

From: [Mastering Software Development in R.](https://bookdown.org/rdpeng/RProgDA/)

The table below shows some common GEOM functions of `ggplot()`.

### Common GEOM functions
Item | Description
---------| -------------
geom_point | Scatter plots, dot plots, etc.     
geom_boxplot | Boxplots    
geom_line | Trend lines, time series, etc.   
geom_path |Connects the observations in the order in which they appear in the data
geom_histogram | Visualise the distribution of a single continuous variable 
geom_hline  | Add horizontal reference lines
geom_vline  | Add vertical reference lines
geom_smooth  | Add a smoothing line in order to see what the trends look like
geom_density | Add a smoothed density estimates of a single continuous variable. 

## 2: Boxplot example

Now, let's try to use ggplot to generate a boxplot. What is the distribution of age by gender in the titanic dataset? 

- We need to estabilish a ggplot object. 
- First, specify the dataset `titanic_train`. Then, you need to specify the y axis is `Age` since you want to display a boxplot based on that.  Next, specify that you are interested in a boxplot, called "geom_boxplot()". We have already set x and y in the `ggplot` function (global settings), we do not need to repeat that again unless you would like to customize the x- and y-axis titles. 

Based on the instruction above, try to execute the script below to generate a general boxplot of the distribution of age:

```r
ggplot(titanic_train, aes(y=Age)) + geom_boxplot() 
```

<img src="F:/Course/UIUC/Fall2021/UP494/Lab/Lab4/Lab-exercise-ggplot-for-data-visualization/docs/index_files/figure-html/unnamed-chunk-8-1.png" width="350px" />

Now try it yourself: Let's display the distribution of age by gender information. Now you need to specify one more item, say `x=Sex`, in order to display the distribution by gender information on the x-axis. 


```r
data(titanic_train)
ggplot(titanic_train, aes(x = Sex, y = Age)) + geom_boxplot() 
```

You can also set a different shape of the boxplot. 

```r
ggplot(titanic_train, aes(x = Sex, y = Age)) + geom_violin() 
```


## 3. Histogram, Scatter plot and trend line (continous variables)
Let's look at the histogram of population the `midwest` dataset. You can directly call the geom_histogram function after ggplot. The binwidth specifies the range of values to aggregate into each bin for counting of your histogram. Apparently this population data is positively skewed. 


```r
ggplot(midwest, aes(x=poptotal))+
geom_histogram(binwidth=100000)
```

<img src="F:/Course/UIUC/Fall2021/UP494/Lab/Lab4/Lab-exercise-ggplot-for-data-visualization/docs/index_files/figure-html/unnamed-chunk-11-1.png" width="350px" />


Alternatively, we can check out the kernel density plot. A kernel density function creates a generalized or smoothed version of the distribution. The x variable is assigned to total population.   `..density..` indicates that y-axis will show density.Use adjust to change the binwidth. 



```r
ggplot(midwest, aes(x=poptotal, ..density..))+
geom_density(fill="Green", adjust=5)
```

<img src="F:/Course/UIUC/Fall2021/UP494/Lab/Lab4/Lab-exercise-ggplot-for-data-visualization/docs/index_files/figure-html/unnamed-chunk-12-1.png" width="350px" />

Let's move to two continuous variables. What is the relationship between total population and the size of the area? Let's first generate a scatter plot, where x is size of the area, and y is the total population. The basic template for ggplot is the same. This time, we need `geom_point` as the geom function. 


```r
ggplot(midwest, aes(x=area, y=poptotal)) + geom_point() 
```

<img src="F:/Course/UIUC/Fall2021/UP494/Lab/Lab4/Lab-exercise-ggplot-for-data-visualization/docs/index_files/figure-html/unnamed-chunk-13-1.png" width="350px" />

If we are interested to see any trend among these points, try to add another function to generate a trend line right on top of  the script above. It is called  `geom_smooth`; "lm" means linear model.


- **+** sign allows you to modify existing ggplot objects
- The **+** sign used to add new layers must be placed *at the end of the line*.


```r
ggplot(midwest, aes(x=area, y=poptotal)) + geom_point() + geom_smooth(method="lm") 
```

Note if there is no apparent linear relationship, feel free to use other method e.g. loess. 

It seems we need to "zoom in" the plot. There are two options to set up the scale limits: 

- use `xlim` or `ylim`

- use `coord_cartesian`

For example, if we force the maximum scale of y-axis (total population) to be 400,000, try out the scripts here and check the plots:

Option 1: use `ylim` below. 

```r
ggplot(midwest, aes(x=area, y=poptotal)) + geom_point() + 
  geom_smooth(method="lm") + ylim(0, 400000)
```


You will notice that the values beyond (0,400000) will just be removed. Though you "zoom in" the plot, but you lost some information within the dataset. 

Option 2: I prefer using coord_cartesian to "zoom" the figure. 


```r
ggplot(midwest, aes(x=area, y=poptotal)) + geom_point() + geom_smooth(method="lm") +
  coord_cartesian(ylim=c(0, 400000))
```

This coord_cartesian function will zoom in/out the plot without delete any data, like you're looking at it with a magnifying glass. Data is unchanged.

We can also change the color of these points. For example, how about color these points in the figure above by different states? You need to specify this within the `aes` function by adding `color=state`. It will automatically assign a unique color to each unique value of column "state". Try the script below: 


```r
ggplot(midwest, aes(x=area, y=poptotal)) + geom_point(aes(color=state)) + 
  geom_smooth(method="lm") +   coord_cartesian(ylim=c(0, 500000))
```

You can also mark/encirlce certain values in the chart so as to draw the attention to those values. The function called `geom_encircle()` in `ggalt` package is your best friends.

Within `geom_encircle()` function, you need to specify the data only includes the points (rows) of your interest. Next, You can establish curves outside the points. You can adjust the color and size of these outlines.



```r
library(ggalt)
midwest_select <- midwest[midwest$poptotal > 250000 & 
            midwest$poptotal <= 500000 &midwest$area > 0.01 & midwest$area < 0.1, ]

# Plot
ggplot(midwest, aes(x=area, y=poptotal)) + geom_point(aes(color=state)) + 
  geom_smooth(method="lm") +   coord_cartesian(ylim=c(0, 500000)) +
  geom_encircle(aes(x=area, y=poptotal), 
                data=midwest_select, 
                color="red", 
                size=.5, 
                expand=0.1)
```

<img src="F:/Course/UIUC/Fall2021/UP494/Lab/Lab4/Lab-exercise-ggplot-for-data-visualization/docs/index_files/figure-html/unnamed-chunk-18-1.png" width="350px" />

## 4. Bar chart (categorical data type)


The bar chart function in ggplot is also very straightforward. Let's go back to Titanic dataset and generate a bar chart to show how many people survived vs people perished. 


```r
ggplot(titanic_train, aes(x = Survived)) + geom_bar()  
```

<img src="F:/Course/UIUC/Fall2021/UP494/Lab/Lab4/Lab-exercise-ggplot-for-data-visualization/docs/index_files/figure-html/unnamed-chunk-19-1.png" width="350px" />

*"Well boys, do your best for the women and children, and look out for yourselves."*
                                         
- From Edward John Smith, the Captain of RMS Titanic

Did the gentlemen on board do the best for the women? What is the survival status based on gender information? 

You can use the `fill` argument to change the interior colouring by another variable for a bar chart. For example, to answer the question above, what is the distribution of people survived or not by gender? Set `fill=sex` within the `aes` function will change the color to each sub-group. Try the command below: 


```r
ggplot(titanic_train, aes(x = Survived)) + geom_bar(aes(fill = Sex))
```

Among the people who perished, there are more male than female; among the people who survived, there are more female than male. 

If you prefer, it will be more straightforward if we set the bar chart side by side based on the gender information. You need to put the `position = "dodge"` within the `geom_bar`. You can see there are more male died than female. Try the command below. 


```r
ggplot(titanic_train, aes(x = Survived)) + 
  geom_bar(aes(fill = Sex),  position = "dodge")
```




## 5. Title and labels

Please also add labels and title for your plot. You need to use the `labs` function.  


```r
ggplot(titanic_train, aes(x = Survived)) +  
  geom_bar(aes(fill = Sex),  position = "dodge") + 
  labs(title="Survival by Passenger gender?",x="Survival Status", y="Number of passengers")
```

<img src="F:/Course/UIUC/Fall2021/UP494/Lab/Lab4/Lab-exercise-ggplot-for-data-visualization/docs/index_files/figure-html/unnamed-chunk-22-1.png" width="350px" />



It is kind of weird to have numeric value as x-axis here -- what does 0.5 mean??

Check out the data type for the column called `Survived`. It is integers. In this case, if we want to plot out integers, R will assume it should be discrete values from 0~infinite. However, the 0s and 1s in this `Survived` column are just certain codes represent a certain status, where 0 means perished and 1 means survived. In other words, it should be converted to a factor instead of the integer data type. So our solution here is to change the data type of `Survived` within the `aes.` 

Try the command below and you will see the difference. 


```r
ggplot(titanic_train, aes(x = as.factor(Survived))) +  
  geom_bar(aes(fill = Sex),  position = "dodge") + 
  labs(title="Survival by Passenger gender?",x="Survival Status", y="Number of passengers")
```


## 6. Themes (non-data related components)

Try the command below to change the position of legend. Themes are a powerful way to customize the non-data components of your plots: i.e. titles, labels, fonts, background, grid lines, and legends. Themes can be used to give plots a consistent customized look. Modify a single plot's theme using `theme()`; see `theme_update()` if you want modify the active theme, to affect all subsequent plots. Use the themes available in complete themes if you would like to use a complete theme such as `theme_bw()`, `theme_minimal()`, and more.

Here we 1) Change the position of legend; 2) Change the background into dark mode. 3) Change the labels name on the x-axis using the `scale_x_discrete` function. Try the command below: 


```r
ggplot(titanic_train, aes(x = as.factor(Survived))) +  
  geom_bar(aes(fill = Sex),  position = "dodge") + 
  labs(title="Survival by Passenger gender?",x="Survival Status", y="Number of passengers")+
  theme_dark() +
   theme(legend.position = "top")+ scale_x_discrete("", 
          labels=c("0"="perished","1"="survived"))
```

Tips to set up colors: 


`scale_fill_manual()`: set up filled color for objects:box plot, bar plot, violin plot, dot plot, etc
`scale_color_manual()`: set up color for lines and points

Alternatively, use colorbrewer palettes:

`scale_fill_brewer()` set up filled color for objects: box plot, bar plot, violin plot, dot plot, etc
`scale_color_brewer()` change color using existing palettes for lines and points

Click [*here*](https://www.rapidtables.com/web/color/RGB_Color.html) for more details of color codes in R. 

## 7. Tips for aes

- If set in original `ggplot` globally, inherited by any other geoms
- If set in only in a `geom`, it will only be used in that geom
- If both, they override the settings in ggplot

### Question:

Execute the script below. Why the bar chart is colored by Passenger Class instead of gender information? 

Note: Pclass was an integer data type by default. Again, we need to convert this variable as a factor in order to use it as a categorical variable (set `aes(fill = as.factor(Pclass))`).


```r
ggplot(titanic_train, aes(x = Survived, fill=Sex)) + 
         geom_bar(aes(fill = as.factor(Pclass)))
```

<img src="F:/Course/UIUC/Fall2021/UP494/Lab/Lab4/Lab-exercise-ggplot-for-data-visualization/docs/index_files/figure-html/unnamed-chunk-25-1.png" width="350px" />

Since we set fill twice: 1) within the `ggplot`; 2) within `geom_bar`. In this case, the settings in `geom_bar` will overwrite the settings in `ggplot`. Though you set the plot to be filled by gender information in `ggplot`, it will be covered by the setting in `geom_bar`, which will fill the color by `Pclass`. 

## 8. Facet: multiple plots
ggplot also allows you to split one plot into multiple plots based on a factor. 

The example below will create the same bar chart (survived or not by gender), for each Passenger Class (set `facet_wrap(~Pclass)`). So we will have three sub-plots. You can read the `(~Pclass)` as `by Pclass`


```r
ggplot(titanic_train, aes(x = Survived, fill=Sex)) + geom_bar() + facet_wrap(~ Pclass)
```

<img src="F:/Course/UIUC/Fall2021/UP494/Lab/Lab4/Lab-exercise-ggplot-for-data-visualization/docs/index_files/figure-html/unnamed-chunk-26-1.png" width="350px" />


## 9. Example 1

Please try out the example below yourself. 

Recall the table about austin weather, which includes all the meteorological variables in austin from 2013-2017, Let's now compare the TempAvgF for year 2014, 2015, 2016 as a  line chart. 

Suggested steps:

1) Subset (use `filter` here) the dataset which only includes data from 2014-2016. 

2) Based on the subset, generate two new columns to store year (`year` function in `lubridate`) and day of the year (`yday` in the `lubridate` package). 

3) By default, the year column is numeric. If you directly use this column to plot out lines by group in aes:

`ggplot(austin_2014_2016, aes(x = day, y = TempAvgF)) + geom_line(aes(color = year, linetype = year)) `, an error message will pop up says `A continuous variable can not be mapped to linetype`. So let's change this column into factor. 

4) Now we can generate a line chart, where the color and linetype are different for each year, in the same chart. 


```r
library(lubridate)
library(tidyverse)
austin_weather <- read_csv("data/austin_Weather.csv")
austin_2014_2016 <- austin_weather %>% filter(year(Date)%in%c(2014,2015,2016))

austin_2014_2016$year <- year(austin_2014_2016$Date)

austin_2014_2016$day <- yday(austin_2014_2016$Date)
    
austin_2014_2016$year <- as.factor(austin_2014_2016$year)

# Plot out the line chart
ggplot(austin_2014_2016, aes(x = day, y = TempAvgF)) + 
  geom_line(aes(color = year, linetype = year), size=2) +
  labs(x = "Day of the year",y = "Average temperature") +
  scale_color_manual(values=c("#00AFBB", "#9999CC", "#990000"))
```




## 10. Example 2

Please try out the example below yourself. 

Using the `gss_sm` dataset, generate a bar chart showing percentage of religious category by four big regions?

First, let's generate new column called `percentage`, which is the percentage (pct) for each religious category, still grouped by region. Round the results to the nearest percentage point. Pipes are recommended here. Let'e call the new dataset `rel_by_region`.  

Execute the script below, and examine the new dataset `rel_by_region`. 

```r
rel_by_region <- gss_sm %>%
    group_by(bigregion, religion) %>%
    summarize(N = n()) %>%
    mutate(freq = N / sum(N),
           pct = round((freq*100), 0))
```

Then we can generate a bar chart. Note here we set up `stat="identity"` within geom_bar means the height of the bar should represent the magnitude of the variable mapped to the y-axis. 


```r
ggplot(rel_by_region, aes(x = bigregion, y = pct, fill = religion))+
  geom_bar(position = "dodge2", stat = "identity") +
  labs(x = "Region",y = "Percent", fill = "Religion") 
```




# Summary: Elements of Grammar of graphics

Here is the conclusion of the grammar of `ggplot()` function covered today:

Component |	Description   
---------| -------------
Data	 | The data-set being plotted   
Aesthetics |	The scales onto which we plot our data   
Geom |	Graphical representations of the data in the plot (points, lines, bars) 
Facet |	Split one plot into multiple plots based on a factor included in the dataset 

If you would like to explore more on `ggplot()` function, click [here](https://rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf) to find a cheat sheet!


# Work flow:
Here are the typically steps for using ggplot for data visualization. 

1) data preps: get your data clean and tidy before moving forward. 
2) start with ggplot object: initiate a ggplot object. 
3) add geoms: Be clear what type of figure is needed for your data
3) adjust other elements: legend, color, title etc. 
