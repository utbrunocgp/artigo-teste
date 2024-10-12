import time
import numpy as np
from collections import deque
import statistics as stats

maze=0
# Função para verificar se a posição está dentro dos limites
def is_valid_move(maze, pos):
    x, y = pos
    return 0 <= x < len(maze) and 0 <= y < len(maze[0]) and maze[x][y] == 0

# Implementação da BFS
def bfs(maze, start, goal):
    queue = deque([start])
    visited = set([start])
    parent = {start: None}
    states_visited = 0

    while queue:
        current = queue.popleft()
        states_visited += 1

        if current == goal:
            path = []
            while current:
                path.append(current)
                current = parent[current]
            return path[::-1], states_visited

        for move in moves:
            next_pos = (current[0] + move[0], current[1] + move[1])
            if is_valid_move(maze, next_pos) and next_pos not in visited:
                queue.append(next_pos)
                visited.add(next_pos)
                parent[next_pos] = current

    return [], states_visited

# Implementação da DFS
def dfs(maze, start, goal):
    stack = [start]
    visited = set([start])
    parent = {start: None}
    states_visited = 0

    while stack:
        current = stack.pop()
        states_visited += 1

        if current == goal:
            path = []
            while current:
                path.append(current)
                current = parent[current]
            return path[::-1], states_visited

        for move in moves:
            next_pos = (current[0] + move[0], current[1] + move[1])
            if is_valid_move(maze, next_pos) and next_pos not in visited:
                stack.append(next_pos)
                visited.add(next_pos)
                parent[next_pos] = current

    return [], states_visited

# Benchmark e execução
def run_algorithm(algorithm, name):
    start_time = time.time()
    path, states_visited = algorithm(maze, start, goal)
    end_time = time.time()
    duration = end_time - start_time
    if(len(path)>0):
        
        guarda_tempo[name].append(duration)
        guarda_estados[name].append(states_visited)
        guarda_caminho[name].append(len(path))
    print(f"\n{name}:")
    print(f"Tempo de execução: {duration:.16f} segundos")
    print(f"Número de estados visitados: {states_visited}")
    print(f"Caminho encontrado: {path}",len(path),'passos')

def teste():
    rows=100
    cols=100
    # Definição do labirinto (0 = caminho livre, 1 = obstáculo)
    global maze
    maze=np.random.choice([0, 1], size=(rows, cols), p=[0.65,0.35])
    
    # Executar BFS e DFS
    run_algorithm(bfs, "BFS")
    run_algorithm(dfs, "DFS")
    

# Coordenadas de início e fim
start = (0, 0)
goal = (99, 99)
# Movimentos possíveis: cima, baixo, esquerda, direita
moves = [(0, 1), (1, 0), (0, -1), (-1, 0)]
guarda_tempo={"BFS":[],
               "DFS":[]
    }
guarda_estados={"BFS":[],
                "DFS":[]
    }
guarda_caminho={"BFS":[],
                "DFS":[]
    }
while len(guarda_tempo["BFS"])<32:
    teste()
    
print("o valor medio de tempo gasto foi de ")
print("BFS:",stats.mean(guarda_tempo["BFS"]))
print("DFS:",stats.mean(guarda_tempo["DFS"]))
    
print("o valor medio de estados visitados foi de ")
print("BFS:",stats.mean(guarda_estados["BFS"]))
print("DFS:",stats.mean(guarda_estados["DFS"]))    
    
print("a distancia media do caminho encontrado foi de ")
print("BFS:",stats.mean(guarda_caminho["BFS"]))
print("DFS:",stats.mean(guarda_caminho["DFS"]))    
