---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# User Authentication and Session Management with Amazon Aurora DSQL

In backend applications, authentication and session management are almost essential components.

Users register, log in, maintain active sessions, log out, and revoke sessions. These flows may sound familiar, but when a system starts to scale, this problem becomes much more complex.

A common challenge is that the database must ensure data consistency almost immediately. For example, after a user registers, they should be able to log in right away. After a session is created, it should be usable in the next request. Once a session is revoked, it should no longer be allowed to access the system.

If the system experiences replication lag or requires manual management of multiple database instances, read replicas, credentials, scaling, and maintenance windows, the authentication layer can become significantly more complicated.

To address this challenge, AWS introduces an approach for building a user authentication and session management service with Amazon Aurora DSQL.

This solution combines Amazon Aurora DSQL, Amazon ECS Express Mode running on AWS Fargate, and AWS IAM to create a serverless backend architecture that is scalable, strongly consistent, and minimizes manual database credential management.

━━━━━━━━━━━━━━━

KEY HIGHLIGHTS

━━━━━━━━━━━━━━━

Use Amazon Aurora DSQL for authentication workloads

Aurora DSQL is a serverless, PostgreSQL-compatible database that supports strong read-after-write consistency. This makes it suitable for registration, login, and session validation flows because newly written data can be read immediately.

No need to manage database instances

Instead of manually provisioning instances, configuring read replicas, or calculating capacity, Aurora DSQL automatically scales based on workload demand. For an authentication service, this is important because login traffic can spike unexpectedly.

Connect to the database using IAM authentication

The application does not need to store database passwords in environment variables or configuration files. The Amazon ECS task role is granted permission to connect to Aurora DSQL, while the connector handles short-lived IAM tokens automatically.

Run the application on Amazon ECS Express Mode/Fargate

The Node.js/Express application is containerized and runs on Fargate. ECS Express Mode supports fast deployment, automatic scaling, load balancing integration, and reduces the amount of infrastructure management required.

Clear users and sessions table design

User data is stored in the users table, while session data is stored in the sessions table. Each session includes a token hash, creation time, expiration time, and revoked timestamp if the session has been revoked.

Do not store session tokens in plaintext

The session token is returned to the client only once when it is created. In the database, only the SHA-256 hash of the token is stored. This reduces risk if database data is accessed unexpectedly.

Protect passwords with bcrypt

User passwords are hashed with bcrypt before being stored. This is a common approach for protecting login credentials in backend applications.

Avoid leaking information during failed login attempts

The system returns a generic error for both cases: when the email does not exist and when the password is incorrect. This helps prevent user enumeration, where an attacker tries to determine which accounts exist in the system.

━━━━━━━━━━━━━━━

HOW DOES THE AUTHENTICATION FLOW WORK?

━━━━━━━━━━━━━━━

The client sends an HTTPS request to the backend application.

The Node.js/Express application running on Amazon ECS Express Mode receives the request, validates the input, and processes registration or login logic.

When a user registers, the application hashes the password with bcrypt, generates a UUID for the user, and stores the user information in the users table in Aurora DSQL.

When a user logs in, the application checks the email and password. If they are valid, the system generates a session token, hashes it using SHA-256, and stores the token hash in the sessions table.

For requests that require authentication, the client sends the session token. The backend hashes the received token, compares it with the token hash in the database, and checks whether the session is still valid.

When the user logs out or the session is revoked, the system updates the revoked_at field. Thanks to Aurora DSQL’s strong consistency, the revoked session will be rejected immediately in following requests.

━━━━━━━━━━━━━━━

WHY IS AURORA DSQL SUITABLE FOR THIS USE CASE?

━━━━━━━━━━━━━━━

The most notable point is that authentication does not only need a database that can “store data.” It also requires consistent and fast responses.

In some distributed systems, newly written data may not be readable immediately because of replication lag. This can cause problems in flows such as registering and logging in immediately afterward, or revoking a session while the session still remains valid for a short period.

Aurora DSQL solves this problem with strong read-after-write consistency. For authentication services, this is a major advantage because user and session states must be accurate almost immediately.

In addition, IAM authentication helps reduce security risks. Instead of manually managing database passwords, the system uses IAM roles and short-lived tokens to connect to the database.

━━━━━━━━━━━━━━━

LESSONS LEARNED

━━━━━━━━━━━━━━━

The main lesson I learned from this article is that although authentication is a familiar feature, designing it on the cloud still requires careful attention to consistency, security, and scalability.

Amazon Aurora DSQL is not only suitable for applications that need SQL. It is also worth considering for workloads that require strong data consistency, such as registration, login, and session management.

When Aurora DSQL is combined with ECS Express Mode/Fargate and IAM authentication, the backend can reduce many manual operations: there is no need to manage database instances, no need to store database passwords, scaling becomes easier, and the security model remains clear.

For me, this is a good example of how to design a modern backend on AWS: clearly separating the compute, database, and security layers while taking advantage of managed and serverless services to reduce operational overhead.

![alt text](/my-hugo-site/images/3-Blogpost/huy.png)


Link bài viết gốc AWS:
[https://aws.amazon.com/.../user-authentication-and.../...](https://aws.amazon.com/.../user-authentication-and.../...)
#AWS#AmazonAurora#AuroraDSQL#Backend#Database#IAM#Security #SessionManagement #CloudComputing #AWSStudyGroup