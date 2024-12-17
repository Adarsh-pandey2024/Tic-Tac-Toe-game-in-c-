# Tic-Tac-Toe-game-in-c-
#include <iostream>
#include <vector>
#include <string>

using namespace std;

class TicTacToe {
private:
    vector<vector<char>> board;
    char currentPlayer;

    void displayBoard() {
        cout << "\n";
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                cout << board[i][j];
                if (j < 2) cout << " | ";
            }
            cout << "\n";
            if (i < 2) cout << "--+---+--\n";
        }
        cout << "\n";
    }

    bool isWin() {
        // Check rows and columns
        for (int i = 0; i < 3; ++i) {
            if (board[i][0] == currentPlayer && board[i][1] == currentPlayer && board[i][2] == currentPlayer)
                return true;
            if (board[0][i] == currentPlayer && board[1][i] == currentPlayer && board[2][i] == currentPlayer)
                return true;
        }
        // Check diagonals
        if (board[0][0] == currentPlayer && board[1][1] == currentPlayer && board[2][2] == currentPlayer)
            return true;
        if (board[0][2] == currentPlayer && board[1][1] == currentPlayer && board[2][0] == currentPlayer)
            return true;

        return false;
    }

    bool isDraw() {
        for (int i = 0; i < 3; ++i)
            for (int j = 0; j < 3; ++j)
                if (board[i][j] == ' ')
                    return false;
        return true;
    }

    void switchPlayer() {
        currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
    }

public:
    TicTacToe() {
        board = vector<vector<char>>(3, vector<char>(3, ' '));
        currentPlayer = 'X';
    }

    void playGame() {
        bool gameRunning = true;

        while (gameRunning) {
            displayBoard();
            int row, col;

            // Prompt the player for input
            cout << "Player " << currentPlayer << "'s turn. Enter row and column (1-3): ";
            cin >> row >> col;

            // Adjust for 0-based indexing
            row--;
            col--;

            // Validate input
            if (row < 0 || row >= 3 || col < 0 || col >= 3 || board[row][col] != ' ') {
                cout << "Invalid move. Try again.\n";
                continue;
            }

            // Update board
            board[row][col] = currentPlayer;

            // Check for win or draw
            if (isWin()) {
                displayBoard();
                cout << "Player " << currentPlayer << " wins!\n";
                gameRunning = false;
            } else if (isDraw()) {
                displayBoard();
                cout << "It's a draw!\n";
                gameRunning = false;
            } else {
                switchPlayer();
            }
        }
    }

    bool playAgain() {
        char choice;
        cout << "Do you want to play again? (y/n): ";
        cin >> choice;
        return choice == 'y' || choice == 'Y';
    }

    void resetGame() {
        board = vector<vector<char>>(3, vector<char>(3, ' '));
        currentPlayer = 'X';
    }
};

int main() {
    TicTacToe game;
    
    do {
        game.playGame();
        if (game.playAgain()) {
            game.resetGame();
        } else {
            break;
        }
    } while (true);

    cout << "Thanks for playing!\n";
    return 0;
}

