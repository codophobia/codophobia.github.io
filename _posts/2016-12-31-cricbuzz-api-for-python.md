---
Layout: post
section-type: post
title: "Pycricbuzz - cricbuzz API for python"
category: python
description: "Python library to fetch cricket scores from cricbuzz site"
tags: ['cricbuzz','python','cricket','API']
---
<b>Update</b>: You can also check out <a href = "https://github.com/codophobia/pycricket" target="blank">pycricket</a> for cricket scorecard of past matches.

Cricbuzz provides the live scores through its XML feed. We can parse this XML file to gather a lot of information. One of the easiest way to parse a XML file in python is through BeautifulSoup library. But you don't need to worry about parsing it  since I have done it. You can directly access the live matches, commentary and scorecards by using pycricbuzz library in python. Pycricbuzz makes it easier to access cricbuzz scores.

#### Installing pycricbuzz

```bash
sudo pip install pycricbuzz
```

Or you can directly download the package from github:<a href="https://github.com/codophobia/pycricbuzz" target="_blank">pycricbuzz github</a>

#### Importing pycricbuzz in your script

```python
from pycricbuzz import Cricbuzz
```

#### Create a cricbuzz object

```
c = Cricbuzz()
```

We can use this object to work with all the functions provided by pycricbuzz.

#### Fetch all the matches provided by cricbuzz XML feed

```python
from pycricbuzz import Cricbuzz
import json
c = Cricbuzz()
matches = c.matches()
print json.dumps(parsed,indent=4) #for pretty prinitng
```

Output:
```python
[
    {
        "status": "Coming up on Dec 24 at 01:10 GMT",
        "mchstate": "nextlive",
        "mchdesc": "AKL vs WEL",
        "srs": "McDonalds Super Smash, 2016-17",
        "mnum": "18TH MATCH",
        "type": "ODI",
        "id": "0"
    },
    {
        "status": "Ind U19 won by 34 runs",
        "mchstate": "Result",
        "mchdesc": "INDU19 vs SLU19",
        "srs": "Under 19 Asia Cup, 2016",
        "mnum": "Final",
        "type": "ODI",
        "id": "17727"
    },
    {
        "status": "PRS won by 48 runs",
        "mchstate": "Result",
        "mchdesc": "PRS vs ADS",
        "srs": "Big Bash League, 2016-17",
        "mnum": "5th Match",
        "type": "T20",
        "id": "16729"
    }
]
```

We get a list of matches with each match having its details. The 'id' attribute of a match is important as it will be used to fetch commentary and scorecard for that match.
Note: Commentary, scorecard and live score won't be available for "nextlive" matches.

#### Fetching the live score for a match

```python
c = Cricbuzz()
matches = c.matches()
for match in matches:
	print json.dumps(c.livescore(match['id']),indent=4)
```
Output:(I am just printing the output for a match from the list)
```python
{
    "matchinfo": {
        "status": "PRS won by 48 runs",
        "mchstate": "Result",
        "mchdesc": "PRS vs ADS",
        "srs": "Big Bash League, 2016-17",
        "mnum": "5th Match",
        "type": "T20",
        "id": "16729"
    },
    "batting": {
        "batsman": [
            {
                "six": "0",
                "runs": "3",
                "balls": "3",
                "fours": "0",
                "name": "B Stanlake*"
            },
            {
                "six": "0",
                "runs": "1",
                "balls": "5",
                "fours": "0",
                "name": "Liam O'Connor"
            }
        ],
        "score": [
            {
                "runs": "149",
                "wickets": "9",
                "overs": "20",
                "desc": "Inns"
            }
        ],
        "team": "ADS"
    },
    "bowling": {
        "bowler": [
            {
                "maidens": "0",
                "runs": "15",
                "overs": "4",
                "name": "D Willey*",
                "wickets": "2"
            },
            {
                "maidens": "0",
                "runs": "32",
                "overs": "3",
                "name": "Jhye Richardson",
                "wickets": "2"
            }
        ],
        "score": [
            {
                "runs": "197",
                "wickets": "7",
                "overs": "20",
                "desc": "Inns"
            }
        ],
        "team": "PRS"
    }
}
```

It gives us the information about the match and details of the batting and bowling team. The batsman and bowlers are the current two batsman batting and current two bowlers bowling.
<b>In case match has not started, only matchinfo will be displayed. This also applies for commentary and scoreboard.

#### Fetch commentary of the match

```python
c = Cricbuzz()
matches = c.matches()
for match in matches:
	print json.dumps(c.commentary(match['id']),indent=4)
	break
```
Output:
```python
{
    "commentary": [
        "Scorecard Updates only",
        "Teams:",
        "Wellington (Playing XI): Hamish Marshall(c), Michael Papps, Tom Blundell(w), Stephen Murdoch, Michael Pollard, Grant Elliott, Matt Taylor, Luke Woodcock, Jeetan Patel, Anurag Verma, Brent Arnel",
        "Auckland (Playing XI): Michael Guptill-Bunce, Glenn Phillips(w), Rob Nicol(c), Jeet Raval, Mark Chapman, SM Solia, Michael Barry, Ben Horne, Donovan Grobbelaar, Tarun Nethula, Tymal Mills",
        "Wellington have won the toss and have opted to bat",
        "Teams:",
        "Wellington (From): Hamish Marshall(c), Matt Taylor, Anurag Verma, Brent Arnel, Hamish Bennett, Tom Blundell(w), Luke Ronchi, Fraser Colson, Jade Dernbach, Evan Gulbis, Matt McEwan, Ian McPeake, Stephen Murdoch, Ollie Newton, Michael Papps, Jeetan Patel, Michael Pollard, Luke Woodcock, Grant Elliott",
        "Auckland (From): Cody Andrews, Michael Barry, Rob Nicol(c), Bradley Cachopa, Mark Chapman, Colin de Grandhomme, Glenn Phillips(w), Lockie Ferguson, Donovan Grobbelaar, Michael Guptill-Bunce, Shawn Hicks, Ben Horne, Tymal Mills, Colin Munro, Tarun Nethula, Robert O'Donnell, Jeet Raval, SM Solia, Mitchell McClenaghan"
    ],
    "matchinfo": {
        "status": "WEL won by 2 runs",
        "mchstate": "Result",
        "mchdesc": "AKL vs WEL",
        "srs": "McDonalds Super Smash, 2016-17",
        "mnum": "18th Match",
        "type": "T20",
        "id": "17326"
    }
}
```

"Commentary" is a list containing all the commentary texts.

#### Fetch scorecard of a match

```python
from cricbuzz import Cricbuzz
import json
c = Cricbuzz()
matches = c.matches()
for match in matches:
	print json.dumps(c.scorecard(match['id']),indent=4)
	break
```

I am not giving the output here as it is too long. You can analyse it yourself.
