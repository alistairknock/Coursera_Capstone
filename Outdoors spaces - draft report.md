# Outdoor spaces, culture, student satisfaction and research excellence at UK universities
## Section 1: Introduction/Business Problem
_where you discuss the business problem and who would be interested in this project._
### Alistair Knock

Prospective students to universities in the UK are now able to access a wealth of quantitative information about the courses and universities they could study at.  Websites such as Unistats.com, whatuni.com, and national government initiatives such as the Research Excellence Framework and Teaching Excellence Framework provide rich statistical measurements which allow comparison and benchmarking between a set of shortlisted university or even courses at the same university.  Staff may also use these indicators as a measure of prestige – indeed, several UK university league tables have adopted many of the same indicators in forming the methodology used for their annual rankings.

While teaching and research reputation are clearly important contributors to the decision of staff and students to join a given university, they do not fully describe the experience of spending several years in one physical location.  The metrics are output-oriented – for instance, student satisfaction at the very end of the course, the employability of graduates, the end-product outputs and impacts of academic research, and so on.  They do not generally provide information about the day-to-day reality of living, working and studying in an environment which is often campus-based or in a region of the city which is now dominated by a monoculture of university buildings.

For students and staff who place high value on the concept of the ‘university of life’, where the experience and the environment is important as well as the acquisition of knowledge and application of knowledge into research, there is an information gap around ‘what it is like to be there’ – the physical environment, green spaces and parklands, gyms and sports provision, and opportunities to engage with cultural events and visits to galleries and museums.  Most students will attend a handful of university open days once they’ve formed a shortlist, a list which will often be academically determined based on course content and predicted grades – but could we help provide ‘soft’ information on the 160 UK universities through the combination of geodata, student satisfaction and research excellence data, so that these shortlists can be informed by a broader set of criteria?  Do satisfied students tend to be concentrated in certain types of environment?  Is world-class research related to the facilities and locale of the university?

## Section 2: Data

The business problem identifies several datasources to explore:

* student satisfaction
* research excellence
* geographic data describing the area around each university

These datasets can be obtained publicly and via registered access to geodata systems as follows:

* student satisfaction data is published by the UK's Office for Students through the [National Student Survey](https://www.officeforstudents.org.uk/advice-and-guidance/student-information-and-data/national-student-survey-nss/get-the-nss-data/)
* research excellence data is captured through the Research Excellence Framework exercise administered by sector agencies, with the [most recent data available from REF 2014](https://www.ref.ac.uk/2014/results/intro/)
* basic geographic data on UK universities including latitude and longitude is available from the [UK Learning Providers website](http://learning-provider.data.ac.uk/)
* detailed geographic data on facilities and services in the vicinity of each universities is available through the [Foursquare API](https://developer.foursquare.com/docs/api/venues/search)

This report focuses on the 166 UK ‘mainstream’ higher education universities as described by the [UK Learning Providers data.ac.uk website](http://learning-provider.data.ac.uk/), which provides name and location data for universities.  While alternative providers have emerged in the past 5 years, there is not sufficient historical contextual data to include them in this analysis – for instance, the last research excellence exercise ran in 2014.  The UK Learning Providers dataset gives latitude and longitude coordinates for each university.  

### Geographic facility data

From these coordinates, [Foursquare data](https://developer.foursquare.com/docs/api/venues/search) was obtained giving the count and types of 'positive facilities' (‘venues’ in Foursquare language) within a five kilometre radius of the university – this radius was selected since it is the average human hourly walking speed and was felt to be a reasonable maximum distance that staff and students would travel to access a facility.  Clearly longer distances can be travelled using public transport or car, but the assumption is that facilities which are within walking or cycling distance will be used regularly, and it is regular use which contributes to the ‘what it is like to be there’ aspects of this investigation.  These Foursquare queries focused on specific venue categories as follows:

* Arts & Entertainment
  *	Amphitheater
  *	Aquarium
  *	Art Gallery
  *	Comedy Club
  *	Concert Hall
  *	Exhibit
  *	Historic Site
  *	Memorial Site
  *	Movie Theater
  *	Museum
  *	Music Venue
  *	Performing Arts Venue
  *	Public Art
  *	Stadium
  *	Zoo
* Event
  *	Conference
  *	Convention
  *	Festival
  *	Music Festival
  *	Sporting Event
  *	Street Fair
  *	Trade Fair
* Outdoors & Recreation
  *	Athletics & Sports
  *	Bathing Area
  *	Beach
  *	Bike Trail
  *	Botanical Garden
  *	Campground
  *	Canal
  *	Castle
  *	Forest
  *	Harbor / Marina
  *	Lake
  *	Mountain
  *	National Park
  *	Nature Preserve
  *	Other Great Outdoors
  *	Park
  *	Pedestrian Plaza
  *	Plaza
  *	Pool
  *	Rafting
  *	Recreation Centre
  *	River
  *	Rock Climbing Spot
  *	Scenic Lookout
  *	Sculpture Garden
  *	Ski Area
  *	Skydiving Drop Zone
  *	State/Provincial Park
  *	Summer Camp
  *	Trail
  *	Waterfall
* Professional & Other Places
  *	Auditorium
  *	Ballroom
  *	Community Center
  *	Cultural Center
  *	Library
  *	Observatory
  *	Social Club
  *	Spiritual Center

For each university therefore, _n_ venue categories will be present, each with a count of venues of that category within 5 kilometres of the university.  In this report we refer to Foursquare venues which appear in one of these categories as a 'positive facility' - i.e. a facility venue which fits the criteria of including natural/cultural provision and which could be considered to fostering a healthy and positive environment for work and study.

While FourSquare's _search_ endpoint can be provided with a comma-separated list of categories, it was discovered that the API will only ever return 50 results and when combined the 5 kilometre radius this meant that requests would frequently hit this limit, limiting the usefulness and completeness of the data.  The categories were therefore queried individually - this resulted in many more queries to the API (around 10,000) but a richer dataset.  The data for each category was cached locally as a CSV file once obtained so that the API need only be used once.

### Research excellence data

Where available for a university, data from the [Research Excellence Framework (REF) 2014](https://www.ref.ac.uk/2014/results/intro/) was incorporated into the university data.  The REF exercise is conducted periodically in the United Kingdom as a means of assessing the research quality of universities.  The REF dataset gives an assessment of research quality for each ‘unit of assessment’ (i.e. subject) where the quality of research is graded as 4 star (world-leading) down to 1 star (nationally recognised).  The data is supplied with staff size information which can be re-processed to form university-wide quality ratings using staff full-time equivalent (FTE) figures as a weighting.

Consideration was given to the appropriate measure of quality to use (e.g. since the quality bands are proportions falling into 4 star to 1 star, different measures such as a _grade point average_ or a _simple percentage rated 4 star_ can be used), and since both 3 star and 4 star are deemed to be internationally excellent, the metric chosen is the proportion of submissions falling in the 3 and 4 star higher quality bandings.

The assessment exercise looks separately at the quality of research outputs, the impact of the research beyond academia, and the research environment at that university.  In the source data these can be combined using weightings to give an Overall score here, but since this is a duplicate of the individual metric scores the Overall score was not used with preference for more granular data and a higher number of features.

### Student satisfaction data

Where available for a university, data from the [National Student Survey 2018](https://www.officeforstudents.org.uk/advice-and-guidance/student-information-and-data/national-student-survey-nss/get-the-nss-data/) was incorporated into the university data.  This dataset gives an assessment of student satisfaction for universities overall as well as by subject, across a range of 27 questions.  Again, student population sizes were available should further processing be required.

The student survey questions can be clustered into a number of dimensions (for instance, the first four questions are in the 'The teaching on my course' dimension.  There are eight dimensions, plus a final question on 'Overall, I am satisfied with the quality of the course'.  The attention of the higher education sector overall is often on the this final question as a simple measure of satisfaction, but there is often notable variation per institution in the dimension scores which can highlight exceptional over or under performance which is not visible in the overall satisfaction question.  This question also is tightly bunched, with satisfaction across universities limited to a ~10 percentage point range.

## Section 3: Methodology
_which represents the main component of the report where you discuss and describe any exploratory data analysis that you did, any inferential statistical testing that you performed, and what machine learnings were used and why._

With the data available, universities can be compared solely by the geographic and contextual factors, by the degree of research excellence, or by the levels of student satisfaction – and these factors can be brought together to explore any correlation between the various features present in the data.  The methodology iterated several times, from a simple feature set using all institutions, to a geographically limited dataset to remove outliers, and then a more much detailed feature set which used granular data from Foursquare and NSS rather than subtotals.

### Iteration 1, simple features

We began by obtaining copies of the three relevant flat files with the aim of removing unneeded columns and to restructure them into one dataframe which contains one record per university, the latitude/longitude, the overall student satisfaction measure, and research excellence measures.

We regularly used statistics and data visualisations to check the integrity of the dataset, for instance ensuring that the geography of universities was appropriate before running the Foursquare queries as in figure 1:

![map of UK](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure1.PNG)

_Figure 1: UK map plotting all universities_

The full set of features from the three data sources contains 3 research indicators (excluding the overall subtotal), 27 satisfaction questions (including the discrete satisfaction question for now since it is a separate question rather than a subtotal), and 59 Foursquare categories.  This gives a total of 89 features to consider.  Initially in the first iteration we focused on a simple subset of these to explore and understand the data, particularly whether there was any clear relationship between the variables.  This iteration used three features (the axis label is specified in single quotes):

* REF outputs, 'Outputs'
* NSS question 27, 'NSS_Q27'
* Foursquare subtotal of positive venues across all specified categories, 'TotalCount'

**Table 1: Summary statistics on the Foursquare dataset**

| Description | Figure |
|---|---|
|Total number of universities | 166 |
|Total number of positive venues |80948 |
|Total number of categories | 59 |
|Average positive venues per universities | 487.64 |
|Average positive venues per category | 1372.00 |

The distribution of facilities in each category was reviewed to check whether the results matched up with expectations given knowledge of the general geography of the UK:

![histogram showing total count of positive facilities across all universities split by category](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure2.PNG)

_Figure 2: Histogram showing total count of positive facilities across all universities split by category_

Of the 81,0000 venues found, parks and churches constitute over 10%, followed by art galleries, gyms and fitness centres.  This seemed appropriate given the relatively green and historic environment of the UK.

We then looked at variability using a boxplot showing the spread of universities within each category:

![box plot distribution of universities and their count of positive facilities in each category](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure3.PNG)

_Figure 3: Box plot distribution of universities and their count of positive facilities in each category_

The box plots show a reasonably high range between upper and lower quartile, which reflects the mixture of urban and rural universities in the UK.  Some of these plots give a clue to the geography - for instance, harbor/marina has a comparatively low median value, but high upper quartile and outlier markers, because where some harbor facilities exist there are likely to be many more around it due to proximity to the coastline.  The same pattern can be observed with Art Gallery (cities) and Trail (countryside).

Finally, we used a histogram to observe the number of universities which have X numbers of venues around them to identify whether the distribution is relatively normal.  At this point a total count of all venues in all categories is added (i.e. the total number of venues within 5 kilometres of the university which are of the 'positive' type of category listed).  This gives a way of comparing the relative environment of each university.

![histogram showing counts of universities with X positive facilities](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure4.PNG)

_Figure 4: Histogram showing counts of universities with X positive facilities_

The high reading in the last bar of this histogram (circa 25 universities with more than 1000 positive facilities within radius) seemed unusual, with a suspicion that it was related to London. So we mapped the results again with count as the colour (black as the highest count):

![UK map focused on London and the south east, with black markers showing the 1000+ positive facility universities](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure5.PNG)

_Figure 5: UK map focused on London and the south east, with black markers showing the 1000+ positive facility universities_

Since there is the possibility of London universities behaving as exceptional cases due to the density of the city, we can return to this in a subsequent iteration with the option of excluding these outlier universities.

We now assessed the relationships between the three main features with scatter plots and a quadratic linear regression:

![scatter plot of universities using NSS question 27 on the X axis and REF outputs on the Y axis](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure6.PNG)

_Figure 6: Scatter plot of universities using NSS question 27 on the X axis and REF outputs on the Y axis_

![scatter plot of universities using NSS question 27 on the X axis and positive facility total count on the Y axis](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure7.PNG)

_Figure 7: Scatter plot of universities using NSS question 27 on the X axis and positive facility total count on the Y axis_

![scatter plot of universities using REF outputs  on the X axis and positive facility total count on the Y axis](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure8.PNG)

_Figure 8: Scatter plot of universities using REF outputs on the X axis and positive facility total count on the Y axis_

The initial scatters indicated there may be a weak negative relationship between student satisfaction - access to greater numbers of venues correlates to a small decrease in satisfaction. Conversely, the quality of REF outputs is positively correlated with the number of venues.  The approach taken to further investigate this was to using k-means clustering and DBSCAN to cluster the data and observe the differences in features between these clusters. Mapping these clusters also helped to identify any confounding and contributing geographical factors not currently present in the featureset.

First, we scaled the features and use the elbow method to identify an appropriate number of clusters (k=4) and then ran k-means clustering on the dataset.  (Running a test using DBScan required high epsilon values (0.5 with a normalised range of 0-1.0) and low minimum sample values (10 from 166) to begin to separate any cluster from the noise, which likely indicates that the dataset does not cluster well (either in general or using this density method).)

We again assessed the relationships between the three main features with scatter plots and a quadratic linear regression:

**Table 2: Pearson's correlation co-efficient and R-squared values for each combination:**

| Combination | Pearson's | R-squared | p-value |
|---|---|---|---|
|NSS Q27 and outputs | 0.096 | 0.009 | 0.259 |
|NSS Q27 and positive facility count | -0.390 | 0.152 | <0.001 |
|Outputs and positive facility count | 0.327 | 0.107  | <0.001 |


![clustered scatter plot of universities using NSS question 27 on the X axis and REF outputs on the Y axis](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure9.PNG)

_Figure 9: Clustered scatter plot of universities using NSS question 27 on the X axis and REF outputs on the Y axis_

Notably cluster 2 does not appear since it has zero NSS values and so is excluded from the graph.

Cluster 3 appears distinct from the other two clusters, and cluster 0 has a very wide range, with points overlapping much of cluster 1.

The regression line is weak and the spread of the clusters does not initially support close correlation.

![clustered scatter plot of universities using NSS question 27 on the X axis and positive facility total count on the Y axis](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure10.PNG)

_Figure 10: Clustered scatter plot of universities using NSS question 27 on the X axis and positive facility total count on the Y axis_

In contrast to the NSS/REF plot, here cluster 0 is distinct from other clusters, with all points showing high values for the positive venues feature.  As before, the zero NSS values mean cluster 2 is not plotted.  Cluster 1 and 3 overlap considerably and are not discrete in this view.  The regression line shows a reasonably strong negative correlation, though this may be influenced by the atypically high values from cluster 0.

![clustered scatter plot of universities using REF outputs  on the X axis and positive facility total count on the Y axis](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure11.PNG)

_Figure 11: Clustered scatter plot of universities using REF outputs on the X axis and positive facility total count on the Y axis_

This chart shows REF outputs against count of positive venues.  With the exception of cluster 2, here we have clear separation of the clusters, with cluster 0 remaining in the top positive venue range, and the other clusters being separated cleanly by low and mid-high REF performance.  The regression line shows positive correlation.

Box plots were used to view the distribution of values within each cluster, summarised in table 3 below and which confirms that cluster 2 contained no NSS results, cluster 3 contained weak REF results, and cluster 0 contained very high counts of positive vanues - notably while cluster 1 and 3 contain small quantities of positive venues, cluster 2 shows significant range.

**Table 3: Cluster labels with lower and upper quartiles**
(note that positive facilities is standardised to 100% in this table, whereas NSS and REF outputs show the original percentage score from the dataset)

| Number | Label | Colour | Number of universities | NSS Q27 lower | NSS Q27 upper | REF output lower | REF output upper| Positive facility lower | Positive facility upper | 
|---|---|---|---|---|---|---|---|---|---|
| 0 | High positive facilities, low NSS, high REF | Red | 20 | 78% | 82% | 61% | 78% | 90% | 97% |
| 1 | Low positive facilities, high NSS, high REF | Purple | 16 | 82% | 87% | 55% | 74% | 14% | 36% |
| 2 | High positive facilities, no NSS, high REF | Blue | 97 | n/a | n/a | 59% | 83% | 32% | 97% |
| 3 | Low positive facilities, high NSS, low REF | Yellow | 33 | 80% | 86% | 29% | 36% | 13% | 36% |


### Iteration 2, focusing on a subset of clusters

![map of UK universities including clusters as colours](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure12.PNG)

_Figure 12: UK map plotting all universities with k-means clusters as colours_

The counts of universities in each cluster are not even, with the largest cluster (97) holding nearly three times the quantity of the second largest (33).  When mapped, the purple cluster contains a large and diverse number of geographic points and London effectively has its own cluster (red), which is presumably a function of the density of all types of venues in the capital when compared to less dense urban areas and rural areas elsewhere in the country.  To investigate this further, we set aside the London cluster and the cluster which has no student satisfaction data, to focus on the remaining 130 institutions which have less polarised features.

We ran the same k-means clustering and exploratory visualisations on this subset, again with k=4 providing the optimal number of clusters.

**Table 4: Pearson's correlation co-efficient and R-squared values for each combination, excluding London:**

| Combination | Pearson's | R-squared | p-value |
|---|---|---|---|
|NSS Q27 and outputs | 0.214 | 0.046 | 0.018 |
|NSS Q27 and positive facility count | -0.239 | 0.057 | 0.008 |
|Outputs and positive facility count | 0.272 | 0.074  | 0.003 |

![Clustered scatter plot of universities using REF outputs on the X axis and positive facility count on the Y axis](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure13.PNG)

_Figure 13: Clustered scatter plot of universities using REF outputs on the X axis and positive facility count on the Y axis_

**Table 5: Cluster labels with lower and upper quartiles, subset excluding London and NSS non-respondents**
(note that positive facilities is standardised to 100% in this table, whereas NSS and REF outputs show the original percentage score from the dataset)

| Number | Label | Colour | Number of universities | NSS Q27 lower | NSS Q27 upper | REF output lower | REF output upper| Positive facility lower | Positive facility upper | 
|---|---|---|---|---|---|---|---|---|---|
| 0 | High positive facilities, low NSS, high REF | Red | 40 | 81% | 85% | 59% | 75% | 35% | 40% |
| 1 | Low positive facilities, high NSS, high REF | Purple | 40 | 84% | 88% | 60% | 76% | 10% | 21% |
| 2 | High positive facilities, low NSS, low REF | Blue | 11 | 79% | 86% | 29% | 35% | 36% | 39% |
| 3 | Low positive facilities, low NSS, low REF | Yellow | 39 | 81% | 85% | 33% | 49% | 9% | 19% |

![map of UK universities including clusters as colours, subset excluding London](https://github.com/alistairknock/Coursera_Capstone/raw/master/os_figure14.PNG)

_Figure 14: UK map plotting all universities with k-means clusters as colours, subset excluding London and NSS non-respondents_

Again, running a test using DBScan required high epsilon values (0.5 with a normalised range of 0-1.0) and low minimum sample values (10 from 130) to begin to separate any cluster from the noise, which likely indicates that the dataset does not cluster well (either in general or using this density method).

### Iteration 3, all features

Finally, we returned to the original three datasets and merged them to form a highly detailed dataset which included all 27 NSS questions, 3 profiles from REF, and a large number of venue categories with counts of positive venues.  (60 categories were provided to the API call, but Foursquare appears to return similar/subcategories in addition, so the total number of features in this dataset is now 340).  Our interest was mainly to test whether the simplified three-feature set was a reasonably proxy for the more granular data.

The same process of k-means clustering was run on this detailed set (this time k=5) and the counts of universities falling into these five clusters was compared with the counts in the original four clusters.  Table 6 summarises these counts:

**Table 6: Counts of universities in simple clustering (4 way) vs detailed clustering (5 way)**

| Simple \ Detailed | 0 | 1 | 2 | 3 | 4 | 
|---|---|---|---|---|---|
| High positive facilities, low NSS, high REF | 1 | 16 | - | 1 | 2 |
| Low positive facilities, high NSS, high REF  | 55 | - | - | 42 | - |
| High positive facilities, no NSS, high REF | - | 4 | 7 | - | 5 |
| Low positive facilities, high NSS, low REF | 15 | - | - | 18 | - |

Taken at face value, original cluster 0 (London) equates to new cluster 1; the NSS non-respondent group is dispersed into other clusters, presumably since other features now have increased weight to offset the absence of one of the three original features; and interestingly, clusters 2 and 3 are each split in near-equal proportions into clusters 0 and 3; that is, half of cluster 2 and half of cluster 3 are now in cluster 0.

## Section 4: Results
_where you discuss the results_

## Section 5: Discussion
_where you discuss any observations you noted and any recommendations you can make based on the results._

## Section 6: Conclusion
_where you conclude the report_


