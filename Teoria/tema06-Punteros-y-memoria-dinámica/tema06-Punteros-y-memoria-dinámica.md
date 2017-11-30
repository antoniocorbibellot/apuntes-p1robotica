
# Tema 6: Punteros y memoria dinámica

## Contenidos

- [1. Punteros](#1)
    - [1.1 Operadores para el manejo de punteros](#1-1)
    - [1.2 Punteros y Arrays](#1-2)
    - [1.3 Punteros a cadenas de caracteres](#1-3)
    - [1.4 Operaciones aritméticas con punteros](#1-4)
- [2. Manejo de memoria dinámica](#2)
    - [2.1. Memoria dinámica](#2-1)
    - [2.2. Funciones para gestionar la memoria dinámica: `malloc`, `calloc`, `realloc`, `free`](#2-2)
    - [2.3 Arrays dinámicos multidimensionales](#2-3)
- [3. Punteros y funciones](#3)


## <a name="1"/> 1. Punteros

Como hemos visto, la **memoria** del ordenador es el lugar donde se almacenan los datos y las instrucciones de un programa. Esta memoria es la memoria RAM, que es diferente a la memoria de almacenamiento como discos duros, etc. Está compuesta por un gran número de **celdas** (bytes) de información. A cada una de estas celdas se le asigna una **dirección de memoria**, que permite distinguir unas celdas de otras. De forma que, dada la dirección de memoria de una celda, se podrá obtener su valor actual y modificarlo (como ya hemos visto por ejemplo en el paso de parámetros por referencia).

Cuando se accede a una variable almacenada en memoria el compilador necesita saber:

- número de bytes que componen la variable
- dirección de memoria del byte inicial de la variable

Estos términos ya los hemos utilizado al definir una variable:

- la dirección de memoria se representa con el nombre de la variable (el compilador se encarga de sustituir el nombre por su dirección de memoria)
- El número de bytes se define con el tipo de datos de la variable (`int`, `char`...). El compilador se encarga de reservar los bytes necesarios.

Los **punteros** son un nuevo tipo de datos. La diferencia con el resto es que los que hemos visto hasta ahora almacenan datos y los punteros almacenan **direcciones de memoria**. Es decir, hacen referencia a otra zona de memoria donde se encuentran los datos. Se dice que un puntero *apunta* a un dato.

Características:

- Un **puntero** es una variable que almacena una dirección de memoria.
- Una variable contiene siempre un valor (de un tipo).
- Un puntero contiene la dirección de una variable que contiene un valor.
- El valor de un puntero es una dirección de memoria.
- Un puntero puede apuntar a una variable de cualquier tipo: tipos básicos, tipos definidos por el usuario, estructuras de datos, ¡incluso a funciones!
- Los punteros se pueden utilizar para referenciar y manipular estructuras de datos, y para referenciar bloques de memoria asignados dinámicamente

Sintaxis:

~~~c
tipo *nombre_variable;
~~~

- La variable declarada es un puntero al tipo de dato
especificado: `nombre_variable` almacenará la dirección de memoria en la cual se almacenará un dato de ese tipo.
- Con esa declaración se reserva memoria SÓLO para el puntero, NUNCA para la variable a la que apunta.

Ejemplos:

~~~c
int *entero; /* entero es un puntero a int */
float *res;   /* res es un puntero a float */
char *mensaje; /* mensaje es un puntero a char */
~~~

###<a name="1.1"/> 1.1 Operadores para el manejo de punteros

Existen dos operadores unarios para trabajar con punteros: `*` y `&`- `*puntero`: devuelve el contenido de la dirección de memoria apuntado por `puntero`- `&variable`: devuelve la dirección de memoria que referencia a `variable`

Ejemplo 1:

~~~c
int main()
{
    int a = 0; //Declaración de variable entera de tipo entero
    int *puntero; //Declaración de variable puntero de tipo entero
    puntero = &a; //Asignación de la dirección memoria de a

    printf("El valor de a es: %d. \nEl valor de *puntero es: %d. \n",a,*puntero);
    printf("La direccion de memoria de *puntero es: %p",puntero);

    return 0;
}
~~~

Ejemplo 2:

~~~c
int y, *yPtr; /* yPtr es puntero a entero */
y = 5;
yPtr = &y; /* yPtr toma la dirección de y */

printf("y: %d, yPtr: %p, &y: %p, &yPtr: %p, *yPtr: %d\n", y, yPtr, &y, &yPtr, *yPtr);

/* Salida por pantalla:
y: 5, yPtr: 0x7fff5d9829ec (1050), &y: 0x7fff5d9829ec (1050), &yPtr: 0x7fff5d9829e0 (1200), *yPtr: 5
*/
~~~

<img src="imagenes/puntero.png" width="500px"/>

- Se dice que una variable se refiere directamente a un valor y un puntero se refiere indirectamente a un valor.
- El puntero, al tener una dirección de memoria es como si apuntara a dicha dirección.
- Las direcciones de memoria de cada variable las asigna el sistema operativo y el programa no puede ni cambiarlas ni usar otras posiciones distintas a las asignadas por el S.O.

Ejemplo 3:

~~~c
void main() {
   int x, y, *py;

   printf ("\n- Introduzca un número: ");
   scanf("%i",&y);

   py = &y;
   x = *py + 10; /* Suma 10 con el contenido de la dirección py */

   printf("\n- Enteros: %d, %d y %d.", y, *py, x);
   printf("\n- Direcciones: %p y %p.", &y, py);
}

/* Salida por pantalla:
- Introduzca un número: 4
- Enteros: 4, 4 y 14.
- Direcciones: 0x7fff5afeb9e8 y 0x7fff5afeb9e8
*/
~~~

Ejemplo 4:

~~~c
int *punteroInt;
int valor = 8;

punteroInt = &valor;
printf("Puntero: dir: %p, valor: %d, referencia: %p\n", punteroInt, *punteroInt, &punteroInt);
printf("Variable: valor: %d, referencia: %p\n", valor, &valor);

/* Salida por pantalla:
Puntero: dir: 0x7fff523e19e4, valor: 8, referencia:0x7fff523e19e8
Variable: valor: 8, referencia: 0x7fff523e19e4
*/
~~~

Muchas de las funciones estándares de C trabajan con punteros, como es el caso del `scanf`. A `scanf` se le pasa la dirección de memoria del dato a leer.

~~~c
char a;
scanf ("%c",&a);
~~~

###<a name="1.2"/> 1.2 Punteros y arrays

En C los punteros y los arrays (vectores) están fuertemente relacionados, hasta el punto de que el nombre de un
vector es en sí mismo un puntero a la primera posición del vector (índice 0). Todas las operaciones que
utilizan vectores e índices pueden realizarse mediante punteros.

~~~c
int v[10];
~~~

![](imagenes/vector.png)

`v`: designa 10 posiciones consecutivas de memoria donde se pueden almacenar enteros.

~~~c
int *p;
~~~

`*p`: designa un puntero a entero, por lo tanto puede acceder a una posición de memoria. Sin embargo,
como se ha dicho antes `v` también puede considerarse como un puntero a entero, de tal forma que las
siguientes expresiones son equivalentes:

~~~c
p = v       ===>  p = &v[0]
x = *p      ===>  x = v[0]
*(v + 1)    ===>  v[1]
v + i       ===>  &v[i]
~~~


###<a name="1.3"/> 1.3 Punteros a cadenas de caracteres

Podemos hacer un array de caracteres usando punteros.

~~~c
char *nombre = "hola qué tal";//Es una cadena de 13 caracteres
printf("%s",nombre);
~~~

Sin embargo al tratarse de una constante de caracteres  no podemos modificarla despues de definir sus valores. Como por ejemplo no podemos remplazar un carácter, o leer un nuevo valor.

~~~c
//Error de ejecución:
strcpy(nombre, "hola");
~~~

Para poder modificar el valor de este puntero, éste tendría que apuntar a una dirección que no sea una constante, como un array.

~~~c
char nombre[] = "hola qué tal"; //declaramos un array de caracteres
char *puntero = nombre;//Asignamos al puntero el comienzo del array
printf("%s \nOtro nombre: ", puntero);
scanf("%s", puntero);
printf("%s\n",puntero);
~~~

Aunque hayamos modificado el nombre, la cantidad de caracteres esta limitada por el array original, se soluciona con memoria dinámica.


## <a name="2"/> 2. Manejo de memoria dinámica

###<a name="2.1"/> 2.1 Memoria dinámica

- La **memoria estática** es el espacio en memoria que se crea al declarar variables de cualquier tipo de dato. La memoria que estas variables ocupan no puede cambiarse durante la ejecución y tampoco puede ser liberada manualmente.
- La **memoria dinámica** es memoria que se reserva en tiempo de ejecución. Su principal ventaja frente a la estática, es que su tamaño puede variar durante la ejecución del programa. En C, el programador es encargado de liberar esta memoria cuando no la utilice más. El uso de memoria dinámica es necesario cuando a priori no conocemos el número de datos o elementos a tratar.


Definimos las **variables dinámicas** como zonas de memoria que pueden reservarse y liberarse durante el transcurso del programa. La única forma de acceder a su contenido es mediante una variable puntero que almacene la dirección de memoria correspondiente. Las variables dinámicas se guardan en la zona de memoria dinámica y se gestionan con las funciones de memoria dinámica (`malloc`, `calloc`, `realloc, `free`).

###<a name="2.2"/> 2.2 Funciones para gestionar la memoria dinámica

La biblioteca estándar de C proporciona las funciones `malloc`, `calloc`, `realloc` y `free` para el manejo de memoria dinámica. Estas funciones están definidas en el archivo de cabecera `stdlib.h`.

#### malloc

La función `malloc` reserva un bloque de memoria y devuelve un puntero al inicio de la misma.

Sintaxis:

~~~c
void *malloc(size_t size);
~~~
donde el parámetro `size` especifica el número de bytes a reservar. En caso de que no se pueda realizar la asignación, devuelve el valor nulo (definido en la macro NULL), lo que permite saber si hubo errores en la asignación de memoria.

~~~c
#include <stdlib.h>

int main() {
   int *vect1, n;

   printf("Número de elementos del vector: ");
   scanf("%d", &n);

   /* reservamos memoria para almacenar n enteros */
   vect1 = (int *) malloc(n * sizeof(int));

   /* Verificamos que la asignación se haya realizado correctamente */
   if (vect1  == NULL) {
   	/* Error al intentar reservar memoria */
   }
}

~~~

El operador `sizeof(tipo)` nos devuelve el número de *bytes* que ocupa el tipo de dato pasado como parámetro.

Uno de los usos más comunes de la memoria dinámica es la creación de vectores/matrices cuyo número de elementos se define en tiempo de ejecución.


#### calloc

La función `calloc` funciona de modo similar a `malloc, pero además de reservar memoria, inicializa a 0 la memoria reservada.

Sintaxis:

~~~c
void *calloc(size_t num, size_t size);
~~~
Devuelve un puntero a una zona de memoria en la que se reservan `num` elementos de `size` bytes. La memoria se inicializa a 0.

~~~c
#include <stdlib.h>

#define TAM = 25

int main() {
   float *vectorFloat;
   vectorFloat = (float *)calloc(TAM, sizeof(float));

   /* Verificamos que la asignación se haya realizado correctamente */
   if (vectorFloat  == NULL) {
      /* Error al intentar reservar memoria */
      ...
   }
}
~~~

#### realloc

La función `realloc` redimensiona el espacio asignado de forma dinámica anteriormente a un puntero

Sintaxis:

~~~c
void *realloc(void *ptr, size_t size);
~~~
Donde `ptr` es el puntero a redimensionar, y `size` el nuevo tamaño, en bytes, que tendrá. Si el puntero que se le pasa tiene el valor `NULL`, esta función actúa como `malloc`. Si la reasignación no se pudo hacer con éxito, devuelve un puntero nulo, dejando intacto el puntero que se pasa por parámetro

~~~c
#include <stdlib.h>

#define TAM = 20

int main() {
   float *vectorFloat;
   vectorFloat = (float *)malloc(sizeof(float) * TAM);

   vectorFloat = (float *)realloc(vectorFloat, sizeof(float)*50);
}
~~~

Cuando se redimensiona la memoria con `realloc`, si el nuevo tamaño es mayor que el anterior, se conservan todos los valores originales, quedando los bytes restantes sin inicializar. Si el nuevo tamaño es menor, se conservan los valores de los primeros size bytes. Los restantes también se dejan intactos, pero no son parte del bloque que devuelve la función.

#### free

La función `free` sirve para liberar memoria que se asignó dinámicamente. Requiere como parámetro la dirección de memoria inicial de una zona de memoria reservada en la memoria dinámica (tal cual se reservó con `malloc` o `realloc`). La función `free`se encarga de marcar como libre esta memoria. Si el puntero es nulo, `free` no hace nada.

Además existe la función `cfree`, que sirve para liberar memoria de los elementos que han sido reservados con `calloc`.

Sintaxis:

~~~c
void free(void *ptr);
void cfree (void *ptr);
~~~
- Libera la memoria reservada para el puntero `ptr`- No se ejecuta automáticamente cuando se acaba el ámbito en el que se definió el puntero- Las llamadas a `free` han de ser explícitas


##### `NULL`

- `NULL` hace referencia a una dirección de memoria nula, no válida- Lo utilizamos cuando queremos indicar que un puntero no tiene memoria reservada o la que tenía ya ha sido liberada por medio de una llamada a `free()`
- `NULL` es también el valor devuelto por funciones que han de devolver un puntero cuando no pueden realizar su labor.
- `[m|c|re]alloc` devuelven `NULL` cuando no hay memoria disponible

~~~c
#include <stdlib.h>

#define TAM = 20

int main() {
   float *vectorFloat;
   vectorFloat = (float *)malloc(sizeof(float) * TAM);
   ...
   free(vectorFloat);
   vectorFloat = NULL;
}
~~~

###<a name="2.3"/> 2.3 Arrays dinámicos multidimensionales

Como hemos visto en el apartado anterior, el lenguaje C posee la capacidad de poder asignar nuevos espacios de memoria en tiempo de ejecución.

Para definir un array multidimensional de manera dinámica utilizaremos punteros a punteros.

~~~c
int ***tabla3D  // 3 dimensiones
~~~

Se interpreta como un puntero a un tipo que es un puntero que a su vez es un puntero a un entero

~~~c
#include<stdlib.h>

#define X 5
#define Y 7
#define Z 20

void rellenarTabla3D(int ***);

int main() {
   int ***tabla3D;  //será de 5x7x20

   int dim1, dim2, dim3;

   // Reservamos la memoria
   tabla3D = (int***)malloc(sizeof(int**) * X);

   for(dim1 = 0; dim1 < X; dim1++){
      tabla3D[dim1] = (int**)malloc(sizeof(int*) * Y);
      for(dim2 = 0; dim2 < Y; dim2++){
         tabla3D[dim1][dim2] = (int*)malloc(sizeof(int) * Z);
         for(dim3 = 0; dim3 < Z; dim3++){
            tabla3D[dim1][dim2][dim3] = 0; /*inicialización*/
         }
      }
   }

   // Rellenamos sus valores en una función
   rellenarTabla3D(tabla3D);
   printf("%d", tabla3D[2][3][1]); // comprobamos una posición al azar

   //Liberamos la memoria de la tabla3D
   for(dim1 = 0; dim1 < X; dim1++){
      for(dim2 = 0; dim2 < Y; dim2++){
         free(tabla3D[dim1][dim2]);
      }
      free(tabla3D[dim1]);
   }
   free(tabla3D);
   tabla3D = NULL;
}

void rellenarTabla3D(int ***tabla) {
  int dim1, dim2, dim3;

  for(dim1 = 0; dim1 < X; dim1++){
    for(dim2 = 0; dim2 < Y; dim2++){
      for(dim3 = 0; dim3 < Z; dim3++){
        tabla[dim1][dim2][dim3] = 100;
      }
    }
  }
}

~~~

## <a name="3"/> 3. Punteros y funciones

Uno de los usos de los punteros es el paso de
parámetros por referencia a una función.
Recordemos que los parámetros de una función se
pueden pasar por valor o por referencia.

En C todos los parámetros de las funciones se pasan por valor. Para simular un
paso de parámetro por referencia en C, lo que se
hace es pasar un puntero al objeto que se pasa. Así,
la función tiene acceso no sólo al valor del
parámetro sino también a su situación en memoria,
lo que permite su modificación.

Ya vimos que para que un parámetro de una función pueda ser modificado, ha de pasarse por referencia, y en
C eso sólo es posible pasando la dirección de la variable en lugar de la propia variable.
Si se pasa la dirección de una variable, la función puede modificar el contenido de esa posición (no así
la propia dirección, que es una copia)

### Arrays y matrices como parámetro

#### Paso de parámetros por valor

~~~c
void funcion(char *cad){
  printf("Función. Valor: %s, Dirección: %p, Referencia: %p\n", cad, cad, &cad);
}


int main() {
  char *cadena = {"Hola mundo"};

  printf("Main. Valor: %s, Dirección: %p, Referencia: %p\n", cadena, cadena, &cadena);
  funcion(cadena);
}

/* Salida por pantalla:
Main. Valor: Hola mundo, Dirección: 0x1046e0f77, Referencia: 0x7fff5b51f9e8
Función. Valor: Hola mundo, Dirección: 0x1046e0f77, Referencia: 0x7fff5b51f9c8
*/
~~~

Como vemos, en `funcion` hay una referencia a los elementos del array pero no al puntero en sí, que es una copia. Es decir, no podemos modificar dinámicamente su memoria porque no tenemos una referencia a la misma.

### Paso de parámetros por referencia

~~~c
void funcionRef(char **cad) {
  printf("Función. Valor: %s, Dirección: %p, Referencia: %p\n", *cad, *cad, cad);
}

int main() {
   char *cadena = {"Hola mundo"};

   printf("Main. Valor: %s, Dirección: %p, Referencia: %p\n", cadena, cadena, &cadena);
   funcionRef(&cadena);
}

/* Salida por pantalla:
Main. Valor: Hola mundo, Dirección: 0x10861af77, Referencia: 0x7fff575e59e8
Función. Valor: Hola mundo, Dirección: 0x10861af77, Referencia: 0x7fff575e59e8
*/
~~~

Ahora tenemos referencia a los datos y al propio puntero con lo que podemos modificar la memoria que tiene reservada.

Ejemplo con vector:

~~~c
#define TAM 20

void creaVector(int **);

int main() {
   int *vector;

   // Reservamos la memoria
   creaVector(&vector);
   vector[0] = 1;  //Rellenamos al azar
   vector[2] = 2;
   printf("Pos 2: %d\n", vector[2]); //Comprobamos

   // Liberamos memoria
   free(vector);
   vector = NULL;
}

void creaVector(int **v) {
   *v = (int*)malloc(sizeof(int) * TAM);
}
~~~

~~~c
// Modificamos el ejemplo de la tabla3D
// Ahora pasamos la tabla3D como parámetro por referencia
// a una función que reserva la memoria necesaria para su creación

int main() {
   int ***tabla3D;  

   creaMatriz(&tabla3D);
   .....
}

void creaMatriz(int ****tabla3D) {
   int dim1, dim2, dim3;

   // Reservamos la memoria
   (*tabla3D) = (int***)malloc(sizeof(int**) * X);

   for(dim1 = 0; dim1 < X; dim1++){
      (*tabla3D)[dim1] = (int**)malloc(sizeof(int*) * Y);
      for(dim2 = 0; dim2 < Y; dim2++){
         (*tabla3D)[dim1][dim2] = (int*)malloc(sizeof(int) * Z);
         for(dim3 = 0; dim3 < Z; dim3++){
            (*tabla3D)[dim1][dim2][dim3] = 0; /*inicialización*/
         }
      }
   }
}

~~~

### Devolución de punteros

Una función también puede devolver un tipo de datos
puntero. La función se declara así:

~~~c
tipo* funcion( argumentos );
~~~

Este tipo de funciones se suelen usar para reservar
memoria o crear elementos en estructuras
dinámicas de datos.

Ejemplo:

~~~c
// Modificamos el ejemplo de la tabla3D
// Ahora la memoria de la tabla3D se reserva en una función
// y se devuelve con return

int*** creaTabla() {
   int ***tabla;
   int dim1, dim2, dim3;

   tabla = (int***)malloc(sizeof(int**) * X);

   for(dim1 = 0; dim1 < X; dim1++){
     tabla[dim1] = (int**)malloc(sizeof(int*) * Y);
     for(dim2 = 0; dim2 < Y; dim2++){
        tabla[dim1][dim2] = (int*)malloc(sizeof(int) * Z);
        for(dim3 = 0; dim3 < Z; dim3++){
           tabla[dim1][dim2][dim3] = 0; /*inicialización*/
         }
      }
   }
   return tabla;
}

int main() {
   int ***tabla3D;  //será de 5x7x20
   int dim1, dim2, dim3;

   tabla3D = creaTabla();

   ...
}
~~~


### Consejos para el manejo de memoria dinámica

- Siempre que se reserve memoria de forma dinámica con `malloc`, `realloc` o `calloc`, se debe verificar que no haya habido errores (verificando que el puntero no sea NULL).
- Se recomienda que después de liberar un puntero siempre se establezca su valor a `NULL`.


## Ejemplo completo con `typedef`

Como hemos visto en temas anteriores, definir tipos propios con `typedef`resulta muy útil. En el caso concreto de punteros a arrays, facilita bastante la tarea porque se simplifica la definición.

Veamos el ejemplo de la `tabla3D` usando `typedef`:

~~~c
#define X 5
#define Y 7
#define Z 20

typedef int*** TTabla3D;

TTabla3D crearTabla3D();
void freeTabla3D(TTabla3D);
void rellenarTabla3D(TTabla3D);

int main() {
   TTabla3D tabla3D;

   tabla3D = crearTabla3D();

   if(tabla3D) { // Comprobamos que malloc no haya devuelto NULL
      rellenarTabla3D(tabla3D);
      printf("%d\n", tabla3D[2][3][1]); // comprobamos una posición al azar
      freeTabla3D(tabla3D);
      tabla3D = NULL;
      // Si queremos poner tabla3D = NULL dentro de la función
      // freeTabla3D, tenemos que pasar el puntero tabla3D por
      // referencia para poder modificarlo
   }
}

TTabla3D crearTabla3D() {
   TTabla3D tabla3D;  //será de 5x7x20
   int dim1, dim2, dim3;

   tabla3D = (int***)malloc(sizeof(int**) * X);

   for(dim1 = 0; dim1 < X; dim1++){
      tabla3D[dim1] = (int**)malloc(sizeof(int*) * Y);
      for(dim2 = 0; dim2 < Y; dim2++){
         tabla3D[dim1][dim2] = (int*)malloc(sizeof(int) * Z);
         for(dim3 = 0; dim3 < Z; dim3++){
            tabla3D[dim1][dim2][dim3] = 0; /*inicialización*/
         }
      }
   }

   return tabla3D;
}

void freeTabla3D(TTabla3D tabla) {
   int dim1, dim2, dim3;

   for(dim1 = 0; dim1 < X; dim1++){
      for(dim2 = 0; dim2 < Y; dim2++){
         free(tabla[dim1][dim2]);
      }
      free(tabla[dim1]);
   }
   free(tabla);
}

void rellenarTabla3D(TTabla3D tabla) {
   int dim1, dim2, dim3;

   for(dim1 = 0; dim1 < X; dim1++){
      for(dim2 = 0; dim2 < Y; dim2++){
         for(dim3 = 0; dim3 < Z; dim3++){
            tabla[dim1][dim2][dim3] = 100;
         }
      }
   }
}
~~~

La función `crearTabla3D` también se puede hacer pasando la tabla3D por referencia para poder reservar la memoria dinámicamente:

~~~c
int main() {
   ...
   crear2Tabla3D(&tabla3D);
   ...
}

void crear2Tabla3D(TTabla3D *tabla3D) {
   int dim1, dim2, dim3;

   *tabla3D = (int***)malloc(sizeof(int**) * X);

   for(dim1 = 0; dim1 < X; dim1++){
      (*tabla3D)[dim1] = (int**)malloc(sizeof(int*) * Y);
      for(dim2 = 0; dim2 < Y; dim2++){
         (*tabla3D)[dim1][dim2] = (int*)malloc(sizeof(int) * Z);
         for(dim3 = 0; dim3 < Z; dim3++){
            (*tabla3D)[dim1][dim2][dim3] = 0; /*inicialización*/
         }
      }
   }
}
~~~
----

Programación 1, Grado de Robótica, curso 2017-18  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Cristina Pomares Puig
