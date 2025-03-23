Buenas, mi nombre es Marco Muñoz y a continuación explicaré la primera parte de la sesión de hoy.

Se realizará una copia de la implementación de la parte III de la sesión anterior, donde el robot envía mensajes al seguidor de caminos según si se ha podido completar el comando o no, ya sea por un error o si está bloqueado.

Se introduce una nueva salida, A, cuyo objetivo será enviar dos tipos nuevos de mensajes dirigidos al nivel L2:

- Mensaje **"OK"**, que indica que se ha podido completar el camino entero.
- En caso de que la ruta haya sido interrumpida por la presencia de un obstáculo durante 7 segundos, se emitirá un mensaje del estilo **"KO"**, seguido del índice de camino en el cual se ha quedado el robot y los centímetros recorridos en ese paso.

Además, se utilizará la función `PrintIn` para enviar un mensaje al nivel **L1** indicando dicho bloqueo junto a los centímetros restantes.
Hasta aquí mi exposición.
