# Getting and Cleaning Data - Course Project

This read me document shows you step by step how to extract the raw data and merge and finally build a today dataset for this assignment.

## Part 1: Merge the training and the test sets to create one data set.

First, download the zip file.


```r
##Read zip mydata
setwd("/Users/mchan/desktop/Coursera")
if(!file.exists("./mydata")){dir.create("./mydata")}
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl,destfile="./mydata/Dataset.zip",method="curl")
unzip(zipfile="./mydata/Dataset.zip",exdir="./mydata")
```

After downloading the zip file, here I set the file path from the downloaded zip.


```r
## set file path from downloaded zip
path_files <- file.path("./mydata" , "UCI HAR Dataset")
files<-list.files(path_files, recursive=TRUE)
```


Next, I extract all the files into the set path.


```r
## Read each file after extraction
subject_train_data <- read.table(file.path(path_files, "train", "subject_train.txt"),header = FALSE)
subject_test_data  <- read.table(file.path(path_files, "test" , "subject_test.txt"),header = FALSE)
Y_test_data  <- read.table(file.path(path_files, "test" , "Y_test.txt" ),header = FALSE)
Y_train_data <- read.table(file.path(path_files, "train", "Y_train.txt"),header = FALSE)
X_test_data  <- read.table(file.path(path_files, "test" , "X_test.txt" ),header = FALSE)
X_train_data <- read.table(file.path(path_files, "train", "X_train.txt"),header = FALSE)
features_dataN <- read.table(file.path(path_files, "features.txt"),head=FALSE)
```

Finally, below I merge the training and test sets into one data set using rbind()


```r
## Merge the training and test sets to create one data set
subject_data <- rbind(subject_train_data, subject_test_data)
activity_data<- rbind(Y_train_data, Y_test_data)
features_data<- rbind(X_train_data, X_test_data)

names(features_data)<- features_dataN$V2

names(subject_data)<-c("subject")
names(activity_data)<- c("activity")

combine_data <- cbind(subject_data, activity_data)
mydata <- cbind(features_data, combine_data)
```



## Part 2: Extracts only the measurements on the mean and standard deviation for each measurement. 

Below, I extract only the mean the std data and update the dataset with just these observations.


```r
##extracts only mean and std and subsets to updates dataset
subfeatures_dataNames<-features_dataN$V2[grep("mean\\(\\)|std\\(\\)", features_dataN$V2)]

selectedNames<-c(as.character(subfeatures_dataNames), "subject", "activity" )
mydata<-subset(mydata,select=selectedNames)
```



## Part 3: Uses descriptive activity names to name the activities in the data set.

Below I update the activities column in my dataset with the description contained in the activity_label text file.


```r
##Uses descriptive activity names to name the activities in the data set
activityLabels <- read.table(file.path(path_files, "activity_labels.txt"),header = FALSE)
mydata[, 68] <- activityLabels[mydata[, 68], 2]
```



## Part 4: Appropriately labels the data set with descriptive variable names. 

Below I do some manual fixes to easier describe the variable names.


```r
##Appropriately labels the data set with descriptive variable names using gsub()
names(mydata)<-gsub("Acc", "Accelerometer", names(mydata))
names(mydata)<-gsub("BodyBody", "Body", names(mydata))
names(mydata)<-gsub("Gyro", "Gyroscope", names(mydata))
names(mydata)<-gsub("Mag", "Magnitude", names(mydata))
names(mydata)<-gsub("^t", "time", names(mydata))
names(mydata)<-gsub("^f", "frequency", names(mydata))
```




## Part 5: From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

Below using hte plyr package, I create a tidy dataset and output to a .txt file.


```r
##Creates a second,independent tidy mydata set and ouput it
library(plyr);
mydata2<-aggregate(. ~subject + activity, mydata, mean)
mydata2<-mydata2[order(mydata2$subject,mydata2$activity),]
write.table(mydata2, file = "tidy_averages.txt",row.name=FALSE)
```







