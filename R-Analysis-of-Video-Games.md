Video Games with R
================

  - [Introduction](#introduction)
      - [The Data](#the-data)
      - [The Tools](#the-tools)
      - [The Questions](#the-questions)
  - [Research Questions (and
    Answers\!)](#research-questions-and-answers)
      - [What years saw the greatest amount of game
        releases?](#what-years-saw-the-greatest-amount-of-game-releases)
      - [Are certain gaming platforms more violent than
        others?](#are-certain-gaming-platforms-more-violent-than-others)
      - [Are shooting games more popular in America than other
        genres?](#are-shooting-games-more-popular-in-america-than-other-genres)
          - [In the US, are mean sales of shooting games different from
            that of other genres? What about the
            world?](#in-the-us-are-mean-sales-of-shooting-games-different-from-that-of-other-genres-what-about-the-world)
          - [Are the mean sales of Platform and Shooter games in America
            recently
            equal?](#are-the-mean-sales-of-platform-and-shooter-games-in-america-recently-equal)
      - [Is PlayStation better than
        Xbox?](#is-playstation-better-than-xbox)
          - [Are the mean sales for PlayStation games greater than
            Xbox?](#are-the-mean-sales-for-playstation-games-greater-than-xbox)
          - [Is the proportion of PlayStation games greater than Xbox
            games?](#is-the-proportion-of-playstation-games-greater-than-xbox-games)
      - [Are North American or Japanese sales better at indicating how a
        video game does
        globally?](#are-north-american-or-japanese-sales-better-at-indicating-how-a-video-game-does-globally)
  - [Limitations](#limitations)
  - [Conclusion](#conclusion)

## Introduction

Video games have become the basis of millions of childhoods across the
world. Generations can attest to ubiquitous Pacman, legendary Zelda, the
adventurous Mario Brothers, and brave Master Chief. From these legends
have arisen corporate conglomerates, cutting-edge computational power,
and a yearning fanbase.

However, what pushes some games to the top and others to crash? What
platforms are more “violent?” What region loves their sacred controllers
and game cartridges the most? Would releasing a game in North America be
more beneficial than releasing in Japan? Although these are questions of
an over-achieving gamer, the quest is to get the bottom of some of them
with R and a public dataset.

### The Data

Video games and their respective sales from 1980-2016 were analyzed to
find answers to questions regarding sales, genres, violence in video
games and regional predictions. The public data set attained from
[Kaggle](www.%20https://www.kaggle.com/gregorut/videogamesales.com)
contained:

  - Rank - Ranking of overall sales
  - Name - The games name
  - Platform - Platform of the games release (i.e. PC,PS4, etc.)
  - Year - Year of the game’s release
  - Genre - Genre of the game
  - Publisher - Publisher of the game
  - NA\_Sales - Sales in North America (in millions)
  - EU\_Sales - Sales in Europe (in millions)
  - JP\_Sales - Sales in Japan (in millions)
  - Other\_Sales - Sales in the rest of the world (in millions)
  - Global\_Sales - Total worldwide sales.

Once the data from Kaggle is downloaded, load it as a csv into RStudio.
The `file.choose()` command can be used to locate the filepath for the
file.

``` r
vgsales=read.csv("C:\\Users\\Prithvi Kinariwala\\Documents\\video-games-with-R\\vgsales.csv")
```

You’ll only need to import the data once, as we’ll copy it within the
RStudio environment for separate data wrangling.

*Keep in mind that the data is only complete from years 1980-2016. Games
marked as released past 2016 are predictions.*

### The Tools

> “It’s dangerous to go alone\! Take this.” -Old Man, The Legend of
> Zelda

Every journey requires the right tools. For this one, the following R
libraries were used. Installations are only needed once per R
installation.

``` r
install.packages("ggplot2")
install.packages("dplyr")
install.packages("RColorBrewer")
```

Next you’ll need to “activate” the individual libraries.

``` r
library(ggplot2)
library(dplyr)
library(RColorBrewer)
```

### The Questions

Questions explored were:

1.  What years saw the greatest amount of game releases?
2.  Are certain gaming platforms more violent than others?
3.  Are shooting games more popular in America?
4.  Is PlayStation better than Xbox?
5.  Are North American or Japanese sales better at indicating how a
    video game does globally?

## Research Questions (and Answers\!)

### What years saw the greatest amount of game releases?

Since the creation of the first rudimentary Pong game, video game
development has spread across the world with demand. International
developers push out games for well-known franchises as well as iconic
standalones. Although it is safe to assume that more games are in
circulation now than at the advent of video gaming, to what extent is
this difference? When exactly were the greatest amount of video games
produced and what platform was the most popular?

The best visual would be a bar graph with the release years as the
x-axis. But first, you’ll need to clean up the data by removing games
with “N/A” as the year. We make a copy of the data and clean up the copy
only as data wrangling differs from question to question.

``` r
yeargames=vgsales
yeargames<-yeargames[!(yeargames$Year=="N/A" ),]
```

Next, use the `ggplot` library to create a graph.

``` r
ggplot(data = yeargames, mapping = aes(x =Year, fill= Platform))+
  geom_bar()+
  theme(axis.text.x=element_text(angle=90,hjust=1,vjust=0.5))+
  labs(x = "Year", y="Number of Games Published",title = "Games Published 1980-2016")
```

![](R-Analysis-of-Video-Games_files/figure-gfm/timegraph-1.png)<!-- -->

*An interactive version of this graph (and the steps to create it) can
be found [at the project’s page on
RPubs.com](https://rpubs.com/prithvikin/video-games-with-R-InteractiveGraphs)*

The maximum in the timeframe with given data was 2008 and 2009. This
peak can be attributed to an overlap between the Sixth Console
Generation (1998-2013) and Seventh Console Generation (2005-present) as
well as a new demand for handheld gaming. The two years have games for
both Xbox and Xbox 360, PS2 and PS3, and handhelds like the Nintendo DS
and PSP.

The following years depict a decrease in the number of games released
mainly due to less game developers being able to create games to new
graphics specifications of Seventh Console Generation games. Effectively
put, creating games became much more intensive, with teams dedicated to
specific functions, like graphics. The change between the Seventh
Console Generation and Eighth Console Generation should portray the
same, as seen with the drop in games in 2012-2016. In summary, as
platforms become more powerful, less game studios can create games that
use the full technical capacity desired by consumers.

### Are certain gaming platforms more violent than others?

Video game platforms, like video games, have target audiences. This
audience targeting is often done with the creation and release of video
games exclusive to certain platforms as well as establishing
developmental tools and game engines that certain genres lend themselves
well to. Mario Kart’s players will never throw shells on non-Nintendo
platforms, Microsoft won’t let Master Chief save the UNSC on a
PlayStation, nor is Uncharted leaving Sony’s tight grasp.

Game developers use hardware differences to create unique games for
respective platforms. Nintendo’s origins have naturally evolved into
platform games with Nintendo originals being the most popular. Games
like Mario Kart, Mario Party, Super Smash Bros., and Wii Sports series
are Nintendo’s incentives for buying it’s consoles. On the other hand,
action, shooter, and fighting games like the Halo, *mostly* Call of
Duty, and Grand Theft Auto series are only offered on Xbox and
PlayStation consoles. Thus, the *assumptions* made by critics attribute
the Xbox and PlayStation to being more violent than their Nintendo
counterparts.

*Sidenote: Although Nintendo has recently released the Switch, the
company has pushed games similar to its older generations. Due to its
recent release and a lack of data, the Switch was not considered for the
report. Call of Duty games are released for Nintendo platforms, but
certain aspects of the game are often changed.*

To tackle the question, a graphical analysis alongside population
parameters would be sufficient. The action, shooter, and fighting genres
were determined to be “violent” while all other genres were determined
“not violent.” Counts and percentages should be analyzed to find
platforms more violent than others.

Before generating the graphs, we need to add two more character vectors
(columns) to the data. One for the major platform brand (Microsoft,
Nintendo, Sony) and one to classify genres as violent or not. Like the
first question, we create a copy of the first data set to create a
unique data set after wrangling. Then, each new vector starts out empty
at `NULL` before the for-loop iterates each row while determining each
row.

``` r
violentgames=vgsales

# For-loop to classify certain genres as Violent and others as Non-violent
Violent = NULL
for(i in 1:nrow(violentgames))
{
    if(violentgames[i,5]=="Action"|violentgames[i,5]=="Fighting"|violentgames[i,5]=="Shooter" ){Violent[i]<-"Violent"}
    else{Violent[i]<-"Not Violent"}
}

# For-loop to classify platforms by platform manufacterers 
ConsoleMaker = NULL
for(i in 1:nrow(violentgames))
{
  if(violentgames[i,3]=="DS"|violentgames[i,3]=="3DS"|violentgames[i,3]=="GB"|violentgames[i,3]=="GBA"|violentgames[i,3]=="N64"|violentgames[i,3]=="NES"|violentgames[i,3]=="Wii"|violentgames[i,3]=="WiiU"){ConsoleMaker[i]<-"Nintendo"}
  else if(violentgames[i,3]=="PS"|violentgames[i,3]=="PS2"|violentgames[i,3]=="PS3"|violentgames[i,3]=="PS4"|violentgames[i,3]=="PSP"|violentgames[i,3]=="PSV"){ConsoleMaker[i]<-"Sony"}
  else if(violentgames[i,3]=="X360"|violentgames[i,3]=="XB"|violentgames[i,3]=="XOne"){ConsoleMaker[i]<-"Microsoft"}
  else{violentgames[-i]}
}
```

Finally, we bind the seperate vectors with the video games data and
remove any lingering “N/A”s.

``` r
violentData=cbind(violentgames,Violent,ConsoleMaker)
violentData <- na.omit(violentData)
```

Now that we have prepared the data, we can finally start graphing\! The
first graph is a simple game count by platform split between violent and
non-violent games. A stacked bar graph is perfect on illustrating many
properties at once.

``` r
ggplot(data = violentData, mapping = aes(x =Platform,fill= Violent)) + 
  geom_bar()+facet_wrap(~ConsoleMaker)+
  labs(y="Number of Video Games",title = "Violence in Video Games by Major Platform Making Companies")+
  theme(axis.text.x=element_text(angle=90,hjust=1,vjust=0.5))+scale_fill_manual(values = c("steelblue1","tomato2"))
```

![](R-Analysis-of-Video-Games_files/figure-gfm/violentgraph-counts-1.png)<!-- -->

*[An interactive version of this
graph](https://rpubs.com/prithvikin/video-games-with-R-InteractiveGraphs)*  
Let’s get a percent bar graph to get another insight\! Mind that
less-popular platforms will have their differences blown out of
proportion *cough cough Wii U*.

``` r
ggplot(data = violentData, mapping = aes(x =Platform,fill= Violent))+ 
  geom_bar(position="fill")+facet_wrap(~ConsoleMaker)+
  labs(y="Percent Composition",title = "Violence in Video Games by Platform Making Companies")+
  theme(axis.text.x=element_text(angle=90,hjust=1,vjust=0.5))+scale_fill_manual(values = c("steelblue1","tomato2"))
```

![](R-Analysis-of-Video-Games_files/figure-gfm/violentgraph-percents-1.png)<!-- -->

*[An interactive version of this
graph](https://rpubs.com/prithvikin/video-games-with-R-InteractiveGraphs)*  
Although you should be able to see visual differences between the three
major brands, the last metric to answer the question would be the
population parameters themselves. We can use the `dplyr` package to
create two-way table commands to give us counts that can be used to find
the parameters.

``` r
violence_two_way_table <- violentData %>%
  group_by(ConsoleMaker, Violent) %>% 
  summarize(count = n())
violence_two_way_table
```

    ## # A tibble: 6 x 3
    ## # Groups:   ConsoleMaker [3]
    ##   ConsoleMaker Violent     count
    ##   <fct>        <fct>       <int>
    ## 1 Microsoft    Not Violent  1267
    ## 2 Microsoft    Violent      1035
    ## 3 Nintendo     Not Violent  4077
    ## 4 Nintendo     Violent      1400
    ## 5 Sony         Not Violent  4348
    ## 6 Sony         Violent      2300

From these numbers, we find the proportions with one of the most basic
functions of R, its calculator\!

``` r
# Microsoft Platform Violent Games Proportion
1035/(1035+1267)
```

    ## [1] 0.449609

``` r
# Nintendo Platform Violent Games Proportion
1400/(1400+4077)
```

    ## [1] 0.2556144

``` r
# Sony Platform Violent Games Proportion
2300/(2300+4348)
```

    ## [1] 0.3459687

The figures show that the Xbox 360 has more violent games than any
Nintendo platform. The same relationship is true of PlayStation 2 and
PlayStation 3 over the Nintendo Platform as well. This relationship is
further seen in the percent bar graphs, where WiiU’s data is skewed only
due to considerably less games being created for its platform before
being discontinued. After using the `dplyr` two-way-table function to
create a list of violent and nonviolent counts by platform company,
Microsoft had the greatest percent of violent games (44.96%) followed by
Sony (34.60%) and lastly Nintendo (25.56%). The data indicates that
Nintendo platforms tend to be less violent than Microsoft and Sony
platforms.

Does this mean you should never buy your child a Xbox or PlayStation?
**No.** My first game was Halo 2, and I turned out alright\!

### Are shooting games more popular in America than other genres?

This is a big question, and can be answered from two different
directions. If you remember, the data contained statistics regarding the
sales of each game. Consumer demand, although a bit faulty, can be used
to predict what genres of games are in demand.

#### In the US, are mean sales of shooting games different from that of other genres? What about the world?

This question employs R’s robust statistical methods. To gain a better
understanding of the data, we create a two-way table of Genres by their
mean sales in North America. Data isn’t being modified here, so we don’t
need to make a copy of it.

``` r
vgsales %>%
group_by(Genre) %>%
summarise(mean_genre = mean(NA_Sales))
```

    ## # A tibble: 12 x 2
    ##    Genre        mean_genre
    ##    <fct>             <dbl>
    ##  1 Action           0.265 
    ##  2 Adventure        0.0823
    ##  3 Fighting         0.264 
    ##  4 Misc             0.236 
    ##  5 Platform         0.505 
    ##  6 Puzzle           0.213 
    ##  7 Racing           0.288 
    ##  8 Role-Playing     0.220 
    ##  9 Shooter          0.445 
    ## 10 Simulation       0.211 
    ## 11 Sports           0.291 
    ## 12 Strategy         0.101

Right off the bat we notice that the Shooter and Platform games have
greater grossing sales. The next step would be to see if Shooter sales
are *significantly greater* than Platform games. For this, we use the
ANOVA test on the several means to determine if one of the populations
is significantly different.

``` r
anova_vg = aov(NA_Sales~Genre, vgsales)
summary(anova_vg)
```

    ##                Df Sum Sq Mean Sq F value Pr(>F)    
    ## Genre          11    165  15.027   22.86 <2e-16 ***
    ## Residuals   16586  10904   0.657                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

A p-value of 2x10^-16 is much lower than the 5% (0.05) level of
significance. Thus, we reject the null and have evidence to conclude
that at least one of the population means for NA Sales across genres is
different.

Performing multiple hypothesis testing using the `TukeyHSD()` function
would compare all the mean sales with each other and give corresponding
confidence intervals. This would allow us to better understand the
ranges of average prices for the various genres.

``` r
anova_vg = aov(NA_Sales~Genre, vgsales)
glimpse(TukeyHSD(anova_vg))
```

    ## List of 1
    ##  $ Genre: num [1:66, 1:4] -0.18245 -0.00106 -0.02882 0.23985 -0.05205 ...
    ##   ..- attr(*, "dimnames")=List of 2
    ##   .. ..$ : chr [1:66] "Adventure-Action" "Fighting-Action" "Misc-Action" "Platform-Action" ...
    ##   .. ..$ : chr [1:4] "diff" "lwr" "upr" "p adj"
    ##  - attr(*, "class")= chr [1:2] "TukeyHSD" "multicomp"
    ##  - attr(*, "orig.call")= language aov(formula = NA_Sales ~ Genre, data = vgsales)
    ##  - attr(*, "conf.level")= num 0.95
    ##  - attr(*, "ordered")= logi FALSE

**Shooter vs Others (NA)**  
Shooter\>Action  
Shooter\>Adventure  
Shooter\>Fighting  
Shooter\>Misc  
Shooter Platform  
Shooter\>Puzzle  
Shooter\>Racing  
Shooter\>Role-Playing  
Simulation\<Shooter  
Sports\<Shooter  
Strategy\<Shooter

After extracting those intervals that compare ‘Shooter’ games with other
genres, we find that Shooter games had higher mean sales than all other
genres except Platform, where the confidence interval of the difference
between the means contained 0. Thus, we cannot make any conclusions
between the mean sales for Shooter and Platform. However, we can say
that Shooter games have a greater number of sales on average compared to
the other 10 genres, in North America.

It could be that Shooter games are simply a generally popular genre of
games across the world, and establishing a correlation between the games
and violence in America could be very wrong. To test this, we used the
same process of analysis but for the Europe and Japan regions.

**Comparison with EU**

``` r
vgsales %>%
group_by(Genre) %>%
summarise(mean_genre = mean(EU_Sales))
```

    ## # A tibble: 12 x 2
    ##    Genre        mean_genre
    ##    <fct>             <dbl>
    ##  1 Action           0.158 
    ##  2 Adventure        0.0499
    ##  3 Fighting         0.119 
    ##  4 Misc             0.124 
    ##  5 Platform         0.228 
    ##  6 Puzzle           0.0873
    ##  7 Racing           0.191 
    ##  8 Role-Playing     0.126 
    ##  9 Shooter          0.239 
    ## 10 Simulation       0.131 
    ## 11 Sports           0.161 
    ## 12 Strategy         0.0666

``` r
anova_eu = aov(EU_Sales~Genre, vgsales)
summary(anova_eu)
```

    ##                Df Sum Sq Mean Sq F value Pr(>F)    
    ## Genre          11     41   3.742   14.79 <2e-16 ***
    ## Residuals   16586   4197   0.253                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
glimpse(TukeyHSD(anova_eu))
```

    ## List of 1
    ##  $ Genre: num [1:66, 1:4] -0.1085 -0.0388 -0.0341 0.0693 -0.0711 ...
    ##   ..- attr(*, "dimnames")=List of 2
    ##   .. ..$ : chr [1:66] "Adventure-Action" "Fighting-Action" "Misc-Action" "Platform-Action" ...
    ##   .. ..$ : chr [1:4] "diff" "lwr" "upr" "p adj"
    ##  - attr(*, "class")= chr [1:2] "TukeyHSD" "multicomp"
    ##  - attr(*, "orig.call")= language aov(formula = EU_Sales ~ Genre, data = vgsales)
    ##  - attr(*, "conf.level")= num 0.95
    ##  - attr(*, "ordered")= logi FALSE

**Shooter vs Others (EU)**  
Shooter\>Action  
Shooter\>Adventure  
Shooter\>Fighting  
Shooter\>Misc  
Shooter Platform  
Shooter\>Puzzle  
Shooter Racing  
Shooter\>Role-Playing  
Simulation\<Shooter  
Sports\<Shooter  
Strategy\<Shooter

In Europe, after an analysis of Shooter games compared to other genres,
we found that their mean sales were greater than all other genres except
Platform and Racing, where the confidence interval of the difference
between the means contained 0. Thus, we cannot make any conclusions
between the mean sales for Shooter and Platform, and Shooter and Racing.
However, we can say that European gamers do prefer Shooting games more
than most of the other genres. Furthermore, we can say that the top
video game genres in Europe have a broader range.

**Comparision with Japan**

``` r
vgsales %>%
group_by(Genre) %>%
summarise(mean_genre = mean(JP_Sales))
```

    ## # A tibble: 12 x 2
    ##    Genre        mean_genre
    ##    <fct>             <dbl>
    ##  1 Action           0.0482
    ##  2 Adventure        0.0405
    ##  3 Fighting         0.103 
    ##  4 Misc             0.0620
    ##  5 Platform         0.148 
    ##  6 Puzzle           0.0985
    ##  7 Racing           0.0454
    ##  8 Role-Playing     0.237 
    ##  9 Shooter          0.0292
    ## 10 Simulation       0.0735
    ## 11 Sports           0.0577
    ## 12 Strategy         0.0726

``` r
anova_jp = aov(JP_Sales~Genre, vgsales)
summary(anova_jp)
```

    ##                Df Sum Sq Mean Sq F value Pr(>F)    
    ## Genre          11   53.2   4.838   52.29 <2e-16 ***
    ## Residuals   16586 1534.5   0.093                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
glimpse(TukeyHSD(anova_jp))
```

    ## List of 1
    ##  $ Genre: num [1:66, 1:4] -0.00775 0.05477 0.01373 0.09936 0.05023 ...
    ##   ..- attr(*, "dimnames")=List of 2
    ##   .. ..$ : chr [1:66] "Adventure-Action" "Fighting-Action" "Misc-Action" "Platform-Action" ...
    ##   .. ..$ : chr [1:4] "diff" "lwr" "upr" "p adj"
    ##  - attr(*, "class")= chr [1:2] "TukeyHSD" "multicomp"
    ##  - attr(*, "orig.call")= language aov(formula = JP_Sales ~ Genre, data = vgsales)
    ##  - attr(*, "conf.level")= num 0.95
    ##  - attr(*, "ordered")= logi FALSE

**Shooting vs Others (Japan)**  
Shooter Action  
Shooter Adventure  
Shooter\<Fighting  
Shooter Misc  
Shooter\<Platform  
Shooter\<Puzzle  
Shooter Racing  
Shooter<Role-Playing  
Simulation>Shooter  
Sports Shooter  
Strategy Shooter

In Japan, after an analysis of mean sales of Shooter games compared to
other genres, we found that the Shooter game sales were inconclusive for
some and less than many other genres, on average. It shows that they are
not very popular in Japan, and Japan’s consumers prefer to play other
genres. With this, we can conclude to some extent that Shooter games are
more popular in America due to their position of highest mean sales
being shared by only one other genre.

#### Are the mean sales of Platform and Shooter games in America recently equal?

Platform games have been around since the very start of the video game
industry. It was the main game genre in the market while the video game
industry was smaller, and thus dominated sales before the 2000s. Thus,
adding those earlier values into the sample may not paint the most
recent picture between the difference in sales between Platform and
Shooter games, on average, in North America. Thus, to determine a more
recent measure of the current popularity between the two genres, we
compare the two mean number of sales for Shooter and Platform using data
after the year 2005. The year was critical in the gaming industry and
marked the start of the Seventh Generation of Consoles, characterized by
the launch of the Wii, Xbox 360 and the PlayStation 3.

The data was wrangled to transform the factor column of years to a
numerical column of “Years from 1980.” This allows us to filter out the
years before 2005, along with removing the NAs from the Year variable
that were causing problems post-transform.

``` r
yeargames<-vgsales[!(vgsales$Year=="N/A" ),]
numeric_year = transform(yeargames, Year = as.numeric(Year))
data_2005 = numeric_year[which(numeric_year$Year > 25),]
final_data = data_2005[which(data_2005$Genre == "Platform" | data_2005$Genre == "Shooter"),]
```

Next, we can create a test to compare two means (t test) with a set
confidence level. For this example, we sent with a 90% confidence
interval.

``` r
t.test(NA_Sales ~ Genre, final_data, conf.level = 0.90)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  NA_Sales by Genre
    ## t = -1.8538, df = 860.38, p-value = 0.06411
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 90 percent confidence interval:
    ##  -0.22389787 -0.01324981
    ## sample estimates:
    ## mean in group Platform  mean in group Shooter 
    ##              0.3592162              0.4777900

We are 90% confident that the mean sales for Platform games is between
223897.87 and 13249.81 dollars lower than Shooter games.

To finally prove that mean sales for Platform games are less than
Shooter games, we can use a hypotheisis test.

``` r
t.test(NA_Sales ~ Genre, final_data, alternative = "less")
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  NA_Sales by Genre
    ## t = -1.8538, df = 860.38, p-value = 0.03206
    ## alternative hypothesis: true difference in means is less than 0
    ## 95 percent confidence interval:
    ##         -Inf -0.01324981
    ## sample estimates:
    ## mean in group Platform  mean in group Shooter 
    ##              0.3592162              0.4777900

level of significance = 10%  
p-value = 3.21%

We have enough statistical evidence to conclude that mean sales for
Platform games are less than Shooter games. This makes shooter games the
more popular gaming genre in the USA.

### Is PlayStation better than Xbox?

Finally, the million dollar question. Although I may be forever biased
for my love for the Master Chief/Cortana duo, what do the sales say?

To start, we add a PlayStation and Xbox column. This time around, we’ll
use the `ifelse` command within the `dplyr` library. Next, as we are
only comparing consoles (and not handhelds) we remove all platforms not
explicitly Xbox or PlayStation. “PCFX” was accidentally grouped with
Xbox, but it is easier to remove it afterwards.

``` r
vgsales2 = vgsales %>%
  mutate(Xbox_or_Playstation = ifelse(grepl("X", Platform), "Xbox", ifelse(grepl("PS", Platform), "PlayStation", NA)))

vgsales2 = vgsales2[-(which(vgsales2$Platform == "PCFX")),]
vgsales3 = vgsales2[-(which(vgsales2$Platform == "PSP" | vgsales2$Platform == "PSV")),]
vgsales4 = na.omit(vgsales3)
```

#### Are the mean sales for PlayStation games greater than Xbox?

For this, we can use the `dplyr` package to summarize the mean global
sales by Platform.

``` r
vgsales4 %>%
  group_by(Xbox_or_Playstation) %>%
  summarize(mean_sales = mean(Global_Sales))
```

    ## # A tibble: 2 x 2
    ##   Xbox_or_Playstation mean_sales
    ##   <chr>                    <dbl>
    ## 1 PlayStation              0.642
    ## 2 Xbox                     0.599

Next, we first construct a confidence interval for the difference of
**Global** Xbox and Playstation sales.

``` r
t.test(Global_Sales ~ Xbox_or_Playstation, vgsales4, conf.level = 0.95)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  Global_Sales by Xbox_or_Playstation
    ## t = 1.3179, df = 4273, p-value = 0.1876
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.02070424  0.10562206
    ## sample estimates:
    ## mean in group PlayStation        mean in group Xbox 
    ##                 0.6416249                 0.5991659

``` r
t.test(Global_Sales ~ Xbox_or_Playstation, vgsales4, alternative = "greater")
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  Global_Sales by Xbox_or_Playstation
    ## t = 1.3179, df = 4273, p-value = 0.09381
    ## alternative hypothesis: true difference in means is greater than 0
    ## 95 percent confidence interval:
    ##  -0.01054576         Inf
    ## sample estimates:
    ## mean in group PlayStation        mean in group Xbox 
    ##                 0.6416249                 0.5991659

The result was a 95% confidence interval that contains 0 and
corresponding p-value of 0.09381 greater than the 5% level of
significance (0.05). Thus, we do not have enough statistical evidence to
conclude that the mean global sales of PlayStations are greater than the
mean global sales for Xbox. This means we cannot say one console is more
popular, and thus better, than the other due to a higher number of
global sales and general global popularity.

We can still create the same confidence interval again, but this time
for North American Sales.

``` r
t.test(NA_Sales ~ Xbox_or_Playstation, vgsales4, conf.level = 0.95)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  NA_Sales by Xbox_or_Playstation
    ## t = -5.1068, df = 3325.3, p-value = 3.461e-07
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.13519526 -0.06018289
    ## sample estimates:
    ## mean in group PlayStation        mean in group Xbox 
    ##                 0.2806472                 0.3783362

``` r
t.test(NA_Sales ~ Xbox_or_Playstation, vgsales4, alternative = "less")
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  NA_Sales by Xbox_or_Playstation
    ## t = -5.1068, df = 3325.3, p-value = 1.73e-07
    ## alternative hypothesis: true difference in means is less than 0
    ## 95 percent confidence interval:
    ##         -Inf -0.06621558
    ## sample estimates:
    ## mean in group PlayStation        mean in group Xbox 
    ##                 0.2806472                 0.3783362

However, restricting the data to just NA Sales, the 95% confidence
interval of the difference between PlayStation’s mean sales and Xbox’s
mean sales is between -135195.26 and -60182.89. The corresponding
p-value is 1.73 x 10^-7, which is much lower than the 5% level of
significance. Thus, we have enough evidence to conclude that on average,
the sales for PlayStation games are less than the sales for Xbox games
in North America, making Xbox the more popular and better console in the
region.

#### Is the proportion of PlayStation games greater than Xbox games?

For this comparison, we use the data including the NA (other console)
values in the dataset and find the proportion of Xbox and PlayStation
games of the total number of games released for all consoles.

``` r
ggplot(vgsales3, aes(x = Xbox_or_Playstation)) + geom_bar(fill = brewer.pal(3, "Set1"))+labs(y="Count", x="Console" ,title = "Video Games per Platform")
```

![](R-Analysis-of-Video-Games_files/figure-gfm/xbox_or_playstation-1.png)<!-- -->

*[An interactive version of this
graph](https://rpubs.com/prithvikin/video-games-with-R-InteractiveGraphs)*  
Using a bar graph, we can see that the number of games available for
PlayStation is much higher than that for Xbox. We further this analysis
by comparing the two proportions using a confidence interval with a
confidence level of 95%.

``` r
prop.test(c(5022,2302), c(14971,14971), conf.level = 0.95)
```

    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(5022, 2302) out of c(14971, 14971)
    ## X-squared = 1336.3, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.1721000 0.1912692
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.3354485 0.1537639

``` r
prop.test(c(5022,2302), c(14971,14971), alternative = "greater")
```

    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(5022, 2302) out of c(14971, 14971)
    ## X-squared = 1336.3, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: greater
    ## 95 percent confidence interval:
    ##  0.1736302 1.0000000
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.3354485 0.1537639

This allows us to conclude that we are 95% confident that the proportion
of PlayStation games is between 0.1721000 and 0.1912692 greater than the
proportion of Xbox games. A hypothesis test further elaborates on this
conclusion, giving a p-value of 2.2x10-16 which is much lower than the
5% level of significance. Hence, PlayStation users have historically had
a greater number of games to choose from and the console is better than
Xbox based on this comparison.

### Are North American or Japanese sales better at indicating how a video game does globally?

Amongst the markets in the data, game development occurs most in North
America and Japan. Microsoft and Sony are headquartered in the United
States whereas Nintendo is Japan’s largest game conglomerate. Game
development studios often release games domestically to gauge demand and
fix bugs before releasing to the global market. However, this prediction
method only works as well as the ability of video game sales in domestic
markets to estimate international sales.

Two scatterplots with linear regression can allow us to make preliminary
conclusions. North American and Japanese Sales would be the predictory
variables in their respective graphs whereas Global Sales will be the
response variable. The `geom_smooth` allows us to have a linear
regression line drawn in the graph.  
**NA - Global Sales**

``` r
ggplot(data = vgsales, mapping = aes(x = NA_Sales, y =Global_Sales))+   
  geom_point(alpha=0.25)+
  labs(x = "North America Sales (million)",y="Global Sales (million)",title="Global Sales by North American Sales")+
  geom_smooth(method = "lm", se = FALSE)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](R-Analysis-of-Video-Games_files/figure-gfm/na-global-sales-1.png)<!-- -->

*[An interactive version of this
graph](https://rpubs.com/prithvikin/video-games-with-R-InteractiveGraphs)*

``` r
NAGlobalModel<-lm(Global_Sales ~ NA_Sales, data = vgsales)
summary(NAGlobalModel)
```

    ## 
    ## Call:
    ## lm(formula = Global_Sales ~ NA_Sales, data = vgsales)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -20.0071  -0.1086  -0.0528   0.0172  11.6456 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 0.063202   0.004292   14.72   <2e-16 ***
    ## NA_Sales    1.791827   0.005000  358.38   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.526 on 16596 degrees of freedom
    ## Multiple R-squared:  0.8856, Adjusted R-squared:  0.8856 
    ## F-statistic: 1.284e+05 on 1 and 16596 DF,  p-value: < 2.2e-16

The linear regression line of North America vs Global sales had a slope
of 1.791827 with an R2 value of 0.8856. This meant that for every
million sales in North America, a video game, on average, made 1.791827
million total global sales. The R^2 value being sufficient enough to
indicate a strong correlation between sales in North America and Global
sales. The slope being close to 1.0 indicated that North America’s sales
are a very good indicator of Global sales.

**JP - Global Sales**

``` r
ggplot(data = vgsales, mapping = aes(x = JP_Sales, y =Global_Sales))+   
  geom_point(alpha=0.25)+
  labs(x = "Japan Sales (million)",y="Global Sales (million)",title="Global Sales by Japanese Sales")+
  geom_smooth(method = "lm", se = FALSE)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](R-Analysis-of-Video-Games_files/figure-gfm/jp-global-sales-1.png)<!-- -->

*[An interactive version of this
graph](https://rpubs.com/prithvikin/video-games-with-R-InteractiveGraphs)*

``` r
JPGlobalModel<-lm(Global_Sales ~ JP_Sales, data = vgsales)
summary(JPGlobalModel)
```

    ## 
    ## Call:
    ## lm(formula = Global_Sales ~ JP_Sales, data = vgsales)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -10.408  -0.319  -0.198   0.062  70.845 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 0.298181   0.009845   30.29   <2e-16 ***
    ## JP_Sales    3.076039   0.030871   99.64   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.23 on 16596 degrees of freedom
    ## Multiple R-squared:  0.3743, Adjusted R-squared:  0.3743 
    ## F-statistic:  9929 on 1 and 16596 DF,  p-value: < 2.2e-16

The linear regression line of Japan vs Global sales had a slope of
3.076039 with R^2 value of 0.3743. This meant that for every million
sales in Japan, a video game, on average, made 3.076039 million in total
global sales.

Next, we can use the `predict` function to predict how a theoretical
quantity of NA or JP sales could predict Global sales.

**North America/Global Sales Prediction**

``` r
NApredictData<-data.frame(NA_Sales=c(35))
predict(NAGlobalModel, NApredictData)
```

    ##        1 
    ## 62.77716

Using this model, if a game got 35 million of NA Sales, its total global
sales could be predicted to be 62.77716 million.

**Japan/Global Sales Prediction**

``` r
JPpredictData<-data.frame(JP_Sales=c(35))
predict(JPGlobalModel, JPpredictData)
```

    ##        1 
    ## 107.9596

Using the same parameters as the model for the NA model, if a game
earned 35 million in Japanese sales, it can be predicted that it would
earn 107.9596 million total global sales.

## Limitations

The dataset didn’t contain information on games released for the year
2017 and after. Despite there not being too many major changes to the
gaming industry since that year, the composition of game genres and mean
sales may be different if this data was included. Furthermore, it would
paint a more recent picture of the data and the conclusions. We may see
a greater increase in the sales for the Shooting games genre as the
years progress. Genres themselves were a bit too broad. The “Fighting”
genre encompassed both the gory Mortal Kombat series as well as the
child-oriented Super Smash Bros. series. Many dates were also missing.
Furthermore, we may also see a greater increase in the number of Xbox
games that could begin to rival that of the PlayStation in recent times.
There are many more indicators that make one console better than the
other, such as specifications, compatibility with new releases, user
experience ratings, etc. which we did not cover and could better answer
the question of comparing Xbox and PlayStation.

## Conclusion

> “The numbers, Mason\! What do they mean?” -Jason Hudson, Call of Duty:
> Black Ops

Some of the key findings were that 2008 and 2009 featured the greatest
number of games entering the market due to an overlap in console
generations. Microsoft and Sony platforms tend to have more violent
games than Nintendo platforms, due to the console brand’s target
audiences. Shooting games were more popular in America compared to other
regions, perhaps because of the country’s fascination with guns and
their ownership along with the establishment of longtime shooter
franchises. PlayStation ranked better than Xbox on our indicator of game
variety, and Xbox ranked better on sales in North America. However, they
both were inconclusive based on global sales. Our last finding concluded
that the North American market is a better indicator of how a game
released domestically would do internationally than the Japanese market.

It has been one heck of a journey, it’s only a matter of time until we
embark on our next one\!
