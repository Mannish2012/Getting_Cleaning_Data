##Reading Features and Activity-Labels 
## We first read the 561 dimension feature vector and the activity labels

features <- read.csv("features.txt", sep = "", header = FALSE)
activity_labels <- read.csv("activity_labels.txt", sep = "", header = FALSE )

## Reading Sets
## We next read the data from the test and training sets, and then bind them using rbind(). These
are 561 column data, and they correspond to the features vector described earlier. Total number of rows in both sets is 10,299. 


testSet <- read.csv("X_test.txt", sep = "", header = FALSE)
trainSet <- read.csv("X_train.txt", sep = "", header = FALSE)
r_bound_Set <- rbind(testSet,trainSet)

## Reading Movement

##We next read the movement data from the test and training sets, and then bind them using rbind(). These are merely one-column data, and there are 6 factors. Total number of rows in both sets is 10,299. 


testMoves <- read.csv("y_test.txt", sep = "", header = FALSE)
trainMoves <- read.csv("y_train.txt", sep = "", header = FALSE)
r_bound_Moves <- rbind(testMoves, trainMoves)


## Reading Person ID 

## We next read the person data from the test and training sets, and then bind them using rbind(). These are merely one-column data, and there are 30 persons. Total number of rows in both sets is 10,299.

testPerson <- read.csv("subject_test.txt", sep = "", header = FALSE)
trainPerson <- read.csv("subject_train.txt", sep = "", header = FALSE)
r_bound_Person <- rbind(testPerson, trainPerson)

#Descriptive Activity

## This is a crucial activity because this is how we "connect" our movements vector, to the activities. Our composite movements vector, r_bound_Moves has only 6 unique elements, corresponding to the activities label, and these need to be connected. We create a new vector, r_bound_Moves1 for this purpose. Then, we merge this new vector to the other "r_bound" vectors, to create a new "merged" vector called "new_merge". The vector "new_merge" is a 10299 X 563 vector. 561 columns are taken by the features vector, and the additional first two columns belong to the activities label and person ID, respectively. We rename these columns as "Activities" and "Person" 

r_bound_Moves1 <- merge(r_bound_Moves, activity_labels, by.x = "V1", by.y = "V1")[2]
new_merge <- bind_cols(r_bound_Moves1,r_bound_Person,r_bound_Set)
names(new_merge)[1:2] = c("Activities", "Person")


#Matching

Using grep, we extract from the features vector those columns that correspond to mean() and std() respectively, as this is what our assignment requires us to do. This is the "match_mean.std" vector. This contains 79 columns corresponding to either mean() or std(). 


match_mean.std <- grep("mean()|std()", features[,2]) # %>% as.data.frame %>% t 



#Cleaning 

We subset our new_merge vector (10299 X 563) with our match_mean.std vector (1 X 79) to obtain a 10299 X 79 dataset. This is "new_merge[match_mean.std]" This is a the dataset that contains only the mean or standard deviation values that we want. Now, we need, for all persons, and for all activities, the means of the columns in this new dataset. To do that, we use the group_by tool from dplyr that groups the dataset in terms of Persons, and then Activities. Then, we use the summarize_each tool from dplyr to get a value of the means of the means and standard deviations that we got earlier. 

This is the new dataset that we need. 

group_by(new_merge[match_mean.std], Activities, Person) %>% summarize_each(funs(mean)) %>% unique
  
  
