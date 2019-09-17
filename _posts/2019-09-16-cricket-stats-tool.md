---
Layout: post
toc: true
section-type: post
header:
  teaser: assets/images/database_schema.png
title: "Cricketscreener - A Tool to Anaylze ball by ball Cricket Data"
category: python
description: "cricketscrrener is a tool to analyse ball by ball data for more than 4000+ matches"
tags: ['python','cricket','cricket stats']
---
I have been playing with cricket data a lot and thought of sharing with everyone a tool I developed to analyse ball by ball data from more than 4000+ matches.
I have named this tool as [CRICKETSCREENER](http://cricketscreener.com/) and just released the first version of it. This tool can be helpful to anyone who wishes to play with ball by ball cricket data.

**NOTE: [cricketscreener.com](http://cricketscreener.com/) works best on desktop browsers**

**Video demo on how to use [cricketscreener.com](http://cricketscreener.com/)**

[DEMO](https://youtu.be/2DkFoFoEvaI)

## Cricket Data Source
The data used for this tool has been taken from [cricsheet](https://cricsheet.org/). I would be very thankful to the cricsheet team to make this data available online.

About cricsheet:
> Cricsheet is Retrosheet for Cricket. We provide ball-by-ball data for Men’s and Women’s Test Matches, One-day internationals, Twenty20 Internationals, some other international T20s, and all Indian Premier League seasons.

> At the moment we have ball-by-ball information for 4,418 matches, comprising 415 Test matches, 6 other multi-day matches, 1,594 One-day internationals, 199 other one-day matches, 821 T20 internationals, 157 international T20s, 756 Indian Premier League matches, 292 Big Bash League matches, 121 NatWest T20 Blast matches, and 57 Pakistan Super League matches featuring 62 countries, 47 club teams, and 2 representative XIs going back as far as 2009 (for women), and 2005 (for men).

Missing matches: [missing matches](https://cricsheet.org/missing/)

## Using the data
Cricsheet provides ball by ball data in many formats such as yaml, csv and xml. As I was alreday good with playing with xml data in python, I chose to use the xml data files.

Data download: [downloads](https://cricsheet.org/downloads/)

I wrote a python script to extract the data from xml files and insert it into a database. My long term goal would be to open-source these scripts. 

Database Schema:

![schema]({{site.baseurl}}/assets/images/database_schema.png)

At the present moment, I have just a single table and it is not optimised for speed and space. Each row in this table is a ball data from a particular match.
Most of the columns in the table should be self-explanatory. Let me exaplain all attributes.

**Match attributes**

1. mid - Each match has a unique match id.
2. gender - This will be either male or female.
3. Date - The day on which the match started.
4. team1 - One of the team that is involved in the match.
5. team2 - other team that is involved in the match.
6. venue - Where is the match being played?
7. match_type - Which format of cricket - odi, test or t-20?
8. comp - This is only specific for t-20 matches(IPL,PSL,BBL)
9. winner - Winner of the match if any.
10. mom - man of the match.

**inning attributes**

1. inning - which inning of the match?
2. bat_team - which team is batting?
3. bow_team - which team is bowling?
4. is_las_ball - Is this the last ball of the inning?

**Per ball attributes**

1. overs - the over number.
2. balls - the ball number of a particular over.
3. batsman - batsman who is facing the ball
4. non-striker - batsman who is not on strike
5. bowler - bowler bowling the ball
6. bruns - how many runs the batsman scored on that ball?
7. eruns - how many extra runs were conceeded if any?
8. e_type - type of extras(wide,no ball etc) if any
9. iswicket - Did wicket fall on the ball - 0/1?
10. w_type - how the player was dismissed(bowled,caught etc)?
11. fielder - which fielder was involved in dismissal?
12. player_out - which player got out?

A sample row from the table.
![table row]({{site.baseurl}}/assets/images/table_row.png)

Once the data is stored in the table, I have created a form in django(python web framework) which takes input from the user and then queries this table.

## What queries one can make

At present, the aggregation is only based on runs, wickets, fours and sixes. My long term goal would be to include more features.

* Find out how many runs a batsman has scored against a particular bowler - Dhoni vs Steyn or Kohli vs Malinga?
* Find out how a bowler has  faired against a batsman - Kohli vs Anderson or Zaheer vs Smith?
* Find out batsman who have hit six on their first ball faced in the match - Sehwag?

These are some of the examples. I will show how to perform these queries in a video tutorial.

## What queries one cannot make

* Finding out the highest run scorers in test or odi
* Finding out how many centuries, fifties, fifers etc. a player has taken

In simple words, you won't be able to use the tool to analyse stats that is based on aggregating data. You can use [espncricinfo stats](http://stats.espncricinfo.com/ci/engine/stats/index.html) for these queries.

## Future

I would love everyone to use the tool and suggest new features that can be added to this or report any bugs. 

* Query support for t-20 leagues like IPL and BBL.
* Query support for women's matches.
* Include aggregated fours, sixes, extras etc. in the result
* Make the tool fast by optimizing the database and using cache.

Do suggest other ideas in the comment.




