---
title: Working with the NHL API
date: 2018-11-07
description: "What I learnt working with the NHL API to create a couple of personal projects"
slug: working-with-nhl-api
blogpost: true
---

## Overview

The NHL API is a great resource for those wanting to work with hockey stats, though it doesn't have any official documentation, fortunately there has been some great work done to document it. 

## Documentation

The best documentation can be found at [https://github.com/dword4/nhlapi](https://github.com/dword4/nhlapi), which has now been moved to [https://gitlab.com/dword4/nhlapi](https://gitlab.com/dword4/nhlapi).

All initial queries will need `https://records.nhl.com/site/api`, which uses a GET request. There are a vast number of different endpoints, which you can find in the documentation above. Filtering is also available, such as `draftedByTeamId=ID`.

## Projects built with the API

I built a couple of projects with the API so far, with [DidYouWatchTheGame.com](https://didyouwatchthegame.com/) pulling live information, such as standings, scores and highlights. I also created [Random Hockey Player](https://randomhockeyplayer.przu.com/) Generator for those wanting to pick a player from a team at random, which can be useful as a drinking game. 

If you have a great idea for something that you would like built on the NHL API get in contact, as I am mildy hockey obsessed.