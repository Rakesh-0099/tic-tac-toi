# tic-tac-toi
import tkinter as tk
import random
from tkinter import messagebox

# Initialize the game state
board = ["-", "-", "-", "-", "-", "-", "-", "-", "-"]
currentPlayer = "X"
winner = None
gameRunning = True

# Game board GUI
def printBoard():
    for i in range(9):
        buttons[i].config(text=board[i])

def playerClick(index):
    global currentPlayer
    if board[index] == "-" and gameRunning:
        board[index] = currentPlayer
        printBoard()
        checkIfWin()
        checkIfTie()
        if gameRunning:
            switchPlayer()
            if currentPlayer == "O":
                computer()

# Check for win or tie
def checkHorizontle():
    global winner
    if board[0] == board[1] == board[2] and board[0] != "-":
        winner = board[0]
        return True
    elif board[3] == board[4] == board[5] and board[3] != "-":
        winner = board[3]
        return True
    elif board[6] == board[7] == board[8] and board[6] != "-":
        winner = board[6]
        return True

def checkRow():
    global winner
    if board[0] == board[3] == board[6] and board[0] != "-":
        winner = board[0]
        return True
    elif board[1] == board[4] == board[7] and board[1] != "-":
        winner = board[1]
        return True
    elif board[2] == board[5] == board[8] and board[2] != "-":
        winner = board[2]
        return True

def checkDiag():
    global winner
    if board[0] == board[4] == board[8] and board[0] != "-":
        winner = board[0]
        return True
    elif board[2] == board[4] == board[6] and board[2] != "-":
        winner = board[2]
        return True

def checkIfWin():
    global gameRunning
    if checkHorizontle() or checkRow() or checkDiag():
        printBoard()
        messagebox.showinfo("Game Over", f"The winner is {winner}!")
        gameRunning = False

def checkIfTie():
    global gameRunning
    if "-" not in board and gameRunning:
        printBoard()
        messagebox.showinfo("Game Over", "It is a tie!")
        gameRunning = False

# Switch player
def switchPlayer():
    global currentPlayer
    if currentPlayer == "X":
        currentPlayer = "O"
    else:
        currentPlayer = "X"

def computer():
    while currentPlayer == "O" and gameRunning:
        position = random.randint(0, 8)
        if board[position] == "-":
            board[position] = "O"
            printBoard()
            checkIfWin()
            checkIfTie()
            switchPlayer()

# Reset the game
def resetGame():
    global board, currentPlayer, winner, gameRunning
    board = ["-", "-", "-", "-", "-", "-", "-", "-", "-"]
    currentPlayer = "X"
    winner = None
    gameRunning = True
    printBoard()

# Set up the GUI window
root = tk.Tk()
root.title("Tic-Tac-Toe")

buttons = []
for i in range(9):
    button = tk.Button(root, text="-", font=('normal', 20), height=3, width=6,
                       command=lambda i=i: playerClick(i))
    button.grid(row=i // 3, column=i % 3)
    buttons.append(button)

# Reset button
reset_button = tk.Button(root, text="Reset", font=('normal', 15), command=resetGame)
reset_button.grid(row=3, column=1)

# Start the GUI loop
printBoard()
root.mainloop()
