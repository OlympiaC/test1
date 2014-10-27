#1 set up wd and install packages needed for this assignment
#2 install httr, plyr,rcurl, tidycurl, swirl
#Setwd 
setwd("C:/coursera/UCI HAR Dataset")
install.packages("httr")
install.packages("plyr")
install.packages("rcurl")
install.packages("tidyr")
install.packages ("swirl")
#Part 1 
## Step 1 - Merge the Training and Test Sets
train <- read.table("train/X_train.txt") 
        test <- read.table("test/X_test.txt") 
                X <- rbind(train, test) 
                train2 <- read.table("train/subject_train.txt") 
        test2 <- read.table("test/subject_test.txt") 
                Sub <- rbind(train2, test2) 
        train3 <- read.table("train/y_train.txt") 
        test3 <- read.table("test/y_test.txt") 
        Y <- rbind(train3, test3)
#Step 2 
features <- read.table("features.txt") 
featureindices <- grep("-mean\\(\\)|-std\\(\\)", features[, 2]) 
X <- X[, featureindices] 
names(X) <- features[featureindices, 2] 
names(X) <- gsub("\\(|\\)", "", names(X)) 
names(X) <- tolower(names(X))
#Step 3
activities <- read.table("activity_labels.txt") 
activities[, 2] = gsub("_", "", tolower(as.character(activities[, 2])))
Y[,1] = activities[Y[,1],2names(Y) <- "activity"
#Step 4 and 5
names(Sub) <- "subject" 
cleantraintest <- cbind(Sub, Y, X) 
write.table(cleantraintest, "tidy_data.txt") 
uniqsubjects = unique(Sub)[,1] 
numbersubjects = length(unique(Sub)[,1]) 
numberactivities = length(activities[,1]) 
numbercols = dim(cleantraintest)[2] 
final = cleantraintest[1:(numbersubjects*numberactivities), ]
count = 1 for (c2 in 1:numbersubjects) 
        {     for (c3 in 1:numberactivities) 
                {         final[count, 1] = uniqsubjects[c2]         
                          final[count, 2] = activities[c3, 2]        
                          tmp <- cleantraintest[cleantraintest$subject==c2 & cleantraintest$activity==activities[c3, 2], ]         
                          final[count, 3:numbercols] <- colMeans(tmp[, 3:numbercols])         
                          count = count+1     }     }
write.table(final, "data_with_averages.txt")
                   
