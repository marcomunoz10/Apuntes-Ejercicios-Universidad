* Utiliza la arquitectura **ARMv8-M Mainline** --> Avanzada y con mejoras de seguridad y eficiencia.
* **Pipeline de 3 etapas**: Fetch, Decode, Execute [+]
* **TrustZone**: Seguridad Hardware que separa código y datos.
* **División por hardware (32 bit)**
* **Límites de pila**: Previene desbordamientos y mejora la seguridad.

#### [+] Explicación **Pipeline**

1. **Fetch** (Búsqueda): Busca en **memoria** donde está almacenada la instrucción y la carga en la unidad de ejecución.
2. **Decode** (Descodificación): El procesador entiende la instrucción para saber qué operación realizar; conocer datos y registros necesarios.
3. **Execute** (Ejecución).

[!] **VENTAJA** --> Permite que las tres tareas actúen en **paralelo** tratando diferentes instrucciones.
Mientras una instrucción está siendo ejecutada, otra puede ser decodificada y una más puede estar siendo obtenida, lo que mejora el rendimiento y la eficiencia del procesador.
