
# Tema 8: Entrada / salida

## Contenidos

- [1. Introducción](#1)
    - [1.1 Ficheros](#1-1)
    - [1.2 Streams (flujos) de E/S](#1-2)
- [2. Manejo de ficheros](#2)
    - [2.1 Abrir el fichero](#2-1)
    - [2.2 Cerrar el fichero](#2-2)
    - [2.3 Leer y escribir en un fichero fichero](#2-3)
- [3. Parámetros a `main` desde línea de comandos](#3)


## <a name="1"/> 1. Introducción

C ofrece un conjunto de funciones para realizar operaciones de entrada y salida (E/S) con las cuales puedes leer y escribir cualquier tipo de fichero.

###<a name="1.1"/> 1.1 Ficheros

- Toda la información manejada hasta ahora se pierde cuando termina la ejecución de un programa, tanto la introducida como la generada.- Si tenemos que manejar una gran cantidad de información (un número elevado de variables, valores, etc.) no parece razonable definir esa información en el código.- Además, puede que la información a tratar no la tengamos, sino que queramos procesar información que nos suministran almacenada.- Un fichero almacena de manera permanente la información, usando la memoria permanente (disco duro) del ordenador.- Para C todo en el ordenador es un fichero: teclado, impresora, cualquier otro dispositivo conectado al ordenador.


###<a name="1.2"/> 1.2 Streams (flujos) de E/S

Cuando se crea o abre un fichero, se crea un ***stream*** asociado al fichero, y se genera un descriptor de archivo para poder identificarlo.

Además, se crea un ***buffer*** (almacén intermedio) por donde pasan los datos del archivo. Se utiliza para optimizar las lecturas y escrituras, sobre todo si se realizan de pocos datos y de forma continua.

Todo archivo abierto lleva asociado un puntero de posición que indica el lugar donde realizará la siguiente lectura o escritura. Cuando se crea un archivo, el **puntero de la posición** es cero (inicio). Se va actualizando conforme se lee o se escribe. Se puede indicar una nueva posición con la función *fseek*.

Hay dos tipos de *streams*:

- Uno es el *stream* de **texto**, consistente en líneas de texto. Una línea es una secuencia de caracteres terminada en el carácter de línea nueva ('\n' ó '\r\n').
- El otro formato es el del *stream* **binario**, que consiste en una secuencia de bytes que representan datos internos como números, estructuras o arrays. Se utiliza principalmente para datos que no son de tipo texto, pueden contener cualquier información. La codificación de estos ficheros ha de estar especificada mediante un estándar o con la documentación del programa.

En el momento de abrir o crear un *stream*, hay que indicar de qué tipo de archivo se trata:

- **Texto**. Formado por líneas de caracteres, terminadas en un salto de línea.
- **Binario**. Formado por una secuencia ordenada de caracteres, sin separadores especiales, que pueden almacenar cualquier tipo de información.

No hay diferencia real entre uno y otro, salvo que en un fichero en modo texto hay fines de línea y hay funciones de ficheros que pueden buscarlos. Si se abre en modo binario, la información puede ser de cualquier tipo y las funciones de ficheros no buscarán fines de línea.


## <a name="2"/> 2. Manejo de ficheros

Cuando trabajamos con ficheros se suele usar el mismo esquema, tanto si escribimos como si leemos:- Abrir el fichero- Comprobar si el fichero está abierto- Si está abierto, leer/escribir en él hasta que se encuentra el fin de fichero- Cerrar el fichero

### `FILE*`

`FILE*` es la estructura que representa un archivo y se encuentra en la cabecera `stdio.h`. En esta estructura se encuentra el atributo que representa el indicador de posición de fichero. La siguiente línea de código declara una variable de tipo puntero a fichero:

~~~c
FILE *ptr_fich;
~~~


###<a name="2.1"/> 2.1 Abrir el fichero

La función `fopen` abre o crea un fichero y lo asocia a un *stream*.
Sintaxis:
~~~cFILE *fopen(char *nombre, char *modo)
~~~Los parámetros que recibe `fopen` son:- `nombre`: cadena de caracteres con la ruta del fichero a abrir- `modo`: cadena de caracteres que indica cómo vamos a abrir el fichero. Lo veremos más adelante.

La función `fopen` devuelve un puntero de tipo `FILE`. Si ha ocurrido algún error al abrirse, devuelve NULL.

El parámetro `modo` es una combinación de los caracteres r (read, lectura), w (write, escritura), b (binario), a (append, añadir), y + (actualizar).


#### Modos de apertura de `fopen`

- **"r"** modo lectura. El flujo se posiciona al principio del fichero.
- **"r+"** modo lectura y escritura. El flujo se posiciona al principio del fichero- **"w"** modo escritura. Si el fichero no existe, se crea. Si existe, se trunca a 0. El flujo se posiciona al principio del fichero- **"w+"** modo lectura y escritura. Si el fichero no existe, se crea. Si existe, se trunca a 0. El flujo se posiciona al principio del fichero
- **"a"** modo escritura añadir al final. Si el fichero no existe, se crea. El flujo se posiciona al final del fichero.- **"a+"** modo lectura y escritura añadir al final. Si el fichero no existe, se crea. El flujo se posiciona al final del fichero.
- Podemos combinar modos: **"rw"**


###<a name="2.2"/> 2.2 Cerrar un fichero

Después de abrir un fichero y leer o escribir en él, hay que desenlazarlo del flujo de datos al que fue asociado. Esto se hace con la función `fclose`:

~~~cint fclose(FILE *f)
~~~
- La función devuelve 0 si se cerró con éxito- Cuando se termina la ejecución de un programa, el sistema operativo se "suele" encargar de cerrar todos los ficheros abiertos- Si no cerramos los ficheros abiertos podemos tener problemas.


###<a name="2.3"/> 2.3 Leer y escribir en un fichero

- La estructura `FILE` contiene una posición (marca) de por dónde vamos leyendo en el fichero.- Si leemos algo del fichero, la marca avanza hasta el siguiente carácter/dato disponible.- Si volvemos a leer del fichero, leemos a partir de la marca.- Para saber si hemos llegado al final del fichero, usaremos la función `feof (FILE *f)` (devuelve 0 si no se ha encontrado).

#### Lectura y escritura con formato: `fscanf`y `fprintf`

~~~cfscanf (FILE *f, char *formato...)fprintf (FILE *f, char * formato...)
~~~

La función `fscanf`permite leer datos de un *stream*. Todos los formatos y opciones de `scanf`se pueden aplicar a `fscanf`.

Para escribir datos con formato en un *stream* se usa `fprintf`. Todos los formatos y opciones de `printf`se pueden aplicar a `fprintf`.


Ejemplo:

~~~c
int main() {
   FILE *descriptor;

   // Creamos el archivo y lo abrimos para escritura

   descriptor = fopen("./ejemplo.txt", "w");
   if(descriptor == NULL)
      printf("Error, no se puede crear el archivo");
   else
      fprintf(descriptor, "%d", 1234);   // Escribimos algo

   //Cerramos el archivo
   fclose(descriptor);
}
~~~


#### Funciones de entrada y salida de caracteres: `fgetc`y `fputc`

Sintaxis:

~~~cint fgetc (FILE *f)
~~~

`fgetc` devuelve el carácter siguiente del *stream* como `unsigned char` y lo convierte a `int`. Devuelve el carácter `EOF` si hemos llegado al final del fichero.

~~~cint fputc (int carácter, FILE *f)
~~~

`putc` escribe el carácter en la posición a la que apunta el indicador de posición. Si todo es correcto, avanza el puntero de posición. Si se produce error, devuelve `EOF`.

Ejemplo:

~~~c
int main() {
   FILE *f1, *f2;
   char ch;

   f1 = fopen("file1.txt","r");
   f2 = fopen("file2.txt","w");

   if (f1 == NULL) {
      printf ("Error al abrir file1.txt\n");
   }
   else {
      if (f2 == NULL) {
         printf ("Error al abrir file2.txt \n");
         fclose(f1);
      }
      else {
         //Bucle de copia carácter a carácter
         while((ch = fgetc(f1)) != EOF)
             fputc(ch,f2);

         fclose(f1);
         fclose(f2);

      }
   }
}
~~~

### Lectura y escritura de líneas completas: `fgets`y `fputs`

Para leer o escribir una línea completa:

~~~cchar *fgets (char *cadena, int tamaño, FILE *f)~~~Le indicamos con `tamaño` cuántos caracteres leer. Si encuentra un `\n` solo devuelve lo leído hasta ese carácter (incluido). El valor leído se guarda en `cadena` y también se devuelve. Si se ha producido un error (fin de fichero, por ejemplo) se devuelve NULL.

~~~c
int fputs (char *cadena, FILE *f)
~~~

Escribe la `cadena` de caracteres en el fichero. Devuelve `EOF` si se ha producido un error.

~~~c
// Copiar ficheros línea a línea modo texto

#define TAM 100

int main() {
   FILE *f;
   FILE *f2;
   char cad[TAM];

   f = fopen("ejemplo1.c","r");
   f2 = fopen("ejemplo3.c","w");

   if (f == NULL) {
      printf ("Error al abrir ejemplo1.c\n");
   }
   else {
      if (f2 == NULL) {
         printf ("Error al abrir ejemplo2.c \n");
         fclose(f);
      }
      else {
         while (!feof(f)) {
            if (fgets(cad, TAM, f) != NULL)
               fputs(cad, f2);
         }

         fclose(f);
         fclose(f2);
      }
   }
}
~~~


### Funciones de entrada y salida para archivos binarios: `fread`y `fwrite`

Lectura y escritura de bloques de datos. Lee o escribe bytes de un fichero, sin importarnos si hay o no fines de línea:

~~~c
size_t fread (void *puntero, size_t tam_el, size_t num_el, FILE *stream)size_t fwrite (void *puntero, size_t tam_el, size_t num_el, FILE *stream)
~~~

Para ambas funciones, los datos binarios se representan como un vector de elementos. Lee o escribe en el fichero la información donde apunta `puntero`. Se indica el tamaño del bloque a escribir y cuántos registros se escriben.

- La función `fread`permite leer desde *stream* a una zona de memoria, cuya dirección de comienzo es `puntero`, un número`num_el`de elementos, donde el tamaño de cada elemento en bytes es `tam_el`. Si todo va bien se devuelve el número de elementos leídos e incrementa el indicador de posición.
- La función `fwrite`permite escribir un número `num_el`de elementos, de tamaño `tam_el`, desde una zona de memoria indicada por `puntero`, a un *stream*.


### Funciones de entrada y salida para tipos de datos compuestos

Si tenemos que escribir una estructura o registro en un *stream*, se puede hacer de dos formas:

- Escribir uno a uno cada uno de los campos, utilizando funciones como `printf`, etc.
- Escribir toda la estructura a la vez usando la función `fwrite`, indicando la dirección de comienzo y el tamaño del registro en bytes (obtenido con `sizeof`).

La segunda estrategia es más eficiente. En el siguiente ejemplo vemos las dos formas:


~~~c
#define TAMCAD 15

typedef struct {
   char nombre[TAMCAD];
   char apellidos[TAMCAD];
   int edad;
}TPersona;

int main() {
   TPersona alumno;
   FILE *f, *f2;

   strcpy(alumno.nombre, "Pepe");
   strcpy(alumno.apellidos, "Garcia Sanchez");
   alumno.edad = 25;

   f = fopen("alumno.dat","wb");

   if (f == NULL) {
      printf ("Error al abrir fichero\n");
   }
   else {
      // Escribimos el registro de una sola vez
      fwrite(&alumno,         // dirección de comienzo
             sizeof(alumno),  // tamaño
             1,               // número de elementos
             f);              // descriptor del fichero

      fclose(f);
   }

   f2 = fopen("alumno.txt","w");

   if (f2 == NULL) {
      printf ("Error al abrir fichero\n");
   }
   else {

      // Escribimos a continuación el registro campo a campo
      fprintf(f2, "\n%s\n", alumno.nombre);
      fprintf(f2, "%s\n", alumno.apellidos);
      fprintf(f2, "%d", alumno.edad);

      fclose(f2);
   }
}
~~~


## <a name="3"/> 3. Parámetros a `main` desde línea de comandos

Para poder pasar parámetros a un programa a través de la línea de comandos utilizamos la siguiente definición de la función `main`:

~~~c
int main(int argc, char *argv[])
~~~

- El primer agumento entero `argc`, contiene el número de argumentos recibidos por el programa, debemos considerar que siempre será el número de argumentos pasados más 1, ya que el primer agumento se reserva para contener el nombre del programa.
- El segundo `argv`es un puntero a un array de chars que contiene los parámetros pasados en el mismo orden en que fueron escritors.

Supongamos que llamemos a un programa de la siguiente manera:

~~~c
./miprograma fichero1.txt fichero2.txt
~~~

- `argc` contendrá el valor 3, debido al nombre del programa, y los dos argumentos pasados.
- argv[0] contendrá el nombre del ejecutable `miprograma`
- arg[1] será fichero1.txt
- arg[2] será fichero2.txt

~~~c
#define TAM 2048

// Ejemplo de lectura y escritura binaria, pasamos el nombre del fichero como parámetro a main

int main(int argc, char **argv) {
   FILE *fe, *fs;
   char buffer[TAM];
   int bytesLeidos;

   // Abrir el fichero de entrada en lectura y binario
   fe = fopen(argv[1], "r");

   if(!fe) {
      printf("El fichero %s no existe o no puede ser abierto.\n", argv[1]);
   } else {
      // Crear o sobreescribir el fichero de salida en binario
      fs = fopen(argv[2], "wb");

      if(!fs) {
         printf("El fichero %s no puede ser creado.\n", argv[2]);
         fclose(fe);
      }else {
      // Bucle de copia:
         while((bytesLeidos = fread(buffer, 1, TAM, fe)))
            fwrite(buffer, 1, bytesLeidos, fs);
         // Cerrar ficheros:
         fclose(fe);
         fclose(fs);
      }
   }
}
~~~


## <a name="4"/> 4. Ejercicios propuestos

1. Hacer un programa que pida palabras al usuario y que las guarde en un fichero. El programa finalizará cuando el usuario introduzca "fin". 2. Hacer un programa que pregunte el nombre de un fichero. Si existe, deberá mostrar el contenido del fichero de 25 en 25 líneas. Cuando muestre 25 líneas, esperará hasta que el usuario pulse una tecla (`getchar`) y mostrará las siguientes 25 líneas.
3. Crear un programa que pida al usuario pares de números enteros y escriba su suma (con el formato `20 + 3 = 23`) en pantalla y en un fichero llamado "sumas.txt". Cada vez que se ejecute el programa, deberá añadir los nuevos resultados a continuación de los resultados de las ejecuciones anteriores.


#### Solución ejercicio 1
~~~c
#define TAMCAD 60

int main() {
   FILE* pf;
   char fin[]="fin";
   char palabra[TAMCAD];

   pf = fopen("palabras.txt", "w");

   if (pf == NULL) {
      printf ("Error al abrir fichero\n");
   }
   else {
      do {
          printf("\nEscriba una palabra (fin para terminar). \n");
          scanf("%s",palabra);

          if (strcmp(palabra, fin) != 0)
            fprintf(pf, "%s\n", palabra);

       } while (strcmp(palabra, fin) != 0);

      fclose(pf);
   }
}

~~~

#### Solución ejercicio 2

~~~c
#define TAMLINEA 100
#define TAM 20

int main() {
   FILE* f;
   char linea[TAMLINEA], nombre[TAM];
   int i;

   printf("\nIntroduzca el nombre de fichero: ");
   scanf("%s",nombre);

   f = fopen(nombre, "r");

   if (f != NULL) {
      while (!feof(f))  {
        for (i = 0; i < 25; i++) {
            fgets(linea, TAMLINEA, f);
            if (!feof(f)) {
                printf("%s\n",linea);
            }
         }
      }
      fclose(f);
   }
}

~~~

#### Solución ejercicio 3

~~~c
int main()
{
   FILE *resultados;
   int numero1=0, numero2=0, suma=0;

   if((resultados = fopen("sumas.txt", "a+")) == NULL)
      printf("No se pudo abrir el archivo.\n");
   else
   {
      printf("Escriba dos numeros.\n\"000\" para salir.\n");
      do
      {
         printf("Primer numero: ");
         scanf("%d", &numero1);
         if (numero1 != 0) {
            printf("Segundo numero: ");
            scanf("%d", &numero2);
            suma = numero1 + numero2;
            printf("%d + %d = %d\n", numero1, numero2, suma);
            fprintf(resultados, "%d + %d = %d\n", numero1, numero2, suma);
         }
      }
      while (numero1 != 0);
      fclose(resultados);
   }
}
~~~

---

Programación 1, Grado de Robótica, curso 2017-18  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Cristina Pomares Puig
