# Introduction

**This project was completed for educational purposes. All views are my own**

The overall objective of this project is to reduce dropout rates of students at the polytechnic institute of Portugal. To do this we need to have a good understanding of what factors can lead to student dropouts. Is it all on what grades the students get, or are their larger socioeconomic reasons at play?
I created a model that will be able to predict if a student is likely to drop out or not. This will allow student services to help them, with pairing them to the department they need. Whether that be financial aid services or a tutor

I also deployed two web apps using transfer models to help the tutors better support the students. One of the models is able to help pair students to tutors by having text classification, it is able to take in a student question and classify which topic it is. Allowing the right tutor to be selected based off their specialty. While the other app is able to summarize the contents of a tutor session. These web apps can be found through this [link](https://huggingface.co/spaces/DivisiBULL/text_classification) for the classification app and this [link](https://huggingface.co/spaces/DivisiBULL/summarymachine) for the summary app

# Data Understanding

The Data consists of a range of demographic, socioeconomic and macroeconomic data for 4,424 students. The data contains 37 features including the target. The original dataset has three target variables (dropout, enrolled, graduate) but I will only be using (dropout, graduate ). All the data comes from the polytechnic institute of Portugal from the years of 2008-2019. The data was aggregated and cleaned by the Institute. A few features include what course they are doing, how many credits they are taking, and the GDP. The data is stored in a CSV format.

The data aggregation as described by the university. The data sources used consist of internal and external data from the institution and include data from the Academic Management System (AMS) of the institution, the Support System for the Teaching Activity of the institution (developed internally and called PAE), the annual data from the General Directorate of Higher Education (DGES) regarding admission through the National Competition for Access to Higher Education (CNAES), and the Contemporary Portugal Database (PORDATA) regarding macroeconomic data.

The institute formatted all of the data numerically (eg the feature Marital Status: 1 – single 2 – married 3 – widower 4 – divorced 5 – facto union 6 – legally separated). This can cause the models and the data in genreal to be harder to interpret. We will use standard scaling as to not give higher importances to certain features just by the numbers they were assigned. The only exception to this is the target variable which comes in string format, and I will convert to numeric.

The feature descriptions as well as a way to download the dataset can be found from this [link](https://archive-beta.ics.uci.edu/dataset/697/predict+students+dropout+and+academic+success). If you want to work with this data you can simply hit download in the top right from the link. You should then move the data into your working directory to be able to bring it in with pd.read_csv().

# Background

There exists a strong correlation between one's level of education and employment rates. In particular, individuals aged 25-64 years old who have completed upper secondary or post-secondary non-tertiary education exhibit a significant increase in employment rates compared to those with educational attainment below upper secondary level. On average, only 58% of individuals with below upper secondary education are employed in OECD countries, whereas 75% of individuals with upper secondary or post-secondary non-tertiary education are employed. For anyone unfamiliar with the acronym OECD is the Organisation for Economic Co-operation and Development which includes 38 member countries, mostly based in Europe.

57%, of college students take longer than six years to complete their degree. Within this group, 33% of students drop out of college. 28% of students drop out before they become sophomores. Among the remaining 72% of students who continue their college education, 60% remain at the same institution where they started.

About 25% of adults aged 25-64 years old, in Portugal have attained tertiary education. This share still falls below the OECD average of nearly 40%. The rates of completion have had a considerable improvement over past decades. Among the younger generation 25-34 years old, the tertiary attainment rate in 2018 was 35%

In OECD countries the share of 25-34 year olds with a tertiary degree has increased from 27% in 2000 to 48% in 2021, with a steady increase in tertiary attainment being a universal trend across OECD countries. Business, administration, and law is the most common field of study, but the combined fields of science, technology, engineering, and mathematics (STEM) are the most prevalent. The decline in the share of the population with upper secondary or post-secondary non-tertiary education as their highest level of attainment has been less pronounced than the increase in tertiary attainment.

# Metrics

I have chose recall as my main metric as it is more important to catch people who are at risk of dropping out. Focusing on **increasing recall** will make the models focus on reducing **False Negatives** (when someone drops out, but predicted they graduate) as these predictions can cause more detrimental outcomes for students. If these students are missed it is much more likely they will drop out as they have not been given the additional assistance they require. While too many **False Positives** (when someone graduates, but predicted they drop out) only runs the risk of overworking student services, which if it went too far could lead to reduced aid to each individual student.

For my dataset I have set my target variables as dropout = 1, graduate = 0. Traditionally for student outcome predictions these labels are flipped, so graduate is set as 1. Given the business case is specifically trying to identify students who are likely to dropout as to provide them aid we will be setting dropout as our 1 label.

Recall = TruePositives / (TruePositives + FalseNegatives). Recall is a metric that quantifies the number of correct positive predictions made out of all positive predictions that could have been made. Recall can be thought of as what percent of the dropout cases did the model catch. There will be recall metrics for both the dropout and graduate label.

Precision = TruePositives/(TruePositives + FalsePositives). Precision is the ability of a classifier not to label an instance positive that is actually negative. Precision can be though of as what percent of your predictions were correct. There will be precision metrics for both the dropout and graduate label.


For more information on the metrics and how to calculate them visit this [link](https://machinelearningmastery.com/precision-recall-and-f-measure-for-imbalanced-classification/)

# Evaluation

The XGBoost gridsearch model with regularization on the full feature set performed the best. It was able to achieve a 91% recall score for our dropout label. Recall can be thought of as what percent of the dropout cases did the model catch. So 9% of students who dropped out were not caught by the model. 

Out of our test set, there were 13 students who dropped out but the model predicted they would graduate. These are referred to as false negatives. These are the bottom left cases on the confusion matrix. These cases are the most detrimental outcome from the model as they do not allow us to provide these students with the support they need. 

Another metric that is important from getting too high is false positives (when someone graduates, but predicted they drop out). These are the top right cases on the confusion matrix. These are not as detrimental as our false negatives, as on a small scale providing aid to a student who didn’t require it will not negatively affect them. But if there are too many this will cause the university to use resources it did not need to

The model metrics are a sort of balancing act of trying to catch as many people who drop out as we can while not just predicting everyone drops out. If we predicted everyone dropped out we would have a recall score of 100% but it would cause one of our other metrics, precision to drastically decrease. Precision can be thought of as what percent of your predictions were correct. For this model we were able to achieve a precision of 89%. So we are able to catch the majority of student dropouts without having a low precision score

# Additional Projects

Projects:
- [Text Classification Model](https://huggingface.co/spaces/DivisiBULL/text_classification) 
- [Text Summarization Model](https://huggingface.co/spaces/DivisiBULL/summarymachine)
- [Presentation](https://github.com/DivisiBULL/predicting_student_outcome/blob/main/Presentation.pdf)

Future Projects: 
- Deploy the highest performing models

# Conclusion

Their could be use cases where you would want a less complex model, that provides faster run times even if it performs a little worse. In these instances our Decision Tree GridSearch preformed quite well and it a very simple model. This could allow for quick turn around on a large number of results. This model does not perform as well on the reduced features set, that imitates a new student but it could still perform "well enough" for certain use cases.

The XGBoost with regularization performs better on both sets of features (original and reduced set to emulate new students). This model should be the one used for most predictions of student outcome by the institute. It is the best for the business case as the business case does not call for lightning fast run times, instead has higher needs of model performance.

Overall there is a pretty limited amount of work that could be done to improve the models. Some people will have dropped out for reasons the models couldn’t predict. For example a family emergency that required the student to dropout and go back home, which would be near impossible for the model to accurately predict. The few things that could be done to improve the models would likely not be worth the time investment to do (gather more data, higher complexity models). These models will allow student services to aid the students who need it while not stretching student services too thin trying to provide tutoring or economic support to all students.
