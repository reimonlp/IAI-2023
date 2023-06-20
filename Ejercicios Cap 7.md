# Ejercicios de Repaso
### Nota: En los ejercicios de programación debe utilizar Modularización.

### 1. Programe al robot para recorrer todas las avenidas de la ciudad. Cada avenida debe recorrerse sólo hasta encontrar una esquina vacía (sin flor ni papel) que seguro existe; al encontrar la esquina, debe informar si la avenida tuvo a lo sumo 45 flores.
### Nota: No modifique el contenido de las esquinas.
```
proceso recorrerAv
variables
  flores: numero
  interrumpir: boolean
comenzar
  interrumpir := F

  mientras (~(~HayFlorEnLaEsquina & ~HayPapelEnLaEsquina) & ~interrumpir)
    flores := 0
    mientras(HayFlorEnLaEsquina)
      tomarFlor
      flores := flores + 1

    repetir flores
      depositarFlor

    si (PosCa < 100)
      mover
    si (PosCa = 100)
      interrumpir := V

  Informar('aLoSumo45Flores', flores >= 45)
fin

robot robot1
variables
  avenida: numero
comenzar
  avenida := 1
  repetir 100
    Pos(avenida, 1)
    recorrerAv
    avenida := avenida + 1
fin
```
##
### 2. Programe al robot para realizar 6 rectángulos, de altura 1 y base 15. El primer rectángulo comienza en (1,1), el segundo en (1,3), el tercero en (1,5) y así siguiendo. Durante el recorrido del rectángulo debe juntar todas las flores y papeles.
### Al completar el recorrido, debe informar la cantidad total de flores y de papeles que tiene en la bolsa.
```
proceso juntarFyP
comenzar
  mientras(HayFlorEnLaEsquina)
    tomarFlor
  mientras(HayPapelEnLaEsquina)
    tomarPapel
fin

proceso rectangulo
comenzar
  repetir 2
    juntarFyP
    mover

    derecha
    repetir 15
      juntarFyP
      mover
fin

robot robot1
variables
  flores: numero
  papeles: numero
comenzar
  flores := 0
  papeles := 0

  repetir 6
    rectangulo
    Pos(1, PosCa + 2)

  mientras(HayFlorEnLaBolsa)
    depositarFlor
    flores := flores + 1
  Informar('flores', flores)

  mientras(HayPapelEnLaBolsa)
    depositarPapel
    papeles := papeles + 1
  Informar('papeles', papeles)
```
##
### 3. Programe al robot para que recorra las calles impares de la ciudad. Cada calle debe recorrerse hasta juntar al menos 10 flores (que pueden no existir). Una vez que ha recorrido todas las calles debe recorrer la avenida 10, la avenida 11, y la avenida 12 juntando todos los papeles.
```
proceso recorrerCalle
variables
  flores: numero
  interrumpir: boolean
comenzar
  interrumpir := F
  mientras(flores < 10 & ~interrumpir)
    mientras(HayFlorEnLaEsquina)
      tomarFlor
      flores := flores + 1

    si(PosAv < 100)
      mover
    si(PosAv = 100)
      interrumpir := V
fin

proceso izquierda
comenzar
  repetir 3
    derecha
fin

proceso juntarPapeles
comenzar
    mientras(HayPapelEnLaEsquina)
      tomarPapel
fin

proceso recorrerAvenida
comenzar
  repetir 99
    juntarPapeles
    mover
  juntarPapeles
fin

robot robot1
variables
  calle: numero
  avenida: numero
comenzar
  calle := 1
  avenida := 10

  derecha
  repetir 50
    Pos(1, calle)
    recorrerCalle
    calle := calle + 2

  izquierda
  repetir 3
    Pos(avenida, 1)
    recorrerAvenida
    avenida := avenida + 1
fin
```
##
### 4. Modifique el ejercicio 3 para que cada calle se recorra hasta juntar exactamente 10 flores.
```
proceso recorrerCalle
variables
  flores: numero
  interrumpir: boolean
comenzar
  interrumpir := F
  mientras(flores < 10 & ~interrumpir)
    mientras(HayFlorEnLaEsquina & flores < 10)
      tomarFlor
      flores := flores + 1

    si(PosAv < 100)
      mover
    si(PosAv = 100 | flores = 10)
      interrumpir := V
fin
```
##
### 5. Programe al robot para recorrer las primeras 10 calles de la ciudad. En cada calle debe juntar las flores y los papeles. Al finalizar cada calle informar la cantidad de esquinas con el doble de flores que papeles.
```
recorrerCalleDobleFP
variables
  flores: numero
  papeles: numero
  doblesFP: numero
comenzar
  flores := 0
  papeles := 0
  doblesFP := 0

  repetir 100
    mientras(HayFlorEnLaEsquina)
      tomarFlor
      flores := flores + 1

    mientras(HayPapelEnLaEsquina)
      tomarPapel
      papeles := papeles + 1
    
    si (flores = (papeles * 2))
      doblesFP := doblesFP + 1
    
    si(PosAv < 100)
      mover
  Informar('esquinasDobleDeFloresQuePapeles', doblesFP)
fin

robot robot1
variables
  calle: numero
comenzar
  calle := 1

  derecha
  repetir 10
    Pos(1, calle)
    recorrerCalleDobleFP
    calle := calle + 1
fin
```
##
### 6. Programe al robot para recorrer las avenidas de la ciudad de la siguiente forma. Las avenidas impares debe recorrerlas hasta encontrar una esquina con exactamente 3 flores o 15 papeles (la esquina seguro existe). En las avenidas pares debe avanzar hasta encontrar una esquina vacía (la esquina puede no existir), y si la encuentra debe informar la cantidad de pasos dados.
### Nota: No modifique el contenido de las esquinas.
```
proceso AvImpar
variables
  flores: numero
  papeles: numero
comenzar
  flores := 0
  papeles := 0

  mientras (flores <> 3 & papeles <> 15)
    mientras(HayFlorEnLaEsquina)
      tomarFlor
      flores := flores + 1
    repetir flores
      depositarFlor

    mientras(HayPapelEnLaEsquina)
      tomarPapel
      papeles := papeles + 1
    repetir papeles
      depositarPapel

    si (flores <> 3 & papeles <> 15)
      mover

    flores := 0
    papeles := 0
fin

proceso AvPar
variables
  pasos: numero
  vacia: boolean
comenzar
  pasos := 0
  vacia := F

  mientras(~vacia & PosCa < 100)
    vacia := vacia | ~(HayFlorEnLaEsquina & HayPapelEnLaEsquina)
    si (~vacia)
      pasos := pasos + 1
      mover

  vacia := vacia | ~(HayFlorEnLaEsquina & HayPapelEnLaEsquina)
  si (vacia)
    Informar('PasosDados', pasos)
fin

robot robot1
variables
  avenida: numero
comenzar
  avenida := 1

  repetir 50
    Pos(avenida, 1)
    AvImpar

    Pos(avenida, 1)
    AvPar
fin
```
##
### 7. Realice un programa que le permita al robot recorrer las calles de la ciudad. Al finalizar cada calle debe informar V si tuvo más papeles que flores o F en caso contrario.
### Nota: No modifique el contenido de las esquinas.
```
proceso recorrerCalleInformarMasPapelesQueFlores
variables
  papeles: numero
  flores: numero
comenzar
  flores := 0
  papeles := 0

  mientras(HayFlorEnLaEsquina)
    tomarFlor
    flores := flores + 1
  repetir flores
    depositarFlor

  mientras(HayPapelEnLaEsquina)
    tomarPapel
    papeles := papeles + 1
  repetir papeles
    depositarPapel

  Informar('MasPapelesQueFlores', papeles > flores)
fin

robot robot1
variables
  calle: numero
comenzar
  calle := 1

  derecha
  repetir 100
    Pos(1, calle)
    recorrerCalleInformarMasPapelesQueFlores
    calle := calle + 1
fin
```
##
### 8. Realice un programa que le permita al robot recorrer las avenidas de la ciudad de la siguiente forma.
### - Las avenidas impares debe recorrerlas hasta encontrar una esquina originalmente vacía (sin flor ni papel, seguro existe), por cada esquina transitada debe juntar todas las flores y papeles. Al encontrar la esquina, informar la cantidad de pasos dados.
### - Las avenidas pares deben recorrerse completas e informar al final de su recorrido la cantidad de esquinas que solamente tenían flor.
```
proceso juntarFyP
comenzar
  mientras(HayFlorEnLaEsquina)
    tomarFlor
  mientras(HayPapelEnLaEsquina)
    tomarPapel
fin

proceso AvImpar
variables
  pasos: numero
comenzar
  pasos := 0

  mientras(HayFlorEnLaEsquina | HayPapelEnLaEsquina)
    juntarFyP
    pasos := pasos + 1
    mover
  
  Informar('PasosDados', pasos)
fin

proceso AvPar
variables
  soloFlor: numero
comenzar
  soloFlor := 0

  repetir 100
    si(HayFlorEnLaEsquina & ~HayPapelEnLaEsquina)
      soloFlor := soloFlor + 1
    si(PosCa < 100)
      mover
  
  Informar('esquinasSolamenteTenianFlor', soloFlor)
fin

robot robot1
variables
  avenida: numero
comenzar
  avenida := 1

  repetir 50
    Pos(avenida, 1)
    AvImpar

    Pos(avenida, 1)
    AvPar
fin
```
##
### 9. Realice un programa que le permita al robot recorrer el perímetro de la ciudad juntando todas las flores. Al terminar el perímetro informar la cantidad total de flores juntadas. Luego, debe recorrer la avenida 25 de la ciudad hasta encontrar una esquina con flor y sin papel.
```
proceso perimetro
variables
  flores: numero
comenzar
  flores := 0

  Pos(1, 1)
  repetir 4
    repetir 99
      mientras(HayFlorEnLaEsquina)
        tomarFlor
        flores := flores + 1
      mover
    derecha
  Informar('totalFloresJuntadas', flores)
fin

proceso recorrerAv25
comenzar
  Pos(25, 1)
  mientras(HayFlorEnLaEsquina & ~HayPapelEnLaEsquina & PosCa < 100)
    mover
fin

robot robot1
comenzar
  perimetro
  recorrerAv25
fin
```
##
### 10. Realice un programa que le permita al robot recorrer la ciudad hasta encontrar una esquina con flor y sin papel (la cual puede no existir). Si la encuentra debe recorrer la calle 20 juntado todas las flores e informar al final de su recorrido la cantidad total juntada.
```
proceso recorrerCiudadBuscando
variables
  avenida: numero
  cumple: boolean
comenzar
  avenida := 1
  cumple := F

  mientras(avenida <= 100 & ~cumple)
    Pos(avenida, 1)
    mientras(~cumple & PosCa < 100)
      cumple := (HayFlorEnLaEsquina & ~HayPapelEnLaEsquina)
      si (~cumple)
        mover
    cumple := (HayFlorEnLaEsquina & ~HayPapelEnLaEsquina)

  si (cumple)
    derecha
    recorrerCalle20

proceso recorrerCalle20
variables
  flores: numero
comenzar
  flores := 0
  Pos(1, 20)

  repetir 100
    mientras(HayFlorEnLaEsquina)
      tomarFlor
      flores := flores + 1
    si(PosAv < 100)
      mover
  Informar('totalFloresJuntadas', flores)
fin

robot robot1
comenzar
  recorrerCiudadBuscando
fin
```

### 11. Realice un programa que le permita al robot recorrer todas las avenidas de la ciudad. Cada avenida debe recorrerse hasta encontrar una esquina con exactamente 50 objetos (suma entre flores y papeles, por ejemplo: 20 flores y 30 papeles). Al finalizar cada avenida debe informar V, si encontró más flores que papeles, o F en caso contrario.
### Nota: La esquina con 50 objetos puede no existir en cada avenida. No debe modificar el contenido de las esquinas.
```
recorrerAv50Objetos
variables
  flores: numero
  papeles: numero
  cumple: boolean
  interrumpir: boolean
comenzar
  flores := 0
  papeles := 0
  cumple := F
  interrumpir := F

  mientras(~interrumpir)
    mientras(HayFlorEnLaEsquina)
      tomarFlor
      flores := flores + 1
    repetir flores
      depositarFlor

    mientras(HayPapelEnLaEsquina)
      tomarPapel
      papeles := papeles + 1
    repetir papeles
      depositarPapel

    cumple := ((flores + papeles) = 50)

    si(PosCa = 100 | cumple)
      interrumpir := V
    sino
      mover

    flores := 0
    papeles := 0

  Informar('MasFloresQuePapeles', flores > papeles)
fin

robot robot1
variables
  avenida
comenzar
  avenida := 1
  repetir 100
    Pos(avenida, 1)
    recorrerAv50Objetos
fin
```
##
### 12. Con los conocimientos adquiridos en éste curso de ingreso, piense si sería posible aplicar modularización a estos ejercicios. Justifique. En caso de poder utilizar modularización, resuelva el ejercicio.

### (a) Programe al robot para recorrer la avenida 1 hasta encontrar una esquina vacía. Durante el recorrido debe juntar las flores de las esquinas transitadas.
### Luego, el robot debe realizar un cuadrado, partiendo desde (1,1), cuyo tamaño de lado se corresponde con la cantidad de flores juntadas en la avenida 1. Suponga que la avenida 1 tiene a lo sumo 99 flores.
```
{ SOLUCION NO MODULARIZABLE SIN UTILIZAR PASAJE DE PARAMETROS }
```
##
### (b) Programe al robot para recorrer 15 cuadrados los cuales comienzan siempre en la esquina (1,1). El primer cuadrado tiene 15 cuadras de lado, el segundo 14 cuadras de lado, el tercero 13 cuadras de lado, y así siguiendo.
```
{ SOLUCION NO MODULARIZABLE SIN UTILIZAR PASAJE DE PARAMETROS }
```
##
### (c) Programe al robot para recorrer la ciudad hasta encontrar una esquina con papel o sin flor.
```
{ SOLUCION NO MODULARIZABLE SIN UTILIZAR PASAJE DE PARAMETROS }
```
##
### (d) Programe al robot para que recorra toda la ciudad por avenidas juntando flores y papeles. Al finalizar el recorrido informar la cantidad total de papeles y flores juntadas.
```
proceso juntarFyP
comenzar
  mientras(HayFlorEnLaEsquina)
    tomarFlor
  mientras(HayPapelEnLaEsquina)
    tomarPapel
fin

proceso recorrerAvsJuntando
variables
  avenida: numero
comenzar
  avenida := 1

  repetir 100
    Pos(avenida, 1)
    repetir 100
      juntarFyP
      si(PosCa < 100)
        mover
    avenida := avenida + 1
fin

proceso contarInformar
variables
  flores: numero
  papeles: numero
comenzar
  flores := 0
  papeles := 0

  mientras(HayFlorEnLaBolsa)
    depositarFlor
    flores := flores + 1

  mientras(HayPapelEnLaBolsa)
    depositarPapel
    papeles := papeles + 1

  Informar('Flores', flores)
  Informar('Papeles', papeles)
fin

robot robot1
comenzar
  recorrerAvsJuntando
  contarInformar
fin
```
##
### 13. Convierta el número 143 a binario y a hexadecimal.
```
  143 / 2 = 71   % 1
   71 / 2 = 35   % 1
   35 / 2 = 17   % 1
   17 / 2 = 8    % 1
    8 / 2 = 4    % 0
    4 / 2 = 2    % 0
    2 / 2 = 1    % 0
    1 / 2 = 0    % 1

  Binario:      1000 1111
  Hexadecimal:  8F  
```
##
### 14. Convierta el número 1011001101 a decimal y a hexadecimal.
```
0010 1100 1101 = 2^9 + 2^7 + 2^6 + 2^3 + 2^2 + 2^0
               = 512 + 128 +  64 +  8  +  4  +  1
               = 717

Decimal:       717
Hexadecimal:  02CD
```
##
### 15. Sume los números binarios 11010111 y 100101. Luego exprese el resultado en hexadecimal.
```
 1101 0111
+  10 0101
 ---------
 1111 1100

Hexadecimal:  FC
```