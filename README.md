# sportradar-advanced-challenge
 
 > use NHL data feeds and build a data pipeline

 The NHL hosts various public data feeds. This coding challenge is to set up a pipeline to ingest data in real time. You'll need to monitor the schedule feed and when a game starts, or goes live, a corresponding job will activate and ingest the live data into a database. The separation of concerns, number of functions, and general approach is entirely up to you. The following functionality is required.

* First process should continually watch for game status changes and toggle the next process on game status changes.
* The second process should only run when games are live. It should close when games are over. This process will ingest game data from the NHL:
  * Player ID
  * Player Name
  * Team ID
  * Team Name
  * Player Age
  * Player Number
  * Player Position
  * Assists
  * Goals
  * Hits
  * Points
  * Penalty Minutes
* We should be able to query the database for up-to-date stats during, and after, a live game.

This exercise is in-lieu of a traditional whiteboard/algorithm style type interview. Not only will this exercise serve as our initial evaluation of your development skills, it will also be a center-piece to further conversations we will have with you. We respect your time and the fact that you have a life and possibly a day-job, so put in as much time as you feel will be a fair representation of your skills.  However, this exercise is purposefully open ended and can be an opportunity for you to show off. Hopefully this is something you can have fun with.

While the approach you take to meeting the above objectives is up to you, here are a couple of things we will expect:

* Use NodeJs
* Use relational database
* There should be a basic README included.
* There should be tests.
* We should be able to run the application (provide instructions if need be).

Please do not create a pull-request against this repository; you should create your own project/repository.  Also--while not required--it would be nice to have access to your commit history (i.e. don't squash) through github. However if you are not comfortable with this for any reason, submitting a zip file with the contents of the project is acceptable as well.

Below you will find complete documentation to a public NHL API. You will not need the majority of these endpoints, they are provided for completeness. 

[Teams](#teams)

[Divisions](#divisions)

[Conferences](#conferences)

[People](#people)

[Game-IDs](#game-ids)

[Game Types](#game-types)

[Tournaments](#tournaments)

[Schedule](#schedule)

[Seasons](#seasons)

[Standings](#standings)

[Standings Types](#standings-types)

[Stats Types](#stats-types)

[Team Stats](#team-stats)

[Draft](#draft)

[Prospects](#prospects)

[Awards](#awards)

[Venues](#venues)


---

### <a name="teams"></a>Teams

`GET https://statsapi.web.nhl.com/api/v1/teams` Returns a list of data about
all teams including their id, venue details, division, conference and franchise information.

`GET https://statsapi.web.nhl.com/api/v1/teams/ID` Returns the same information as above just
for a single team instead of the entire league.
#### Modifiers
`?expand=team.roster` Shows roster of active players for the specified team 

`?expand=person.names` Same as above, but gives less info.

`?expand=team.schedule.next` Returns details of the upcoming game for a team

`?expand=team.schedule.previous` Same as above but for the last game played

`?expand=team.stats` Returns the teams stats for the season

`?expand=team.roster&season=20142015` Adding the season identifier shows the roster for that season

`?teamId=4,5,29` Can string team id together to get multiple teams

`?stats=statsSingleSeasonPlayoffs` Specify which stats to get. Not fully sure all of the values

```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "teams" : [ {
    "id" : 1,
    "name" : "New Jersey Devils",
    "link" : "/api/v1/teams/1",
    "venue" : {
      "name" : "Prudential Center",
      "link" : "/api/v1/venues/null",
      "city" : "Newark",
      "timeZone" : {
        "id" : "America/New_York",
        "offset" : -5,
        "tz" : "EST"
      }
    },
    "abbreviation" : "NJD",
    "teamName" : "Devils",
    "locationName" : "New Jersey",
    "firstYearOfPlay" : "1982",
    "division" : {
      "id" : 18,
      "name" : "Metropolitan",
      "link" : "/api/v1/divisions/18"
    },
    "conference" : {
      "id" : 6,
      "name" : "Eastern",
      "link" : "/api/v1/conferences/6"
    },
    "franchise" : {
      "franchiseId" : 23,
      "teamName" : "Devils",
      "link" : "/api/v1/franchises/23"
    },
    "shortName" : "New Jersey",
    "officialSiteUrl" : "http://www.truesince82.com",
    "franchiseId" : 23,
    "active" : true
  }, {
```

`GET https://statsapi.web.nhl.com/api/v1/teams/ID/roster` Returns entire roster for a team
including id value, name, jersey number and position details.

```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "roster" : [ {
    "person" : {
      "id" : 8477474,
      "fullName" : "Madison Bowey",
      "link" : "/api/v1/people/8477474"
    },
    "jerseyNumber" : "22",
    "position" : {
      "code" : "D",
      "name" : "Defenseman",
      "type" : "Defenseman",
      "abbreviation" : "D"
    }
  },
```
---
### <a name="divisions"></a>Divisions
`GET https://statsapi.web.nhl.com/api/v1/divisions`  Returns full list of divisions
and associated data like which conference they belong to, id values and API links.
Does not show inactive divisions

`GET https://statsapi.web.nhl.com/api/v1/divisions/ID` Same as above but only for a
single division. This can show old inactive divisions such as 13 Patrick.
```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "divisions" : [ {
    "id" : 17,
    "name" : "Atlantic",
    "link" : "/api/v1/divisions/17",
    "abbreviation" : "A",
    "conference" : {
      "id" : 6,
      "name" : "Eastern",
      "link" : "/api/v1/conferences/6"
    },
    "active" : true
  },
```
---
### <a name="conferences"></a>Conferences
`GET https://statsapi.web.nhl.com/api/v1/conferences` Returns conference details
for all current NHL conferences.

`GET https://statsapi.web.nhl.com/api/v1/conferences/ID` Same as above but for
specific conference, also can look up id 7 for World Cup of Hockey.
```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "conferences" : [ {
    "id" : 6,
    "name" : "Eastern",
    "link" : "/api/v1/conferences/6",
    "abbreviation" : "E",
    "shortName" : "East",
    "active" : true
  }, {
    "id" : 5,
    "name" : "Western",
    "link" : "/api/v1/conferences/5",
    "abbreviation" : "W",
    "shortName" : "West",
    "active" : true
  } ]
}
```
---
### <a name="people"></a>People
`GET https://statsapi.web.nhl.com/api/v1/people/ID` Gets details for a player, must
specify the id value in order to return data.
```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2020. All Rights Reserved.",
  "people" : [ {
    "id" : 8476792,
    "fullName" : "Torey Krug",
    "link" : "/api/v1/people/8476792",
    "firstName" : "Torey",
    "lastName" : "Krug",
    "primaryNumber" : "47",
    "birthDate" : "1991-04-12",
    "currentAge" : 28,
    "birthCity" : "Livonia",
    "birthStateProvince" : "MI",
    "birthCountry" : "USA",
    "nationality" : "USA",
    "height" : "5' 9\"",
    "weight" : 186,
    "active" : true,
    "alternateCaptain" : false,
    "captain" : false,
    "rookie" : false,
    "shootsCatches" : "L",
    "rosterStatus" : "Y",
    "currentTeam" : {
      "id" : 6,
      "name" : "Boston Bruins",
      "link" : "/api/v1/teams/6"
    },
    "primaryPosition" : {
      "code" : "D",
      "name" : "Defenseman",
      "type" : "Defenseman",
      "abbreviation" : "D"
    }
  } ]
}

```
`GET https://statsapi.web.nhl.com/api/v1/people/ID/stats` Complex endpoint with
lots of append options to change what kind of stats you wish to obtain

`GET https://statsapi.web.nhl.com/api/v1/positions` Simple endpoint that 
obtains an array of eligible positions in the NHL

#### Modifiers
`?stats=statsSingleSeason&season=20182019`  Obtains single season statistics
for a player

```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2020. All Rights Reserved.",
  "stats" : [ {
    "type" : {
      "displayName" : "statsSingleSeason"
    },
    "splits" : [ {
      "season" : "20182019",
      "stat" : {
        "timeOnIce" : "1237:45",
        "assists" : 43,
        "goals" : 38,
        "pim" : 32,
        "shots" : 235,
        "games" : 66,
        "hits" : 57,
        "powerPlayGoals" : 17,
        "powerPlayPoints" : 33,
        "powerPlayTimeOnIce" : "222:18",
        "evenTimeOnIce" : "1012:48",
        "penaltyMinutes" : "32",
        "faceOffPct" : 33.33,
        "shotPct" : 16.2,
        "gameWinningGoals" : 4,
        "overTimeGoals" : 0,
        "shortHandedGoals" : 0,
        "shortHandedPoints" : 0,
        "shortHandedTimeOnIce" : "02:39",
        "blocked" : 25,
        "plusMinus" : 6,
        "points" : 81,
        "shifts" : 1395,
        "timeOnIcePerGame" : "18:45",
        "evenTimeOnIcePerGame" : "15:20",
        "shortHandedTimeOnIcePerGame" : "00:02",
        "powerPlayTimeOnIcePerGame" : "03:22"
      }
    } ]
  } ]
}

```
`?stats=yearByYear` Provides a list of every season for a player's career
```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2020. All Rights Reserved.",
  "stats" : [ {
    "type" : {
      "displayName" : "yearByYear"
    },
    "splits" : [ {
      "season" : "20092010",
      "stat" : {
        "assists" : 24,
        "goals" : 28,
        "pim" : 8,
        "games" : 30,
        "penaltyMinutes" : "8",
        "plusMinus" : 32,
        "points" : 52
      },
      "team" : {
        "name" : "HC Havirov U16",
        "link" : "/api/v1/teams/null"
      },
      "league" : {
        "name" : "Czech U16",
        "link" : "/api/v1/league/null"
      },
      "sequenceNumber" : 83727
    }, {
      "season" : "20102011",
      "stat" : {
        "assists" : 12,
        "goals" : 7,
        "pim" : 4,
        "games" : 16,
        "penaltyMinutes" : "4",
        "points" : 19
      },
      "team" : {
        "name" : "Havirov U18",
        "link" : "/api/v1/teams/null"
      },
      "league" : {
        "name" : "CzR-U18",
        "link" : "/api/v1/league/null"
      },
      "sequenceNumber" : 1
    }, {
      "season" : "20112012",
      "stat" : {
        "assists" : 22,
        "goals" : 18,
        "pim" : 28,
        "games" : 17,
        "penaltyMinutes" : "28",
        "points" : 40
      },
      "team" : {
        "name" : "Havirov U18",
        "link" : "/api/v1/teams/null"
      },
      "league" : {
        "name" : "CzR-U18",
        "link" : "/api/v1/league/null"
      },
      "sequenceNumber" : 1
    },  {
      "season" : "20152016",
      "stat" : {
        "timeOnIce" : "711:12",
        "assists" : 11,
        "goals" : 15,
        "pim" : 20,
        "shots" : 108,
        "games" : 51,
        "hits" : 52,
        "powerPlayGoals" : 0,
        "powerPlayPoints" : 1,
        "powerPlayTimeOnIce" : "23:52",
        "evenTimeOnIce" : "686:55",
        "penaltyMinutes" : "20",
        "faceOffPct" : 0.0,
        "shotPct" : 13.9,
        "gameWinningGoals" : 2,
        "overTimeGoals" : 0,
        "shortHandedGoals" : 0,
        "shortHandedPoints" : 0,
        "shortHandedTimeOnIce" : "00:25",
        "blocked" : 15,
        "plusMinus" : 3,
        "points" : 26,
        "shifts" : 947
      },
      "team" : {
        "id" : 6,
        "name" : "Boston Bruins",
        "link" : "/api/v1/teams/6"
      },
      "league" : {
        "id" : 133,
        "name" : "National Hockey League",
        "link" : "/api/v1/league/133"
      },
      "sequenceNumber" : 1
    }, {
      "season" : "20152016",
      "stat" : {
        "assists" : 3,
        "goals" : 1,
        "pim" : 2,
        "shots" : 3,
        "games" : 3,
        "powerPlayGoals" : 0,
        "penaltyMinutes" : "2",
        "shotPct" : 33.3,
        "gameWinningGoals" : 0,
        "shortHandedGoals" : 0,
        "plusMinus" : 1,
        "points" : 4
      },
      "team" : {
        "id" : 4982,
        "name" : "Providence",
        "link" : "/api/v1/teams/4982"
      },
      "league" : {
        "name" : "AHL",
        "link" : "/api/v1/league/null"
      },
      "sequenceNumber" : 2
    }, {
      "season" : "20152016",
      "stat" : {
        "assists" : 3,
        "goals" : 1,
        "pim" : 0,
        "shots" : 19,
        "games" : 4,
        "powerPlayGoals" : 1,
        "penaltyMinutes" : "0",
        "gameWinningGoals" : 1,
        "shortHandedGoals" : 0,
        "plusMinus" : 1,
        "points" : 4
      },
      "team" : {
        "id" : 61,
        "name" : "Czech Republic",
        "link" : "/api/v1/teams/61"
      },
      "league" : {
        "id" : 147,
        "name" : "WJC-A",
        "link" : "/api/v1/league/147"
      },
      "sequenceNumber" : 3
    }, {
      "season" : "20152016",
      "stat" : {
        "assists" : 5,
        "goals" : 1,
        "pim" : 4,
        "shots" : 28,
        "games" : 8,
        "powerPlayGoals" : 0,
        "penaltyMinutes" : "4",
        "gameWinningGoals" : 0,
        "shortHandedGoals" : 0,
        "plusMinus" : 4,
        "points" : 6
      },
      "team" : {
        "id" : 61,
        "name" : "Czech Republic",
        "link" : "/api/v1/teams/61"
      },
      "league" : {
        "id" : 147,
        "name" : "WC-A",
        "link" : "/api/v1/league/147"
      },
      "sequenceNumber" : 4
    }, {
      "season" : "20162017",
      "stat" : {
        "timeOnIce" : "1348:41",
        "assists" : 36,
        "goals" : 34,
        "pim" : 34,
        "shots" : 262,
        "games" : 75,
        "hits" : 72,
        "powerPlayGoals" : 10,
        "powerPlayPoints" : 24,
        "powerPlayTimeOnIce" : "197:05",
        "evenTimeOnIce" : "1150:25",
        "penaltyMinutes" : "34",
        "faceOffPct" : 66.67,
        "shotPct" : 13.0,
        "gameWinningGoals" : 6,
        "overTimeGoals" : 2,
        "shortHandedGoals" : 0,
        "shortHandedPoints" : 0,
        "shortHandedTimeOnIce" : "01:11",
        "blocked" : 32,
        "plusMinus" : 11,
        "points" : 70,
        "shifts" : 1597
      },
      "team" : {
        "id" : 6,
        "name" : "Boston Bruins",
        "link" : "/api/v1/teams/6"
      },
      "league" : {
        "id" : 133,
        "name" : "National Hockey League",
        "link" : "/api/v1/league/133"
      },
      "sequenceNumber" : 1
    }, {
      "season" : "20162017",
      "stat" : {
        "assists" : 6,
        "goals" : 1,
        "pim" : 4,
        "games" : 8,
        "penaltyMinutes" : "4",
        "plusMinus" : 1,
        "points" : 7
      },
      "team" : {
        "name" : "Czech Republic",
        "link" : "/api/v1/teams/null"
      },
      "league" : {
        "name" : "WC",
        "link" : "/api/v1/league/null"
      },
      "sequenceNumber" : 16081
    }, {
      "season" : "20162017",
      "stat" : {
        "assists" : 0,
        "goals" : 0,
        "pim" : 0,
        "games" : 3,
        "penaltyMinutes" : "0",
        "plusMinus" : -1,
        "points" : 0
      },
      "team" : {
        "name" : "Czech Republic",
        "link" : "/api/v1/teams/null"
      },
      "league" : {
        "name" : "WCup",
        "link" : "/api/v1/league/null"
      },
      "sequenceNumber" : 16105
    }, {
      "season" : "20172018",
      "stat" : {
        "timeOnIce" : "1473:12",
        "assists" : 45,
        "goals" : 35,
        "pim" : 37,
        "shots" : 246,
        "games" : 82,
        "hits" : 55,
        "powerPlayGoals" : 13,
        "powerPlayPoints" : 26,
        "powerPlayTimeOnIce" : "253:04",
        "evenTimeOnIce" : "1219:04",
        "penaltyMinutes" : "37",
        "faceOffPct" : 20.0,
        "shotPct" : 14.2,
        "gameWinningGoals" : 5,
        "overTimeGoals" : 0,
        "shortHandedGoals" : 0,
        "shortHandedPoints" : 0,
        "shortHandedTimeOnIce" : "01:04",
        "blocked" : 31,
        "plusMinus" : 10,
        "points" : 80,
        "shifts" : 1739
      },
      "team" : {
        "id" : 6,
        "name" : "Boston Bruins",
        "link" : "/api/v1/teams/6"
      },
      "league" : {
        "id" : 133,
        "name" : "National Hockey League",
        "link" : "/api/v1/league/133"
      },
      "sequenceNumber" : 1
    }, {
      "season" : "20172018",
      "stat" : {
        "assists" : 2,
        "goals" : 4,
        "pim" : 0,
        "games" : 5,
        "penaltyMinutes" : "0",
        "plusMinus" : 6,
        "points" : 6
      },
      "team" : {
        "name" : "Czech Republic",
        "link" : "/api/v1/teams/null"
      },
      "league" : {
        "name" : "WC",
        "link" : "/api/v1/league/null"
      },
      "sequenceNumber" : 16081
    }, {
      "season" : "20182019",
      "stat" : {
        "timeOnIce" : "1237:45",
        "assists" : 43,
        "goals" : 38,
        "pim" : 32,
        "shots" : 235,
        "games" : 66,
        "hits" : 57,
        "powerPlayGoals" : 17,
        "powerPlayPoints" : 33,
        "powerPlayTimeOnIce" : "222:18",
        "evenTimeOnIce" : "1012:48",
        "penaltyMinutes" : "32",
        "faceOffPct" : 33.33,
        "shotPct" : 16.2,
        "gameWinningGoals" : 4,
        "overTimeGoals" : 0,
        "shortHandedGoals" : 0,
        "shortHandedPoints" : 0,
        "shortHandedTimeOnIce" : "02:39",
        "blocked" : 25,
        "plusMinus" : 6,
        "points" : 81,
        "shifts" : 1395
      },
      "team" : {
        "id" : 6,
        "name" : "Boston Bruins",
        "link" : "/api/v1/teams/6"
      },
      "league" : {
        "id" : 133,
        "name" : "National Hockey League",
        "link" : "/api/v1/league/133"
      },
      "sequenceNumber" : 1
    }, {
      "season" : "20192020",
      "stat" : {
        "timeOnIce" : "1018:02",
        "assists" : 35,
        "goals" : 37,
        "pim" : 30,
        "shots" : 205,
        "games" : 52,
        "hits" : 43,
        "powerPlayGoals" : 16,
        "powerPlayPoints" : 29,
        "powerPlayTimeOnIce" : "196:30",
        "evenTimeOnIce" : "821:00",
        "penaltyMinutes" : "30",
        "faceOffPct" : 30.77,
        "shotPct" : 18.0,
        "gameWinningGoals" : 6,
        "overTimeGoals" : 0,
        "shortHandedGoals" : 0,
        "shortHandedPoints" : 0,
        "shortHandedTimeOnIce" : "00:32",
        "blocked" : 18,
        "plusMinus" : 15,
        "points" : 72,
        "shifts" : 1102
      },
      "team" : {
        "id" : 6,
        "name" : "Boston Bruins",
        "link" : "/api/v1/teams/6"
      },
      "league" : {
        "id" : 133,
        "name" : "National Hockey League",
        "link" : "/api/v1/league/133"
      },
      "sequenceNumber" : 1
    } ]
  } ]
}
```

`?stats=homeAndAway&season=20182019` Provides a split between home and away games.
```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "stats" : [ {
    "type" : {
      "displayName" : "homeAndAway"
    },
    "splits" : [ {
      "season" : "20162017",
      "stat" : {
        "timeOnIce" : "751:09",
        "assists" : 18,
        "goals" : 20,
        "pim" : 26,
        "shots" : 163,
        "games" : 41,
        "hits" : 97,
        "powerPlayGoals" : 10,
        "powerPlayPoints" : 15,
        "powerPlayTimeOnIce" : "160:21",
        "evenTimeOnIce" : "590:34",
        "penaltyMinutes" : "26",
        "shotPct" : 0.0,
        "gameWinningGoals" : 6,
        "overTimeGoals" : 2,
        "shortHandedGoals" : 0,
        "shortHandedPoints" : 0,
        "shortHandedTimeOnIce" : "00:14",
        "blocked" : 12,
        "plusMinus" : 13,
        "points" : 38,
        "shifts" : 848,
        "timeOnIcePerGame" : "18:19",
        "evenTimeOnIcePerGame" : "14:24",
        "shortHandedTimeOnIcePerGame" : "00:00",
        "powerPlayTimeOnIcePerGame" : "03:54"
      },
      "isHome" : true
    }, {
      "season" : "20162017",
      "stat" : {
        "timeOnIce" : "754:52",
        "assists" : 18,
        "goals" : 13,
        "pim" : 24,
        "shots" : 150,
        "games" : 41,
        "hits" : 119,
        "powerPlayGoals" : 7,
        "powerPlayPoints" : 11,
        "powerPlayTimeOnIce" : "145:00",
        "evenTimeOnIce" : "607:52",
        "penaltyMinutes" : "24",
        "shotPct" : 0.0,
        "gameWinningGoals" : 1,
        "overTimeGoals" : 0,
        "shortHandedGoals" : 0,
        "shortHandedPoints" : 0,
        "shortHandedTimeOnIce" : "02:00",
        "blocked" : 17,
        "plusMinus" : -7,
        "points" : 31,
        "shifts" : 889,
        "timeOnIcePerGame" : "18:24",
        "evenTimeOnIcePerGame" : "14:49",
        "shortHandedTimeOnIcePerGame" : "00:02",
        "powerPlayTimeOnIcePerGame" : "03:32"
      },
      "isHome" : false
    } ]
  } ]
}
```

`?stats=winLoss&season=20162017` Very similar to the previous modifier except it provides the W/L/OT split instead of Home and Away

`?stats=byMonth&season=20162017` Monthly split of stats

`?stats=byDayOfWeek&season=20162017` Split done by day of the week

`?stats=vsDivision&season=20162017` Division stats split

`?stats=vsConference&season=20162017` Conference stats split

`?stats=vsTeam&season=20162017` Conference stats split

`?stats=gameLog&season=20162017` Provides a game log showing stats for each game of a season

`?stats=regularSeasonStatRankings&season=20162017` Returns where someone stands vs
the rest of the league for a specific regularSeasonStatRankings
```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "stats" : [ {
    "type" : {
      "displayName" : "regularSeasonStatRankings"
    },
    "splits" : [ {
      "season" : "20162017",
      "stat" : {
        "rankPowerPlayGoals" : "1st",
        "rankBlockedShots" : "405th",
        "rankAssists" : "51st",
        "rankShotPct" : "246th",
        "rankGoals" : "13th",
        "rankHits" : "19th",
        "rankPenaltyMinutes" : "111th",
        "rankShortHandedGoals" : "133rd",
        "rankPlusMinus" : "176th",
        "rankShots" : "2nd",
        "rankPoints" : "20th",
        "rankOvertimeGoals" : "9th",
        "rankGamesPlayed" : "1st"
      }
    } ]
  } ]
}
```

`?stats=goalsByGameSituation&season=20162017` Shows number on when goals for a
player happened like how many in the shootout, how many in each period, etc.

```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "stats" : [ {
    "type" : {
      "displayName" : "goalsByGameSituation"
    },
    "splits" : [ {
      "season" : "20162017",
      "stat" : {
        "goalsInFirstPeriod" : 8,
        "goalsInSecondPeriod" : 12,
        "goalsInThirdPeriod" : 11,
        "goalsInOvertime" : 2,
        "gameWinningGoals" : 7,
        "emptyNetGoals" : 0,
        "shootOutGoals" : 0,
        "shootOutShots" : 3,
        "goalsTrailingByOne" : 3,
        "goalsTrailingByTwo" : 3,
        "goalsTrailingByThreePlus" : 1,
        "goalsWhenTied" : 14,
        "goalsLeadingByOne" : 7,
        "goalsLeadingByTwo" : 4,
        "goalsLeadingByThreePlus" : 1,
        "penaltyGoals" : 0,
        "penaltyShots" : 0
      }
    } ]
  } ]
}
```

`?stats=onPaceRegularSeason&season=20182019` This only works with the current
in-progress season and shows **projected** totals based on current onPaceRegularSeason
```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "stats" : [ {
    "type" : {
      "displayName" : "onPaceRegularSeason"
    },
    "splits" : [ {
      "season" : "20172018",
      "stat" : {
        "timeOnIce" : "1598:04",
        "assists" : 34,
        "goals" : 52,
        "pim" : 32,
        "shots" : 362,
        "games" : 82,
        "hits" : 154,
        "powerPlayGoals" : 14,
        "powerPlayPoints" : 28,
        "powerPlayTimeOnIce" : "338:16",
        "evenTimeOnIce" : "1258:48",
        "penaltyMinutes" : "32",
        "faceOffPct" : 40.0,
        "shotPct" : 14.4,
        "gameWinningGoals" : 10,
        "overTimeGoals" : 6,
        "shortHandedGoals" : 0,
        "shortHandedPoints" : 0,
        "shortHandedTimeOnIce" : "01:00",
        "blocked" : 20,
        "plusMinus" : 24,
        "points" : 86,
        "shifts" : 1676,
        "timeOnIcePerGame" : "09:44",
        "evenTimeOnIcePerGame" : "07:40",
        "powerPlayTimeOnIcePerGame" : "02:03"
      }
    } ]
  } ]
}
```

---
### <a name="game"></a>Game
`GET https://statsapi.web.nhl.com/api/v1/game/ID/feed/live` Returns all data about
a specified game id including play data with on-ice coordinates and post-game
details like first, second and third stars and any details about shootouts.  The
data returned is simply too large at often over 30k lines and is best explored
with a JSON viewer.

`GET https://statsapi.web.nhl.com/api/v1/game/ID/boxscore` Returns far less detail
than `feed/live` and is much more suitable for post-game details including goals,
shots, PIMs, blocked, takeaways, giveaways and hits.

`GET https://statsapi.web.nhl.com/api/v1/game/ID/linescore` Even fewer details than
boxscore. Has goals, shots on goal, powerplay and goalie pulled status, number of 
skaters and shootout information if applicable

`GET http://statsapi.web.nhl.com/api/v1/game/ID/content` Complex endpoint returning
multiple types of media relating to the game including videos of shots, goals and saves.

`GET https://statsapi.web.nhl.com/api/v1/game/ID/feed/live/diffPatch?startTimecode=yyyymmdd_hhmmss`
Returns updates (like new play events, updated stats for boxscore, etc.) for the specified game ID
since the given startTimecode. If the startTimecode param is missing, returns an empty array.

#### <a name="game-ids">Game IDs
The first 4 digits identify the season of the game (ie. 2017 for the 2017-2018 season). The next 2 digits give the type of game, where 01 = preseason, 02 = regular season, 03 = playoffs, 04 = all-star. The final 4 digits identify the specific game number. For regular season and preseason games, this ranges from 0001 to the number of games played. (1271 for seasons with 31 teams (2017 and onwards) and 1230 for seasons with 30 teams). For playoff games, the 2nd digit of the specific number gives the round of the playoffs, the 3rd digit specifies the matchup, and the 4th digit specifies the game (out of 7).


#### <a name="game-types">Game Types

`GET https://statsapi.web.nhl.com/api/v1/gameTypes`

Returns list of game types with description and post-season status

#### <a name="game-status">Game Status

`GET https://statsapi.web.nhl.com/api/v1/gameStatus`

Returns a list of game status values

#### <a name="play-types">Play Types

`GET https://statsapi.web.nhl.com/api/v1/playTypes`

This shows all the possible play types found within the liveData/plays portion of the game feed

---
### <a name="tournaments">Tournaments

`GET https://statsapi.web.nhl.com/api/v1/tournamentTypes`

Gets the possible different tournament types.

`GET https://statsapi.web.nhl.com/api/v1/tournaments/playoffs`

This is used for tracking nested tournaments, specifically the Playoffs due to the nature of their structure.

```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2019. All Rights Reserved.",
  "id" : 1,
  "name" : "Playoffs",
  "season" : "20182019",
  "defaultRound" : 1,
  "rounds" : [ {
    "number" : 1,
    "code" : 1,
    "names" : {
      "name" : "First Round",
      "shortName" : "R1"
    },
    "format" : {
      "name" : "BO7",
      "description" : "Best of 7",
      "numberOfGames" : 7,
      "numberOfWins" : 4
    }
  }, {
    "number" : 2,
    "code" : 2,
    "names" : {
      "name" : "Second Round",
      "shortName" : "R2"
    },
    "format" : {
      "name" : "BO7",
      "description" : "Best of 7",
      "numberOfGames" : 7,
      "numberOfWins" : 4
    }
  }, {
    "number" : 3,
    "code" : 3,
    "names" : {
      "name" : "Conference Finals",
      "shortName" : "CF"
    },
    "format" : {
      "name" : "BO7",
      "description" : "Best of 7",
      "numberOfGames" : 7,
      "numberOfWins" : 4
    }
  }, {
    "number" : 4,
    "code" : 4,
    "names" : {
      "name" : "Stanley Cup Final",
      "shortName" : "SCF"
    },
    "format" : {
      "name" : "BO7",
      "description" : "Best of 7",
      "numberOfGames" : 7,
      "numberOfWins" : 4
    }
  } ]
}
```

In order to get additional information the expand modifier can be used such as this example

`?expand=round.series,schedule.game.seriesSummary&season=20182019` This will add in details  like the game summary and the season


---
### <a name="schedule">Schedule

`GET https://statsapi.web.nhl.com/api/v1/schedule` Returns a list of data about the schedule for a specified date range. If no date range is specified, returns results from the current day.

#### Notes

Without any flags or modifiers this endpoint will NOT return pre-season games that occur on the current day. In order for pre-season games to show up the date must be specified as show below in the Modifiers section

#### Modifiers
`?expand=schedule.broadcasts` Shows the broadcasts of the game

`?expand=schedule.linescore` Linescore for completed games

`?expand=schedule.ticket` Provides the different places to buy tickets for the upcoming games

`?teamId=30,17` Limit results to a specific team(s). Team ids can be found through the teams endpoint

`?date=2018-01-09` Single defined date for the search

`?startDate=2018-01-09` Start date for the search

`?endDate=2018-01-12` End date for the search

`?season=20172018` Returns all games from specified season

`?gameType=R` Restricts results to only regular season games. Can be set to any value from [Game Types](#game-types) endpoint

`GET https://statsapi.web.nhl.com/api/v1/schedule?teamId=30` Returns Minnesota Wild games for the current day.

```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "totalItems" : 1,
  "totalEvents" : 0,
  "totalGames" : 1,
  "totalMatches" : 0,
  "wait" : 10,
  "dates" : [ {
    "date" : "2018-01-09",
    "totalItems" : 1,
    "totalEvents" : 0,
    "totalGames" : 1,
    "totalMatches" : 0,
    "games" : [ {
      "gamePk" : 2017020659,
      "link" : "/api/v1/game/2017020659/feed/live",
      "gameType" : "R",
      "season" : "20172018",
      "gameDate" : "2018-01-10T01:00:00Z",
      "status" : {
        "abstractGameState" : "Preview",
        "codedGameState" : "1",
        "detailedState" : "Scheduled",
        "statusCode" : "1",
        "startTimeTBD" : false
      },
      "teams" : {
        "away" : {
          "leagueRecord" : {
            "wins" : 21,
            "losses" : 16,
            "ot" : 4,
            "type" : "league"
          },
          "score" : 0,
          "team" : {
            "id" : 20,
            "name" : "Calgary Flames",
            "link" : "/api/v1/teams/20"
          }
        },
        "home" : {
          "leagueRecord" : {
            "wins" : 22,
            "losses" : 17,
            "ot" : 3,
            "type" : "league"
          },
          "score" : 0,
          "team" : {
            "id" : 30,
            "name" : "Minnesota Wild",
            "link" : "/api/v1/teams/30"
          }
        }
      },
      "venue" : {
        "name" : "Xcel Energy Center",
        "link" : "/api/v1/venues/null"
      },
      "content" : {
        "link" : "/api/v1/game/2017020659/content"
      }
    } ],
    "events" : [ ],
    "matches" : [ ]
  } ]
}
```

`GET https://statsapi.web.nhl.com/api/v1/schedule?teamId=30&startDate=2018-01-02&endDate=2018-01-02` Returns Minnesota Wild games for January 2, 2018 with attached linescores and broadcasts.

```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "totalItems" : 1,
  "totalEvents" : 0,
  "totalGames" : 1,
  "totalMatches" : 0,
  "wait" : 10,
  "dates" : [ {
    "date" : "2018-01-02",
    "totalItems" : 1,
    "totalEvents" : 0,
    "totalGames" : 1,
    "totalMatches" : 0,
    "games" : [ {
      "gamePk" : 2017020608,
      "link" : "/api/v1/game/2017020608/feed/live",
      "gameType" : "R",
      "season" : "20172018",
      "gameDate" : "2018-01-03T01:00:00Z",
      "status" : {
        "abstractGameState" : "Final",
        "codedGameState" : "7",
        "detailedState" : "Final",
        "statusCode" : "7",
        "startTimeTBD" : false
      },
      "teams" : {
        "away" : {
          "leagueRecord" : {
            "wins" : 17,
            "losses" : 17,
            "ot" : 5,
            "type" : "league"
          },
          "score" : 1,
          "team" : {
            "id" : 13,
            "name" : "Florida Panthers",
            "link" : "/api/v1/teams/13"
          }
        },
        "home" : {
          "leagueRecord" : {
            "wins" : 21,
            "losses" : 16,
            "ot" : 3,
            "type" : "league"
          },
          "score" : 5,
          "team" : {
            "id" : 30,
            "name" : "Minnesota Wild",
            "link" : "/api/v1/teams/30"
          }
        }
      },
      "linescore" : {
        "currentPeriod" : 3,
        "currentPeriodOrdinal" : "3rd",
        "currentPeriodTimeRemaining" : "Final",
        "periods" : [ {
          "periodType" : "REGULAR",
          "startTime" : "2018-01-03T01:08:44Z",
          "endTime" : "2018-01-03T01:44:06Z",
          "num" : 1,
          "ordinalNum" : "1st",
          "home" : {
            "goals" : 1,
            "shotsOnGoal" : 13,
            "rinkSide" : "right"
          },
          "away" : {
            "goals" : 0,
            "shotsOnGoal" : 9,
            "rinkSide" : "left"
          }
        }, {
          "periodType" : "REGULAR",
          "startTime" : "2018-01-03T02:03:03Z",
          "endTime" : "2018-01-03T02:48:52Z",
          "num" : 2,
          "ordinalNum" : "2nd",
          "home" : {
            "goals" : 3,
            "shotsOnGoal" : 19,
            "rinkSide" : "left"
          },
          "away" : {
            "goals" : 0,
            "shotsOnGoal" : 2,
            "rinkSide" : "right"
          }
        }, {
          "periodType" : "REGULAR",
          "startTime" : "2018-01-03T03:07:33Z",
          "endTime" : "2018-01-03T03:43:39Z",
          "num" : 3,
          "ordinalNum" : "3rd",
          "home" : {
            "goals" : 1,
            "shotsOnGoal" : 9,
            "rinkSide" : "right"
          },
          "away" : {
            "goals" : 1,
            "shotsOnGoal" : 15,
            "rinkSide" : "left"
          }
        } ],
        "shootoutInfo" : {
          "away" : {
            "scores" : 0,
            "attempts" : 0
          },
          "home" : {
            "scores" : 0,
            "attempts" : 0
          }
        },
        "teams" : {
          "home" : {
            "team" : {
              "id" : 30,
              "name" : "Minnesota Wild",
              "link" : "/api/v1/teams/30"
            },
            "goals" : 5,
            "shotsOnGoal" : 41,
            "goaliePulled" : false,
            "numSkaters" : 5,
            "powerPlay" : false
          },
          "away" : {
            "team" : {
              "id" : 13,
              "name" : "Florida Panthers",
              "link" : "/api/v1/teams/13"
            },
            "goals" : 1,
            "shotsOnGoal" : 26,
            "goaliePulled" : false,
            "numSkaters" : 5,
            "powerPlay" : false
          }
        },
        "powerPlayStrength" : "Even",
        "hasShootout" : false,
        "intermissionInfo" : {
          "intermissionTimeRemaining" : 0,
          "intermissionTimeElapsed" : 0,
          "inIntermission" : false
        },
        "powerPlayInfo" : {
          "situationTimeRemaining" : 0,
          "situationTimeElapsed" : 0,
          "inSituation" : false
        }
      },
      "venue" : {
        "name" : "Xcel Energy Center",
        "link" : "/api/v1/venues/null"
      },
      "broadcasts" : [ {
        "id" : 14,
        "name" : "FS-N",
        "type" : "home",
        "site" : "nhl",
        "language" : "en"
      }, {
        "id" : 12,
        "name" : "FS-F",
        "type" : "away",
        "site" : "nhl",
        "language" : "en"
      } ],
      "content" : {
        "link" : "/api/v1/game/2017020608/content"
      }
    } ],
    "events" : [ ],
    "matches" : [ ]
  } ]
}
```
---

### <a name="seasons">Seasons
`GET https://statsapi.web.nhl.com/api/v1/seasons` Returns data on each season such as if ties were used, divisions, wildcards or the Olympics were participated in

```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2019. All Rights Reserved.",
  "seasons" : [ {
    "seasonId" : "20172018",
    "regularSeasonStartDate" : "2017-10-04",
    "regularSeasonEndDate" : "2018-04-08",
    "seasonEndDate" : "2018-06-07",
    "numberOfGames" : 82,
    "tiesInUse" : false,
    "olympicsParticipation" : false,
    "conferencesInUse" : true,
    "divisionsInUse" : true,
    "wildCardInUse" : true
  } ]
}
```

`GET https://statsapi.web.nhl.com/api/v1/seasons/20172018` Gets just the data for a specific season

`GET https://statsapi.web.nhl.com/api/v1/seasons/current` Returns the current season,  very useful for code that depends upon this information
---
### <a name="standings">Standings

`GET https://statsapi.web.nhl.com/api/v1/standings` Returns ordered standings data
for each team broken up by divisions

```
{
  "team" : {
    "id" : 52,
    "name" : "Winnipeg Jets",
    "link" : "/api/v1/teams/52"
  },
  "leagueRecord" : {
    "wins" : 37,
    "losses" : 17,
    "ot" : 9,
    "type" : "league"
  },
  "goalsAgainst" : 170,
  "goalsScored" : 213,
  "points" : 83,
  "divisionRank" : "2",
  "conferenceRank" : "3",
  "leagueRank" : "6",
  "wildCardRank" : "0",
  "row" : 35,
  "gamesPlayed" : 63,
  "streak" : {
    "streakType" : "losses",
    "streakNumber" : 1,
    "streakCode" : "L1"
  },
}
```

#### Modifiers
`?season=20032004` Standings for a specified season

`?date=2018-01-09` Standings on a specified date

`?expand=standings.record` Detailed information for each team including home and away records, record in shootouts, last ten games, and split head-to-head records against divisions and conferences

```
{
  "records" : {
    "divisionRecords" : [ {
      "wins" : 11,
      "losses" : 7,
      "ot" : 2,
      "type" : "Central"
    }, {
      "wins" : 5,
      "losses" : 3,
      "ot" : 3,
      "type" : "Atlantic"
    }, {
      "wins" : 15,
      "losses" : 4,
      "ot" : 2,
      "type" : "Pacific"
    }, {
      "wins" : 6,
      "losses" : 3,
      "ot" : 2,
      "type" : "Metropolitan"
    } ],
    "overallRecords" : [ {
      "wins" : 23,
      "losses" : 7,
      "ot" : 2,
      "type" : "home"
    }, {
      "wins" : 14,
      "losses" : 10,
      "ot" : 7,
      "type" : "away"
    }, {
      "wins" : 2,
      "losses" : 2,
      "type" : "shootOuts"
    }, {
      "wins" : 6,
      "losses" : 4,
      "ot" : 0,
      "type" : "lastTen"
    } ],
    "conferenceRecords" : [ {
      "wins" : 11,
      "losses" : 6,
      "ot" : 5,
      "type" : "Eastern"
    }, {
      "wins" : 26,
      "losses" : 11,
      "ot" : 4,
      "type" : "Western"
    } ]
  }
}
```
---

### <a name="standings-types">Standings Types

`GET https://statsapi.web.nhl.com/api/v1/standingsTypes` Returns all the standings types
to be used in order do get a specific standings

```
{
[ {
  "name" : "regularSeason",
  "description" : "Regular Season Standings"
}, {
  "name" : "wildCard",
  "description" : "Wild card standings"
}, {
  "name" : "divisionLeaders",
  "description" : "Division Leader standings"
}, {
  "name" : "wildCardWithLeaders",
  "description" : "Wild card standings with Division Leaders"
}, {
  "name" : "preseason",
  "description" : "Preseason Standings"
}, {
  "name" : "postseason",
  "description" : "Postseason Standings"
}, {
  "name" : "byDivision",
  "description" : "Standings by Division"
}, {
  "name" : "byConference",
  "description" : "Standings by Conference"
}, {
  "name" : "byLeague",
  "description" : "Standings by League"
} ]
```
Ex: https://statsapi.web.nhl.com/api/v1/standings/wildCardWithLeaders?date=2018-01-16

Returns the complete wildcard (with leaders) standings on 01/16/2018.

---

### <a name="stats-types">Stats Types

`GET https://statsapi.web.nhl.com/api/v1/statTypes` Returns all the stats types
to be used in order do get a specific kind of player stats

---

### <a name="team-stats">Team Stats

`GET https://statsapi.web.nhl.com/api/v1/teams/5/stats` Returns current season stats and the current season rankings for a specific team

Ex:

```
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "stats" : [ {
    "type" : {
      "displayName" : "statsSingleSeason"
    },
    "splits" : [ {
      "stat" : {
        "gamesPlayed" : 46,
        "wins" : 24,
        "losses" : 19,
        "ot" : 3,
        "pts" : 51,
        "ptPctg" : "55.4",
        "goalsPerGame" : 2.891,
        "goalsAgainstPerGame" : 3.043,
        "evGGARatio" : 0.6602,
        "powerPlayPercentage" : "26.5",
        "powerPlayGoals" : 43.0,
        "powerPlayGoalsAgainst" : 28.0,
        "powerPlayOpportunities" : 162.0,
        "penaltyKillPercentage" : "82.6",
        "shotsPerGame" : 34.7174,
        "shotsAllowed" : 30.3043,
        "winScoreFirst" : 0.76,
        "winOppScoreFirst" : 0.238,
        "winLeadFirstPer" : 0.857,
        "winLeadSecondPer" : 1.0,
        "winOutshootOpp" : 0.6,
        "winOutshotByOpp" : 0.375,
        "faceOffsTaken" : 2889.0,
        "faceOffsWon" : 1474.0,
        "faceOffsLost" : 1415.0,
        "faceOffWinPercentage" : "51.0",
        "shootingPctg" : 8.3,
        "savePctg" : 0.9
      },
      "team" : {
        "id" : 5,
        "name" : "Pittsburgh Penguins",
        "link" : "/api/v1/teams/5"
      }
    } ]
  }, {
    "type" : {
      "displayName" : "regularSeasonStatRankings"
    },
    "splits" : [ {
      "stat" : {
        "wins" : "15th",
        "losses" : "25th",
        "ot" : "30th",
        "pts" : "17th",
        "ptPctg" : "21st",
        "goalsPerGame" : "15th",
        "goalsAgainstPerGame" : "22nd",
        "evGGARatio" : "30th",
        "powerPlayPercentage" : "1st",
        "powerPlayGoals" : "1st",
        "powerPlayGoalsAgainst" : "17th",
        "powerPlayOpportunities" : "4th",
        "penaltyKillOpportunities" : "28th",
        "penaltyKillPercentage" : "10th",
        "shotsPerGame" : "1st",
        "shotsAllowed" : "5th",
        "winScoreFirst" : "8th",
        "winOppScoreFirst" : "24th",
        "winLeadFirstPer" : "10th",
        "winLeadSecondPer" : "1st",
        "winOutshootOpp" : "6th",
        "winOutshotByOpp" : "6th",
        "faceOffsTaken" : "1st",
        "faceOffsWon" : "3rd",
        "faceOffsLost" : "24th",
        "faceOffWinPercentage" : "12th",
        "savePctRank" : "25th",
        "shootingPctRank" : "24th"
      },
      "team" : {
        "id" : 5,
        "name" : "Pittsburgh Penguins",
        "link" : "/api/v1/teams/5"
      }
    } ]
  } ]
}
```

---

### <a name="draft">Draft

`GET https://statsapi.web.nhl.com/api/v1/draft` Get round-by-round data for current year's NHL Entry Draft.

`GET https://statsapi.web.nhl.com/api/v1/draft/YEAR` Takes a YYYY format year and returns draft data

```
{
  "copyright": "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "drafts": [{
    "draftYear": 2017,
    "rounds": [{
      "roundNumber": 1,
      "round": "1",
      "picks": [{
        "year": 2017,
        "round": "1",
        "pickOverall": 1,
        "pickInRound": 1,
        "team": {
          "id": 1,
          "name": "New Jersey Devils",
          "link": "/api/v1/teams/1"
        },
        "prospect": {
          "id": 65242,
          "fullName": "Nico Hischier",
          "link": "/api/v1/draft/prospects/65242"
        }
      },
```

### <a name="prospects">Prospects

`GET https://statsapi.web.nhl.com/api/v1/draft/prospects` Get all NHL Entry Draft prospects.

`GET https://statsapi.web.nhl.com/api/v1/draft/prospects/ID` Get an NHL Entry Draft prospect.

```json
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "prospects" : [ {
    "id" : 53727,
    "fullName" : "Zbynek Horak",
    "link" : "/api/v1/draft/prospects/53727",
    "firstName" : "Zbynek",
    "lastName" : "Horak",
    "birthDate" : "1995-03-08",
    "birthCountry" : "CZE",
    "height" : "5' 10\"",
    "weight" : 168,
    "shootsCatches" : "L",
    "primaryPosition" : {
      "code" : "L",
      "name" : "Left Wing",
      "type" : "Forward",
      "abbreviation" : "LW"
    },
    "draftStatus" : "Elig",
    "prospectCategory" : {
      "id" : 2,
      "shortName" : "Euro Skater",
      "name" : "European Skater"
    },
    "amateurTeam" : {
      "name" : "Znojmo Jr.",
      "link" : "/api/v1/teams/null"
    },
    "amateurLeague" : {
      "name" : "AUSTRIA-JR.",
      "link" : "/api/v1/league/null"
    },
    "ranks" : { }
  } ]
}
```

### <a name="awards">Awards

`GET https://statsapi.web.nhl.com/api/v1/awards` Get all NHL Awards.

`GET https://statsapi.web.nhl.com/api/v1/awards/ID` Get an NHL Award.

```json
{
  "copyright": "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2018. All Rights Reserved.",
  "awards": [ {
    "name": "Stanley Cup",
    "shortName": "The Cup",
    "description": "History: The Stanley Cup, the oldest trophy competed for by professional athletes in North America, was donated by Frederick Arthur, Lord Stanley of Preston and son of the Earl of Derby, in 1893. Lord Stanley purchased the trophy for 10 guineas ($50 at that time) for presentation to the amateur hockey champions of Canada. Since 1906, when Canadian teams began to pay their players openly, the Stanley Cup has been the symbol of professional hockey supremacy. It has been contested only by NHL teams since 1926-27 and has been under the exclusive control of the NHL since 1947.",
    "recipientType": "Team",
    "history": "It all started on March 18, 1892, at a dinner of the Ottawa Amateur Athletic Association. Lord Kilcoursie, a player on the Ottawa Rebels hockey club from Government House, delivered the following message on behalf of Lord Stanley, the Earl of Preston and Governor General of Canada  -- &quot;I have for some time been thinking that it would be a good thing if there were a challenge cup which should be held from year to year by the champion hockey team in the Dominion (of Canada).  There does not appear to be any such outward sign of a championship at present, and considering the general interest which matches now elicit, and the importance of having the game played fairly and under rules generally recognized, I am willing to give a cup which shall be held from year to year by the winning team.&quot; --  Shortly thereafter, Lord Stanley purchased a silver cup measuring 7.5 inches high by 11.5 inches across for the sum of 10 guineas (approximately $50); appointed two Ottawa gentlemen, Sheriff John Sweetland and Philip D. Ross, as trustees of that cup.  The first winner of the Stanley Cup was the Montreal Amateur Athletic Association (AAA) hockey club, champions of the Amateur Hockey Association of Canada for 1893. Ironically, Lord Stanley never witnessed a championship game nor attended a presentation of his trophy, having returned to his native England in the midst of the 1893 season. Nevertheless, the quest for his trophy has become one of the worlds most prestigious sporting competitions.",
    "imageUrl": "http://3.cdn.nhle.com/nhl/images/upload/2017/09/Stanley-Cup.jpg",
    "homePageUrl": "http://www.nhl.com/cup/index.html",
    "link": "/api/v1/awards/1"
  }]
}
```

### <a name="venues">Venues

`GET https://statsapi.web.nhl.com/api/v1/venues` Get all NHL Venues in API database.

`GET https://statsapi.web.nhl.com/api/v1/venues/ID` Get an NHL Venue.

```json
{
  "copyright" : "NHL and the NHL Shield are registered trademarks of the National Hockey League. NHL and NHL team marks are the property of the NHL and its teams. © NHL 2019. All Rights Reserved.",
  "venues" : [ {
    "id" : 5064,
    "name" : "Pepsi Center",
    "link" : "/api/v1/venues/5064",
    "appEnabled" : true
  } ]
}
```














