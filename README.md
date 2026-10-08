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

First, I want to compare male and female students.  The next graph shows success on a final exam for male and female students.

ggplot(data = students) +
  geom_bar(mapping = aes(x=final_exam_score, fill=gender)) + 
  labs(title = "Graph 1", subtitle = "Success of male and female students on final exam")

duplicates <- students[duplicated(students), ]


Again, there are no duplicates. Considering that there are only one thousand rows, it is probably to expect no errors.
