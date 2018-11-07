##manejo de errores y excepciones:##
  Enabling Tracing (Activación del seguimiento)  https://studio.uipath.com/v2018.1/docs/logging-levels
  
__________________________________________________________________________________________________
  ##Ejercicio práctico: trycath
Al intentar ejecutarlo la primera vez, observará que el flujo de trabajo falla con este mensaje “Object reference not set to an instance of an object.” (la referencia del objeto no tiene asignada una instancia de un objeto) al ejecutar Assign.

○      Se debe a que la variable index no está inicializada. Los tipos Int32 y String se crean con valores predeterminados aunque no se indiquen de forma explícita, pero una variable de tipo GenericValue no se crea de forma implícita porque no es posible saber si contendrá un Integer (con valor predeterminado 0), un String (con valor predeterminado “”), etc.

○      Puede cambiar el tipo de variable a Int32, o definir Default value a 0. Optemos por esto último.

Al volverlo a ejecutar, observará que el primer problema está resuelto, pero ahora falla después de varias iteraciones con el mensaje “Index was outside the bounds of the array.” (índice fuera de los límites de la matriz).
Se debe a que en ese momento, el valor de la variable de índice es 5, es decir, intenta acceder al índice/posición número 5 de filesArray y en este caso no existe. La matriz tiene 5 elementos, desde el índice 0 al índice 4. El índice 5 queda fuera de los límites de la matriz.
Se debe a un error de lógica del algoritmo porque el bucle se está ejecutando más veces de las necesarias.
El índice solo debería alcanzar un valor máximo de 4 cuando entra en la secuencia, por eso lo más sencillo es eliminar el = de la expresión, dejándola así:  index < filesArray.Count.
Si comprobamos los registros, veremos una contradicción entre los mensajes del archivo 5.txt, uno que indica que el contenido no se añadirá y otro que dice que ha finalizado la anexión.
Además, en los requisitos se indica que si falla la lectura del archivo (como en el caso del 5), la línea Append no se debe ejecutar.
En tercer lugar, si analizamos el archivo combinado, el contenido es incorrecto porque hay varias líneas duplicadas.
Esto se debe a que la variable de texto tiene como ámbito While, por eso se mantiene de una iteración a otra. Esto significa que, si no se le asigna el valor del texto del archivo a causa de la excepción Read Text File (emitida por 5.txt), mantendrá el valor anterior (en este caso, el texto de 4.txt).
Esto se soluciona fácilmente cambiando el ámbito de la variable text de While a Process para un archivo, pero soluciona solo una parte del problema y sigue sin cumplir el requisito. De hecho, si solo cambiamos el ámbito, el robot escribirá el valor de text predeterminado, “DEFAULT_TEXT” en la última línea.
La forma más sencilla de arreglar la función es colocando el contenido de los dos bloques Try Catch en uno solo.
De este modo, si falla la primera actividad, la ejecución salta al bloque Catch de manejo de excepciones, y la segunda actividad ya no se vuelve a ejecutar.
Arrastre y suelte las actividades Append Line y Log Message al primer bloque Try, justo detrás de Read Text File. Append Line depende de Read Text File y por eso tienen que estar dentro del mismo bloque Try Catch para que funcione correctamente.
Elimine el segundo bloque Try Catch.
