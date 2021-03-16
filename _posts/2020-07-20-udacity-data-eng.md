---
title: "The Udacity Data Engineering Nanodegree"
date: 2020-04-01
tags: [Data Engineering, PostgreSQL, Cassandra, Apache Spark, Redshift, Lambda, Apache Airflow]
excerpt: "My experience with the Udacity Data Engineering Nanodegree"
---

Back at the start of the COIVD crisis I was presented with the opportunity to take one Udacity class for free, providing I could finish the course the within 30 days.  I chose to take the data engineering course as I found the concept of moving and bending data to fit an analytical need very appealing and appropriate considering my day job as a data scientist.  The course had several lessons which covered:

<ul>
  <li>PostgreSQL</li>
  <li>Cassandra</li>
  <li>AWS Redshift</li>
  <li>Apache Spark</li>
  <li>Apache Airflow</li>
</ul>

Each course covered the basics with a capstone at the end of the course.  While the courses were interesting I would not say I developed an expert level of understanding in any one area and it was more like a 100 level course on any one subject.  In the sections below I have broken out each course along with some of my thoughts on the course and a link to it in my portfolio.

<br>

<strong>PostgreSQL-Data-Modeling Project</strong>

This was the first course in the program and covered the basics of the relational data model and PostgreSQL.  This was a solid introduction to the relational data model along with the star data schema.  For me the course was a basic review as I have taken a data fundamentals course while pursuing my Master's degree.  The course videos were ok interesting and the speak was very well educated, but there were a couple of mistakes in the videos and I can't help but wonder why this professional organization didn't reshoot at least that part of the video.  The course and associate project were easy enough, but this was not new material to me.

[PostgreSQL Data Modeling](https://www.descriptdata.com/portfolio/data_model_postgresql/)

<br>

<strong>Data Modeling with Cassandra Project</strong>

The second course covered Apache Cassandra and again was a basic level course.  I did enjoy this course and learning more about the NoSQL wide column store concept.  I have worked with document and key value store NoSQL databases, but not a wide column.  Perhaps the most interesting part of this project was building a table for each query.  Once again the final project was not too difficult, at least in retrospect, but did offer some challenges.

[Data Modeling with Cassandra](https://www.descriptdata.com/portfolio/data_model_cassandra/)

<br>

<strong>Data Warehouse with AWS Redshift Project</strong>

This lesson focused on using AWS Redshift.  Other than the focus on use of AWS resources this project was similar to the PostgreSQL lesson.  I think the biggest take away from this project was interacting with AWS resources through Python.  Before this project I had only interacted with AWS through their web interface.  Using Python to move data between s3 and Redshift was also fun.  The project itself was not difficult, but learning how to interact with the different resources through code alone did take some time and reading through documentation to understand.

[Data Warehouse with AWS Redshift](https://www.descriptdata.com/portfolio/data_warehouse_redshift/)

<br>

<strong>Data Lakes with Apache Spark Project</strong>

The Spark lesson was the one I most interested in, but ultimately had the least satisfaction of.  While I understand the basic concept of Spark I was hopeful to understand more of the inner workings and how to interact with Spark.  The Spark lessons were pretty short when I went through, but I have noticed the addition of two lessons the Setting up Spark Clusters in AWS and Debugging and Optimization lessons.  I hope these provide more value to the lesson.  The project was not very difficult, but interaction with Spark was primarily through a SQL interface in Jupyter Notebooks.  

[Data Lakes with Apache Spark](https://www.descriptdata.com/portfolio/data_lake/)

<br>

<strong>Workflows with Apache Airflow Project</strong>

The previous lessons focused on moving data and inserting it into a database.  The Airflow lesson was focused on how to use Airflow to manage and monitor the process.  This lesson introduced a Directed Acyclic Graph or DAG.  In a DAG data flows in linear fashion from left to right and processes can be sequential or parallel depending on the requirement stages.  I think this was a much needed lesson to understand how to move from creating an ETL pipeline to actually managing it.  While I like working with Airflow as it is Python based, it may have been worth while to explore more industry standards such as Apache Nifi or Apache Kafka.

[Workflows with Apache Airflow](https://www.descriptdata.com/portfolio/airflow/)

<br>

<strong>ETL Pipeline for NFL Game Data Capstone Project</strong>

For the Udacity Capstone students have a choice to either use provided datasets or to use datasets from other places.  I choose to use the dataset from a personal project.  The goal was to build a data pipeline to ingest data from NFL endpoints and store it in an AWS Redshift Cluster.  I was able to use these endpoints; however, after I completely the project the NFL closed public access to these endpoints.  For this project I incorporated not only the tools in the lessons but also brought in AWS Lambda to help with some of the data transformations.  As this was a pet project I truly enjoyed it, and while the methodology worked it would be cost prohibitive for a personal budget.  As such I shut down all the services and deleted the data once the project was complete.

[ETL Pipeline for NFL Game Data](https://www.descriptdata.com/portfolio/nfl-redshift/)

<br>

<strong>Conclusion</strong>

This was an entertaining course and finishing all the lessons and project in under 30 days was a challenge.  As I stated before these classes gave a basic introduction and was at the 100 level of instruction.  My concern is the Udacity cost for five months of access is <strong>$1795</strong> or <strong>$359</strong> per month.  I feel this is a very large price tag for introductory level content.  I feel I could find similar content on a site like Udemy for 10 dollars or so.  I would be willing to pay about 50 dollars for this class, but not much more.

That being said I am glad I did it and I am really glad I didn't pay for it.  

<br>

<img src="/images/blog/2020-07-11/degree.PNG" alt="Degree Certificate">
