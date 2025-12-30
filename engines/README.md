# Why I chose MinMax algorithm

I chose the Minimax algorithm for my chess bot because chess is a turn-based, two-player game with opposing goals. In every position, one player tries to maximize their chances of winning, while the other tries to minimize them. Minimax models this exact situation.
-	Maximizing player (my bot): tries to get the highest score
-	Minimizing player (opponent): tries to force the lowest score for you

The algorithm works by looking ahead several moves, and it evaluates possible future board positions. On my bot’s turn, it chooses the move that gives the best guaranteed outcome, and so my bot also is assuming that the opponent will also play the best move against it. So, this makes the bot strategic rather than random, and its then capable of planning ahead, because in a chess match the you don’t just move the pawn or bishop randomly there is a goal behind each movement.

### So I went with the minimax because it is especially suitable for chess and its simple:
•	The rules are clear and deterministic (there is no randomness)
•	Both players have complete information
•	And the decisions taken can be evaluated using a board evaluation function

# What did I add to improve the performance of the bot
I added **iterative deepening** at the move-selection level. The engine searches increasingly deeper levels and keeps the best move found so far. This also improves alpha–beta pruning efficiency and makes better use of limited computation time. 

### In general, this:
-	improves move ordering automatically
-	allows alpha–beta pruning to cut more branches
-	avoids “random-looking” equal-depth moves
-	reduces draws caused by shallow evaluation

## So, increasing the search depth increases the complexity, and that means that the bot is going to look more accurately.