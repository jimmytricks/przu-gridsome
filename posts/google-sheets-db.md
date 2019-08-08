---
title: Using Google Sheets as a DB
date: 2019-01-02 07:42:34
description: "See how easy it is to use Google Sheets as a database when you only need to pull public data"
slug: google-sheets-db
blogpost: true
---

## Overview

This is a short guide on how you can use a public Google Sheet as a database.

There is plenty of content online about the pains of figuring out the Google Sheets API. What isn't very well documented, however, is that it's actually really simple to use as a DB, as long as you are only wanting to read data from a public sheet. 

## Diving in

After doing some light research into using Google Sheets as a database, much of the information that I was coming across all involved using a backend, and for the quick project I was creating, I didn't want to take this route.

On a deeper dive into the documentation, it is noted that you can make calls to public spreadsheets without any authentication or backend, with the only requirement being that you need to accompany the GET request with a Google API key. This makes for a quick and easy to use database on a small project, if you are happy for all information to be public. 

## Steps required

The steps for making the request are:

- Have your public Google Sheet
- Register for a Google API key
- Make your API Call 
- Profit(?)

## My project

Among some friends of mine we have a sports betting group for fun, where we share in each others' wins and losses. There was already a Google Sheet where this information was recorded, but it wasn't a very stylish or easily accessible way to view the data. 

I wanted to improve this to a one-page website that relayed this information, showing all winnings for each year, along with other information. 

For the frontend, I used the Vue CLI to spin up a quick application, displaying it in tables for my friends & I to look at, and used the existing Google Sheet as the database. [Here is the end product](https://lanchnation.przu.com) and [the source](https://github.com/jimmytricks/lanchnation) for those wanting to skip ahead. 

## Implementation

Once you have your Google Sheet up and running, you will need to register for a [Google Api Key](https://console.developers.google.com/apis/credentials).

After that, you are ready to make your API call, which will be a GET request like so:

`https://sheets.googleapis.com/v4/spreadsheets/SPREADSHEET_ID/Sheet1!A1:D5?key=YOUR_API_KEY`

- Replace SPREADSHEET_ID with the Google ID that you can find when viewing your sheet in the URL bar
- The 'Sheet1!A1:D5' portion represents the particular sheet name in your file, followed by the selection criteria. So, this will look at Sheet1 and copy all information between A1 and D5.
- Lastly, you need to add your Google API key where it says 'YOU_API_KEY

(Please note: To re-iterate, as no backend is used, your API key and data is not secured. Only use data / information that you are happy to be in the public domain).

Here is how the call might look with ES6 async/await syntax:

```
async function fetchData() {
    const response = await fetch('https://sheets.googleapis.com/v4/spreadsheets/1XnEPoMkYmKsjaZU2CqIFZ8W9jb7kBrVawm6qL2Tg0PU/values/currentapisheet!A1:B10?key=AIzaSyBrDvwps6J1X3Kges2TCnWTcJ_3LV7ZLqo').then(response => response.json());
    console.log(response);
}
```

This should then return a list of values matching the Google Sheet which you can manipulate and shape however you would like. As soon as you update the Google Sheet, the next request will show the updated information - simple as that, with no tears required. 

A link to [my project on GitHub](https://github.com/jimmytricks/lanchnation) for anyone that gets stuck or is curious. 

[The LanchNation website](https://lanchnation.przu.com)

Further Reading:

Google Developer Documentation info on making the request:
https://developers.google.com/sheets/api/samples/reading

Google Developer Documentation info on
https://developers.google.com/sheets/api/guides/authorizing