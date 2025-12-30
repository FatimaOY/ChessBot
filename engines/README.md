Put your engines and opening books here.

# (In my main.py)
# This function is the main decision-making entry point of the chess engine.

def get_move_at_depth(board, depth):
    # First, we check whether the current board position matches a known opening.
    # If an opening move exists, we play it immediately. To:
    # avoid unnecessary computation.
    opening_move = play_opening(board)

    if opening_move:
        print("PLAYING OPENING MOVE: ", opening_move)
        return opening_move


    # If no opening move is found, here we fall back to the minimax algorithm.
    top_move = None; #will store the best move found so far.

    # Opposite of our minimax
    if board.turn == chess.WHITE:
      # White is the maximizing player:
      # - White wants the highest possible evaluation.
      top_eval = -np.inf
    else:
      # Black is the minimizing player:
      # - Black wants the lowest possible evaluation.
      top_eval = np.inf


### Here we iterate through all allowed moves available in the current position.
#### First: Each move is temporarily played on the board to simulate future positions.
    for move in board.legal_moves:
        board.push(move)

        # WHEN WE ARE BLACK, WE WANT TRUE AND TO GRAB THE SMALLEST VALUE
        # After making a move, we call the minimax algorithm recursively.
          # depth - 1 because we have already made one move.
          # alpha is initialized to negative infinity.
          # beta is initialized to positive infinity.
        # board.turn tells minimax whose turn it is, which determines
        # whether the algorithm should maximize or minimize.
        eval = minimax(board, depth - 1, -np.inf, np.inf, board.turn)

        # After evaluating the move, we undo it.
        # This ensures the board state remains unchanged for the next iteration.
        board.pop()

        # If it is White's turn, we want to maximize the evaluation score.
        # If the current move leads to a better evaluation, we update our best move.
        if board.turn == chess.WHITE:
            if eval > top_eval:
                top_move = move
                top_eval = eval
        # If it is Black's turn, we want to minimize the evaluation score.
        # If the current move leads to a lower evaluation, we update our best move.
        else:
            if eval < top_eval:
                top_move = move
                top_eval = eval

    # After evaluating all legal moves, we select the move
    # that leads to the best possible outcome according to minimax.
    print("CHOSEN MOVE: ", top_move, "WITH EVAL: ", top_eval)
    return top_move

### def get_move(board, max_depth):
    best_move = None

    # Iterative deepening loop
    for depth in range(1, max_depth + 1):
        best_move = get_move_at_depth(board, depth)

    return best_move

## The bot will now:
- Search shallow first
- Gradually search deeper
- Always return the best move found

# --------------------------------------------------------------------
# (In my minmax.py) My Minmax algorithm:

### The evaluation function assigns a score to a board position. This score represents how favorable the position is for White. Minimax uses this function at leaf nodes to approximate future outcomes.
from .evaluation import get_evaluation
import numpy as np

### Minimax algorithm with Alpha-Beta pruning.
def minimax(board, depth, alpha, beta, maximizing_player):
  if depth == 0 or board.is_game_over(): #Base case of the recursion: If the maximum search depth is reached or the game is over.
    return get_evaluation(board)
  
  if maximizing_player:   # we try to find the move that produces the highest evaluation.
    max_eval = -np.inf
    for move in board.legal_moves:     # Explore all legal moves for the maximizing player.
      board.push(move)
      # Recursively evaluate the position after the move
      # The role switches to the minimizing player in the next depth level
      eval = minimax(board, depth - 1, alpha, beta, False)
      board.pop()
      max_eval = max(max_eval, eval)  # Update the best evaluation found so far.
      alpha = max(alpha, eval) # Update alpha with the best value the maximizing player can guarantee.
      # Alpha-Beta pruning condition:
      # If beta is less than or equal to alpha, further exploration
      # cannot improve the result, so we prune this branch.
      if beta <= alpha:   
        break
    return max_eval # Return best evall found for the maximizing player 
  #I player Black we try to find the move that produces the lowest evaluation.
  else:
    # Initialize the best evaluation to positive infinity
    # so any real evaluation will be worse.
    min_eval = np.inf
    for move in board.legal_moves: #explore all legal moves
      board.push(move)
      eval = minimax(board, depth - 1, alpha, beta, True)# The role switches back to the maximizing player.
      board.pop()
      min_eval = min(min_eval, eval)
      beta = min(beta, eval)

      if beta <= alpha:
        break
    return min_eval     #Returns the lowest evaluation found for the minimizing player.