# sportradar-advanced-challenge
 
 > Build a Data Pipeline using NHL Data

 The NHL hosts various public data feeds. This coding challenge is to set up a pipeline to ingest data. You'll need to monitor the schedule feed and when a game starts, or goes live, a corresponding job will activate and ingest the live data into a database. If it is off-season, then we should be able to reload data for a provided season or game. The separation of concerns, number of functions, and general approach is entirely up to you. 

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
  * Opponnet Team
  
* We should be able to query the database for up-to-date stats during, and after, a live game.

This exercise will serve as our initial evaluation of your development skills, it will also be used to further conversations we will have with you. We respect your time and the fact that you have a life and possibly a day-job, so put in as much time as you feel will be a fair representation of your skills. Ideally we'd like to see as much of this done as possible, but scoping and focus are relevant. First and foremost, we are looking for a solid representation of your code. 

While the approach you take to meeting the above objectives is up to you, here are a couple of things we will expect:

* Use NodeJs
* Use relational database
* There should be a basic README included.
* There should be tests.
* We should be able to run the application (provide instructions if need be).

Please do not create a pull-request against this repository; you should create your own project/repository.  Also--while not required--it would be nice to have access to your commit history (i.e. don't squash) through github. However if you are not comfortable with this for any reason, submitting a zip file with the contents of the project is acceptable as well.

[NHL API Documentation Summary](documentation.md)

