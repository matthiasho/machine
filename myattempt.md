
Upon close scrutiny of the training data found https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv, 
I realize that there are a lot of columns with the value "NA".  

As part of basic preprocessing, I therefore proceed to remove these columns.  

Next, I removed all the columns that are timestamps.  My reasoning is that we should not be concerned about how much or how long (i.e. timestamp2 - timestamp 1) of a particular activity people perform do, but they rather how well they do it. Hence no features with time measurement (*raw_timestamp_part_1*, *raw_timestamp_part_2* and *cvtd_timestamp* etc) are included.  

Lastly, the ids and user_name columns were also removed, as I am not of the opinion that they should be useful in training and testing the model.  Consequently only 53 features were used to train the model.

The training data of 19622 observations were sub split into 70% of sub-training set and 30% of sub-testing data set.  Therefore 13737 observations were used to train the model.
To obtain the prediction for the 20 test cases, I eventually settled for the random forest, method ="rf" provided by the caret package.


```

library(caret)

set.seed(44542)

health <- read.table("c:/harvest/pml-training-cleansed.csv", header=T, sep=",") 

inTrain  <- createDataPartition(y=health$classe,p=0.70,list=FALSE)
training <-health[inTrain,]
testing  <-health[-inTrain,]

dim(training) # checking only
dim(testing) #checking only


ctrl = trainControl(method = "cv", number = 6)

modelFit <- train(classe ~.,data=training,method="rf",trControl = ctrl) # for health example
modelFit
modelFit$finalModel

exercise <- read.table("c:/harvest/pml-testing-cleansed.csv", header=T, sep=",") 

predictions <- predict(modelFit,newdata=exercise)
predictions

```




The 20 correct predictions that I submitted for part II are:

B A B A A E D B A A B C B A E E A B B B


