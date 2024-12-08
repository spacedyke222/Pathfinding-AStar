# 2270-Term-Project


Enemy Pathfinding Using A* Algorithm in Video Games

	
Pathfinding is a fundamental problem in video game development, particularly for modeling navigation and NPC behavior. In a nutshell, pathfinding in gaming is when an agent needs to move from point A to point B. In open-world games like The Legend of Zelda: Tears of the Kingdom (a current obsession of mine), enemies must traverse complex terrains - climbing over boulders, navigating through trees and wooden structures, and even swimming through open ocean - to pursue the player, creating engaging and dynamic gameplay. This process relies on representing the game world as a very complex graph, where nodes represent locations, and edges are possible paths between these nodes.

In this class, we previously explored such graph traversal algorithms as breadth-first search and depth-first search. Breadth-first search (BFS) explores all nodes at the current depth level before moving to the next depth level, which may be particularly useful in the event that all edges of our graph are unweighted. While BFS does technically guarantee that the shortest path will be found, it is not very efficient. Depth-first search (DFS) on the other hand explores as deeply as possible along one path before backtracking and considering alternative routes. While this doesn’t necessarily guarantee that the shortest path will be found, worry not, a path of some sort will be found from point A to point B (this is assuming that both points are indeed present in the graph). Both of these algorithms have their disadvantages computationally, and neither would be ideal for such a problem as enemy pathfinding. No, we want to instill fear in the player; to do so, we need the quickest path the enemy can take, and fast. Bring in A*!

The A* algorithm is a highly efficient method for real-time pathfinding. It combines the accuracy of Dijkstra’s algorithm (another pathfinding algorithm) with the speed of a heuristic-based approach, making it a popular choice for games requiring quick and adaptive enemy AI. Others may know it as a commonly used algorithm in map applications, as it finds the shortest route between point A and point B. Put simply, A* explores nodes in a guided manner based on the estimated total cost of a node. This allows the algorithm to prioritize nodes closer to the goal. A* is more efficient for pathfinding in games because it can account for both obstacles and dynamic terrain, focusing on the most promising path
  By implementing A* on a simple graph, I aim to explore how algorithms enable intelligent behavior in games, making the player experience more immersive.

What I’ll Do

Graph Representation: Implement a grid-based graph structure in C++ to simulate a game map. Each cell in the grid represents a traversable or blocked area, and edges between nodes represent paths.

A* Algorithm Implementation: Write the A* algorithm to calculate the shortest path between two nodes (an enemy’s starting position and the player’s location). This will include:

The cost function, which is the fundamental aspect of A*: f(n) = g(n) + h(n)
Where:
g(n): The actual cost to reach the current node.
h(n): A heuristic estimate of the cost to the goal.


Why It’s Interesting

Pathfinding is at the core of game AI, and A* is a practical example of how algorithms bring maps and characters to life. Exploring its application reveals how games balance complexity and efficiency, even in large, dynamic environments. The project also highlights the versatility of graphs in representing spatial data and solving problems beyond games, such as route optimization.

By implementing A* for an enemy chase, this project will illustrate:

How enemies adapt their movements in real-time to player actions.
The balance between realism and computational efficiency in game AI.

