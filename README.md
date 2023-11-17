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




| gameid                | date_x              |   game_x |   patch_x | side   | position_x   | playername_x   | teamname_x               | champion_x   | ban1_x   | ban2_x   | ban3_x   | ban4_x   | ban5_x   |   gamelength_x |   result_x |   kills_x |   deaths_x |   assists_x |   teamkills_x |   teamdeaths_x |   doublekills_x |   triplekills_x |   quadrakills_x |   pentakills_x | firstblood_x   | firstbloodkill_x   | firstbloodassist_x   | firstbloodvictim_x   |   team kpm_x |   ckpm_x |   barons_x |   inhibitors_x |   damagetochampions_x |   dpm_x |   damageshare_x |   damagetakenperminute_x |   damagemitigatedperminute_x |   wardsplaced_x |   wpm_x |   wardskilled_x |   wcpm_x |   controlwardsbought_x |   visionscore_x |   vspm_x |   totalgold_x |   earnedgold_x |   earned gpm_x |   earnedgoldshare_x |   goldspent_x |   total cs_x |   minionkills_x |   monsterkills_x |   cspm_x |   goldat10_x |   xpat10_x |   csat10_x |   golddiffat10_x |   xpdiffat10_x |   csdiffat10_x |   killsat10_x |   assistsat10_x |   deathsat10_x |   goldat15_x |   xpat15_x |   csat15_x |   golddiffat15_x |   xpdiffat15_x |   csdiffat15_x |   killsat15_x |   assistsat15_x |   deathsat15_x |   date |   game |   patch |   position |   playername |   teamname |   champion |   ban1 |   ban2 |   ban3 |   ban4 |   ban5 |   gamelength |   result |   kills |   deaths |   assists |   teamkills |   teamdeaths |   doublekills |   triplekills |   quadrakills |   pentakills |   firstblood |   firstbloodkill |   firstbloodassist |   firstbloodvictim |   team kpm |   ckpm |   barons |   inhibitors |   damagetochampions |   dpm |   damageshare |   damagetakenperminute |   damagemitigatedperminute |   wardsplaced |   wpm |   wardskilled |   wcpm |   controlwardsbought |   visionscore |   vspm |   totalgold |   earnedgold |   earned gpm |   earnedgoldshare |   goldspent |   total cs |   minionkills |   monsterkills |   cspm |   goldat10 |   xpat10 |   csat10 |   golddiffat10 |   xpdiffat10 |   csdiffat10 |   killsat10 |   assistsat10 |   deathsat10 |   goldat15 |   xpat15 |   csat15 |   golddiffat15 |   xpdiffat15 |   csdiffat15 |   killsat15 |   assistsat15 |   deathsat15 |   firstdragon |   dragons |   elementaldrakes |   infernals |   mountains |   clouds |   oceans |   chemtechs |   hextechs |   elders |   firstherald |   heralds |   firstbaron |   firsttower |   towers |   firstmidtower |   firsttothreetowers |   turretplates |
|:----------------------|:--------------------|---------:|----------:|:-------|:-------------|:---------------|:-------------------------|:-------------|:---------|:---------|:---------|:---------|:---------|---------------:|-----------:|----------:|-----------:|------------:|--------------:|---------------:|----------------:|----------------:|----------------:|---------------:|:---------------|:-------------------|:---------------------|:---------------------|-------------:|---------:|-----------:|---------------:|----------------------:|--------:|----------------:|-------------------------:|-----------------------------:|----------------:|--------:|----------------:|---------:|-----------------------:|----------------:|---------:|--------------:|---------------:|---------------:|--------------------:|--------------:|-------------:|----------------:|-----------------:|---------:|-------------:|-----------:|-----------:|-----------------:|---------------:|---------------:|--------------:|----------------:|---------------:|-------------:|-----------:|-----------:|-----------------:|---------------:|---------------:|--------------:|----------------:|---------------:|-------:|-------:|--------:|-----------:|-------------:|-----------:|-----------:|-------:|-------:|-------:|-------:|-------:|-------------:|---------:|--------:|---------:|----------:|------------:|-------------:|--------------:|--------------:|--------------:|-------------:|-------------:|-----------------:|-------------------:|-------------------:|-----------:|-------:|---------:|-------------:|--------------------:|------:|--------------:|-----------------------:|---------------------------:|--------------:|------:|--------------:|-------:|---------------------:|--------------:|-------:|------------:|-------------:|-------------:|------------------:|------------:|-----------:|--------------:|---------------:|-------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|--------------:|----------:|------------------:|------------:|------------:|---------:|---------:|------------:|-----------:|---------:|--------------:|----------:|-------------:|-------------:|---------:|----------------:|---------------------:|---------------:|
| ESPORTSTMNT01_2690210 | 2022-01-10 07:44:08 |        1 |     12.01 | Blue   | top          | Soboro         | Fredit BRION Challengers | Renekton     | Karma    | Caitlyn  | Syndra   | Thresh   | Lulu     |           1713 |          0 |         2 |          3 |           2 |             9 |             19 |               0 |               0 |               0 |              0 | False          | False              | False                | False                |       0.3152 |   0.9807 |          0 |              0 |                 15768 | 552.294 |       0.278784  |                 1072.4   |                      777.793 |               8 |  0.2802 |               6 |   0.2102 |                      5 |              26 |   0.9107 |         10934 |           7164 |        250.928 |            0.253859 |         10275 |          231 |             220 |               11 |   8.0911 |         3228 |       4909 |         89 |               52 |            -44 |              8 |             0 |               0 |              0 |         5025 |       7560 |        135 |              391 |            345 |             14 |             0 |               1 |              0 |    nan |    nan |     nan |        nan |          nan |        nan |        nan |    nan |    nan |    nan |    nan |    nan |          nan |      nan |     nan |      nan |       nan |         nan |          nan |           nan |           nan |           nan |          nan |          nan |              nan |                nan |                nan |        nan |    nan |      nan |          nan |                 nan |   nan |           nan |                    nan |                        nan |           nan |   nan |           nan |    nan |                  nan |           nan |    nan |         nan |          nan |          nan |               nan |         nan |        nan |           nan |            nan |    nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |           nan |       nan |               nan |         nan |         nan |      nan |      nan |         nan |        nan |      nan |           nan |       nan |          nan |          nan |      nan |             nan |                  nan |            nan |
| ESPORTSTMNT01_2690210 | 2022-01-10 07:44:08 |        1 |     12.01 | Blue   | jng          | Raptor         | Fredit BRION Challengers | Xin Zhao     | Karma    | Caitlyn  | Syndra   | Thresh   | Lulu     |           1713 |          0 |         2 |          5 |           6 |             9 |             19 |               0 |               0 |               0 |              0 | True           | False              | True                 | False                |       0.3152 |   0.9807 |          0 |              0 |                 11765 | 412.084 |       0.208009  |                  944.273 |                      650.158 |               6 |  0.2102 |              18 |   0.6305 |                      6 |              48 |   1.6813 |          9138 |           5368 |        188.021 |            0.19022  |          8750 |          148 |              33 |              115 |   5.1839 |         3429 |       3484 |         58 |              485 |            432 |             -5 |             1 |               2 |              0 |         5366 |       5320 |         89 |              541 |           -275 |            -11 |             2 |               3 |              2 |    nan |    nan |     nan |        nan |          nan |        nan |        nan |    nan |    nan |    nan |    nan |    nan |          nan |      nan |     nan |      nan |       nan |         nan |          nan |           nan |           nan |           nan |          nan |          nan |              nan |                nan |                nan |        nan |    nan |      nan |          nan |                 nan |   nan |           nan |                    nan |                        nan |           nan |   nan |           nan |    nan |                  nan |           nan |    nan |         nan |          nan |          nan |               nan |         nan |        nan |           nan |            nan |    nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |           nan |       nan |               nan |         nan |         nan |      nan |      nan |         nan |        nan |      nan |           nan |       nan |          nan |          nan |      nan |             nan |                  nan |            nan |
| ESPORTSTMNT01_2690210 | 2022-01-10 07:44:08 |        1 |     12.01 | Blue   | mid          | Feisty         | Fredit BRION Challengers | LeBlanc      | Karma    | Caitlyn  | Syndra   | Thresh   | Lulu     |           1713 |          0 |         2 |          2 |           3 |             9 |             19 |               0 |               0 |               0 |              0 | False          | False              | False                | False                |       0.3152 |   0.9807 |          0 |              0 |                 14258 | 499.405 |       0.252086  |                  581.646 |                      227.776 |              19 |  0.6655 |               7 |   0.2452 |                      7 |              29 |   1.0158 |          9715 |           5945 |        208.231 |            0.210665 |          8725 |          193 |             177 |               16 |   6.7601 |         3283 |       4556 |         81 |              162 |             71 |              0 |             0 |               1 |              0 |         5118 |       6942 |        120 |             -475 |            153 |              1 |             0 |               3 |              0 |    nan |    nan |     nan |        nan |          nan |        nan |        nan |    nan |    nan |    nan |    nan |    nan |          nan |      nan |     nan |      nan |       nan |         nan |          nan |           nan |           nan |           nan |          nan |          nan |              nan |                nan |                nan |        nan |    nan |      nan |          nan |                 nan |   nan |           nan |                    nan |                        nan |           nan |   nan |           nan |    nan |                  nan |           nan |    nan |         nan |          nan |          nan |               nan |         nan |        nan |           nan |            nan |    nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |           nan |       nan |               nan |         nan |         nan |      nan |      nan |         nan |        nan |      nan |           nan |       nan |          nan |          nan |      nan |             nan |                  nan |            nan |
| ESPORTSTMNT01_2690210 | 2022-01-10 07:44:08 |        1 |     12.01 | Blue   | bot          | Gamin          | Fredit BRION Challengers | Samira       | Karma    | Caitlyn  | Syndra   | Thresh   | Lulu     |           1713 |          0 |         2 |          4 |           2 |             9 |             19 |               0 |               0 |               0 |              0 | True           | False              | True                 | False                |       0.3152 |   0.9807 |          0 |              0 |                 11106 | 389.002 |       0.196358  |                  463.853 |                      218.879 |              12 |  0.4203 |               6 |   0.2102 |                      4 |              25 |   0.8757 |         10605 |           6835 |        239.405 |            0.242201 |         10425 |          226 |             208 |               18 |   7.9159 |         3600 |       3103 |         78 |              296 |            265 |            -12 |             1 |               1 |              0 |         5461 |       4591 |        115 |             -793 |          -1343 |            -34 |             2 |               1 |              2 |    nan |    nan |     nan |        nan |          nan |        nan |        nan |    nan |    nan |    nan |    nan |    nan |          nan |      nan |     nan |      nan |       nan |         nan |          nan |           nan |           nan |           nan |          nan |          nan |              nan |                nan |                nan |        nan |    nan |      nan |          nan |                 nan |   nan |           nan |                    nan |                        nan |           nan |   nan |           nan |    nan |                  nan |           nan |    nan |         nan |          nan |          nan |               nan |         nan |        nan |           nan |            nan |    nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |           nan |       nan |               nan |         nan |         nan |      nan |      nan |         nan |        nan |      nan |           nan |       nan |          nan |          nan |      nan |             nan |                  nan |            nan |
| ESPORTSTMNT01_2690210 | 2022-01-10 07:44:08 |        1 |     12.01 | Blue   | sup          | Loopy          | Fredit BRION Challengers | Leona        | Karma    | Caitlyn  | Syndra   | Thresh   | Lulu     |           1713 |          0 |         1 |          5 |           6 |             9 |             19 |               0 |               0 |               0 |              0 | True           | True               | False                | False                |       0.3152 |   0.9807 |          0 |              0 |                  3663 | 128.301 |       0.0647631 |                  475.026 |                      490.123 |              29 |  1.0158 |              14 |   0.4904 |                     11 |              69 |   2.4168 |          6678 |           2908 |        101.856 |            0.103054 |          6395 |           42 |              42 |                0 |   1.4711 |         2678 |       2161 |         16 |              528 |           -587 |              1 |             1 |               1 |              0 |         3836 |       3588 |         28 |              443 |           -497 |              7 |             1 |               2 |              2 |    nan |    nan |     nan |        nan |          nan |        nan |        nan |    nan |    nan |    nan |    nan |    nan |          nan |      nan |     nan |      nan |       nan |         nan |          nan |           nan |           nan |           nan |          nan |          nan |              nan |                nan |                nan |        nan |    nan |      nan |          nan |                 nan |   nan |           nan |                    nan |                        nan |           nan |   nan |           nan |    nan |                  nan |           nan |    nan |         nan |          nan |          nan |               nan |         nan |        nan |           nan |            nan |    nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |        nan |      nan |      nan |            nan |          nan |          nan |         nan |           nan |          nan |           nan |       nan |               nan |         nan |         nan |      nan |      nan |         nan |        nan |      nan |           nan |       nan |          nan |          nan |      nan |             nan |                  nan |            nan |


### Univariate Analysis

<iframe src="./assets/all_winrates.html" width=800 height=600 frameBorder=0></iframe>

---

## Assessment of Missingness


---

## Hypothesis Testing


---