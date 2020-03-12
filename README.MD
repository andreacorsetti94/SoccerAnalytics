Data Pre-Processing operations.



In the file 'original_database.sqlite' you can find the data downloaded from this data-set:
https://www.kaggle.com/hugomathien/soccer

I copied this database, and created a new one without not useful data, called 'database.sqlite'.
In this database, I created 3 new tables: MatchEvent, LeagueTable and TeamDataPreMatch, with the objective of creating a table 
in which could be stored, for every match, the pre-match statistics for the home team and the away team.

Once created these 3 tables, the only thing left was to populate them.
So I wrote 5 Python notebooks to accomplish this task. These notebooks need to run in the following order:

1) db_utils.ipynb
2) match_events_extractor.ipynb
3) match_events_populator.ipynb
4) league_table_calculator.ipynb
5) pre-match_data_populator.ipynb

In 'db_utils.ipynb' I collected some basic methods to query and update data in the database.

With 'match_events_extractor.ipynb' I defined some methods to obtain match events from xml strings, for the home and the away teams. These strings are present in the 'Match' table.

In 'match_events_populator.ipynb' I iterated over the 'Match' table, and for each match I used the methods defined in 'match_events_extractor.ipynb' to obtain the events and to store them in the 'MatchEvent' table.

In 'league_table_calculator.ipynb' we iterate over each league and season and will store the league table at the end of each stage of the league. For every stage the table will be poplated with teams data obtained from stage 1 to the current stage of the league, such as: total points at home, totat points at away, average points at home, etc.

In 'pre-match_data_populator.ipynb', for every Match, there will be stored statistics for the home team and the away team *prior* to the match itself. These statistics include: total points obtained at home, away, the average goals scored or conceived and many more.


Now that for each match it is possible to have the pre-game statistics for both the home team and the away team, it's time to use
the bookmaker odds to create the ML models.
Firstly, I created a 'MatchOdds' table from the original 'Match' table, using the python notebook 'match_odds_populator.ipynb'.
In this table, for each match we can find the average and the max odds available for each market: home win, draw and away. 
Also, from the average odds offered by the bookies, we derived the 'average' real chance of each outcome occurring.

Now, let's take a step back.

The matches data-set has got the match events attributes, but this is not true for each of the 25000+ matches inside the 'Match' table.
If we want to train a model giving in input these attributes, we need to think of a solution.
We found two:
- 1) fill the unpopulated data with default values, representing an average or fair representation (i.e. if match has no 'yellow card' attribute
     we assign each team two yellow cards by default, since 2 yellow cards for each team is the average)

- 2) filter the dataset considering only matches that contain the real data for these attributes (about 14000 matches out of 25000+)

So, two solutions mean two different input datasets for our models, and so different outputs. It's going to be interesting to compare the results


