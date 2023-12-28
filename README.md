# tic-tac-toe
import random

def print_board(board):
    print("-------------")
    for i in range(3):
        print("|", board[i][0], "|", board[i][1], "|", board[i][2], "|")
        print("-------------")

def check_winner(board, player):
    for i in range(3):
        if all(board[i][j] == player for j in range(3)):
            return True
        if all(board[j][i] == player for j in range(3)):
            return True
    if all(board[i][i] == player for i in range(3)):
        return True
    if all(board[i][2 - i] == player for i in range(3)):
        return True
    return False

def available_moves(board):
    return [(i, j) for i in range(3) for j in range(3) if board[i][j] == " "]

def minimax(board, depth, maximizing_player):
    if check_winner(board, "X"):
        return 10 - depth
    elif check_winner(board, "O"):
        return -10 + depth
    elif len(available_moves(board)) == 0:
        return 0

    if maximizing_player:
        max_eval = float("-inf")
        for move in available_moves(board):
            board[move[0]][move[1]] = "X"
            eval = minimax(board, depth + 1, False)
            board[move[0]][move[1]] = " "
            max_eval = max(max_eval, eval)
        return max_eval
    else:
        min_eval = float("inf")
        for move in available_moves(board):
            board[move[0]][move[1]] = "O"
            eval = minimax(board, depth + 1, True)
            board[move[0]][move[1]] = " "
            min_eval = min(min_eval, eval)
        return min_eval

def best_move(board):
    best_eval = float("-inf")
    best_move = None
    for move in available_moves(board):
        board[move[0]][move[1]] = "X"
        eval = minimax(board, 0, False)
        board[move[0]][move[1]] = " "
        if eval > best_eval:
            best_eval = eval
            best_move = move
    return best_move

def play_game():
    board = [[" " for _ in range(3)] for _ in range(3)]
    print("Welcome to Tic Tac Toe!")
    print_board(board)
    while True:
        human_move = None
        while human_move not in available_moves(board):
            human_move = tuple(map(int, input("Enter your move (row[0-2] column[0-2]): ").split()))
        board[human_move[0]][human_move[1]] = "O"
        print_board(board)
        if check_winner(board, "O"):
            print("Congratulations! You win!")
            break
        elif len(available_moves(board)) == 0:
            print("It's a draw!")
            break
        
        print("AI's move...")
        ai_move = best_move(board)
        board[ai_move[0]][ai_move[1]] = "X"
        print_board(board)
        if check_winner(board, "X"):
            print("AI wins! Better luck.")
            break
        elif len(available_moves(board)) == 0:
            print("It's a draw!")
            break

play_game()
