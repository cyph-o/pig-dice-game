# Pig Dice Game: manual testing

## Test session

| | |
|---|---|
| Date | 16 November 2026 |
| Browser | Google Chrome |
| Winning score in this version | 20 |
| Tests run | 15 of 16 (T16 still to run) |
| Passed | 13 |
| Failed | 2 (T13, T14) |
| Confirmed by Obinna in his own browser | <!-- Replace with Yes and the date once you have repeated the tests yourself --> |

Dice values were chosen for each step so that every test could be repeated exactly.

## Test cases

| ID | Test | Steps | Expected result | Actual result | Result |
|---|---|---|---|---|---|
| T1 | Starting state | Open `obi.html` | Both total scores are 0, both current scores are 0, the dice is hidden, Player 1 is highlighted | Totals 0 and 0, current scores 0 and 0, dice hidden, Player 1 highlighted | Pass |
| T2 | Roll shows the dice | Click **Roll dice** | A dice face from 1 to 6 appears | Dice appeared showing a 4 | Pass |
| T3 | Roll adds to current score | Roll a 4 as Player 1 | Player 1's current score goes up by 4 | Player 1's current score went from 0 to 4 | Pass |
| T4 | Rolls add up | Roll a 3 straight after the 4 | Current score is 7, not 43 | Current score showed 7 | Pass |
| T5 | Rolling a 1 loses the turn | Roll a 1 with a current score of 7 | Current score goes back to 0 and the highlight moves to Player 2 | Current score reset to 0 and Player 2 was highlighted | Pass |
| T6 | Rolling a 1 keeps the total | Player 2 holds 5 points, then on a later turn rolls a 3 followed by a 1 | Player 2's total stays at 5 | Player 2's total stayed at 5 | Pass |
| T7 | Hold banks the score | Roll a 6 as Player 1, then click **Hold** | Player 1's total becomes 6, current score goes back to 0, highlight moves to Player 2 | Total 6, current score 0, Player 2 highlighted | Pass |
| T8 | Hold with nothing to bank | Click **Hold** as Player 2 before rolling | Player 2's total is unchanged and the turn passes to Player 1 | Total stayed at 0 and Player 1 was highlighted | Pass |
| T9 | Winning the game | Hold until Player 1's total reaches 20 | Player 1's panel changes to the winner style | Player 1's total showed 20 and the winner style was applied | Pass |
| T10 | Display after a win | Look at both panels straight after the win in T9 | The winner is clearly marked | Player 1's panel turned black with the winner styling, clearly showing who had won (confirmed on screen with a 25 to 10 result) | Pass |
| T11 | Roll after a win | Click **Roll dice** after the win | Nothing changes | Scores, dice and highlights all unchanged | Pass |
| T12 | Hold after a win | Click **Hold** after the win | Nothing changes | Scores and highlights all unchanged | Pass |
| T13 | New game after a win | Click **New game** after the win | Game returns to the starting state in T1: all scores 0, winner style removed, dice hidden, Player 1 highlighted | Scores reset to 0, winner style removed and Player 1 highlighted, but the dice from the last roll was still showing | Fail |
| T14 | New game mid-turn | As Player 2, roll a 3 and a 4, then click **New game**, then roll a 5 | Game returns to the starting state in T1 and the 5 counts for Player 1 | Scores reset, Player 1 highlighted and the 5 counted for Player 1, but the dice from the last roll was still showing after **New game** | Fail |
| T15 | Play again after New game | After **New game**, play a full game through to a win | Rolling, holding and winning all work as in the first game | Player 1 reached 20 and won, and the buttons stopped responding as in the first game | Pass |
| T16 | Narrow screen | Resize the browser window to phone width | The scores, dice and buttons are all still visible and usable | <!-- Run this one yourself and record what you see --> | |

## Defects found

| Test | Defect | Suggested fix |
|---|---|---|
| T13, T14 | **New game** resets the scores and highlights but does not hide the dice | Hide the dice as part of the **New game** reset |

<!-- Once you log this as a GitHub Issue, add the issue number here. -->

## Observations

- After a win, the code still passes the turn to the other player. This does not affect the result or the winner display, but the losing player is marked as active in a finished game. Wrapping the final `switchPlayer()` call in an `else` would tidy this up.

## Bugs I fixed while building

### New game did not clear the current session

- **What went wrong:** after clicking **New game**, the current score numbers from the game just played did not go away, and the page did not go back to how it looked at the start.
- **How I fixed it:** I reset everything by hand inside the **New game** button's click handler. It sets the four score displays back to 0, uses `classList` to remove the winner style from both players and put the active highlight back on Player 1, and resets the variables behind the game: the `scores` array, `currentScore`, `activePlayer` and `Playing`.
- **What I learned:** the page and the variables behind it are two separate things. Clearing the numbers on screen is not enough if the variables still hold the old game, and resetting the variables is not enough if the page still shows the old one.
