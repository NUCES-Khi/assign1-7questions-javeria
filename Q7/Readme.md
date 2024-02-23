# Assignment 1 : Question 7
|Std ID|Name|
|------|-|
|K228721|Attaullah Ansar|
|K228717|Javeria Rao|
import random

def calculate_attacks(totalqueens):
    attacks = 0
    for i in range(len(totalqueens)):
        for j in range(i + 1, len(totalqueens)):
            # Check for same row or diagonal attacks
            if totalqueens[i] == totalqueens[j] or abs(totalqueens[i] - totalqueens[j]) == j - i:
                attacks += 1
    return attacks

def generate_neighbors(state1):
    neighbors = []
    for i in range(len(state1)):
        for j in range(len(state1)):
            if state1[i] != j:
                # Move the queen to the new row in the same column
                neighbor = state1.copy()
                neighbor[i] = j
                neighbors.append(neighbor)
    return neighbors

def print_board(total_queens):
    
    for row in range(len(total_queens)):
        for col in range(len(total_queens)):
            if total_queens[row] == col:
                print(" Q ", end="")
            else:
                print(" * ", end="")
        print()

def hill_climbing(n1):
    solutions = []
    
    for _ in range(10):  
       
        current = list(range(n1))
        random.shuffle(current)

        while True:
            print_board(current)  
            print("\n" + "=" * 20 + "\n")

            neighbors = generate_neighbors(current)
            # Select the neighbor with the least number of attacks
            next_state = min(neighbors, key=calculate_attacks)

            # If no neighbor has fewer attacks, we've reached a local maximum
            if calculate_attacks(next_state) >= calculate_attacks(current):
                break
            current = next_state

        solutions.append(current)

    return solutions

# Ask the user for the size of the board
n = int(input("Enter the value of n (4≤n≤8): "))
solutions = hill_climbing(n)

# Print all solutions
print("All solutions:")
for i, sol in enumerate(solutions, start=1):
    print(f"\nSolution {i}:\n")
    print_board(sol)
    print("=" * 20)
    ![image](https://github.com/NUCES-Khi/assign1-7questions-javeria/assets/159615746/a8019bdb-53eb-4090-b87a-3aba0987c09a)
![image](https://github.com/NUCES-Khi/assign1-7questions-javeria/assets/159615746/c3daf020-36e8-4f8e-ad3a-32d3636f4ae7)
