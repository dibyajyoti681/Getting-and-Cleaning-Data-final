#Download the file and put the file in the dataforanalysis folder

if(!file.exists(“C:/Users/Dibyajyoti/Documents/dataforanalysis”){dir.create(“C:/Users/Dibyajyoti/Documents/dataforanalysis”)} fileUrl <- “https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip” download.file(fileUrl,destfile=“C:/Users/Dibyajyoti/Documents/dataforanalysis/Dataset.zip”) #2.Unzip the file unzip(zipfile=“C:/Users/Dibyajyoti/Documents/dataforanalysis/Dataset.zip”,exdir=“C:/Users/Dibyajyoti/Documents/dataforanalysis”) #3.unzipped files are in the folderUCI HAR Dataset. Get the list of the files path_rf <- file.path(“./dataforanalysis” , “UCI HAR Dataset”) files<-list.files(path_rf, recursive=TRUE) #files dataActivityTest <- read.table(file.path(path_rf, “test” , “Y_test.txt” ),header = FALSE) dataActivityTrain <- read.table(file.path(path_rf, “train”, “Y_train.txt”),header = FALSE) dataSubjectTrain <- read.table(file.path(path_rf, “train”, “subject_train.txt”),header = FALSE) dataSubjectTest <- read.table(file.path(path_rf, “test” , “subject_test.txt”),header = FALSE) dataFeaturesTest <- read.table(file.path(path_rf, “test” , “X_test.txt” ),header = FALSE) dataFeaturesTrain <- read.table(file.path(path_rf, “train”, “X_train.txt”),header = FALSE)

#1.Concatenate the data tables by rows

dataSubject <- rbind(dataSubjectTrain, dataSubjectTest) dataActivity<- rbind(dataActivityTrain, dataActivityTest) dataFeatures<- rbind(dataFeaturesTrain, dataFeaturesTest)

#2.set names to variables

names(dataSubject)<-c(“subject”) names(dataActivity)<- c(“activity”) dataFeaturesNames <- read.table(file.path(path_rf, “features.txt”),head=FALSE) names(dataFeatures)<- dataFeaturesNames$V2

#3.Merge columns to get the data frame Data for all data

dataCombine <- cbind(dataSubject, dataActivity) Data <- cbind(dataFeatures, dataCombine)

#Subset Name of Features by measurements on the mean and standard deviation #i.e taken Names of Features with “mean()” or “std()”

subdataFeaturesNames<-dataFeaturesNames$V2[grep(“mean|std”, dataFeaturesNames$V2)]

#Subset the data frame Data by seleted names of Features selectedNames<-c(as.character(subdataFeaturesNames), “subject”, “activity” ) Data<-subset(Data,select=selectedNames)

#1.Read descriptive activity names from “activity_labels.txt”

activityLabels <- read.table(file.path(path_rf, “activity_labels.txt”),header = FALSE)

##Appropriately labels the data set with descriptive variable names #In the former part, variables activity and subject and names of the activities have been labelled using descriptive names.In this part, Names of Feteatures will labelled using descriptive variable names. #descriptive names according to activity_labels. Data$activity[Data$activity=='1'] <- 'WALKING' Data$activity[Data$activity=='2'] <- 'WALKING_UPSTAIRS' Data$activity[Data$activity=='3'] <- 'WALKING_DOWNSTAIRS' Data$activity[Data$activity=='4'] <- 'SITTING' Data$activity[Data$activity=='5'] <- 'STANDING' Data$activity[Data$activity=='6'] <- 'LAYING'

##prefix t is replaced by time #Acc is replaced by Accelerometer #Gyro is replaced by Gyroscope #prefix f is replaced by frequency #Mag is replaced by Magnitude #BodyBody is replaced by Body names(Data)<-gsub(“t”, “time”, names(Data)) names(Data)<-gsub(“f”, “frequency”, names(Data)) names(Data)<-gsub(“Acc”, “Accelerometer”, names(Data)) names(Data)<-gsub(“Gyro”, “Gyroscope”, names(Data)) names(Data)<-gsub(“Mag”, “Magnitude”, names(Data)) names(Data)<-gsub(“BodyBody”, “Body”, names(Data)) #Creates a second,independent tidy data set and ouput it #In this part,a second, independent tidy data set will be created with the average of each variable for each activity and each subject based on the data set in step 4.

library(plyr)

Data2<-aggregate(. ~subject + activity, Data, mean) Data2<-Data2[order(Data2$subject,Data2$activity),] write.table(Data2, file = “tidydata.txt”,row.name=FALSE)

