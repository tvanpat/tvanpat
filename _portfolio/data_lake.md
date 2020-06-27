---
title: "Data Lakes with Spark"
excerpt: "Constructing an ETL pipeline from s3 to AWS Spark."
header:
  overlay_image:  images/port/spark/AWS-Main-banner.jpg
  overlay_filter: 0
  teaser: images/port/spark/spark_b.png
---

<h2> Project Background </h2>
This project is part of the Udacity Data Engineering nano-degree.  As such it will follow a similar style as [PostgreSQL-Data-Modeling Project](https://www.descriptdata.com/portfolio/data_model_postgresql/) and [Data Warehouse Project](https://www.descriptdata.com/portfolio/data_warehouse_redshift/) in that the company Sparkify is rapid gowning and need a data system which can support the analytical needs of their staff.  The Sparkify team started with PostgreSQL and then moved to Redshift and now wants to use Spark.

The GitHub for this project is located here: [Data-Lake-With-AWS](https://github.com/tvanpat/data-lake-with-aws)

<h2> Datasets </h2>
As with the [PostgreSQL-Data-Modeling Project](https://www.descriptdata.com/portfolio/data_model_postgresql/) and [Data Warehouse Project](https://www.descriptdata.com/portfolio/data_warehouse_redshift/) there are two primary datasets, the <strong>Song Dataset</strong> and the <strong>Log Dataset</strong>


<h3> Song Dataset</h3>
This is located at s3://udacity-dend/song_data.  A sample of the data is below:
 >{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}

<h3> Log Dataset</h3>
This is located at s3://udacity-dend/log_data.  A sample of the data is below:
 > {"artist":"Pavement", "auth":"Logged In", "firstName":"Sylvie", "gender", "F", "itemInSession":0, "lastName":"Cruz", "length":99.16036, "level":"free", "location":"Klamath Falls, OR", "method":"PUT", "page":"NextSong", "registration":"1.541078e+12", "sessionId":345, "song":"Mercy:The Laundromat", "status":200, "ts":1541990258796, "userAgent":"Mozilla/5.0(Macintosh; Intel Mac OS X 10_9_4...)", "userId":10}

<h2> Final Database Schema </h2>
There are 5 tables in the database.  This design focuses on the songplay table which houses the most important information for the analytics team.  The fact tables of time, users, songs, and artists help to provide context and additional details for the dimension songplay table.

The use tables are the **songplay_fact**, **time_dim**, **user_dim**, **song_dim**, and **artist_dim** tables.  These tables are in the

The **time table** which contains:

| Field        | Data Type          |
 |-------------  | ------------- |
| start_time      |int |
| hour      | int     |
| day | int      |
| week | int     |
| month | int      |
| year | int     |  
| weekday | int     |

The **users table** which contains:

| Field        | Data Type          |
| ------------- | ------------- |
| user_id      | int |
| first_name      | varchar      |
| last_name | varchar      |
| gender | varchar      |
| level | varchar     |

The **songs table** which contains:

| Field        | Data Type          |
| ------------- | ------------- |  
| song_id      | varchar |
| title      | varchar      |
| artist_id | varchar      |
| artist_name | varchar      |
| year | int     |
| duration | float     |

The **artists table** which contains:

| Field        | Data Type          |
| ------------- | ------------- |  
| artist_id      | varchar |
| name      | varchar      |
| location | varchar      |  
| latitude | varchar      |
| longitude | varchar   |

The **songplay table** which contains:

| Field        | Data Type          |
| ------------- | ------------- |
| songplay_id      | int |
| start_time      | int    |  
| user_id | int  |  
| song_id | varchar      |  
| artist_id | varchar     |  
| session_id | int  |
| location | varchar      |
| user_agent | varchar    |

<img src="/images/port/spark/snowflake_erd.PNG">

<h2> Lessons Learned/Final Thoughts </h2>
Of all the lessons in the nano-degree this was the one I was the most excited for and the one that I was the most let down by.  The lesson discussed the high level concepts of Spark but the actual details of working with Spark were limited.  In fact most of the time a Spark view was created to work with the data in an SQL like environment.  I will say since completing the nano-degree Udacity has added a new section with a focus on setting up Spark clusters on AWS and debugging in Spark.  I have not watched these but hopefully they add more value to this portion of the course.

Overall I did not learn much nor was I impressed with this section.
