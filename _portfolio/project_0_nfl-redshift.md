---
title: "ETL Pipeline for NFL Game Data"
excerpt: "Building a full ETL Pipeline for NFL game data from the NFL API to AWS Redshift."
header:
  overlay_image:  images/port/capstone/banner2.png
  teaser: images/port/capstone/teaser.png
---

<h2> Project Background </h2>
The purpose of this project was to take NFL game data from the now deprecated NFL API extract the details, conduct required data transformations and store the data in AWS Redshift. I was going to use this project to enable me to further analyze NFL game data; however, the NFL has since restricted access to this dataset. As such it is impossible to replicate this project.

The end goal of this project is to have a database where it is possible to conduct complex aggregate based queries against NFL data.  To do this I used game data available from the [NFL API](https://api.nfl.com/docs/getting-started/index.html), but as I stated earlier this API is no longer available to unregistered users.

This project used several technologies:
<ul>
      <li>Apache Airflow for the workflow management</li>
      <li>AWS Lambda to fetch data, extract and transform data</li>
      <li>AWS s3 for JSON storage</li>
      <li>AWS Redshift used as the analytical database</li>
      <li>Python was used as the primary programming language</li>
</ul>

The GitHub for this project is located here: [NFL-Data-Pipeline](https://github.com/tvanpat/nfl_etl_redshift)

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

**Schedule**:  The full schedule for the entire year was posted at https://www.nfl.com/feeds-rs/schedules/<year>.json.  For example the 2019 season use to be found here https://www.nfl.com/feeds-rs/schedules/2019.json.  The only changes that are made during the year are for the post season game locations.  The following data is provided from this endpoint:

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

<h2> Final Database Schema </h2>

The data model used is a simple star data model.  This data model works well as it is easy to query with quick speeds.  The table has not been taken to 3NF, while this increases redundancy it will result in faster return times on queries.  Once the data has been preprocessed and placed in the correct s3 folder it can be copied into the the staging table.  From the staging tables it will be inserted into the correct fact and dimension tables.

The mapping for S3 folder to staging table to fact or dim table is as follows:

s3://nfl-cap/coaches >> coaches_staging >> coaches_dim

s3://nfl-cap/gameinfo >> gameinfo_staging >> game_dim

s3://nfl-cap/schedules_details >> schedules_details_staging >> game_dim

s3://nfl-cap/gameplays >> gameplays_staging >> play_fact

s3://nfl-cap/player_info >> player_info_staging >> players_dim

s3://nfl-cap/statids/ >> statid_codes_staging >> statid_dim

s3://nfl-cap/teams >> team_staging >> team_dim

<h3>Staging Tables </h3>

There are seven staging tables for this project.  They can be seen in this <a href= "https://github.com/tvanpat/udacity_data_eng_capstone_nfl_data/blob/master/assets/Staging%20Tables.pdf">PDF</a>

<strong> Coaches Staging Table</strong>

| Field           | Data Type          |
 |-------------  | ------------------- |
| coach_id     | varchar                        |
| season         | int                        |
| week                 | varchar                        |
| display_name   | varchar                        |
| first_name   | varchar                        |
| last_name   | varchar                        |
| esbid   | varchar                        |
| status   | varchar                        |
| birthdate   | date                        |
| hometown   | varchar                        |
| collge   | varchar                        |
| team_id   | varchar                        |
| isdeceased   | varchar                        |
| pic_url   | varchar                        |

<strong> Game Info Staging Table</strong>

| Field           | Data Type          |
 |-------------  | ------------------- |
| game_id     | varchar                        |
| season         | int                        |
| season_type                 | varchar                        |
| week   | varchar                        |
| game_key   | varchar                        |
| game_date   | date                        |
| game_time_iso   | timestamp                        |
| vis_points_total   | int                        |
| vis_points_q1   | int                        |
| vis_points_q2   | int                        |
| vis_points_q3   | int                        |
| vis_points_q4   | int                        |
| vis_points_ot   | int                        |
| home_points_total   | int                        |
| home_points_q1   | int                        |
| home_points_q2   | int                        |
| home_points_q3   | int                        |
| home_points_q4   | int                        |
| home_points_ot   | int                        |
| win_team  | varchar                        |
|  lose_team | varchar                        |
| site_id  | varchar                        |
| site_city  | varchar                        |
| site_full_name  | varchar                        |
| site_state  | varchar                        |
| roof_type   | varchar                        |
| phase  | varchar                        |

<strong> Game Plays Staging Table</strong>

| Field           | Data Type          |
 |-------------  | ------------------- |
| game_id     | varchar                        |
| week         | varchar                        |
| drive_seq                 | varchar                        |
| play_id   | varchar                        |
| play_stat_id   | varchar                        |
| season   | int                        |
| home_team   | varchar                        |
| def_team   | varchar                        |
| off_team   | varchar                        |
| vis_team   | varchar                        |
| penalty   | varchar                        |
| scoring   | varchar                        |
| scoring_team   | varchar                        |
| play_type   | varchar                        |
| quarter   | varchar                        |
| down   | int                        |
| yard_to_go   | numeric                        |
| first_down   | varchar                        |
| play_descript   varchar                        |
| play_vid  | varchar                        |
|  stat_id | varchar                        |
| yards  | int                        |
| player_id  | varchar                        |
| player_team  | varchar                        |

<strong> Player Info Staging Table</strong>

| Field           | Data Type          |
 |-------------  | ------------------- |
| nfl_id     | varchar                        |
| esb_id         | varchar                        |
| gsis_id                 | varchar                        |
| status   | varchar                        |
| display_name   | varchar                        |
| first_name   | varchar                        |
| last_name   | varchar                        |
| middle_name   | varchar                        |
| suffix   | varchar                        |
| birth_date   | date                        |
| home_town   | varchar                        |
| college_id   | varchar                        |
| college_name   | varchar                        |
| position_group   | varchar                        |
| position   | varchar                        |
| jersey_number   | varchar                        |
| height   | int                        |
| weight   | int                        |
| current_team   | varchar                        |
| player_pic_url   | varchar                        |

<strong> Statid Code Staging Table</strong>

| Field           | Data Type          |
 |-------------  | ------------------- |
| stat_id     | varchar                        |
| name         | varchar                        |
| comment                 | varchar                        |

<strong> Schedule Details Staging Table</strong>

| Field           | Data Type          |
 |-------------  | ------------------- |
| game_id     | varchar                        |
| season         | int                       |
| season_type                 | varchar                        |
| week     | varchar                        |
| game_key         | varchar                        |
| home_id                 | varchar                        |
| vis_id     | varchar                        |
| game_type         | varchar                        |
| week_name_abbr                 | varchar                        |
| week_name                 | varchar                        |

<strong> Team Staging Table</strong>

| Field           | Data Type          |
 |-------------  | ------------------- |
| team_id     | varchar                        |
| season         | int                       |
| abbr                 | varchar                        |
| citystate     | varchar                        |
| full_name         | varchar                        |
| nick                 | varchar                        |
| team_type     | varchar                        |
| conference_abbr         | varchar                        |
| division_abbr                 | varchar                        |
| year_found                 | int                        |
| stadium_name                 | varchar                        |

<h3>Fact and Dim Tables </h3>
There are five dimension tables and 1 fact table.  They can be seen in this <a href= "https://github.com/tvanpat/udacity_data_eng_capstone_nfl_data/blob/master/assets/Fact_Dim_Table%20ERD.pdf ">PDF</a>

<strong> Coaches Dim Table</strong>

| Field           | Data Type          | Key          |
 |-------------  | ------------------- | ------------------- |
| coach_id     | varchar  | PRIMARY |
| season         | int     | PRIMARY |
| week     | varchar   | PRIMARY|
| display_name     | varchar   | |
| full_name   | varchar    | |
| last_name    | varchar    | |
| esbid     | varchar    | |
| birthdate    | date     | |
| hometown    | varchar   | |
| college   | int    | |
| team_id   | varchar  | |
| isdeceased   | varchar  | |
| pic_url   | varchar  | |

<strong> Players Dim Table</strong>

| Field           | Data Type          | Key          |
 |-------------  | ------------------- | ------------------- |
| nflid     | varchar  | PRIMARY |
| esbid         | varchar     |  |
| gsisid     | varchar   | |
| status     | varchar   | |
| display_name   | varchar    | |
| first_name    | varchar    | |
| last_name     | varchar    | |
| middle_name    | date     | |
| suffix    | varchar   | |
| birthdate   | date    | |
| hometown   | varchar  | |
| college_id   | varchar  | |
| college   | varchar  | |
| position_group   | varchar  | |
| position   | varchar  | |
| jersey_number   | varchar  | |
| height   | int  | |
| weight   | int  | |
| current_team   | varchar  | |
| player_pic_url   | varchar  | |

<strong> Game Dim Table</strong>

| Field           | Data Type          | Key          |
 |-------------  | ------------------- | ------------------- |
| game_id     | varchar                        | PRIMARY |
| season         | int                        | |
| season_type                 | varchar                        | |
| week   | varchar                        | |
| game_key   | varchar                        | |
| game_date   | date                        | |
| game_time_iso   | timestamp                        | |
| vis_points_total   | int                        | |
| vis_points_q1   | int                        | |
| vis_points_q2   | int                        | |
| vis_points_q3   | int                        | |
| vis_points_q4   | int                        | |
| vis_points_ot   | int                        | |
| home_points_total   | int                        | |
| home_points_q1   | int                        | |
| home_points_q2   | int                        | |
| home_points_q3   | int                        | |
| home_points_q4   | int                        | |
| home_points_ot   | int                        | |
| win_team  | varchar                        | |
|  lose_team | varchar                        | |
| site_id  | varchar                        | |
| site_city  | varchar                        | |
| site_full_name  | varchar                        | |
| site_state  | varchar                        | |
| roof_type   | varchar                        | |
| game_phase  | varchar                        | |
| week_name_abbr  | varchar                        | |
| week_name  | varchar                        | |
| game_type  | varchar                        | |
| home_id   | varchar                        | |
| away_id  | varchar                        | |

<strong> Team Dim Table</strong>

| Field           | Data Type          | Key          |
 |-------------  | ------------------- | ------------------- |
| team_id     | varchar                        | PRIMARY |
| season         | int                       | |
| abbr                 | varchar                        | |
| citystate     | varchar                        | |
| full_name         | varchar                        | |
| nick                 | varchar                    |     |
| team_type     | varchar                        | |
| conference_abbr         | varchar                        | |
| division_abbr                 | varchar                        | |
| year_found                 | int                        | |
| stadium_name                 | varchar                        | |

<strong> Stat ID Dim Table</strong>

| Field           | Data Type          | Key          |
 |-------------  | ------------------- | ------------------- |
| stat_id     | varchar                        | PRIMARY |
| name         | int                       | |
| comment                 | varchar                        | |

<strong> Play Fact Table</strong>

| Field           | Data Type          | KEY |
 |-------------  | ------------------- | ------------------- |
 | guid     | sequence                       |  PRIMARY|
| game_id     | varchar                        |  |
| week         | varchar                        |  |
| drive_seq                 | varchar                        |  |
| play_id   | varchar                        |  |
| play_stat_id   | varchar                        |  |
| season   | int                        |  |
| home_team   | varchar                        |  |
| def_team   | varchar                        |  |
| off_team   | varchar                        |  |
| vis_team   | varchar                        |  |
| penalty   | varchar                        |  |
| scoring   | varchar                        |  |
| scoring_team   | varchar                        |  |
| play_type   | varchar                        |  |
| quarter   | varchar                        |  |
| down   | int                        |  |
| yard_to_go   | numeric                        |  |
| first_down   | varchar                        |  |
| play_descript  | varchar                        |  |
| play_vid  | varchar                        |  |
|  stat_id | varchar                        |  |
| yards  | int                        |  |
| player_id  | varchar                        |  |
| player_team  | varchar                        |  |


<h2>Project Process</h2>

There are two primary problems with this dataset. The first is gathering the data and the second is the primary dataset called games is a heavily nested json document and will require preprocessing before it can be loaded into a staging table. To gather a all the required json files the following process was used.

<img src="/images/port/capstone/data_preprocess.PNG" alt="Data Preprocess">

<h3> Step 1</h3>
 The data gathering process is initiated with a python script where the user enters in the season of the NFL data they want to enter into the database.  The script code can be viewed in the access_raw_data Jupyter notebook.  This script will execute four function functions

- The script will go to https://www.nfl.com/feeds-rs/schedules/<season>.json and do the following:
    - Save each Game ID as a separate JSON in s3
    - Save Game detailed information in s3

- For each team in the NFL schedule for that season the script will go to https://www.nfl.com/feeds-rs/teams/<season_to_get>.json and get detailed information for each team for the season selected.  Each team will be saved as a separate json in s3.

- For each team in the NFL schedule the script will go to https://www.nfl.com/feeds-rs/coach/byTeam/<team_id>/<season>.json and save a json file for each coach for each season.  The json file is saved by team, season, week.  If the script is run on a weekly basis it will be able to log coaching changes that occur during the regular season.


<h3> Step 2</h3>
The time to process a single season, deconstruct each game file save each play stat and player json file could easily take over an hour.  By using AWS Lambda this time can be reduced to minutes.  For step 2 for each gameid saved in s3 will start an AWS process which will download the complete game json and save it in another s3 bucket.  The only downside to this is each s3 prefix can only have one lambda trigger.  Also Lambda functions can run for a maximum of 15 minutes, so it is a best practice to keep Lambda functions simple.  It takes less than 30 seconds for Lambda to open each json file and download the game file and save it in the correct s3 folder.  The code for this Lambda function is saved in the lambda_functions folder under the name gameids.py.

<h3> Step 3</h3>
For each full game saved in s3, Lambda will launch and deconstruct each game saving each play stat as a separate json file in s3.  This function will execute a second task of saving each player from the game as a single json document in a separate json document.  The code for this function can be viewed in the lambda_functions folder under the name gamesheet_decon.py.

<h3> Step 4</h3>
For each Player ID saved Lambda will save the Player json file into an s3 bucket in a format that is ready to load into the Redshift staging table.

<h3> Step 5</h3>
Once the initial data transformation from has taken place and the de-nested json files are in the staging s3 buckets, the process can be started in Apache Airflow.  The data will be copied from s3 into the Redshift staging tables.

<h3> Step 6</h3>
Once the data has been moved from each bucket to the appropriate staging table, the staging tables will begin to populate the fact and dimension tables.  

<h3> Step 7</h3>
After all the staging tables have populated the fact and dimension tables a data quality check is conducted, and Airlow will stop the process.

<img src="/images/port/capstone/dag_chart.PNG" alt="DAG Chart">



<h2> Lessons Learned/Final Thoughts </h2>
The goal of this project was to build a database from NFL endpoints. This database will allow people to conduct queries on players, teams, and coaches performances. This can be used by NFL fans as well as those who play fantasy football to help plan their team and track potential points.
The primary tools for this project were Python, AWS Lambda, AWS s3, AWS Redshift, and Airflow. AWS Lambda was used to help preprocess data that could take hours if done sequentially, by using Lambda this data preprocessing was reduced to minutes. AWS s3 storage was used for the json files as the storage is cheap and easily connects to AWS Lambda and Redshift. AWS Redshift was used to store the data. By using a column store distributed database, data should be readily available and returned quickly. By using Airflow the ability to schedule the transfer of the data from S3 to staging tables and finally to the dimension and fact tables.

For me this project is bittersweet.  After working with the NFL data for about a year, I finally found a way to easily move the data into a database which allowed for aggregate analysis only for the NFL to stop public access to the endpoints.  This project allowed me to incorporate all the tools I had used in several other projects and was a great exercise.
