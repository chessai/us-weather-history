# us-weather-history

## Introduction

This is a practise problem, for teaching beginner and novice haskellers
some practical haskell skills. The goal is to learn how to:

  - Set up a haskell project from scratch using Cabal
  - Use libraries that a professional (and/or practical) haskell
    developer might in order to accomplish a specific task
  - Develop a simple application that performs some common task(s)
    seen in real-world programming

Beginners and Novices will be paired up with Experienced Haskell
developers. No more than 2 Beginners/Novices to 1 Experienced person.

Experienced Haskellers: Your role is mostly to facilitate the learning
of the lesser-experienced Haskellers. If you do all the work, they won't
learn anything. Try to be more of a guiding voice.

## Prerequisites

Before continuing, complete the following:

  1. Install [GHC](https://www.haskell.org/ghc/) and [Cabal](https://www.haskell.org/cabal/users-guide/intro.html). There are multiple ways
     of getting these. I recommend [ghcup](https://www.haskell.org/ghcup/) (Linux,MacOS,Windows)
     or [nix](https://nixos.org/nix/download.html) (Linux,MacOS).

     I recommend GHC 8.4 and newer, and Cabal 2 and newer.
     I myself will be using GHC 8.6.5 and Cabal 3.0.

  2. Set up a Cabal project (usually via `cabal init`).
     This will walk you through the process of setting up a cabal file.
     You will be building an application, so make sure you select
     the necessary option to get an executable in the cabal file.
     At the end it will ask if you want to include informative comments
     on the fields in the cabal file. I recommend you include these if
     you are not familiar with cabal files.

## Application requirements

  You will be calculating some simple statistics about United States
  Weather History, using a [FiveThirtyEight](https://github.com/fivethirtyeight/data) dataset. The datasets are stored in CSVs and
  are located in the data/ directory of this repo for your convenience.

  The csvs all have the same format:

Column | Description
---|---------
`date` | The date of the weather record, formatted YYYY-MM-DD
`actual_mean_temp` | The measured average temperature for that day
`actual_min_temp` | The measured minimum temperature for that day
`actual_max_temp` | The measured maximum temperature for that day
`average_min_temp` | The average minimum temperature on that day since 1880
`average_max_temp` | The average maximum temperature on that day since 1880
`record_min_temp` | The lowest ever temperature on that day since 1880
`record_max_temp` | The highest ever temperature on that day since 1880
`record_min_temp_year` | The year that the lowest ever temperature occurred
`record_max_temp_year` | The year that the highest ever temperature occurred
`actual_precipitation` | The measured amount of rain or snow for that day
`average_precipitation` | The average amount of rain or snow on that day since 1880
`record_precipitation` | The highest amount of rain or snow on that day since 1880

  Your task is to parse these CSVs and calculate some statistics about
  the data set. These statistics should then be placed into two separate
  json files, called "stats1.json" and "stats2.json", respectively.
  These statistics should be simple to calculate, since the focus of
  this project is to learn how to create a practical haskell application.

  The metrics you need to calculate are below:
    
  1. For each date, what is the range (difference between max and min) of the temperatures for that date?
       
  The output json should be an array of objects with the following format:

  ```json
  { "date": "YYYY-MM-DD",
    "temp_range": <SomeDouble, rounded to 3 decimal places>
  }
  ```

  2. For the date 2014-07-01 which is the date for which the statistic X is most similar (the difference between the two is smallest),
     for each X in:
       a) actual mean temperature
       b) actual max temperature
       c) actual min temperature
       d) average precipitation

  The output json should be an object with the following format:

  ```json
  { "actual_mean_temp_similar": {
      "year": YYYY,
      "difference": <SomeDouble, rounded to 3 decimal places>
    },
    "actual_max_temp_similar": {
      "year": YYYY,
      "difference": <SomeDouble, rounded to 3 decimal places>
    },
    "actual_min_temp_similar": {
      "year": YYYY,
      "difference": <SomeDouble, rounded to 3 decimal places>
    },
    "average_precipitation_similar": {
      "year": YYYY,
      "difference": <SomeDouble, rounded to 3 decimal places>
    },
  }
  ```

## Library recommendations

For CSV parsing: [cassava](http://hackage.haskell.org/package/cassava), [siphon](http://hackage.haskell.org/package/siphon)</br>
For generating JSON: [aeson](http://hackage.haskell.org/package/aeson)</br>
For time: [time](http://hackage.haskell.org/package/time), [chronos](http://hackage.haskell.org/package/chronos)
Text/Bytestring: [text](http://hackage.haskell.org/package/text), [bytestring](http://hackage.haskell.org/package/bytestring)</br>

Please note that both `cassava` and `aeson` have very powerful mechanisms
for automatically deriving CSV encoders/decoders and JSON encoders/decoders,
respectively, for your datatypes. Please do not do this.
You should understand how to marshal haskell datatypes to and fro
manually before you use the nice magic tools. This will help you
understand how these libraries work.

## Non-obvious Function helpers

For rounding, I recommend using `showFFloat` from `base`'s `Numeric`
module.

```haskell
showRoundedTo3Digits :: Double -> String
showRoundedTo3Digits d = showFFloat (Just 3) d ""
```

## Help
If any part of this document is unclear to a novice/beginner, feel free
to ask your experienced partner for clarification. If they do not know,
feel free to ask the organiser (me, @chessai).
