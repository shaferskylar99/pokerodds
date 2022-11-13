# Texas Hold'Em Odds - Modified Raw Sim

## Introduction/Project Summary

Honestly, this project is pretty self explanatory. I think poker and the associated odds are pretty interesting, and so, given the amount of players and your cards (as well as the cards visible in the middle), I wanted to determine the odds of winning a game of poker with a certain hand. The code generates every possible outcome of the rest of the cards in the middle as well as the possible hands of the opposing players, and determines in which percentage of outcomes you win, to give your probability of victory.

In a move away from a few of my other projects, I didn't use popular machine learing tactics on this one. I decided that rather than teaching a machine how to play poker, instead, it was easier (with a few practical tricks) to generate every possible set of hands at the table, and in each instance, see who would win, and then take the win percentage of the player against all others out of all of those potential scenarios as the probability of winning the hand.

You input the amount of players in, as well as your cards, and the three cards on the flop, and it'll tell you your active probability. Then, it will prompt you for the 4th card shown once it appears in the game, and show you your new odds again, and then do so again after the 5th card is revealed in the middle.

The code in the first step takes around 2 minutes or less (sometimes around 40 seconds) to run because the scope of possibilities is so large, then a couple seconds in the next step, and under a second for the third input. I understand this speed isn't necessarily ideal for use in a game, but that would likely be cheating anyway, so I spent time speeding up the code enough for it to be useful as a learning tool, but I didn't deem it necessary to speed it up beyond that (you can do so if you would like, obviously). Enjoy!

IMPORTANT NOTE ON INPUT: WHEN INPUTTING CARDS, INPUT 2 AS 0, 10 AS AN 8, QUEEN AS A 10, ACE IS 12, AND SO ON. SO IT IS A SCALE WHICH STARTS AT 0. FOR SUITS, IT IS ARBITRARY, BUT I FIND THE FOLLOWING ORDERING HELPS: ['diamond','clubs','hearts','spades'] = [0,1,2,3] SO THE EIGHT OF CLUBS WOULD BE INPUT OF 6 FOR THE COUNT, THEN 1 FOR THE SUIT.

## Project Tricks/Advantages/Limitations:

One thing that helped me in this project was being careful about how I defined the game. Poker is a game of 52 cards, and so if you know the 2 cards in your hand, and the 3 in the flop, there are 47 cards left that be the next possible card flipped, 46 for the 5th card flipped in the center, 45 for the first card in the opponent's hand and so on. This seems computationally small at first, but it gets incredibly large.

Say you need to simulate 2 more cards (with 47 remaining at the start): There are 47*46 = 2,162 possible hands. 

If you need 2 more cards in the middle and cards for one other player(with 47 again, so basically playing one other person): 4,280,760 possible hands.

If there are now only 3 other players each needing two cards, and you still need the last two in the center: 12,678,926,198,400 possible hands.

That is an insane number! Due to the exponential nature of the problem, and the fact that I only had my cheap laptop available to run this code (and I couldn't wait for thousands of years for this code to run, which I vaguely determined it would have likely taken), I had to be smart about how I designed the game and how I ran the code.

First of all, I partitioned the game. Rather than treating the game as though I was playing all players at once, I subdivided the game into individual matchups against each player at the table. If I won each of these individual matchups, I won the game! So, rather than having to generate the hands of 3 other people every time (or more, as poker games often have more players than that), I could just generate every hand the other person could have in each 1v1 matchup, and predict the probability of me beating one person, and then take the probability that I beat that person, and also the next person, and so on. For example, if my win probability was .5 against each person and I was playing two people, then my overall win probability would be .25 as I'd have to beat both, and the probability of both of those events occuring is .5*.5=.25. 

To be clear, I understand this is not a perfect system. The cards of one person impact the next, as cards can't be repeated. So, if say one person failed to beat me, than that losing hand (and all possible hands with either of those 2 cards in it) is now discarded from the set of possible hands, so the next may be slightly more likely to beat me. Given the large scope of this problem though, I think in order to make my predictions practical in terms of time it took the program to run, this was far and away the best system, and I think my probabilities are only marginally off, but obviously, it isn't perfect. I do think it is a practical solution, and I'm proud of how it turned out. It massively improved scalability, making the amount of people in the game marginal to the code runtime. This is the way in which I felt I could get the closest odds in the best timeframe, and reduces the problem from years if there are even a few other players to 2 minutes or less, sometimes even roughly 40 seconds. 

Another strategy I took is to reduce the overall cardset by understanding that some cards are interchangeable. In our system in which we only play against one player and expand from there, the 4th and 5th cards in the center, if only the first 3 are shown, are interchangeable. They both work with your hand and the hand of your opponent equally, and so their order of prediction doesn't matter. So, if you get the 7 of clubs and the 10 of diamonds as the 4 and 5, they could also be presented as the 5 and 4, respectively, it would not change your odds of winning the hand at all (it could definitely affect betting scenarios, but that is not what this program is concerned with, I may add that capability later, if I feel up for it). So, these two scenarios, in which the same cards come in opposite spots, can be treated as identical. So, we can remove one of the two duplicates, as the order doesn't matter, so in this situation we can remove the 5 and 4, and this same thing can also be applied to every set of 2 cards in the 4 and 5 spot, reducing the total hands by half. The same can be said for the two cards in the hand of your opponent, as their order doesn't matter either, and so we can again remove one of the two duplicates. Applying the same concept to both of these instances reduces the number of hands to 25% of what it was previously, as each reduces the amount of hands at a rate of %50, and again .5*.5 = .25. This helped in reducing code speed.

Two other intricacies of my system are how it treats pot splits, and that it only has 3 comparitive tiebreakers. When comparing two hands, it compares first what pattern they have (full house, flush, straight, etc.), and then it can compare the next two tie-breaking high cards in each hand. After these first two cards, if they remain the same, the hand is assumed to be a tie. Again, this was for speed and efficiency, as at a certain point, the likelihood of a continuing tie becomes marginal. If a tie does occur in a simulation or is assumed to have occurred after checking the hand strength and then the first high card and the second in each hand, the pot splits, and so rather than returning a 1.0 corresponding to a win, it returns .5 if there is a 2-way tie, .333 if there is a 3 way tie, and so on. So, my system focuses on pot share. The winning probability returned is actually the expected value of your return, as a share of the pot. If you have a .4 winning probability, you on average will win 40% of the pot, or to describe it another way, that is the mean or average proportional return of the pot your hand will provide.

Another limitation is that it needs the first 3 cards in the middle (the flop), along with your hand. I felt I had to make it this way, otherwise timewise it would no longer be effective to simulate every hand, and I didn't have the time with school to deepdive any more into code speed.

## Conclusion

In conclusion, this project, I think, was a good example of a practical project exploring data and its possibilities. I had to make certain assumptions and practical systems that deviated from natural law in order to make my system efficient enough to be usable and useful. I know you can do Monte Carlo Simulations or other systems like that, but I wanted to generate out every single hand (practically, with the limitations above) because I thought it was an interesting task to undertake. This was a relatively quick side project, but I thought it was interesting in its outcomes, and I'm proud of how I found interesting solutions to combat a problem of such a large scale and explore it while still providing utility. I hope you enjoyed!
