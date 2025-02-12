---
observations: "Discrete"
title: "Leduc Hold'em"
agents: "2"
manual-control: "No"
action-shape: "Discrete(4)"
action-values: "Discrete(4)"
observation-shape: "(36,)"
observation-values: "[0, 1]"
num-states: "10^2"
import: "from pettingzoo.classic import leduc_holdem_v3"
agent-labels: "agents= ['player_0', 'player_1']"
---

{% include info_box.md %}



Leduc Hold'em is a variation of Limit Texas Hold'em with 2 players, 2 rounds and a deck of six cards (Jack, Queen, and King in 2 suits). At the beginning of the game, each player receives one card and, after betting, one public card is revealed. Another round follow. At the end, the player with the best hand wins and receives a reward (+1) and the loser receives -1. At any time, any player can fold.   

Our implementation wraps [RLCard](http://rlcard.org/games.html#leduc-hold-em) and you can refer to its documentation for additional details. Please cite their work if you use this game in research.


### Observation Space

The observation is a dictionary which contains an `'obs'` element which is the usual RL observation described below, and an  `'action_mask'` which holds the legal moves, described in the Legal Actions Mask section.

As described by [RLCard](https://github.com/datamllab/rlcard/blob/master/docs/games#leduc-holdem), the first 3 entries of the main observation space correspond to the player's hand (J, Q, and K) and the next 3 represent the public cards. Indexes 6 to 19 and 20 to 33 encode the number of chips by the current player and the opponent, respectively.

|  Index  | Description                                                                  |
|:-------:|------------------------------------------------------------------------------|
|  0 - 2  | Current Player's Hand<br>_`0`: J, `1`: Q, `2`: K_                            |
|  3 - 5  | Community Cards<br>_`3`: J, `4`: Q, `5`: K_                                  |
|  6 - 20 | Current Player's Chips<br>_`6`: 0 chips, `7`: 1 chip, ..., `20`: 14 chips_   |
| 21 - 35 | Opponent's Chips<br>_`21`: 0 chips, `22`: 1 chip, ..., `35`: 14 chips_       |


#### Legal Actions Mask

The legal moves available to the current agent are found in the `action_mask` element of the dictionary observation. The `action_mask` is a binary vector where each index of the vector represents whether the action is legal or not. The `action_mask` will be all zeros for any agent except the one whos turn it is. Taking an illegal move ends the game with a reward of -1 for the illegally moving agent and a reward of 0 for all other agents.

### Action Space

| Action ID | Action |
|:---------:|--------|
|     0     | Call   |
|     1     | Raise  |
|     2     | Fold   |
|     3     | Check  |

### Rewards

|      Winner       |       Loser       |
| :---------------: | :---------------: |
| +raised chips / 2 | -raised chips / 2 |
