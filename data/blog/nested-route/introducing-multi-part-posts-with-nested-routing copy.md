---
title: Discovering SQL Injections
date: '2024-03-09'
tags: ['SQL', 'Web Application', 'Server Side']
draft: false
summary: 'This post is a technical analysis of SQL injection vulnerabilites.'
---

# Why SQL?

As the world increasingly relies on powerful technologies, web applications play a prominent role as a primary form of computation for various purposes. These applications are a composite of multiple layers of technology, carefully stacked to provide a user-friendly and business-centric experience. However, amid the performance and functionality of web applications, certain configurations can inadvertently expose vulnerabilities, potentially allowing attackers to gain unauthorized access to confidential information or manipulate data. This poses a significant risk to the owners of these applications, making it crucial to address and mitigate such security concerns. In this post, i want to take a deeper dive in the world of SQL.

## Common Relational Database Management Systems (RDMS)

- [Oracle](https://www.oracle.com/database/)
- [MySQL](https://www.mysql.com/)
- [Microsoft SQL Server](https://ubuntu.com/)
- [PostgreSQL](https://www.microsoft.com/en-us/software-download/windows10ISO)

## What makes SQL Vulnerable?

Many times, applications need to use data from a database; think of passwords/usernames or patient records. The technology that manages this data is known as a Relational Database Management System or RDMS. The web application must interact with this database to pull the records it needs to perform its task. Occasionaly, the method in which the web application pulls this data is improperly set - allowing hackers to manipulate input, and access data that they should not have access to. SQL injections are born from improper and insecure SQL Query functions.

## Where can i find SQL Vulnerabilites?

SQL vulnerabilities exist within application logic. For instance, if a developer uses string concatenation in a SQL query, attackers can exploit this vulnerability by injecting SQL code to manipulate the data retrieved from the database. The attacker's goal is to identify a part of the application that interacts with the SQL query.

As an example, consider a shopping store with a product page that retrieves product IDs and prices from a database. If the web application extracts parameters for the SQL query from a Uniform Resource Locator (URL), an attacker could manipulate the URL to inject SQL query language, leading to data manipulation.
