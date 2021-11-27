## [Assignment3 description](https://jrwright.info/cmput455/assignments/a3.html)

In this assignment, you develop and then improve a simulation-based player for the game of Gomoku. You add your improvements to the same starter code as for Assignment 2, which is a random Gomoku player.

### Setup
1. First, as in previous assignments, make sure you have your Python 3, NumPy and GoGui set up. You can review the procedures under Lecture 3 activities.
2. Download assignment3.tgz and expand it. The directory assignment3 contains:
    The same starter code as for assignment 2. This is based on a sample solution to assignment 1, and implements a random Gomoku player.
    A file assignment3-public-tests.gtp with some sample test cases for your program.
3. Modify/add code in directory assignment3 only to implement your solution.
4. For more information about concepts related to this assignment, see the following resources:
    Assignments 1 and 2: Game of Gomoku
    Lecture 12 - random simulations, simulations in TicTacToe, basics of Go3 player
    Lecture 13 - Improving simulations, filtering rules and pattern-based playouts
    Lecture 12 python codes which include simulation-based players for TicTacToe.
    The Go3 program and the Go3 documentation.



### PART 1: SIMULATION-BASED GOMOKU PLAYER
Create a new file Gomoku3.py inside your assignment3 folder. Then implement a simulation-based "Flat Monte Carlo" player there. The default player, when run without any of the behavior-changing options below, should work as follows:

A simulation consists of a series of moves generated uniformly at random, and ends when the game is over (win or draw) as defined in assignment 1.
The player runs N=10 simulations for each legal move, and picks one with highest win percentage. See simulation_player.py for a sample implementation of the algorithm. You are free to break ties between equally good moves in any way you wish.
As in assignment 1, your player should resign or pass only when the game is over.



### PART 2: RULE-BASED SIMULATIONS
In rule-based simulations, we restrict the number of moves that we choose from during simulation. For example, in class we looked at the MoGo patterns and some tactical rules in the case of Go. In this part of the assignment, you implement a rulebased policy with the rules below for your Gomoku3 player. The policy recognizes five types of moves. Go through the rules in order from most urgent (higher up in the list) to least urgent. For each rule, create the list of moves that match the rule. If the rule generates one or more moves, then randomly pick one of these moves. If the rule leads to an empty list of moves (no moves match), then proceed to the next rule.

1. Win: if you can win directly, in one move, then only consider one of the winning moves.
2. BlockWin: if the opponent can win directly, then only play a move that blocks the win. For example, OO.OO can be blocked by a move OOXOO.
             Even if you cannot prevent the win, as in the case .OOOO., or when there is more than one open four on the board, you should generate a blocking move.
3. OpenFour: if you have a move that creates an open four position of type .XXXX., then play it.
             Examples of this scenario are: .X.XX. and .XXX..
4. BlockOpenFour: play a move that prevents the opponent from getting an open four. For example, the situation ..OOO.. can be blocked by moves .XOOO.. or ..OOOX.. Note that all moves that block an open four should be generated. Specifically, in patterns .OO.O. and X.OOO.. all three moves work to block the open four and should be generated.
5. Random: if none of the previous cases applies, then generate a move uniformly at random, as in part 1.

All the examples above are given for X to play. However, your code should work for both players.

In Gomoku, there are more rules that make sense. For example, it is often a good move to make a threat that can be blocked, such as playing one of the empty points in ..XXXO However, in this assignment you do not implement such other rules.



### GTP COMMANDS
policy policytype
Sets the playout policy to be used from now on to the given type. The argument policytype is either random or rule_based.
policy_moves
This command prints the set of moves considered by the simulation policy for the current player in the current position, in the format

= MoveType movelist 
Where MoveType is one of: {Win, BlockWin, OpenFour, BlockOpenFour, Random}. Movelist is an alphabetically sorted list of moves (same sorting order as implemented in gogui-rules_legal_moves).

Example:

= BlockOpenFour b3 c2 d4
If playout policy is set to random, then the policy_moves response should also be of MoveType Random. If playout policy is set to rule_based, then the command should generate the move list according to the five types above.

genmove color
Your genmove command should generate a move using the rule-based simulation player with your new improved simulation policy.
All other existing GTP commands should be left as-is.



### The Usual Warnings - Read them All
Please make sure that these details are correct in your assignment 3 submission.
Your file must be a valid tgz file named assignment3.tgz which can be uncompressed with tar -xf assignment3.tgz
Name your main file Gomoku3.py and put it in the assignment3 directory.
Do not introduce extra levels of directories or different directory names. Keep the same simple structure as in the starter code.
You may add extra python files within the same directory.
Pre-submission Test and Submission
Follow the same general steps as in assignment 1 to create your presubmission.log file and your submission, but (of course) using your assignment3 directory, assignment3.tgz as file name, and assignment3-public-tests.gtp as test. Remember to include your new presubmission.log and readme.txt.



### Hints and Details - Read them All
Your simulation should stop immediately if the game is over according to the rules.
No special error handling code is required in this assignment. We will only test with valid moves and valid GTP commands which have valid arguments.
We will only test with alternating play, so any play commands will be for the color toPlay. Clearing the board resets toPlay to Black.
By default, we assume that teams will stay the same as in assignment 2. If you change your team, email Chenjun the new information before the assignment 3 deadline.
For examples of how to write new GTP commands, see the go, go2 and go3 codes, especially files gtp_connection_go1.py, gtp_connection_go2.py and gtp_connection_go3.py.
If both players follow the rulebased moves, then in practical play, for some move types there will be only zero or one possible moves of that type in any state. However, for testing we will set up arbitrary other scenarios. For example, there could be five open-four situations on the board in a test case. Your rulebased move generator should generate moves to block all of them at both ends.


