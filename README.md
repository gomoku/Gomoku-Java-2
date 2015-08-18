Introduction to Programming Using Java, Seventh Edition Version 7.0, 

August 2014 - Solution for Programming Exercise 7.8, author David Eck (eck@hws.edu) - Gomoku.java --- 

Original website: http://math.hws.edu/eck/cs124/javanotes7/c7/ex8-ans.html

Introduction to Programming Using Java Version 3.1, 

February 2001 - Solution for Programming Exercise 8.6 (older version), author David Eck (eck@hws.edu) - GomokuOld.java --- 

Original website: http://math.hws.edu/eck/cs124/javanotes3/c8/ex-8-6-answer.html

 [ Exercises | Chapter Index | Main Index ]
Solution for Programming Exercise 7.8

This page contains a sample solution to one of the exercises from Introduction to Programming Using Java.
Exercise 7.8:

The game of Go Moku (also known as Pente or Five Stones) is similar to Tic-Tac-Toe, except that it is played on a much larger board and the object is to get five squares in a row rather than three. Players take turns placing pieces on a board. A piece can be placed in any empty square. The first player to get five pieces in a row -- horizontally, vertically, or diagonally -- wins. If all squares are filled before either player wins, then the game is a draw. Write a program that lets two players play Go Moku against each other.

Your program will be simpler than the Checkers program from Subsection 7.5.3. Play alternates strictly between the two players, and there is no need to highlight the legal moves. You will only need two classes, a short panel class to set up the interface and a Board class to draw the board and do all the work of the game. Nevertheless, you will probably want to look at the source code for the checkers program, Checkers.java, for ideas about the general outline of the program.

The hardest part of the program is checking whether the move that a player makes is a winning move. To do this, you have to look in each of the four possible directions from the square where the user has placed a piece. You have to count how many pieces that player has in a row in that direction. If the number is five or more in any direction, then that player wins. As a hint, here is part of the code from my program. This code counts the number of pieces that the user has in a row in a specified direction. The direction is specified by two integers, dirX and dirY. The values of these variables are 0, 1, or -1, and at least one of them is non-zero. For example, to look in the horizontal direction, dirX is 1 and dirY is 0.

int ct = 1;  // Number of pieces in a row belonging to the player.

int r, c;    // A row and column to be examined

r = row + dirX;  // Look at square in specified direction.
c = col + dirY;
while ( r >= 0 && r < 13 && c >= 0 && c < 13 
                                  && board[r][c] == player ) {
        // Square is on the board, and it 
        // contains one of the players' pieces.
   ct++;
   r += dirX;  // Go on to next square in this direction.
   c += dirY;
}

r = row - dirX;  // Now, look in the opposite direction.
c = col - dirY;
while ( r >= 0 && r < 13 && c >= 0 && c < 13 
                                 && board[r][c] == player ) {
   ct++;
   r -= dirX;   // Go on to next square in this direction.
   c -= dirY;
}

Here is a picture of my program, just after black has won the game.

![Tags: Five in a Row, Tic Tac Toe, TicTacToe, 5 in a Row, Go-Moku, Connect, Connect5, Connect6, Caro, Noughts and Crosses, Gomoku, Renju, Pente, Piskvork, Amoba, Kó³ko i Krzy¿yk, Gomocup, AI, Engine, Artificial Intelligence, Brain, Pbrain, Gra, Game, Source Code Files, Program, Programming, Github, Board, Coding.](Gomoku.png "Tags: Five in a Row, Tic Tac Toe, TicTacToe, 5 in a Row, Go-Moku, Connect, Connect5, Connect6, Caro, Noughts and Crosses, Gomoku, Renju, Pente, Piskvork, Amoba, Kó³ko i Krzy¿yk, Gomocup, AI, Engine, Artificial Intelligence, Brain, Pbrain, Gra, Game, Source Code Files, Program, Programming, Github, Board, Coding.")

And here an old version:

![Tags: Five in a Row, Tic Tac Toe, TicTacToe, 5 in a Row, Go-Moku, Connect, Connect5, Connect6, Caro, Noughts and Crosses, Gomoku, Renju, Pente, Piskvork, Amoba, Kó³ko i Krzy¿yk, Gomocup, AI, Engine, Artificial Intelligence, Brain, Pbrain, Gra, Game, Source Code Files, Program, Programming, Github, Board, Coding.](GomokuOld.png "Tags: Five in a Row, Tic Tac Toe, TicTacToe, 5 in a Row, Go-Moku, Connect, Connect5, Connect6, Caro, Noughts and Crosses, Gomoku, Renju, Pente, Piskvork, Amoba, Kó³ko i Krzy¿yk, Gomocup, AI, Engine, Artificial Intelligence, Brain, Pbrain, Gra, Game, Source Code Files, Program, Programming, Github, Board, Coding.")

gomoku game showing a winning position
Discussion

This is a fairly complicated program, but it's possible to design and build it in stages, testing each stage separately. The first stage, the general layout of the panel, is already done in the Checkers.java program. With just a few changes, the main panel class, the layout of the panel, and the button and message handling come directly from that program. Let's take the rest of the Go Moku game one stage at a time.

A two-dimensional array is used to store the contents of the board. This array is of type int[][] and is named board. It is defined as an instance variable in the Board class, and it is initialized in the constructor of that class to be a 13-by-13 array. The value in each position of the array is one of three constants: EMPTY, WHITE, or BLACK. When a game begins, each of the entries in the array is set to empty. When a player clicks on an empty square, the corresponding entry in the array is changed from EMPTY to the player's color, BLACK or WHITE. In the paintComponent() method, the contents of the board array are used to decide what pieces to draw on the board.

Drawing the Board

We need a paintComponent() method for the Board class that can draw the board. The board has 13 rows and 13 columns of spaces. How wide should the board be? If each square in the board is x pixels wide, we need a total of 13*x pixels just for the spaces. But there are also lines between the spaces. These require another 12 pixels. And there is a 2-pixel border on each side, for another 4 pixels added to the width. So, with squares of side x, we need a board that is 13*x+16 pixels wide. Since I wanted something about the same size as the original checkerboard, I choose x to be 12, giving a board width of 172 pixels. The height is also 172. The dimension of the board is actually set in the constructor for the main class, which uses a null layout and so has to set the sizes of all the components that it contains by hand.

The left edge of the col-th column of squares in the board is 2+13*col. This allows for the two-pixel border on the left and for 13 pixels for each of the preceding columns of squares. (That's 12 pixels for the square plus one pixel for the line between that column and the next.) The lines between the columns are drawn one pixel to the left of each column, at x values 1 + 13*i, for i from 1 to 12. Rows work the same way. To draw a piece in row number row and column number col, the command

g.fillOval(3 + 13*col, 3 + 13*row, 10, 10);

can be used. This allows a one-pixel border between the oval that represents that piece and the side of the square. In my program, I defined a method to draw a piece:

private void drawPiece(Graphics g, int piece, int row, int col) {
   if (piece == WHITE)
      g.setColor(Color.WHITE);
   else
      g.setColor(Color.BLACK);
   g.fillOval(3 + 13*col, 3 + 13*row, 10, 10);
}

The background of the canvas is gray, so the paintComponent() method only has to draw the black border around the board, the lines between the squares, and all the pieces on the board. Remember that the piece in a given row and column is recorded in the board array as board[row][col]. The board can be drawn by the following paintComponent() method:

public void paintComponent(Graphics g) {

   super.paintComponent(g); // Fill with background color, lightGray.
   
   /* Draw grid lines in dark gray.  */
   
   g.setColor(Color.DARK_GRAY);
   for (int i = 1; i < 13; i++) {
      g.drawLine(1 + 13*i, 0, 1 + 13*i, getSize().height);
      g.drawLine(0, 1 + 13*i, getSize().width, 1 + 13*i);
   }
   
   /* Draw a two-pixel black border around the edges of the board. */

   g.setColor(Color.BLACK;
   g.drawRect(0,0,getSize().width-1,getSize().height-1);
   g.drawRect(1,1,getSize().width-3,getSize().height-3);
   
   /* Draw the pieces that are on the board. */
   
   for (int row = 0; row < 13; row++)
      for (int col = 0; col < 13; col++)
         if (board[row][col] != EMPTY)
            drawPiece(g, board[row][col], row, col);
            
}  // end paintComponent()

Playing the Game

The logic of the GoMoku game itself is mostly in the method "void doClickSquare(int row, int col)", which is called by the mousePressed() method when the user clicks on the square in row number row and column number col. This method must check whether the move is legal. If so, the move is made. The method then checks whether the move wins the game. If so, the game ends. The game will also end if the board has become completely full. Otherwise, play passes to the other player.

The current player is recorded in an instance variable named currentPlayer. The value of this variable is one of the two constants WHITE or BLACK. The game can be ended by calling a method named gameOver(). I wrote a boolean-valued method called winner() to check whether a move wins the game. (When I first wrote this method, it did nothing but "return false". This let me try out the program at this stage of development, before I started working on the difficult problem of testing for a winner.) The doClickSquare() method can be written:

void doClickSquare(int row, int col) {
       // This is called by mousePressed() when a player clicks 
       // on the square in the specified row and col.  It has already 
       // been checked that a game is, in fact, in progress.
       
    /* Check that the user clicked an empty square.  If not, show an
       error message and exit. */
       
    if ( board[row][col] != EMPTY ) {
       if (currentPlayer == BLACK)
          message.setText("BLACK:  Please click an empty square.");
       else
          message.setText("WHITE:  Please click an empty square.");
       return;
    }
    
    /* Make the move.  Check if the board is full or if the move
       is a winning move.  If so, the game ends.  If not, then it's
       the other user's turn. */
       
    board[row][col] = currentPlayer;  // Make the move.
    repaint();
    
    if (winner(row,col)) {  // First, check for a winner.
       if (currentPlayer == WHITE)
          gameOver("WHITE wins the game!");
       else
          gameOver("BLACK wins the game!");
       return;
    }
    
    boolean emptySpace = false;     // Check if the board is full.
    for (int i = 0; i < 13; i++)
       for (int j = 0; j < 13; j++)
          if (board[i][j] == EMPTY)
             emptySpace = true;  // The board contains an empty space.
    if (emptySpace == false) {
       gameOver("The game ends in a draw.");
       return;
    }
    
    /* Continue the game.  It's the other player's turn. */
    
    if (currentPlayer == BLACK) {
       currentPlayer = WHITE;
       message.setText("WHITE:  Make your move.");
    }
    else {  
       currentPlayer = BLACK;
       message.setText("BLACK:  Make your move.");
    }
 
}  // end doClickSquare()

Determining the Winner

The winner() method is certainly the hardest part of the program. The method must look in each of the four possible directions from the square where the user has placed a piece. If the player has five or more pieces in a row in that direction, then the player has won. As indicated in the exercise, a direction can be indicated by two variables, dirX and dirY. The values of these variables for each of the four directions are:

                      dirX    dirY    Why?
                      ----    ----    --------------------------------
horizontal direction    1       0       Only x changes.
vertical direction      0       1       Only y changes.
falling diagonal        1       1       Both x and y change.
rising diagonal         1      -1       Change in opposing directions.

I wrote a method "int count(int player, int row, int col, int dirX, int dirY) that counts the number of pieces the specified player has in a row, starting from the square in row number row and column number col and looking in the direction indicated by dirX and dirY. This method contains the code given in the exercise. It returns the number of pieces found. My winner method just calls this method for each of the four directions:

/**
 * This is called just after a piece has been played on the
 * square in the specified row and column.  It determines
 * whether that was a winning move by counting the number
 * of squares in a line in each of the four possible
 * directions from (row,col).  If there are 5 squares (or more)
 * in a row in any direction, then the game is won.
 */
private boolean winner(int row, int col) {
     
   if (count( board[row][col], row, col, 1, 0 ) >= 5)
      return true;
   if (count( board[row][col], row, col, 0, 1 ) >= 5)
      return true;
   if (count( board[row][col], row, col, 1, -1 ) >= 5)
      return true;
   if (count( board[row][col], row, col, 1, 1 ) >= 5)
      return true;
      
   /* When we get to this point, we know that the game is not won. */

   return false;
   
}  // end winner()

When I first wrote this method, I checked whether the number of pieces was "== 5" instead of ">= 5". This was a bug. It's possible for a player to get more than 5 pieces in a row, if the player plays a piece in an empty square that joins two shorter rows of pieces together.

Marking the Winning Pieces

In my program, when a player wins, the row of pieces that wins the game is marked with a red line. To do this, I added four instance variables of type int to the Board class. The instance variables are named win_r1, win_c1, win_r2, and win_c2. If the game has not yet been won, then the value of win_r1 is -1. The paintComponent() method uses this value as a signal that it should not draw any red line. After a player has won the game, the values of these variables are set to mark the squares at the two ends of the winning row of pieces. The positions of these squares are given by (win_r1,win_c1) and (win_r2,win_c2). If win_r1 is greater than -1, then the paintComponent() method draws a red line between these two squares.

I added some code to the winner() and count() methods to set the values of these variables properly. As the count() method counts pieces in a row, it sets win_r1, win_c1, win_r2, and win_c2 to mark the location of the last piece it finds in the two directions it checks. If the game is won, this will set the values correctly. To handle the case where the game is not won, I added the line "win_r1 = -1;" to the winner() method, just before the "return false;" statement. This ensures that the value of this variable will be -1 whenever the game is not yet won. (Perhaps this is all too tricky, but I really wanted to mark the winning pieces...)

Tags: Five in a Row, Tic Tac Toe, TicTacToe, 5 in a Row, Go-Moku, Connect, Connect5, Connect6, Caro, Noughts and Crosses, Gomoku, Renju, Pente, Piskvork, Amoba, Kó³ko i Krzy¿yk, Gomocup, AI, Engine, Artificial Intelligence, Brain, Pbrain, Gra, Game, Source Code Files, Program, Programming, Github, Board, Coding.
