# 2270-Term-Project


Enemy Pathfinding Using A* Algorithm in Video Games

	
Pathfinding is a fundamental problem in video game development, particularly for modeling navigation and NPC behavior. In a nutshell, pathfinding in gaming is when an agent needs to move from point A to point B. In open-world games like The Legend of Zelda: Tears of the Kingdom (a current obsession of mine), enemies must traverse complex terrains - climbing over boulders, navigating through trees and wooden structures, and even swimming through open ocean - to pursue the player, creating engaging and dynamic gameplay. This process relies on representing the game world as a very complex graph, where nodes represent locations, and edges are possible paths between these nodes.

In this class, we previously explored such graph traversal algorithms as breadth-first search and depth-first search. Breadth-first search (BFS) explores all nodes at the current depth level before moving to the next depth level, which may be particularly useful in the event that all edges of our graph are unweighted. While BFS does technically guarantee that the shortest path will be found, it is not very efficient. Depth-first search (DFS) on the other hand explores as deeply as possible along one path before backtracking and considering alternative routes. While this doesn’t necessarily guarantee that the shortest path will be found, worry not, a path of some sort will be found from point A to point B (this is assuming that both points are indeed present in the graph). Both of these algorithms have their disadvantages computationally, and neither would be ideal for such a problem as enemy pathfinding. No, we want to instill fear in the player; to do so, we need the quickest path the enemy can take, and fast. Bring in A*!

The A* algorithm is a highly efficient method for real-time pathfinding. It combines the accuracy of Dijkstra’s algorithm (another pathfinding algorithm) with the speed of a heuristic-based approach, making it a popular choice for games requiring quick and adaptive enemy AI. Others may know it as a commonly used algorithm in map applications, as it finds the shortest route between point A and point B. Put simply, A* explores nodes in a guided manner based on the estimated total cost of a node. This allows the algorithm to prioritize nodes closer to the goal. A* is more efficient for pathfinding in games because it can account for both obstacles and dynamic terrain, focusing on the most promising path

By implementing A* on a simple graph, I aim to explore how algorithms enable intelligent behavior in games, making the player experience more immersive.

**What I’ll Do**

**Graph Representation:** Implement a grid-based graph structure in C++ to simulate a game map. Each cell in the grid represents a traversable or blocked area, and edges between nodes represent paths.

**A* Algorithm Implementation:** Write the A* algorithm to calculate the shortest path between two nodes (an enemy’s starting position and the player’s location). This will include:

The cost function, which is the fundamental aspect of A*: f(n) = g(n) + h(n)
	Where:
	g(n): The actual cost to reach the current node.
	h(n): A heuristic estimate of the cost to the goal.


**Why It’s Interesting**

Pathfinding is at the core of game AI, and A* is a practical example of how algorithms bring maps and characters to life. Exploring its application reveals how games balance complexity and efficiency, even in large, dynamic environments. The project also highlights the versatility of graphs in representing spatial data and solving problems beyond games, such as route optimization.




**A* Implementation & Walkthrough**

The first step in implementing the A* algorithm is to create the graph it will traverse. For this project, I designed a straightforward 2D grid structure, illustrated below. Each cell in the grid is represented by a node, which holds its x and y coordinates to indicate its position, along with a property to determine if it is traversable. Additionally, each node stores the necessary values for calculating its heuristic score (more on that shortly).

The Node structure is defined as follows:

	struct Node {
    		int x;         // Row index
   		int y;         // Column index
   		bool walkable; // True if the node can be traversed
    		float gCost;   // Cost from the start node
    		float hCost;   // Heuristic cost to the goal node
    		float fCost;   // Total cost (gCost + hCost)
    		Node* parent;  // Pointer to the parent node for path reconstruction

    		//Constructor for node
    		Node(int x, int y, bool walkable)
        		: x(x), y(y), walkable(walkable), gCost(INFINITY), hCost(0), fCost(INFINITY), parent(nullptr) {}

  		// Operator for comparing node heuristics
    		bool operator<(const Node* other) const {
        		if (fCost == other->fCost) {
            		return hCost < other->hCost;  // Prioritize lower hCost when fCost is equal
       		 }
        		return fCost > other->fCost;

My graph illustrates the valiant journey of our hero, Link, as he ventures through the treacherous Hylian Wood in search of the legendary Master Sword. Along the way, he must navigate a perilous path filled with lurking monsters and fearsome dragons. Time is of the essence, as the fate of Princess Zelda depends on his swift and courageous quest.

🧝 ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ 
⬜ 🌳 🌳 🌳 🌳 🌳 ⬜ ⬜ ⬜ ⬜ ⬜ 🌳 🌳 🌳 ⬜ 
⬜ 🌳 🌳 🌳 🌳 🌳 ⬜ ⬜ ⬜ ⬜ ⬜ 🌳 🌳 🌳 ⬜ 
⬜ ⬜ ⬜ ⬜ 🌳 🌳 ⬜ ⬜ ⬜ ⬜ ⬜ 🌳 🌳 🌳 ⬜ 
⬜ ⬜ ⬜ ⬜ 🌳 🌳 ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ 
⬜ ⬜ 🧌 ⬜ 🌳 🌳 ⬜ ⬜ 🌳 🌳 ⬜ ⬜ ⬜ ⬜ ⬜ 
⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ 🌳 🌳 ⬜ ⬜ 🐉 ⬜ ⬜ 
⬜ ⬜ ⬜ ⬜ 🌳 🌳 🌳 🌳 🌳 🌳 ⬜ ⬜ ⬜ ⬜ ⬜ 
⬜ ⬜ ⬜ ⬜ 🌳 🌳 🌳 🌳 🌳 🌳 ⬜ ⬜ ⬜ ⬜ ⬜ 
🌳 🌳 🌳 ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ 
🌳 🌳 🌳 ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ 
⬜ ⬜ ⬜ ⬜ 🌳 🌳 🌳 🌳 🌳 🌳 🌳 🌳 🌳 ⬜ ⬜ 
⬜ ⬜ ⬜ ⬜ 🌳 🌳 🌳 🌳 🌳 🌳 🌳 🌳 🌳 ⬜ ⬜ 
⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ 
⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ ⬜ 🗡️ 

The graph is created and displayed using the following code:

	// Function to display the grid with start and end nodes
	void createAndDisplayGrid(const vector<vector<Node>>& grid, const Node& start, const Node& goal, const Node& enemy1, const Node& enemy2) {
  		for (int i = 0; i < grid.size(); ++i) {
        		for (int j = 0; j < grid[i].size(); ++j) {
            			if (i == start.x && j == start.y) {
                			cout << "🧝 "; // Start node
           			} 
	       			else if (i == goal.x && j == goal.y) {
             				cout << "🗡️ "; // Goal node
           			} 
           			else if ( i == enemy1.x && j == enemy1.y){
                			cout << "🧌  "; // Enemy node
           			}
            			else if ( i == enemy2.x && j == enemy2.y){
                			cout << "🐉 "; // Enemy node
            			}			
	    			else {
               				cout << (grid[i][j].walkable ? "⬜ " : "🌳 "); // Walkable: '.', Blocked: '#'
           			}
        		}
       			cout << endl;
   		 }
	}

The start node (our guy, Link) is represented by "🧝", while his end goal is represented by "🗡️"
Enemies are represented as either a troll "🧌" (enemy1) or a dragon "🐉" (enemy2).
Walkable nodes (or, nodes that can be traversed by our hero) are represented by "⬜", while nodes that CANNOT be traversed are represented by "🌳".

The main function is where the grid is actually constructed—determining its size and placing any blockades. While I could have made the code more dynamic by allowing the user to input the grid's dimensions and randomly generate blockades and enemy locations, I opted to manually build the grid for this project. This approach provided more control over the structure and allowed for better visualization of the algorithm's functionality. Thoughts for next time, though!


    int rows = 15; // Number of rows in the grid
    int cols = 15; // Number of columns in the grid
    
    // creates our grid;
    vector<vector<Node>> grid(rows, vector<Node>(cols, Node(0, 0, true)));
    for (int i = 0; i < rows; ++i) {
        for (int j = 0; j < cols; ++j) {
            grid[i][j] = Node(i, j, true);
        }
    }
	// First blockade
    grid[2][2].walkable = false;
    grid[2][1].walkable = false;
    grid[2][3].walkable = false;
    grid[2][4].walkable = false;
    grid[2][5].walkable = false;
    grid[1][2].walkable = false;
    grid[1][1].walkable = false;
    grid[1][3].walkable = false;
    grid[1][4].walkable = false;
    grid[1][5].walkable = false;
    grid[3][4].walkable = false;
    grid[3][5].walkable = false;
    grid[4][4].walkable = false;
    grid[4][5].walkable = false;
    grid[5][4].walkable = false;
    grid[5][5].walkable = false;

    // Second blockade
    grid[7][6].walkable = false;
    grid[8][6].walkable = false;
    grid[7][7].walkable = false;
    grid[7][8].walkable = false;
    grid[8][7].walkable = false;
    grid[8][8].walkable = false;
    grid[6][8].walkable = false;
    grid[6][9].walkable = false;
    grid[5][8].walkable = false;
    grid[5][9].walkable = false;
    grid[7][9].walkable = false;
    grid[8][9].walkable = false;
    grid[7][5].walkable = false;
    grid[7][4].walkable = false;
    grid[8][5].walkable = false;
    grid[8][4].walkable = false;

    // Third Blockade
    grid[9][0].walkable = false;
    grid[10][0].walkable = false;
    grid[9][1].walkable = false;
    grid[10][1].walkable = false;
    grid[9][2].walkable = false;
    grid[10][2].walkable = false;

    // Fourth Blockade
    grid[11][4].walkable = false;
    grid[11][5].walkable = false;
    grid[11][6].walkable = false;
    grid[11][7].walkable = false;
    grid[11][8].walkable = false;
    grid[11][9].walkable = false;
    grid[11][10].walkable = false;
    grid[11][11].walkable = false;
    grid[11][12].walkable = false;
    grid[12][4].walkable = false;
    grid[12][5].walkable = false;
    grid[12][6].walkable = false;
    grid[12][7].walkable = false;
    grid[12][8].walkable = false;
    grid[12][9].walkable = false;
    grid[12][10].walkable = false;
    grid[12][11].walkable = false;
    grid[12][12].walkable = false;

    // Fifth Blockade
    grid[1][11].walkable = false;
    grid[1][12].walkable = false;
    grid[1][13].walkable = false;
    grid[2][11].walkable = false;
    grid[2][12].walkable = false;
    grid[2][13].walkable = false;
    grid[3][11].walkable = false;
    grid[3][12].walkable = false;
    grid[3][13].walkable = false;

    // Define start and goal nodes
    Node* start = &grid[0][0];
    Node* goal = &grid[14][14];
    Node* enemy1 = &grid[5][2];
    Node* enemy2 = &grid[6][12];
    grid[enemy1->x][enemy1->y].walkable = false;
    grid[enemy2->x][enemy2->y].walkable = false;

Once the grid structure was built and displayed as envisioned, the next step was implementing the A* algorithm. To recap, A* is designed to find the shortest path between two points: in this case, Point A (Link's starting position) and Point B (the location of the Master Sword). The goal is to calculate the safest and quickest route for Link to reach the Master Sword, navigating through potential obstacles along the way.

But how does a computer determine the quickest path? This brings us back to the operator defined in the Node structure. The operator is responsible for comparing the cost of the current node and its neighboring nodes, selecting the one with the lowest calculated F-Cost (or, in the case of a tie, the lowest H-Cost). But what exactly does this mean?

The A* algorithm relies on a cost function: f(n) = g(n) + h(n), which is calculated and assigned to each node, to guide the search for the shortest path. 

- g(n) (G-Cost): The cost to move from the start node to the current node. This is the distance traveled so far.
- h(n) (H-Cost): A heuristic estimate of the cost to the goal.

Both of these added together determines the total cost of a particular node. Furthermore, I needed to find a heuristic to calculate my H-Cost. After some research, I discovered that the two most commonly used heuristics are the Manhattan and Euclidean methods. For this project, I opted for the Manhattan heuristic, as it is particularly well-suited for simple grid-based systems like this one, offering both accuracy and efficiency.

The Manhattan method:

	// Manhattan distance heuristic - used to estimate the cost from the current node to the goal node
	float calculateHeuristic(const Node& a, const Node& b) {
  		return abs(a.x - b.x) + abs(a.y - b.y);

Now on the the good stuff!

Within the A* function itself, the first step is to initialize a priority queue. This is where pointers to current nodes will be temporarily stored as the information within that node is compared to its neighboring ones. A 2D vector is also initialized here, entitled closedList, which keeps track of nodes that have already been processed. A processed node is one whose neighbors have already been evaluated, and the node is no longer considered for further exploration. Once a node is moved to the closedList, it is "closed" and will not be revisted.

	vector<Node> aStarAlgorithm(vector<vector<Node>>& grid, Node& start, Node& goal) {
 		// Creates a priority queue;
    		priority_queue<Node*> openList;   
      		// Creates a 2D vector;
    		vector<vector<bool>> closedList(grid.size(), vector<bool>(grid[0].size(), false));

The next step is to initialize a pointer to our start node (the location of which is determined by the user - in this case, me) as well as the start node's cost values. The G-Cost of the starting node will always be 0, as this is the starting point. The H-Cost is calculated with the Manhattan method above using the coordinates of both the start and goal nodes. The F-Cost is calculated by adding the G-Cost and the H-Cost.

	Node* start_point = &start;
 	start.gCost = 0;  // Cost is 0 because it's start
    	start.hCost = calculateHeuristic(start, goal);  // Use Manhattan heuristic to calculate cost
    	start.fCost = start.gCost + start.hCost;  // Total cost
 	
At this point, a pointer to the start node (start_point) is added to the priority queue. The algorithm then enters a while loop, which continues to execute as long as the openList is not empty. I also added in a debugging checkpoint here that prints out coordinates and F-Costs of traversed nodes to essentially see how the algorithm works through the grid. I found this very fascinating.

	openList.push(start_point);                              

	while (!openList.empty()) {
		priority_queue<Node*> tempQueue = openList; // Create a temporary queue to access the elements
        		cout << "OpenList contents: ";
        		while (!tempQueue.empty()) {
            			Node* n = tempQueue.top();
            			cout << "(" << n->x << ", " << n->y << ", fCost: " << n->fCost << ") ";
            			tempQueue.pop();
        		}
        		cout << endl;

A pointer is then created to reference the first item in the openList, and that item is removed from the list. At this stage, we proceed to the cost comparison. The first check is to determine if the x and y coordinates of the current node match those of the goal node. If they do, the path to the goal (now the current node) is returned. If the current node is not the goal, the costs of that node and all of its neighboring nodes—considering up, down, left, right, and diagonal directions—are compared.

	if (current->x == goal.x && current->y == goal.y) {
            vector<Node> path;
            while (current != nullptr) {
                path.push_back(*current);
                current = current->parent;
            }
            reverse(path.begin(), path.end());
            return path;  // Return the reconstructed path
        }

        closedList[current->x][current->y] = true;

        // Check neighbors (4 directions: up, down, left, right, diagonal)
        vector<pair<int, int>> directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}, {-1, -1}, {-1, 1}, {1, -1}, {1, 1}}; // Includes diagonals!
        for (auto& dir : directions) {
            int newX = current->x + dir.first;
            int newY = current->y + dir.second;

            if (newX >= 0 && newX < grid.size() && newY >= 0 && newY < grid[0].size() &&
                grid[newX][newY].walkable && !closedList[newX][newY]) {

                Node& neighbor = grid[newX][newY];
                float tentativeGCost = current->gCost + 1;

                if (tentativeGCost < neighbor.gCost) {
                    neighbor.gCost = tentativeGCost;
                    neighbor.hCost = calculateHeuristic(neighbor, goal);
                    neighbor.fCost = neighbor.gCost + neighbor.hCost;
                    neighbor.parent = current;
                    openList.push(&neighbor);
                }           
            }
        }
    }
    return {};

The A* algorithm will be guided by the nodes with the lowest cost - often, those with the shortest distance from the current node to the end node as calculated by the A* function and accompanying heuristic. If no path is found, an empty path is returned. Seeing as my little grid was particularly simplistic, a path was indeed found. When my main function is run, the following path is output from start to goal.

Path found!
(0, 0) (1, 0) (2, 0) (3, 1) (4, 2) (5, 3) (6, 4) (7, 3) (8, 3) (9, 4) (10, 5) (10, 6) (10, 7) (10, 8) (10, 9) (10, 10) (10, 11) (10, 12) (11, 13) (12, 14) (13, 14) (14, 14)

I think the next step in this implementation is to actually represent the path on the grid somehow, but I'm honestly just happy that I got it to work at all.



**Conclusion**

When I first began this project, I was initially overwhelmed. Open-ended, "choose-your-own-adventure" projects like this can be challenging for me, as I tend to overthink the best approach. However, I decided to reconnect with the reason I embarked on this computer science journey in the first place: video games! I wanted to focus on something that truly resonated with my core motivations. During my research, I came across the A* Algorithm and was intrigued by its history and underlying principles.
Overall, I found this project to be both enlightening and enjoyable. Walking through the program line by line was a particularly effective way to understand complex algorithms like A*. Though there are areas I could improve with more time—such as dynamic grid construction and path display—I feel this assignment has greatly expanded my understanding of graph data structures, particularly in their application to gaming.

