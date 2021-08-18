# CoDaPack-Introduction
Hands on session on CoDA for the NORBIS summer school 2021. This session has been designed by Sven Le Moine Bauer, Vera Pawlowsky-Glahn and Juan José Egozcue. It follows a presentation from Vera and Juan José (see pdf).

In this hands on session, you will learn some basics of CoDA. As the session is very short, the aim here is not to cover extensively the methods available in CoDA, but rather to have you understand the logic of reasoning with compositional data and ratios. We use an old version of CoDaPack (v.2.02.21) as the new version is currently not available for Linux. [CoDaPack](http://ima.udg.edu/codapack/) is a software suited for beginners as it does not request any knowledge in coding.

## Open CoDaPack
* Open the friday directory on the desktop. Right click inside, and select "Open in the terminal".
* Enter: `java -jar CoDaPack-2.02.21-jar-with-dependencies.jar`

## Preparation of the dataset
The dataset that you will use in this hands on session comes from a paper I published in 2018: </br> [Water Masses and Depth Structure Prokaryotic and T4-Like Viral Communities Around Hydrothermal Systems of the Nordic Seas](https://www.frontiersin.org/articles/10.3389/fmicb.2018.01002/full) </br>
At that time, I did not use CoDA to analyse the data, so here is the opportunity to rerun part of the dataset using proper methods!
* Import the csv file by clicking on `File`, `Import`, `Import CSV/Text Data...`. Select `DataSet_WaterMasses_0s_cols.csv`. Change the separator to `,`. This is a simplified OTU table with only the 20<sup>th</sup> most abundant OTUs. Note that the samples are on the rows, while your variables (OTUs) are on the columns.
* Next thing is to factorise the Watergr variable which is currently numerical (we are not interested in the numbers per se, but we want to use the values to group the samples in 3 groups). Click on `Data`, `Numeric to categorical`. Select `Watergr`, and accept. You can now see that the variable is shown in yellow. These 3 groups corresponds to 3 water masses from which the samples were taken.
* Then, the 0s need to be replaced. There aren't many in this dataset, but we still have to care ebout them. There exist many ways to substitute them, though no one being perfect. In this version of CoDaPack, only the simplest way of treating them is available: Replacing them by a very low value. Here we gonna use the standard settings: 0.65 time the detection limit (1), so 0.65. First we need to give the detection limit: `Data`, `Set detection limit`, Select all the data, and set the detection limit to 1. Go in `Data`, `Rounded zero replacements`. Select all the data. Choose 0.65 of DL, and Closure result to 1. The columns with the modified values are added to the right of the table (starting with z_).  

*Question 1. Why are Os problematic when using the CoDA approach? Remember that in the CoDA approach we are studying log-ratios.*  
*Question 2. Why is it difficult to find a good solution to replace 0s in the case of microbial OTU tables.*  
*Question 3. What happened by setting up the closure to 1? Feel free to play with other values.*  

## The clr biplot
A biplot is a convenient tool allowing to visualise on the same plots the data points (samples) and the variables (OTUs). However, in the case of compositional data, the interpretation needs to be adapted due to the relationship between the different parts.
* Go in `Graphs`, `CLR biplot`, select all variables starting with "z_", select "Watergr" in `groups`, and accept.
* There are several things to understand about the clr biplot produced by CoDaPack:
  * The biplot is made of several parts: 
    * The 3 first principal components (PC). These are the axes of the plot.
    * Markers for each of the data points (Samples). You can double click on it, the line number of the sample in the OTU table will appear.
    * A ray (arrow) representing the variability of each single part. Careful though, one ray should not be interpreted independently from other rays. 2 rays heading in the same direction provide roughly the same information. 2 rays that are overlapping are proportional.
    * Links are the imaginary segment between the tip of 2 rays and are roughly proportional to the stdev of the log ratio between the two OTUs. Links are what should be used to understand the relationship between your variables. If 2 links are orthogonal, then there is no correlation between these 2 pairs of OTUs.
  * The plot is in 3D, showing the 3 first principal components (PC). It can be rotated by clicking/holding the graph and moving the mouse. You can click on the `XY`, `YZ`, `XZ` buttons to come back to specific orientations.
  * 
  * In the output from CoDaPack (behind the plot), you can see the amount of variance explained by each PC in the "Cum.Prop.Exp." column.
  * Back to the plot, you can look at the formula on the bottom right:
    * Z<sub>clr</sub> is your input data, in this case the clr-transformed OTU table. 
    * The right part of the equation is the singular value decomposition of Z<sub>clr</sub>. In short it is a mathematical tool that decomposes a matrix in 3 other matrices (U, Γ and V) and allows to perform PCA. U would be the matrix containing scores of the PCA, V<sup>T</sup> are the loadings, and Γ is a diagonal matrix containing the standard deviation of each component. More info on SVD [here](https://towardsdatascience.com/singular-value-decomposition-and-its-applications-in-principal-component-analysis-5b7a5f08d0bd) and on the biplots for compositional data [here](https://dugi-doc.udg.edu/handle/10256/13607).
  * Under the equation, you can move a cursor from the covariance biplot to the form biplot. You will then see that the equation and the visualisation slightly change. The covariance biplot is optimal to study the rays (the covariance between OTUs), while the form biplot plots the points in ilr coordinates and is optimal to analyse the clustering of your samples. In the form biplot, the distance between the points is the aitchison distance (ie. euclidean distance on clr data).
* So let's look at this plot now. By looking at the table output, you can see that the 2 first PCs explain 90% of the variance, which is very nice.  

*Question 4. How can you by looking at the covariance biplot understand that much of the variance is explained by PC1 and PC2, while PC3 does not bring much more to the picture?*  

* In CoDaPack, go in `Statistics`, `Compositional statistic summary`, select all OTUs starting with "z_", and no specific grouping. The output table gives you the variance of the log ratio between 2 OTUs. Small values (dark blue) mean that throughout all samples, the ratio between these 2 OTUs does no change too much, while high values (dark red) mean that the ratio changes a lot throughout your samples.  

*Question 5. Which aspect of the biplot allow you to visualise these variances? Try to look for the highest and smallest values on the biplot.*  
*Question 6. Find some links (ie. ratios) that allow you to explain what happens to the compositions when moving from the green group to the red group. Similarly, how do you segregate the blue roup from the 2 others?*

## The CoDA dendrogram
The biplot we just did is a good tool to describe the data, but can become complicated when there are too many variables (rays). The aim of the dendrogram is to be able to visualise the variability of your data using a restricted amount of OTUs. In our case here, several groups of rays can be found to go in the same directions, so they convey nearly the same information.
* Let's start by building the dendrogram: `Graphs`, `Balance dendrogram`. Select the following OTUs starting with "z_": 5, 7, 9, 11, 15. Select manually the partition. Click A15, next, B7, B11, next, C9 or C15, next, the last one (D), accept. Select groups (watergr). Note that the selection of the OTUs can be done using "expert knowledge" (ie. looking at the biplot or using previous knowledge) or using appropriate tools (Principal balances, selbal, among others, but this is beyond the scope of this session).

*Question 7. By looking at the biplot, can you think why we selected these OTUs to build the dendrogram?*  

* Here is how one can interpret a dendrogram:
  * The dendrogram is a sequential binary partitioning. That means that at each step, the variance explained by the balance (ie. log ratio betwen the geometric means) between two groups of OTUs is being studied. 
  * Let's look at the first balance (z_O15 vs. the rest, near the root of the dendrogram).
    * The vertical lines show the variance explained by the balance: The longer the bar, the more variance is explained. The black bar is for all samples, the coloured bars are within each group. So here the variance within groups is smaller than the variance between groups, which is good to discriminate the different groups. 
    * The position of the bar along the horizontal line (left to right) is the mean value of the balance for the group (or all samples for the black line). 
    * The boxplot shows the distribution of the sample within each group.
    * So, this balance seems very good to separate Gr2 (blue) from Gr1 and Gr3 (green and red).
  * The principle is same with the following balances, each of them explaining more or less well a part of the variance.

*Question 8. Which discrimination allows the second balance? Look back at the form biplot, could you have predicted the results of the two first balances?*  
*Question 9. Look at the two last balances (z_O9 vs. z_O5 and z_O7 vs. z_O11). Do they explain much variance? Do they allow to discriminate the groups? Which issue arises with these two balances? Could you have predicted this from the form biplot?*  

## Bonus questions
*Question 10. Does it mean 1 increse the other one decrease?*  
*Question 11. Aitchison distance is perturbation ivariant.*  
*Question 12. Subcompositional invariance.*  
*Question 13. Scale invariance.*  
*What happens if you do not close the dataset*  


