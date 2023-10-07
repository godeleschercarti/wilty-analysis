# wilty-analysis
This is a Jupyter notebook and associated csvs used in analysis of the UK television show "Would I Lie to You?". 

## Files
**seasons.csv** is the raw data that details every single statement and the opposing candidates' verdict.
**winners.csv** details the winner of each episode.
**contestants.csv** details every contestant in every episode and their various statistics and awards.
**players.csv** details every player and their all-time statistics and awards.
**get_season.ipynb** takes in seasons.csv and winners.csv to create contestants.csv, players.csv, and various other graphs.

## Notes

* Only Home Truths and Quick-Fire Lies are measured in seasons.csv.
* There are three "dummy lies" in seasons.csv. These are Chris Tarrant in 06x01, Claude Littner in 11x08, and Deborah Frances-White in 15x09. These statements do not actually exist in the show - they were added because in the episode, their team did not have a single statement. This messes up the code I have and I was too lazy to fix it. I don't think this changes the numbers substantially. 
* Lee Mack actually has one less appearance than David Mitchell in the show as there is an episode where Greg Davies takes his place - I've just pretended that Davies _was_ Mack in this episode. Again, the code breaks if there's another captain, so I've just handwaved it away. This also means that Davies has one less appearance than reality.
* The Children in Need special isn't in seasons.csv. I just couldn't find it anywhere online to watch.

## Calculations

Offensive Rating (OR) and Defensive Rating (DR) are a straightforward percentage calculation, multiplied by 9 (to make it a cleaner number). If the contestant was never on defense (i.e, they did not make a statement) in the episode, they default to 3.6 DR.  Total Rating (TR) is the sum of OR and DR. 

There is an intermediate "Modified Offensive Rating" (MOR) and "Modified Defensive Rating" (MDR). MOR is 1 * OR if the contestant was on offense against more than 3 statements, 0.75 * OR if the contestant was on offense against 2 statements, or 0.5 * OR if they were on offense against less than 2 statements. 

MDR is 1 * DR if the contestant made more than 1 statement, and 0.8 * DR if the contestant made 1 or less. 

To determine the Relative Offensive Rating (ROR) and the Relative Defensive Rating (RDR), a Team Offensive Rating (TOR) and a Team Defensive Rating (TDR) are first calculated. This is the average of the minimum rating and the median rating. I've chosen the mean of the minimum and the median rather than just the median as there are only 3 contestants in a team - this essentially means the Team rating is only dependent on the weakest two contestants on the team. The ROR and RDR are just the MOR and MDR minus the TOR and TDR. 

### Award Calculations

Team MVP (TMVP) is given to the contestant on each team with the best TR. In case of a tie, this defaults to the first index (i.e, usually the captains). Episode MVP (EMVP) is given to the TMVP on the winning team as per winners.csv. In case of a tie, the TMVP with the greater TR is given the EMVP. 

Offensive Player of the Episode (OPOE) is given to the contestant on each episode with the best ROR. In case of a tie, this is given to the contestant with the best OR. In case of a further tie, this is given to the contestant with the best TR. ROR is first priority here instead of OR (unlike defense as you will see below), as offense is more of a team effort than defense. As such, contestants that break with their team and get it right are rewarded more than contestants that go with their teams and get lucky. 

Defensive Player of the Episode (DPOE) is given to the contestant on each episode with the best MDR. In case of a tie, this is given to the contestant with the best DR. MDR is used here to not punish contestants that have more than one statement in one episode. A contestant that has two statements and convinces 3/3 people in the first statement and 2/3 in the second statement has provided the team 2 points and should be considered more valuable than a contestant that only had one statement and convinced 3/3 people, so MDR is used. 

Offensive Player of the Season (OPOS) is given to the OPOE on each season with the best ROR. This means that the contestant with the best offensive performance relative to their team gets the award. Similarly, Defensive Player of the Season (DPOS) is given to the DPOE with the best RDR. 
