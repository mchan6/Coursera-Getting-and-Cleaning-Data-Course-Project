# Getting and Cleaning Data Project CookBook


# The Code Book

Below are descriptions of the variables, the data, and any transformations or work that I performed to clean up the data and produce a tidy dataset.

# The Data
The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone.

Link to the data description is here: http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

Here are the data for the project: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

###Dataset Description:

The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain.

# The Variables 

###For each record in the dataset it is provided: 
  1. Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration. 
  
  2. Triaxial Angular velocity from the gyroscope. 

  3. A 561-feature vector with time and frequency domain variables. 
  
  4. Its activity label. 
  
  5. An identifier of the subject who carried out the experiment.

# Additional Transformations

The script run_analysis.R performs the 5 steps below to merge and subset the data into the average of each variable for each activity and each subject.

###1. Merges the training and the test sets to create one data set.
Uses the Rbind() function to combine the test,activity, and features data from our extraction.

###2. Extracts only the measurements on the mean and standard deviation for each measurement. 
Subset data to only those with ID, mean and stdev columns. 

###3. Uses descriptive activity names to name the activities in the data set
I assign the activities in the dataset using activity names from activity_labels text file.

###4. Appropriately labels the data set with descriptive variable names. 
Some of the variables are renammed to make more streamline. These include replacing 'Acc' with 'Accelerometer', 'Gyro' with 'Gyroscope', and 'Mag' with 'Magnitude'. This is done with gsub() function.

###5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
A tidy dataset is finally produced with the average of each veriable for each activity and subject.






