# Activity 03: Use Cases for AutoML

Azure Machine Learning's Automated Machine Learning (AutoML) functionality provides an accessible entry point for machine learning development, but it is not always going to be the correct choice for the job.  In this activity, you will help a customer understand when it makes sense to use AutoML and when it does not.

## Requirements

* For each scenario, answer the scenario **as-is**.  Make note of the following key factors:
  * Who would perform the work?
  * What is the specific task in question?
  * What is the specific goal is for this use case?
* After providing the as-is answer, discuss under what circumstances you would change your mind.  Focus on the key factors above and discuss which circumstances changing might make you change your answer.

## Use Cases

**TODO:  whiteboard?**

Open your whiteboard for the event, and in the area for Activity 4 provide your answers to the following challenges.

*The following challenges are already present within the whiteboard template provided.*

Challenges

**TODO:  split out answers into the "answers" repo**

1. A data scientist has been assigned to a new project, predicting how many copies of specific books a store will sell on a weekly basis.  She does not have much domain expertise and wishes to see if there is a "minimum viable solution" for this problem.

    **Answer**:  Yes, this would be a great scenario for AutoML.  Because the data scientist does not have much knowledge of the domain, she might not know which techniques are most likely to generate reasonably accurate predictions.  She is not looking for the best solution to the problem, only one which establishes a floor.  With that information, she can return to the business side and see if her "minimum viable solution" is close enough to what the business needs to make it worth pursuing a better solution.  AutoML helps in this regard by performing a wide variety of tests, training different types of models over different subsets of the data.  This can help the data scientist understand which techniques are particularly good (or bad) at solving the problem, and which explanatory variables contribute the most to a solution.

    **Alternatives**:  The key factor which might shift this answer from yes to no would be the data scientist's level of domain experience.  A data scientist with significant domain experience may already know which algorithms typically perform well and what sorts of features are likely to drive sales activities.  Even in this case, AutoML may still be useful as a low-effort way of checking the data scientist's assumptions.

2. After building a "minimum viable solution" to the problem of predicting book sales, the data scientist would now like to find the best possible model to predict book sales.

    **Answer**:  No.  AutoML has a limited set of available algorithms.  Most problems have **good enough** solutions within those algorithms, but there are specific techniques not available in AutoML which can provide superior answers in specific circumstances.  As an example, certain types of deep learning algorithms might end up delivering a superior model over what AutoML can create.  Much of this answer comes down to the skill level and domain knowledge of the data scientist:  the more skilled and domain-knowledgeable the data scientist is, the less likely a general-purpose search tool like AutoML will be to beat the data scientist.

    **Alternatives**:  For problems with a fairly limited domain, the data scientist could run AutoML using all of the available regression algorithms.  If the best choice of algorithm given the particulars of this book store and the specific books sold there happens to be one of the algorithms AutoML chooses, then AutoML may in fact be capable of delivering the best possible model given enough time and compute resources.

3. A data scientist needs to perform image classification on tens of thousands of images, picking out whether a given image is a front shot of clothing, a rear shot of clothing, a front shot of a person modeling clothing, a rear shot of a person modeling clothing, or an action shot of a person wearing a garment.  All of the images should fit in one of these classes, but the data scientist has an extremely limited set of labeled data to start with.

    **Answer**:  No.  The key here is that the data scientist has very little available data, making image classification a difficult task.  Instead of using AutoML to perform this task, a better route would be to [use self-supervised learning to train](https://arxiv.org/pdf/2006.10029.pdf) instead of directly performing image classification.

    **Alternatives**:  The key constraint in this problem is the limited set of labeled data.  With a larger starting set of labeled data, [performing multi-class image classification is a reasonable task in AutoML](https://github.com/swatig007/automlForImages/blob/main/MultiClass/AutoMLImage_MultiClass_SampleNotebook.ipynb).

4. A database administrator would like to estimate when his SQL Server instances will run out of disk space based on daily database growth metrics.

    **Answer**:  Yes.  The user is a database administrator, who probably is not well-versed on machine learning techniques.  The DBA has a large amount of domain experience and probably knows which factors drive database growth, but is lacking knowledge of appropriate algorithms or ways of shaping data.  AutoML can help with both of these tasks and provide an answer fairly easily.

    **Alternatives**:  A database administrator who is familiar with machine learning techniques might want to use the Azure ML designer view.  Another alternative would be to use SQL Server Machine Learning Services to perform model training and inference locally, rather than connecting to a remote service.  Given that the user in question is a DBA, that person might not have classic application development skills, such as connecting to and reading data from web services; this makes a product like ML Services potentially more interesting.

5. An application developer would like to implement a spam filter, tagging incoming posts as spam or not.  End users would be able to review the spam posts for legitimate posts, so the spam filter does not need to be perfect.  The developer has a wide variety of post data, including posts which users have manually reported as spam.

    **Answer**:  Yes.  Spam filtering is a classic problem with known solutions, and with sufficient labeled data, AutoML can come up with a good classification algorithm for tagging spam based on text.  AutoML includes capabilities for automatic featurization of text data, something an application developer may not know to do in an efficient and effective way.  Furthermore, AutoML includes algorithms which work well when dealing with imbalanced data, and spam classification is typically an imbalanced data problem:  typically there are many more non-spam messages than spam messages, and so most labeled messages will be non-spam.  AutoML has built-in functionality to deal with this class imbalance, such as [applying weights to samples belonging to a particular class](https://techcommunity.microsoft.com/t5/azure-ai/dealing-with-imbalanced-data-in-automl/ba-p/1625043).

    **Alternatives**:  If the developer is familiar with the ML.NET library, there is an AutoML capability built into this library which might be even better for the application developer.  Furthermore, depending on the minimum requirements for this classification task, an algorithm like Naive Bayes classification is easy to implement (and has a native implementation in a variety of libraries, including ML.NET) and typically performs well on spam classification with enough labeled data.

    Another factor which may nudge the answer toward "No" is if the number of posts is extremely high and users need real-time or near-real-time responses.  In that case, the overhead cost of API calls against a deployed Azure ML web service might be large enough that it would make more sense to deploy a trained model to the server and perform inference locally.

6. A data scientist has read a new academic article outlining a novel technique for image classification and would like to use this technique to train a new, (hopefully) more accurate model.

    **Answer**:  No.  The algorithms available in AutoML do not tend to change very frequently, certainly not at the speed of academic research.  AutoML tends to include a set of "industry-standard" algorithms which have been proven valuable over time and across a wide variety of datasets.  This has the upside of not including many low-value or "unstable" algorithms, but has the downside of not including novel algorithms which may in fact be superior to classical variants.

    **Alternatives**:  The answer focused on algorithms for image classification, but "technique" can also include things such as pre-processing of images or transforming image classification into a different type of problem.  In this case, it could be possible to use a novel data shaping technique and use the resulting data and a "classical" algorithm to train a model.
