---
title: "Workflows with Apache Airflow"
excerpt: "Automating ETL workflows with Apache Airflow."
header:
  overlay_image:  images/port/airflow/airflow_banner.jpg
  overlay_filter: .25
  teaser: images/port/airflow/airflow_t.jpg
---

<h2> Project Background </h2>
This project will build upon the work completed in [Data Warehouse Project](https://www.descriptdata.com/portfolio/data_warehouse_redshift/).  This project will take that baseline work and build an automated ELT process using Apache Airflow.  This project used the baseline python sql scripts written earlier and the database schemas already developed and focused on building the workflow using Airflow

The GitHub for this project is located here: [Data-Pipeline-With-Airflow](https://github.com/tvanpat/data-pipeline-with-airflow)

<h2> Designing the DAG </h2>
The DAG is a Directed Acyclic Graph.  Generally speaking this is a map of the work and processes that need to take place.  Work can branch off and even happen in parallel.  The important part is that work never goes backwards.  

<ol>
  <li>In the first stage the data will be copied from the s3 buckets to the staging tables.
  <ul>
  <li><strong>Staging Events</strong>: For this portion data was copied from the log s3 bucket into the staging table.  This portion was a bit difficult to format correctly as the log JSON is a JSON-line file.  Airflow must have a configuration listing this specifically.  </li>
  <li><strong>Staging Songs</strong>: Data was copied from the Song s3 bucket into the songs staging table.  This data was not in JSON-lines format and just JSON.</li>
  </ul>
  </li>
  <li><strong>Load Songplay Fact Table</strong>: As the songplay fact table is the central table in the final schema, this table is loaded first, because it is important to know early if this insert fails.  It could be done with the dimension tables, but this way ensures the inserts are conducted without an error.  </li>
  <li> The dimension tables were loaded in parallel.
  <ul>
  <li><strong>Load Song Dimension Table</strong></li>
  <li><strong>Load Users Dimension Table</strong></li>
  <li><strong>Load Time Dimension Table</strong></li>
  <li><strong>Load Artist Dimension Table</strong></li>
  </ul>
  </li>
  <li><strong>Run Data Quality Checks</strong>:The last step was to run a basic count query against the tables to ensure they had been loaded with data.</li>
</ol>

<br>
<img src="/images/port/airflow/layout.png">
<br>


<h2> Lessons Learned/Final Thoughts </h2>
This was perhaps one of my favorite projects.  This project went beyond just writing table creation statements and insert queries, but rather focused on how to make this type of data movement sustainable.  While running a Python ETL script manually once a day is not difficult or time consuming, it is a process that can easily be automated.  By using a product such as airflow it is easy to not only automate the process but also monitor the pipeline for issues.

While the project itself was not difficult, it was important to learn the concepts and how to implement a sustainable process into Airflow.
