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
head(ccbhc_adult)
adult_base = subset(ccbhc_adult, ccbhc_adult$InterviewType_07 == 1)
child_base = subset(ccbhc_child, ccbhc_child$InterviewType_07 == 1)

dim(adult_base)[1]+dim(child_base)[1]

```
Age, sex, race, 
```{r}


#install.packages("descr")
library(descr)
#install.packages("Hmisc")
library(Hmisc)
#install.packages("prettyR")
library(prettyR)

describe.factor(adult_base$Agegroup)
describe.factor(na.omit(adult_base$RaceWhite))
describe.factor(na.omit(adult_base$RaceBlack))
describe.factor(na.omit(adult_base$RaceAsian))
describe.factor(na.omit(adult_base$HispanicLatino))
describe.factor(na.omit(adult_base$Gender))
dim(adult_base)[1]
??describe.factor

White_Data_complete=na.omit(adult_base$RaceWhite)
describe.factor(White_Data_complete)'

Black_Data_complete=na.omit(adult_base$RaceBlack)
describe.factor(Black_Data_complete)

Asian_Data_complete=na.omit(adult_base$RaceAsian)
describe.factor(Asian_Data_complete)

HispanicLatino_Data_complete=na.omit(adult_base$HispanicLatino)
describe.factor(HispanicLatino_Data_complete)

  
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
###Opiods
describe.factor(adult_base$RxOpioids_Use)
RxOpioids_Use_code = ifelse(adult_base$RxOpioids_Use == 1, "Never", ifelse(adult_base$RxOpioids_Use > 1, "Any use", "NA"))
RxOpioids_Use_code_complete = na.omit(RxOpioids_Use_code)
describe.factor(RxOpioids_Use_code_complete)

#### Alcohol
alcohol_use = ifelse(adult_base$Alcohol_Use == 4, "Daily or almost daily", "Non-daily use")
alcohol_use_complete = na.omit(alcohol_use)
describe.factor(alcohol_use_complete)
#### Stimulants
stim_use = na.omit(ifelse(adult_base$Stimulants_Use > 1, "Any use", "No use"))
describe.factor(stim_use)

#### Trauma
trauma = na.omit(adult_base$ViolenceTrauma)
describe.factor(trauma)


### Average Night homeless
describe.factor(adult_base$NightsHomeless)

mean(adult_base$NightsHomeless, na.rm = TRUE)

housing_data_complete = data.frame(na.omit(adult_base$Housing))
describe.factor(housing_data_complete)

describe.factor(na.omit(adult_base$TimesER))
describe.factor(na.omit(adult_base$NightsHospitalMHC))
describe.factor(na.omit(adult_base$NightsJail))

###
describe.factor(na.omit(adult_base$Education))
```
Blood pressure
120 below
```{r}
adult_base.BloodPressureSystolicResponse
syst_low = ifelse(adult_base.BloodPressureSystolicResponse < 120, "Normal", ifelse(adult_base.BloodPressureSystolicResponse <= 129, "Elevated", ifelse(adult_base.BloodPressureSystolicResponse >= 130, "Hypertension", "Wrong!!!!!")))

dat_sys = data.frame(syst_low, sys = adult_base$BloodPressureSystolicResponse)

BPData=data.frame(adult_base$BloodPressureSystolicResponse)
BPData_complete = na.omit(BPData)
attach(BPData_complete)

describe.factor(syst_low)

```
Trauma
```{r}
describe.factor(adult_base$ViolenceTrauma)

TraumaData=data.frame(adult_base$ViolenceTrauma)
TraumaData_complete = na.omit(TraumaData)
attach(TraumaData_complete)
describe.factor(TraumaData_complete)

BustancesData=data.frame(adult_base4)
```
Substance use
```{r}
describe.factor(na.omit(adult_base$Alcohol_Use))

```

