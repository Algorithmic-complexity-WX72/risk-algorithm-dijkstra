import csv
import random
import networkx as nx
import matplotlib.pyplot as plt
from collections import defaultdict

def generar_grafo(csv_file, total_nodes):
    G = nx.Graph()
    # Leer el archivo CSV y seleccionar nodos aleatorios
    with open(csv_file, 'r') as file:
        reader = csv.reader(file)
        lines = [line for line in reader if len(line) >= 2]  # Verificar si la línea tiene al menos dos valores
        lines = random.sample(lines, total_nodes)
        
        # Agregar nodos al grafo
        for line in lines:
            node_name, _ = line
            G.add_node(node_name)
        
        # Asignar distancias aleatorias entre nodos
        for i in range(len(lines)):
            for j in range(i+1, len(lines)):
                distance = random.randint(10, 20)
                G.add_edge(lines[i][0], lines[j][0], weight=distance)
    
    return G

def dibujar_grafo(G):
    pos = nx.spring_layout(G)
    nx.draw(G, pos, with_labels=True, font_weight='bold')
    labels = nx.get_edge_attributes(G, 'weight')
    nx.draw_networkx_edge_labels(G, pos, edge_labels=labels)
    plt.show()

def dijkstra(G, inicio, destino):
    distancias = {nodo: float('inf') for nodo in G.nodes()}
    distancias[inicio] = 0
    caminos = defaultdict(list)
    nodos_vistos = []

    while nodos_vistos != list(G.nodes()):
        nodos_no_vistos = {nodo: distancias[nodo] for nodo in set(G.nodes()) - set(nodos_vistos)}
        if not nodos_no_vistos:
            break  # Si no hay nodos no visitados, salir del bucle
        nodo_min = min(nodos_no_vistos, key=nodos_no_vistos.get)
        nodos_vistos.append(nodo_min)

        for vecino in G[nodo_min]:
            peso = G[nodo_min][vecino]['weight']
            nueva_distancia = distancias[nodo_min] + peso
            if nueva_distancia < distancias[vecino]:
                distancias[vecino] = nueva_distancia
                caminos[vecino] = caminos[nodo_min] + [nodo_min]

    return distancias, caminos

dataset = "names.csv"
nodos_totales = 5
mapa = generar_grafo(dataset, nodos_totales)
dibujar_grafo(mapa)

nodo_origen = input("Introduce el planeta de origen: ")
nodo_destino = input("Introduce el planeta de destino: ")
distancias, caminos = dijkstra(mapa, nodo_origen, nodo_destino)

if nodo_destino in caminos:
    ruta = ' -> '.join(caminos[nodo_destino] + [nodo_destino])
    distancia = distancias[nodo_destino]
    print(f"El camino más corto desde {nodo_origen} a {nodo_destino} es: {ruta} (Distancia: {distancia})")
else:
    print(f"No hay un camino válido desde {nodo_origen} a {nodo_destino}")

print("Todas las rutas posibles:")
for nodo in mapa.nodes():
    if nodo != nodo_origen:
        ruta = ' -> '.join(caminos[nodo] + [nodo])
        distancia = distancias[nodo]
        print(f"{nodo_origen} -> {ruta} (Distancia: {distancia})")
