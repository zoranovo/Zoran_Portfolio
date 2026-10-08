# Zoran_Portfolio
Data science portfolio
# Project 1
# Student performance and study habits dataset
This dataset was downloaded from Kaggle and represents grade outcomes depending on multiple parameters. Data analysis was done using R. Dataset has 12 variables and 1000 observations in csv format. I assume here that the data are genuine because they where downloaded from Kaggle
(https://www.kaggle.com/datasets/harshadapatil31/student-performance-and-study-habits-dataset).

How do various parameters influence grade outcomes? In order to find out, I compared different variables to grade outcomes.

I started with data preparation. First, necessary packages were installed.

install.packages("tidyverse")

library(tidyverse)

install.packages("ggplot2")

library(ggplot2)

library(tidyverse)


Next step was to upload dataset and to remove any NA rows.

students <- read.csv("student_performance_dataset.csv")

students_d <- drop_na(students)

This didn’t remove any rows, so next would be to check for any negative values, since there shouldn’t be any in the structure of the data.

any(students_d<0, na.rm = TRUE)

It shows no negative values. Last thing is to look for duplicates.

duplicates <- students[duplicated(students), ]

Again, there are no duplicates. Considering that there are only one thousand rows, it is probably to expect no errors.

First, I want to compare male and female students.  The next graph shows success on a final exam for male and female students.

ggplot(data = students) +
  geom_bar(mapping = aes(x=final_exam_score, fill=gender)) + 
  labs(title = "Graph 1", subtitle = "Success of male and female students on final exam")



<img width="864" height="546" alt="grafik" src="https://github.com/user-attachments/assets/50528803-4d00-47ce-971a-c21fe836589d" />

We see from the graph that students who had 100% score in the final exam are by far more numerous than students with any other score, about 7,8% of the total number of students. I am going to omit these results in order to better illustrate success of male and female students on a final exam.

ggplot(data=students %>% filter(final_exam_score<100)) +
  geom_bar(mapping = aes(x=final_exam_score, fill=gender)) +
  labs(title = "Graph2", subtitle = "Success of male and female students on final exam(omited 100%)")

  <img width="864" height="546" alt="grafik" src="https://github.com/user-attachments/assets/c59631f4-ace2-4f31-a318-c1cea62148e6" />

  We see from the graph, that female students score better results on a final test. However, this display could be misleading, because if you look at the average scores per gender, you find out that the difference is not large.

sum_scoresm <- sum(students[students$gender=="Male",]$final_exam_score)/sum(students$gender=="Male")

sum_scoresf <- sum(students[students$gender=="Female",]$final_exam_score)/sum(students$gender=="Female")

Where sum_scoresm is an average final exam score for male students, and sum_scoresf is an average score for female students. It gives results of 83.48 for male students and 83.61 for female students. Because difference is only 0.13 I conclude that both gender score about equally on a final exam.

Next, we are going to look at dependency of hours of study per day with success on a final exam. One thinks that the harder (or put in more hours) you learn, better will the result be. Let’s look at graph and the statistics.

ggplot(data = students) +
 geom_point(mapping = aes(x=study_time_hours, y=final_exam_score)) + facet_wrap(~gender) +
 labs(title = "Graph 3", subtitle = "Sucess of male and female students on a final exam compared to study hours")

<img width="864" height="546" alt="grafik" src="https://github.com/user-attachments/assets/685cdfd5-8a8d-4851-9100-3fd3f606e41e" />

We see from the graph, that for majority of students, is true that effort pays off. It seems that for both gender, hours are concentrated around 4. We can look at the statistics, to see what the mean is.

students_sth <- students %>% group_by(gender) %>%
 summarise(studytimehours=mean(study_time_hours))

The result is that for male students, mean study time is 3.58h and for female students is 3.56h, confirming what we see on the graph.

Next, we can compare attendance percent with final exam score.

ggplot(data = students) +
 geom_point(mapping = aes(x=attendance_percent, y=final_exam_score)) + facet_wrap(~gender) +
 labs(title = "Graph 4", subtitle = "Sucess of male and female students on a final exam compared to attendance percent")
 
<img width="850" height="546" alt="grafik" src="https://github.com/user-attachments/assets/3d32ee54-fe2b-46eb-b225-88f7f3b2a62e" />

It is clear from the graph that attendance influences final score, because results are concentrated in upper right part of the graph.

students_ah <- students %>% group_by(gender) %>%
 summarise(attendancepercent=mean(attendance_percent))

Mean for male students is 85% and for female students 85.2% of attendance percent, showing again that the difference between male and female students is negligible.


Next, we will look at students success depending on whether they have internet access. It is hard to conclude something from the graph, because majority of students do have access, so it is unclear whether students with or without internet access score better. So, for that purpose, I have calculated mean of student success, depending on the internet accesss.

students_ia <- students %>% group_by(internet_access) %>%
  summarise(finalexam=mean(final_exam_score))
 
Calculation gives the result of success on a final exam as 84.3 for students with internet access against 79.3 for those without. It shows difference of exactly 5%.

Now, let’s see how does success on final exam is influenced by existence of extracurricular activities.

<img width="849" height="546" alt="grafik" src="https://github.com/user-attachments/assets/5a8cb1b0-9a58-4730-b739-97d836d8018a" />

We can see from the graph 5, that students with extracurricular activities score somewhat better than students without. This is somewhat surprising, because one would think that extra time they spent on these activities would take away from attendance, studying or sleep hours. But, it is obviously giving them some benefit that make up for the “lost” time. Results from analysis are the following:

students_ea <- students %>% group_by(extracurricular_activities) %>%
 summarise(finalexamscore=mean(final_exam_score))

students_eag <- students %>% group_by(extracurricular_activities, gender) %>%
 summarise(finalexamscore=mean(final_exam_score))

Results are 83.7% for students that have activities against 83.4% for students without. For male students it is 84.1% that have against 82.7% that don’t. For female students it is 83.4% that have
against 83.9% that don’t. We see here that there is a small difference between students in general, with male students performing slightly better.




Finally, we are going to check dependence of student success on having or not a part time job. 

ggplot(data = students) +
  geom_bar(mapping = aes(x=final_exam_score, fill=gender)) + facet_wrap(~part_time_job) + 
  labs(title = "Graph 6", subtitle = "Success of male and female students depending on part time job")

  <img width="849" height="546" alt="grafik" src="https://github.com/user-attachments/assets/466a93bd-ebe2-4550-a9f7-2a6d868a81c1" />

It is clear from graph 6, that students without part time job are somewhat better achievers. However, more than two thirds of the students don’t have a part time job, so again we look at the statistics.

students_pt <- students %>% group_by(part_time_job) %>%
 summarise(finalexamscore=mean(final_exam_score))

students_ptg <- students %>% group_by(part_time_job, gender) %>%
 summarise(finalexamscore=mean(final_exam_score))


The data says, that students without part time job achieve 84.6% while those with part time job achieve 81.3% on a final exam. For male students it is 84.7% without and 80.5% for those with part time job. For female students it is 84.4% without and 82% for those with part time job. It is clear from the data that having part time job lowers the rate of success, but only by 3-4%.



In conclusion, all the extracurricular activities, having a part time job and having internet access shows small correlation with final exam score, with differences up to only 5%. there is however strong correlation between study hours and attendance with final exam score, showing that on average students perform better on a final exam with higher attendance and study hours, Last, I conclude that there is no significant difference between male and female students, with differences up to 2%.





















