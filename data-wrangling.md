Data Wrangling with R
======================================
author: Jeho Park
date: March 5, 2021
autosize: true

QCL Workshop Participation Requirements: 
========================================================
Those of you attending this workshop as part of requirements for your research group, student employment, or fellowship position, you must attend the workshop fully, do all the hands-on exercises plus homework to be qualified. 

If you have to leave in the middle of the workshop, please leave a note on the chat for the record.

Recommendations for Engagement:  
(1) Use your camera to show your attention  
(2) Use gestures such as nodding, thumbs-up, and raising hand to signal your understanding/misunderstanding  
(3) Unmute yourself to ask questions *anytime* 

Workshop Environment Setup
====================================
### 1. Log in to RStudio Cloud
### 2. Create a new project -->  Clone the workshop files from my GitHub Repo (i.e., "New Project from Git Repo")
### 3. Open data-wrangling.Rpres
### 4. Open QCL-R-Workshop-L2-Hands-On.Rmd

    
*GitHub repo: https://github.com/CMC-QCL/R-Data-Wrangling-L2-Workshop.git

Agenda: Data Wrangling with R
====================================
- What is data wrangling?
- Built-in datasets
- Simple data examination and exploration
- Simple data visualization
- Data manipulation using dplyr package
- Data import (later in hands-on)
- Hands-On: 
  - Find information from a "birth2015" dataset; 
  - Create a barplot showing COVID-19 daily new cases in the U.S.

What is Data Wrangling?
======================================
<img src="./data-wrangling-figure/wrangling-nat-geo.jpg" title="Data Wrangling" alt="Data Wrangling" width="70%" style="display: block; margin: auto;" />
  
Source: Wrangling Wild Horses in the Mountains of Montana (Director: Kristopher Rey-Talley) | Short Film Showcase | National Geographic Channel on Youtube (https://youtu.be/vkBtEe-lieU?t=131) | clip from 2:11 to 3:11

What is Data Wrangling?
=============================================
Data wrangling is the process of obtaining, cleaning, reshaping and transforming raw (and messy) data into a usable form of processed (and tidy) data.

![Data Wrangling](./data-wrangling-figure/Data_Wrangling_with_R.png)  
Source: "Data Wrangling with R" by Bradley C. Boehmke | Use R! Series

CO2 Dataset from "datasets" Package
====================================
- The CO2 data frame has 84 rows and 5 columns of data from an experiment on the cold tolerance of the grass species Echinochloa crus-galli.

```r
help(CO2) # see what the dataset is about
CO2 # display all the contents of the data frame, CO2
View(CO2)
```

- The `datasets` package contains several useful toy datasets. 
- Try `data()` from the console

A Quick Look of a Dataset (data.frame)
========================================================
Here are some simple and handy functions for everyday use.

```r
head(CO2) # show the first few observations
tail(CO2, 1) # show the last observation
summary(CO2) # this is a very handy function!
str(CO2) # show the structure of the data.frame
names(CO2) # show the variable names
```

Data Visualization
========================================
- Basic plots such as histogram, box plot, and scatter plot are within a few key strokes away (type `his` in the console and wait for a sec)


```r
hist(CO2$uptake) # Use help function to check its arguments
boxplot(CO2$uptake ~ CO2$conc)
plot(x = CO2$conc, y = CO2$uptake)
```
**Note that we used '$' to access (or extract) a variable (or a column) of a dataframe. 

Data Manipulation using dplyr
===========================================
<img src="./data-wrangling-figure/dplyr.png" alt="dplyr" width="500"/>

Data Manipulation using dplyr
===========================================
- `dplyr` is the most popular package for data exploration and transformation
- `dplyr` includes 
   - `filter` 
   - `select` 
   - `arrange` 
   - `mutate`
   - `summarise` 
   - `group_by`
   

Data Manipulation using dplyr
===========================================
- `dplyr` is the most popular package for data exploration and transformation
- `dplyr` includes 
   - `filter` picks variables based on their values.
   - `select` picks cases based on their names.
   - `arrange` changes the ordering of the rows.
   - `mutate` adds new variables that are functions of existing variables.
   - `summarise` reduces multiple values down to a single summary.
   - `group_by` performs any operation by group.

## Check if you have the `dplyr` package installed and loaded. If not installed, install `tidyverse` package which includes `dplyr` and other useful packages such as `ggplot2`.

Data Manipulation using dplyr::filter
========================================
- `filter` with a logical operator on a value will filter in/out those observations (rows) with the value.


```r
# Logical Operators in R
5 < 2
(5 < 2) & (3 > 2)
5 %in% c(1,2,3,4,5)
is.na(c(1,2,NA,4,5))
```

- `filter` manipulates observations (rows).


```r
filter(CO2, Treatment=='chilled')
```

Hands-On 1
=========================================
Which group of plants has a higher average uptake rate, chilled plants or nonchilled plants? 


```r
#_FILL-IN_# # your work here
```

Data Manipulation using dplyr::select
===========================================
- `select` picks cases based on their names.
- `select` manipulates variables (columns).

The following shows the first 6 observations containing only Plant and uptake variables from the CO2 data.frame:

```r
head(select(CO2, Plant, uptake))
```

Hands-On 2
===================================================
Create a new data.frame, `x`, with two variables, Plant and uptake, containing only non-chilled plant cases from CO2 dataset.


```r
#_FILL-IN_# # your work here
```

Data Manipulation - Chaining using %>%
========================================================
- You can chain dplyr functions together using a special looking operator called a pipe operator: `%>%` 
- The pipe operator feeds the resulting object into the 1st argument of the next function.
- The keyboard shortcut is Ctrl + Shift + M (Windows) or Cmd + Shift + M (Mac).

For example,

```r
CO2 %>% 
  filter(Treatment=='nonchilled') %>% 
  select(Plant, uptake) %>% 
  head()
```

Compare that with the following:

```r
head(select(filter(CO2, Treatment=='nonchilled'), Plant, uptake))
```

Hands-On 3
===================================================
Now let's try chaining (piping) to create a new data.frame, `y`, with two variables, Plant and uptake, containing only non-chilled plant cases from CO2 dataset. And then compare `x` you created in the previous hands-on and `y` to see if they are indeed the same.


```r
#_FILL-IN_# # your work here
```


Data Manipulation using dplyr::arrange
========================================================
- `arrange` sorts observations (rows) by a variable (column) in ascending order
- `arrange` manipulates the order of observations.

What are the three lowest CO2 uptake cases?

```r
CO2 %>% arrange(uptake) %>% head(3) # ascending order is default
```

Hands-On 4
====================================================
What are the three highest CO2 uptake cases?

```r
#_FILL-IN_# # your work here
```


Data Manipulation using dplyr::mutate
========================================================
- `mutate()` creates new variables and adds it to the end of the data.frame.
- `mutate` manipulates variables (columns).

Create a new variable `conc_L` containing concentration values divided by 1000

```r
CO2 %>% mutate(conc_L = conc / 1000)
```

Data Manipulation using dplyr::summarise and dplyr::group_by
========================================================
- `summarise` applies a summary function (e.g., mean, sum, etc.) and returns a result.
- To aggregate variables, `group_by` and `summarise` work together as follows:

Aggregate `CO2` into average values of uptake by plant type:

```r
CO2 %>% 
  group_by(Type) %>% 
  summarise(avg = mean(uptake)) 
```

Data Manipulation - Pro Tips
========================================================
- In datasets with a lot of variables, you can use select function to exclude certain variables by using the '-' operator. For example, try `select(CO2, -Plant)`
- When aggregating data, you need an aggregate function whose result is a single value such as mean, median, n (number of rows in a group), sum, etc.
- If your data have missing values AND you want to exclude them, you need to add the parameter `na.rm = TRUE`. _Caution!_ This will skip any observation having a missing value. _Caution2!_ Sometimes missing values are meaningful. So do not simply exclude them in your data manipulation stage.
- When working with data, the data manipulation process often takes more time than the analysis.

End of Session I
=====================
You've learned:
- data wrangling 
- a couple of simple ways to examine your dataset
- a couple of data visualizations
- various ways to manipulate data using dplyr's `filter`, `select`, `arrange`, `mutate`, `summarise`, and `group_by` function.

Session II
======================
For the rest of the workshop, you will be working on a couple of hands-on exercises. If you cannot finish them all in time, you are welcome to finish it after the workshop. A QCL staff can help you remotely and synchronously via Zoom or asynchronously via emails.

Hands-On Exercise 1 (Difficulty: low)

Hands-On Exercise 2 (Difficulty: medium-high)


Hands-On Exercise 1 (Difficulty: low)
========================================================
First, import "Births2015.csv" from GitHub as a data.frame `births2015` to answer the questions below:

1. What are the variables of `births2015` data.frame?
2. What is the total number of babies born in 2015?
3. Make a histogram of number of births.  
4. How many babies were born on Wednesdays? (must use the pipe operator)
5. Which `date` had the least amount of babies born?

The Births2015 CSV file is at https://raw.githubusercontent.com/jehopark/data_wrangling_with_r_beginners/master/Births2015.csv

To import a CSV data file from the Internet:





```
Error in library(readr) : there is no package called 'readr'
```
