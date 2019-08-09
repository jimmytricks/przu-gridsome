---
title: Random Hockey Player
date: 2018-12-28
description: "Javascript application, uses the NHL API to randomly selects players from a team"
slug: randomhockeyplayer
website: true
websiteurl: https://randomhockeyplayer.przu.com/
sourceurl: https://github.com/jimmytricks/Random-Hockey-Player-Generator/
tech: ["JavaScript", "SCSS", "Postman"]
featureimg: "./imgs/randomhockeyplayer1.png"
---

## Overview

The random hockey player generator was created to coincide with an ice hockey viewing party. I wanted to create a drinking game, where across multiple games several people could have particular players selected.

If that player was to score or assist, the person would take a sip of their drink but it can be used for any other reason you might want to randomly choose a hockey player. 

The site uses the latest version of players as per the NHL database, so it will always be up to date. I used [Postman](https://www.getpostman.com/) extensively for setting up the data from the NHL API, which is an awesome tool that I couldn't live without, especially for undocumented API's like the NHL has.

## Features

* Vanilla JS
* Fetch API with chained promises
* NHL Rest API
* Players are not duplicated
* Message notifying when player has picked all players on team