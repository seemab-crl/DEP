import java.util.*;

public class TicTacToe {

    static char[][] board = {
            {' ', ' ', ' '},
            {' ', ' ', ' '},
            {' ', ' ', ' '}
    };

    static final char HUMAN_PLAYER = 'X';
    static final char COMPUTER_PLAYER = 'O';

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Welcome to Tic-Tac-Toe!");

        printBoard();

        while (true) {
            humanMove(scanner);
            if (checkGame(HUMAN_PLAYER)) {
                System.out.println("You won! Congratulations!");
                break;
            }
            if (isBoardFull()) {
                System.out.println("It's a tie!");
                break;
            }

            computerMove();
            if (checkGame(COMPUTER_PLAYER)) {
                System.out.println("Computer won!");
                break;
            }
            if (isBoardFull()) {
                System.out.println("It's a tie!");
                break;
            }

            printBoard();
        }

        scanner.close();
    }

    static void printBoard() {
        System.out.println("-------------");
        for (int row = 0; row < 3; ++row) {
            System.out.print("| ");
            for (int col = 0; col < 3; ++col) {
                System.out.print(board[row][col] + " | ");
            }
            System.out.println();
            System.out.println("-------------");
        }
    }

    static void humanMove(Scanner scanner) {
        int row, col;
        do {
            System.out.print("Enter your move (row [1-3] column [1-3]): ");
            row = scanner.nextInt() - 1;
            col = scanner.nextInt() - 1;
        } while (!isValidMove(row, col));

        board[row][col] = HUMAN_PLAYER;
    }

    static boolean isValidMove(int row, int col) {
        if (row >= 0 && row < 3 && col >= 0 && col < 3 && board[row][col] == ' ') {
            return true;
        } else {
            System.out.println("Invalid move. Please try again.");
            return false;
        }
    }

    static boolean isBoardFull() {
        for (int row = 0; row < 3; ++row) {
            for (int col = 0; col < 3; ++col) {
                if (board[row][col] == ' ') {
                    return false;
                }
            }
        }
        return true;
    }

    static boolean checkGame(char player) {
        // Check rows, columns, and diagonals
        for (int i = 0; i < 3; ++i) {
            if (board[i][0] == player && board[i][1] == player && board[i][2] == player) {
                return true; // Row win
            }
            if (board[0][i] == player && board[1][i] == player && board[2][i] == player) {
                return true; // Column win
            }
        }
        if (board[0][0] == player && board[1][1] == player && board[2][2] == player) {
            return true; // Diagonal win
        }
        if (board[0][2] == player && board[1][1] == player && board[2][0] == player) {
            return true; // Diagonal win
        }
        return false;
    }

    static void computerMove() {
        int[] move = minimax(2, COMPUTER_PLAYER);
        board[move[0]][move[1]] = COMPUTER_PLAYER;
    }

    static int[] minimax(int depth, char player) {
        List<int[]> possibleMoves = generateMoves();

        int bestScore = (player == COMPUTER_PLAYER) ? Integer.MIN_VALUE : Integer.MAX_VALUE;
        int currentScore;
        int bestRow = -1;
        int bestCol = -1;

        if (possibleMoves.isEmpty() || depth == 0) {
            bestScore = evaluate();
        } else {
            for (int[] move : possibleMoves) {
                board[move[0]][move[1]] = player;
                if (player == COMPUTER_PLAYER) {
                    currentScore = minimax(depth - 1, HUMAN_PLAYER)[2];
                    if (currentScore > bestScore) {
                        bestScore = currentScore;
                        bestRow = move[0];
                        bestCol = move[1];
                    }
                } else {
                    currentScore = minimax(depth - 1, COMPUTER_PLAYER)[2];
                    if (currentScore < bestScore) {
                        bestScore = currentScore;
                        bestRow = move[0];
                        bestCol = move[1];
                    }
                }
                board[move[0]][move[1]] = ' ';
            }
        }
        return new int[] {bestRow, bestCol, bestScore};
    }

    static List<int[]> generateMoves() {
        List<int[]> possibleMoves = new ArrayList<>();
        for (int row = 0; row < 3; ++row) {
            for (int col = 0; col < 3; ++col) {
                if (board[row][col] == ' ') {
                    possibleMoves.add(new int[] {row, col});
                }
            }
        }
        return possibleMoves;
    }

    static int evaluate() {
        int score = 0;
        score += evaluateLine(0, 0, 0, 1, 0, 2); // Row 0
        score += evaluateLine(1, 0, 1, 1, 1, 2); // Row 1
        score += evaluateLine(2, 0, 2, 1, 2, 2); // Row 2
        score += evaluateLine(0, 0, 1, 0, 2, 0); // Column 0
        score += evaluateLine(0, 1, 1, 1, 2, 1); // Column 1
        score += evaluateLine(0, 2, 1, 2, 2, 2); // Column 2
        score += evaluateLine(0, 0, 1, 1, 2, 2); // Diagonal
        score += evaluateLine(0, 2, 1, 1, 2, 0); // Anti-diagonal
        return score;
    }

    static int evaluateLine(int row1, int col1, int row2, int col2, int row3, int col3) {
        int score = 0;

        // First cell
        if (board[row1][col1] == COMPUTER_PLAYER) {
            score = 1;
        } else if (board[row1][col1] == HUMAN_PLAYER) {
            score = -1;
        }

        // Second cell
        if (board[row2][col2] == COMPUTER_PLAYER) {
            if (score == 1) { // cell1 is computer
                score = 10;
            } else if (score == -1) { // cell1 is human
                return 0;
            } else { // cell1 is empty
                score = 1;
            }
        } else if (board[row2][col2] == HUMAN_PLAYER) {
            if (score == -1) { // cell1 is human
                score = -10;
            } else if (score == 1) { // cell1 is computer
                return 0;
            } else { // cell1 is empty
                score = -1;
            }
        }

        // Third cell
        if (board[row3][col3] == COMPUTER_PLAYER) {
            if (score > 0) { // cell1 and/or cell2 is computer
                score *= 10;
            } else if (score < 0) { // cell1 and/or cell2 is human
                return 0;
            } else { // cell1 and cell2 are empty
                score = 1;
            }
        } else if (board[row3][col3] == HUMAN_PLAYER) {
            if (score < 0) { // cell1 and/or cell2 is human
                score *= 10;
            } else if (score > 1) { // cell1 and/or cell2 is computer
                return 0;
            } else { // cell1 and cell2 are empty
                score = -1;
            }
        }
        return score;
    }
}
