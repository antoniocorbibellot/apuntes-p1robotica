
# Tema 5: Datos estructurados: Arrays

## Contenidos 

- [1. Tipos de datos estructurados](#1)
- [2. Tipo Array](#2)
	- [2-1. Definición](#2-1)
	- [2-2. Arrays y funciones](#2-2)
- [3. Arrays multidimensionales](#3)
- [4. Cadenas de caracteres](#4)
 

## <a name="1"/> 1. Tipos de datos estructurados

A partir de los tipos de datos simples que hemos visto, se pueden definir en C otros tipos de datos compuestos por colecciones o agrupaciones de elementos de tipos simples. 
Los tipos estructurados o compuestos pueden almacenar más de un elemento (valor) a la vez. Se dividen en:

- **Arrays**: todos los elementos que almacena una variable de tipo array deben ser del **mismo tipo**. Pueden ser:
	- Unidimensionales
	- Multidimensionales
	- Cadenas de caracteres
- **Registros** o estructuras: una variable de tipo registro puede almacenar elementos de**distinto tipo**. En C, se utiliza el tipo `struct`equivalente al `record` de otros lenguajes.  

## <a name="2"/> 2. Tipo Array  

Es una estructura de datos que contiene una colección de datos finita, homogénea y ordenada de elementos, y se almacena en posiciones de memoria contiguas.- **finita**: debe determinarse cuál será el número máximo de elementos que podrán almacenarse en el array- **homogénea**: todos los elementos deben ser del mismo tipo- **ordenada**: se puede determinar cuál es el n-ésimo elemento del array   

A un elemento específico de un array se accede mediante un **índice**, que siempre empieza en la posición 0 (la primera posición del array). La última posición tendrá como índice el número de elementos del array menos uno. 

###<a name="2-1"/> 2.1  Definición de un array

**Sintaxis** para definir un array **unidimensional** en C:

~~~c
tipoDato nombreArray[dimension];
~~~

donde `tipoDato`representa el tipo de los elementos que constituyen el array, `nombreArray`el nombre de la variable utilizada para el array y `dimension`el número de elementos del array. Por ejemplo:

~~~c
int numeros[10];
~~~

define un array llamado `numeros` que está formado por 10 elementos de tipo `int`. A cada elemento se acccede mediante un índice entre 0 y `dimension-1` (0 y 9 en este caso).

**Sintaxis** para acceder a un elemento del array:

~~~c
array[indice]
~~~

Así, por ejemplo, `numeros[0]` representa el primer elemento del array, y `numeros[6]` el séptimo elemento. Hay que tener cuidado y no utilizar valores de índices fuera del rango ya que provocaría errores en la ejecución de nuestro programa.

Otro ejemplo:

<img src="imagenes/array.png" width="500px"/>

- El tipo del valor de la variable `a` es un entero.
- Cada elemento del array `b` (b[0], ...,b[9]) es un entero y puede
ser usado en cualquier contexto donde es usado un entero
- Si nos referimos a `b`sin corchetes, obtenemos la dirección de memoria donde empieza el array (veremos los punteros más adelante)

#### Inicialización de un Array

- Si se conocen los valores que toman las componentes del array al definirlo, podemos definir y asignar valores simultáneamente. Un array se puede inicializar en su declaración utilizando llaves {}:

~~~c
int vector[5] = {10, 20, 30, 40, 50}; 
~~~	
Los valores se asignan uno a uno consecutivamente:

<img src="imagenes/array2.png" width="300px"/>

- Tamaño automático: si no especificamos el tamaño del array, el compilador cuenta el número de elementos de la inicialización y ése es el tamaño del array

~~~c
int vectorB[] = {11, 23, 3, 10}; 
~~~

- Inicialización incompleta:  

~~~c
// Se inicializan sólo los 4 primeros elementos
int vectorC[10] = {7, 7, 7, 7}; 
~~~
~~~c
// Si hay más valores, da ERROR:
int vectorD[5]={1, 20, 3, 40, 5, 60};
~~~

- También podemos inicializar un array haciendo que el usuario introduzca los datos por teclado:

~~~c
void inicializarArray(float calificaciones[]);

int main () {
    float calificaciones[50];
    inicializarArray(calificaciones);
}

// función para inicializar el array
void inicializarArray(float calificaciones[]) {
    int i;
    for (i = 0;i < 50 ;i++) {
        printf("Introduce la calificación %d: ", i);
        scanf("%f", &calificaciones[i]);
	}
}~~~

###<a name="2-2"/> 2.2   Arrays y funciones

- En lenguaje C, el paso de parámetros de los arrays siempre es por **referencia**.- En lenguaje C, las funciones no pueden devolver un tipo array. Para modificar un array, ha de ser pasado como parámetro (siempre es por referencia y por tanto se modificará el array original)
- Los siguientes prototipos de funciones son equivalentes:

~~~c
float maximo(float arr[], int tama);
float maximo(float *arr, int tama);
~~~

- Es imposible que la función determine el tamaño del array.
- Si se necesita el tamaño en la función, se tiene que pasar como argumento
- Si el tamaño es fijo, se puede utilizar una constante#### Ejemplos

Ejemplo 1:

~~~c
//Función que calcula la media de notas de alumnosfloat calculaMedia(float a[], int len) {    int i;    float suma;    suma = 0.0;    for (i = 0; i < len; i++)        suma = suma + a[i];    // suponemos len > 0    return(suma / len); 
}
~~~

Ejemplo 2:

~~~c
void printArray(double array[], int len){
    int i;

    for (i = 0; i < len; i++)
        printf("[%.2f]\n", array[i]);
}
~~~

Ejemplo 3:

~~~c
/* Dado un array de enteros, mover todos sus elementos una posición a la derecha. 
El desplazamiento será circular, es decir, el último elemento pasará a ser el primero*/
void moverEnCircular (int v[]) {    int i, ult;    // guardar el valor de la última posición del array    ult = v[LMAX-1];    // mover todos los elementos una posición a la derecha, excepto el último    for (i = LMAX-1; i > 0; i--) 
        v[i] = v[i-1];    // actualizar la primera posición con el valor que teníamos en la última    v[0] = ult; 
}
~~~

Ejemplo 4:

~~~c
/* Dado un array de enteros, devolver el mayor valor, el número de ocurrencias de dicho valor,
y la posición de la primera y última aparición en la que se encuentra almacenada*/
void ocurrencias(int v[], int *mayor, int *num_ocur, int *pos_pri, int *pos_ult) {
     int i;

    *mayor = v[0]; // inicialmente el número mayor será el que está en la primera posición num_ocur = 1;
    *pos_pri = 0;
    *pos_ult = 0;

    // recorrer el array: desde la segunda posición hasta la posición final (constante LMAX)
    for (i=1; i < LMAX; i++) {
        if (v[i] > *mayor) { // encontramos un nuevo número mayor
            *mayor = v[i];
            *num_ocur = 1;
            *pos_pri = i;
            *pos_ult = i;
        }
        else if (v[i] == *mayor) {
        // encontramos una nueva ocurrencia del número mayor hasta el momento
            *num_ocur = *num_ocur +1;
            *pos_ult = i;
        }
    }
 }

int main()
{
    int mayor, num_ocur, pos_pri, pos_ult;
    int v[] = {1,3,5,1,3,5};

    ocurrencias(v, &mayor, &num_ocur, &pos_pri, &pos_ult);

}
~~~

## <a name="3"/> 3. Arrays multidimensionales

Hemos visto los arrays unidimensionales, cuyos elementos se almacenan en posiciones contiguas de memoria, a cada una de las cuales se puede acceder directamente mediante un índice.

A los arrays de más de una dimensión se les denomina **multidimensionales**.

Sintaxis:

~~~c
tipo_elemento nombre_array [a][b][c]...[z];
~~~

- Una matriz es un array de 2 dimensiones, es decir un array
unidimensional de arrays unidimensionales.
- En general, un array de dimensión n es un array unidimensional de
arrays de dimensión n–1.

Ejemplos:

~~~c
// Array bidimensional de 6*10 enteros (matriz de 60 elems):
int matriz[6][10]; 
//Array tridimensional de 3*2*5 reales (cubo de 30 elems):
float cubo[3][2][5]; 
~~~

#### Almacenamiento en memoria arrays multidimensionales

Los elementos se almacenan contiguos en memoria:

<img src="imagenes/array3.png" width="400px"/>

#### Inicialización arrays multidimensionales

- Si se conocen todos los elementos al declarar el array, hay dos modos de escribir la lista de inicializaciones:
	
	-  Todos los valores seguidos:
	
	~~~c
	int matriz[2][3]={0,1,2,10,11,12};
	~~~
	
	- Por partes (mejor, mayor claridad):

	~~~c
	int matriz[2][3] = {{ 0, 1, 2 },
						{ 10, 11, 12 }};
	~~~

- Si no se conocen, es útil recorrer el array (un bucle por cada dimensión) e ir asignando los valores

#### Arrays multidimensionales y funciones

Un array multidimensional, al igual que uno unidimensional, también puede pasarse como argumento a una función.

- Se pasa la dirección del primer elemento del array.
- Ese primer elemento es otro array (de 1 dimensión menos).

- Se puede usar el siguiente prototipo:

~~~c
// Recibe una matriz en la cual cada fila
// tiene 10 enteros y con cualquier número de filas
void func(int mat[][10]); 

// También es CORRECTO: 
void func(int (*mat)[10]);

// Es INCORRECTO: 
void func1(int **mat);
~~~

#### Acceso a los elementos (por índices)

- Para identificar un elemento de un array multidimensional, se
debe dar un índice para cada dimensión, en el mismo orden
que en la declaración.
- Cada índice se encierra en sus propios corchetes

<img src="imagenes/array4.png" width="450px"/>

Ejemplo 1:

~~~c
// Array bidimensional con los números del 1 al 12

void main(){
    int num[3][4], i,j;

    for(i = 0;i < 3; i++)
        for(j = 0;j < 4; j++)
            num[i][j] = (i * 4) + j + 1;
}
~~~

<img src="imagenes/array5.png" width="150px"/>

Ejemplo 2:

~~~c
//Programa que muestra los valores almacenados en
//un array bidimensional 2 por 3 

#define FILAS 2
#define COLUMNAS 3

int main() {
    int matriz[FILAS][COLUMNAS]= {{1,2,3},{4,5,6}};
    int fil,col;

    for(fil=0;fil<FILAS;fil++)
        for(col=0;col<COLUMNAS;col++)
            printf("El valor de [%d][%d] es %d\n", fil, col, matriz[fil][col]);
}

// Salida por pantalla:
/*
El valor de [0][0] es 1
El valor de [0][1] es 2
El valor de [0][2] es 3
El valor de [1][0] es 4
El valor de [1][1] es 5
El valor de [1][2] es 6
*/
~~~

Ejemplo 3: 

~~~c
// Suma de matrices

#define FILAS 5
#define COLUMNAS 7

/* Función que rellena los
valores de una matriz */
void rellena(int m[][COLUMNAS]) {
    int i,j;

    for(i = 0; i < FILAS; i++)
        for(j = 0; j < COLUMNAS; j++) {
            printf("Valor [%d,%d]: ",
i,j);
            scanf("%d", &m[i][j]);
        }
}
/* Suma dos matrices */
void suma(int m1[][COLUMNAS], int m2[][COLUMNAS], int r[][COLUMNAS]) {
    int i, j;

    for(i = 0; i < FILAS; i++)
        for( j = 0; j < COLUMNAS; j++) {
            r[i][j] = m1[i][j] + m2[i][j];
    }
}

/* Muestra una matriz*/
void imprime(int m[][COLUMNAS]) {
    int i,j;

    for(i = 0; i < FILAS; i++) {
        for(j = 0; j < COLUMNAS; j++)
            printf(" %d", m[i][j]);
        printf("\n");
    }
}
int main() {
    int a[FILAS][COLUMNAS],
        b[FILAS][COLUMNAS],
        c[FILAS][COLUMNAS];

    rellena(a);
    rellena(b);
    suma(a,b,c);
    imprime(c);
}

~~~


## <a name="4"/> 4. Cadenas de caracteres 
              
- En C no existe un tipo específico para trabajar concadenas de caracteres (en otros lenguajes es el tipo `String`).
- En C, una cadena de caracteres es un array unidimensional de tipo
`char`.
- El último carácter visible de la cadena debe estar seguido del
carácter nulo que se representa por `'\0'`. Este carácter marca el final de la cadena de caracteres
- Existen librerías con funciones para realizar la mayor
parte de las operaciones básicas sobre cadenas.

Ejemplos:

~~~c
char cadena[20];
char *cadena="Hola";
char cadena[]="Adios";
~~~                                                                                                             

En las dos últimas declaraciones el tamaño del array será el
número de caracteres dado en la inicialización más 1 (que corresponde
al carácter ‘\0’).

##### `printf` y `scanf` con cadenas
Las funciones `printf` y `scanf` tratan el `'\0'` automáticamente con `%s`

~~~cprintf("%s\n",cadena); 
scanf("%s", cadena);scanf(%[^\n], cadena); //lee la entrada estandar hasta encontrar \n, sin detenerse en espacios
~~~

##### Librería `string.h`

Para trabajar con cadenas de caracteres, en C tenemos la librería `string.h`:

~~~c#include <string.h>
~~~

Algunas de las funciones que incluye:

- **`int strlen(const char *s);`** Devuelve el tamaño de la cadena antes de `'\0'`- **`char *strcpy(char *dest, const char *src);`**Copia la cadena origen src en la cadena destino dest.- **`char *strcat(char *dest, const char *src);`**Concatena la cadena origen src al final de la cadena destino dest- **`int strcmp(const char *s1, const char *s2);`**
Compara dos cadenas. Devuelve 0 en caso de que sean iguales. <0 si la primera cadena es menor y >0 si la primera cadena es mayor. Orden lexicográfico

`strcpy` y `strcat` devuelven un puntero a la cadena
resultante. No comprueban si el resultado cabe en la cadena final.

#### Ejemplos con cadenas de caracteres

Ejemplo 1:

~~~c
// devuelve la longitud de una cadena de caracteres// esta función es equivalente a strlen()
int longitudCadena(char cad[]){    int len;
	    len = 0;    while (cad[len] != '\0')        len++;
		    return(len); 
}
~~~

Ejemplo 2:

~~~c
int main() {
    char cad1[10], cad2[10];

    strcpy(cad1, "Hola "); /* Se guardan en cad1 6
                            caracteres (incluido el ‘\0’)*/
    strcpy(cad2, "y adios"); /* Se guardan en cad2 8
                            caracteres (incluido el ‘\0’) */
    strcat(cad1, cad2); /* Se concatena la cadena cad2 al
                        final de cad1. Observa que se intentan guardar
                        en cad1 más caracteres que la longitud de la
                        cadena. El compilador no da error y se escribe
                        a continuación de cad1, en memoria que no
                        pertenece a cad1. */
    printf("cad1: %s\n", cad1); /* ERROR. Se intenta escribir cad1 hasta que encuentre un
                          ‘\0’. El resultado es impredecible: puede
                            escribir "Hola y adios" a pesar de que en total
                            son más de 10 caracteres, puede escribir otra
                            cosa o puede quedarse colgado el terminal. */

}
~~~

Para solucionar el problema anterior (en `cad1`no cabe la cadena), podemos comprobar si se puede concatenar antes de hacerlo:

~~~c
// comprobar si se puede concatenar antes de hacerlo:
if(strlen(cad1) + strlen(cad2) < 10) 
    strcat(cad1,cad2);
~~~

Ejemplo 3:

~~~c
int main() {
    int n;
    char nombre1[20], nombre2[20];

    printf("Teclea el primer nombre: ");
    scanf("%s", nombre1);
    printf("Teclea el segundo nombre: ");
    scanf("%s", nombre2);

    n = strcmp(nombre1,nombre2);
    if(n == 0)
        printf("Nombres iguales\n");
    else if (n>0)
        printf("Primer nombre mayor que el segundo\n");
    else
        printf("Primer nombre menor que el segundo\n");
}
~~~

#### Funciones relacionadas con cadenas de caracteres

Algunas funciones de conversión incluidas en la librería `stdlib.h`

- **`double atof(char *s)`**: convierte la cadena `s` a `double`

~~~c
char numero[11] = "123.456789";
printf( "Convertimos la cadena \"%s\" en un float: %f\n", numPtr, atof(numero) );
~~~

- **`int atoi(char *s)`**: convierte la cadena `s` a `int`

~~~c
int num;
num = atoi("123");
printf("El int es: %d\n", num);
~~~

- **`long atol(char *s)`**: convierte la cadena `s` a `long int`


~~~c
char numero[11] = "1234567890";
printf( "Convertimos la cadena \"%s\" en un long int: %u\n", numPtr, atol(numero) );
~~~

- **`int sprintf(char *s,const char *f [,argumento,... ])`**: escribe en `s` la cadena de caracteres especificada en `f` (formato). El resto de parámetros es exactamente igual que en printf().

~~~c
int numero = 123;
char cadena[4]; 
sprintf(cadena, "%d", numero);
printf("%s\n", cadena); // imprime "123" 
~~~

### Ejemplos de arrays utilizando `typedef`


1. Escribe un programa completo que lea un vector de enteros positivos e imprima el número mayor.Para la realización del programa utilizaremos tres funciones, una para leer el vector, otra para imprimir el vector por pantalla y otra para encontrar el elemento mayor. 

~~~c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define TAM 10
typedef int TVector[TAM];

void leerVector(TVector vector);
void imprimirVector(TVector vector);
int mayorVector(TVector vector);

int main() {
   TVector vector;
   int mayor;

   leerVector(vector);
   imprimirVector(vector);
   mayor = mayorVector(vector);

   printf("El elemento mayor del vector es : %d\n",mayor);
}
//Función para leer el contenido del vector.
//Leeremos números enteros positivos
void leerVector(TVector vector)
{
    int i;
    i = 0;

    do {
      printf("Introduce el numero de la posición %d: ",i);
      scanf("%d", &vector[i]);
      if (vector[i] < 0)
         printf("Número incorrecto\n");
      else
         i++; // Incrementamos contador
      } while (i < TAM);
}


//Imprime por pantalla los elementos del vector.
void imprimirVector(TVector vector)
{
   int i;

   printf("Valores del vector: \n");
   for(i = 0; i < TAM; i++)
      printf("%d ",vector[i]);
   printf("\n");
}

//Función que devuelve el mayor número del vector.
int mayorVector(TVector vector)
{
   int i, mayor;
   mayor = vector[0]; // Al principio el mayor será el primer elemento.

   for(i = 1; i < TAM; i++)
      if(vector[i] > mayor) 
         mayor = vector[i];
         
   return mayor; 
}
~~~

2. Escribe un programa que pida una cadena de caracteres (de máximo 15 caracteres) y devuelva la cadena escrita al revés. 

~~~c
#include <stdio.h>
#include <string.h>

#define TAM 15

typedef char TCadena[TAM];

int main(){
   TCadena palabra, palabra_reves;
   int i, cont, longitud_palabra;

   printf("Introduzca una palabra: ");
   scanf("%s", palabra);

   cont = 0;
   longitud_palabra = strlen(palabra);

   for(i = longitud_palabra-1; i>=0; i--) {
      palabra_reves[cont] = palabra[i];
      cont++;
   }

   palabra_reves[cont] = '\0';
   printf("Palabra escrita al revés: %s\n", palabra_reves);
}
~~~

3. Escribe un programa que lea los datos de un array bidimensional o matriz de 5 filas por 4 columnas y luego imprima esta matriz por pantalla.

~~~c
#define FILS 5
#define COLS 4

typedef int TMatriz[FILS][COLS];

void leerMatriz(TMatriz matriz);
void escribirMatriz(TMatriz matriz);

int main() {
   TMatriz matriz;

   leerMatriz(matriz);
   escribirMatriz(matriz);
}

void leerMatriz(TMatriz matriz) {
   int i, j;

   for(i = 0; i< FILS; i++)
      for(j = 0; j< COLS; j++) {
         printf("Introduzca el elemento (%d,%d): ", i, j);
         scanf("%d", &matriz[i][j]);
      }
}

void escribirMatriz(TMatriz matriz) {
   int i, j;

   for(i = 0; i< FILS; i++) {
      for(j = 0; j< COLS; j++) {
         // %5d formatea rellenando con espacios a tam 5
         printf("%5d", matriz[i][j]);
      }
      printf("\n");
   }
}
~~~

### Ejercicios propuestos


1. Define una función que reciba una cadena de caracteres y compruebe si se trata de un palíndromo.2. Define una función que reciba dos parámetros: una cadena de caracteres y un carácter. La función realizará la búsqueda del carácter en la cadena y nos devolverá la posición de la primera ocurrencia del carácter en la cadena o -1 en caso de que no lo encuentre.3. Define una función similar a la del ejercicio anterior pero que devuelva la posición de la última ocurrencia del carácter.

----

Programación 1, Grado de Robótica, curso 2017-18  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Cristina Pomares Puig
