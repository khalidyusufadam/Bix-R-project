﻿# Bix-R-project
This was a project I developed after the completion of the 'Exploratory Data Analysis and Essential Statistics using R' module at Cranfield University, UK. I analysed three set of datasets to showcase my understanding of the concepts of programming with R. 
1) A_SC.csv is data of some quality parameters of UK Asparagus plant obtained after storage in a Dynamic Controlled Atmosphere (DCA) environment after harvest. This was done in an attempt to extend the shelf-life of the Asparagus plant.
2) Mean_Central_Eng_Temp.csv is dataset of the Mean Central England Temperature in Degrees Celsius from 1659 to 2022.
3) event_times.txt contains the cumulative time it takes for a cell to undergo 134 consecutive cell divisions (in minutes).

Below is a detailed reports on how the 3 different datasets were processed using R environment. For details of the tasks please see the attached file EDA_Stats_Assignment_2023-2.pdf

TASK 1
Q1. The A_CS.csv file provided was loaded into the R environment using the read.csv() function and was named ASP accordingly. The data has 24 observations across 12 variables. A quality control and initial exploratory data analysis of the data (ASP) provided was carried on and the summary of the data using summary() function revealed that the data has a missing value (NA). Further checks using is.na() and complete.cases() functions confirms that there is one missing value in the DPA metabolite variable. The NA was then replaced by the mean of the DPA values using the mean(), is.rm() and is.na() functions accordingly. Histrograms for each variable was created using the hist() function to check the distributions of each variable as shown in Fig1.

 ![image](https://github.com/user-attachments/assets/bc240af6-dd9c-489e-8be6-53cbba1b5990)

Figure 1: Histograms showing the distributions of all the ASP variables.

Comment: a variety of patterns were observed due to different responses of the variables to the treatment and the time combinations. 

Q2. Two new columns were created and added to the ASP dataframe using the dplyr::mutate() function. The new dataframe was named ASPext. The new variables are sum of all sugars (sum_sugar) and sum of ABA and its metabolites (ABA_metab).
Q3. To create boxplot for the variables sum_sugar, ABA_metab, TSS and moisture_loss to visualise the distribution of values for each time/treatment combinations, sample, treatment and time were first converted to factors using the dplyr::mutate_at() function and a frequency table was generated using the table() function to view the frequencies of time and treatment.
The following boxplots were generated for the above variables using the ggboxplot() function:

 ![image](https://github.com/user-attachments/assets/299af1a2-7670-4fc5-acba-13dc75fb7426)

Figure2: Boxplot plot showing the distribution of Sum of sugars.

Comment: No suspected outliers. There seems to be not much difference in sugar levels between the control (AIR) and the treatment (DCA) at 8 days but a clear difference in sugar levels is observed at 28 days of storage with a higher level observed at the DCA. This is an indication that the dynamic control atmosphere has successfully reduced the metabolic activities of the Asparagus plant.

 ![image](https://github.com/user-attachments/assets/0cf2d7b4-2cdf-4836-afb5-89e3d2050487)

Figure3: Boxplot plot showing the distribution of sum of ABA and its metabolites.

Comment: Again, no suspected outliers here, but this figure reaffirms the observation of the previous figure (fig2), we can see a higher levels of metabolites in the controls (AIR) compared to the treatments (DCA) at both 8 and 28 days of storage. Low levels of metabolites in the DCA means lower metabolic process in the treatment groups.

 ![image](https://github.com/user-attachments/assets/8704f513-e411-4545-866b-4791a4258b7e)

Figure4: Boxplot plot showing the distribution of Total Soluble Solids (TSS)

Comment: There is one suspected outlier observed in this figure sitting on top of the DCA treatment of 28 days after storage although a relatively higher amounts of TSS is observed here compared to its corresponding control. However, not much difference was observed at 8 days after storage.

 ![image](https://github.com/user-attachments/assets/466b2602-530a-4379-be05-392fd6586963)

Figure5: Boxplot plot showing the distribution of Moisture loss.

Comment: Three (3) suspected outliers can be observed in this figure, both at 28 days of storage with the treatment and control having one (1) and two (2) respectively.
Outliers: the outliers observed in figure 4 and 5 can be as a result of a faulty equipment or even from the technician/researcher taking the results but can be dealt with using appropriate packages in R.


Q4. Two-way anova was performed using the aov() function to infer the effect of Treatment and Time and their interaction on each of sum_sugar, ABA_metab, TSS and and Moisture_loss. The following statistical inference can be deduced from the summary anova results of each of the variables:

•	Sum_sugar – a very high significant difference (***) was observed for both Treatment and Time while a high significant difference (**) was observed for the Treatment/Time combination as all shows a P-value > F-value. This indicate that both treatment, time and their combination influence the sugar levels of the Asparagus plant.

•	ABA_metab – a very high significant difference (***) was observed for Treatment, Time and Treatment/Time as all shows P>F which indicate that both treatment and time as well as their combinations have a statistically significant effect on the ABA level and its metabolites.

•	TSS – no significant difference was observed for Treatment and Treatment/Time as both shows P-value < F-value, but a very high significant difference (***) was observed for Time as P>F which signifies that only time has a statistically significant difference on the total soluble solids of the plant.

•	Moisture_loss – all the independent variables and their combination here, shows a very significant difference (***) as all showed a P-value higher than the F-value and this indicate that they all have a statistically significant effects on the moisture content of the plant.

The Tukey Honest Significance Difference (TukeyHSD) test was conducted for each of the variables and the following statistically significant effects and/or interactions were observed at 95% confidence levels:

Sum_sugar 

Treatment/Time		P-values

AIR:28-AIR:8	  	0.0000000

DCA:28-AIR:8		  0.0000000

AIR:28-DCA:8		  0.0000000

DCA:28-DCA:8	  	0.0000000

DCA:28-AIR:28	  	0.0000287


ABA_metab

Treatment/Time		P-values

DCA:8-AIR:8	    	0.0000463

DCA:28-AIR:8	  	0.0000000

AIR:28-DCA:8	  	0.0000000

DCA:28-DCA:8	  	0.0003268

DCA:28-AIR:28		  0.0000000


TSS

Treatment/Time		P-values

AIR:28-AIR:8	  	0.0000005

DCA:28-AIR:8		  0.0000622

AIR:28-DCA:8	  	0.0000009

DCA:28-DCA:8	  	0.0001265


Moisture_loss

Treatment/Time		P-values

AIR:28-AIR:8	  	0.0000000

DCA:28-AIR:8		  0.0006375

AIR:28-DCA:8	  	0.0000000

DCA:28-DCA:8	  	0.0011025

DCA:28-AIR:28		  0.0000001



Q5. The following barplots showing the concentrations for AIR and DCA samples at 8 and 28 days respectively were created for the variables (sum_sugar, ABA_metab, TSS and Moisture_loss) using the ggplot() function:


 ![image](https://github.com/user-attachments/assets/408c85a1-3c82-475a-9b52-ffe618038533)

Figure6: Barplot for sum of sugars


 ![image](https://github.com/user-attachments/assets/734e8ef0-938b-4ea2-955f-57e10d66a0dd)

Figure7: Barplot for ABA and its metabolites


 ![image](https://github.com/user-attachments/assets/8263eaf6-2e87-4f15-8a19-b3cd7c4b7dec)

Figure8: Barplot for Total Soluble Solids


 ![image](https://github.com/user-attachments/assets/a011909f-464e-469a-8410-89a09561ffd1)

Figure9: Barplot for Moisture loss


Q6. Principal Component Analysis (PCA) was performed using the fviz_eig() function for the original data frame ASP and the following result was obtained:


 ![image](https://github.com/user-attachments/assets/0a652f8f-7612-4106-b43b-cb2a07ccdc8f)

Figure10: Biplot for ASP data


Assumptions: From the plot we can see a strong correlation between the sugars, ABA and the total soluble solids and that is observed mostly in the control group of 8 days of storage while ABA metabolites and moisture loss are observed in the control group of 28 days of storage. However, most of these parameters are observed to be low in both the 8 and 28 days of the treatment groups which is an indication of effectiveness.


Q7. Barplots for the first two PCs were created using the barplot() function.


 ![image](https://github.com/user-attachments/assets/e2208e56-064f-4ba9-b139-137d740f8a1a)

Figure11: Barplot for PC1


 ![image](https://github.com/user-attachments/assets/12f0a15e-2eb5-4fee-a7ee-fbf22ae74130)

Figure12: Barplot for PC2


Q8. Dendrograms were created to visualize the separation of different Treatments/Timepoints


 ![image](https://github.com/user-attachments/assets/70f1b083-5d49-41e6-8b4f-21430bc0cca6)

Figure13: Dendrograms showing the separation of different Treatments/Timepoints.

Comment: The relationships between treatments/timepoints are well represented in the heatmaps.


Q9. K-means clustering was performed for the raw and scaled data as shown below:


 ![image](https://github.com/user-attachments/assets/9d605b0e-1285-4551-bdac-3370e2b57f36)

Figure14: Result of K-mean clustering for raw data


 ![image](https://github.com/user-attachments/assets/129992b9-f7b7-4bf6-a1d4-0e3a65071d23)

Figure15: Result of K-mean clustering for scaled data

Comment: From the above result, K-mean has successfully clustered samples belonging to the same group.


TASK 2

a.	The provided csv file was loaded using the read.csv() function and the below plot was generated using the ggplot() function.


 ![image](https://github.com/user-attachments/assets/5600c950-8c19-4fe2-9b3d-70e51cf8ed0d)

Figure16: Raw plot for yearly average temperature of Central England

Comment: The data shows variations across the years with possible outliers in some years.

b. Loess smoothing was performed using the ggplot() function equivalence to a roughly 10-year moving average and for the overall long-trend across all years. The overlayed result is shown in fig17 below.


 ![image](https://github.com/user-attachments/assets/821c66bc-577d-4eaa-b94f-52abce0960d1)

Figure17: Results of loess smoothing of the yearly average temperature of Central England, equivalent to a 10-year moving average (red) and the overall long-trend across all years (black).

Observations: A relatively smooth line is formed (red), reducing the noise caused by the fluctuating line (black) of the raw data as a result of the smoothing technique used.

c. The 1961-1990 mean annual temperature was calculated using the mean() function and was subtracted from the annual mean temperatures of all the years. The new column was added to the original dataframe using the dplyr::mutate() function.The ne column was plotted against the Year column as shown below.


 ![image](https://github.com/user-attachments/assets/d56af25b-2113-498e-b8b2-5448fec47e59)

Figure18: Showing the plot of the annual mean difference of the mean annual temperature of Central England


TASK 3

a)	The waiting times data was imported and using the scan() function and was plotted as shown below:


  ![image](https://github.com/user-attachments/assets/d7b38ff1-15fa-4df7-9ce7-92300c8072c5)

Figure19: A plot showing how division times changes.

b.	A new vector with the waiting times between cell division was create and named diff.time.

c.	A histogram to visualize the distribution of the waiting times as created as shown below.


 ![image](https://github.com/user-attachments/assets/52728965-02c0-48a4-94e0-2899f59b6d5f)

Figure20: Histogram showing the distribution of the waiting times between successive cell divisions.

Comment: The data follows a negative exponential distribution.



