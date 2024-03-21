# League-of-Legends-Result-Predictions

by Monica Dai (mwdai@ucsd.edu)

---

## Introduction

The data used in this project is League of Legends 2023 Esports stats found on [Oracle's Elixir](https://oracleselixir.com/). The original dataset contains 125904 rows and 131 columns. Each 12 rows in the dataset corresponds to one match, in which the first 10 are individual players' data and the remaining two are rows of aggregrated team data. In this project, I focus my attention on data regarding gold stats. This theme emerged from my curiosity towards how gold can impact the outcomes of games, as I do not have any conscious understanding of this despite having played similar games before. I explore how players in each lane may have different dynamics with gold stats, and look into how this relates to the final victory or loss.

Descripions of Columns:

## Data Cleaning and Exploratory Data Analysis

To start, I filtered the columns to only keep the ones of interest: `gameid'`, `league`, `side`, `position`, `playerid`, `champion`, `result`, `earnedgold`, `earnedgoldshare`, `goldat10`, `golddiffat10`, `goldat15`, `golddiffat15`. I also added columns `more_gold_at_10` and `more_gold_at_15` to 