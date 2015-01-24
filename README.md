# Getting and Cleaning Data Project

### Initial Assignment

>The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected. 
>
>One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained:
>
>http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones
>
>Here are the data for the project:
>
>https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip
>
> You should create one R script called run_analysis.R that does the following. 
>
>    Merges the training and the test sets to create one data set.
>    Extracts only the measurements on the mean and standard deviation for each measurement. 
>    Uses descriptive activity names to name the activities in the data set
>    Appropriately labels the data set with descriptive variable names. 
>
>    From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
>
>Good luck!


### Description of original dataset
1. The initial ZIP file should be downloaded into the working directory for RStudio and un-zipped there manually. 
2. The ZIP file will produce "UCI HAR Dataset" folder containing:
   + 2 subfolders of Test and Train datasets
   + README.txt - detailed information on UCI HAR dataset
   + activity_labels.txt - description of activities performed 
   + features_info.txt - detailed information on features/measurements used
   + features.txt - list of the actual features
3. For the purposes of this project, the files in the Inertial Signals folders are not needed.
4. Test and Train subfolders contains 3 data files each
```    
test/subject_test.txt
test/X_test.txt
test/y_test.txt
``` 
```
train/subject_train.txt
train/X_train.txt
train/y_train.txt
```
   Where **subject** files identify the subject/person who performed the activity, **X_** files are the actual measurements and **Y_** files are activity label IDs, which will be linked to activity names by merging with *activity_labels*

### Preparation
The script uses the following libraries: "data.table" and "dplyr". If they are not present, they need to be installed by running the following code in R:
```{r}   
   install.packages("data.table")
   library(data.table)
   install.packages("dplyr")
   library(dplyr)
```   

### Description of how the script works
The script run_analysis.R does the following:
   1. Reads initial files to R global environment
   2. Combines rows of each Train and Test file sets
   3. Combines columns to create one raw dataset 
   4. Reads the features file containing variable names for all measurements
   5. Assigns names to columns in the combined raw dataset, which are taken from the features file
   6. Selects the subset of the raw dataset containing only measurements on the mean or standard deviation
   7. Reads the activity labels file to merge it with the subset from the above step to produce activity description variable instead of ID 
   8. Removes the numeric activity label ID since we now have the actual activity description
   9. Makes variable names descriptive and adjusts them, removing reserved R symbols like "-" or "()"
   10. Calculates the average for each variable using "aggregate" function grouping by subject and activity, and excluding subject and activity from calculation using dot notation
   12. Saves ordered tidy dataset to tidydata.txt file

The script can be executed from the working directory:
```{r}
source("run_analysis.R")
```
To view the result:
```{r}
View(tidydata)
```