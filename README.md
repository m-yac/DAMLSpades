This project models a game of [spades](https://en.wikipedia.org/wiki/Spades_(card_game)), a four-player card game, in [DAML](https://en.wikipedia.org/wiki/Spades_(card_game)). The particular ruleset used is detailed in the rules section below.

In order to ensure the privacy of each player's hand, I included a dealer party along with the four players. The dealer cannot influence the game, but in order for the game to proceed must be trusted to actively participate as outlined in the workflow below. Ideally such a fifth party would not be needed, but I don't know how to remove them quite yet...

## Workflow

1. A dealer or player creates a `GameProposal`, and can invite a dealer or other players through the invite/accept pattern.
2. Once one dealer and four players have joined, any party (i.e. the dealer or any player) can exercise the choice `StartGame`, which creates a `Bidding` contract, signed by all parties, and a `Deal` contract, signed by only the dealer.
3. The dealer exercises the `DoDeal` choice on their `Deal` contract, which creates a `Hand` contract between each player and the dealer. *Each `Hand` is only visible to the dealer and the player to which it belongs*, ensuring that no player can see another's hand.
4. Each player then must exercise the `AddBid` choice, and once all bids have been added, the dealer exercises the `StartRound` choice to begin the game and create a `Spades` contract.
5. When it is their turn, a player exercises the `MakePlay` choice in their `Hand` contract, which create a `Play` contract (containing the card played) and a new `Hand` contract (with has the appropriate card removed), assuming their play is valid.
   - Importantly, the dealer must then exercise the `MakeNextPlay` choice in the `Spades` contract with the aforementioned `Play` contract in order to have this play take effect.
   - This 2-step process ensures that players can only play cards from their hand, while at the same time keeping the remainder of their hand private from the rest of the players.
6. After the round is finished, the `MakeNextPlay` choice will either return a new `Bidding` contract if the game is still ongoing, or a list of scores if the game has ended.

## Rules

We will use the following rules for non-partnership spades. Before the game, an order of play is decided upon and a player is chosen to lead the first round.

#### Setup

- Each player is dealt 13 cards from a shuffled standard deck.
- After seeing their cards, each player sets a bid between 0 and 13 for the number of tricks (see below) they expect to win.

#### Gameplay

- In every round, the player who leads places a card face up on the table. Following the order of play, the remaining players each do the same. Each card placed after the led card must must be of the same suit as the led card (the 'led suit') unless a player does not have any cards of that suit, in which case they may place any card.
- This set of four cards is called a trick. A trick is won by the player who placed the highest spade, or if no spades were played, the highest card of the led suit.
- The player who wins a trick leads the next round.

#### Scoring

- At after the final trick has been won, each player is awarded points.
- If a player wins at least as many tricks as they bid, they receive 10 points for every trick they bid and one point for each trick they win over their bid. For example, if a player bid 6 but wins 8 tricks, they would receive 62 points.
- If a player wins less tricks than they bid, they receive -10 points for every trick they bid. For example, if a player bid 6 but wins 5 tricks, they would receive -60 points.
- There is a special case for a bid of zero: If a player bids 0 and wins no tricks, they receive 100 points, but if they win any tricks they receieve -100 points.

This repeats until any player amasses a total of 250 points, in which case the game ends.