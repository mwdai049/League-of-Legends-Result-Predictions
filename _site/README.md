by Monica Dai (mwdai@ucsd.edu)

---

## Introduction

The data used in this project is League of Legends 2023 Esports stats found on [Oracle's Elixir](https://oracleselixir.com/). The original dataset contains 125904 rows and 131 columns. Each 12 rows in the dataset corresponds to one match, in which the first 10 are individual players' data and the remaining 2 are rows of aggregrated team data. In this project, I focus my attention on data regarding gold stats. This theme emerged from my curiosity towards how gold can impact the outcomes of games, as I do not have much understanding of this despite having some experience with similar games before. I explore how players in each lane may have different dynamics with gold stats, and look into how this relates to the final victory or loss.

Description of Columns:

`gameid`: a unique identification for each match in the dataset

`league`: the league in which the match takes place in

`side`: which side a player is on for the given match, either blue or red

`position`: the position a player played, either 'top', 'jng', 'mid', 'bot', or 'sup'

`playerid`: a unique identification for each professional player

`champion`: the name of the champion that the player chose for the given match

`result`: the final victory or loss, 1 means victory and 0 means loss

`earnedgold`: the total gold earned throughout the game

`earnedgoldshare`: each player's share of gold among the team's total gold

`goldat10`: the gold earned at 10 minutes into the game

`golddiffat10`: the difference in gold between given player and their lane opponent at 10 minutes into the game

`goldat15`: the gold earned at 15 minutes into the game

`golddiffat15`: the difference in gold between given player and their lane opponent at 15 minutes into the game


## Data Cleaning and Exploratory Data Analysis

To start, I filtered the rows to only keep player data. That is, I performed a query on the DataFrame to only keep rows in which `position` did not equal 'team', meaning it was either 'top', 'jng', 'mid', 'bot', or 'sup'. I also filtered the columns to only keep the ones of interest: `gameid`, `league`, `side`, `position`, `playerid`, `champion`, `result`, `earnedgold`, `earnedgoldshare`, `goldat10`, `golddiffat10`, `goldat15`, `golddiffat15`. Lastly, I added columns `more_gold_at_10` and `more_gold_at_15` which denoted `True` if this player had more gold than their lane opponent 10 or 15 minutes into the match, and `False` if they had less.

The first five rows to the processed DataFrame looks as follows:

<div style="overflow-x: auto;">
    <table>
    <thead>
        <tr>
        <th></th>
        <th>gameid</th>
        <th>league</th>
        <th>side</th>
        <th>position</th>
        <th>playerid</th>
        <th>champion</th>
        <th>result</th>
        <th>earnedgold</th>
        <th>earnedgoldshare</th>
        <th>goldat10</th>
        <th>golddiffat10</th>
        <th>goldat15</th>
        <th>golddiffat15</th>
        <th>more_gold_at_10</th>
        <th>more_gold_at_15</th>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td>0</td>
        <td>ESPORTSTMNT06_2753012</td>
        <td>LFL2</td>
        <td>Blue</td>
        <td>top</td>
        <td>oe:player:60aff1184bec1d2b2efdae84f5b6e3e</td>
        <td>Jax</td>
        <td>1</td>
        <td>13251</td>
        <td>0.295868</td>
        <td>3163</td>
        <td>76</td>
        <td>5059</td>
        <td>322</td>
        <td>True</td>
        <td>True</td>
        </tr>
        <tr>
        <td>1</td>
        <td>ESPORTSTMNT06_2753012</td>
        <td>LFL2</td>
        <td>Blue</td>
        <td>jng</td>
        <td>oe:player:fd78e127e45463dcfc2ea3836af0335</td>
        <td>Poppy</td>
        <td>1</td>
        <td>6478</td>
        <td>0.14464</td>
        <td>3035</td>
        <td>87</td>
        <td>4325</td>
        <td>-357</td>
        <td>True</td>
        <td>False</td>
        </tr>
        <tr>
        <td>2</td>
        <td>ESPORTSTMNT06_2753012</td>
        <td>LFL2</td>
        <td>Blue</td>
        <td>mid</td>
        <td>oe:player:baf7147fedeec5de54ca1f240952a3f</td>
        <td>Taliyah</td>
        <td>1</td>
        <td>10118</td>
        <td>0.225914</td>
        <td>3117</td>
        <td>-338</td>
        <td>4956</td>
        <td>-479</td>
        <td>False</td>
        <td>False</td>
        </tr>
        <tr>
        <td>3</td>
        <td>ESPORTSTMNT06_2753012</td>
        <td>LFL2</td>
        <td>Blue</td>
        <td>bot</td>
        <td>oe:player:8204ca38dc1c42012b5d53131271eb1</td>
        <td>Ezreal</td>
        <td>1</td>
        <td>11728</td>
        <td>0.261862</td>
        <td>3344</td>
        <td>329</td>
        <td>5217</td>
        <td>200</td>
        <td>True</td>
        <td>True</td>
        </tr>
        <tr>
        <td>4</td>
        <td>ESPORTSTMNT06_2753012</td>
        <td>LFL2</td>
        <td>Blue</td>
        <td>sup</td>
        <td>oe:player:bb97cd2e43cb0855f6485e6f9e93ea2</td>
        <td>Karma</td>
        <td>1</td>
        <td>3212</td>
        <td>0.0717161</td>
        <td>1953</td>
        <td>-79</td>
        <td>2827</td>
        <td>-216</td>
        <td>False</td>
        <td>False</td>
        </tr>
    </tbody>
    </table>
</div>

### Univariate Analysis

As the first step to EDA, I created a plot that visualizes the distribution of gold earned at 15 minutes for all players. I observed two peaks, roughly at the 3K and 5K gold marks. From prior knowledge, I know that Supports generally follow the Bot Laner into the bottom lane, and that they should be leaving last hits and kills to other players. Since last hits on minions and player kills all award gold, I believe this could explain the smaller peak at 3K gold, in that Supports will generally have less gold than the other players, thus forming its own peak. The other players seem to follow a more normalized distribution centered around 5K gold.

<iframe
  src="assets/uni-2.html"
  width="800"
  height="425"
  frameborder="0"
></iframe>

### Bivariate Analysis

To further explore the distribution of gold earned at 15 from above, I created a histogram facet grid for the distribution of gold earned at 15 for each position (top, jng, mid, bot, sup) and result (won, lost). All plots are on the same scale and follow a roughly normal distribution, though a little right skewness could be observed for the distributions of winning players (the first row of plots). The winning players' distributions (first row) also seem to have overall higher gold values than the losing players' distributions (second row), which makes sense because more gold seems to be tied with greater advantage. It's also noticeable that Supports, no matter the result, have distributions that are more centered around the mean than any other position. This observation brought me back to the gameplay patterns of Supports in which they generally leave last hits and kills to their teammates. This pattern could result in less variation in gold earned than other players, since Supports generally avoid the previously mentioned actions that gain gold. And as previously hypothesized, the gold distribution of Supports mostly lies in the 2-4K region, whereas the distribution of other positions mostly lies in the 4-6K region. This likely corresponds to the two peaks in the univariate analysis plot.

<iframe
  src="assets/bivar-2.html"
  width="850"
  height="425"
  frameborder="0"
></iframe>

### Interesting Aggregates

In searching for aggregates, I pivoted by the columns `more_gold_at_15` and `position`, taking the average of `result`. This evalutes to a table where I can examine the win rates of each position depending on if they had more or less gold than their lane opponent at 15 minutes. Since gold earned is a marker of advantage, it isn't difficult to see that those who had more gold (first row) had higher win rates than those who had less gold (second row).

| more_gold_at_15   |      bot |      jng |      mid |      sup |      top |
|:------------------|---------:|---------:|---------:|---------:|---------:|
| True              | 0.673164 | 0.633178 | 0.659186 | 0.663832 | 0.613327 |
| False             | 0.37426  | 0.40337  | 0.384426 | 0.381125 | 0.417681 |

To further examine these win rates, I took the difference in win rates between more gold and less gold to produce the following.

|      |      bot |      jng |      mid |      sup |      top |
|:-----|---------:|---------:|---------:|---------:|---------:|
| Diff in Win Rates | 0.298904 | 0.229808 | 0.274761 | 0.282708 | 0.195646 |

What caught my attention was the difference between Bot Laners and Top Laners. There's a difference of 10% between these two laners' difference in win rates, which led me to consider that gold advantage may tell more about a Bot Laner's result than about a Top Laner's result.

## Assessment of Missingness

I started out with finding out how many missing values each column had in the DataFrame. Results are as follows:

|                 |     null_count |
|:----------------|------:|
| gameid          |     0 |
| league          |     0 |
| side            |     0 |
| position        |     0 |
| playerid        |   934 |
| champion        |     0 |
| result          |     0 |
| earnedgold      |     0 |
| earnedgoldshare |     0 |
| goldat10        | 16660 |
| golddiffat10    | 16660 |
| goldat15        | 16660 |
| golddiffat15    | 16660 |
| more_gold_at_10 |     0 |
| more_gold_at_15 |     0 |
