# Assignment 1 : Question 6
|Std ID|Name|
|------|-|
|K228721|Attaullah Ansar|
|K228717|Javeria Rao|

import heapq
from collections import deque

# representation of the romania map as a graph
romania_map = {
    'Arad': {'Zerind': 75, 'Sibiu': 140, 'Timisoara': 118},
    'Zerind': {'Arad': 75, 'Oradea': 71},
    'Oradea': {'Zerind': 71, 'Sibiu': 151},
    'Sibiu': {'Oradea': 151, 'Arad': 140, 'Fagaras': 99, 'Rimnicu Vilcea': 80},
    'Timisoara': {'Arad': 118, 'Lugoj': 111},
    'Lugoj': {'Timisoara': 111, 'Mehadia': 70},
    'Mehadia': {'Lugoj': 70, 'Drobeta': 75},
    'Drobeta': {'Mehadia': 75, 'Craiova': 120},
    'Craiova': {'Drobeta': 120, 'Rimnicu Vilcea': 146, 'Pitesti': 138},
    'Rimnicu Vilcea': {'Sibiu': 80, 'Craiova': 146, 'Pitesti': 97},
    'Fagaras': {'Sibiu': 99, 'Bucharest': 211},
    'Pitesti': {'Rimnicu Vilcea': 97, 'Craiova': 138, 'Bucharest': 101},
    'Bucharest': {'Fagaras': 211, 'Pitesti': 101, 'Giurgiu': 90, 'Urziceni': 85},
    'Giurgiu': {'Bucharest': 90},
    'Urziceni': {'Bucharest': 85, 'Hirsova': 98, 'Vaslui': 142},
    'Hirsova': {'Urziceni': 98, 'Eforie': 86},
    'Eforie': {'Hirsova': 86},
    'Vaslui': {'Urziceni': 142, 'Iasi': 92},
    'Iasi': {'Vaslui': 92, 'Neamt': 87},
    'Neamt': {'Iasi': 87}
}

#heuristics (straight-line distance to Bucharest) table for the informed strategies
heuristics = {
    'Arad': 366, 'Bucharest': 0, 'Craiova': 160, 'Drobeta': 242, 'Eforie': 161,
    'Fagaras': 176, 'Giurgiu': 77, 'Hirsova': 151, 'Iasi': 226, 'Lugoj': 244,
    'Mehadia': 241, 'Neamt': 234, 'Oradea': 380, 'Pitesti': 100, 'Rimnicu Vilcea': 193,
    'Sibiu': 253, 'Timisoara': 329, 'Urziceni': 80, 'Vaslui': 199, 'Zerind': 374
}
def bfs(graph, start1, goal1):
    visited = set() 
    queue1 = [] 
    queue1.append((start1, [start1]))  

    while queue1:
        (vertex1, path1) = queue1.pop(0)
        if vertex1 not in visited:
            visited.add(vertex1)
            if vertex1 == goal1:
                return path1  # Return the path if goal is reached
            for neighbor in graph[vertex1]:
                if neighbor not in visited:
                    queue1.append((neighbor, path1 + [neighbor]))  # Enqueue the node and path to it
    return None
def ucs(graph, start2, goal2):
    visited = set()
    queue2 = []
    heapq.heappush(queue2, (0, start2, [start2]))  #priority queue

    while queue2:
        (cost, vertex2, path2) = heapq.heappop(queue2)
        if vertex2 not in visited:
            visited.add(vertex2)
            if vertex2 == goal2:
                return (cost, path2)  # Return the cost and path if goal is reached
            for neighbor, weight in graph[vertex2].items():
                if neighbor not in visited:
                    heapq.heappush(queue2, (cost + weight, neighbor, path2 + [neighbor]))
    return None

# implementing Greedy best first search for the romanian map
def greedy_best_first_search(graph, heuristics, start3, goal3):
    visited = set()  # set to keep track of visited nodes.
    queue3 = []  # initialize a priority queue
    heapq.heappush(queue3, (heuristics[start3], start3, [start3]))

    while queue3:
        # pop the vertex with the lowest heuristic value
        _, current, path3 = heapq.heappop(queue3)
        if current == goal3:
            return path3  # return the path if goal is reached

        visited.add(current)

        # go through all the neighbors of the current node
        for neighbor in graph[current]:
            if neighbor not in visited:
                # calculate heuristic for the neighbor
                priority = heuristics[neighbor]
                # add the neighbor to the queue with its heuristic value
                heapq.heappush(queue3, (priority, neighbor, path3 + [neighbor]))
    return None
# implementing iterative deepening depth first search for the romanian map
def iddfs(graph, start4, goal4, max_depth):
    #recursive depth limited search
    def dls(current, goal4, depth):
        if depth == 0:
            return None if current != goal4 else [current]
        if current == goal4:
            return [current]
        for neighbor in graph[current]:
            if neighbor not in visited:
                visited.add(neighbor)
                path4 = dls(neighbor, goal4, depth - 1)
                if path4 is not None:
                    return [current] + path4
        return None
    
    for depth in range(max_depth):
        visited = set([start4])
        path4 = dls(start4, goal4, depth)
        if path4 is not None:
            return path4
    return None




source = input("Enter the source city: ")
destination = input("Enter the destination city: ")
# Execute UCS

bfs_path = bfs(romania_map, source, destination)
print(f"BFS path from {source} to {destination}: {bfs_path}")

# Test UCS
ucs_cost, ucs_path=ucs(romania_map, source, destination)
print(f"UCS path from {source} to {destination} with total cost {ucs_cost}: {ucs_path}")
gbfs_path = greedy_best_first_search(romania_map, heuristics, source,destination)
print(f"Greedy Best-First Search path from {source} to {destination}: {gbfs_path}")
max_depth = 20  # Example depth limit
iddfs_path = iddfs(romania_map, source, destination, max_depth)
print(f"Iterative Deepening Depth-First Search path from {source} to {destination}: {iddfs_path}")
import heapq
from collections import deque



def compare_algorithms(graph, heuristics, source, destination):
    # Execute BFS
    bfs_path = bfs(graph, source, destination)
    bfs_cost = calculate_path_cost(bfs_path, graph)

    # Execute UCS
    ucs_cost, ucs_path = ucs(graph, source, destination)

    # Execute Greedy Best-First Search
    gbfs_path = greedy_best_first_search(graph, heuristics, source, destination)
    gbfs_cost = calculate_path_cost(gbfs_path, graph)

    # Execute IDDFS
    max_depth = 20  # Example depth limit
    iddfs_path = iddfs(graph, source, destination, max_depth)
    iddfs_cost = calculate_path_cost(iddfs_path, graph)

    # Display results
    print(f"BFS Path: {bfs_path}, Cost: {bfs_cost}")
    print(f"UCS Path: {ucs_path}, Cost: {ucs_cost}")
    print(f"Greedy Best-First Search Path: {gbfs_path}, Cost: {gbfs_cost}")
    print(f"IDDFS Path: {iddfs_path}, Cost: {iddfs_cost}")

    # Compare and find the best algorithm
    costs = {
        "BFS": bfs_cost,
        "UCS": ucs_cost,
        "Greedy Best-First": gbfs_cost,
        "IDDFS": iddfs_cost
    }

    best_algorithm = min(costs, key=costs.get)
    print(f"\nThe best algorithm is: {best_algorithm}")

def calculate_path_cost(path, graph):
    cost = 0
    for i in range(len(path) - 1):
        cost += graph[path[i]][path[i + 1]]
    return cost

compare_algorithms(romania_map, heuristics, source, destination)
![image](https://github.com/NUCES-Khi/assign1-7questions-javeria/assets/159615746/1fa3d6ee-14b3-4c35-b055-a333cdd9f08a)
