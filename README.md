# matrix_bash

~~~
while :;
do
    echo $LINES $COLUMNS $(( $RANDOM % $COLUMNS)) $(printf "\U$(($RANDOM % 500))")
    sleep 0.05;
done |
gawk '{
    a[$3]=0;
    for (x in a) {
        o=a[x];
        a[x]=a[x]+1;
        printf "\033[%s;%sH\033[2;32m%s",o,x,$4;
        printf "\033[%s;%sH\033[1;37m%s\033[0;0H",a[x],x,$4;
        if (a[x]>=$1) {
            a[x]=0;
        }
    }
}'

~~~
Desglose:

while :; es un bucle infinito que no tiene una condición de salida explícita. Esto significa que el bucle se ejecutará continuamente hasta que se interrumpa manualmente (por ejemplo, con Ctrl+C).

echo $LINES $COLUMNS $(( $RANDOM % $COLUMNS)) $(printf "\U$(($RANDOM % 500))") genera una línea de salida que contiene cuatro valores separados por espacios:

$LINES: Representa el número de líneas visibles en la terminal actual.
$COLUMNS: Representa el número de columnas visibles en la terminal actual.
$(( $RANDOM % $COLUMNS)): Genera un número aleatorio entre 0 y la cantidad de columnas - 1.
$(printf "\U$(($RANDOM % 500))"): Genera un carácter aleatorio cuyo código Unicode está entre 0 y 499.
sleep 0.05; pausa la ejecución del bucle durante 0.05 segundos para controlar la velocidad de actualización en la terminal.

El resultado de la secuencia de comandos dentro del bucle se pasa al siguiente comando a través de una tubería (pipe |).

gawk es un intérprete de Awk que procesa la entrada de la tubería.

En el bloque gawk, se realiza lo siguiente:

Se crea un array asociativo a donde la clave es el tercer valor de la salida del comando original ($3) y se inicializa a 0.
Se recorren todos los elementos del array a con el bucle for (x in a).
Se guarda el valor actual de a[x] en o.
Se incrementa a[x] en 1.
Se imprime un carácter en la terminal usando secuencias de escape ANSI para posicionar el cursor en las coordenadas o (fila) y x (columna), y se le asigna el color verde (código 32).
Se imprime el mismo carácter en otra posición usando secuencias de escape ANSI para posicionar el cursor en las coordenadas a[x] (fila) y x (columna) pero con color blanco brillante (código 37).
Si el valor de a[x] es mayor o igual que el primer valor de la línea original ($1), se reinicia a 0.
