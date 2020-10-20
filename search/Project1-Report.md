# <center>Project Report</center>
#### <p align = "right"> 17307130246</p>
#### <p align = "right">Yuzhu Zhang</p>
#### <p align= "right"> SDS, Fudan</p>

## Q1 Depth-First-Search
the algorithms used in Q1 to Q4 are identical, the only difference is the implement of frontier/fringe, whereas:

    Q1 dfs use stack, 
    Q2 bfs use queue, 
    Q3 ucs use priority queue, updated with cost
    Q4 A*  update priority queue with heuristic+cost

DFS will not always get the lowest cost.
If we construct all possible states into a tree, then for the worst case where the goal state occurs on the rightmost route of the tree, then the complexity is $O(n)$, n is the total number of possible states.

Also for eight-puzzle
```bash
A random puzzle:
-------------
| 1 | 5 | 4 |
-------------
| 3 | 2 | 8 |
-------------
| 6 |   | 7 |
-------------
BFS found a path of 9 moves: ['right', 'up', 'up', 'left', 'down', 'right', 'up', 'left', 'left']
...
```
## Q5 Corner Search Problem
In this problem,  *Goal State* changes *" whether  four corners are visited"*. 

To implement this, *state* of node need to store : 
1) current position: (x, y)
2) corners that are visited:  *VISITED_CORNERS*

once the agent found that it has reached an unvisited corner, it  should update and push current state into *VISITED_CORNERS*

At last the function only need to check if all four corners are in the *state*, if so, it should be the goal state.

```bash
tinyCorners
[SearchAgent] using function bfs
[SearchAgent] using problem type CornersProblem
Path found with total cost of 28 in 0.0 seconds
Search nodes expanded: 435
Pacman emerges victorious! Score: 512
...
mediumCorners
[SearchAgent] using function bfs
[SearchAgent] using problem type CornersProblem
Path found with total cost of 106 in 0.3 seconds
Search nodes expanded: 2448
Pacman emerges victorious! Score: 434
...
```

## Q6 Corner Heuristic:

The heurisitc for each state is the MINIMUM Manhattan distance from the current position to all UNVISITED CORNERS. Therefore we need to check  which corners have been visited and only calculate the left corners.

The steps:
   1) Derive the unvisited corners for each state
   2) Derive all the Manhattan distances from the current position to those state
   3) From these distances choose the minimum
   4) Tell(Return) the agent to go which direction.

The choice of minimum ensures the heuristics are admissive.

The following proves that heuristics are consistent:

```bash
python pacman.py -l mediumCorners -p AStarCornersAgent -z 0.5
Path found with total cost of 106 in 0.1 seconds
Search nodes expanded: 901
Pacman emerges victorious! Score: 434
...
```

```bash
python pacman.py -l mediumCorners -p SearchAgent -a fn=ucs,prob=CornersProblem       5
[SearchAgent] using function ucs
[SearchAgent] using problem type CornersProblem
Path found with total cost of 106 in 0.3 seconds
Search nodes expanded: 2448
Pacman emerges victorious! Score: 434
...
```
The cost of ucs equals to 106, which is the same with A*.
Note that the cost of each step remains 1, this implies length of path is actually the number of cost.
Therefore A* and UCS has the same length of path, the heuristics are consistent.

## Q7 Eat All Dots 

### Result: 
```bash
testSearch
Path found with total cost of 7 in 0.0 seconds
Search nodes expanded: 10
Pacman emerges victorious! Score: 513
...
trickySearch
Path found with total cost of 60 in 18.2 seconds
Search nodes expanded: 4137
Pacman emerges victorious! Score: 570
...
```
Problem occurrs:

[![BphbNR.md.png](https://s1.ax1x.com/2020/10/20/BphbNR.md.png)](https://imgchr.com/i/BphbNR)

[![BphHE9.md.png](https://s1.ax1x.com/2020/10/20/BphHE9.md.png)](https://imgchr.com/i/BphHE9)

One food are lefted until all the other dots are eaten.

Anlaysis:
for each state, agent always find the farest food in the sense that it would eat as many dots along its road. However it will cause the agent ignore the close food when the food is not on its route to the farest food.

Waiting to be improved:
1.  change the heuristic, not find the farest, but indicates the direction where the most food concentrated.
2.  or finding the farest food, get to that food, and return to the initial state, then start over again.

## Q8
implemented using DFS built in search.py 
```bash
bigSearch
[SearchAgent] using function depthFirstSearch
[SearchAgent] using problem type PositionSearchProblem
Path found with cost 350.
Pacman emerges victorious! Score: 2360
...
```