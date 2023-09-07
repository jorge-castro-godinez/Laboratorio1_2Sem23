# Laboratorio 1: Congruencia de Zeller
### EL4314 - Arquitectura de Computadoras I
### Escuela de Ingeniería Electrónica
### Tecnológico de Costa Rica

<br/><br/>

### Preámbulo

Para el desarrollo de este laboratorio, usted deberá contar con una instalación de liteX, ya sea de forma nativa o mediante una máquina virtual. En la [wiki](https://github.com/EL4314/Laboratorio1_1Sem23/wiki/Home) de este repositorio se tiene una serie de guías que lo ayudaran a tener ya sea una maquina virtual con liteX o un contenedor de docker con liteX. 

<br/><br/>

### Congruencia de Zeller

La congruencia de Zeller es una algoritmo que permite determinar el día de la semana para cualquier fecha del calendario. En este laboratorio usted implementará dicho algoritmo en empleando lenguaje ensamblador para RV32I. Dicho algoritmo se ejecutará en un sistema en chip (SoC), creado con LiteX, y en el cual se tendrá un _core_ de RISC-V.

<br/><br>

### Implementación
Para la implementación de su programa, deberá guiarse con código de ejemplo que le es provisto en este repositorio. Deberá crear una archivo en C, `zeller.c` (y su respectivo `zeller.h`) que le permita capturar del usuario día, mes y año y despliegue como resultado el día de la semana correspondiente. Por ejemplo, se espera recibir una entrada como se muestra a continuación. El resultado obtenido se deberá mostrar como se ejemplifica. 

```bash
lab1> zeller
23.09.2023

Día: Sábado 
```

Como se muestra anteriormente, debe ingresar la fecha separando el día, mes y año con un punto; y como salida debe indicar el día al que corresponder. Usted deberá investigar cómo implementar el algoritmo de una forma sencilla ya que lo hará usan un lenguaje de muy bajo nivel.

Las funcionalidades que implemente en `zeller.c` solamente serán para la captura y preparación de los datos que se procesarán con el algoritmo que será implementado en lenguaje ensamblador. dicho programa deberá recibir los tres datos como tres parámetros diferentes. Se debe limitar al uso de las instrucciones del ISA RV32I y deberá implementar una forma eficiente para realizar operaciones como multiplicación y división, según sea neceario

_Hint_: El siguiente fragmento de código ilustra, desde la perspectiva de C, la entrada y la salida a su programa para el filtro que deberá implementar en lenguaje ensamblador.

```c
int zeller(int day, int month, int year) {
    ...
    return day_of_week;
}
```

<br/><br>

### Resultados
Evalúe su implementación del filtro para al menos los siguientes casos:

````
06.09.2023
01.01.2000
27.02.1985
31.07.1992
````

<br/><br>

### Construcción del SoC y del código

Empleando LiteX, usted podrá construir un SoC para ejecutar el código desarrollar. Empleando:

```bash
litex_sim --integrated-main-ram-size=0x10000 --cpu-type=vexriscv --no-compile-gateware
```

podrá crearlo. Como producto de este comando, se producirá una carpeta `build`, la cuál deberemos referenciar al compilar el código.

En la carpeta `lab1` de este repositorio se encuentra el código de referencia a utilizar y modificar. En dicha carpera usted deberá agregar su código fuente, tanto `.c/.h` como `.S`. Además, deberá modificar el `Makefile` para asegurarse de que su código se compile y quede dentro del `.bin`.


```makefile
OBJECTS="helloc.o add_asm.o add.o fir_asm.o fir.o crt0.o main.o"
```

Una vez realizadas las modificaciones necesarias, desde la carpeta `lab1` podrá ejecutar un script de Python que se encargará de disparar la compilación 

```bash
./lab1.py --build-path=../build/sim
```

Para cargar el `.bin` generado y que se ejecute en el SoC, usted deberá ejecutar:

```bash
litex_sim --integrated-main-ram-size=0x10000 --cpu-type=vexriscv --ram-init=./lab1/lab1.bin
```

<br/><br>

### Evaluación
Este laboratorio se evaluará con la siguiente rúbrica


| Rubro | % | C | EP | D | NP |
|-------|---|---|----|---|----|
|`zeller.S` produce el resultado correcto| 60|   |    |   |    |
|Integración en `main.c`|30|   |    |   |    |
|Uso de repositorio |10|   |    |   |    |

C: Completo,
EP: En progreso ($\times 0,8$),
D: Deficiente ($\times 0,5$),
NP: No presenta ($\times 0$)

- El uso del repositorio implica que existan contribuciones de todos los miembros del equipo. El último _commit_ debe registrarte antes de las 23:59 del jueves 14 de setiembre de 2023.
