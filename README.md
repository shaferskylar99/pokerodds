# Poker Odds Based on Hand

Introduction

Honestly, this project is pretty self explanatory. I think poker and the associated odds are pretty interesting, and so, given the amount of players and your cards (as well as the cards, if any, in the middle), the code generates every possible outcome of the rest of the cards in the middle as well as the possible hands of the opposing players, and determines in which percentage of outcomes you win.

Project Limitations:

I know you can do Monte Carlo Simulations or other systems like that, but I wanted to generate out every single hand because I thought it was interesting. 

Flop needed, otherwise computationally it gets too vast to where it is no longer effective

Still speeding up code, it takes like 90 seconds to run at stage 1, 2 at stage 2, less than a second at stage 3.

Interesting Model Note: To reduce computation size I played against one person 5 times rather than playing five people, this may adjust odds slightly but I considered the effect. 
This was necessary for computational speed and I believe the effect would be marginal on probability.
