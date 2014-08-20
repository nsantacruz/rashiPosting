rashiPosting
============

After inputting a mesechta and commentary name, will post that commentary to sefaria.org


this file takes parsed rashi and tosafos commentary on gemara and posts it to the corresponding lines of gemara on sefaria.org
problems encountered:
1-rashis aren't necessarily in the exact order of the gemara
2- rashi's version of the gemara often doesn't exactly match the gemara we have
3- either rashi or the gemara will use an abbreviation, while the other won't

solutions
1- although rashis are not necessarily in order, they are generally in the right place, give or take 1 or 2 rashis:
        #search through the dhms linearly and find single line of gemara that seems to match
        #continue with next dhm, starting from beginning of amud
        #for next dhm, start from where first dhm was found and continue AT LEAST until second was found, if not further
        #in parallel with this search, relook at first dhm and see if any lines in this range match, if they do, add them to possible matching lines list
        #repeat. last two dhms search until the end of the amud just in case, because you will never go back to check them
2- use a library called fuzzywuzzy, plus a little logic to determine when two strings are a close enough match to consider it significant
        #basically, you are trying to match one string with another. compare each word of one with the other and see if there are any strings that are at least a textCutoff% match
        #count up all the words that meet this criterion. if the percentage of matching words is above stringCutoff%, the lines are considered matching.
        #associate a score with each match which says how likely you think it is an actual match
        #at the end of each amud, sort the scores and weed out the ones that are below scoreCutoff (this is cutoff relative to the top score for that dhm)
        #if there's only one matching line left, congrats! you (hopefully) matched rashi to gemara!
3- modify the text before any checking is done
        #get rid of unwantedWords
        #convert abbreviations to full words
