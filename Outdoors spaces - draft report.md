# Outdoor spaces, culture, student satisfaction and research excellence at UK universities
## Section 1: Introduction/Business Problem
_where you discuss the business problem and who would be interested in this project._
### Alistair Knock

Prospective students to universities in the UK are now able to access a wealth of quantitative information about the courses and universities they could study at.  Websites such as Unistats.com, whatuni.com, and national government initiatives such as the Research Excellence Framework and Teaching Excellence Framework provide rich statistical measurements which allow comparison and benchmarking between a set of shortlisted university or even courses at the same university.  Staff may also use these indicators as a measure of prestige – indeed, several UK university league tables have adopted many of the same indicators in forming the methodology used for their annual rankings.

While teaching and research reputation are clearly important contributors to the decision of staff and students to join a given university, they do not fully describe the experience of spending several years in one physical location.  The metrics are output-oriented – for instance, student satisfaction at the very end of the course, the employability of graduates, the end-product outputs and impacts of academic research, and so on.  They do not generally provide information about the day-to-day reality of living, working and studying in an environment which is often campus-based or in a region of the city which is now dominated by a monoculture of university buildings.

For students and staff who place high value on the concept of the ‘university of life’, where the experience and the environment is important as well as the acquisition of knowledge and application of knowledge into research, there is an information gap around ‘what it is like to be there’ – the physical environment, green spaces and parklands, gyms and sports provision, and opportunities to engage with cultural events and visits to galleries and museums.  Most students will attend a handful of university open days once they’ve formed a shortlist, a list which will often be academically determined based on course content and predicted grades – but could we help provide ‘soft’ information on the 160 UK universities through the combination of geodata, student satisfaction and research excellence data, so that these shortlists can be informed by a broader set of criteria?  Do satisfied students tend to be concentrated in certain types of environment?  Is world-class research related to the facilities and locale of the university?

## Section 2: Data
_where you describe the data that will be used to solve the problem and the source of the data._

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

### Research excellence data

Where available for a university, data from the [Research Excellence Framework (REF) 2014](https://www.ref.ac.uk/2014/results/intro/) was incorporated into the university data.  This REF dataset gives an assessment of research quality for each ‘unit of assessment’ (i.e. subject) where the quality of research is graded as 4 star (world-leading) down to 1 star (nationally recognised).  The data is supplied with staff size information which can be re-processed to form university-wide figures.  

TALK ABOUT Consideration will be given to the appropriate measure of quality to use (e.g. grade point average versus percentage rated 4*), and to the elements of assessment used (the assessment exercise looks separately at the quality of research outputs, the impact of the research beyond academic, and the research environment at that university).

REF data is captured per university and per unit of assessment (subject) - we are looking for a university average, so we must recalculate this average using the individual unit scores and the weighting dimension (staff full-time equivalent (FTE) gives a relative weighting between units).

REF data also exists for four profiles - Outputs, Impact, Environment and then an Overall score. We transform the data for each of these four profiles.

The REF assessment categorises each university's submission into 4 quality bands (plus unclassified), where 4* is the best and 1* is the lowest band.

There are different methods of aggregating up from the 4 quality bands, including a grade point average. In this report we have decided to focus on the proportion of submissions which attract the top quality bands (4 and 3 star submissions) as a way of differentiating between research excellence at universities, so we create a new column to hold this subtotal, and then pivot the dataset so that there is one row per university and a 3+4 star score for each of the profiles. This is the same structure as the main university dataset so we can then merge it to create our single version of the three static data files.

### Student satisfaction data

Where available for a university, data from the [National Student Survey 2018](https://www.officeforstudents.org.uk/advice-and-guidance/student-information-and-data/national-student-survey-nss/get-the-nss-data/) was incorporated into the university data.  This dataset gives an assessment of student satisfaction for universities overall as well as by subject, across a range of 27 questions.  Again, student population sizes were available should further processing be required.

TALK ABOUT Q27 and dimensions
TALK ABOUT MISSING DATA

### Dataset processing summary

Through, universities can be compared solely by the geographic and contextual factors, by the degree of research excellence, or by the levels of student satisfaction – and these factors can be brought together to explore any correlation between the various features present in the data.

We began by obtaining copies of the three relevant flat files with the aim of removing unneeded columns and to restructure them into one dataframe which contains one record per university, the latitude/longitude, the student satisfaction measure, and research excellence measures.

FS

The getMatchingVenues function takes lat/long and a single category (it would also take a list of comma-separated categories but here we have chosen to iterate through each category individually, since early tests showed that many results of a multi-category search would exceed the 50 result Foursquare limit and so be limited in their usefulness. By querying each category individually we increase the number of calls to the Foursquare API but increase the possible number of results substantially and lower the risk of hitting the 50 result limit).

The writecsv argument can be provided to write a static CSV copy of the results to avoid hitting the API unnecessarily. These CSVs can be deleted should a refresh of the data be required.


## Section 3: Methodology
_which represents the main component of the report where you discuss and describe any exploratory data analysis that you did, any inferential statistical testing that you performed, and what machine learnings were used and why._

## Section 4: Results
_where you discuss the results_

## Section 5: Discussion
_where you discuss any observations you noted and any recommendations you can make based on the results._

## Section 6: Conclusion
_where you conclude the report_

