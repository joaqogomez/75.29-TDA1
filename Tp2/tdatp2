#!/usr/bin/python3

"""
Se debe invocar al programa como:

./tdatp2 <E>|<A>|<A2> <datos.txt>

Donde E indica que se debe calcular la solución exacta, A indica que se debe
calcular la solución usando la aproximación propuesta por el curso, A2 indica
que se debe calcular la solución usando la aproximación propuesta por el grupo.
"""

import sys
from copy import deepcopy
from math import inf
from time import time
import readline


# Función que calcula la solución exacta del problema.
def _empaquetar_back(items, paquetes, tabs=0, len_min=inf):
    if len(items) == 0:
        # print("\t"*tabs, "items:", items)
        # print("\t"*tabs, "paquetes:", paquetes)
        return paquetes
    
    # tomamos el primer item
    item = items.pop(0)

    # agrego un nuevo paquete
    if paquetes == []:
        paquetes.append([item])
        return _empaquetar_back(items.copy(), deepcopy(paquetes), tabs+1)

    # hay que empaquetar el item
    # puede ser en un paquete existente
    # o en un paquete nuevo
    opciones = []
    # print("\t"*tabs, "item:", item)
    # print("\t"*tabs, "paquetes:", paquetes)
    for i in range(len(paquetes)):
        if sum(paquetes[i]) + item <= 1:
            paquetes_copia = deepcopy(paquetes)
            paquetes_copia[i].append(item)
            opcion = _empaquetar_back(items.copy(), paquetes_copia, tabs+1, len_min)
            if len(opcion) < len_min:
               len_min = len(opcion)
               opciones = [opcion]

    
    if len(opciones) + 1 <= len_min:
        paquetes_copia = deepcopy(paquetes)
        paquetes_copia.append([item])
        opciones.append(_empaquetar_back(items.copy(), paquetes_copia, tabs+1, len_min))
                
    # print("\t"*tabs, "opciones:", opciones)
    min = opciones[0]

    for opcion in opciones:
        if len(opcion) < len(min):
            min = opcion

    # print("\t"*tabs, "min:", min)

    return min
            
def empaquetar_back(items):
    paquetes = []
    return _empaquetar_back(items.copy(), paquetes)


# Función que calcula la solución usando la aproximación propuesta por el curso.
def empaquetar_aprox(items):
    paquetes = []
    paquete_actual = []
    for item in items:
        if round(sum(paquete_actual) + item, 4) <= 1:
            paquete_actual.append(item)
        else:
            paquetes.append(paquete_actual)
            paquete_actual = [item]
    paquetes.append(paquete_actual)
    return paquetes


# Función que calcula la solución usando la aproximación propuesta por el grupo.
def empaquetar_greedy(items):
    #ordenamos los items de mayor a menor
    items.sort(reverse=True)
    paquetes = []

    for item in items:
        
        # si no hay paquetes creo el primero
        if paquetes == []:
            paquetes.append([item])
            continue

        # itero sobre los paquetes
        agregado = False
        for i in range(len(paquetes)):
            # si el item entra en el paquete
            if round(sum(paquetes[i]) + item, 4) <= 1:
                paquetes[i].append(item)
                agregado = True
                break

        # si no entra en ningun paquete
        if not agregado:
            paquetes.append([item])
            
    return paquetes

def return_error():
    print("Error: argumentos inválidos.")
    print("Uso: ./tdatp2 <E>|<A>|<A2> <datos.txt>")
    sys.exit(1)

def main():
    if len(sys.argv) != 3:
        return_error()

    funcion = empaquetar_greedy
    nombre = "Solución Aproximada Alumnos"
    if sys.argv[1] == "E":
        funcion = empaquetar_back
        nombre = "Solución Exacta"
    elif sys.argv[1] == "A":
        funcion = empaquetar_aprox
        nombre = "Solución Aproximada"
    elif sys.argv[1] == "A2":
        funcion = empaquetar_greedy
        nombre = "Solución Aproximada Alumnos"
    else:
        return_error()

    items = []
    try:
        with open(sys.argv[2]) as f:
            n = int(f.readline())
            _ = f.readline()
            items = []
            for _ in range(n):
                items.append(float(f.readline()))
    except:
        return_error()

    start = time()
    paquetes = funcion(items)
    end = time()
    print(f"{nombre}: {len(paquetes)} paquetes")
    print(paquetes)
    print([sum(p) for p in paquetes])
    print(f"Tiempo de ejecución: {round((end - start) * 1000, 4)} ms")
    

if __name__ == "__main__":
    main()