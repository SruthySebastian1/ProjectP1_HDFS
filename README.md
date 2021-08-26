# ProjectP1_HDFS
I set up a standalone single node cluster in Hadoop to utilize YARN and HDFS to perform Apache Hive queries through a CLI application to analyze data from a Movie-lens dataset.

## Technologies Used
* Hadoop - version 2.7.3.2.6.5.0-292
* HDFS - version 2.7.3.2.6.5.0-292
* Hive - version 2.6.5.0-292
* Git -version 2.32.0.windows.1

## Features
List of features ready and TODOs for future development using this project some query are solved using hiveQL ,following questions are listed below:

1. first 5 entries from movies table
2. number of unique movies
3. summary of ratings data
4. minimum rating given to a movie
5. maximum rating given to a movie
6. is any row null in tags
7. number of unique tags
8. drop null rows from tags and create another table of the resulting rows
9. filtering to get the list of drama movies
10. total number of drama movies
11. filtering to get the list of comedy movies
12. total no. of comedy movies
13. search movie id by tag search
14. displays first 5 data from ratings table
15. merging two tables movies and ratings into a new table without the "timestamp" column from the ratings table.
16. display high rated movies (rating > 4)
17. display low rated movies (rating < 4)
18. total number of unique movie genres
19. top 25 most rated movies
20. slicing out columns to display only title and genres columns from movies table
21. extract year from title of the movie
22. count how many times each of genres occur
23. Which movie received the highest number of ratings?
 
## Getting Started
To start this project user need to install sandbox-Hortonworks in virtual machine.After installing the VM start the VM then put the following command in Git bash then connect to VM using SSH command "ssh maria_dev@sandbox-hdp.hortonworks.com -p 2222" after this you need to put the password the default password for USER maria_dev is "maria_dev"

Performing the above action you are enter to VM CLI then do the following command which are listed below for the project.

* Create a folder in local VM using command "mkdir folder_name"
* In this folder clone the git repository from where we pull the dataset using command "git clone 'git repository link' "
* All dataSet are in a zip file we have to unzip it using command "unzip file_name" (N.B: if you are unzip in the current file ) .
* To open HiveQL terminal we have have to give command "Hive".
* Before performing all the above action try to check whether your Ambari which is management platform for hadoop started all services or not.
* In hive terminal ,check database and if not create one and use the database using following command and after creating database create tables and load data from the folder in   hdfs to the respected table and perform analysis using the query which are mention the txt file "project_1_command.txt"

### For creating database
create database mrt;                (#"mrt" is the database name here)

### Choose database to work with
use mrt;

### Create Folder in HDFS and Copy files from local to HDFS
* -------create folder-------
hdfs dfs -mkdir /projectp1/movie
hdfs dfs -mkdir /projectp1/rate
hdfs dfs -mkdir /projectp1/tag

* ------copy csv to hdfs-----
hdfs dfs -put movies.csv /projectp1/movie
hdfs dfs -put ratings.csv /projectp1/rate
hdfs dfs -put tags.csv /projectp1/tag

### Enforce Bucketing 
set hive.enforce.bucketing = true;

### Create External table
####  ------movies_t--------------
create external table if not exists movies_t(movieid int comment 'Id of Movie',
title string comment 'Movie Title',
genres string comment 'Movie Genre')
comment 'This is MOVIES table..'
row format delimited
fields terminated by ','
location '/projectp1/movie'
TBLPROPERTIES("skip.header.line.count"="1");

#### ------ratings_t--------------
create external table if not exists ratings_t(userId int comment 'User Id',
movieId int comment 'Movie Id',
rating int comment 'Movie Ratings',
`timestamps` bigint comment 'Time Stamp')
comment 'This is RATINGS table..'
row format delimited
fields terminated by ','
location '/projectp1/rate'
TBLPROPERTIES("skip.header.line.count"="1");

#### -------tags_t----------------
create external table if not exists tags_t(userId int comment 'User Id',
movieId int comment 'Movie Id',
tag string comment 'Movie Tags',
timestamps bigint comment 'Time Stamp')
comment 'This is TAGS table..'
row format delimited
fields terminated by ','
location '/projectp1/tag'
TBLPROPERTIES("skip.header.line.count"="1");

### load data 
* LOAD DATA INPATH '/projectp1/movie/movies.csv'
INTO TABLE mrt.movies_t;

* LOAD DATA INPATH '/projectp1/rate/ratings.csv'
INTO TABLE mrt.ratings_t;

* LOAD DATA INPATH '/projectp1/tag/tags.csv'
INTO TABLE mrt.tags_t;

The above Query are for creating table and load data in it,after loading data to the table all data from the hdfs folder are automatically removed i.e they are now loaded in the respective tables.

## Usage
Any one can perform analysis with the movielens dataset using the code.
