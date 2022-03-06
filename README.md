import numpy as np
import matplotlib.pyplot as plt

dic_fruit = {    #diccionario para asignar valores a las frutas
    'orange' : 0,
    'grapefruit' : 1,

}
diccionario_inverso = {v: k for k, v in dic_fruit.items()} # transforma datos numéricos
# en categórico

info = open('citrus.csv','r')                                       # datos file es una variable de tipo objeto donde se guarda la informacion de las frutas
info_en_lineas=info.read().splitlines()                             #remueve los saltos de linea del archivo
fila1=info_en_lineas[0]                                             #Guarda la primer fila
conjunto=[]                                                         #variable donde se almacenara el conjunto S

for lineas in  info_en_lineas:                                      #verifica la informacion cargada en memoria

    if lineas !=fila1:                                              #acepta todos los numeros los miembros del csv excepto el encabezado

        valores = lineas.split(',')                                 # Lee la información separada por","

        comas_naranjas = []                                         #variable donde se guarda el valor de citrus
        if len(valores)==6:                                         # condicion para excluir espacios en blanco

            for i in range(6):                                      #ciclo para recorrer las columnas
                if i != 0:                                          #condición para tomar valores numericos
                    filas= float(valores[i])                        # transforma str a floar
                else:                                               #Condición para tomar los valores str
                    filas = dic_fruit[valores[i]]                   #condición para cambiar los valores str a int
                comas_naranjas.append(filas)                        #los valores que toman la variable fila se asigna a comas_naranjas

        conjunto.append(comas_naranjas)                             #conjunto toma los valores de comas_naranjas
info.close()                                                        #Cierra la parte del programa encargada del texto

conjunto= np.array(conjunto)                                        #se transforma los valores de conjunto a valores tipo numpy
print(conjunto)                                                     #imprime los datos de conjunto

#comienza la parte gráfica
'''
print("PARTE GRÁFICA")
plt.figure("Ejercicio 2")                                           #Imprime nombre a la ventana
plt.title("Frutas")                                                 #Imprime nombre a la gráfica
plt.xlabel("Diametro de la fruta")                                  #Imprime en el eje x a la gráfica
plt.ylabel("Peso de la fruta")                                      #Imprime en el eje y a la gráfica
scatter=plt.scatter(conjunto[:,1], conjunto[:,2], c=conjunto[:,0])   #Se asignan los parametros para gráficar
plt.legend(handles= scatter.legend_elements()[0], labels =dic_fruit)#Se asignan nombres a los gráficos
plt.show()                                                          #muestra el gráfico
'''


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
#print("copia",copia_conjunto)#imprime datos escalados


etiqueta=conjunto[:,0:1]


copia_conjunto=np.append(copia_conjunto,etiqueta,axis=1)

#np.insert(copia_conjunto, etiqueta, axis=0)


#print("copia 2: ",copia_conjunto)








print("algoritmo knnnnnnnnnnnnnnnnnnnnnnn")
x = [19.35,200.51,145,68,2]
# x = [5.9, 3.0, 5.1, 1.8]

sx = [0, 0, 0, 0, 0]  # scaled x

for ft in range(5):
    sx[ft] = (x[ft] - valor_medio[ft]) / desv_est[ft]

print('x (scaled) = ', sx)



def distancia_eucl(p1, p2):
    d = 0
    for ft in range(5):
        d += (p2[ft] - p1[ft]) ** 2
    d **= 0.5
    return d




def knn(S, D, x, K):
    print('Running KNN...')

    clase_d = np.zeros((len(S),2))

    for i, row in enumerate(S):
        s = row[:]
        clase_d[i, 0] = row[5]
        clase_d[i,1]= D(x, s)
    print("Clase d: ",clase_d)

    # Step 2
    # Bubble sort
    for i in range(1, len(clase_d)):

        for j in range(0, len(clase_d) - i):
            if clase_d[j, 1] > clase_d[j + 1, 1]:
                aux = clase_d[j].copy()
                clase_d[j] = clase_d[j + 1]
                clase_d[j + 1] = aux
    print("burja", clase_d)

    #Paso 3


    print('K = ', K)

    contador = {}
    for i in range(K):
        if clase_d[i,0] in contador:
            contador[clase_d[i, 0]] += 1
        else:
            contador[clase_d[i, 0]] = 1


    print("es contador", contador)
    #paso 4
    mf_k = list(contador.keys())[0]
    mf_v = contador[mf_k]

    for k, v in contador.items():
        if v > mf_v:
            mf_k = k
            mf_v = v

    print(mf_k)
    print(mf_v)

    return mf_k

resultado = knn(copia_conjunto, distancia_eucl, sx, 5)

print('RESULT = ', diccionario_inverso[resultado])
