# Titanic Kaggle Competition

The Titanic Challenge on Kaggle is a competition in which the task is to predict whether or not a passenger survived the shipwreck. See the full overview here: https://www.kaggle.com/c/titanic/overview

## Data Dictionary

| Variable	| Definition	| Key |
|:---:|:---:|:---:|
| survival | Survival	| 0 = No, 1 = Yes |
| pclass | Ticket class	| 1 = 1st, 2 = 2nd, 3 = 3rd |
| sex	| Sex | |	
| Age	| Age in years | |
| Name | name of the passenger| |
| sibsp	| # of siblings / spouses aboard the Titanic | |
| parch	| # of parents / children aboard the Titanic | |
| ticket | Ticket number | |	
| fare | Passenger fare	| |
| cabin	| Cabin number | |
| embarked | Port of Embarkation | C = Cherbourg, Q = Queenstown, S = Southampton |

## Variable Notes

`pclass`: A proxy for socio-economic status (SES)
- 1st = Upper
- 2nd = Middle
- 3rd = Lower

`age`: Age is fractional if less than 1. If the age is estimated, is it in the form of xx.5

`sibsp`: The dataset defines family relations in this way...
- Sibling = brother, sister, stepbrother, stepsister
- Spouse = husband, wife (mistresses and fiancés were ignored)

`parch`: The dataset defines family relations in this way...
- Parent = mother, father
- Child = daughter, son, stepdaughter, stepson
- Some children travelled only with a nanny, therefore parch=0 for them.

## Feature Engineering and Preprocessing

I created several features based upon the initial given variables, and these features greatly improved model performance

| Variable	| Definition |
|:---:|:---:|
| Title | Extracted each passenger's title from name variable |
| AgeGroup | Grouped passengers into age groups based off of age quantiles	|
| FareGroup	| Grouped passengers into fare groups based off of fare quantiles |
| familySize | Sum of sibsp and parch variables |
| Alone | Binary variable indicating whether or not the passenger was alone |
| Cabin | Extracted first letter of cabin to create ordinal feature |
| Fare per Person | fare divided by family size, since families had same initial fare values |

I applied a standard scaling of all numerical features, and I also dropped the Ticket feature. For the relevant models, I applied feature selection methods to extract true signal and prevent overfitting.

## Model

I evaluated several models, including Logistic Regression, Random Forest, K Nearest Neighbors, Support Vector Classifier, and Naive Bayes. Each of these models achieved a decent cross validation accuracy after hyperparameter tuning, but they all yielded slightly different predictions. Thus, I ultimately used a voting classifier between the 5 tuned models to create my final predictions.

## Conclusions

Because this dataset is observational, we cannot make causal claims from our model. Additionally, due to the voting model and rigorous variable selection, it can be difficult to extract associations from this model. 

We can still make associations based upon data visualizations in the EDA section though. We observed that female passengers, passengers in higher classes, and passengers who traveled in smaller groups (but not alone) were all more likely to survive.

Ultimately, the model's primary use is for prediction. It cannot be applied directly to future voyages, but it can potentially be compared to other models that predict survival on catostrophic shipwrecks. By feature importances of some of the invidiual models (such as random forest), we can get a sense of how the Titanic made decisions about who should have a spot on the life boat, and whether or not this process makes sense for the future. A future task may be to train a single decision tree classifier to create a distinct set of rules for shipwrecks, but during the actual event, there may be exceptions, of course.
