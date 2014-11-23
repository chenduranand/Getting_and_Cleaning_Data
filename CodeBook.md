---
title: "CodeBook"
author: "Chendur Anand"
date: "Sunday, November 23, 2014"
output: html_document
---

  The variables that are used for this analysis are any features that have mean() or std() from the features file. Also, the subjects and activities are used as factor variables. The train, test and subject data are transformed into a single data set. The activity data is transformed to have the activity name corresponding to the number. Then the activity is added to the data set ('data') as a variable.
  The features data set is transformed to contain only the observations that contain mean() or std(). This transformed features is used to transform the data set ('data') to contain only those variables that correspond to the observations in the features data set. This will result in a subset of data that contains only the mean and standard deviation variables and the variables are also named using the observations from the features data set.
  Then the data set transformed into a tidy data set which is a table that contains only the activity and subjects (as variables). The row and column means are added to the data set and are named 'Activity Mean' and 'Subject Mean' respectively.
  This wide data set is then written to a file named 'Tidy_Data.txt' as a comma seperated data set.


