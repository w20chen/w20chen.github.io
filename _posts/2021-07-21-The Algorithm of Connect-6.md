---
layout:     	post
title: 	Algorithm of Connect-6
subtitle: 	Greedy Algorithm
date:      	2021-07-21
author:    	Chen
header-img: img/post-bg-unix-linux.jpg
catalog:   true
tags:
    - CS
---
# Algorithm of Connect 6

<img src="/img/RobotE.png" width="800" height="600" /><br>

### Greedy Algorithm

<strong>Greedy algorithm</strong> is an algorithmic paradigm that follows the problem solving heuristic of making the locally optimal choice at each stage with the intention of finding a global optimum. To put it simply, greedy algorithm is just like playing a chess game - if you make the best choice at each step, it is very likely that you will win the game. Greedy algorithm will not necessarily find out the perfect solution to a problem, but it proves efficient.<br>

### Rule of Connect 6

<strong>Connect 6</strong> is a chess game similar to <strong>Gomoku</strong>. The game emerged on the internet around 1999 and was presented by the team of Professor I-Chen Wu at the 11th Advances in Computer Games Conference in 2005.<br>
Two players, Black and White, alternately place two stones of their own color, black and white respectively, on empty intersections of a Go-like board, except for that Black (the first player) places one stone only for the first move. The one who gets six consecutive stones of his color first (horizontally, vertically or diagonally) wins.<br>


### Strategy

The crux of this strategy is rating each available spot and deciding where your two stones are going to be placed based upon the rating.<br>
The data structure of this method is a two-dimensional array. In order to differentiate three states of a spot (white, balck, empty), the data type is integer. (e.g. white = 0, black = 1, empty = -1)<br>
After receiving data from the server, the local chess board should be immediately updated.<br>
Then update the score of every spot on the chess board and make the optimal choice.<br>


### How to rate the chess board
<strong>Based upon Tuple</strong><br>
<img src="/img/Connect6_rating_criterium.png" width="800" height="600" /><br>
The score of one spot is determined by the situation of <strong>24</strong> tuples it belongs to. (Each tuple has 6 elements. There are 4 directions. On each direction there are 6 tuples.) The rating table is shown above.<br>
Different rating tables will lead to different results. I am quite sure there's a better rating table.<br>
As a matter of fact, you do not have to update every spot on the board. Two stones of one step only influent 48 tuples at most.<br>
In order to make it convenient to debug, the score of the chess borad will be saved in file.<br>
<img src="/img/Solution_Connect6.png" width="800" height="600" />
<br>
    
### By the way
Apparently, a better strategy of a chess game is based upon the data structure <strong>Game Tree</strong>, using some searching method like <strong>Alpha - Beta Pruning.</strong><br>
Simple greedy algorithm is an efficient and quick method.<br>
Another thing need to be mentioned is the double-criteria method, in which you can rate <strong>the whole chess board</strong> and decide which rating table (to play aggressively or defensively) is better.<br>

```cpp
/*This is what the double-criteria rating table is like.*/
const int defensiveArray[8] = { 0,5,15,100,4500,4000000,4000000,4000000 };
const int aggressiveArray[8] = { 0,10,380,1050,50000,1000000,1000000,1000000 };

/*Let these two pointers point to either of the rating arrays above.*/
const int* blackValue = aggressiveArray;
const int* whiteValue = defensiveArray;
```

### Code (C/C++)
Here is how to rate all the vertical tuples.<br>
Horizontal, diagonal, back-diagonal tuples are rated in similar ways.<br>

```cpp
static void vertical(void) {
	for (register int col = 0; col < 19; col++) {
		for (register int row = 0; row <= 13; row++) {
			register int wCnt = 0, bCnt = 0;
			for (register int i = row; i < row + 6; i++) {
				if (chessboard[i][col] == WHITE) wCnt++;
				else if (chessboard[i][col] == BLACK) bCnt++;
			}
			baseScore += wCnt;
			baseScore -= bCnt;
			if (wCnt && bCnt);
			else if (wCnt) {
				for (register int i = row; i < row + 6; i++) {
					if (chessboard[i][col] == EMPTY)
						value[i][col] += whiteValue[wCnt];
				}
			}
			else if (bCnt) {
				for (register int i = row; i < row + 6; i++) {
					if (chessboard[i][col] == EMPTY)
						value[i][col] += blackValue[bCnt];
				}
			}
		}
	}
}
```
