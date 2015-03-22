# Getting_and_Cleaning_Data
Title: README to explain what run_analysis.R does

Author: Tracy Shen

Date: Sunday, March 22nd, 2015

This README.md file is composed to describe how the run_analysis.R script works based on the requirement from the course project: https://class.coursera.org/getdata-012/human_grading/view/courses/973499/assessments/3/submissions. As far as study design and variable explanation, please refer to the CodeBook.md file also included in the Getting_and_Cleaning_Data directory. 

==Preparation==

In order to run the script "run_analysis.R", you need to have some preparation. They are (based on what the author personally used):

R Version: R version 3.1.3 (2015-03-09) -- "Smooth Sidewalk" (Other Version should work as well)

Platform: Windows 7 professional x86_64-w64-mingw32/x64 (64-bit) (Mac or other platforms should work as well)

RStudio: RStudio-0.98.1091. It's an R console to write and run R codes more intuitively  (Other version should work as well).

R Packages: dplyr and gdata additional packages are required to install in order to run the codes. 

Raw Data: it should be saved in your working directory and can be downloaded from here: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip
It includes the below files:(The inertial Signal files are not used to run this script)
- 'README.txt'

- 'features_info.txt': Shows information about the variables used on the feature vector.

- 'features.txt': List of all features.

- 'activity_labels.txt': Links the class labels with their activity name.

- 'train/X_train.txt': Training set.

- 'train/y_train.txt': Training labels.

- 'test/X_test.txt': Test set.

- 'test/y_test.txt': Test labels.

Text editor: Notepad++ is used to view and observe the data that originally came in as ".txt" file. It's opted than Notepad becaused it can provide better experience of viewing subject and activity as well as the feature file. 


==WorkFlow==

Once you have the tools and raw data at handy, we can start running the run_analysis.R script. The workflow is referenced to the project instruction here in 5 distinct steps. https://class.coursera.org/getdata-012/human_grading/view/courses/973499/assessments/3/submissions

1.Merges the training and the test sets to create one data set.

2.Extracts only the measurements on the mean and standard deviation for each measurement. 

3.Uses descriptive activity names to name the activities in the data set

4.Appropriately labels the data set with descriptive variable names. 

5.From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

Note: As the project itself didn't speficify if students should follow the steps strictly to get the outcome, the run-analysis.R codes completed steps in a order of Step 1,3,2,4,5. Below is the details:

Step1: Merges the training and the test sets to create one data set

Read the "X_train.txt", "X_test.txt", "features.txt" file using read.table() into R and save them in the objects "X_test", "X_train" and "feature" respectively. As "X_train.txt", "X_test.txt" come with no column names, assign features to both the files as column names. Merge the two data sets using rbind() into object "rawdata". For the convenience of carrying out other steps, subject files are read into R here and saved in the objects " subject1", "subject2".They are then merged into object "joint" and have their column renamed as "subject". To verify correct outcome: you should get a 10299 * 561 data sets with column names as features. 

step3: Uses descriptive activity names to name the activities in the data set

Read the activity code files "y_test.txt", "y_train.txt" and save them under the objects "nact1", nact2".Combine them together to be one dataset named "activity". Per the project request, we need to substitute codes for descriptive names which provided by the "activity_lables.txt" file. That is "1=walking" , "2=walking_upstairs", "3=walking_downstairs","4=sitting","5=standing", "6=laying". This has been achieved via a code string (e.g.) 'activity$V1[activity$V1=="1"]<-"walking"'. After it's done, rename the column as "activity" and combine the columns of "joint" and "activity" and "rawdata" together and create a new dataset called "newdata".They should be in 10299 rows and 563 columns. 

Step2:Extracts only the measurements on the mean and standard deviation for each measurement.

Install and load packages {dplyr} and {gdata}. After observation using Notepad++, you will find the naming convention in the feature.txt file is bit messy. There are names such as "303 fBodyAcc-bandsEnergy()-1,8","304 fBodyAcc-bandsEnergy()-9,16" etc. In R, those names will be treated as duplicates.Therefore, make.names() is used to remove the duplicated and clean up the name pattern. e.g.the name "fBodyGyro-bandsEnergy()-17,32" becomes "fBodyGyro.bandsEnergy.17.32". Dots have been adopted to be the major naming convention as it's allowed in R according to the R documentation here: https://stat.ethz.ch/R-manual/R-devel/library/base/html/make.names.html. 

