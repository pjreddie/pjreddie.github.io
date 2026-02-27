---
title: "Joe's Go Database"
description: "Joe's Go Database (JGDB) is a dataset of more than 500,000 games by professional and top amateur Go players for training machine learning models to play Go."
layout: projects
order: 0
permalink: /projects/jgdb/
---

Joe's Go Database (JGDB) is a collection of more than 500,000 games by professional and top amateur Go (Baduk) players. It is one of the largest Go datasets online. The dataset is in `.sgf` format with limited metadata. I compiled the dataset for training machine learning models to play Go. Hopefully it will be useful for others who are looking for Go training data.

## Download ##

- [jgdb.tar.gz (194 MB)](https://data.pjreddie.com/files/jgdb.tar.gz)
- [jgdb.zip (345 MB)](https://data.pjreddie.com/files/jgdb.zip)

## The Dataset ##

JGDB was compiled from numerous sources, cleaned, stripped of metadata and comments, and saved as `.sgf` files. It contains more than 500,000 games of Go (Baduk) and is split into training, validation, and testing sets.

    train: 515,749 games
    val:     9,982 games
    test:    9,990 games

## Processing ##

The data is cleaned and stripped of metadata, comments, and variations. Only player rank, handicap, komi, result, and game moves are kept. Many games did not have final passes from both players, even if the result was a score, not resignation (i.e. `B+3.5`). For these games I added passing moves from one or both players. Due to processing there may be errors in some games, probably not too many. If you find anything let me know and I will fix it!

## Usage ##

You are free to use the dataset however you wish! The sequence of moves in a game is generally not copyrightable thus I am releasing JGDB into the public domain.
