---
title: "CCBHC"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Subset data for baseline for child and adult
```{r}
setwd("P:/Evaluation/CCBHC IN/Data")
ccbhc_adult = read.csv("Adult data.csv", header = TRUE, na.strings = c(-1:-11))
head(ccbhc)
adult_base = subset(ccbhc_adult, ccbhc_adult$InterviewType_07 == 1)

```
Age, sex, race, 
```{r}
describe.factor(adult_base$Agegroup)
describe.factor(adult_base$RaceWhite)
describe.factor(adult_base$RaceBlack)
describe.factor(adult_base$RaceAsian)
describe.factor(adult_base$HispanicLatino)
describe.factor(adult_base$Gender)
dim(adult_base)[1]
```
Baseline outcomes
weight(kg) / height(cm/100)^2
18 or below =  underweight
19 and 24 = normal
25 to 30  = overweight
30+ = obese
```{r}
bmi_data = data.frame(weight = adult_base$WeightResponse, height_m_2 = (adult_base$HeightResponse/100)^2, bmi_adult = adult_base$WeightResponse / (adult_base$HeightResponse/100)^2)
bmi_data_complete = na.omit(bmi_data)
head(bmi_data_complete)

# Number of people with completed the data
dim(bmi_data_complete)

###
typeof(bmi_data_complete)
bmi_data_complete$bmi_category = as.integer(bmi_data_complete$bmi_adult)
bmi_data_complete$bmi_category =  ifelse(bmi_data_complete$bmi_adult < 18.5, "Underweight", ifelse(bmi_data_complete$bmi_adult < 25, "Normal", ifelse(bmi_data_complete$bmi_adult < 30, "Overweight", ifelse(bmi_data_complete$bmi_adult >= 30, "Obese", "Wrong!!!!"))))
describe.factor(bmi_data_complete$bmi_category)
```
Decrease mental health symptomatology
Decrease substance use by 40% among consumers diagnosed with substance use disorder. 
Experience of violence or trauma 
Diagnosis
```{r}
describe.factor(adult_base$RxOpioids_Use)
RxOpioids_Use_code = ifelse(adult_base$RxOpioids_Use == 1, "Never", ifelse(adult_base$RxOpioids_Use== 2, "Once or twice", ifelse(adult_base$RxOpioids_Use==3, "Weekly", ifelse(adult_base$RxOpioids_Use == 4, "Daily or Almost Daily", "NA")))) 
describe.factor(RxOpioids_Use_code)
```
