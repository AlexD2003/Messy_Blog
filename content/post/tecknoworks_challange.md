+++
author = "Alex Domnit"
title = 'Tecknoworks-Intenship challange'
date = 2024-08-30T16:03:22+03:00
draft = false
description = "Assessment dev trace"
tags = [
    "Completed",
]
categories = [
    "Assignment",
]

toc = false
+++

# Premise

#### Overview
This technical project is designed to evaluate your technical skills, creativity, and problem-solving abilities
in both Data Engineering and Artificial Intelligence. The project will help us assess how well you
understand the concepts and tools you'll be working with during the internship. The project should take
no more than 2 to 3 hours to complete.
#### Project Description
You are tasked with designing a simple data pipeline using Microsoft Fabric and performing data science
tasks using Python. Most likely, you don't have experience with Microsoft Fabric. We're interested in how
your mind works when you have to face a challenge like this. Microsoft Fabric is a comprehensive data
analytics platform that integrates data engineering, data science, data warehousing, and business
intelligence in one environment. By using Microsoft Fabric, we expect our interns to gain hands-on
experience with a tool that Microsoft expects to be widely adopted in the industry. This exposure not only
makes you more competitive in the job market but also prepares you for real-world scenarios where you
are likely to encounter similar integrated platforms.\
Microsoft Fabric is a paid tool, so in order to solve these exercises, you have to simply read the
documentation to have an introduction to the tool, watch a few tutorials if needed and answer the
questions based on your findings and your prior knowledge from University courses. You don't need to
access Fabric for this basic assignment. You don't have to implement the solution, only to design it based
on the documentation that you read. You can use text descriptions or any type of visual representation to
answer the questions (e.g. Dataflow diagrams) for Section 1. For Section 2 you can implement the solution
for free using Google Colab or any other local python environment.\
The project is divided into two main sections: Data Engineering and Artificial Intelligence.

# Section 1

#### Scenario

You are a new data engineer at a retail company. The company has a dataset containing information on
customer transactions. Your task is to design and implement a data pipeline that ingests, processes, and
stores this data using Microsoft Fabric.\
The dataset is available here: https://www.kaggle.com/datasets/fahadrehman07/retail-transaction-dataset?resource=download

#### Task 1 premise

Question: First, you have to bring this data into Fabric. How do you envision this? What will be the expected
steps? How exactly will it be stored? Is the data in a suitable format and size to ingest in Fabric? What can
you do related to storage in order to improve the performance of the future data queries if the size of the
dataset is expected to grow in time (hint: e.g. partition the data)?
(Answer with text descriptions or diagrams)

#### Task 1 solution

Firstly, I will begin by looking at the data set given in the premise in order to get a feeling for what I am working with.\
The data set is presented in **csv format and has a size of around 13MB (important for later)**. It seems to be a **retail transaction data set**, basically oferring you a peak at common consumer purchase patterns with the present collumns being : **CustomerId, ProductId, Quantity, Price, TransactionDate, PaymentMethod, StoreLocation, ProductCategory, AppliedDiscount and TotalAmount**.\
Since the data set is given in csv format which is compatible with the import feature of **Microsoft Fabric** and the size is not significant, the import should be pretty straight forward. We should plan for further expansion tho.\
**Okay, how how do I import it?** Firstly, in what? A quick look around google later, the most obvious choice was between a **Data Lake or a Data Warehouse**. I would go with a **Data Warehouse** since it is meant for already structured data (exactly like my data set) while a **Data Lake** is more inclined towards raw data meant for analysis and further implications in various projects. [Source.](https://azure.microsoft.com/en-us/resources/cloud-computing-dictionary/what-is-a-data-lake#:~:text=risks%20more%20efficiently.-,What's%20the%20difference%20between%20a%20data%20lake%20and%20a%20data,as%20specific%20BI%20use%20cases.)\
In order to import this data into Microsoft Fabric we could setup a pipeline. **Pipeline setup:**
1. Configure an automated data pipeline within Microsoft Fabric.
2. Source : We should point the pipeline towards the source location of the **csv file**.
3. Transformation : We should always think about transforming data when we are doing an import in order to further suit our needs, for example remove unnecessary collumns or filter just what we need.
4. Destination : Finally, we set the pipeline to store the ingested data into our Data Warehouse, as explained earlier.
#### [Source.](https://www.youtube.com/watch?v=LoESCJpru8w)

**What can you do related to storage in order to improve the performance of the future data queries if the size of the dataset is expected to grow in time (hint: e.g. partition the data)?**\

Since the ingested data is stored in a **Data Warehouse**, it is prone to efficient querying. In order to further improve the performance, from my work experience, I would suggest partitioning the data by **TransactionDate** and possibly by **ProductId**. Additionally, converting the data into Parquet format will improve storage efficiency. [Source.](https://www.macrometa.com/articles/what-is-data-partitioning)

#### Data flow diagram :

<img src="/img/dataingdiagram.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">
<br>