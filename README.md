# Tic-Tac-toe-project-code
import tkinter as tk
from tkinter import messagebox

# Initialize the board as a 3x3 list
def initialize_board():
    return [[' ' for _ in range(3)] for _ in range(3)]

# Check if a player has won
def check_win(board, player):
    # Check rows, columns, and diagonals
    win_conditions = [
        [board[0][0], board[0][1], board[0][2]],
        [board[1][0], board[1][1], board[1][2]],
        [board[2][0], board[2][1], board[2][2]],
        [board[0][0], board[1][0], board[2][0]],
        [board[0][1], board[1][1], board[2][1]],
        [board[0][2], board[1][2], board[2][2]],
        [board[0][0], board[1][1], board[2][2]],
        [board[2][0], board[1][1], board[0][2]],
    ]
    return any(all(cell == player for cell in condition) for condition in win_conditions)

# Check if the board is full
def is_draw(board):
    return all(cell != ' ' for row in board for cell in row)

class TicTacToe:
    def __init__(self, root, starting_player):
        self.root = root
        self.root.title("Tic Tac Toe")
        self.board = initialize_board()
        self.current_player = starting_player
        self.buttons = [[None for _ in range(3)] for _ in range(3)]
        self.create_widgets()

    def create_widgets(self):
        for row in range(3):
            for col in range(3):
                button = tk.Button(self.root, text=' ', font=('normal', 40), width=5, height=2,
                                   command=lambda r=row, c=col: self.make_move(r, c))
                button.grid(row=row, column=col)
                self.buttons[row][col] = button

    def make_move(self, row, col):
        if self.board[row][col] == ' ':
            self.board[row][col] = self.current_player
            self.buttons[row][col].config(text=self.current_player)

            if check_win(self.board, self.current_player):
                messagebox.showinfo("Tic Tac Toe", f"Player {self.current_player} wins!")
                self.reset_game()
            elif is_draw(self.board):
                messagebox.showinfo("Tic Tac Toe", "It's a draw!")
                self.reset_game()
            else:
                self.current_player = 'O' if self.current_player == 'X' else 'X'

    def reset_game(self):
        self.board = initialize_board()
        self.current_player = starting_player
        for row in range(3):
            for col in range(3):
                self.buttons[row][col].config(text=' ')

def start_game(starting_player):
    game_window = tk.Toplevel(root)
    game = TicTacToe(game_window, starting_player)

# Create the main window
root = tk.Tk()
root.title("Main Window")

# Instructions label
instructions = tk.Label(root, text="Welcome to Tic Tac Toe!\nClick the button below to start the game.", font=('normal', 14))
instructions.pack(pady=20)

# Player selection
player_selection_label = tk.Label(root, text="Choose the starting player:", font=('normal', 14))
player_selection_label.pack(pady=10)

# Start game buttons for X and O
start_button_x = tk.Button(root, text="Start with X", font=('normal', 14), command=lambda: start_game('X'))
start_button_x.pack(pady=10)

start_button_o = tk.Button(root, text="Start with O", font=('normal', 14), command=lambda: start_game('O'))
start_button_o.pack(pady=10)

# Run the main loop
root.mainloop()
