---
title: "Paul Allen Computing Challenge - SQLite Starter"
description: "SQLite is a great tool to get started with the PACC because it is self contained, serverless, and easy to set up. I provide the database files and some sample code to get you started."
layout: projects
order: 21
permalink: /projects/paul-allen-computing-challenge-sqlite-starter/
---

[SQLite](http://www.sqlite.org/) is standalone database software that can run SQL queries on database files without all of the hassle of setting up things like MySQL servers or dealing with configuration or authentication. All you have to do is [download](http://www.sqlite.org/download.html) a pre-compiled binary for your system and you are ready to do some data analysis!

## Installation

Installation is easy because the program is a small, stand-alone binary. Each student can just download a copy locally and keep it wherever they keep their data, no system-wide installation required!

## Get The Data

You do also need some data to work on, here are the SQLite database files for the PACC challenge:
-  [pacc_geo.db](https://data.pjreddie.com/files/pacc_geo.db) contains the geolocated tweets.

## Start SQLite

Now lets do some queries on the data! First, move the executable (`sqlite` on Linux/Mac, `sqlite.exe` on Windows) and the database file to the same folder. Then, from a terminal, start sqlite. On Windows this will look like:

    C:\Users\pjreddie>sqlite3.exe pacc.db
    SQLite version 3.7.13 2012-07-17 17:46:21
    Enter ".help" for instructions
    Enter SQL statements terminated with a ";"
    sqlite>

On Windows you can also double-click the `sqlite.exe` file, you'll just have to load the database once the shell starts:

    C:\Users\pjreddie>sqlite3.exe
    SQLite version 3.8.4.3 2014-04-03 16:53:12
    Enter ".help" for usage hints.
    Connected to a transient in-memory database.
    Use ".open FILENAME" to reopen on a persistent database.
    sqlite> .open pacc.db
    sqlite>

On Linux/Mac, you start the binary and pass in the database file name as a command line argument, just like on windows.

    [~/data] ls
    pacc.db    sqlite3
    [~/data] ./sqlite3 pacc.db
    SQLite version 3.8.4.3 2014-04-03 16:53:12
    Enter ".help" for usage hints.
    sqlite>

If SQLite is installed systemwide, you should leave off the `./` specifier when starting the program.

## Analyze Some Data

Once the SQLite command line interface (CLI) is running, we can start submitting SQL queries on our data. Here are some examples to get you started.

First, lets see what the database looks like:

    sqlite> .tables
    tweet
    sqlite> pragma table_info(tweet);
    0|id|bigint(20)|1||1
    1|createdAt|datetime|0|NULL|0
    2|collectionType|varchar(20)|0|NULL|0
    3|source|varchar(255)|0|NULL|0
    4|text|varchar(255)|0|NULL|0
    5|uuid|bigint(20)|0|NULL|0
    ...

It looks like all of the tweet information is in a single table called tweet. We can view the columns of this table using the `pragma table_info` command.

### Read Some Tweets
To read the first 5 tweets we can execute a command like:

    sqlite> select text from tweet limit 5;
    RT @bia_WHITness: snow day?? ..more like eat everything insight day
    I just want to go sledding. Why does it have to be so damn cold out?
    @Philippahanna Get Well Soon. Small dogs are the hot water bottles that never go cold :-)
    Bummed about the snow and cold weather?... KERBOOMKAFY your winter wardrobe with a KerBoomKa hat! While supplies... http://t.co/HMNMqRlVYu
    Im at Ice Lounge http://t.co/DrBnCbYtgu

To see who sent those tweets, we can do:

    sqlite> select fromUser from tweet limit 5;
    krwiles
    rachael_sawicki
    NeilBoast
    KerBoomKaFit
    miguelmarioneto

### Aggregate Statistics!

While individual tweets can be interesting, we really want to look for trends in our data. For that we can use commands that aggregate data in SQL. For example, to count the number of tweets in the dataset:

    sqlite> select count(*) from tweet;
    6784080

We can also check how many of these tweets are just re-tweets from other people:

    sqlite> select count(*) from tweet where retweeted_id != 0;
    2254380

Now for something a bit more complicated, lets see how tweets contain the phrase `#selfie`:

    sqlite> select count(*) from tweet where text like '%#selfie %';
    1637

We can also read some of the tweets:

    sqlite> select text from tweet where text like '%#selfie %' limit 5;
    ice in my veins. #selfie #belowzero #winter http://t.co/6c8J4GwLFH
    Its too cold outside for angels to fly  #selfie http://t.co/P6SmArYsYu
    Its cold so I wore my leather jacket and combat boots. #ootd #edgy #nofilter #selfie #becauseican http://t.co/44JcjQKq4u
    #balling #cold #selfie http://t.co/SwMfiO6oHb
    A cold #Selfie for #SelfieMonday because #SelfieSunday is too #Mainstream for this #Hipster. #Hat http://t.co/TZY6WzqM7g

That's *So* #Ratchet!
