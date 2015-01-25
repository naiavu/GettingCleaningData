# Codebook for Getting and Cleaning Data Project

### Original Description
>Feature Selection 

>The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ. These time domain signals (prefix 't' to denote time) were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals (tBodyAcc-XYZ and tGravityAcc-XYZ) using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 

>Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag). 

>Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag. (Note the 'f' to indicate frequency domain signals). 

>These signals were used to estimate variables of the feature vector for each pattern:  
'-XYZ' is used to denote 3-axial signals in the X, Y and Z directions.

* tBodyAcc-XYZ
* tGravityAcc-XYZ
* tBodyAccJerk-XYZ
* tBodyGyro-XYZ
* tBodyGyroJerk-XYZ
* tBodyAccMag
* tGravityAccMag
* tBodyAccJerkMag
* tBodyGyroMag
* tBodyGyroJerkMag
* fBodyAcc-XYZ
* fBodyAccJerk-XYZ
* fBodyGyro-XYZ
* fBodyAccMag
* fBodyAccJerkMag
* fBodyGyroMag
* fBodyGyroJerkMag

>The set of variables that were estimated from these signals are: 
* mean(): Mean value
* std(): Standard deviation
* mad(): Median absolute deviation 
* max(): Largest value in array
* min(): Smallest value in array
* sma(): Signal magnitude area
* energy(): Energy measure. Sum of the squares divided by the number of values. 
* iqr(): Interquartile range 
* entropy(): Signal entropy
* arCoeff(): Autorregresion coefficients with Burg order equal to 4
* correlation(): correlation coefficient between two signals
* maxInds(): index of the frequency component with largest magnitude
* meanFreq(): Weighted average of the frequency components to obtain a mean frequency
* skewness(): skewness of the frequency domain signal 
* kurtosis(): kurtosis of the frequency domain signal 
* bandsEnergy(): Energy of a frequency interval within the 64 bins of the FFT of each window.
* angle(): Angle between to vectors.

>Additional vectors obtained by averaging the signals in a signal window sample. These are used on the angle() variable:
* gravityMean
* tBodyAccMean
* tBodyAccJerkMean
* tBodyGyroMean
* tBodyGyroJerkMean

>The complete list of variables of each feature vector is available in 'features.txt'

### Variables selected

The original data set contained 561 variables as combination of 3-axial signals with various patterns described above. As stated in the assignment we needed to extract only the measurements on the mean and standard deviation for each measurement. To accomplish this the **grep** function was used:
```{r}
featureSubset <- featureList$name[grep("mean\\(\\)|std\\(\\)", featureList$name)]
```
As the subset was extracted, the total of 66 variables from the original dataset contained measurements on the mean and standard deviation. 

Two more variables were added to this subset to hold the information on the subject and activity respectively, thus making the total number of variables as 68.

Then activity_labels file was merged with the subset from the above step in order to have the descriptive activity names in the file such as WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS,  SITTING, STANDING, LAYING. This increased the number of variables in the subset to 69 because it added the activity name variable.

Since we now have the actual activity description in the dataset, the numeric activity label ID was not needed and therefore removed bringing back the total number of variables to 68.

### Naming variables

The following online guides were reviewed to help make a decision on naming convention for this project: 

http://www.r-bloggers.com/consistent-naming-conventions-in-r/

https://google-styleguide.googlecode.com/svn/trunk/Rguide.xml

Since period separated variables are confusing for users of many programming languages where they may separate a property from an object; and because underscores were not recommended, the  lowerCamelCase was my choice of naming variables.

The following modifications were done on variable names of the original dataset to make them follow selected naming convention as well as be more understandable: 
* prefix t was replaced by time
* prefix f was replaced by frequency
* Acc was replaced by Accelerometer
* Gyro was replaced by Gyroscope
* Mag was replaced by Magnitude
* BodyBody was replaced by Body
* mean was replaced by Mean 
* std was replaced by STD

Also the round brackets () were removed from the variable names since they are often serve various purposes in most programming languages.

### Final Calculation and Output

To calculate the requested average of each variable for each activity and each subject, the "aggregate" function was used with dot notation, which is very convenient way to exclude from the calculation variables by which the data was grouped (e.g. subject and activity)
```{r}
tidyData <- aggregate(. ~subject + activity, rawDataSet, FUN = "mean")
```
Because there are 30 subjects and 6 activity types, and the mean was calculated for every combination of subject and activity, the final output file contains 180 observations and 68 variables as described above.

At the end the final dataset was ordered by subject and activity and saved into text file as requested.









