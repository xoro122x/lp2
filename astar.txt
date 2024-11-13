#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

// Define a structure for representing a node in the search tree
struct Node {
    vector<vector<char>> data; // Puzzle state represented as a matrix
    int level; // Level of the node in the search tree
    int fval; // Calculated f-value for the node

    // Constructor to initialize the node
    Node(vector<vector<char>> d, int l, int f){
        data=d;
        level=l;
        fval=f; {}
    } 
};

// Define a class for the puzzle
class Puzzle {
private:
    int n; // Size of the puzzle
    vector<Node> open; // Open list
    vector<Node> closed; // Closed list
    Node startNode; // Initial state node

public:
    // Constructor to initialize the puzzle size and lists
    Puzzle(int size) : n(size), startNode(vector<vector<char>>(size, vector<char>(size)), 0, 0) {}

    // Function to generate child nodes from the given node
    vector<Node> generateChild(Node current, vector<vector<char>>& goal) {
        vector<Node> children;
        int x, y;
        // Find the position of the blank space
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (current.data[i][j] == '_') {
                    x = i;
                    y = j;
                    break;
                }
            }
        }
        // Define possible moves (up, down, left, right)
        vector<pair<int, int>> moves = {{x, y - 1}, {x, y + 1}, {x - 1, y}, {x + 1, y}};
        // Generate child nodes by moving the blank space
        for (auto move : moves) {
            int newX = move.first, newY = move.second;
            if (newX >= 0 && newX < n && newY >= 0 && newY < n) {
                vector<vector<char>> tempPuzzle = current.data;
                swap(tempPuzzle[x][y], tempPuzzle[newX][newY]);
                Node child(tempPuzzle, current.level + 1, 0);
                children.push_back(child);
            }
        }
        return children;
    }

    // Heuristic function to calculate h-value (Manhattan distance)
    int h(vector<vector<char>>& start, vector<vector<char>>& goal) {
        int count = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (start[i][j] != goal[i][j] && start[i][j] != '_') {
                    ++count;
                }
            }
        }
        return count;
    }

    // A* search algorithm
    void process(vector<vector<char>>& start, vector<vector<char>>& goal) {
        startNode = Node(start, 0, 0);
        startNode.fval = h(start, goal);
        open.push_back(startNode);
        while (!open.empty()) {
            Node current = open[0];
            open.erase(open.begin());
            if (h(current.data, goal) == 0) {
                break;
            }
            vector<Node> children = generateChild(current, goal);
            for (auto& child : children) {
                child.fval = h(child.data, goal) + child.level;
                open.push_back(child);
            }
            sort(open.begin(), open.end(), [](const Node& a, const Node& b) {
                return a.fval < b.fval;
            });
        }
    }
};

int main() {
    int size;
    cout << "Enter the size of the puzzle: ";
    cin >> size;
    Puzzle puzzle(size);
    vector<vector<char>> start(size, vector<char>(size));
    vector<vector<char>> goal(size, vector<char>(size));
    cout << "Enter the start state matrix: " << endl;
    for (int i = 0; i < size; ++i) {
        for (int j = 0; j < size; ++j) {
            cin >> start[i][j];
        }
    }
    cout << "Enter the goal state matrix: " << endl;
    for (int i = 0; i < size; ++i) {
        for (int j = 0; j < size; ++j) {
            cin >> goal[i][j];
        }
    }
    puzzle.process(start, goal);
    return 0;
}
