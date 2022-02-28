# KNN
actividad III

conjunto2=conjunto[:,1:6] #Nueva matriz sin la primer columna

valor_min= [np.inf, np.inf, np.inf, np.inf, np.inf]#Arreglo para valor minimo
valor_max= [-np.inf, -np.inf, -np.inf, -np.inf, -np.inf]#Arreglo para valor maximo

for fila in conjunto2:#For para recorrer filas
    for ft in range(5):#For para recorrer columnas

        if fila[ft] < valor_min[ft]:#condición para ir comparando el valor minimo
            valor_min[ft]=fila[ft]#En caso que el valor sea menor se almacena en la variable

        if fila[ft] > valor_max[ft]:#condición para ir comparando el valor maximo
            valor_max[ft]=fila[ft]#En caso que el valor sea menor se almacena en la variable

print("Valor minimo: ", valor_min)#imprime valor min
print("Valor maximo: ",valor_max)#imprime valor max

copia_conjunto= conjunto2.copy() #se hace una copia de los datos

#Escalar los datos

for fila_indice in range(len(conjunto2)):  #For para recorrer filas
    for columna_indice in range(5):  #For para recorrer columnas
        copia_conjunto[fila_indice][columna_indice] = (conjunto2[fila_indice][columna_indice] - valor_min[columna_indice]) / \
                                    (valor_max[columna_indice] - valor_min[columna_indice])  #Aplicando escalarización
print(copia_conjunto)#imprime datos escalados


#Calcular el valor medio
valor_medio = [0, 0, 0, 0, 0]#Vector para guardar el valor medio

for fil in conjunto2:  #For para recorrer filas
    for col in range(5):  #For para recorrer columnas
        valor_medio[col] += fil[col]  #Acumulador de valor medio

for col in range(5):
    valor_medio[col] /= len(conjunto2)#División para valor medio

print('Valor medio = μ =', valor_medio)#Imprime valor medio

#Calcula desviación estándar
desv_est = [0, 0, 0, 0, 0]

for fila in conjunto2:  # #For para recorrer filas
    for col in range(5):  #For para recorrer columnas
        desv_est[col] += (fila[col] - valor_medio[col]) ** 2  # calculando suma

for col in range(5):
    desv_est[col] /= len(conjunto2) - 1  # dividiendo numeros de objetos-1
    desv_est[col] **= 0.5  # obteniendo Raíz

print('Desvicación estándar = σ = ', desv_est)

copia_conjunto = conjunto2.copy()

# Escalando los datos

for fila_indice in range(len(conjunto2)):  #For para recorrer filas
    for columna_indice in range(5):  #For para recorrer columnas
        copia_conjunto[fila_indice][columna_indice] = (conjunto2[fila_indice][columna_indice] - valor_min[columna_indice]) / \
                                    desv_est[columna_indice]  #Aplicando escalarización
print(copia_conjunto)#imprime datos escalados
