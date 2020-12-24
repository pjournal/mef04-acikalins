
```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = FALSE)

library(rmarkdown)
library(rpart)
library(rpart.plot)
library(rattle)
library(caret)
library(tidyverse)
library(data.table)

data(diamonds)

```

Price Estimation Model on Diamonds Dataset

# A Glance at the Data

## Structure

Dataframe consists of both numeric and categorical variables
  
```{r str, echo=TRUE}
str(diamonds)
```

## Head

```{r head, echo=TRUE}
head(diamonds)
```

# Visual Inspection
## Preparing Data for ggplot
Since price will be response variable in the model first we need to look at if there were any correlation between numeric variables with price.
Getting ready a longer dataframe will be used by ggplot


```{r longer, echo=TRUE}
longer_diamonds <- diamonds %>% select(1, 5,6,7,8,9, 10) %>% pivot_longer(-price, names_to = "names", values_to="values")
longer_diamonds
```

## Correlation Detection by Visual Inspection
By Visual inspection it looks like carat, x, y and z variables have strong correlation with price

```{r longerggplot, echo=TRUE, warning=FALSE}
ggplot(longer_diamonds, aes(x=price, y= values)) +
  geom_point() +
  facet_wrap(~ names) +
  scale_y_log10()
```

## Examining Correlation with cor() Function
After cor() function results we could drop depth and table variables for the model since they do not have strong correlation


```{r corrfunct, echo=TRUE}
cor(diamonds$price, diamonds$carat)
cor(diamonds$price, diamonds$depth)
cor(diamonds$price, diamonds$table)
cor(diamonds$price, diamonds$x)
cor(diamonds$price, diamonds$y)
cor(diamonds$price, diamonds$z)
```


# Properties of New Dataframe w/o depth and table

```{r new_df, echo=TRUE}
diamonds_new <- diamonds %>% select(7, 2, 3, 4, 1, 8, 9, 10)
str(diamonds_new)
head(diamonds_new)

```

# Model Preparation - Splitting Dataset

We need two sets of data for both training and test.

## Total Rownumber
To do that first we need to know the total row number of diamonds dataframe

```{r nrow, echo=TRUE}
n <- nrow(diamonds_new)

```

## Definition of n_train.
Split will be conducted with ratios = %80

```{r n_train, echo=TRUE}
n_train <- round(0.80 * n)

```

## Definition of Randomness
For reproducability of whole process

```{r seed, echo=TRUE}
set.seed(87)

```

## Train_Indices

```{r trincides, echo=TRUE}
train_indices <- sample(1:n, n_train)

```

## Definitions of Sets
### Training Set
Which is a subset contains the diamonds indices equal to train indices. We will use this data frame to train the model.

```{r trainset, echo=TRUE}
diamonds_train <- diamonds_new[train_indices, ]

```

### Test Set
which is a subset contains the diamonds indices not equal to train indices. We will use this data frame to test the model.

```{r testset, echo=TRUE}
diamonds_test <- diamonds_new[-train_indices, ]

```

# Model
## Setting the Model

```{r modelset, echo=TRUE}
diamonds_model <- rpart(price ~ ., data = diamonds_train)

```

## Summary of Model

```{r modelsummary, echo=TRUE}
summary(diamonds_model)

```

## Tree View of Model

```{r modelplot, echo=TRUE}
fancyRpartPlot(diamonds_model)

```

## Details of Model

```{r printcppred, echo=TRUE}
printcp(diamonds_model)

```

## Plot View

```{r plotcppred, echo=TRUE}
plotcp(diamonds_model)

```

# Prediction
## Prediction values from Model

```{r prediction, echo=TRUE}
prediction <- predict(diamonds_model, diamonds_test)

head(prediction,50)

```

## Pruning the Tree for Optimal Decision Tree
```{r prune, echo=TRUE}
prune_diamonds_model <- prune(diamonds_model, 
                cp=diamonds_model$cptable[which.min(diamonds_model$cptable[,"xerror"]),"CP"])

fancyRpartPlot(prune_diamonds_model)               

```

## New Prediction with Pruned Model

```{r prunedprediction, echo=TRUE}
prediction_11 <- predict(prune_diamonds_model, newdata = diamonds_test)

```


# RESULT: Looking for Correlation of Pruned Model

Strong correlation coefficient

```{r prunedcorrelation, echo=TRUE}
cor(diamonds_test$price, prediction_11)^2

```


# EXTRA: Additional Version of Model
## Turning categorical variables into binary variables

```{r altermodel1, echo=TRUE}
diamonds_new_2 <- mutate(diamonds_new, 
                         clarity.I1=ifelse(clarity=='I1',1,0),
                         clarity.SI2=ifelse(clarity=='SI2',1,0),
                         clarity.SI1=ifelse(clarity=='SI1',1,0),
                         clarity.VS2=ifelse(clarity=='VS2',1,0),
                         clarity.VS1=ifelse(clarity=='VS1',1,0),
                         clarity.VVS2=ifelse(clarity=='VVS2',1,0),
                         clarity.VVS1=ifelse(clarity=='VVS1',1,0),
                         clarity.IF=ifelse(clarity=='IF',1,0),
                         color.J=ifelse(color=='J',1,0),
                         color.I=ifelse(color=='I',1,0),
                         color.H=ifelse(color=='H',1,0),
                         color.G=ifelse(color=='G',1,0),
                         color.F=ifelse(color=='F',1,0),
                         color.E=ifelse(color=='E',1,0),
                         color.D=ifelse(color=='D',1,0),
                         cut.Fair=ifelse(cut=='Fair',1,0),
                         cut.Good=ifelse(cut=='Good',1,0),
                         cut.VeryGood=ifelse(cut=='Very Good',1,0),
                         cut.Premium=ifelse(cut=='Premium',1,0),
                         cut.Ideal=ifelse(cut=='Ideal',1,0)
                         ) %>% select(-clarity) %>% select(-color) %>% select(-cut)

```

## Same steps before

```{r altermodel2, echo=TRUE}

str(diamonds_new_2)

```

```{r altermodel3, echo=TRUE}

head(diamonds_new_2)

```

```{r altermodel4, echo=TRUE}

n <- nrow(diamonds_new_2)
n_train <- round(0.80 * n)
set.seed(87)
train_indices <- sample(1:n, n_train)

diamonds_train_2 <- diamonds_new_2[train_indices, ]
diamonds_test_2 <- diamonds_new_2[-train_indices, ]

diamonds_model_2 <- rpart(price ~ ., data = diamonds_train_2)

summary(diamonds_model_2)

```

```{r altermodel5, echo=TRUE}

fancyRpartPlot(diamonds_model_2)

```

```{r altermodel6, echo=TRUE}

printcp(diamonds_model_2)

```

```{r altermodel7, echo=TRUE}

plotcp(diamonds_model_2)

```

```{r altermodel8, echo=TRUE}

prediction_2 <- predict(diamonds_model_2, newdata = diamonds_test_2)

```

```{r altermodel9, echo=TRUE}

head(prediction_2, 50)

```
