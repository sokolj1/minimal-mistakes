---
header:
  image: /assets/foles.jpg

author_profile: true

classes: wide

---

## Dashboard visualization of Super Bowl XLVI - LII showing play by play win probabilities and play descriptions

As a lifelong Giants fan, it was painful to watch the Philadelphia Eagles win their first championship 
since 1960, and their first Super Bowl victory in the history of the franchise, on February 4th, 2018. Gone are the days
of using the default end all argument with the hypothetical question “How many Super Bowl rings do the Eagles have?” 
The idea is anathema to any Giants fan and any football fan that has a disdain for the birdgang (Cowboys, Redskins, Patriots, etc). 

But as someone who has a love for the game of Football, Super Bowl LII was an incredibly entertaining matchup that set several precedents, such as: 
- 1,151 total offensive yardage, more than 200 yards more than any previous Super Bowl.
- 


After reading these facts, I wanted to dive deeper into this Super Bowl. I wanted to see the pivotal turning points in the game, and how these plays affected both teams chances of winning the game at the given moment. So I decided to create an interactive data visualization deliverable for my Data Visualization semester project. 

## Data Source
Determining the right data source wasn't easy, considering data science/statistics for Football does not have the luxury of a built-up analytics infastructure like Baseball as of 2018. I looked into proprietary software like [SportRadar](https://www.sportradar.com/) but I determined paying for a few months of access to their API was not worth the money. Surreptitiously, while researching data sources, I found the open-source R package [nflscrapR](https://github.com/maksimhorowitz/nflscrapR), which scrapes data off the official NFL API. Although I was looking to work with Python due to instructor preference, I was already fluent in R from implementing machine learning techniques and a Data Exploration course from Fall 2017, so I knew the learning curve would be minimal to take advantage of nflscrapR. 

### nflscrapR
The package Github page provides several examples of querying the official NFL API to help new users hit the ground running. Two important aspects of the package are the following:
- Game level analysis.
- Data available after the 2009 season.

Discovering the data reaches back to 2009 made me reassess the scope of my project: why not visualize team data from _every_ Super Bowl between 2009 and present day? One game didn't seem like enough work for a semester long project. Besides, this meant Super Bowl XLVI (46) would be included, when the Giants upset the Patriots a second time for their 4th franchise Super Bowl. All for that! 

## Choosing the Right Data Visualization Tool



### Tableau

<iframe src = "https://public.tableau.com/views/SuperBowlWinProbabilities/SuperBowl46-52?:showVizHome=no&:embed=true" width="1100" height="785"></iframe>



