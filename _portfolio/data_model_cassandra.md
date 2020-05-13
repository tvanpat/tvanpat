---
title: "Data Modeling with Cassandra"
excerpt: " Data Modeling with Cassandra: Second project for the Udacity Data Engineering Nanodegree."
header:
  overlay_image:  images/port/cassandra_model/cassandra_banner.png
  overlay_filter: 0.5
  teaser: images/port/cassandra_model/cassandra_banner.png
---

<h2> Project Background </h2>
This project is part of the Udacity Data Engineering Nanodegree.  This is the introductory project after several classes on data modeling in the wide column store NoSQL database Cassandra.  For this project the student is to design a NoSQL database that is able to retrive information about songs users are listening to.  The student is required to design, build, and populate an analytical database from user logs and song data files.  

The GitHub for this project is located here: [Cassandra-Data-Modeling](https://github.com/tvanpat/data-modeling-with-apache-cassandra)

<h2> Project Datasets </h2>
The data set for this project is the same as for the [Cassandra-Data-Modeling Project](https://www.descriptdata.com/portfolio/data_model_postgresql/)

There are two primary datasets for this project.  The Song Dataset is a subset of data from the [Million Song Dataset](http://millionsongdataset.com/) and the Log Dataset which is simulated log data for the "Sparkify" company.  


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
In the Log Dataset each file contains one day's worth of user logs.  This log information is stored in a JSON Line format.  In a JSON line file each line is a valid JSON object; however, this means the actual file itself cannot be treated as a JSON file.  To avoid errors while reading this file into Python it is recommend to use pandas with the following format:

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


<h2> Project Process </h2>



<h2> Data Test </h2>


<h2> Lessons Learned/Final Thoughts </h2>
