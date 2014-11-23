---
title: "ReadME"
author: "Chendur Anand"
date: "Sunday, November 23, 2014"
output: html_document
---

The second column of the features file was converted to "character" data type. All the mean and standard deviation features are subsetted using pattern. The variable (Activity) in y_Train and y_Test are converted to factors. Then these factor levels are substituted with the 'Activity' names and then column bound to the respective 'X' data sets.
  The variable from the subject data set is converted to a factor variable and added to the train and test sets respectively. Then the train and test sets are row bound. The subsetted features data set is used to subset the mean and standard deviation variables from the combined data set.
  The column names are added to the data set and the data set is ordered based on activity and subject. The tidy data set is a table of activities with the subjects as variables. Then the column means will be the Subject means and the row means will the activity means.
  This wide data set is then written to a file named 'Tidy_Data.txt' as a comma seperated data set.

```{r}
```{r}
#Read the files
xTrain <- read.table("~/Misc/Course_Era/Data_Science_Specialization/3_Getting_and_Cleaning_data/R/Assignment/UCI HAR Dataset/train/X_train.txt", quote="")
yTrain <- read.table("~/Misc/Course_Era/Data_Science_Specialization/3_Getting_and_Cleaning_data/R/Assignment/UCI HAR Dataset/train/y_train.txt", quote="")
xTest <- read.table("~/Misc/Course_Era/Data_Science_Specialization/3_Getting_and_Cleaning_data/R/Assignment/UCI HAR Dataset/test/X_test.txt", quote="\"")
yTest <- read.table("~/Misc/Course_Era/Data_Science_Specialization/3_Getting_and_Cleaning_data/R/Assignment/UCI HAR Dataset/test/y_test.txt", quote="\"")
features <- read.table("~/Misc/Course_Era/Data_Science_Specialization/3_Getting_and_Cleaning_data/R/Assignment/UCI HAR Dataset/features.txt", quote="\"")
subject_train <- read.table("~/Misc/Course_Era/Data_Science_Specialization/3_Getting_and_Cleaning_data/R/Assignment/UCI HAR Dataset/train/subject_train.txt", quote="\"")
subject_test <- read.table("~/Misc/Course_Era/Data_Science_Specialization/3_Getting_and_Cleaning_data/R/Assignment/UCI HAR Dataset/test/subject_test.txt", quote="\"")

#Subset mean and standard deviation column names from features file
features[,2] <- as.character(features[,2])
features2 <- subset(features, grepl("mean()", features[,2]))
features3 <- subset(features, grepl("std()", features[,2]))
ft <- rbind(features2, features3)
ft <- ft[order(ft$V1),]

yTrain[,1] <- as.factor(as.character(yTrain[,1]))
yTest[,1] <- as.factor(as.character(yTest[,1]))

#Step 3: Use descriptive activity names
levels(yTrain[,1]) <- c("WALKING", "WALKING_UPSTAIRS", "WALKING_DOWNSTAIRS", "SITTING", "STANDING", "LAYING")
levels(yTest[,1]) <- c("WALKING", "WALKING_UPSTAIRS", "WALKING_DOWNSTAIRS", "SITTING", "STANDING", "LAYING")
Train <- cbind(xTrain, yTrain)
Test <- cbind(xTest, yTest)
names(Train)[562] <- "Activity"
names(Test)[562] <- "Activity"
Train$Subject <- as.factor(as.character(subject_train[,1]))
Test$Subject <- as.factor(as.character(subject_test[,1]))


#Step 1: Merge Training and Test dataset
data <- rbind(Train, Test)
data <- data[order(data$Activity, data$Subject),]

#Step 2: Extract only the measurements on mean and standard deviation
# using the subsetted features file
data2 <- subset(data, select=ft[,1])
data3 <- subset(data, select = 562:563)
data <- cbind(data3, data2)

#Step 4: Label the variable names
i = 3
for(i in 3:ncol(data)){
  names(data)[i] <- ft[i-2,2]
}

data <- data[order(data$Subject, data$Activity),]

#Step 5:  independent tidy data set with the average of each variable
# for each activity and each subject
tidyData <- as.data.frame.matrix(table(data$Activity, data$Subject))
tidyData <- tidyData[,order(names(tidyData))]
tidyData[7,] <- colMeans(tidyData)
tidyData[,31] <- rowMeans(tidyData)
tidyData[,32] <- row.names(tidyData)
row.names(tidyData)[7] <- "Subject Average"
names(tidyData)[31:32] <- c("Activity Average", "Activity")
tidyData <- tidyData[c(32, 1:31)]
is.num <- sapply(tidyData, is.numeric)
tidyData[is.num] <- lapply(tidyData[is.num], round, 2)

#Write to a file
write.table(tidyData, file = "Tidy_Data.txt", sep = ",", row.names = FALSE)
```
```