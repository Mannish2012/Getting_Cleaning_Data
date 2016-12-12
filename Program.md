##Reading Features and Activity-Labels 

features <- read.csv("features.txt", sep = "", header = FALSE)
activity_labels <- read.csv("activity_labels.txt", sep = "", header = FALSE )

## Reading Sets

testSet <- read.csv("X_test.txt", sep = "", header = FALSE)
trainSet <- read.csv("X_train.txt", sep = "", header = FALSE)
r_bound_Set <- rbind(testSet,trainSet)

## Reading Movement

testMoves <- read.csv("y_test.txt", sep = "", header = FALSE)
trainMoves <- read.csv("y_train.txt", sep = "", header = FALSE)
r_bound_Moves <- rbind(testMoves, trainMoves)


## Reading Person ID 

testPerson <- read.csv("subject_test.txt", sep = "", header = FALSE)
trainPerson <- read.csv("subject_train.txt", sep = "", header = FALSE)
r_bound_Person <- rbind(testPerson, trainPerson)

#Descriptive Activity


r_bound_Moves1 <- merge(r_bound_Moves, activity_labels, by.x = "V1", by.y = "V1")[2]
new_merge <- bind_cols(r_bound_Moves1,r_bound_Person,r_bound_Set)
names(new_merge)[1:2] = c("Activities", "Person")


#Matching

match_mean.std <- grep("mean()|std()", features[,2]) # %>% as.data.frame %>% t 

#Cleaning 

group_by(new_merge[match_mean.std], Activities, Person) %>% summarize_each(funs(mean)) %>% unique
