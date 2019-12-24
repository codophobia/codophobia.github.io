---
Layout: post
classes: wide
section-type: post
title: "Pycricbuzz - Cricket API for Python"
category: python
description: "Python library to fetch cricket scores from cricbuzz site or cricket api for python"
tags: ['cricbuzz','python','cricket','API','cricket api']
---
{% include article_ad.html %}
Pycricbuzz is a python library which can be used to get live scores, commentary and full scorecard for recent and live matches.
In case you want to know how the library was developed, you can watch the below video. If you just want to use the library, then you can skip the video.

<iframe width="560" height="315" src="https://www.youtube.com/embed/OQqYbC1BKxw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Installing pycricbuzz

```bash
pip install pycricbuzz or pip3 install pycricbuzz
```
Or you can directly download the package from github:<a href="https://github.com/codophobia/pycricbuzz" target="_blank">pycricbuzz github</a>

### Importing pycricbuzz in your script

```python
from pycricbuzz import Cricbuzz
```

### Create a cricbuzz object

```python
c = Cricbuzz()
```

We can use this object to work with all the functions provided by pycricbuzz.
{% include article_ad.html %}
### Fetch all the matches provided by cricbuzz

```python
from pycricbuzz import Cricbuzz
import json
c = Cricbuzz()
matches = c.matches()
print (json.dumps(matches,indent=4)) #for pretty prinitng
```

Output(Printing details of one of matches from list of matches):

```json
[
    {
        "id": "21653",
        "mchstate": "toss",
        "mnum": "5th ODI",
        "official": {
            "referee": {
                "country": "Ind",
                "id": "3894",
                "name": "Javagal Srinath"
            },
            "umpire1": {
                "country": "Afg",
                "id": "11150",
                "name": "Bismillah Jan Shinwari"
            },
            "umpire2": {
                "country": "Ind",
                "id": "7498",
                "name": "S Ravi"
            },
            "umpire3": {
                "country": "Afg",
                "id": "11152",
                "name": "Ahmed Shah Pakteen"
            }
        },
        "srs": "Afghanistan v Ireland in India 2019",
        "start_time": "2019-03-10 13:00:00",
        "status": "IRE opt to bowl",
        "team1": {
            "name": "Afghanistan",
            "squad": [
                "Mohammad Shahzad",
                "Javed Ahmadi",
                "Rahmat",
                "Asghar Afghan",
                "Najibullah",
                "Shenwari",
                "Nabi",
                "Rashid Khan",
                "Mujeeb",
                "Zahir Khan",
                "Shapoor"
            ],
            "squad_bench": [
                "Hazratullah Zazai",
                "Noor Ali",
                "Ikram Ali Khil",
                "Naib",
                "Aftab Alam",
                "Dawlat Zadran",
                "Shahidi",
                "Shirzad",
                "Fareed Malik",
                "Karim Janat"
            ]
        },
        "team2": {
            "name": "Ireland",
            "squad": [
                "Porterfield",
                "Stirling",
                "Andy Balbirnie",
                "Simi Singh",
                "Kevin O Brien",
                "Dockrell",
                "Stuart Poynter",
                "Andy McBrine",
                "James Cameron",
                "Murtagh",
                "Rankin"
            ],
            "squad_bench": [
                "Chase",
                "James McCollum",
                "McCarthy",
                "Lorcan Tucker",
                "S Thompson"
            ]
        },
        "toss": "Ireland elect to bowl",
        "type": "ODI",
        "venue_location": "Dehradun, Uttarakhand, India",
        "venue_name": "Rajiv Gandhi International Cricket Stadium"
    }
]
```

We get a list of matches with each match having its details. The 'id' attribute of a match is important as it will be used to fetch commentary and scorecard for that match.
There is also another way to get the information for a match using match id.

```python
def match_info(mid):
    c = Cricbuzz()
    minfo = c.matchinfo(mid)
    print(json.dumps(minfo, indent=4, sort_keys=True))
```

The output will be same as above. It's another way of getting match information when you have match_id with you.
{% include article_ad.html %}
### Fetching the live score of a match

```python
def live_score(mid):
    c = Cricbuzz()
    lscore = c.livescore(mid)
    print(json.dumps(lscore, indent=4, sort_keys=True))
```

Output:

```json
{
    "batting": {
        "batsman": [
            {
                "balls": "43",
                "fours": "0",
                "name": "Asghar Afghan",
                "runs": "8",
                "six": "0"
            },
            {
                "balls": "32",
                "fours": "1",
                "name": "Nabi",
                "runs": "19",
                "six": "0"
            }
        ],
        "score": [
            {
                "declare": null,
                "inning_num": "1",
                "overs": "25.2",
                "runs": "77",
                "wickets": "4"
            }
        ],
        "team": "Afghanistan"
    },
    "bowling": {
        "bowler": [
            {
                "maidens": "0",
                "name": "James Cameron",
                "overs": "4.2",
                "runs": "15",
                "wickets": "0"
            }
        ],
        "score": [],
        "team": "Ireland"
    }
}
```

It gives us the information about the match and details of the batting and bowling team. The batsman and bowlers are the current two batsman batting and current two bowlers bowling.
{% include article_ad.html %}
### Fetch commentary of the match

```python
def commentary(mid):
    c = Cricbuzz()
    comm = c.commentary(mid)
    print(json.dumps(comm, indent=4, sort_keys=True))
```

Output:

```json
{
    "commentary": [
        {
            "comm": "Simi Singh to Nabi, no run",
            "over": "26.2"
        },
        {
            "comm": "Simi Singh to Nabi, no run, tossed up outside off, Nabi gets an inside edge on the clip to short fine",
            "over": "26.1"
        },
        {
            "comm": "Asghar Afghan is still struggling from the shoulder injury he sustained in the last match. Winced in pain after that last six, but going on for his team",
            "over": null
        },
        {
            "comm": "James Cameron to Asghar Afghan, no run, tossed up on off, Afghan leans well forward and blocks",
            "over": "25.6"
        },
        {
            "comm": "James Cameron to Asghar Afghan, no run, flighted on the leg-stump line, clipped straight to the fielder at backward leg, wanted a single and is sent abck",
            "over": "25.5"
        },
        {
            "comm": "James Cameron to Asghar Afghan, <b>SIX</b>, that has been <b>thumped</b>, juicy full-toss from Cameron-Dow, Afghan moves across and slugs it high and over backward square leg for a maximum",
            "over": "25.4"
        },
        {
            "comm": "James Cameron to Asghar Afghan, <b>FOUR</b>, that's a poor ball from Cameron-Dow, dropped short and wide of off, Afghan made room and slapped it wide of cover - beats the fielder getting across from the deep",
            "over": "25.3"
        },
        {
            "comm": "James Cameron to Asghar Afghan, no run, dropped short and wide of off, cracked straight to Dockrell at cover",
            "over": "25.2"
        },
        {
            "comm": "James Cameron to Asghar Afghan, no run, turn for Cameron-Dow, but once again the length is short, allows Afghan to push it to the off-side",
            "over": "25.1"
        },
        {
            "comm": "Simi Singh to Nabi, no run, full and at the stumps, whipped straight to the fielder at mid-wicket",
            "over": "24.6"
        }
    ]
}
```

"Commentary" is a list containing all the commentary texts.

### Fetch scorecard of a match

```python
def scorecard(mid):
    c = Cricbuzz()
    scard = c.scorecard(mid)
    print(json.dumps(scard, indent=4, sort_keys=True))
```

```json
{
    "scorecard": [
        {
            "batcard": [
                {
                    "balls": "4",
                    "dismissal": "c James Cameron b Murtagh",
                    "fours": "0",
                    "name": "Mohammad Shahzad",
                    "runs": "6",
                    "six": "1"
                },
                {
                    "balls": "30",
                    "dismissal": " b Andy McBrine",
                    "fours": "2",
                    "name": "Javed Ahmadi",
                    "runs": "24",
                    "six": "1"
                },
                {
                    "balls": "42",
                    "dismissal": "c Stirling b Dockrell",
                    "fours": "0",
                    "name": "Rahmat",
                    "runs": "17",
                    "six": "1"
                },
                {
                    "balls": "47",
                    "dismissal": "batting",
                    "fours": "1",
                    "name": "Asghar Afghan",
                    "runs": "18",
                    "six": "1"
                },
                {
                    "balls": "1",
                    "dismissal": "c Andy McBrine b Dockrell",
                    "fours": "0",
                    "name": "Shenwari",
                    "runs": "0",
                    "six": "0"
                },
                {
                    "balls": "38",
                    "dismissal": "batting",
                    "fours": "1",
                    "name": "Nabi",
                    "runs": "20",
                    "six": "0"
                }
            ],
            "batteam": "Afghanistan",
            "bowlcard": [
                {
                    "maidens": "1",
                    "name": "Murtagh",
                    "nballs": "0",
                    "overs": "5",
                    "runs": "18",
                    "wickets": "1",
                    "wides": "0"
                },
                {
                    "maidens": "0",
                    "name": "Rankin",
                    "nballs": "0",
                    "overs": "3",
                    "runs": "13",
                    "wickets": "0",
                    "wides": "1"
                },
                {
                    "maidens": "1",
                    "name": "Andy McBrine",
                    "nballs": "0",
                    "overs": "5",
                    "runs": "7",
                    "wickets": "1",
                    "wides": "1"
                },
                {
                    "maidens": "0",
                    "name": "Dockrell",
                    "nballs": "0",
                    "overs": "6",
                    "runs": "18",
                    "wickets": "2",
                    "wides": "0"
                },
                {
                    "maidens": "0",
                    "name": "James Cameron",
                    "nballs": "0",
                    "overs": "5",
                    "runs": "25",
                    "wickets": "0",
                    "wides": "1"
                },
                {
                    "maidens": "0",
                    "name": "Simi Singh",
                    "nballs": "0",
                    "overs": "3",
                    "runs": "7",
                    "wickets": "0",
                    "wides": "0"
                }
            ],
            "bowlteam": "Ireland",
            "extras": {
                "byes": "0",
                "lbyes": "0",
                "nballs": "0",
                "penalty": "0",
                "total": "3",
                "wides": "3"
            },
            "fall_wickets": [
                {
                    "name": "Mohammad Shahzad",
                    "overs": "0.4",
                    "score": "6",
                    "wkt_num": "1"
                },
                {
                    "name": "Javed Ahmadi",
                    "overs": "11.3",
                    "score": "39",
                    "wkt_num": "2"
                },
                {
                    "name": "Rahmat",
                    "overs": "14.3",
                    "score": "50",
                    "wkt_num": "3"
                },
                {
                    "name": "Shenwari",
                    "overs": "14.4",
                    "score": "50",
                    "wkt_num": "4"
                }
            ],
            "inng_num": "1",
            "overs": "27",
            "runs": "88",
            "wickets": "4"
        }
    ]
}
```
{% include article_ad.html %}
