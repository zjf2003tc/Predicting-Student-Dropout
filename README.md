## Project Goals

* Construct classification algorithms to predict student dropout
* Compare the accuracy of three models: CART, Conditional Inference Tree, and C5.0

## Data Set
The data comes from an unnamed university registrar's office. 
Below are definitions for each of the variables and six example rows:

* student_id = Student ID
* years = Number of years the student has been enrolled in their program of study
* entrance_test_score = Entrance exam test score
* courses_taken = Number of courses a student has taken during their program
* complete = Whether or not a student completed a course or dropped out (yes = completed)
* enroll_data_time = Date and time student enrolled in POSIXct format
* course_id = Course ID
* international = Is the student from overseas
* online = Is the student only taking online courses
* gender = One of five possible gender identities

![](sampledata.png)

## Exploratory Analysis

Associations were analyzed between each pair of variables using ggpairs. There were two key findings:

1. The correlation between each pair of continuous variables (Entrance test score, Courses taken, and Date of enrollment) was significant in every case. 

  * The most significant relationship was between entrance test score and time of enrollment (r = -.369). This makes sense because the higher score someone gets on an entrance exam, the more prepared they are for a class and the more likely they are to have a higher number of credits entering University. Colleges often schedule registration dates by number of credits; students with more credits can schedule first.

  * For the same reason, it is also sensible why there would be a negative correlation between courses taken and date of enrollment and why there would be a positive relationship between entrance test score and courses taken.

2. There were no notable categorical associations.

## Modeling

* Data were separated into a train and test dataframe, with 75% of the rows in test.
* Data were centered and scaled to make the weights of the variables uniform regardless of the model.
* Three models within the caret package were used: "rpart", "ctree" and "C5.0" 
* K-fold cross validation was applied to all of the models, and the final model choice was made according to the sensitivity metric.

## Model Example: Conditional Inference Tree

Below is a plot illustrating the parameters selected by the conditional inference tree model and the associated classification distributions. Scaling and normalization in this model instance were not performed to enhance interpretability of the tree plot.

![](condinftree.png)

* The model has several branches with many variables directing the classification: years, entrance test score, total courses taken, and course_id.

* Similarly to the rpart model, it is clear that having been enrolled in a program at the University for a longer period of time before taking a course is an excellent predictor of completion, with more years resulting in zero chance of completion.

* More total courses taken seems to result in a lower chance of completion. This might be because it is correlated with the years of study variable, which is also inversely related to successful completion.

* Entrance test score was not an attribute of the rpart model but was in this model. It appears just once as a final decision node, but it does end up slightly impacting the probability of completion of about 1700 students in the train data.


## Comparison

Models were compared on several metrics of performance:
* Accuracy 
* AUC 
* F
* Kappa
* Precision
* Recall
* ROC
* Sensitivity
* Specificity

Below are the performance metrics for the rpart (cart), conditional inference (condinf), and C5.0 (cfiveo) models. They are represented as boxplots after resampling was performed (resamples = 30).

![](accuracyclassification.png)

* The three models are extremely similar in all model performance measures. However, the conditional inference model had the highest median value in a plurality of categories. Notably, the first quartile specificity and precision were both 1.00 for the CondInf model. It also had the smallest interquartile range for a plurality of the categories, meaning it was the most consistent model.

## Conclusion

Within the final conditional inference tree model these are several important features:

* The model has 17 terminal branches which are directed by years, course_id, courses taken, and gender. Notably, entrance test scores were not an attribute of this tree, nor many of the other trees. Entrance tests may only be necessary in deterring students from taking a class in the first place, but they don’t seem to be efficient at predicting dropout.

* Having been enrolled in a program for more than one year is almost a perfect predictor of dropout for this data. It might be the case that students were unable to enroll in courses at this university if they were not freshmen. Regardless, these students are lacking awareness of this serious hurdle to completing a course. Proper restrictions for sign-up and better communication about this rule are imperative.

* Unsurprisingly, there are variable dropout rates by course. The exact details regarding which courses align with which predicted rates of drop out once accounting for other student characteristics like courses taken could be helpful to explore. For example, students taking course 658438 might be more likely to drop out if they have taken fewer classes previously.

* Overcoming the low sensitivity of the model is possible via a more simplistic implementation: an adjusted total enrollment prediction. If the model was used to set an upper bound on the total number of students that will stay in courses, it would certainly not under count and is consistently 10% higher than the real count.

## Resources

* [The caret package](https://topepo.github.io/caret/train-models-by-tag.html)





