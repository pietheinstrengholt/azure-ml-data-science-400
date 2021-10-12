# Activity 03: Use Cases for AutoML

Azure Machine Learning's Automated Machine Learning (AutoML) functionality provides an accessible entry point for machine learning development, but it is not always going to be the correct choice for the job.  In this activity, you will help a customer understand when it makes sense to use AutoML and when it does not.

## Requirements

* For each scenario, answer the scenario **as-is**.  Make note of the following key factors:
  * Who would perform the work?
  * What is the specific task in question?
  * What is the specific goal is for this use case?
* After providing the as-is answer, discuss under what circumstances you would change your mind.  Focus on the key factors above and discuss which circumstances changing might make you change your answer.

## Use Cases

1. A data scientist has been assigned to a new project, predicting how many copies of specific books a store will sell on a weekly basis.  She does not have much domain expertise and wishes to see if there is a "minimum viable solution" for this problem.

2. After building a "minimum viable solution" to the problem of predicting book sales, the data scientist would now like to find the best possible model to predict book sales.

3. A data scientist needs to perform image classification on tens of thousands of images, picking out whether a given image is a front shot of clothing, a rear shot of clothing, a front shot of a person modeling clothing, a rear shot of a person modeling clothing, or an action shot of a person wearing a garment.  All of the images should fit in one of these classes, but the data scientist has an extremely limited set of labeled data to start with.

4. A database administrator would like to estimate when his SQL Server instances will run out of disk space based on daily database growth metrics.

5. An application developer would like to implement a spam filter, tagging incoming posts as spam or not.  End users would be able to review the spam posts for legitimate posts, so the spam filter does not need to be perfect.  The developer has a wide variety of post data, including posts which users have manually reported as spam.

6. A data scientist has read a new academic article outlining a novel technique for image classification and would like to use this technique to train a new, (hopefully) more accurate model.
