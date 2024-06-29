# Codealpha_sudoko

#include <iostream>
#include <vector>
using namespace std;

const int N = 9; // Size of Sudoku grid

// Function to print the Sudoku grid
void printGrid(const vector<vector<int>>& grid) {
    for (int i = 0; i < N; ++i) {
        for (int j = 0; j < N; ++j) {
            cout << grid[i][j] << " ";
        }
        cout << endl;
    }
}

// Function to check if a number can be placed in a particular cell
bool isSafe(vector<vector<int>>& grid, int row, int col, int num) {
    // Check row
    for (int j = 0; j < N; ++j) {
        if (grid[row][j] == num) {
            return false;
        }
    }

    // Check column
    for (int i = 0; i < N; ++i) {
        if (grid[i][col] == num) {
            return false;
        }
    }

    // Check 3x3 box
    int startRow = row - row % 3;
    int startCol = col - col % 3;
    for (int i = startRow; i < startRow + 3; ++i) {
        for (int j = startCol; j < startCol + 3; ++j) {
            if (grid[i][j] == num) {
                return false;
            }
        }
    }

    return true;
}

// Function to solve the Sudoku puzzle using backtracking
bool solveSudoku(vector<vector<int>>& grid) {
    int row, col;
    bool isEmpty = true;

    // Find empty cell
    for (row = 0; row < N; ++row) {
        for (col = 0; col < N; ++col) {
            if (grid[row][col] == 0) {
                isEmpty = false;
                break;
            }
        }
        if (!isEmpty) {
            break;
        }
    }

    // No empty cell found, puzzle solved
    if (isEmpty) {
        return true;
    }

    // Try placing numbers 1 to 9 in empty cell
    for (int num = 1; num <= 9; ++num) {
        if (isSafe(grid, row, col, num)) {
            grid[row][col] = num;

            if (solveSudoku(grid)) {
                return true;
            }

            // Backtrack
            grid[row][col] = 0;
        }
    }

    return false;
}

int main() {
    vector<vector<int>> grid(N, vector<int>(N, 0));

    // Input Sudoku grid
    cout << "Enter Sudoku grid (enter 0 for empty cells):\n";
    for (int i = 0; i < N; ++i) {
        for (int j = 0; j < N; ++j) {
            cin >> grid[i][j];
        }
    }

    // Print initial Sudoku grid
    cout << "\nInitial Sudoku grid:\n";
    printGrid(grid);

    // Prompt user to solve the Sudoku puzzle
    cout << "\nPress 1 to see the solution: ";
    int choice;
    cin >> choice;

    if (choice == 1) {
        // Solve Sudoku
        if (solveSudoku(grid)) {
            cout << "\nSolution:\n";
            printGrid(grid);
        }
        else {
            cout << "\nNo solution exists.\n";
        }
    }
    else {
        cout << "\nInvalid choice. Exiting...\n";
    }

    return 0;
}
