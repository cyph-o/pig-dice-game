# Pig Dice Game: manual testing

I tested my Pig Dice Game by hand in the browser against the test cases below. For each one I followed the steps, wrote down what happened and compared it with what I expected.

## Test session

| | |
|---|---|
| Tested by | Obinna Ejiofor |
| Date | 16 November 2026 |
| Browser | Google Chrome |
| Winning score in this version | 20 |
| Tests run | 15 of 16 |
| Passed | 13 |
| Failed | 2 (T13, T14) |

## Test cases

| ID | What I tested | Steps | What I expected | What happened | Result |
|---|---|---|---|---|---|
| T1 | Starting state | Open `obi.html` | Both total scores are 0, both current scores are 0, the dice is hidden and Player 1 is highlighted | Both totals and both current scores showed 0, the dice was hidden and Player 1 was highlighted | Pass |
| T2 | Roll shows the dice | Click **Roll dice** | A dice face from 1 to 6 appears | The dice appeared and showed the number I rolled | Pass |
| T3 | Roll adds to current score | As Player 1, roll a number other than 1 | Player 1's current score goes up by the number on the dice | My current score went up by the number I rolled | Pass |
| T4 | Rolls add up | Roll a second number other than 1 straight after | The two rolls are added together (for example 4 then 3 gives 7, not 43) | The current score showed the two rolls added together | Pass |
| T5 | Rolling a 1 loses the turn | Keep rolling until a 1 appears | The current score goes back to 0 and the highlight moves to the other player | The current score reset to 0 and the other player was highlighted | Pass |
| T6 | Rolling a 1 keeps the total | Hold some points, then on a later turn roll a 1 | The total I had already held stays the same | My held total did not change | Pass |
| T7 | Hold banks the score | Roll a number other than 1, then click **Hold** | The current score is added to my total, the current score goes back to 0 and the highlight moves to the other player | My total went up by the current score, the current score reset to 0 and the other player was highlighted | Pass |
| T8 | Hold with nothing to bank | Click **Hold** at the start of a turn, before rolling | The total is unchanged and the turn passes to the other player | The total stayed the same and the turn passed | Pass |
| T9 | Winning the game | Hold until one player's total reaches 20 or more | That player's panel changes to the winner style | The winning player's panel changed to the winner style | Pass |
| T10 | Display after a win | Look at both panels straight after a win | The winner is clearly marked | Player 1's panel turned black, clearly showing who had won. I confirmed this with a 25 to 10 result | Pass |
| T11 | Roll after a win | Click **Roll dice** after a win | Nothing changes | The scores, dice and highlights did not change | Pass |
| T12 | Hold after a win | Click **Hold** after a win | Nothing changes | The scores and highlights did not change | Pass |
| T13 | New game after a win | Click **New game** after a win | The game goes back to the starting state in T1: all scores 0, winner style removed, dice hidden and Player 1 highlighted | The scores reset to 0, the winner style was removed and Player 1 was highlighted, but the dice from my last roll was still showing | Fail |
| T14 | New game mid-turn | As Player 2, roll two or three times, click **New game**, then roll again | The game goes back to the starting state in T1 and the next roll counts for Player 1 | The scores reset, Player 1 was highlighted and the next roll counted for Player 1, but the dice from my last roll was still showing after **New game** | Fail |
| T15 | Play again after New game | After **New game**, play a full game through to a win | Rolling, holding and winning all work as in the first game | I played a second game through to a win and everything worked as in the first game | Pass |
| T16 | Narrow screen | Resize the browser window to phone width | The scores, dice and buttons are all still visible and usable | Not yet tested | |

## Defect I found

| Test | Defect | Fix |
|---|---|---|
| T13, T14 | **New game** resets the scores and highlights but does not hide the dice | Hide the dice as part of the **New game** reset |

## A bug I fixed while building

### New game did not clear the current session

- **What went wrong:** when I clicked **New game**, the current score numbers from the game I had just played did not go away, and the page did not go back to how it looked at the start.
- **How I fixed it:** I reset everything by hand inside the **New game** button's click handler. I set the four score displays back to 0, used `classList` to remove the winner style from both players and put the active highlight back on Player 1, and reset the variables behind the game: the `scores` array, `currentScore`, `activePlayer` and `Playing`.
- **What I learned:** the page and the variables behind it are two separate things. Clearing the numbers on screen is not enough if the variables still hold the old game, and resetting the variables is not enough if the page still shows the old one.
