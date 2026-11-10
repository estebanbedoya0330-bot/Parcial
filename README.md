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


pedido = []

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



def registrar_producto(productos, pedido, codigo, cantidad):
    
    if codigo not in productos:
        print("Producto no encontrado.")
        return

    if cantidad <= 0:
        print("Cantidad no valida.")
        return

    nombre, precio, existencias = productos[codigo]

  
    if cantidad > existencias:
        print("No hay suficientes unidades disponibles.")
        return

    
    subtotal = precio * cantidad

    pedido.append((codigo, nombre, cantidad, subtotal))

   
    productos[codigo] = (nombre, precio, existencias - cantidad)

    print(f"Se agregaron {cantidad} x {nombre} al pedido. Subtotal: ${subtotal}")



def calcular_total(pedido):
    total = 0
    for item in pedido:
        total += item[3]  
    return total



def leer_entero(mensaje):
    texto = input(mensaje)
    if texto.lstrip("-").isdigit():
        return int(texto)
    return None



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
