---
title: "ETL Pipeline for NFL Game Data"
excerpt: "Building a full ETL Pipeline for NFL game data for the Udacity Data Engineering Nanodegree."
header:
  overlay_image:  images/port/capstone/banner2.png
  teaser: images/port/capstone/teaser.png
---

<h2> Project Background </h2>
For the capstone Udacity project students have the choice of using one of their datasets or creating their own project.  I chose to do my own project using NFL data to build an ETL pipeline.  I was in the process of building a basic ETL pipeline for my [nfl2sql](https://github.com/tvanpat/nfl2sql) project, but I wanted to employ the methods I learned in this class to have a more robust and better database.  The good news is I was able to complete this project using data available from an NFL endpoint.  The bad news is as of early May 2020, this API closed to unregistered access, so I am unable to build upon the success in this project and as it stands this project is impossible to recreate.

The end goal of this project is to have a database where it is possible to conduct complex aggregate based queries against NFL data.  To do this I used game data available from the [NFL API](https://api.nfl.com/docs/getting-started/index.html), but as I stated earlier this API is no longer available to unregistered users.

The GitHub for this project is located here: [NFL-Data-Pipeline](https://github.com/tvanpat/udacity_data_eng_capstone_nfl_data)

<h2> Project Datasets </h2>
The NFL datasets used in this picture come from the NFL Teams, Schedule, Coaches, Players, and Games come from the NFL endpoint.

**Teams**:  The team data by year use to be found at https://www.nfl.com/feeds-rs/teams/<year>.json.  For example the data for all NFL teams for the year 2000 use to be found here https://www.nfl.com/feeds-rs/teams/2000.json.  The following data is provided from this endpoint:

  <ul>
      <li>Season: Season of the record </li>
      <li>Team ID: A unique ID for each team in the NFL. </li>
      <li>Team Abbreviation: Commonly used team abbreviation for example the Denver Broncos is "DEN". </li>
      <li>City/State: How the team is identified by location for example the Denver Broncos is "Denver". </li>
      <li>Full Name: The full name of the team. </li>
      <li>Nickname: Commonly nickname for the team. </li>
      <li>Team Type: Used to annotate if the team is a regular season team or the "Pro Bowl Team" </li>
      <li>Conference Abbreviation, ID, and Full Name </li>
      <li>Division Abbreviation, ID, and Full Name </li>
      <li>Year Founded </li>
      <li>Stadium name </li>
      <li>Ticket Phone Number </li>
      <li>Team URL </li>
      <li>Team Ticket URL </li>
  </ul>

**Schedule**:  The full schedule for the entire year was posted at https://www.nfl.com/feeds-rs/schedules/<yeaer>.json.  For example the 2019 season use to be found here https://www.nfl.com/feeds-rs/schedules/2019.json.  The only changes that are made during the year are for the post season game locations.  The following data is provided from this endpoint:

<ul>
    <li>Season: </li>
    <li>Season Type: This annotates if the is a preseason, regular season or post season game. </li>
    <li>Week: </li>
    <li>Game ID: A unique ID used for each game. </li>
    <li>Game Key: A unique ID used for each game </li>
    <li>Game Time Eastern: </li>
    <li>Game Time Local: </li>
    <li>ISO TIME: </li>
    <li>Home Team ID: </li>
    <li>Visitor Team ID: </li>
    <li>Home Team Abbr: </li>
    <li>Visitor Team Abbr: </li>
    <li>Home Display Name: </li>
    <li>Visitor Display Name: </li>
    <li>Home Nick Name: </li>
    <li>Visitor Nick Name: </li>
    <li>Game Type: </li>
    <li>Week Name Abbrievation: </li>
    <li>Week Name: </li>
    <li>Visitor Team, Season, Team ID, City/State, Full Name, Nickname, Team Type, Conference Abbr, Division Abbrievation: </li>
    <li>Home Team, Season, Team ID, City/State, Full Name, Nickname, Team Type, Conference Abbr, Division Abbrievation: </li>
    <li>Site ID, Site City, Site Full Name, Site State, Roof type, and network channel: </li>
</ul>

**Coaches** : Information for each coach for each team by year use to be found at https://www.nfl.com/feeds-rs/coach/byTeam/<teamid>/<season>.json.  For example the Denver Broncos coach information for 2019 could be found here: https://www.nfl.com/feeds-rs/coach/byTeam/1400/2019.json.  This contains basic biographical information for each coach.  The following data is provided:

<ul>
    <li>NFLID: This is a unique ID used for coaches and players. </li>
    <li>Status: This indicates if the coach or player is active, retired, or free agent. </li>
    <li>Display Name: </li>
    <li>First Name: </li>
    <li>Last Name: </li>
    <li>ESBID: This is a unique ID used for coaches and players.  This number is also used to generate the url for the picture used. </li>
    <li>Birthdate: </li>
    <li>Home Town: </li>
    <li>Is Deceased: </li>
    <li>Current Status: This indicates if the coach or player is active, retired, or free agent. </li>
    <li>College ID: </li>
    <li>College Name: </li>
</ul>

**Players**:  Player information was found at https://www.nfl.com/feeds-rs/playerStats/<nflId>.json.  The player information for Von Miller could found at https://www.nfl.com/feeds-rs/playerStats/2495202.json.  While this site provides some statistical information for the player this project will only use the following:

<ul>
    <li>NFLID: This is a unique ID used for coaches and players. </li>
    <li>Status: This indicates if the coach or player is active, retired, or free agent. </li>
    <li>Display Name: </li>
    <li>First Name: </li>
    <li>Last Name: </li>
    <li>ESBID: This is a unique ID used for coaches and players. </li>
    <li>GSISIS: This is a unique ID used for coaches and players. </li>
    <li>Middle Name: </li>
    <li>Suffix: </li>
    <li>Birth Date: </li>
    <li>Home Town: </li>
    <li>College ID: </li>
    <li>College Name: </li>
    <li>Position Group: </li>
    <li>Position: </li>
    <li>Jersey Number: </li>
    <li>Height: </li>
    <li>Weight: </li>
    <li>Team ID: </li>
</ul>

**Games**:  The site to get detailed game information was found at https://www.nfl.com/feeds-rs/boxscorePbp/<gameid>.json.  An example for this data source can be seen at: https://www.nfl.com/feeds-rs/boxscorePbp/2016020700.json.  This data source was primarily to fill the fact table.  This json document is deeply nested.  As part of the data preprocessing the JSON document will be flattened to be placed in the staging table.  The field from this data source which will be used are:

<ul>
    <li>Season:  </li>
    <li>Season Type:  This indicates if the game is Preseason, Regular Season, or Post Season.</li>
    <li>Week: </li>
    <li>Game Id: This is a unique identifier for each game.</li>
    <li>Game Key: This is a unique identifier for each game.</li>
    <li>Game Date:  </li>
    <li>ISO Time:  </li>
    <li>Home Team ID: This the unique id for the home team</li>
    <li>Visitor Team ID: This is the unique id for the visitor team.</li>
    <li>Game Type: This is the common name for the game, it can be Preseason, Regular Season, or even SB for Superbowl.</li>
    <li>Phase: This indicates the current phase of the game.  Unless the game is currently in session this should read "FINAL"</li>
    <li>Visitor Team Score Total: </li>
    <li>Visitor Team Score Q1:</li>
    <li>Visitor Team Score Q2: </li>
    <li>Visitor Team Score Q3:</li>
    <li>Visitor Team Score Q4: </li>
    <li>Visitor Team Score OT: </li>
    <li>Home Team Score Total: </li>
    <li>Home Team Score Q1:</li>
    <li>Home Team Score Q2: </li>
    <li>Home Team Score Q3:</li>
    <li>Home Team Score Q4: </li>
    <li>Home Team Score OT: </li>
    <li>Drive Sequence: This is a unique identifier for the drive for this specific game.</li>
    <li> Play ID: This is a unique identifier for this play for this drive sequence.</li>
    <li> Scoring: This is a Boolean value for this play resulted in a scoring drive.</li>
    <li> Scoring Team ID: This will indicate the ID of the scoring team if the play resulted in a score.</li>
    <li> Possession Team ID: This indicates the ID of the team that currently on offense.</li>
    <li> Play Type: This is a categorical variable which indicates the play type.  Most common forms are Pass, Rush, and Kick-off. </li>
    <li> Quarter: </li>
    <li>  Down: </li>
    <li>  Yards to Go: </li>
    <li>  First Down or Touch Down: </li>
    <li>  Play Stat Sequence: As a play can have multiple participants this provides a unique value for each portion of a play. </li>
    <li>  Stat ID: This is the Stat ID of the play.  This can be cross referenced in the stat_codes.csv, which will be included as a dimension table.</li>
    <li>  NFLID: This the unique ID of the player involved with this specific portion o f the play.</li>
    <li>  Yards: This is the number of yards this player is responsible for.  If they player had no yards the value will be zero.</li>
    <li>  Play Description: This is the number of yards this player is responsible for.  If they player had no yards the value will be zero.</li>
    <li>  Play Video: If the play contains a highlight video there will be a direct link to the video here, <a href= "http://video.nfl.com/films/2015/GAME_HIGHLIGHT/in-game-highlight/NFLCOM/POST/22/160207_nfl_super_bowl_cmp_strip_sack_touchdown_410520_500k.mp4">this is an example.</a>  </li>
</ul>


**STAT ID**:  This is a CSV containing the STAT ID codes and descriptions for each stat used in a game. The CSV can be seen here
 <a href= "./assets/statid_codes.csv ">STAT ID</a>.  The fields are described below.

 <ul>
    <li>STAT ID:  This is the unique identifier for each stat.</li>
    <li>Name:  This is the common name for the stat.</li>
    <li>Comment:  This is a detailed description of the stat.</li>
 </ul>


<h2> Project Tools </h2>
This project incorporated tools from both the Udacity course as well as other AWS offerings.  The tools used included:
<ul>
   <li>Python</li>
   <li>AWS S3</li>
   <li>AWS Lambda</li>
   <li>AWS Redshift</li>
   <li>Apache Airflow</li>
</ul>


<strong>Python:</strong> Python was used to access the NFL endpoints and conduct the data transformations to load the data into Redshift staging tables as well as the fact and dimension tables.  Python code was used on a local computer to initiate the data ingest and transformation, in AWS Lambda to access multiple endpoints simultaneously, and in Apache Airflow.

<strong>AWS s3:</strong> s3 cloud storage was used as a temporary storage location for the team data json, NFL schedule json, coach data json, player data json, and game data json files.

<strong>AWS Lambda:</strong> Lambda was used for a majority of the data transformation.  Lambda is serverless in nature and can be configured to run python code.  Lambda works very well on small pieces of code which can be fully executed in less than fifteen minutes.  As the transformations for the NFL data was simple, but there were many files to transform Lambda was used to parallelize these data transformations.  When a json file was loaded into an s3 bucket the appropriate Lambda would execute.

<strong>AWS Redshift:</strong> Redshift was used as the analytical database.  Data was loaded into staging tables from s3 and then sql queries were used to move the data into the fact and dimension tables.

<strong>AWS Apache Airflow:</strong> Airflow was used to monitor and manage some of the data transformation workflows for this project.

<h2>Project Process</h2>

<h2> Final Database Schema </h2>


<h2> Lessons Learned/Final Thoughts </h2>
I have worked with SQL databases for many years and part of my Master's program involved an in-depth class on relational theory and Oracle SQL databases, so the SQL basics of this class were not difficult.  Perhaps the thing I learned the most was to be willing to break away from third normal form when moving data from a record-based system to an analytic based system.  While I am still not entirely comfortable with this the though the speed impacts are hard to argue against.  For me the key would be an automated process to ensure values are keeping some type of consistency.  

Overall, this was a fun project and helped brush off the dust on my SQL skills.
