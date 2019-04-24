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

FS



## Section 4: Results
_where you discuss the results_

## Section 5: Discussion
_where you discuss any observations you noted and any recommendations you can make based on the results._

## Section 6: Conclusion
_where you conclude the report_


