This project models a game of [spades](https://en.wikipedia.org/wiki/Spades_(card_game)), a four-player card game, in [DAML](https://en.wikipedia.org/wiki/Spades_(card_game)). We will use the following rules for non-partnership spades:

#### Setup

- Each player is dealt 13 cards from a shuffled standard deck.
- After seeing their cards, each player sets a bid between 0 and 13 for the number of tricks (see below) they expect to win.
- An order of play is decided upon and a player is chosen to lead the first round.

#### Gameplay

- In every round, the player who leads places a card face up on the table. Following the order of play, the remaining players each do the same. Each card placed after the led card must must be of the same suit as the led card (the 'led suit') unless a player does not have any cards of that suit, in which case they may place any card.
- This set of four cards is called a trick. A trick is won by the player who placed the highest spade, or if no spades were played, the highest card of the led suit.
- The player who wins a trick leads the next round.

#### Scoring

- At after the final trick has been won, each player is awarded points.
- If a player wins at least as many tricks as they bid, they receive 10 points for every trick they bid and one point for each trick they win over their bid. For example, if a player bid 6 but wins 8 tricks, they would receieve 62 points.
- If a player wins less tricks than they bid, they receive -10 points for every trick they bid. For example, if a player bid 6 but wins 5 tricks, they would receive -60 points.
- There is a special case for a bid of zero: If a player bids 0 and wins no tricks, they receive 100 points, but if they win any tricks they receieve -100 points.

This repeats until one player amasses a total of 250 points, in which case they win. If there is a tie, the game proceeds until one player has a higher number of points.