# League of Legends Win Rate Analysis

by WonJae Lee (wolee@ucsd.edu)

---

## Introduction

Our dataset consists of professional competitive games of League of Legends in 2022. For each game, there are 12 rows, 5 player rows from each team that contain statistics for each player and 2 team rows that contain statistics for each team. Each row contains information about various statistics to assess the performance of the player or team using objectives (towers, dragons, barons, heralds) and combat and overall statistics like kills, assists, deaths, gold earned as well as their information like team name, player name, and champion played. There are 123 columns containing such statistics, and 149400 rows.
 
League of Legends players may be interested in matchup statistics (i.e. the win rate of a champion against a specific champion). Knowing such statistic will allow them to pick the right champions to facilitate their victory in games or they will have better understanding of a matchup when watching professional competitive games, especially at times like this when Worlds is going on. We will look at one specific champion for such analysis: **Sylas**.

Sylas is a very popular champion that is preferred by many professional League of Legends players for its capability for playmaking. Does Sylas perform consistently well against certain champions or does his win rate mostly remains constant against all champions?

---

## Cleaning and EDA

### Data Cleaning

From our dataset, we drop the columns we won't be using for our analysis for various reasons--some columns are all null, some are not informative, and some are redaundant. 

After we drop unnecessary columns, we check for null values in each column. From looking at the imported dataframe, we see that some columns are missing for the rows with 'team' position that are not missing in non-'team' positions. The non-'team' positions are positions of players, which are 'top', 'jng,' 'mid', 'bot', and 'sup'. The 'player' rows are largely missing some team statistics like how many dragons, towers, and heralds they got as a team. On the other hand, the 'team' rows are missing individual statistics like who got the first blood and its victim. Many of the columns in 'team' rows are the sum of 5 players ' statistics for the columns. To carry out the analysis to answer our question, we will incorporate the team statistics in the missing columns for player rows. 

After we merge players statistics with team statistics, we have a dataframe with both player and team statistics. However, looking at the dataframe, we see some columns should be boolean but aren't. Columns with 'first' keywords seem to indicate 'yes' and 'no' as their only values are 0 and 1, indicating it happened or didn't. Therefore, we convert these columns to boolean values.

This is our cleaned dataset:


| gameid                | date_x              |   game_x |   patch_x | side   | position_x   | playername_x   | teamname_x               | champion_x   | ban1_x   | ban2_x   | ban3_x   | ban4_x   | ban5_x   |   gamelength_x |   result_x |   kills_x |   deaths_x |   assists_x |   teamkills_x |   teamdeaths_x |   doublekills_x |   triplekills_x |   quadrakills_x |   pentakills_x | firstblood_x   | firstbloodkill_x   | firstbloodassist_x   | firstbloodvictim_x   |   team kpm_x |   ckpm_x |   barons_x |   inhibitors_x |   damagetochampions_x |   dpm_x |   damageshare_x |   damagetakenperminute_x |   damagemitigatedperminute_x |   wardsplaced_x |   wpm_x |   wardskilled_x |   wcpm_x |   controlwardsbought_x |   visionscore_x |   vspm_x |   totalgold_x |   earnedgold_x |   earned gpm_x |   earnedgoldshare_x |   goldspent_x |   total cs_x |   minionkills_x |   monsterkills_x |   cspm_x |   goldat10_x |   xpat10_x |   csat10_x |   golddiffat10_x |   xpdiffat10_x |   csdiffat10_x |   killsat10_x |   assistsat10_x |   deathsat10_x |   goldat15_x |   xpat15_x |   csat15_x |   golddiffat15_x |   xpdiffat15_x |   csdiffat15_x |   killsat15_x |   assistsat15_x |   deathsat15_x |   date |   game |   patch |   position |   playername |   teamname |   champion |   ban1 |   ban2 |   ban3 |   ban4 |   ban5 |   gamelength |   result |   kills |   deaths |   assists |   teamkills |   teamdeaths |   doublekills |   triplekills |   quadrakills |   pentakills |   firstblood |   firstbloodkill |   firstbloodassist |   firstbloodvictim |   team kpm |   ckpm |   barons |   inhibitors |   damagetochampions |   dpm |   damageshare |   damagetakenperminute |   damagemitigatedperminute |   wardsplaced |   wpm |   wardskilled |   wcpm |   controlwardsbought |   visionscore |   vspm |   totalgold |   earnedgold |   earned gpm |   earnedgoldshare |   goldspent |   total cs |   minionkills |   monsterkills |   cspm |   goldat10 |   xpat10 |   csat10 |   golddiffat10 |   xpdiffat10 |   csdiffat10 |   killsat10 |   assistsat10 |   deathsat10 |   goldat15 |   xpat15 |   csat15 |   golddiffat15 |   xpdiffat15 |   csdiffat15 |   killsat15 |   assistsat15 |   deathsat15 |   firstdragon |   dragons |   elementaldrakes |   infernals |   mountains |   clouds |   oceans |   chemtechs |   hextechs |   elders |   firstherald |   heralds |   firstbaron |   firsttower |   towers |   firstmidtower |   firsttothreetowers |   turretplates |
|:----------------------|:--------------------|---------:|----------:|:-------|:-------------|:---------------|:-------------------------|:-------------|:---------|:---------|:---------|:---------|:---------|---------------:|-----------:|----------:|-----------:|------------:|--------------:|---------------:|----------------:|----------------:|----------------:|---------------:|:---------------|:-------------------|:---------------------|:---------------------|-------------:|---------:|-----------:|---------------:|----------------------:|--------:|----------------:|-------------------------:|-----------------------------:|----------------:|--------:|----------------:|---------:|-----------------------:|----------------:|---------:|--------------:|---------------:|---------------:|--------------------:|--------------:|-------------:|----------------:|-----------------:|---------:|-------------:|-----------:|-----------:|-----------------:|---------------:|---------------:|--------------:|----------------:|---------------:|-------------:|-----------:|-----------:|-----------------:|---------------:|---------------:|--------------:|----------------:|---------------:|-------:|-------:|--------:|-----------:|-------------:|-----------:|-----------:|-------:|-------:|-------:|-------:|-------:|-------------:|---------:|--------:|---------:|----------:|------------:|-------------:|--------------:|--------------:|--------------:|-------------:|-------------:|-----------------:|-------------------:|-------------------:|-----------:|-------:|---------:|-------------:|--------------------:|------:|--------------:|-----------------------:|---------------------------:|--------------:|------:|--------------:|-------:|---------------------:|--------------:|-------:|------------:|-------------:|-------------:|------------------:|------------:|-----------:|--------------:|---------------:|-------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|--------------:|----------:|------------------:|------------:|------------:|---------:|---------:|------------:|-----------:|---------:|--------------:|----------:|-------------:|-------------:|---------:|----------------:|---------------------:|---------------:|
| ESPORTSTMNT01_2690210 | 2022-01-10 07:44:08 |        1 |     12.01 | Blue   | top          | Soboro         | Fredit BRION Challengers | Renekton     | Karma    | Caitlyn  | Syndra   | Thresh   | Lulu     |           1713 |          0 |         2 |          3 |           2 |             9 |             19 |               0 |               0 |               0 |              0 | False          | False              | False                | False                |       0.3152 |   0.9807 |          0 |              0 |                 15768 | 552.294 |       0.278784  |                 1072.4   |                      777.793 |               8 |  0.2802 |               6 |   0.2102 |                      5 |              26 |   0.9107 |         10934 |           7164 |        250.928 |            0.253859 |         10275 |          231 |             220 |               11 |   8.0911 |         3228 |       4909 |         89 |               52 |            -44 |              8 |             0 |               0 |              0 |         5025 |       7560 |        135 |              391 |            345 |             14 |             0 |               1 |              0 |    nan |    nan |     nan |        nan |          nan |        nan |        nan |    nan |    nan |    nan |    nan |    nan |          nan |      nan |     nan |      nan |       nan |         nan |          nan |           nan |           nan |           nan |          nan |          nan |              nan |                nan |                nan |        nan |    nan |      nan |          nan |                 nan |   nan |           nan |                    nan |                        nan |           nan |   nan |           nan |    nan |                  nan |           nan |    nan |         nan |          nan |          nan |               nan |         nan |        nan |           nan |            nan |    nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |           nan |       nan |               nan |         nan |         nan |      nan |      nan |         nan |        nan |      nan |           nan |       nan |          nan |          nan |      nan |             nan |                  nan |            nan |
| ESPORTSTMNT01_2690210 | 2022-01-10 07:44:08 |        1 |     12.01 | Blue   | jng          | Raptor         | Fredit BRION Challengers | Xin Zhao     | Karma    | Caitlyn  | Syndra   | Thresh   | Lulu     |           1713 |          0 |         2 |          5 |           6 |             9 |             19 |               0 |               0 |               0 |              0 | True           | False              | True                 | False                |       0.3152 |   0.9807 |          0 |              0 |                 11765 | 412.084 |       0.208009  |                  944.273 |                      650.158 |               6 |  0.2102 |              18 |   0.6305 |                      6 |              48 |   1.6813 |          9138 |           5368 |        188.021 |            0.19022  |          8750 |          148 |              33 |              115 |   5.1839 |         3429 |       3484 |         58 |              485 |            432 |             -5 |             1 |               2 |              0 |         5366 |       5320 |         89 |              541 |           -275 |            -11 |             2 |               3 |              2 |    nan |    nan |     nan |        nan |          nan |        nan |        nan |    nan |    nan |    nan |    nan |    nan |          nan |      nan |     nan |      nan |       nan |         nan |          nan |           nan |           nan |           nan |          nan |          nan |              nan |                nan |                nan |        nan |    nan |      nan |          nan |                 nan |   nan |           nan |                    nan |                        nan |           nan |   nan |           nan |    nan |                  nan |           nan |    nan |         nan |          nan |          nan |               nan |         nan |        nan |           nan |            nan |    nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |           nan |       nan |               nan |         nan |         nan |      nan |      nan |         nan |        nan |      nan |           nan |       nan |          nan |          nan |      nan |             nan |                  nan |            nan |
| ESPORTSTMNT01_2690210 | 2022-01-10 07:44:08 |        1 |     12.01 | Blue   | mid          | Feisty         | Fredit BRION Challengers | LeBlanc      | Karma    | Caitlyn  | Syndra   | Thresh   | Lulu     |           1713 |          0 |         2 |          2 |           3 |             9 |             19 |               0 |               0 |               0 |              0 | False          | False              | False                | False                |       0.3152 |   0.9807 |          0 |              0 |                 14258 | 499.405 |       0.252086  |                  581.646 |                      227.776 |              19 |  0.6655 |               7 |   0.2452 |                      7 |              29 |   1.0158 |          9715 |           5945 |        208.231 |            0.210665 |          8725 |          193 |             177 |               16 |   6.7601 |         3283 |       4556 |         81 |              162 |             71 |              0 |             0 |               1 |              0 |         5118 |       6942 |        120 |             -475 |            153 |              1 |             0 |               3 |              0 |    nan |    nan |     nan |        nan |          nan |        nan |        nan |    nan |    nan |    nan |    nan |    nan |          nan |      nan |     nan |      nan |       nan |         nan |          nan |           nan |           nan |           nan |          nan |          nan |              nan |                nan |                nan |        nan |    nan |      nan |          nan |                 nan |   nan |           nan |                    nan |                        nan |           nan |   nan |           nan |    nan |                  nan |           nan |    nan |         nan |          nan |          nan |               nan |         nan |        nan |           nan |            nan |    nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |           nan |       nan |               nan |         nan |         nan |      nan |      nan |         nan |        nan |      nan |           nan |       nan |          nan |          nan |      nan |             nan |                  nan |            nan |
| ESPORTSTMNT01_2690210 | 2022-01-10 07:44:08 |        1 |     12.01 | Blue   | bot          | Gamin          | Fredit BRION Challengers | Samira       | Karma    | Caitlyn  | Syndra   | Thresh   | Lulu     |           1713 |          0 |         2 |          4 |           2 |             9 |             19 |               0 |               0 |               0 |              0 | True           | False              | True                 | False                |       0.3152 |   0.9807 |          0 |              0 |                 11106 | 389.002 |       0.196358  |                  463.853 |                      218.879 |              12 |  0.4203 |               6 |   0.2102 |                      4 |              25 |   0.8757 |         10605 |           6835 |        239.405 |            0.242201 |         10425 |          226 |             208 |               18 |   7.9159 |         3600 |       3103 |         78 |              296 |            265 |            -12 |             1 |               1 |              0 |         5461 |       4591 |        115 |             -793 |          -1343 |            -34 |             2 |               1 |              2 |    nan |    nan |     nan |        nan |          nan |        nan |        nan |    nan |    nan |    nan |    nan |    nan |          nan |      nan |     nan |      nan |       nan |         nan |          nan |           nan |           nan |           nan |          nan |          nan |              nan |                nan |                nan |        nan |    nan |      nan |          nan |                 nan |   nan |           nan |                    nan |                        nan |           nan |   nan |           nan |    nan |                  nan |           nan |    nan |         nan |          nan |          nan |               nan |         nan |        nan |           nan |            nan |    nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |           nan |       nan |               nan |         nan |         nan |      nan |      nan |         nan |        nan |      nan |           nan |       nan |          nan |          nan |      nan |             nan |                  nan |            nan |
| ESPORTSTMNT01_2690210 | 2022-01-10 07:44:08 |        1 |     12.01 | Blue   | sup          | Loopy          | Fredit BRION Challengers | Leona        | Karma    | Caitlyn  | Syndra   | Thresh   | Lulu     |           1713 |          0 |         1 |          5 |           6 |             9 |             19 |               0 |               0 |               0 |              0 | True           | True               | False                | False                |       0.3152 |   0.9807 |          0 |              0 |                  3663 | 128.301 |       0.0647631 |                  475.026 |                      490.123 |              29 |  1.0158 |              14 |   0.4904 |                     11 |              69 |   2.4168 |          6678 |           2908 |        101.856 |            0.103054 |          6395 |           42 |              42 |                0 |   1.4711 |         2678 |       2161 |         16 |              528 |           -587 |              1 |             1 |               1 |              0 |         3836 |       3588 |         28 |              443 |           -497 |              7 |             1 |               2 |              2 |    nan |    nan |     nan |        nan |          nan |        nan |        nan |    nan |    nan |    nan |    nan |    nan |          nan |      nan |     nan |      nan |       nan |         nan |          nan |           nan |           nan |           nan |          nan |          nan |              nan |                nan |                nan |        nan |    nan |      nan |          nan |                 nan |   nan |           nan |                    nan |                        nan |           nan |   nan |           nan |    nan |                  nan |           nan |    nan |         nan |          nan |          nan |               nan |         nan |        nan |           nan |            nan |    nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |           nan |       nan |               nan |         nan |         nan |      nan |      nan |         nan |        nan |      nan |           nan |       nan |          nan |          nan |      nan |             nan |                  nan |            nan |


### Univariate Analysis

<iframe src="./assets/number_of_games.html" width=1000 height=500 frameBorder=0></iframe>
As we can see from the graph, Sylas was played in 1608 games, providing us with sufficient number of games to analyze. We also notice that some champions are preferred or played a lot more than others from the graph trend.

<iframe src="./assets/sylas_overall_wr.html" width=500 height=500 frameBorder=0></iframe>
Sylas has a decent overall winrate of 51.4% throughout all competitive games in 2022.


### Bivariate Analysis

<iframe src="./assets/dpm_vs_kills.html" width=1000 height=500 frameBorder=0></iframe>
Plotting dpm against kills, we can find a clear trend that higher dpm is correlated with more kills, meaning when the champion does a lot of damage in a game, the champion is likely to have more kills.

<iframe src="./assets/kills_vs_deaths.html" width=1200 height=600 frameBorder=0></iframe>
When we plot kills against death, and we see a negative correlation, meaning when a champion has a lot of kills, it's likely that it doesn't have a lot of deaths.

### Interesting Aggregates

We group by champions and analyze their performance by result, kills, pentakills, dmp, and damageshare statistics. We will mainly look at result column to calculate win rates for each champion. Note that champions are designed differently so that some are strong early in the game, some are strong late in the game, and some are in between. We will mark 30 minutes as our standard for dividing early and late games. We will use champions with more than 10 games played.

Theses are the top 5 highest win rate champions for games lasting less than 30 minutes (early game):

| champion   |   result |    kills |   pentakills |     dpm |   damageshare |
|:-----------|---------:|---------:|-------------:|--------:|--------------:|
| Kha'Zix    | 0.675676 | 4.97297  |    0         | 490.364 |     0.193384  |
| Darius     | 0.643678 | 5.54023  |    0.0229885 | 459.732 |     0.206364  |
| Talon      | 0.636364 | 4.36364  |    0         | 427.439 |     0.177384  |
| Sona       | 0.614035 | 0.807018 |    0         | 187.92  |     0.0815233 |
| Kled       | 0.6      | 3.24     |    0         | 441.795 |     0.188456  |


These are the top 5 highest win rate champions for games lasting longer than 30 minutes (late game):

| champion   |   result |   kills |   pentakills |     dpm |   damageshare |
|:-----------|---------:|--------:|-------------:|--------:|--------------:|
| Annie      | 0.75     | 5.5     |            0 | 709.525 |      0.251561 |
| Illaoi     | 0.7      | 3.4     |            0 | 755.309 |      0.266665 |
| Malzahar   | 0.636364 | 2.81818 |            0 | 496.422 |      0.245004 |
| Neeko      | 0.625    | 4.5625  |            0 | 602.367 |      0.259555 |
| Ekko       | 0.617647 | 4.23529 |            0 | 538.918 |      0.253847 |


Interestingly, there is no overlap among the top 5 champions for each game length. From this, we can see that different champions are strong for each game length.

---

## Assessment of Missingness

### NMAR Analysis

When we first looked at our data, there were a lot of missing values in a number of columns. After some EDA, we found that the statistics for banned champions was missing in all player rows but it was mostly present in team rows. Looking at the ban statistics data by league, most leagues are missing some ban data, but more popular and competitve leagues tend to have lower percentage of missing bans. Here, popular and competitive leagues refer to international competitions, where teams from each league has to qualify to play in by competing in their own leagues first, and leagues that frequently make it to Worlds. More specifically, I'm referring to Worlds, MSI, LCK, LPL, LCS, and LEC. These leagues tend to have lower missing data for bans, and it could be because they are more careful in collecting and saving league data for analysis purposes for their teams because of the popularity of League of Legends in their leagues.

If we could obtain data of the popularity of each leagues, then we could shift the NMAR missingness to MAR by providing a column that ban missingness depends on.


### Missingness Dependency

**Missingness of Bans on League**

Now we look at the result of the games and try to see the missingness of bans depend on league. We expect that there is some kind of dependency on league column as more popular leagues tend to do a better job including statistics.


| league     |    missing |   not missing |
|:-----------|-----------:|--------------:|
| CBLOL      | 0.0197983  |    0.00826446 |
| CBLOLA     | 0.0176991  |    0.00330579 |
| CDF        | 0.00596831 |    0.00165289 |
| CT         | 0.00214036 |    0          |
| DCup       | 0.00304589 |    0.132231   |
| DDH        | 0.017164   |    0.00165289 |
| EBL        | 0.0150237  |    0.00826446 |
| EL         | 0.0107018  |    0.0165289  |
| ESLOL      | 0.0195925  |    0.0132231  |
| EUM        | 0.0219387  |    0.00165289 |
| GL         | 0.0141593  |    0.00661157 |
| GLL        | 0.0162173  |    0.0198347  |
| HC         | 0.0130891  |    0.00991736 |
| HM         | 0.0124717  |    0.00495868 |
| IC         | 0.00609179 |    0.00330579 |
| LAS        | 0.0183988  |    0.0115702  |
| LCK        | 0.0381972  |    0.00991736 |
| LCKC       | 0.0321465  |    0.0115702  |
| LCL        | 0.00127598 |    0.00165289 |
| LCO        | 0.0168759  |    0.0231405  |
| LCS        | 0.025108   |    0.00330579 |
| LCSA       | 0.0440831  |    0.014876   |
| LDL        | 0.0663923  |    0.447934   |
| LEC        | 0.019963   |    0.00165289 |
| LFL        | 0.0202511  |    0.00330579 |
| LFL2       | 0.0197572  |    0.00330579 |
| LHE        | 0.0192221  |    0.031405   |
| LJL        | 0.0175756  |    0.00165289 |
| LJLA       | 0.00312822 |    0          |
| LLA        | 0.0153118  |    0.00330579 |
| LMF        | 0.0251904  |    0.0429752  |
| LPL        | 0.0641696  |    0.0214876  |
| LPLOL      | 0.0167936  |    0.0165289  |
| LVP SL     | 0.0201276  |    0.00165289 |
| MSI        | 0.00658572 |    0          |
| NEXO       | 0.0156411  |    0.00991736 |
| NLC        | 0.0309529  |    0.0231405  |
| PCS        | 0.0221856  |    0.00495868 |
| PGC        | 0.0456884  |    0.0231405  |
| PGN        | 0.0122247  |    0.00165289 |
| PRM        | 0.029965   |    0.00991736 |
| SL (LATAM) | 0.0133772  |    0.00826446 |
| TAL        | 0.0166701  |    0.00826446 |
| TCL        | 0.018193   |    0          |
| UL         | 0.0227619  |    0.00495868 |
| UPL        | 0.0337106  |    0.00826446 |
| VCS        | 0.0264252  |    0.00661157 |
| VL         | 0.01383    |    0.00661157 |
| WLDs       | 0.0127187  |    0.00165289 |

Now we plot this.

<iframe src="./assets/league_by_bans.html" width=1000 height=500 frameBorder=0></iframe>

<iframe src="./assets/tvd_bans_league.html" width=1000 height=500 frameBorder=0></iframe>

The observed statistic is 0.5567808905345624, and the resulting p-value is 0.0, so we reject the null and conclude that the missingness of bans is dependent on league.


**Missingness of Bans on Result**

Now we try to see if the missingness of bans depend on the result of the game. We hope there is no dependency because if the missingness of bans depended on the result, it might mean lost teams didn't record their statistics for the game--very unprofessional.

|   result |   missing |   not missing |
|---------:|----------:|--------------:|
|        0 |  0.499444 |       0.52562 |
|        1 |  0.500556 |       0.47438 |

We plot this.

<iframe src="./assets/result_by_bans.html" width=1000 height=500 frameBorder=0></iframe>

<iframe src="./assets/tvd_bans_result.html" width=1000 height=500 frameBorder=0></iframe>

The observed statistic is 0.026175504601667843, and the resulting p-value is 0.218, so we fail to reject the null and conclude that the missingness of bans is not dependent on the result of the game.


---

## Hypothesis Testing


---