---
title: "Blogs Posted"
date: 2026-06-19
weight: 3
chapter: false
pre: " <b> 3. </b> "
---



###  [Blog 1 - User Authentication and Session Management with Amazon Aurora DSQL](3.1-Blog1/)
In backend applications, authentication and session management are almost essential components.
Users register, log in, maintain active sessions, log out, and revoke sessions. These flows may sound familiar, but when a system starts to scale, this problem becomes much more complex.
A common challenge is that the database must ensure data consistency almost immediately. For example, after a user registers, they should be able to log in right away. After a session is created, it should be usable in the next request. Once a session is revoked, it should no longer be allowed to access the system.


###  [Blog 2 - AUTOMATICALLY REPLICATING S3 BUCKET CONFIGURATIONS ACROSS AWS REGIONS](3.2-Blog2/)
When enterprises operate systems on AWS for a long period of time, it is very common to have hundreds or even thousands of Amazon S3 buckets in the same Region. Each bucket may have been created at different times, by different teams, and through different methods such as the AWS Management Console, internal scripts, or older automation tools.

###  [Blog 3 - PERFORMANCE OPTIMIZATION AND SERVICE ORCHESTRATION IN LARGE-SCALE SERVERLESS ARCHITECTURE](3.3-Blog3/)
When first adopting the **Serverless** model, most of us are attracted by the familiar promise from cloud providers: just focus on writing code, deploy the application, and the system will automatically scale based on demand.
However, there is always a significant gap between theory and real-world production operations at enterprise scale.
An important question arises: what happens when the system does not only handle a few thousand requests, but needs to scale to hundreds of thousands or even **1 million concurrent AWS Lambda functions**?
