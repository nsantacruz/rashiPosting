rashiPosting
============

takes parsed rashi and tosafos commentary on gemara and posts it to the corresponding lines of gemara on sefaria.org
- currently matches ~80% of commentary precisely. 
- ~15% of commentaries match multiple lines and are considered ambiguous. These are logged and need to be reviewed by a human
- ~5% are not matched at all due to incongruencies in the text. These are also logged for review.
#problems encountered:


1. rashis aren't necessarily in the exact order of the gemara. you can't rely on a simple linear search to match phrase that would otherwise match multiple lines of gemara.

2. rashi's version of the gemara often doesn't exactly match the gemara we have. also, there is no definitive spelling for Aramaic. vavs and yuds can be added or subtracted to words to add vowelization.

3. either rashi or the gemara will use an abbreviation, while the other won't

#solutions

1. although rashis are not necessarily in order, they are generally in the right place, give or take 1 or 2 positions. Therefore, my strategy was:
  - search through the dibur hamaschils (dhm) linearly and find a single line of gemara that seems to match
  - continue with next dhm, starting from beginning of amud
  - for next dhm, start from where first dhm was found and continue AT LEAST until second was found, if not further
  - in parallel with this search, relook at first dhm and see if any lines in this range match, if they do, add them to possible matching lines list
  - repeat. for the last two dhms search until the end of the amud just in case, because you will never go back to check them

2. use a library called fuzzywuzzy, plus a little logic to determine when two strings are a close enough match to consider it significant
  - basically, you are trying to match one string with another. compare each word of one with the other and see if there are any strings that are at least a textCutoff% match
  - count up all the words that meet this criterion. if the percentage of matching words is above stringCutoff%, the lines are considered matching.
  - associate a score with each match which says how likely you think it is an actual match
  - at the end of each amud, sort the scores and weed out the ones that are below scoreCutoff (this cutoff relative to the top score for that dhm)
  - if there's only one matching line left, congrats! you (hopefully) matched rashi to gemara!

3. modify the text before any checking is done
  - get rid of unwanted words
  - convert abbreviations to full words
