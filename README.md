# Parcial
# ==============================================================
# PARCIAL 3 - LÓGICA DE PROGRAMACIÓN
# Sistema de pedidos de una cafetería universitaria
#
# Integrantes del grupo:
#   - Joan Nicolas Rey Gomez
#   - (Agregar aquí los nombres de los demás integrantes)
# ==============================================================

# --------------------------------------------------------------
# 1. DICCIONARIO Y TUPLAS: catálogo de productos
#    Clave: código del producto
#    Valor: tupla (nombre, precio, existencias)
# --------------------------------------------------------------
productos = {
    1: ("Cafe", 3000, 20),
    2: ("Jugo natural", 5000, 15),
    3: ("Sandwich", 8000, 10),
    4: ("Empanada", 2500, 25),
    5: ("Hamburguesa", 12000, 8),
    6: ("Gaseosa", 3500, 30),
    7: ("Agua", 2000, 30),
    8: ("Brownie", 3500, 12),
    9: ("Porcion de pizza", 6000, 10),
    10: ("Papas fritas", 4000, 15),
    11: ("Ensalada frutas", 4500, 10),
    12: ("Galletas", 1500, 40)
}

# --------------------------------------------------------------
# 3. LISTA para almacenar el pedido
#    Cada elemento es una tupla: (codigo, nombre, cantidad, subtotal)
# --------------------------------------------------------------
pedido = []


# --------------------------------------------------------------
# 2. FUNCIÓN PARA MOSTRAR EL CATÁLOGO
# --------------------------------------------------------------
def mostrar_productos(productos):
    print("\n========== CATALOGO ==========")
    print("Codigo  Producto          Precio    Existencias")
    print("----------------------------------------------")
    for codigo, info in productos.items():
        nombre = info[0]
        precio = info[1]
        existencias = info[2]
        print(f"{codigo:<7} {nombre:<17} ${precio:<8} {existencias}")
    print("----------------------------------------------")


# --------------------------------------------------------------
# 4. FUNCIÓN PARA REGISTRAR PRODUCTOS
# --------------------------------------------------------------
def registrar_producto(productos, pedido, codigo, cantidad):
    # Consultar si el código existe en el diccionario
    if codigo not in productos:
        print("Producto no encontrado.")
        return

    # Verificar que la cantidad sea mayor que cero
    if cantidad <= 0:
        print("Cantidad no valida.")
        return

    nombre, precio, existencias = productos[codigo]

    # Comprobar que existan suficientes unidades
    if cantidad > existencias:
        print("No hay suficientes unidades disponibles.")
        return

    # Calcular el subtotal
    subtotal = precio * cantidad

    # Agregar la tupla al pedido
    pedido.append((codigo, nombre, cantidad, subtotal))

    # Actualizar las existencias (las tuplas son inmutables,
    # por eso se crea una nueva tupla y se reemplaza en el diccionario)
    productos[codigo] = (nombre, precio, existencias - cantidad)

    print(f"Se agregaron {cantidad} x {nombre} al pedido. Subtotal: ${subtotal}")


# --------------------------------------------------------------
# 6. FUNCIÓN PARA CALCULAR EL TOTAL
# --------------------------------------------------------------
def calcular_total(pedido):
    total = 0
    for item in pedido:
        total += item[3]  # posición 3 = subtotal
    return total


# --------------------------------------------------------------
# Función auxiliar: leer números enteros sin que el programa se caiga
# --------------------------------------------------------------
def leer_entero(mensaje):
    texto = input(mensaje)
    if texto.lstrip("-").isdigit():
        return int(texto)
    return None


# --------------------------------------------------------------
# Función auxiliar: mostrar el resumen final del pedido
# --------------------------------------------------------------
def mostrar_resumen(pedido):
    total = calcular_total(pedido)

    if total >= 50000:
        descuento = total * 0.10
    else:
        descuento = 0

    total_final = total - descuento

    print("\n========== RESUMEN DEL PEDIDO ==========")
    print(f"{'Producto':<18}{'Cantidad':<10}{'Subtotal'}")
    print("-----------------------------------------")
    for codigo, nombre, cantidad, subtotal in pedido:
        print(f"{nombre:<18}{cantidad:<10}{subtotal}")
    print("-----------------------------------------")
    print(f"Total : ${int(total)}")
    print(f"Descuento : ${int(descuento)}")
    print(f"TOTAL A PAGAR : ${int(total_final)}")
    print("========================================")


# --------------------------------------------------------------
# 5. CICLO PARA REGISTRAR EL PEDIDO (menú principal)
# --------------------------------------------------------------
def main():
    opcion = 0
    while opcion != 3:
        print("\n========== CAFETERIA ==========")
        print("1. Mostrar productos")
        print("2. Agregar producto al pedido")
        print("3. Finalizar pedido")
        opcion = leer_entero("Seleccione una opcion: ")

        if opcion == 1:
            mostrar_productos(productos)

        elif opcion == 2:
            codigo = leer_entero("Ingrese el codigo del producto: ")
            cantidad = leer_entero("Ingrese la cantidad: ")
            if codigo is None:
                print("Producto no encontrado.")
            elif cantidad is None:
                print("Cantidad no valida.")
            else:
                registrar_producto(productos, pedido, codigo, cantidad)

        elif opcion == 3:
            if len(pedido) == 0:
                print("No se registraron productos en el pedido.")
            else:
                mostrar_resumen(pedido)

        else:
            print("Opcion no valida.")


main()
