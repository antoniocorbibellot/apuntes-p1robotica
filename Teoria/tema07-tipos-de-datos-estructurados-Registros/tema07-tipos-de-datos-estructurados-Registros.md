
# Tema 7: Tipos de datos estructurados: Registros

## Contenidos

- [1. Registros](#1)
	- [1-1. Definición ](#1-1)
	- [1-2. Operadores ](#1-2)
	- [1-3. Registros como paso de parámetros a funciones](#1-3)
- [2. Arrays de registros](#2)


## <a name="1"/> 1. Registros

Los tipos de datos compuestos o estructurados, son tipos compuestos por otros tipos. Vimos que habían dos tipos:

- Array: Está compuesto por elementos del mismo tipo. Por ejemplo, un array de enteros, todos sus elementos son enteros.
- Registro o estructura: Está compuesto por elementos heterogéneos, pueden tener distintos tipos.

#### Definición

Un registro es un tipo de datos que permite agrupar bajo un mismo nombre elementos del mismo o de distinto tipo de datos, que se encuentran relacionados entre sí.

Cada elemento se denomina **campo** o miembro de la estructura. Un registro permite encapsular entidades donde cada campo representa los atributos o propiedades de dicha entidad.

Caraterísticas:

- Un registro o estructura es la manera que tenemos de encapsular variables de diferentes tipos bajo una única entidad: nos referimos a ellos con un identificador único.
- Tienen la peculiaridad de que las variables que forman parte del registro estarán alineadas en memoria: eficiencia
- En un registro se agrupan atributos de una entidad
- Cada uno de los elementos de un registro se denomina **campo**. Para referirse a un determinado elemento de un registro se deberá utilizar el identificador del registro, seguido de un punto ‘.’ y del identificador del campo correspondiente

> Persona:
>
> 	- Nombre
> 	- Apellidos
> 	- NIF
> 	- edad
> 	- género
>
> Dirección:
>
> 	- Calle
> 	- Número
> 	- CP
> 	- Población
> 	- Provincia
>
> Cliente:
>
> 	- NumCliente
> 	- Persona
> 	- Dirección
> 	- Empresa
>

Diferencias respecto a los arrays:

- Los elementos de un array son todos del mismo tipo, en una estructura no.
- En un array se selecciona un elemento por su posición dentro del array, en una estructura cada elemento tiene su identificador


### <a name="1-1"/> 1.1  Definición de un registro

Sintaxis utilizando `typedef`:

~~~c
typedef struct {...} nombreRegistro;
~~~
Dentro de las llaves escribiremos los campos que va a contener el registro.

~~~c
typedef struct {
	char nombre[20];
	char apellidos[30];
	char nif[10];
	int edad;
	char genero;
} TPersona;

typedef struct{
	int codCliente;
	TPersona *datosCliente;
	char direccion[100];
	char nombreEmpresa[50];
} TCliente;

int main() {
	TCliente cliente1;
	TCliente *cliente2;
}
~~~

#### Uso de los registros

Con un registro se puede trabajar a dos niveles: con el registro completo y con sus campos.

Podemos utilizar la operación de asignación entre registros, pero no la de comparación. Es decir, podemos asignar a una variable de tipo `struct`el valor de otra del mismo tipo. También es posible pasar un registro como parámetro a una función y que una función devuelva un registro.


### <a name="1-2"/> 1.2  Operadores para manejo de registros

#### El operador `.`
Para acceder a los campos de una variable de tipo registro utilizamos el operador `.`

~~~c
int main() {
	TCliente cliente1;
	TCliente *cliente2; // puntero a registro

	cliente1.codCliente = 2;
	cliente1.datosCliente = NULL;
	strcpy(cliente1.direccion, "Desconocida");
	strcpy(cliente1.nombreEmpresa, "Unknown Inc.");
}
~~~

#### Punteros a registros

La definición de una variable de tipo puntero a registro es similiar a la de cualquier otro tipo de puntero:

~~~c
TCliente *cliente2; // puntero a registro
~~~

Ya hemos visto que una variable de tipo puntero almacena direcciones de memoria, en este caso, direcciones de memoria que almacenan registros. Podemos dar una dirección válida de dos maneras:

~~~c
// A partir de la dirección de otro registro:
   TCliente cliente1;
   TCliente *cliente2;

   cliente2 = &cliente1;
~~~

~~~c
// Reservando una zona de memoria dinámica:

   cliente2 = (TCliente *)malloc(sizeof(TCliente));

   ...

// Después de su uso hay que liberar la memoria:

   free(cliente2);
~~~

#### El operador `->`

Para acceder a los campos de una variable de tipo puntero a registro utilizamos el operador `->`. Este operador indica que en la parte izquierda está la dirección de memoria de un registro, y la parte derecha los campos del mismo.

~~~c
    cliente2 = (TCliente *)malloc(sizeof(TCliente));
	cliente2->codCliente = 2;
	cliente2->datosCliente = (TPersona *)malloc(sizeof(TPersona));
	strcpy(cliente2->datosCliente->nombre, "Pepe");
	strcpy(cliente2->datosCliente->apellidos, "Pérez");
	strcpy(cliente2->datosCliente->nif, "22444555A");
	cliente2->datosCliente->edad = 20;
	cliente2->datosCliente->genero = 'm';
	strcpy(cliente2->direccion, "Calle street, 23");
	strcpy(cliente2->nombreEmpresa, "Horchatería Pepe");
~~~

### <a name="1-3"/> 1.3  Paso de registro como parámetro a función

- De forma general:	- Paso por valor cuando se use la información delregistro sin modificarlo	- Paso por referencia cuando modifiquemos algúncampo del registro	- Puntero doble a registro cuando reservemosmemoria para el registro dentro de la función

- Tenemos que tener en cuenta que el tamaño del registro en memoria es elevado (paso por valor), por lo que valoraremos en la implementación cuándo nos conviene pasarlo por valor o por referencia.

### <a name="1-4"/> 1.4 Funciones que devuelven registros

También se pueden definir funciones que devuelven registros o punteros a registros.

Por ejemplo, si queremos definir una función que devuelva la ficha vacía de un cliente de forma dinámica, es decir, devuelva un puntero a `TCliente`que apunte a una zona de memoria reservada dinámicamente:

~~~c
TCliente * creaCliente();

int main() {
   TCliente *cliente3 = NULL;

   // Reservar memoria
   cliente3 = creaCliente();

   // Resto del código
   ...

   // Liberar memoria
   free(cliente3);
   cliente3 = NULL;
}

TCliente * creaCliente() {
   TCliente * cliente;

   cliente = (TCliente *) malloc(sizeof(TCliente));

   return cliente;
}
~~~


## <a name="3"/> 2. Arrays de registros

Podemos definir arrays donde su tipo base sea un registro, es decir, cada elemento del vector es un registro, con memoria reservada para su uso

~~~c
typedef struct {
	char nombre[50];
	char descripcion[100];
	float precio;
	int stock;
} TProducto;

TProducto vectorProductos[100];
~~~

También podemos definir el tipo base como un puntero a registro. De esta forma ahorramos memoria cuando no es necesario su uso

~~~c
TProducto *vectorProductos[100];
int numProductos = 0;

vectorProductos[numProductos] = (TProducto *)malloc(sizeof(TProducto));
numProductos++;
~~~

---

### Ejercicio 1

Define los tipos de datos necesarios que permitan almacenar los datos de 100 alumnos. De cada alumno se quiere almacenar su nombre, apellidos y dirección. Cada uno tiene 10 asignaturas compuestas de código de la asignatura y nota.

~~~c
//En primer lugar definimos las constantes necesarias.
#define TAMCAD 45
#define TAMASIG 3
#define NUMASIG 10
#define NUMALU 100

//Definimos las estructuras necesarias
//Cada alumno tiene 10 asignaturas compuesto de código de la asignatura y nota
typedef struct {
   char codigoAsignatura[TAMASIG];
   float nota;
}TAsignatura;

//Estructura con la información de los alumnos
typedef struct {
   char nombre[TAMCAD];
   char apellidos[TAMCAD];
   char direccion[TAMCAD];
   TAsignatura asig[NUMASIG];
}TFichaAlumno;

//Array que contendrá los datos de todos los alumnos
typedef TFichaAlumno TAlumnos [NUMALU];
~~~

### Ejercicio 2

Escribe un programa en C que guarde información de 30 alumnos. De cada alumno leeremos su número de expediente, nombre, fecha de nacimiento, fecha de ingreso y su nota media. El programa debe permitir dar de alta un alumno y mostrar todos los alumnos.

~~~c
#define TAMCAD 45
#define NUMALU 30

typedef struct {
   int dia;
   int mes;
   int anyo;
}TFecha;

typedef struct {
   int exp;
   char nombre[TAMCAD];
   TFecha fechaNac;
   TFecha fechaIng;
   float notaMedia;
}TFichaAlumno;

//Array de alumnos
typedef TFichaAlumno TAlumnos[NUMALU];

//Prototipos funciones
void mostrarFecha(TFecha);
void mostrarAlumnos(TAlumnos, int);
void darDeAltaAlumno(TAlumnos, int *);
void pedirFecha(TFecha *);
int pedirOpcion();

int main(){
   TAlumnos alumnos;
   int opcion, numAlumnos;
   numAlumnos = 0;

   do{
      opcion = pedirOpcion();

      switch(opcion){
         case 1:
            if(numAlumnos < NUMALU)
               darDeAltaAlumno(alumnos, &numAlumnos);
            else
               printf("No es posible introducir más alumnos");
            break;
         case 2:
            mostrarAlumnos(alumnos, numAlumnos);
            break;
         case 3:
            printf("Terminado\n");
            break;
         default:
            printf("Opción incorrecta\n");
      }
   }while(opcion != 3);
}

int pedirOpcion() {
   int opcion;

   printf("******Gestión de alumnos******\n");
   printf("Opciones disponibles: \n");
   printf("1. Dar de alta un alumno\n");
   printf("2. Mostrar listado alumnos\n");
   printf("3. Salir\n");
   printf("Seleccione una opción: ");
   scanf("%d", &opcion);

   return opcion;
}

//Muestra la fecha separada por /
void mostrarFecha(TFecha fecha){
   printf("%d / %d / %d\n",fecha.dia, fecha.mes,fecha.anyo);
}

void mostrarAlumnos(TAlumnos alumnos, int numAlumnos) {
   int i;

   for(i = 0; i<numAlumnos;i++) {
      printf("Alumno: %d\n", alumnos[i].exp);
      printf("Nombre: %s\n", alumnos[i].nombre);
      printf("Fecha de nacimiento: ");
      mostrarFecha(alumnos[i].fechaNac);
      printf("Fecha de ingreso: ");
      mostrarFecha(alumnos[i].fechaIng);
      printf("Nota media: %.2f\n", alumnos[i].notaMedia);
   }
}

//Solicita la fecha
void pedirFecha(TFecha *fecha) {
   printf("\tIntroduce día:");
   scanf("%d", &fecha->dia);
   printf("\tIntroduce mes: ");
   scanf("%d", &fecha->mes);
   printf("\tIntroduce año: ");
   scanf("%d", &fecha->anyo);
}

//Solicita los datos de los alumnos
void darDeAltaAlumno(TAlumnos alumnos, int *numAlumnos) {
   int i;

   i = *numAlumnos;
   printf("Introduce el número de expediente: ");
   scanf("%d", &alumnos[i].exp);
   printf("Introduce el nombre: ");
   scanf("%s",alumnos[i].nombre);
   printf("Fecha de nacimiento:\n");
   pedirFecha(&alumnos[i].fechaNac);
   printf("Fecha de ingreso:\n");
   pedirFecha(&alumnos[i].fechaIng);
   printf("Introduce la nota media: ");
   scanf("%f", &alumnos[i].notaMedia);
   (*numAlumnos)++;
}
~~~

### Ejercicio 3


Cartera de clientes: Una empresa quiere manejar una cartera de hasta 100 clientes. La información a manejar de cada cliente es:

> Persona:
>
> 	- Nombre
> 	- Apellidos
> 	- NIF
> 	- edad
> 	- género
>
>
> Cliente:
>
> 	- NumCliente
> 	- Persona
> 	- Dirección
> 	- Empresa

Declaración de registros:

~~~c
#define MAX_CLIENTES 100

typedef struct {
	char nombre[20];
	char apellidos[30];
	char nif[10];
	int edad;
	char genero;
} TPersona;

typedef struct{
	int codCliente;
	TPersona *datosCliente;
	char direccion[100];
	char nombreEmpresa[50];
} TCliente;

~~~

Registros por valor:

~~~c
void imprimirDatosPersona(TPersona persona) {
	printf("Nombre: \t%s\n", persona.nombre);
	printf("Apellidos: \t%s\n", persona.apellidos);
	printf("NIF: \t\t%s\n", persona.nif);
	printf("Edad: \t\t%d años \nGénero: \t%s\n", persona.edad, persona.genero == 'm'?"Masculino":"Femenino");
}

void imprimirDatosCliente(TCliente cliente) {
...
}
~~~

Registros por referencia:

~~~c
void obtenerDatosPersona(TPersona *persona) {
...
}

void cambiarDireccionCliente(TCliente *cliente) {
...
}

void cambiarEmpresaCliente(TCliente *cliente) {
...
}

int crearNuevoCliente(TCliente **cliente, int id) {
...
}
~~~

Función principal:

~~~c
int main() {
...
}
~~~

Realizar:

1. Completar el código del ejercicio anterior
2. Sobre el ejercicio resuelto anterior, añadir un registro para la dirección y crear las funciones que permitan leer el registro de la entrada estándar y escribir el registro en la salida estándar. Modificar el código del ejercicio anterior para incorporar el nuevo código.

- Dirección:
	- Calle
	- Número
	- CP
	- Ciudad
	- Provincia
	- País



----

Programación 1, Grado de Robótica, curso 2017-18  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Cristina Pomares Puig
