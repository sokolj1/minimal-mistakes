---
header:
  image: /assets/foles.jpg

author_profile: true

classes: wide

---

## Dashboard visualization of Super Bowl XLIV - LII showing win probabilities and play descriptions

As a lifelong Giants fan, it was painful to watch the Philadelphia Eagles win their first championship 
since 1960, and their first Super Bowl victory in the history of the franchise. Gone are the days
of using the default end all argument with the hypothetical question “How many Super Bowl rings do the Eagles have?” 
The idea is anathema to any Giants fan and football fans that has a disdain for the birdgang (Cowboys, Redskins, Patriots, etc). 

But as someone who loves the game of Football, Super Bowl LII was an incredibly entertaining matchup that established several precedents, such as: 
- 1,151 total offensive yardage (Patriots 613 and Eagles 513), 200 yards more than any previous Super Bowl.
- The Eagles are the first team in NFL history (regular season or postseason) to win a game despite allowing more than 600 yards.
- The Eagles are the 4th team in NFL history to win the Super Bowl after having a losing record the year before.  
Source: [Sportingnews](http://www.sportingnews.com/nfl/news/super-bowl-52-eagles-patriots-stats-fast-facts-records-milestones/1kbmcltvjrukzzty6cpb8796y).

Considering the magnitude of this game, the following question arose: 

>*What were the turning points of the game, the key plays that had the most impact, and how did these plays affect the likelihood of either team winning at any given moment? 

The manifestation of these questions is an interactive Tableau dashboard. The time remaining in the game (independent variable) is plotted against win probability (dependent variable), a statistical tool that suggests a team's likelihood of winning after each play. The true value of Tableau is the ability to swap between Super Bowls in one view, and accessing each play description using tooltips. 

## Data Source
Determining the right data source wasn't easy. Statistical analysis with American Football does not have the luxury of a built-up infastructure like Baseball as of May 2018. I looked into proprietary software like [SportRadar](https://www.sportradar.com/) but I determined paying for a few months of API access was not worth it. Surreptitiously, while researching data sources, I found the open-source R package [nflscrapR](https://github.com/maksimhorowitz/nflscrapR), which scrapes data off the official NFL API.

### nflscrapR
The nflscrapR Github page provides several examples of querying the official NFL API to help new users hit the ground running. Two important aspects of the package are the following:
- Game level analysis.
- Data available after the 2009 season.

Discovering the data goes back to 2009 made me reassess the scope of my project: why not visualize key plays and win probabilities from _every_ Super Bowl between 2009 and present day?

|                 Super Bowl   | Teams (away vs. home)                          |
|                    ---       |        ---                                     |
| Super Bowl XLIV (44)         | New Orleans Saints vs. Indianapolis Colts      |
| Super Bowl XLV (45)          | Pittsburgh Steelers vs. Green Bay Packers      |
| Super Bowl XLVI (46)         | New York Giants vs. New England Patriots       |
| Super Bowl XLVII (47)        | Baltimore Ravens vs. San Francisco 49ers       |
| Super Bowl XLVIII (48)       | Seattle Seahawks vs. Denver Broncos            |
| Super Bowl XLIX (49)         | New England Patriots vs. Seattle Seahawks      |
| Super Bowl 50                | Carolina Panthers vs. Denver Broncos           |
| Super Bowl LI (51)           | New England Patriots vs. Atlanta Falcons       |
| Super Bowl LII (52)          | Philadelphia Eagles vs. New England Patriots   |


### Data Cleaning & Preprocessing
This step is extremely important in the project workflow. Knowledgeable manipulation skills can save hours of work that can be devoted to data interpretation and implementing good data visualization practices, while ensuring data integrity.

Download the nflscrapR package directly from Github using a few keystrokes in RStudio:
{% highlight r %}
# need 'devtools' to download packages from Github
install.packages('devtools')
devtools::install_github(repo = "maksimhorowitz/nflscrapR")

# load the package
library(nflscrapR)
{% endhighlight %}

The following is R code for retrieving the win probability statistics for Super Bowl LII, and a quick ggplot graphic for exploratory data analysis. The complete R script is available [here](https://github.com/sokolj1/Super-Bowl-Data-Visualization/blob/master/data_visualization_nflscrapR.R).

{% highlight r %}
# import additional libraries for data viz and manipulation
library(ggplot2)
library(dplyr)

# extract the statistics for the last game of the 2017 season (Super Bowl LII)
super_bowl52 <- game_play_by_play(GameID = tail(extracting_gameids(2017, 
playoffs = TRUE), n = 1))

# queries time remaining after each play, home team win probability, away team win probability, and play description 
eagles_pats <- data.frame(super_bowl52$TimeSecs,super_bowl52$Home_WP_post,
super_bowl52$Away_WP_post, super_bowl52$desc)

# omit erroneous instances where home team win probability == away team win probability
eagles_pats_final <- na.omit(eagles_pats[!(eagles_pats$super_bowl52.Home_WP_post == eagles_pats$super_bowl52.Away_WP_post),])

# rename columns
colnames(eagles_pats_final) = c("time_remaining", "Home", 
"Away", "Play Description")

# ggplot of Super Bowl LII for EDA
ggplot(eagles_pats_final, aes(x = eagles_pats_final$time_remaining, 
y = eagles_pats_final$Home)) + geom_line(aes(x = eagles_pats_final$time_remaining, 
y = eagles_pats_final$Home,color = "#c60c30")) + 
geom_line(aes(x = eagles_pats_final$time_remaining, 
y = eagles_pats_final$Away, color = "#004953")) + 
scale_x_reverse(breaks = c(3600, 3300, 3000, 2700, 2400, 2100, 
1800, 1500, 1200, 900, 600, 300, 0), labels = c("Kickoff", "", "","End of Q1","","", 
"Halftime", "","","End of Q3","","","End of Regulation")) + ylab("Win Probability") 
+ xlab("") + ggtitle("Super Bowl LII Win Probability Chart") + 
scale_color_manual(values=c("#004953", "#c60c30"), labels = c("PHI", "NE")) + 
labs(color = "", caption = "Source: nflscrapR") 
{% endhighlight %}

<img src="../assets/2018-04-05-Interactive-Super-Bowl-Tableau-Dashboard/PHI_NE_PLOT.jpeg" align="center" > 

A few noticeable observations is the Eagles commanded the greater win probability for a majority of the game, suggesting the Eagles were in the drivers seat with the exception of a few minutes in the first quarter and the final minutes of the game. Although this is a great visualization tool, the graphic doesn't provide context for the data itself, such as what play occurred that resulted in a change in win probability. This involves another layer of data complexity, preferably with plot interactivity. Unfortunately, ggplot does not support this functionality, so Tableau was my go to interactive data viz solution. 

One nflscrapR attribute for game_play_by_play data is play description after each play, so this attribute is ideal for providing the user context with respect to win probability. After extracting the win probablities and play description, the individual dataframes were concatenated using rbind(), then written to a [csv file](https://github.com/sokolj1/sokolj1.github.io/blob/master/assets/2018-04-05-Interactive-Super-Bowl-Tableau-Dashboard/super_bowl46_52.csv). 

Perhaps the biggest challenge was extracting and tabluating the team scores data. nflscrapR only provides the possession team and defensive team scores. So the scores had to be organized by possession and defense for each team, then joined together by the common fields of TimeRemaining and Super Bowl.

{% highlight r %}
# PHI Score
# filters by possession team 
sb52_phi_pos <- super_bowl52 %>% filter(super_bowl52$posteam == "PHI")

# queries time remaining and scores when Philly possessed the ball 
sb52_phi_pos <- data.frame(sb52_phi_pos$TimeSecs, sb52_phi_pos$PosTeamScore)

# rename columns
colnames(sb52_phi_pos) = c("TimeRemaining", "Score")

# filters by defensive team 
sb52_phi_def <- super_bowl52 %>% filter(super_bowl52$DefensiveTeam == "PHI")

# queries time remaining and scores when Philly played defense
sb52_phi_def <- data.frame(sb52_phi_def$TimeSecs, sb52_phi_def$DefTeamScore) 

# rename columns
colnames(sb52_phi_def) = c("TimeRemaining", "Score")

# join both possession and defensive dataframes by common field TimeRemaining
sb52_phi_merge <- merge(sb52_phi_pos, sb52_phi_def, by = "TimeRemaining", all = TRUE)

{% endhighlight %}

The dataframe needs to be reversed to show beginning of the game counting down to the end; NA values also need to be removed.

{% highlight r %}
# reverses the dataframe so TimeRemaining is organized in decreasing order, also removes na values 
sb52_phi_scores  <- cbind(sb52_phi_merge[1], mycol = apply(sb52_phi_merge[-1], 1, max, na.rm = TRUE))
sb52_phi_scores <- sb52_phi_scores[dim(sb52_phi_scores)[1]:1,]

# rename columns
colnames(sb52_phi_scores) = c("TimeRemaining", "Away")
{% endhighlight %}

This is just for one team for Super Bowl LII. Unfortunately, not all the data was clean and valid. I cross validated the scores after each significant play with ESPN, and for a few games the scores were incorrect. A notable example was Super Bowl 50, so I had to manually correct the scores of the dataframe with the appropriate timeRemaining value. Albeit a tedious process, the result of rigourous data cleaning was another separate [csv file](https://github.com/sokolj1/sokolj1.github.io/blob/master/assets/2018-04-05-Interactive-Super-Bowl-Tableau-Dashboard/super_bowl_scores.csv) that contains the time remaining, home and away scores, and corresponding Super Bowl. Now the data is ready for visualization. 

## Choosing the Right Data Visualization Tool
As an intern at AtlantiCare Health System, I picked up Tableau to create dashboards for doctors and administrative staff to track prevalence of Venus Thromoembolism (VTE). With this experience, I learned the idiosyncrasies of the high level business intelligence software. After looking into open-source alternatives like Plotly and Bokeh, the interactivity is there, but customization of the interactivity is limited. Tableau's tooltip is customizable and has little to no lag time between hover over and displaying information. Although Tableau tutorials are out of scope for this post, here are a few links to help get started with Tableau: 

- [Udemy Tableau 10 Desktop Training](https://www.udemy.com/tableau-10/learn/v4/content).
- [Official Tableau Training Videos](https://www.tableau.com/learn/training).

Students can download a one-year [Tableau Desktop](https://www.tableau.com/academic/students) license for _free_. 
For everyone else, Tableau skills can still be honed with [Tableau Public](https://public.tableau.com/en-us/s/), the medium that I will use to ultimately post my Tableau workbook. 

### Final Deliverable: Tableau Dashboard
The end product is the dashboard below rendered using Tableau Public. The workbook can be downloaded by clicking on the 'download' icon on the bottom right corner of the dashboard.

<iframe src = "https://public.tableau.com/views/SuperBowlWinProbabilities/SuperBowl46-52?:showVizHome=no&:embed=true" width="1100" height="805"></iframe>

Since I created this dashboard for a data visualization course, I incorporated the three key concepts that were learned during the semester: Tufte's Principles, Kosslyn's Principles, and Cairo's Wheel. A few major implementations of these ideas are described below, and can be accessed using the tooltip in the dashboard: 

- Tufte's Principles: 
1. Above all else, show the data. The focus should be on the content of the data. 
2. Data-ink / total ink. One should maximize the data-ink ratio, within reason.
3. Emphasize important data elements using color.
- Kosslyn's Principles:
1. The color scheme should be cogent and simple to understand in context of the visualized data
2. Understanding breaks down once too much information. Provide neither too much or too little information.
3. Provide neither too much or too little information.
- Cairo's Wheel:
1. Functionality (0.9)  to Decoration (0.1): Measures the embellishment/decorative detail incorporated into the visualization.
2. Multidimensionality (0.7) to Familiarity (0.2): Assess the structure of the visualization and the dimensions used to access additional data
3. Density (0.7) to Lightness (0.3): Measures the granularity of the data. 

I hope this post serves as an example for how powerful Tableau dashboards can be for visualizing data. Although I am an open-source data science advocate at heart, Tableau is my preferable tool for data visualization because of the seamless interactivity it provides for accessing additional information about the data.

