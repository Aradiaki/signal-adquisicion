import wfdb
import matplotlib.pyplot as plt
import math
import numpy as np


# Leer la señal del archivo
signal = wfdb.rdrecord('0319214B-1559-4B14-8252-6659E91A480D')

#almacenar 100 primeros datos de la señal en un vector 
valores = signal.p_signal[:1000,0]

plt.plot(valores)
plt.show()
# Obtener la longitud y la suma de los valores
length = len(valores)
suma = sum(valores)

# Calcular la media
media = suma / length
print('La media es:', media)

# calcular potencia de la señal
valores_cuadrados=np.zeros(length)
for i in range(length):
    valores_cuadrados[i]=valores[i]**2
    
sumap=sum(valores_cuadrados)
potencia=sumap/length
print('potencia de la señal: ',potencia)

# Calcular las diferencias al cuadrado y la suma de las diferencias al cuadrado
diferencia=np.zeros(length)
suma2=0
for i in range(length):
    diferencia[i] = (valores[i] - media)**2
    suma2=sum(diferencia)
    
    
# calcular desviacion estandar
   
desviacion_estandar=math.sqrt(suma2/length)
print('Desviacion estandar: ',desviacion_estandar)
# calcular coeficiente de variacion

coeficiente_variacion=desviacion_estandar/media
print('coeficiente de variacion: ',coeficiente_variacion)

# crear ruido gaussiano
numerosazar=np.zeros(length)
numeros_aleatorios=numerosazar
for i in range(length):
 numeros_aleatorios[i]=np.random.randn()

ruido_gaussiano=(0.7*numeros_aleatorios)/125
plt.plot(ruido_gaussiano)
plt.show()

signal_ruidg=valores+ruido_gaussiano
plt.plot(signal_ruidg)
plt.show()

valores_cuadradosg=np.zeros(length)
for i in range(length):
    valores_cuadradosg[i]=ruido_gaussiano[i]**2
sumag=sum(valores_cuadradosg)
potenciag=sumag/length

SNRg=10*np.log10(potencia/potenciag)
print('relacion ruido ',SNRg)

# crear ruido artefacto
Fs=1200
F=60
t=np.arange(0,length,1/Fs)
y=np.sin(2*np.pi*F*t)[:1000]
plt.plot(y[:100])
plt.show()
signal_ruidoa=valores+y

valores_cuadradosa=np.zeros(length)
for i in range(length):
    valores_cuadradosa[i]=y[i]**2
sumaa=sum(valores_cuadradosa)
potenciaa=sumaa/length
print('potencia de la señal ruido a: ',potenciaa)
SNRa=10*np.log10(potencia/potenciaa)
print('SNRa ',SNRa)

# ruido impulso
ti=np.arange(0,length,1)
impulso=(ti==500)*0.7
signal_impulso=valores+impulso
plt.plot(signal_impulso)
plt.show()

valores_cuadradosi=np.zeros(length)
for i in range(length):
    valores_cuadradosi[i]=impulso[i]**2
    
sumai=sum(valores_cuadradosi)
potenciai=sumai/length
print('potencia de la señal impulso: ',potenciai)

SNRi=10*np.log10(potencia/potenciai)
print('SNRi ',SNRi)
# histograma
normalizacion=(valores-media)/desviacion_estandar
plt.hist(normalizacion,bins=10,edgecolor='black',density=True)
plt.show()

# signal
![image](https://github.com/user-attachments/assets/620dcbad-a741-4c09-8dec-0a2b5ecb3cf2)
# ruido gaussiano 
![image](https://github.com/user-attachments/assets/0eefd948-bba2-4818-8147-7b9f9bc7cf72)

# senal con ruido gaussiano
![image](https://github.com/user-attachments/assets/d1ef572a-78aa-4191-be90-65860af7c528)

# ruido artefacto 
![image](https://github.com/user-attachments/assets/2c9270ff-73eb-45af-901a-fde602ce7553)

# senal ruido artefacto
![image](https://github.com/user-attachments/assets/b2fcf4e3-0e24-4ebc-915a-dc5313c6c866)

# senal con ruido impulso 
![image](https://github.com/user-attachments/assets/92d93221-3abc-4093-a122-870187e51766)

# histograma
![image](https://github.com/user-attachments/assets/b8e58dbe-b689-44cd-9691-538713c837b2)

# valores estadisticos
La media es: 0.0032151599584023033
potencia de la señal:  0.0022070566422237427
Desviacion estandar:  0.04686917311693933
coeficiente de variacion:  14.57755561879722
relacion ruido  18.388740564236187
potencia de la señal ruido a:  1.5680000000000023e-05
SNRa  21.484674207532212
potencia de la señal impulso:  0.00049
SNRi  6.536173990731279
