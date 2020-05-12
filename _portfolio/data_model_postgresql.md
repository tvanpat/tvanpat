---
title: "Data Modeling with PostgreSQL"
excerpt: "Relational Data Modeling with PostgreSQL: First project for the Udacity Data Engineering Nanodegree."
header:
  overlay_image:  /images/port/data_model/data_erd.jpg
  overlay_filter: 0.5
  teaser: /images/port/data_model/data_erd-th.jpg
---

<h2> Project Background </h2>
This project is part of the Udacity Data Engineering Nanodegree.  This is the introductary project after several classes on data modeling in a traditional SQL database.  For this project the student is to take on the role of a data engineer for a startup called Sparkify which is a music streaming app similar to Spotify.  The student is required to design, build, and populate an analytical database from user logs and song data files.  

<h2> Project Datasets </h2>
There are two primary datasets for this project.  The Song Dataset is a subset of data from the [Millon Song Dataset](http://millionsongdataset.com/) and the Log Dataset which is simulated log data for the "Sparkify" company.  

<h3> Song Dataset</h3>
Each song is stored in a different JSON file.  An exmaple of the data reads as
<pre><code>
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
</code></pre>

The data schema for this JSON is:

| Field           | Data Type          |
 |-------------  | -------------         |
| num_songs            | INT                   |
| artist_id   | VARCHAR                   |
| artist_latitude  | REAL                 |
| artist_longitude            | REAL                    |
| artist_location   | VARCHAR                   |
| artist_name  | VARCHAR                 |
| song_id  | VARCHAR                 |
| title            | VARCHAR                    |
| duration   | NUMERIC                   |
| year  | INT                |

<h3> Log Dataset</h3>
In the Log Dataset each file contains one day's worth of user logs.  This log information is stored in a JSON Line format.  In a JSON line file each line is a valid JSON object; however this means the actual file itself cannot be treated as a JSON file.  To avoid errors while reading this file into Python it is recommend to use pandas with the following format:

>df = pd.read_json('<filename>.json', lines=True)

A sample of this log data in a dataframe is shown below:
<img src="/images/port/data_model/log-data_example.png" alt="Log File Example">

The data schema for the log dataset is:

| Field           | Data Type          |
 |-------------  | -------------         |
| artist            | VARCHAR                   |
| auth   | VARCHAR                   |
| firstName  | VARCHAR                 |
| gender            | VARCHAR                    |
| itemInSession   | INT                   |
| lastName   | VARCHAR                  |
| length  | REAL                 |
| level  | VARCHAR                 |
| location            | VARCHAR                    |
| method   | VARCHAR                   |
| page  | VARCHAR                |
| registration  | NUMERIC                |
| sessionId  | INT                |
| song  |  VARHCHAR                |
| status  | INT                |
| ts  | TIMESTAMP                |
| userAgent  | VARCHAR                |
| userId  | INT                |

<h2> Final Database Schema </h2>
To enable fast returns on the analytic queries the project database desing used a star schema.  By using the star schema the database will likely be fully 3NF, but the trade off is queries will return faster.  As the project dataset is small and the entries are being inserted via a python script the redunacy in the database will have little impact on the actual operation.

The star schema database has five tables: **songplays**, **users**, **songs**, **artists**, and **time**.  The database ERD is shown below:

<img src="/images/port/data_model/erd.png" alt="Project ERD Diagram">
