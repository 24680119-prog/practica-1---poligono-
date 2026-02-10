## Apunte : Poligono en Blender con python 
Se crea un poligono es 2D calculado sus vertices con matematicas donde la figura se ubica en el plano XY manteniendo una altura constante al eje z para asegurar que sea un objeto en 2D, este programa trabaja directamente con mallas, que son la base de objetos geoemetricos en Blender. Cada malla se define por vertices y aristas las lineas que conectan puntos, en este codigo solo se utiliza vertices y aristas, sin caras para mantener la figura en dos dimensiones.
## Explicacion del codigo.

Estas lineas sirven para traer herramientas que el codigo necesita para funcionar, bpy es la libreria principal de Blender para Python sin esta libreria Python no podria comunicarse con blender.
Math es un modelo estandar de python que sirve para hacer calculos matematicos, esta funcion es necesaria para calcular la posicion de los vertices del poligono de forma correcta y simetrica.
```Python
import bpy
import math
```

Se define la funcion con def la cual cuenta con tres parametros: 
nombre que es el objeto , lados que seran los lados que tendra el poligono y radio que controla el tamaño del poligono.
```Python
def crear_poligono_2d(nombre, lados, radio):
```
Se crea la malla que es la estructura que define los vertices, aristas y caras.
```Python
     malla = bpy.data.meshes.new(nombre)
```
En esta linea de codigo se crea un objeto que se le asigna a la malla creada antes, donde la funcion recibe el nombre del objeto al numero de lados y radio.
```Python
     objeto = bpy.data.objects.new(nombre, malla)
```
Esta linea sirve para agregar el objeto a la escena Blender donde tenemos:

bpy: libreria que permite trabajar con blender desde python.

context:representa el estado actual de blender es decir la escena activa o la coleccion activa.

collection: en blender los obejtos se organizan es colecciones.

objects: lista de objetos que pertencen a la coleccion.
```Python
     bpy.context.collection.objects.link(objeto)
```

Se crean dos listas vacias en python: 

Lista vertice: Se guardara las coordenadas de cada punto del poligono.
```Python
     vertices = []
``` 
Lista arista: esta lista se va a usar para guardar las conexiones entre los vertices.
```Python
     aristas = []
```
Se calcula la posicion de cada vertice del poligono usando matematicas, se convierte coordenadas polares (angulo + radio ) en coordendas cartesinas (x,y) obteniendo uniformemente distribuidos en un circulo formando un poligono regular.

1.El ciclo for se repite tantas veces como los lados que tenga el poligono, cada repeticion crea un vertice.
2.Se calcula el angulo donde se colocara cada vertice del poligono.
3.Se realiza la conversion a coordendas cartesianas se convierte el angulo y el radio en una posicion (x, y) tambien se usa seno y coseno para posicionar el punto en el plano.
4.Por ultimo se guarda el vertice en la lista de vertices 
z= 0 nos sirve para que todos los puntos esten en el mismo plano.
```Python
     # Calculo de vertices usando coordenadas polares a cartesianas 
     for i in range(lados):
         angulo = 2 * math.pi * i / lados
         x = radio * math.cos(angulo)
         y = radio * math.sin(angulo)
         vertices.append((x, y, 0)) # Z = 0 para mantenerlo en 2D
 
```
En esta bloque se define las aristas es decir las lineas que conectan los vertices para formar el contorno del poligono, la cual el uso del operador modulo permite que el ultimo vertice se conecta con el primero, cerrando correctamente la figura.
```Python
     # Definir las conexiones (aristas) entre los vertices 
     for i in range(lados):
         aristas.append((i, (i + 1) % lados))
```
Blender recibe los datos de los vertices y aristas y se construye la malla para que el objeto exista visualmente.

En la prinera linea se construye la geometria de la malla a partir de los datos calculados previamente donde se le indica a blender las coordenadas de los vertices que formaran la figura y las aristas que conectan dichos vertices. El tercer parametro se deja vacio porque no se esta definiendo caras, lo que permite mantener la figura en dos dimensiones. 
Mientras que la instruccion malla.update() se encarga de actualizar la malla despues de haber cargado los vertices y las aristas. Su funcion es indicarle a Blender que procese los datos y aplique los cambios realizados, permitiendo que el objeto se muestre correctamente en la escena.
```Python
     # Cargar los datos en la malla
     malla.from_pydata(vertices, aristas, [])
     malla.update()

```
Este bloque de codigo se utiliza para limpiar la escena antes de crear el nuevo poligono. Primero se seleccionan todos los objetos presentes en la escena y posteriormente se eliminan con el fin de dejar el entorno limpio antes de crear el nuevo objeto.
```Python
# Limpiar la escena antes de empezar
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()
```
Se realiza la llamada a la llamada funcion crear_poligono_2d indicando los valores que se deben utilizar. Se especifica el nombre del objeto , el numero de lados y el radio, lo que provoca que el programa ejecute la funcion y genere el hexagono del radio 5.
crear_poligono_2d es el nombre de la funcion 
"poligono2D" es el nombre del obejto 
lados=6 Indica que el poligono tendere 6 lados 
radio=5 Define el tamaño del poligono 
```Python
# Llamada a la funcion: Un hexagono de radio 5 
crear_poligono_2d("Poligono2D", lados=6, radio=5)
```
## Codigo

```Python
import bpy
import math

def crear_poligono_2d(nombre, lados, radio):
     # Crear una nueva malla y un nuevo objeto
     malla = bpy.data.meshes.new(nombre)
     objeto = bpy.data.objects.new(nombre, malla)
     
     # Vincular el objeto a la escena actual
     bpy.context.collection.objects.link(objeto)
     
     vertices = []
     aristas = []
     
     # Calculo de vertices usando coordenadas polares a cartesianas 
     for i in range(lados):
         angulo = 2 * math.pi * i / lados
         x = radio * math.cos(angulo)
         y = radio * math.sin(angulo)
         vertices.append((x, y, 0)) # Z = 0 para mantenerlo en 2D
         
     # Definir las conexiones (aristas) entre los vertices 
     for i in range(lados):
         aristas.append((i, (i + 1) % lados))
         
     # Cargar los datos en la malla
     malla.from_pydata(vertices, aristas, [])
     malla.update()
 
# Limpiar la escena antes de empezar
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()
 
# Llamada a la funcion: Un hexagono de radio 5 
crear_poligono_2d("Poligono2D", lados=6, radio=5)
```
## hexagono
<img width="1663" height="932" alt="image" src="https://github.com/user-attachments/assets/39494596-8ccc-457a-8f24-c59d02cd305b" />







