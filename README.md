# Engineering-projects

# AirFlow

This project implements an AirFlow pipeline to process air quality data from OpenAQ and store the results in a database.

## Requirements 
To evaluate the quality of living in some cities, we need to analyze the air quality. To do this, we can rely on a subset of the open dataset provided by OpenAQ. You are given a few datasets in ndjson format, which we expect you to process. You can find them [here](https://drive.google.com/file/d/1bH6BM7hrVI9ufuJ5GVGE7QPEwIJAM1xX/view?usp=sharing).

 We need to monitor the 24hr rolling average of the following measures for each city:

1. PM2.5
2. PM10
3. O3
4. NO2
5. CO

We are also interested in the Air Quality Index (AQI) values of the PM2.5 AQI and PM10 AQI, defined on the 24-hour average of PM2.5 and PM10, respectively (see formula in annex). We are only interested in the cities from the countries provided in `countries.csv`.

The data pipeline must take  data from Google drive, process it, and eventually store the resulting data in a SQLite database. The database should contain one or more tables with the hourly value of the metrics mentioned above (24-hour rolling average of PM2.5, PM10, O3 Ozone, NO2, and CO; And AQI PM2.5, and AQI PM10) for each city. 

## Implementation

The pipeline has three sequential tasks: extract_files() , get_data(), process_data()

This implementation follows the following principles:

1) The operations are idempotent, given that the previous task was successfully executed
2) Thus, the initial task (extract_files() ) is entirely idempotent

The staging area is the folder c:/tmp and tables obs_tmp in the sqlite database airquality.
The output area is airquality database.  
The file createdb.py creates airquality database.  

## Tasks

### 1) extract_files()

This function extracts from the URL the file input-dataserts.zip. Unzip this file and store its contents in the folder c:/tmp

### 2) get_data()

This function extracts the contents of the ndjson files, filters by countries, save these contents into the table obs_tmp, which stores the raw data from ndson files.

In detail, this function does the following:
This function reads c:/tmp/countries.csv  and load list of countries (country_list).
For each ndjson file, this function reads each json file inside and the contents. 
If the country of the observation is in  country_list and the parameter value is positive, then the data are inserted into obs_tmp 

At the end  obs_tmp  is sorted by country, city, and date and inserted into obs_air. 
Obs_air must be sorted since we need to compute the rolling averages. 

### 3) process_data()

This function reads the table obs_air and accumulates the values and observations for each parameter into the table indicators (from airquality database) for each combination of (country,city, date, and hour). With these accumulated values, we compute the hourly averages of the parameters, and in every insertion, the 24-hour rolling averages are computed based on hourly averages of the parameters.    After the computation of the rolling averages, Air Quality Index of pm25 and pm10 are computed and saved into the Indicators table.


## Usage:
In a Linux environment do 
1) Install python libraries i.e. pip install -r requirements.txt
2) Run pipeline.py


# Graphon Optimization
Graphons are limits of sequences of growing graphs and are functions defined on the unit square i.e. $W: [0,1]^2 \to [0,1]$. Graphons were invented to solve problems combinatorial problems on graphs asymptotically. Graphons are the ideal tool to model large networks.

In my PhD, I worked on the conjecture the most typical random networks satisfying combinatorial constraints like the density of edges, density of triangles, etc, can be represented by stepfunctions, which also are functions $X:[0,1]^2 \to [0,1]$ defined on the unit square in which are locally constant on squares $U \times U$ where $U$ is an interval in $[0,1]$. 

The code in the folder graphon_optimization computes the optimal stepfunctions for some combinatorial constraints by solving the following optimization problem:
$\min_X I(X)$

subjected to 
$SubgraphDensity1(X) = u_1$
...
$SubgraphDensityn(X) = u_n$ 

where $I(X)$ is the density of the Shannon Entropy generated by the random edges in $X$

## Usage:
1) Install python libraries i.e. pip install -r requirements.txt
2) Run OptimizaAG.py



# LIMS

This folder contains example of CRUD programs based on python to CRUD data from Excel sheets to a SQL Server database. The table used on this example are from LIMS (Laboratory Information Manangement System).

The CRUD programs assume the data of the excel sheet is sql view based on  one main table plus the name values derivated from the foreign keys of the main table. 

The view has the following columns:

Action |  several columns of the view |  id

The 'Action' column is string column in which the user can fill with 'I', 'U' or 'D' if the user wants to insert, update or delete a row in the view and 'id' store the id of the main table and it should not modified. 

Once the user has modified the excel sheet, the excel file is uploaded to the database using the corresponding function in tablesavers.py

In the folder, there are 2 python files:

1) tablesavers.py : Contain functions to read and save the contens of the excel files into the database  based on the view definition. 
2) utildb.py : contain functions to connect to the database and validate data.

## Usage:
1) Install python libraries i.e. pip install -r requirements.txt
2) Run test_savers.py


# Fake-erp-data

This is a project that simulated data of an ERP database for a retail business. The ERP base is a SQL Server database.

The main program is transaction_generator.py, which generates random documents to be consumed by accounting transactions.

The rest of programs are:

retail_transactions.py  contains all the functions for recording accounting transactions for a retail.

app_codes.py sets the constants for the REP

create_table_strings.py contains the string to create the tables

randomdocs.py contains functions to generate python dictionaries (docs) containing data of accounting transactions

randomrows.py contains functions to generate data of master tables 

utildb.py contains general functions to connect and manipulate the database

## Usage:

1) Create a blank database in Sql Server
2) Set the user, passsword, database_name and server in util.db 
3) Install python libraries i.e. pip install -r requirements.txt
4) Run transaction_generator.py


# erp-datamodels-factorization

This is a in-progress project whose purpose is to factor data models of ERPs contained the repository of Common Data Models from https://github.com/microsoft/CDM into data patterns given from David Hay's books "Data Model Patterns Conventions of Thought" and "DATA MODEL PATTERNS A Metadata Map". The goal is to use these patterns to build a deformable universal data model in which the business processes of ERPs will induce deformation operators of the universal data model into the final data model used by the ERP.

So far, I have created two files :

read_cmd.py: a python program that reads a CDM manifest file and then translates the definitions found in the repository into a json file containts these definitions more easily to manipulate.

DavidHay'sDataModelPatterns.txt : this file contains all data pattern models found in "Data Model Patterns Conventions of Thought" . The patterns are written in pseudo dml language similar to sql-dml language which includes subtyping and object-role-model relations 



