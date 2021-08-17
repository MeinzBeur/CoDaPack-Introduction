# CoDaPack-Introduction
Hands on session on CoDA for the NORBIS summer school 2021. This session has been designed by Sven Le Moine Bauer, Vera Pawlowsky-Glahn and Juan José Egozcue. It follows a presentation from Vera and Juan José (see pdf).

In this hands on session, you will learn some basics of CoDA. As the session is very short, the aim here is not to cover extensively the methods available in CoDA, but rather to have you understand the logic of reasoning with compositional data and ratios. We use an old version of CoDaPack (v.2.02.21) as the new version is currently not available for Linux. [CoDaPack](http://ima.udg.edu/codapack/) is a software suited for beginners as it does not request any knowledge in coding.

## Open CoDaPack
- Open the friday directory on the desktop. Right click inside, and select "Open in the terminal".
- Enter: `java -jar CoDaPack-2.02.21-jar-with-dependencies.jar`

## Preparation of the dataset
The dataset that you will use in this hands on session comes from a paper I published in 2018: </br> [Water Masses and Depth Structure Prokaryotic and T4-Like Viral Communities Around Hydrothermal Systems of the Nordic Seas](https://www.frontiersin.org/articles/10.3389/fmicb.2018.01002/full) </br>
At that time, I did not use CoDA to analyse the data, so here is the opportunity to rerun part of the dataset using proper methods!
- Import the csv file by clicking on `File`, `Import`, `Import CSV/Text Data...`. Select `DataSet_WaterMasses_0s_cols.csv`. Change the separator to `,`. This is a simplified OTU table with only the 20<sup>th</sup> most abundant OTUs. Note that the samples are on the rows, while your variables (OTUs) are on the columns.
- Next thing is to factorise the Watergr variable which is currently numerical (we are not interested in the numbers per se, but we want to use the values to group the samples in 3 groups). Click on `Data`, `Numeric to categorical`. Select `Watergr`, and accept. You can now see that the variable is shown in yellow.
- Then, the 0s need to be replaced. There aren't many in this dataset, but we still have to care ebout them. There exist many ways to substitute them, though no one being perfect. In this version of CoDaPack, only the simplest way of treating them is available: Replacing them by a very low value. Here we gonna use the standard settings: 0.65 time the detection limit (1), so 0.65. First we need to give the detection limit: `Data`, `Set detection limit`, Select all the data, and set the detection limit to 1. Go in `Data`, `Rounded zero replacements`. Select all the data. Choose 0.65 of DL, and Closure result to 1. The columns with the modified values are added to the right of the table (starting with z_).  

*Questions*  
*1. Why are Os problematic when using the CoDA approach? Remember that in the CoDA approach we are studying log-ratios.*  
*2. Why is it difficult to find a good solution to replace 0s in the case of microbial OTU tables.*  
*3. What happened by setting up the closure to 1? Feel free to play with other values.*  

## Make a clr biplot
A biplot is a convenient tool allowing to visualise on the same plots the data points (samples) and the variables (OTUs). However, in the case of compositional data, the interpretation needs to be adapted due to the relationship between the different parts.
- Go in `Graphs`, `CLR biplot`, select all variables starting with "z_", select "Watergr" in `groups`, and accept.
- 
