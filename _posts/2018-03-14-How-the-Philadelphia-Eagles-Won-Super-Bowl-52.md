---
header:
  image: /assets/foles.jpg

author_profile: true

classes: wide

---

## A statistical breakdown of the birdgang's success in the 2017-2018 season

As a lifelong Giants fan, it was painful to watch the Philadelphia Eagles win their first championship 
since 1960, and their first Super Bowl victory in the history of the franchise. Oh well, gone are the days of using the default end all argument with the hypothetical question “How many Super Bowl rings do the Eagles have?” It makes me sick to my stomach.

Although the Eagles are not my favorite NFL team in the world (ranked 31/32 of my favorite teams right in front of Dallas), from an objective standpoint, the Philadelphia Eagles performed at an outstanding level this past season. They ended the season with a 13-3 win/loss record. The birdgang also cliched the NFC East division title by week 14, but this seemed like a pyrric victory with the loss of QB Carson Wentz to a torn ACL and LCL. With backup quarterback Nick Foles now at the helm, even the media inside the city of Philadelphia wrote this team off as possibly losing the divisional round game. As for the fans of the other 31 NFL teams, they doubted the Eagle's ability to sustain their level of success.

So how did this team manage to win a Super Bowl against the invincible New England Patriots featuring an offensive shootout that totaled over 1000 passing yards? In the words of Jason Kelce, 
“underdogs are hungry dogs.” Our main focus is diving into the regular season statistics of key players, comparing the 2016 to 2017 season to observe which facets of the team markedly improved over the course of one year. We'll also look at the magic carpet ride of their playoff run, and how this obstensibly ragtag team beat Tom Brady on the football world's biggest stage in Super Bowl LII. The power of data visualization will shed light on the Philadelphia Eagle’s rise to power from last place in the NFC East Division in 2016 go one of the best teams in pro football one year later.

## Data Source

### nflscrapR

NFL data for statistical analysis is notoriously difficult to obtain, unlike other sports such as Baseball and Basketball where statistics for on-field/on-court improvement have been established. Initially I considered using [SportRadar](https://www.sportradar.com/choose_region/), but their lack of open source options turned me away from their proprietary software. But after an additional week of research, I found [nflscrapR](https://github.com/maksimhorowitz/nflscrapR) by [Ronald Yurko](https://twitter.com/Stat_Ron), Samuel Ventura, and Maksim Horowitz. This R package scraps data from the official NFL API; the deliverables are clean datasets, boxscores, and more advanced statistics for _every_ NFL play in _every_ season since 2009. 

>"This package allows NFL data enthusiasts to examine each facet of the game at a more insightful level. The creation of this package puts granular data into the hands of the any R user with an interest in performing analysis and digging up insights about the game of American Football" - Maksim Horowitz

Although this list is not inclusive of every function this package provides, there are several important functions used frequently for this analysis: 
- season_games(): provides end of game results with an associated game ID and home/away team abbreviations. 
- player_game(): parses player level game summary statistics. 
- season_player_game(): binds the results of player game() for all games in a season.
- game_play_by_play(): parses play by play data, then uses data manipulation commands to extract detailed information about each play in a game(players involved in action, play type, penalty information, air yards gained, yards gained after the catch, etc).
- season_rosters(): returns a dataframe of all rostered players and their non-performance statistics for a specified team. 

{% highlight r %}

# Must install the devtools package
install.packages('devtools')
library(devtools)

devtools::install_github(repo = "maksimhorowitz/nflscrapR")
library(nflscrapR)

{% endhighlight %}


## Framework for Data Visualizations

### Shiny

Once the pertinent data is tabulated via nflscrapR, data manipulation and visualization can begin with [Shiny](http://shiny.rstudio.com), an open source R package that builds interactive web applications similar to D3.js. It also has the capability to build dashboards similar to Tableau. So think of Shiny as a cross between D3.js and Tableau. 

### teamcolors R Package

To represent the teams of individual players in the data visualizations, I used [Ben Baumer's](https://github.com/beanumber?page=1&tab=repositories) R package [teamcolors](https://github.com/beanumber/teamcolors).

{% highlight r %}

devtools::install_github("beanumber/teamcolors")
library(teamcolors)
library(dplyr)
nfl_colors <- teamcolors %>% filter(league == "nfl")

{% endhighlight %}

## NFL WAR (Wins Above Replacement) 

Wins above replacement is a non-standardized sabermetric statistic commonly used in baseball to summarize a player's total contribution to their team. A player's WAR value answers the question "If this player got injured and their team had to replace them with a freely available player from their bench, how much value would the team be losing?" This value is expressed in a wins format. So a player with a WAR value of 6+ approximately contributes an additional 6 wins compared to a backup or third string (exception to this rule: Nick Foles). 

Although this statistic has been optimized for baseball, Yurko et al. developed a methology to implement [WAR for offensive players in the NFL](https://arxiv.org/abs/1802.00998). 


## Carson Wentz QBR

{% highlight r %}
ggplot(wentz_2016, aes(x = dates2, y = qbr_stats, color = factor(szn))) 
+ geom_line(color = "#a5acaf") + ylim(0,100) + scale_x_date(date_breaks = "1 month") 
+ ggtitle("Carson Wentz, QBR 2016") + xlab("Time Progression") + ylab("QBR (0 - 100)") 
+ theme(legend.position="none")

ggplot(wentz_2017, aes(x = dates2, y = qbr_stats, color = factor(szn))) 
+ geom_line(color = "#004953") + ylim(0,100) + scale_x_date(date_breaks = "1 month")
+ ggtitle("Carson Wentz, QBR 2017") + xlab("Time Progression") + ylab("QBR (0 - 100)") 
+ theme(legend.position="none")

{% endhighlight %}

<figure class="half">
    <img src="/assets/2018-03-14-How-the-Philadelphia-Eagles-Won-Super-Bowl-52/wentz_qbr_2016_02.jpeg" >
    <img src="/assets/2018-03-14-How-the-Philadelphia-Eagles-Won-Super-Bowl-52/wentz_qbr_2017.jpeg" >
    <figcaption> Source: [ESPN](http://www.espn.com/nfl/player/gamelog/_/id/2573079/carson-wentz) </figcaption>
</figure>





