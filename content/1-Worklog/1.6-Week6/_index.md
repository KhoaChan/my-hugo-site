---
title: "Week 6 Worklog"
date: 2026-05-22
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---



## I. Executive Summary

Week 6 focused on **Data Lake architecture, NoSQL database design, serverless analytics, and big data processing on AWS**. While the previous weeks mainly covered networking, security, servers, CI/CD, and access management, this week allowed me to explore how data platforms are built for analysis and visualization.

The labs this week included **Data Lake on Amazon S3 with AWS Glue**, **NoSQL data modeling with Amazon DynamoDB**, **cost and performance analysis using AWS Glue and Amazon Athena**, and **building an end-to-end analytics pipeline with Amazon Kinesis, Amazon EMR, and Amazon QuickSight**.

Through this week’s practice, I gained a clearer understanding of the role of data in modern cloud systems. An AWS system does not only require servers, networking, and databases. It also needs the ability to collect, store, process, query, and visualize data to support decision-making.

---

## II. Strategic Objectives

The main objectives for Week 6 were:

* Understand the concept of a Data Lake and how to build one on AWS.
* Use Amazon S3 as a storage layer for raw, semi-structured, and processed data.
* Learn the role of AWS Glue Crawler in automatically detecting data schemas.
* Understand how Glue Data Catalog stores metadata for analytics services.
* Learn NoSQL data modeling principles with Amazon DynamoDB.
* Analyze access patterns before designing DynamoDB tables.
* Understand how to use Partition Key, Sort Key, and Global Secondary Index.
* Query data stored in S3 using Amazon Athena.
* Learn how to optimize query cost using columnar formats such as Parquet.
* Build an analytics pipeline with Kinesis, EMR, and QuickSight.
* Develop a data architecture mindset focused on scalability and cost optimization.

---

## III. Weekly Activity Log

| Time      | Activity Category         | Tasks Performed                                                                                           | Results / Evidence Achieved                                                 |
| --------- | ------------------------- | --------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| Day 1 - 2 | Data Lake Lifecycle       | Built a Data Lake using Amazon S3, created storage buckets, and configured AWS Glue Crawler to scan data. | A centralized data storage foundation was completed on S3                   |
| Day 3     | NoSQL Data Modeling       | Studied DynamoDB table design, Partition Key, Sort Key, and secondary indexes based on access patterns.   | Understood how to optimize DynamoDB schema based on real query requirements |
| Day 4     | Cost & Query Analytics    | Used AWS Glue and Amazon Athena to query logs or data stored in S3 using SQL.                             | Queried S3 data without managing analytics servers                          |
| Day 5 - 6 | End-to-End Analytics      | Explored an analytics pipeline using Kinesis, EMR, and QuickSight for data processing and visualization.  | Understood the full analytics workflow from ingestion to dashboard          |
| Day 7     | Documentation and Summary | Summarized technical notes, prepared screenshots/evidence, and updated the Hugo portfolio.                | Week 6 Worklog was completed                                                |

---

# IV. Detailed Technical Practice Through Labs

---

# LAB 35: Data Lake on AWS

## 1. Overview

A Data Lake is a centralized storage architecture that can store many types of data, including raw data, semi-structured data, system logs, and processed data. On AWS, Amazon S3 is commonly used as the main storage layer for Data Lakes because it is highly scalable, durable, and cost-effective.

In this lab, I practiced building a basic Data Lake using Amazon S3 and then used AWS Glue Crawler to automatically scan data and create metadata in the Glue Data Catalog. This metadata can later be used by analytics services such as Amazon Athena to query data directly.

## 2. Knowledge Learned

### Amazon S3 in a Data Lake

Amazon S3 acts as the central storage location for multiple types of data. Data can be organized into logical zones such as raw data, processed data, or curated data to simplify management and support later processing stages.

### AWS Glue Crawler

AWS Glue Crawler automatically scans data in S3, detects the data format, and infers the schema. After the crawler runs successfully, the schema information is updated in the Glue Data Catalog.

### Glue Data Catalog

Glue Data Catalog stores metadata about the data. Analytics services such as Athena can use this metadata to understand the structure of the data and run SQL queries on files stored in S3.

## 3. Implementation Process

The steps performed:

* Accessed the Amazon S3 service.
* Created a bucket for Data Lake storage.
* Organized data into logical folders such as `raw/`, `processed/`, or `analytics/`.
* Uploaded sample data into the S3 bucket.
* Accessed the AWS Glue service.
* Created an IAM Role that allowed Glue to read data from S3.
* Created an AWS Glue Crawler.
* Configured the input data path as an S3 folder.
* Ran the crawler to scan the data.
* Checked the schema generated in Glue Data Catalog.
* Verified that the metadata table was ready for analytics services.

## 4. Practice Evidence

![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image.png)
![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy.png)

## 5. Results Achieved

After completing this lab, I understood how to build a basic Data Lake foundation on AWS. I also learned the role of Amazon S3 in data storage and AWS Glue in automatic metadata discovery, which makes data easier to query and analyze.

---

# LAB 39: Amazon DynamoDB Immersion Day

## 1. Overview

Amazon DynamoDB is a fully managed NoSQL database service provided by AWS. It is suitable for applications that require high performance, low latency, and large-scale scalability. Unlike traditional relational databases, DynamoDB data design must be based heavily on application access patterns.

In this lab, I focused on DynamoDB table design, selecting Partition Key and Sort Key, and using Global Secondary Indexes to support queries outside the primary key structure.

## 2. Knowledge Learned

### NoSQL Data Modeling

NoSQL data modeling does not focus on table normalization like relational databases. Instead, the main access patterns of the application must be identified first, and the table keys and indexes should be designed accordingly.

### Partition Key and Sort Key

The Partition Key determines how data is distributed in DynamoDB. The Sort Key helps organize and query data within the same partition. Combining both keys allows more flexible query patterns.

### Global Secondary Index

A Global Secondary Index allows data to be queried using a key different from the table’s primary key. It is important when an application requires multiple query patterns.

## 3. Implementation Process

The steps performed:

* Accessed the Amazon DynamoDB service.
* Created a new DynamoDB table.
* Defined the appropriate Partition Key and Sort Key.
* Configured a capacity mode suitable for the lab environment.
* Added sample data to the table.
* Created a Global Secondary Index if additional access patterns were needed.
* Used PutItem to insert data.
* Used Query or Scan to verify the data.
* Observed how DynamoDB returned query results.
* Took notes on how key design affected query efficiency.

## 4. Practice Evidence

![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy%202.png)
![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy%203.png)

## 5. Results Achieved

After completing this lab, I understood the difference between SQL and NoSQL design thinking. With DynamoDB, access patterns should be identified before designing the table. I also understood how Partition Key, Sort Key, and GSI affect query performance and operating cost.

---

# LAB 40: Cost and Performance Analysis with AWS Glue and Amazon Athena

## 1. Overview

AWS Glue and Amazon Athena are commonly combined to build a serverless analytics system. AWS Glue automatically discovers schemas and manages metadata, while Amazon Athena allows users to run SQL queries directly on data stored in Amazon S3.

In this lab, I practiced using Glue and Athena to analyze logs or cost-related data stored in S3. This approach is efficient because no analytics server needs to be provisioned, and data can be queried on demand.

## 2. Knowledge Learned

### Serverless Analytics

Serverless Analytics allows users to analyze data without managing servers. Users only need to prepare data, metadata, and SQL queries.

### Amazon Athena

Amazon Athena enables SQL queries directly on S3 data. Athena charges based on the amount of data scanned, so data format and file organization directly affect query cost.

### Parquet Format

Parquet is a columnar data format that helps reduce the amount of data scanned during queries. Compared with CSV or JSON, Parquet is usually more efficient for large-scale analytics workloads.

## 3. Implementation Process

The steps performed:

* Prepared log data or sample data in Amazon S3.
* Configured AWS Glue Crawler to scan the data.
* Created or updated metadata tables in Glue Data Catalog.
* Accessed the Amazon Athena Query Editor.
* Selected the database and table created by Glue.
* Wrote SQL statements to query the data.
* Ran the query and checked the results.
* Observed the amount of data scanned.
* Took notes on query optimization through data organization and file format selection.

## 4. Practice Evidence

![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy%204.png)
![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy%205.png)

## 5. Results Achieved

After completing this lab, I understood how to use AWS Glue and Amazon Athena to analyze data stored in S3. I also learned that optimizing data formats, especially by using Parquet, can reduce scanned data and lower query costs.

---

# LAB 72: Analytics on AWS Workshop

## 1. Overview

In modern systems, data can be continuously generated from users, applications, devices, or operational logs. Therefore, building an end-to-end data processing pipeline is important for collecting, processing, and visualizing data for analysis.

In this lab, I learned about AWS analytics architecture using Amazon Kinesis, Amazon EMR, and Amazon QuickSight. Each service has a different role in the data pipeline: Kinesis collects streaming data, EMR processes big data, and QuickSight visualizes the results.

## 2. Knowledge Learned

### Amazon Kinesis

Amazon Kinesis is used to collect and process real-time streaming data. It is suitable for systems that need to handle logs, user events, or continuous data flows.

### Amazon EMR

Amazon EMR is a big data processing service based on frameworks such as Apache Spark or Hadoop. EMR is suitable for distributed processing, data cleaning, and large-scale analytics workloads.

### Amazon QuickSight

Amazon QuickSight is a data visualization and dashboard service. QuickSight helps transform processed data into charts, reports, and dashboards for end users or business teams.

## 3. Implementation Process

The steps performed:

* Studied the overall architecture of an analytics pipeline.
* Created or configured an Amazon Kinesis Data Stream.
* Selected the appropriate shard count or capacity mode.
* Monitored streaming data entering Kinesis.
* Learned how Amazon EMR processes large datasets.
* Configured an EMR cluster or EMR steps based on the lab objective.
* Prepared processed data for visualization.
* Connected the dataset to Amazon QuickSight.
* Created charts or analytics dashboards.
* Checked the visualization result and documented the workflow.

## 4. Practice Evidence

![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy%206.png)
![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy%207.png)

## 5. Results Achieved

After completing this lab, I understood how to build an end-to-end analytics system on AWS. I learned the role of Kinesis in streaming data ingestion, EMR in big data processing, and QuickSight in data visualization.

---

## V. Challenges Encountered and Solutions

During Week 6, I encountered several issues related to permissions, cost, and data optimization while practicing AWS analytics services.

### 1. Permission Error When Creating AWS Glue Crawler

During the Glue Crawler setup, the practice account had IAM permission limitations, which prevented automatic role creation or access to certain resources.

The solution was to review the error message, use an existing IAM Role if available, or simulate metadata and perform Athena queries within the allowed permission scope.

### 2. Cost Optimization When Using Athena

Athena charges based on the amount of data scanned. If data is stored in large CSV or JSON files, queries may scan more data and cost more.

I learned that data should be organized properly, partitioned by time or category, and converted into columnar formats such as Parquet to reduce scanned data.

### 3. DynamoDB Design Thinking

At first, I tended to design DynamoDB tables like relational databases. However, after learning more, I realized that DynamoDB should be designed based on access patterns rather than entity relationships alone.

The solution was to identify the main application queries first and then design the Partition Key, Sort Key, and GSI accordingly.

### 4. Cost Management for Large Analytics Services

Services such as EMR, Kinesis, and QuickSight can generate costs if left running. Therefore, resources should be monitored, screenshots should be captured immediately after practice, and unused resources should be stopped or deleted.

---

## VI. Reflection After Week 6

Week 6 helped me understand the importance of data in modern cloud architecture. Previously, I mainly focused on deploying applications, servers, and databases. After this week, I realized that data plays a key role in evaluating system performance, optimizing cost, and supporting decision-making.

I was especially impressed by how AWS combines multiple services to create a complete analytics pipeline. Amazon S3 can store data at scale, AWS Glue manages metadata, Athena enables direct SQL queries, DynamoDB supports low-latency applications, and Kinesis, EMR, and QuickSight support big data processing and visualization.

The most important lesson this week is that when designing cloud data systems, performance, cost, and scalability must be balanced. A good design not only improves query speed but also reduces long-term operating costs.

---

## VII. Plan for Next Week

In the next week, I will continue strengthening my AWS knowledge and start defining a clearer direction for the practical project.

The planned activities include:

* Continuing to complete advanced labs in the program.
* Discussing with mentors about errors encountered during practice.
* Researching the architecture of a Vietnam travel booking system integrated with an AI chatbot.
* Analyzing data flow, main services, and overall architecture for the project.
* Learning how to integrate AI services with backend and database layers.
* Preparing an architecture diagram for mentor review.
* Updating the Hugo portfolio with technical notes and practice screenshots/evidence.
